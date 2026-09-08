# KASILog

한국천문연구원 사업관리실 연구행정 안내 사이트. https://www.kasilog.workers.dev/

## 폴더 구조 (2026-09-08 정리)

```
upload/          ★ 사이트 파일. 여기만 고친다
  index.html                    첫 화면 (검색·일정·공고·챗봇·자료·우체통·피드백)
  kasi-allowance-calculator.html  연구수당 한도 계산기
  kasi-budget-helper.html         연구비 편성 도우미
  KASI_수탁사업_안내문.pdf 외 자료·폰트
wrangler.toml    배포 설정 (assets = ./upload)
scrape_kasa.py   우주항공청 공고 수집 (GitHub Actions가 하루 2회 실행)
kasa_feed.json   수집 결과. 사이트가 raw.githubusercontent 로 직접 읽는다
```

**최상위에 사이트 파일을 두지 않는다.** 2026-09-08 이전에는 최상위와 `upload/`에 두 벌이 있어
어느 것을 배포했는지 헷갈렸고, 실제로 옛 파일이 올라간 적이 있다.

## 배포

**GitHub 연결 후 (목표):** `main`에 푸시하면 Cloudflare가 `upload/`를 자동 배포한다.

**연결 전 (현재):** Cloudflare 대시보드 → Workers 및 Pages → `www` → 배포에서
`upload/` 안의 파일을 직접 올린다.

반영 확인:

```bash
curl -s https://www.kasilog.workers.dev/ | grep -o 'data-cf-beacon=.\{0,50\}'
```

## 사용량 측정

두 곳을 나눠 본다. 자세한 내용은 `kasi-office/docs/kasilog-개선메모.md`.

- **Cloudflare Web Analytics** — 방문자 수, 유입 경로, 어느 페이지를 봤나. 보관 기간이 있다.
- **기능별 누적 카운터** — 어떤 기능을 몇 번 눌렀나. 영구 누적. 화면에는 표시하지 않는다.
  조회: `python ../kasi-office/tools/kasilog_stats.py --md`

## 주의

- `upload/index.html` 은 줄바꿈이 **CRLF**, 계산기 두 파일은 **LF** 다. 편집 도구가 바꾸지 않도록 주의한다.
  형식이 바뀌면 변경 내역이 1000줄 넘게 부풀어 검토가 불가능해진다.
- 사이트에 보안과제 정보를 넣지 않는다. 공개 도메인이다.
