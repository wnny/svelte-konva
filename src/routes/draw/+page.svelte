<script lang="ts">
  import Konva from 'konva';

  let container = $state<HTMLDivElement | null>(null);
  let currentColor = $state('#2563eb');
  let currentWidth = $state(5);
  let currentTool = $state<'pen' | 'eraser'>('pen');
  
  let stage: Konva.Stage;
  let bgLayer: Konva.Layer;
  let drawingLayer: Konva.Layer;
  let isDrawing = false;
  let currentLine: Konva.Line;
  let linesArray: Konva.Line[] = [];

  $effect(() => {
    if (!container) return;

    stage = new Konva.Stage({
      container: container,
      width: 800,
      height: 600,
    });
    
    bgLayer = new Konva.Layer();
    drawingLayer = new Konva.Layer();
    stage.add(bgLayer);
    stage.add(drawingLayer);

    const background = new Konva.Rect({
      x: 0, y: 0, width: stage.width(), height: stage.height(),
      fill: 'white', listening: false,
    });
    bgLayer.add(background);

    // ✨ [핵심] 확대/축소된 캔버스 비율에 맞게 정확한 마우스 좌표를 계산하는 함수
    function getRelativePointerPosition(stage: Konva.Stage) {
      const pointer = stage.getPointerPosition();
      if (!pointer) return null;
      const scale = stage.scaleX();
      return {
        x: (pointer.x - stage.x()) / scale,
        y: (pointer.y - stage.y()) / scale
      };
    }

    stage.on('mousedown touchstart', () => {
      isDrawing = true;
      const pos = getRelativePointerPosition(stage); // ✨ 비율이 적용된 좌표 사용
      if (!pos) return;

      currentLine = new Konva.Line({
        stroke: currentColor,
        strokeWidth: currentWidth,
        lineCap: 'round',
        lineJoin: 'round',
        tension: 0.5,
        points: [pos.x, pos.y, pos.x, pos.y], 
        globalCompositeOperation: currentTool === 'eraser' ? 'destination-out' : 'source-over',
      });
      
      drawingLayer.add(currentLine);
      linesArray.push(currentLine);
    });

    stage.on('mousemove touchmove', (e) => {
      if (!isDrawing) return;
      e.evt.preventDefault();
      
      const pos = getRelativePointerPosition(stage); // ✨ 비율이 적용된 좌표 사용
      if (!pos) return;

      const newPoints = currentLine.points().concat([pos.x, pos.y]);
      currentLine.points(newPoints);
      drawingLayer.batchDraw();
    });

    stage.on('mouseup touchend', () => {
      isDrawing = false;
    });

    // ✨ [추가] 마우스 휠을 이용한 확대/축소 (Zoom in/out) 기능
    stage.on('wheel', (e) => {
      e.evt.preventDefault(); // 페이지 스크롤 방지
      const scaleBy = 1.1; // 한 번 휠을 굴릴 때 커지는 비율
      const oldScale = stage.scaleX();
      const pointer = stage.getPointerPosition();
      if (!pointer) return;

      // 마우스가 위치한 곳을 기준으로 확대/축소 중심점 계산
      const mousePointTo = {
        x: (pointer.x - stage.x()) / oldScale,
        y: (pointer.y - stage.y()) / oldScale,
      };

      // 휠을 위로 올리면 확대, 아래로 내리면 축소
      const newScale = e.evt.deltaY < 0 ? oldScale * scaleBy : oldScale / scaleBy;
      
      // 너무 과도하게 축소되거나 확대되는 것을 방지 (0.5배 ~ 5배 제한)
      if (newScale < 0.5 || newScale > 5) return;

      stage.scale({ x: newScale, y: newScale });
      
      // 마우스 중심점을 유지하면서 캔버스 위치 이동
      const newPos = {
        x: pointer.x - mousePointTo.x * newScale,
        y: pointer.y - mousePointTo.y * newScale,
      };
      
      stage.position(newPos);
      stage.batchDraw();
    });

    return () => stage.destroy();
  });

  function handleUndo() {
    if (linesArray.length > 0) {
      const lastLine = linesArray.pop();
      lastLine?.destroy();
      drawingLayer.batchDraw();
    }
  }

  function handleClear() {
    // ✨ [추가] 모두 지우기 전 확인(Confirm) 창 띄우기
    if (confirm("캔버스를 정말 모두 지우시겠습니까?\n이 작업은 복구할 수 없습니다.")) {
      drawingLayer.destroyChildren();
      linesArray = [];
      drawingLayer.batchDraw();
      
      // 지우고 나면 캔버스의 확대 비율과 위치도 원상복구 시킴 (옵션)
      stage.scale({ x: 1, y: 1 });
      stage.position({ x: 0, y: 0 });
    }
  }

  function handleDownload() {
    if (!stage) return;
    const dataURL = stage.toDataURL({ mimeType: 'image/png', pixelRatio: 2 });
    const link = document.createElement('a');
    link.download = 'yumi-artwork.png';
    link.href = dataURL;
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  }
</script>

<svelte:head>
  <title>캔버스 - Yumi Draw</title>
</svelte:head>

<div class="page-wrapper">
  <div class="board-container">
    <div class="canvas-wrapper" bind:this={container}></div>

    <div class="floating-toolbar">
      
      <div class="tool-group toggle-group">
        <button 
          class="icon-btn toggle-btn" 
          class:active={currentTool === 'pen'} 
          onclick={() => currentTool = 'pen'} 
          title="펜 모드"
        >
          ✏️
        </button>
        <button 
          class="icon-btn toggle-btn" 
          class:active={currentTool === 'eraser'} 
          onclick={() => currentTool = 'eraser'} 
          title="지우개 모드"
        >
          🧽
        </button>
      </div>

      <div class="divider"></div>

      <div class="tool-group" style="opacity: {currentTool === 'eraser' ? 0.3 : 1}; transition: opacity 0.2s;">
        <div class="color-picker-wrapper" style="background-color: {currentColor}">
          <input type="color" bind:value={currentColor} disabled={currentTool === 'eraser'} />
        </div>
      </div>

      <div class="tool-group width-control">
        <span class="dot-icon" style="width: {Math.max(4, currentWidth/2)}px; height: {Math.max(4, currentWidth/2)}px;"></span>
        <input type="range" min="1" max="50" bind:value={currentWidth} class="modern-slider" />
        <span class="width-value">{currentWidth}px</span>
      </div>

      <div class="divider"></div>

      <button class="icon-btn" onclick={handleUndo} title="되돌리기">↩️</button>
      <button class="icon-btn danger" onclick={handleClear} title="모두 지우기">🗑️</button>

      <div class="divider"></div>

      <button class="primary-btn" onclick={handleDownload}>💾 저장하기</button>

    </div>
  </div>
</div>

<style>
  /* 기본 레이아웃 및 캔버스 스타일 유지 */
  .page-wrapper {
    display: flex; justify-content: center; align-items: center;
    min-height: calc(100vh - 64px); padding: 24px; background-color: #f8fafc;
  }
  .board-container { position: relative; display: flex; flex-direction: column; align-items: center; }
  .canvas-wrapper {
    width: 800px; height: 600px; background-color: white; border-radius: 24px;
    box-shadow: 0 10px 40px -10px rgba(0, 0, 0, 0.1); overflow: hidden;
    cursor: crosshair; border: 1px solid #e2e8f0;
  }

  .floating-toolbar {
    position: absolute; bottom: 32px; display: flex; align-items: center; gap: 16px;
    padding: 12px 24px; background: rgba(255, 255, 255, 0.85);
    backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
    border: 1px solid rgba(255, 255, 255, 0.5); border-radius: 9999px;
    box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s;
  }
  .floating-toolbar:hover { transform: translateY(-2px); }
  .tool-group { display: flex; align-items: center; gap: 12px; }
  
  /* 🌟 [추가] 토글 그룹 스타일 */
  .toggle-group {
    background-color: #f1f5f9;
    padding: 4px;
    border-radius: 9999px;
    gap: 4px;
  }
  
  .toggle-btn {
    border-radius: 50%;
  }

  /* 현재 선택된 도구 (펜/지우개) 강조 효과 */
  .toggle-btn.active {
    background-color: white;
    box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    transform: scale(1.1);
  }

  .color-picker-wrapper {
    width: 36px; height: 36px; border-radius: 50%; overflow: hidden; position: relative;
    box-shadow: inset 0 0 0 2px rgba(0,0,0,0.1); cursor: pointer; transition: transform 0.2s;
  }
  .color-picker-wrapper:hover { transform: scale(1.1); }
  .color-picker-wrapper input[type="color"] {
    position: absolute; top: -10px; left: -10px; width: 60px; height: 60px; opacity: 0; cursor: pointer;
  }
  
  .width-control { background: #f1f5f9; padding: 6px 16px; border-radius: 9999px; }
  .dot-icon { background-color: #475569; border-radius: 50%; display: inline-block; transition: all 0.1s; }
  .width-value { font-size: 13px; font-weight: 700; color: #475569; min-width: 32px; text-align: right; }
  .modern-slider { 
    -webkit-appearance: none; 
    appearance: none; /* ✨ 이 줄을 추가하세요! */
    width: 100px; 
    height: 6px; 
    background: #cbd5e1; 
    border-radius: 4px; 
    outline: none; 
  }
  .modern-slider::-webkit-slider-thumb {
    -webkit-appearance: none; appearance: none; width: 18px; height: 18px; border-radius: 50%;
    background: white; box-shadow: 0 2px 4px rgba(0,0,0,0.2); cursor: pointer; transition: transform 0.1s;
  }
  .modern-slider::-webkit-slider-thumb:hover { transform: scale(1.2); }
  .divider { width: 2px; height: 24px; background-color: #e2e8f0; border-radius: 1px; }
  
  .icon-btn {
    width: 40px; height: 40px; border-radius: 50%; border: none; background: transparent;
    font-size: 18px; cursor: pointer; display: flex; align-items: center; justify-content: center; transition: all 0.2s;
  }
  .icon-btn:hover:not(.active) { background: #e2e8f0; transform: scale(1.05); }
  .icon-btn.danger:hover { background: #fef2f2; }
  
  .primary-btn {
    padding: 10px 20px; background-color: #0f172a; color: white; border: none; border-radius: 9999px;
    font-size: 15px; font-weight: 700; cursor: pointer; display: flex; align-items: center; gap: 6px; transition: all 0.2s;
  }
  .primary-btn:hover { background-color: #334155; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1); transform: scale(1.02); }
</style>