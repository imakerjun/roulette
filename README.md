# Marble Roulette

구슬을 떨어뜨려 당첨자를 뽑는 웹 기반 럭키드로우 게임입니다.

**[Demo](https://lazygyu.github.io/roulette)**

## Features

- **물리 엔진 기반** — [box2d-wasm](https://github.com/Birch-san/box2d-wasm)을 사용한 실시간 물리 시뮬레이션
- **다양한 맵** — Wheel of fortune, BubblePop, Pot of greed 등 여러 맵 선택 가능
- **스킬 시스템** — 구슬마다 쿨타임 기반으로 충격파(Impact) 스킬 발동
- **가중치 & 복수 입력** — 이름에 `/N`(가중치), `*N`(복수) 문법 지원 (예: `Alice/3*2`)
- **우승 조건 설정** — 첫 번째 도착, 마지막 도착, 또는 N등 도착 선택
- **빨리감기** — 캔버스 중앙을 누르면 빨리감기
- **미니맵** — 드래그로 뷰포트 이동 가능
- **자동 녹화** — 게임 과정을 영상으로 녹화
- **다크/라이트 테마**
- **다국어 지원** — 한국어, 영어 (브라우저 언어 자동 감지)
- **커스텀 스킨** — [MarbleRouletteShop](https://marblerouletteshop.com)과 연동
- **PWA** — 오프라인 지원, 홈 화면에 추가 가능
- **URL 공유** — `?names=A,B,C` 쿼리 파라미터로 이름 전달 가능

## Usage

1. 텍스트 영역에 이름을 입력합니다 (쉼표 또는 줄바꿈으로 구분)
2. 필요에 따라 맵, 스킬, 우승 조건 등을 설정합니다
3. **Shuffle** 버튼으로 구슬을 배치하고 **Start** 버튼으로 게임을 시작합니다

### Name Syntax

| 입력 | 의미 |
|------|------|
| `Alice` | Alice 구슬 1개 |
| `Alice*3` | Alice 구슬 3개 |
| `Alice/5` | Alice 구슬 1개 (가중치 5, 크기에 반영) |
| `Alice/5*2` | 가중치 5인 Alice 구슬 2개 |

## Development

```shell
yarn          # 의존성 설치
yarn dev      # 개발 서버 실행 (http://localhost:1235)
```

## Build

```shell
yarn build    # 프로덕션 빌드 (dist/)
```

## Tech Stack

- TypeScript
- Parcel (번들러)
- box2d-wasm (물리 엔진)
- Canvas2D (렌더링)
- Workbox (서비스 워커 / PWA)
- SCSS (스타일)

## License

[MIT](LICENSE) &copy; LazyGyu
