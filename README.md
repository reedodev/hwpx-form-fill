<p align="center"><img src="docs/banner.png" alt="한글 양식 채우기: 한컴오피스 없이 신청서 칸을 채웁니다" width="100%"></p>

<p align="center">
<a href="https://github.com/reedodev/hwpx-form-fill/actions/workflows/test.yml"><img src="https://github.com/reedodev/hwpx-form-fill/actions/workflows/test.yml/badge.svg" alt="test"></a>
<img src="https://img.shields.io/badge/Claude_Code-skill-2547F4" alt="Claude Code skill">
<img src="https://img.shields.io/badge/license-MIT-black" alt="MIT">
</p>

# 한글 양식 채우기 (hwpx-form-fill)

한글(HWPX) 신청서·공문 양식의 표 칸을 라벨로 찾아 채우고, 이미 내용이 있는 칸은 덮어쓰지 않으며, 저장한 파일을 다시 열어 들어간 글자를 확인합니다. 못 찾은 칸이 있으면 완료라고 하지 않습니다.

<!-- prereq-adapt -->
## 무엇이 필요한가요?

꼭 필요한 것은 Python 3.10 이상과 `pip install python-hwpx`, 채울 양식 파일(.hwpx), 채울 값(`values.json`)입니다. 권장·선택 준비물까지 정리한 표는 [SKILL.md의 준비물](skills/hwpx-form-fill/SKILL.md#준비물-쓰기-전에-있어야-하는-것)에 있습니다.

## 바꿔 쓰기

에이전트에게 다음처럼 말하면 옵션을 바꿔서 실행합니다.

- "라벨 아래 칸에 넣어 줘" → `values.json`에서 `"신청 동기@below": "..."`
- "같은 라벨이 두 개라 헷갈려" → `inspect`로 위치를 보고 `"#0:5,1": "..."`(표 0번, 5행 1열)로 직접 지정
- "이미 적힌 칸도 덮어써 줘" → `--force` (사용자가 분명히 원할 때만)

말과 옵션의 대응표는 [SKILL.md의 바꿔 쓰기](skills/hwpx-form-fill/SKILL.md#바꿔-쓰기-이렇게-말하세요)에 있습니다.

## 설치

**Claude Code 플러그인으로 설치**

```
/plugin marketplace add reedodev/hwpx-form-fill
/plugin install hwpx-form-fill@hwpx-form-fill
```

**폴더만 복사**

```bash
git clone https://github.com/reedodev/hwpx-form-fill.git
cp -r hwpx-form-fill/skills/hwpx-form-fill ~/.claude/skills/
```

필요한 것: Python 3.10+, python-hwpx

## 바로 해 보기

```bash
pip install python-hwpx
python skills/hwpx-form-fill/scripts/hwpx_form.py fill skills/hwpx-form-fill/examples/form-sample.hwpx skills/hwpx-form-fill/examples/values.example.json filled.hwpx
```

자세한 절차, 옵션, 한계는 [SKILL.md](skills/hwpx-form-fill/SKILL.md)에 있습니다. 에이전트가 이 파일을 읽고 그대로 따릅니다.

## 시험

`node tests/run.mjs`가 SKILL.md 형식을 검사하고 예시 파일로 스크립트를 실제로 실행합니다. 푸시할 때마다 GitHub Actions가 같은 시험을 돌립니다(위 test 배지).

<!-- credits -->
## 출처와 영감

- **HWPX 읽기·쓰기**: [airmang/python-hwpx](https://github.com/airmang/python-hwpx)를 그대로 불러 씁니다. 표 칸을 찾고 덮어쓰기를 막고 다시 열어 확인하는 부분은 이 저장소에서 만든 것입니다.
- **변환과 PDF 확인**: [neolord0/hwp2hwpx](https://github.com/neolord0/hwp2hwpx)(.hwp를 .hwpx로), [LibreOffice](https://www.libreoffice.org/) + [ebandal/H2Orestart](https://github.com/ebandal/H2Orestart)(PDF 변환)를 안내합니다. 각 프로젝트의 라이선스를 따릅니다.
- 한글 문서를 새로 만들거나 크게 편집하려면 [jkf87/hwpx-skill](https://github.com/jkf87/hwpx-skill), [airmang/hwpx-plugins](https://github.com/airmang/hwpx-plugins)도 함께 보세요.

## 만든 사람

**리도**, AI 좌충우돌 실습 중인 20년차 엔지니어입니다. 매일 AI에게 일을 맡기다가 막힌 부분을 고쳐 가며 이 스킬을 만들었습니다.
[YouTube @Reedodev](https://www.youtube.com/@Reedodev) · [Threads @reedo.dev](https://www.threads.com/@reedo.dev) · [GitHub @reedodev](https://github.com/reedodev)

같이 만든 다른 스킬: [옵시디언 3D 지식그래프](https://github.com/reedodev/obsidian-brain-3d) · [노트 먼저 찾아보기](https://github.com/reedodev/vault-recall) · [작업 기록 강의자료](https://github.com/reedodev/work-to-lecture-deck) · [한국어 말투 점검](https://github.com/reedodev/korean-tone-check) · [네이버 황금키워드](https://github.com/reedodev/naver-golden-keywords) · [TTS 끝소리 고치기](https://github.com/reedodev/tts-tail-fix) · [카드뉴스 렌더](https://github.com/reedodev/html-card-render) · [영상 비율 검사](https://github.com/reedodev/social-video-gate) · [유튜브 게시 점검](https://github.com/reedodev/youtube-publish-checklist)

---

## English

Fill Hangul (HWPX) form tables by label without Hancom Office, refuse to overwrite filled cells, re-open the file to verify, and fail loudly on missing or ambiguous labels.

**Credits.**
- Uses [airmang/python-hwpx](https://github.com/airmang/python-hwpx) for HWPX I/O; label lookup, overwrite guard and verification are original. Points to [hwp2hwpx](https://github.com/neolord0/hwp2hwpx), [LibreOffice](https://www.libreoffice.org/) and [H2Orestart](https://github.com/ebandal/H2Orestart). For authoring whole documents see [jkf87/hwpx-skill](https://github.com/jkf87/hwpx-skill) and [airmang/hwpx-plugins](https://github.com/airmang/hwpx-plugins).

Install: `/plugin marketplace add reedodev/hwpx-form-fill` then `/plugin install hwpx-form-fill@hwpx-form-fill`, or copy `skills/hwpx-form-fill` into `~/.claude/skills/`. See [SKILL.md](skills/hwpx-form-fill/SKILL.md) for the full procedure. MIT License; third-party models, fonts and libraries keep their own licenses.
