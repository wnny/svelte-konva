# 🎨 Yumi Draw (유미 드로우)

> 빠르고 가벼운 나만의 웹 기반 드로잉 보드 🖌️

[![Svelte 5](https://img.shields.io/badge/Svelte_5-FF3E00?style=for-the-badge&logo=svelte&logoColor=white)](https://svelte.dev/)
[![Konva.js](https://img.shields.io/badge/Konva.js-0D83CD?style=for-the-badge&logo=javascript&logoColor=white)](https://konvajs.org/)
[![GitHub Pages](https://img.shields.io/badge/Deployed_on-GitHub_Pages-222222?style=for-the-badge&logo=github&logoColor=white)]()

Yumi Draw는 **Svelte 5**의 최신 상태 관리(Runes)와 **Konva.js**의 강력한 캔버스 렌더링 엔진을 결합하여 만든 웹 기반 스케치북입니다. 무거운 설치나 로그인 없이 브라우저에서 바로 아이디어를 스케치하고 이미지로 저장할 수 있습니다.

## ✨ 주요 기능 (Features)

- **자유 드로잉 & 지우개**: 마우스 및 모바일 터치를 완벽하게 지원하는 부드러운 드로잉
- **스마트 레이어**: 그림과 배경 레이어를 분리하여, 지우개 사용 시 캔버스 배경이 손상되지 않음
- **커스텀 브러시**: 직관적인 UI(플로팅 툴바)를 통한 색상 및 굵기(1px~50px) 조절
- **무한 캔버스 줌**: 마우스 휠을 이용한 부드러운 캔버스 확대/축소 (Zoom In/Out) 지원
- **실행 취소 & 전체 지우기**: `Undo` 기능과 실수 방지를 위한 전체 지우기 확인창 제공
- **고화질 이미지 저장**: 그린 그림을 `.png` 형식으로 즉시 다운로드 (AI 모델 학습용 224x224 리사이징 기능 포함)
- **Glassmorphism UI**: 화면에 예쁘게 떠 있는 아이패드 감성의 모던한 툴바 디자인

## 🛠️ 기술 스택 (Tech Stack)

- **Framework**: SvelteKit (Svelte 5)
- **Language**: TypeScript, CSS3
- **Canvas Engine**: Konva.js
- **Deployment**: GitHub Pages (`@sveltejs/adapter-static`)
- **Typography**: Asta Sans (Google Fonts)

## 🚀 시작하기 (Getting Started)

로컬 환경에서 직접 실행해 보려면 아래 명령어를 순서대로 입력하세요.

### 패키지 설치
```bash
pnpm install