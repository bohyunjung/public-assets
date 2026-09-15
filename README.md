# public-assets

공개 이미지/파일 호스팅용 저장소.

## URL 규칙

파일 `blog/example.png` 기준:

| 방식 | URL | 캐시 |
| --- | --- | --- |
| **GitHub Pages** (기본) | `https://bohyunjung.github.io/public-assets/blog/example.png` | `max-age=600` |
| jsDelivr (캐시 중요할 때) | `https://cdn.jsdelivr.net/gh/bohyunjung/public-assets@main/blog/example.png` | `max-age=604800` |
| raw (임시/fallback) | `https://raw.githubusercontent.com/bohyunjung/public-assets/main/blog/example.png` | `max-age=300` |

기본은 Pages. Fastly CDN을 타고 `content-type`, `access-control-allow-origin: *` 다 제대로 붙는다.

jsDelivr는 캐시가 훨씬 길다. `@main` 대신 커밋 SHA를 박으면
(`@57b2e2b/blog/example.png`) immutable로 잡혀서 무효화 걱정이 없다.

raw는 Pages 빌드(30초~1분)를 기다리기 싫을 때만.

> **주의 — Pages 경로가 살아있는 이유**
> `bohyunjung.github.io/<repo>/` 형태의 프로젝트 페이지는, 유저 페이지
> (`bohyunjung.github.io` 레포)에 커스텀 도메인이 걸려 있으면 **전부** 그 도메인으로
> 301 리다이렉트되어 막힌다. 그래서 그쪽 `cname` 설정을 제거하고 `gh-pages` 브랜치의
> 리다이렉트 shim으로 대체해 둔 상태다. 거기에 커스텀 도메인을 다시 걸면
> 이 레포의 Pages URL은 즉시 전부 깨진다. 그땐 jsDelivr나 raw로 갈아타야 한다.

## 디렉토리

- `blog/` — 블로그 글에 쓰는 이미지
- `talks/` — 발표 자료, 슬라이드 캡처
- `misc/` — 그 외

## 규칙

1. **덮어쓰지 않는다.** 같은 이름으로 재업로드하면 git 히스토리에 매 버전이 영구히 쌓이고,
   CDN 캐시 때문에 갱신도 바로 안 된다. 수정본은 새 파일명으로 올린다.
   예: `chart.png` → `chart-v2.png` 또는 `chart-20260915.png`
2. **민감한 건 절대 올리지 않는다.** 퍼블릭이고, 지워도 히스토리·포크·CDN 캐시에 남는다.
3. 파일명은 소문자 + 하이픈. 공백·한글 금지 (URL 인코딩 지옥 방지).
4. 올리기 전에 압축한다. `pngquant`, `oxipng`, `cwebp` 등.

## 제한

- 파일 1개 100MB (50MB부터 경고)
- 저장소 권장 1GB / 하드 5GB
- Pages 대역폭 100GB/월 (soft limit)

## 업로드

```sh
cp ~/Desktop/shot.png blog/my-post-hero.png
git add blog/my-post-hero.png
git commit -m "Add hero image for my-post"
git push
```

Pages 반영까지 보통 30초~1분. 그 전엔 raw 링크로 접근 가능.
