# public-assets

공개 이미지/파일 호스팅용 저장소.

## URL 규칙

파일 `blog/example.png` 기준:

| 방식 | URL |
| --- | --- |
| GitHub Pages (기본) | `https://bohyunjung.github.io/public-assets/blog/example.png` |
| raw (fallback) | `https://raw.githubusercontent.com/bohyunjung/public-assets/main/blog/example.png` |

Pages 쪽이 Fastly CDN을 타고 캐시 헤더도 제대로 붙으니 기본으로 쓴다.
raw는 Pages 빌드를 기다리기 싫을 때나 임시로만.

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
