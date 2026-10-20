<p align="center"><img src="docs/banner.png" alt="한글 양식 채우기: 한컴오피스 없이 신청서 칸을 채웁니다" width="100%"></p>

<p align="center">
<a href="https://github.com/reedodev/hwpx-form-fill/actions/workflows/test.yml"><img src="https://github.com/reedodev/hwpx-form-fill/actions/workflows/test.yml/badge.svg" alt="test"></a>
<img src="https://img.shields.io/badge/Claude_Code-skill-2547F4" alt="Claude Code skill">
<img src="https://img.shields.io/badge/license-MIT-black" alt="MIT">
</p>

# 한글 양식 채우기 (hwpx-form-fill)

한글(HWPX) 신청서·공문 양식의 표 칸을 라벨로 찾아 채우고, 이미 내용이 있는 칸은 덮어쓰지 않으며, 저장한 파일을 다시 열어 들어간 글자를 확인합니다. 못 찾은 칸이 있으면 완료라고 하지 않습니다.

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

## 만든 사람

**리도**, AI 좌충우돌 실습 중인 20년차 엔지니어입니다. 매일 AI에게 일을 맡기다가 막힌 부분을 고쳐 가며 이 스킬을 만들었습니다.
[YouTube @Reedodev](https://www.youtube.com/@Reedodev) · [Threads @reedo.dev](https://www.threads.com/@reedo.dev) · [GitHub @reedodev](https://github.com/reedodev)

같이 만든 다른 스킬: [옵시디언 3D 지식그래프](https://github.com/reedodev/obsidian-brain-3d) · [노트 먼저 찾아보기](https://github.com/reedodev/vault-recall) · [작업 기록 강의자료](https://github.com/reedodev/work-to-lecture-deck) · [한국어 말투 점검](https://github.com/reedodev/korean-tone-check) · [네이버 황금키워드](https://github.com/reedodev/naver-golden-keywords) · [TTS 끝소리 고치기](https://github.com/reedodev/tts-tail-fix) · [카드뉴스 렌더](https://github.com/reedodev/html-card-render) · [영상 비율 검사](https://github.com/reedodev/social-video-gate) · [유튜브 게시 점검](https://github.com/reedodev/youtube-publish-checklist)

---

## English

Fill Hangul (HWPX) form tables by label without Hancom Office, refuse to overwrite filled cells, re-open the file to verify, and fail loudly on missing or ambiguous labels.

Install: `/plugin marketplace add reedodev/hwpx-form-fill` then `/plugin install hwpx-form-fill@hwpx-form-fill`, or copy `skills/hwpx-form-fill` into `~/.claude/skills/`. See [SKILL.md](skills/hwpx-form-fill/SKILL.md) for the full procedure. MIT License; third-party models, fonts and libraries keep their own licenses.
