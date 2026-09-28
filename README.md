# 연구안보DB

Astro 정적 사이트. 기준 문서는 `CLAUDE.md`, 데이터 배치는 `docs/data-layout.md`, 원본과 달라진 점은 `docs/design-changes.md`.

## 설치

```bash
npm install
```

캡처와 동작 확인 스크립트는 Playwright 브라우저가 필요하다 (`npx playwright install chromium firefox webkit`).

## 명령

```bash
npm run dev      # 개발 서버
npm run build    # 타입·스키마 검사 후 dist/에 정적 출력
npm run seed     # design/fixtures + sample.json → data/*.json 임시 데이터 다시 만들기
npm run shots    # 1440px 라이트·다크 캡처 → shots/ (build 뒤)
node scripts/check-cites.mjs   # 근거 카드, 변경 행, 검색, 등재 기관, 비교 방식을 Chromium·Firefox·WebKit에서 확인 (build 뒤)
node scripts/check-links.mjs   # dist/의 사이트 안 링크와 앵커 확인 (build 뒤)
node scripts/check-theme.mjs   # 라이트·다크 전환 버튼을 세 엔진에서 확인 (build 뒤)
python3 scripts/compare.py     # 원본 PNG와 캡처를 나란히 → shots/compare/
```

## 배포 (GitHub Pages)

- 주소: https://casslab-kaist.github.io/research-security-db (`astro.config.mjs`의 `site`, `base`)
- `npm run build` 뒤 `dist/`를 그대로 올린다. `public/.nojekyll`이 들어 있어 `_astro/` 폴더도 그대로 서비스된다.
- 개발 서버 주소도 base 아래다: http://localhost:4321/research-security-db/
- 사이트 안 링크는 꼭 `src/lib/url.ts`의 `url('/lists/el')`를 거친다. 설명 글(HTML) 안의 링크는 `rich()`와 `withBase()`가 붙인다.
  데이터에는 base 없는 경로(`/lists/el`)를 적는다. `node scripts/check-links.mjs`가 base가 빠진 링크를 찾아 준다.
