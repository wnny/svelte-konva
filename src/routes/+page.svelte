<script lang="ts">
  // Svelte 5 정적 페이지
</script>

<svelte:head>
  <title>Yumi Draw - 크리에이티브 드로잉 보드</title>
</svelte:head>

<div class="bg-pattern"></div>

<main class="home-container">
  <section class="hero">
    <div class="hero-text fade-up">
      <div class="badge">✨ Svelte 5 + Konva.js</div>
      <h1>나만의 아이디어를<br/>자유롭게 스케치하세요</h1>
      <p class="subtitle">
        가볍고 빠른 웹 기반 드로잉 보드입니다.<br/>설치 없이 브라우저에서 바로 그림을 그리고 메모를 남겨보세요.
      </p>
      
      <a href="/draw" class="cta-button">
        그리기 시작하기
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M5 12h14M12 5l7 7-7 7"/>
        </svg>
      </a>
    </div>

    <div class="hero-visual fade-up delay-1">
      <div class="mock-canvas">
        <div class="mock-header">
          <div class="dot red"></div>
          <div class="dot yellow"></div>
          <div class="dot green"></div>
        </div>
        <div class="mock-body">
          <svg viewBox="0 0 200 100" class="mock-drawing">
            <path d="M10,50 Q40,10 90,50 T180,40" fill="none" stroke="#2563eb" stroke-width="6" stroke-linecap="round" />
            <path d="M30,80 Q80,90 120,60" fill="none" stroke="#db2777" stroke-width="6" stroke-linecap="round" />
          </svg>
          <div class="floating-brush">🖌️</div>
        </div>
      </div>
      <div class="blob blob-1"></div>
      <div class="blob blob-2"></div>
    </div>
  </section>

  <section class="features">
    <div class="feature-card fade-up delay-1">
      <div class="icon-wrapper blue">✍️</div>
      <h3>자유 드로잉</h3>
      <p>마우스와 모바일 터치를 완벽하게 지원하는 부드러운 펜슬 기능을 제공합니다.</p>
    </div>
    
    <div class="feature-card fade-up delay-2">
      <div class="icon-wrapper pink">🎨</div>
      <h3>다양한 브러시</h3>
      <p>원하는 색상과 굵기를 자유롭게 선택하여 나만의 스타일로 표현할 수 있습니다.</p>
    </div>
    
    <div class="feature-card fade-up delay-3">
      <div class="icon-wrapper yellow">⚡</div>
      <h3>초고속 렌더링</h3>
      <p>Svelte 5의 최신 문법과 Konva 엔진을 적용하여 끊김 없이 매끄럽게 동작합니다.</p>
    </div>
  </section>
</main>

<style>
  /* 배경 도트 패턴 생성 */
  .bg-pattern {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    z-index: -1;
    background-color: #f8fafc;
    background-image: radial-gradient(#cbd5e1 1px, transparent 1px);
    background-size: 24px 24px;
    opacity: 0.5;
  }

  .home-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 24px;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: calc(100vh - 64px); /* 헤더 제외 높이 */
  }

  /* 히어로 섹션 (좌우 2단 레이아웃으로 변경) */
  .hero {
    width: 100%;
    margin-top: 80px;
    margin-bottom: 120px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 40px;
  }

  .hero-text {
    flex: 1;
    max-width: 540px;
  }

  .badge {
    display: inline-block;
    padding: 6px 16px;
    background-color: white;
    color: #3b82f6;
    border: 1px solid #bfdbfe;
    border-radius: 9999px;
    font-size: 14px;
    font-weight: 700;
    margin-bottom: 24px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  }

  h1 {
    font-size: 52px;
    font-weight: 900;
    line-height: 1.2;
    margin: 0 0 24px 0;
    background: linear-gradient(135deg, #1e3a8a, #2563eb, #db2777);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    letter-spacing: -1px;
  }

  .subtitle {
    font-size: 18px;
    color: #475569;
    line-height: 1.7;
    margin: 0 0 40px 0;
  }

  .cta-button {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: linear-gradient(135deg, #0f172a, #334155);
    color: white;
    padding: 16px 36px;
    border-radius: 14px;
    font-size: 18px;
    font-weight: 700;
    text-decoration: none;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: 0 10px 15px -3px rgba(15, 23, 42, 0.3);
  }

  .cta-button:hover {
    transform: translateY(-4px);
    box-shadow: 0 20px 25px -5px rgba(15, 23, 42, 0.4);
  }

  /* 우측 가상 캔버스 아트워크 */
  .hero-visual {
    flex: 1;
    display: flex;
    justify-content: center;
    position: relative;
  }

  .mock-canvas {
    width: 100%;
    max-width: 440px;
    background: white;
    border-radius: 20px;
    border: 1px solid #e2e8f0;
    box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.15);
    position: relative;
    z-index: 10;
    animation: float 6s ease-in-out infinite; /* 둥둥 떠다니는 애니메이션 */
  }

  .mock-header {
    height: 40px;
    border-bottom: 1px solid #f1f5f9;
    display: flex;
    align-items: center;
    padding: 0 16px;
    gap: 8px;
  }

  .dot { width: 12px; height: 12px; border-radius: 50%; }
  .dot.red { background-color: #ef4444; }
  .dot.yellow { background-color: #f59e0b; }
  .dot.green { background-color: #10b981; }

  .mock-body {
    height: 280px;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }

  .mock-drawing {
    width: 80%;
    height: 80%;
    opacity: 0.8;
  }

  .floating-brush {
    position: absolute;
    right: 30px;
    bottom: 30px;
    font-size: 40px;
    animation: brushDraw 4s infinite alternate ease-in-out;
  }

  /* 배경 장식용 블랍(가우시안 블러 효과) */
  .blob {
    position: absolute;
    border-radius: 50%;
    filter: blur(60px);
    z-index: 0;
    opacity: 0.4;
  }
  .blob-1 {
    width: 250px; height: 250px;
    background-color: #3b82f6;
    top: -40px; right: -20px;
  }
  .blob-2 {
    width: 200px; height: 200px;
    background-color: #f43f5e;
    bottom: -20px; left: 40px;
  }

  /* 기능 소개 섹션 */
  .features {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 32px;
    width: 100%;
    margin-bottom: 100px;
  }

  .feature-card {
    background: white;
    padding: 40px 32px;
    border-radius: 24px;
    border: 1px solid #f1f5f9;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
  }

  .feature-card:hover {
    transform: translateY(-8px);
    box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1);
    border-color: #e2e8f0;
  }

  .icon-wrapper {
    width: 64px;
    height: 64px;
    border-radius: 16px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 32px;
    margin-bottom: 24px;
  }
  .icon-wrapper.blue { background-color: #eff6ff; }
  .icon-wrapper.pink { background-color: #fdf2f8; }
  .icon-wrapper.yellow { background-color: #fefce8; }

  .feature-card h3 {
    margin: 0 0 16px 0;
    font-size: 22px;
    font-weight: 800;
    color: #0f172a;
  }

  .feature-card p {
    margin: 0;
    color: #64748b;
    line-height: 1.6;
    font-size: 16px;
  }

  /* --- 애니메이션 정의 --- */
  @keyframes float {
    0% { transform: translateY(0px); }
    50% { transform: translateY(-15px); }
    100% { transform: translateY(0px); }
  }

  @keyframes brushDraw {
    0% { transform: translate(0, 0) rotate(0deg); }
    50% { transform: translate(-30px, -20px) rotate(-15deg); }
    100% { transform: translate(10px, -40px) rotate(10deg); }
  }

  /* 등장 애니메이션 (Fade up) */
  .fade-up {
    opacity: 0;
    transform: translateY(30px);
    animation: fadeUpAnim 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
  }
  .delay-1 { animation-delay: 0.2s; }
  .delay-2 { animation-delay: 0.4s; }
  .delay-3 { animation-delay: 0.6s; }

  @keyframes fadeUpAnim {
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  /* 반응형 (모바일 뷰) */
  @media (max-width: 992px) {
    .hero {
      flex-direction: column;
      text-align: center;
      margin-top: 40px;
    }
    .hero-text { max-width: 100%; }
    h1 { font-size: 42px; }
    .hero-visual { width: 100%; margin-top: 40px; }
    .features { grid-template-columns: 1fr; }
  }
</style>