<p align="center"><img src="docs/banner.png" alt="한글 양식 채우기: 한컴오피스 없이 신청서 칸을 채웁니다" width="100%"></p>

<p align="center">
<a href="https://github.com/reedodev/hwpx-form-fill/actions/workflows/test.yml"><img src="https://github.com/reedodev/hwpx-form-fill/actions/workflows/test.yml/badge.svg" alt="test"></a>
<img src="https://img.shields.io/badge/Claude_Code-skill-2547F4" alt="Claude Code skill">
<img src="https://img.shields.io/badge/license-MIT-black" alt="MIT">
</p>

# 한글 양식 채우기 (hwpx-form-fill)

한글(HWPX) 신청서·공문 양식의 표 칸을 라벨로 찾아 채우고, 이미 내용이 있는 칸은 덮어쓰지 않으며, 저장한 파일을 다시 열어 들어간 글자를 확인합니다. 못 찾은 칸이 있으면 완료라고 하지 않습니다.

## 처음 쓰시는 분을 위한 안내

### 이 스킬은 언제 쓰나요?

- 구청이나 학교에서 받은 신청서(.hwpx)에 이름, 생년월일, 이메일 같은 칸을 여러 장 반복해서 채워야 할 때
- 한컴오피스가 없는 컴퓨터에서 한글 양식을 채워야 할 때

사람이 칸마다 복사해서 붙이면 칸이 밀리거나 이미 적힌 내용을 덮어쓰기 쉽습니다. 이 스킬은 "성명" 같은 글자(라벨)를 찾아 그 옆 칸에만 값을 넣고, 저장한 파일을 다시 열어 들어간 글자가 맞는지 확인합니다. 못 찾은 칸이 하나라도 있으면 성공이라고 말하지 않습니다.

### 용어 풀이

| 용어 | 뜻 |
|---|---|
| Claude Code | 터미널에서 AI와 대화하며 파일 작업을 시키는 프로그램입니다. |
| 스킬(skill) | Claude Code에게 "이 일은 이렇게 하라"고 알려 주는 설명서와 도구 묶음입니다. |
| 터미널 | 글자로 명령을 입력하는 창입니다. Windows는 PowerShell, Mac은 터미널 앱입니다. |
| HWPX | 한글의 새 파일 형식(.hwpx)입니다. 옛 형식인 .hwp는 바로 쓸 수 없습니다. |
| 라벨 | 양식에서 "성명", "이메일"처럼 칸의 이름이 적힌 칸입니다. |

### 준비 확인

터미널에 아래를 한 줄씩 입력해 숫자가 나오는지 봅니다.

```bash
node -v        # v18 이상이면 됩니다. 없으면 https://nodejs.org 에서 설치합니다.
python --version
```

- **Windows**: `python --version`이 안 되면 https://www.python.org/downloads/ 에서 3.10 이상을 설치합니다. 설치할 때 "Add python.exe to PATH"를 꼭 체크합니다. 이후 `py --version`도 같은 결과를 냅니다.
- **Mac**: 터미널에서 `python3 --version`으로 확인합니다. 없으면 https://www.python.org/downloads/ 에서 설치합니다. 아래 명령의 `python`은 `python3`로 바꿔 입력합니다.

그다음 이 스킬이 쓰는 파이썬 도구를 설치하고 확인합니다.

```bash
pip install python-hwpx          # Mac은 pip3 install python-hwpx
pip show python-hwpx             # Name: python-hwpx 가 보이면 설치된 것입니다.
```

### 따라 하기 3단계

**1단계. 설치합니다.** Claude Code에서 아래 두 줄을 차례로 입력합니다.

```
/plugin marketplace add reedodev/hwpx-form-fill
/plugin install hwpx-form-fill@hwpx-form-fill
```

**2단계. 에이전트에게 이렇게 말합니다.** 아래 문장을 그대로 복사해서 쓰고, 파일 이름만 내 것으로 바꿉니다.

1. `신청서.hwpx 양식의 칸 구조를 먼저 보여 줘.` (칸 위치와 라벨을 확인하는 단계입니다.)
2. `values.json에 성명은 홍길동, 이메일은 gildong@example.com으로 적어서 신청서.hwpx를 채운 뒤 신청서-작성본.hwpx로 저장해 줘.`
3. `못 찾은 칸이나 이미 적혀 있어서 건너뛴 칸이 있으면 알려 줘.`

**3단계. 결과를 확인합니다.** 만들어진 `신청서-작성본.hwpx`를 한글에서 열어 칸마다 눈으로 확인합니다. 원본 파일은 바뀌지 않습니다.

### 직접 실행해 보기

예시 양식으로 먼저 해 보시면 어떤 결과가 나오는지 알 수 있습니다. 아래는 이 저장소를 새로 받아서 실제로 실행한 결과입니다(`filled.hwpx`가 새로 생깁니다).

```bash
python skills/hwpx-form-fill/scripts/hwpx_form.py fill skills/hwpx-form-fill/examples/form-sample.hwpx skills/hwpx-form-fill/examples/values.example.json filled.hwpx
```

```
{
 "filled": [
  { "key": "성명", "table": 0, "row": 0, "col": 1, "verified": true },
  { "key": "생년월일", "table": 0, "row": 0, "col": 3, "verified": true },
  { "key": "이메일", "table": 0, "row": 1, "col": 3, "verified": true },
  { "key": "신청 동기", "table": 0, "row": 3, "col": 1, "verified": true }
 ],
 "missing": [],
 "ambiguous": [],
 "skipped_filled": [],
 "validate_ok": true
}
```

(실제 출력은 칸마다 줄이 나뉘어 길게 나오며, 여기서는 읽기 쉽게 한 줄로 줄였습니다.) 읽는 법은 이렇습니다. `filled`에 들어간 칸이 있고 모두 `"verified": true`이면 저장한 파일을 다시 열어 글자가 맞다는 뜻입니다. `missing`, `ambiguous`, `skipped_filled`가 모두 `[]`이고 마지막 줄 종료 코드가 0이면 성공입니다.

### 막힐 때

| 증상 | 원인 | 해결 |
|---|---|---|
| `python-hwpx is not installed. Run: pip install python-hwpx` | 파이썬 도구가 설치되지 않았습니다. | `pip install python-hwpx`(Mac은 `pip3`)를 실행한 뒤 다시 시도합니다. |
| `Error: form file not found: nofile.hwpx` (종료 코드 2) | 양식 파일 경로가 틀렸습니다. | 파일 이름과 폴더를 확인하고, 터미널의 현재 위치에서 보이는 경로를 적습니다. |
| `Error: values file not found: nov.json` (종료 코드 2) | 채울 값 파일(`values.json`)이 없습니다. | 예시 `values.example.json`을 복사해서 내 값으로 고칩니다. |
| `skipped_filled`에 칸이 나오고 종료 코드가 1입니다. | 그 칸에 이미 내용이 있어서 덮어쓰지 않았습니다. | 덮어써도 되는 칸이면 `--force`를 붙입니다. 아니면 그대로 둡니다. |
| `missing`에 이름이 나옵니다. | 양식에 그 라벨이 없습니다. 띄어쓰기는 무시하지만 글자가 달라도 못 찾습니다. | `inspect` 명령으로 실제 라벨을 확인하고 `values.json`의 이름을 맞춥니다. |
| `ambiguous`에 이름이 나옵니다. | 같은 라벨이 두 곳 이상에 있습니다. | `"#0:2,1": "값"`처럼 표 번호와 행·열 번호로 칸을 직접 지정합니다. |

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

같이 만든 다른 스킬: [설명 영상 만들기](https://github.com/reedodev/explainer-studio) · [대본 하나로 만드는 단편](https://github.com/reedodev/scenario-to-film) · [옵시디언 3D 지식그래프](https://github.com/reedodev/obsidian-brain-3d) · [노트 먼저 찾아보기](https://github.com/reedodev/vault-recall) · [작업 기록 강의자료](https://github.com/reedodev/work-to-lecture-deck) · [한국어 말투 점검](https://github.com/reedodev/korean-tone-check) · [네이버 황금키워드](https://github.com/reedodev/naver-golden-keywords) · [TTS 끝소리 고치기](https://github.com/reedodev/tts-tail-fix) · [카드뉴스 렌더](https://github.com/reedodev/html-card-render) · [영상 비율 검사](https://github.com/reedodev/social-video-gate) · [유튜브 게시 점검](https://github.com/reedodev/youtube-publish-checklist)

---

## English

Fill Hangul (HWPX) form tables by label without Hancom Office, refuse to overwrite filled cells, re-open the file to verify, and fail loudly on missing or ambiguous labels.

### New here? A beginner's guide

**When to use it.** You have an application form (.hwpx) from a school or a government office and must fill the same fields (name, birth date, email) again and again. Or you are on a computer without Hancom Office. Copy-pasting by hand shifts cells and overwrites text. This skill finds a label such as "성명" (name), writes only into the cell next to it, then re-opens the saved file to check the text. It never reports success if even one cell was missed.

**Glossary**

| Term | Meaning |
|---|---|
| Claude Code | A program you talk to in the terminal; it works on your files for you. |
| Skill | A bundle of instructions and tools that tells Claude Code how to do one kind of job. |
| Terminal | The window where you type commands (PowerShell on Windows, Terminal on Mac). |
| HWPX | The newer file format of Hangul (.hwpx). The older .hwp cannot be used directly. |
| Label | The cell holding a field name such as "성명" or "이메일". |

**Check your setup**

```bash
node -v        # v18 or newer. If missing, install from https://nodejs.org
python --version
```

- **Windows**: if `python --version` fails, install Python 3.10 or newer from https://www.python.org/downloads/ and tick "Add python.exe to PATH". `py --version` then works too.
- **Mac**: use `python3 --version`. Install from python.org if missing. Replace `python` with `python3` in the commands below.

Then install and check the Python tool this skill uses:

```bash
pip install python-hwpx          # on Mac: pip3 install python-hwpx
pip show python-hwpx             # you should see: Name: python-hwpx
```

**Three steps**

1. Install in Claude Code:
   ```
   /plugin marketplace add reedodev/hwpx-form-fill
   /plugin install hwpx-form-fill@hwpx-form-fill
   ```
2. Tell the agent (copy these, change the file names):
   - `Show me the cell layout of application.hwpx first.`
   - `In values.json put name = Hong Gildong and email = gildong@example.com, fill application.hwpx and save it as application-filled.hwpx.`
   - `Tell me about any labels you could not find and any cells you skipped because they already had text.`
3. Open `application-filled.hwpx` in Hangul and check each cell by eye. The original file is not changed.

**Try it with the sample form.** This is real output from a fresh clone (it creates `filled.hwpx`):

```bash
python skills/hwpx-form-fill/scripts/hwpx_form.py fill skills/hwpx-form-fill/examples/form-sample.hwpx skills/hwpx-form-fill/examples/values.example.json filled.hwpx
```

```
{
 "filled": [
  { "key": "성명", "table": 0, "row": 0, "col": 1, "verified": true },
  { "key": "생년월일", "table": 0, "row": 0, "col": 3, "verified": true },
  { "key": "이메일", "table": 0, "row": 1, "col": 3, "verified": true },
  { "key": "신청 동기", "table": 0, "row": 3, "col": 1, "verified": true }
 ],
 "missing": [],
 "ambiguous": [],
 "skipped_filled": [],
 "validate_ok": true
}
```

(The real output puts every field on its own line; it is shortened here for reading.) How to read it: each entry in `filled` with `"verified": true` was checked after re-opening the saved file. If `missing`, `ambiguous` and `skipped_filled` are all `[]` and the exit code is 0, it worked.

**Troubleshooting**

| Symptom | Cause | Fix |
|---|---|---|
| `python-hwpx is not installed. Run: pip install python-hwpx` | The Python tool is missing. | Run `pip install python-hwpx` (`pip3` on Mac), then retry. |
| `Error: form file not found: nofile.hwpx` (exit code 2) | Wrong path to the form. | Check the name and folder; use a path that works from your current terminal folder. |
| `Error: values file not found: nov.json` (exit code 2) | The values file does not exist. | Copy `values.example.json` and edit it. |
| `skipped_filled` lists cells and the exit code is 1 | Those cells already hold text, so they were not overwritten. | Add `--force` only if overwriting is fine. Otherwise leave them. |
| A name shows up in `missing` | No cell has that label. Spaces are ignored but other differences are not. | Run `inspect` to see the real labels and match them in `values.json`. |
| A name shows up in `ambiguous` | The same label appears in two or more places. | Address the cell directly: `"#0:2,1": "value"` (table 0, row 2, column 1). |

Install: `/plugin marketplace add reedodev/hwpx-form-fill` then `/plugin install hwpx-form-fill@hwpx-form-fill`, or copy `skills/hwpx-form-fill` into `~/.claude/skills/`. See [SKILL.md](skills/hwpx-form-fill/SKILL.md) for the full procedure.

**Credits.**
- Uses [airmang/python-hwpx](https://github.com/airmang/python-hwpx) for HWPX I/O; label lookup, overwrite guard and verification are original. Points to [hwp2hwpx](https://github.com/neolord0/hwp2hwpx), [LibreOffice](https://www.libreoffice.org/) and [H2Orestart](https://github.com/ebandal/H2Orestart). For authoring whole documents see [jkf87/hwpx-skill](https://github.com/jkf87/hwpx-skill) and [airmang/hwpx-plugins](https://github.com/airmang/hwpx-plugins).

MIT License; third-party models, fonts and libraries keep their own licenses.