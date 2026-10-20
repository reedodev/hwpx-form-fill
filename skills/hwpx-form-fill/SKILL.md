---
name: hwpx-form-fill
description: "한컴오피스 없이 한글(HWPX) 신청서·공문 양식의 표 칸을 라벨로 찾아 채우고, 결과를 다시 열어 검증한 뒤 PDF로 쪽마다 확인한다. '한글 파일 채워줘', 'hwpx 양식 작성', '신청서 칸 채우기', '공모전 서식' 요청에 쓴다. Use to fill Korean Hangul (HWP/HWPX) form tables by label without Hancom Office, verify the result and review rendered PDF pages."
---

# 한글(HWPX) 양식 채우기 (hwpx-form-fill)

공모전 신청서나 관공서 양식은 대부분 한글 파일로 나오는데, 한컴오피스가 없거나 버전이 오래되면 칸이 깨지기 쉽습니다. 이 스킬은 오픈소스 [python-hwpx](https://github.com/airmang/python-hwpx)로 표의 **정확한 칸**을 찾아 채우고, 저장한 파일을 다시 열어 들어간 글자를 확인합니다.

## 준비
```bash
pip install python-hwpx
```
`.hwp`(옛 바이너리 형식)만 있다면 먼저 [hwp2hwpx](https://github.com/neolord0/hwp2hwpx)로 `.hwpx`로 바꿉니다.

<!-- prereq-adapt -->
## 준비물 (쓰기 전에 있어야 하는 것)
| 준비물 | 어디에 쓰나요 | 꼭 필요한가요 |
|---|---|---|
| Python 3.10 이상과 `pip install python-hwpx` | HWPX 읽기·쓰기 | 필수 |
| 채울 양식 파일(.hwpx) | .hwp만 있으면 먼저 hwp2hwpx로 바꿉니다 | 필수 |
| 채울 값(`values.json`) | 사용자가 준 실제 값만 씁니다 | 필수 |
| LibreOffice + H2Orestart | PDF로 바꿔 쪽마다 눈으로 확인 | 권장(마지막 확인용) |

## 바꿔 쓰기: 이렇게 말하세요
에이전트에게 왼쪽처럼 말하면 오른쪽을 바꿔서 실행합니다.

| 이렇게 말하면 | 바뀌는 것 |
|---|---|
| "라벨 아래 칸에 넣어 줘" | `values.json`에서 `"신청 동기@below": "..."` |
| "같은 라벨이 두 개라 헷갈려" | `inspect`로 위치를 보고 `"#0:5,1": "..."`(표 0번, 5행 1열)로 직접 지정 |
| "이미 적힌 칸도 덮어써 줘" | `--force` (사용자가 분명히 원할 때만) |
| "칸 위치만 먼저 보여 줘" | `python scripts/hwpx_form.py inspect form.hwpx` |

어떤 글자를 안내 문구로 보고 덮어쓸지는 `scripts/hwpx_form.py`의 `GUIDE_RE`에서 정합니다. 양식마다 쓰는 안내 기호가 다르면 여기에 더합니다.

## 절차
1. **양식 구조 보기**: 모든 표의 칸 위치와 글자를 봅니다.
   ```bash
   python scripts/hwpx_form.py inspect form.hwpx > cells.json
   ```
2. **채울 값 정하기** (`values.json`):
   ```json
   {
     "성명": "홍길동",
     "생년월일": "1990-01-01",
     "신청 동기@below": "아래 칸에 들어갈 내용",
     "#0:5,1": "표 0번, 5행 1열에 직접 넣기"
   }
   ```
   - 기본은 라벨 **오른쪽** 칸이고, `@below`를 붙이면 **아래** 칸입니다.
   - 같은 라벨이 여러 개면 `ambiguous`로 멈추므로, 1단계 결과를 보고 `#표:행,열` 주소로 지정합니다.
3. **채우기**:
   ```bash
   python scripts/hwpx_form.py fill form.hwpx values.json filled.hwpx
   ```
   - 빈 칸이나 안내 문구(□ ○ ※ -, "적어 주세요" 등)만 덮어씁니다. 이미 내용이 있는 칸은 `skipped_filled`로 남기고 건드리지 않습니다(`--force`로 강제할 수 있지만 꼭 필요할 때만).
   - 저장한 파일을 다시 열어 칸마다 `verified`를 확인하고, 문제가 하나라도 있으면 종료 코드 1을 돌려줍니다.
4. **PDF로 눈으로 확인**: 무료 [LibreOffice](https://www.libreoffice.org/)에 [H2Orestart](https://github.com/ebandal/H2Orestart) 확장을 깔고 변환합니다.
   ```bash
   soffice --headless --convert-to pdf filled.hwpx
   ```
   PDF를 쪽마다 이미지로 열어 칸 넘침, 빈칸, 중복, 남은 안내 기호, 체크박스, 서명란, 첨부 누락을 확인합니다. **이 확인이 끝나기 전에는 완료라고 하지 않습니다.**

## 지키는 것
- XML을 문자열로 직접 바꾸지 않습니다. 같은 문단을 두 번 넣지 않습니다.
- 개인정보(주민번호, 연락처, 주소)는 사용자가 준 값만 쓰고, 로그·파일명·보고에 그대로 옮기지 않습니다.
- 서명란에 손글씨 서명을 흉내 내지 않습니다. 동의 체크와 서명은 사용자가 허락한 범위에서만 합니다.
- 한컴오피스를 자동으로 띄워 변환해야 한다면 창을 숨기고, 사용자가 이미 열어 둔 한글 창은 절대 닫지 않습니다.

## 한계
- 병합된 칸이 복잡한 양식은 라벨 오른쪽 칸이 기대와 다를 수 있습니다. 1단계 결과로 위치를 확인하고 주소로 지정하세요.
- `verified`는 글자가 들어갔는지만 봅니다. 줄바꿈, 글자 크기, 칸 넘침은 4단계 PDF로만 확인할 수 있습니다.

<!-- credits -->
## 출처와 영감
- **HWPX 읽기·쓰기**: [airmang/python-hwpx](https://github.com/airmang/python-hwpx)를 그대로 불러 씁니다. 표 칸을 찾고 덮어쓰기를 막고 다시 열어 확인하는 부분은 이 저장소에서 만든 것입니다.
- **변환과 PDF 확인**: [neolord0/hwp2hwpx](https://github.com/neolord0/hwp2hwpx)(.hwp를 .hwpx로), [LibreOffice](https://www.libreoffice.org/) + [ebandal/H2Orestart](https://github.com/ebandal/H2Orestart)(PDF 변환)를 안내합니다. 각 프로젝트의 라이선스를 따릅니다.
- 한글 문서를 새로 만들거나 크게 편집하려면 [jkf87/hwpx-skill](https://github.com/jkf87/hwpx-skill), [airmang/hwpx-plugins](https://github.com/airmang/hwpx-plugins)도 함께 보세요.
