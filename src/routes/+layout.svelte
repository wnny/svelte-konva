<script lang="ts">
  // SvelteKit에서 현재 주소(URL)를 알아내기 위해 page 스토어를 가져옵니다.
  import { page } from '$app/stores';

  // Svelte 5에서는 하위 페이지의 내용을 'children'이라는 이름의 prop으로 받습니다.
  let { children } = $props();
</script>

<header class="navbar">
  <div class="nav-content">
    <a href="/" class="logo">
      <span class="logo-icon">🎨</span>
      Yumi Draw
    </a>

    <nav class="menu">
      <a 
        href="/" 
        class="nav-link" 
        class:active={$page.url.pathname === '/'}
      >
        홈
      </a>
      <a 
        href="/draw" 
        class="nav-link" 
        class:active={$page.url.pathname === '/draw'}
      >
        그리기 보드
      </a>
    </nav>
  </div>
</header>

<main class="page-content">
  {@render children()}
</main>

<style>
:global(body), :global(button), :global(input) {
  font-family: "Asta Sans", sans-serif;
  font-optical-sizing: auto;
  font-style: normal;
}
  /* 헤더를 화면 상단에 고정하고 그림자를 줍니다. */
  .navbar {
    position: sticky;
    top: 0;
    z-index: 100;
    background-color: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(8px); /* 반투명 블러 효과 */
    border-bottom: 1px solid #e2e8f0;
    width: 100%;
  }

  .nav-content {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 24px;
    height: 64px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  /* 로고 스타일 */
  .logo {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 20px;
    font-weight: 800;
    color: #0f172a;
    text-decoration: none;
    letter-spacing: -0.5px;
  }

  .logo-icon {
    font-size: 24px;
  }

  /* 메뉴 스타일 */
  .menu {
    display: flex;
    gap: 24px;
  }

  .nav-link {
    font-size: 15px;
    font-weight: 600;
    color: #64748b;
    text-decoration: none;
    padding: 8px 12px;
    border-radius: 8px;
    transition: all 0.2s;
  }

  .nav-link:hover {
    color: #0f172a;
    background-color: #f1f5f9;
  }

  /* 현재 보고 있는 페이지의 메뉴를 강조(파란색)해줍니다. */
  .nav-link.active {
    color: #2563eb;
    background-color: #eff6ff;
  }

  /* 하위 콘텐츠 영역 설정 */
  .page-content {
    display: flex;
    flex-direction: column;
    min-height: calc(100vh - 64px); /* 전체 높이에서 헤더 높이(64px) 제외 */
  }
</style>