<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>就是一棵树 | Just A Tree · 歌词翻译集</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <style>
    body {
      margin: 0; padding: 0; box-sizing: border-box;
      font-family: '微软雅黑', 'Segoe UI', system-ui, sans-serif;
      background-color: #f0f7e9; overflow-x: hidden;
    }
    * { box-sizing: border-box; }

    /* 开幕页 */
    #splash {
      position: fixed; top: 0; left: 0; width: 100%; height: 100vh;
      background: #2e6b3e; display: flex; flex-direction: column;
      align-items: center; justify-content: center; z-index: 1000;
      cursor: pointer; transition: opacity 0.6s ease;
    }
    .splash-tree {
      font-size: 110px; color: white;
      animation: gentleBlink 2.2s infinite ease-in-out;
      text-shadow: 0 4px 12px rgba(0,0,0,0.1);
      margin-bottom: 20px;
    }
    @keyframes gentleBlink {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.55; }
    }
    .splash-text {
      text-align: center; color: white;
    }
    .splash-text .cn {
      font-size: 48px; font-weight: 400; letter-spacing: 8px; margin-bottom: 8px;
    }
    .splash-text .en {
      font-size: 22px; font-weight: 300; letter-spacing: 4px; opacity: 0.9;
    }
    .hint-text {
      margin-top: 40px; color: rgba(255,255,255,0.7); font-size: 14px;
    }

    #app { display: none; }

    .navbar {
      display: flex; justify-content: space-between; align-items: baseline;
      padding: 20px 32px; background: rgba(255,255,255,0.6); backdrop-filter: blur(8px);
      border-bottom: 1px solid rgba(80,120,70,0.15); position: sticky; top: 0; z-index: 50;
    }
    .nav-left { font-size: 22px; font-weight: 700; color: #1e4a2b; }
    .nav-right { font-size: 18px; font-weight: 600; color: #2d5e3b; }

    .home-container { max-width: 1100px; margin: 30px auto; padding: 0 24px 80px; }
    .cards-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 28px; }

    .card {
      height: 300px; border-radius: 24px; box-shadow: 0 12px 28px rgba(0,20,0,0.12);
      background-size: cover; background-position: center; background-color: #c0dbc0;
      position: relative; overflow: hidden; transition: 0.2s; cursor: pointer;
      border: 1px solid rgba(100,130,80,0.2);
    }
    .card:hover { transform: translateY(-4px); box-shadow: 0 20px 32px rgba(30,60,20,0.18); }
    .card-footer {
      position: absolute; bottom: 0; left: 0; width: 100%; height: 20%;
      background: rgba(255,255,255,0.5); backdrop-filter: blur(6px);
      display: flex; align-items: center; padding: 0 18px;
      border-radius: 0 0 24px 24px; font-family: 'Arial', sans-serif; font-weight: bold;
      font-size: 1.4rem; color: #1f3f1a;
    }
    .pin-badge {
      position: absolute; top: 12px; left: 12px; background: #2e6b3e; color: white;
      border-radius: 20px; padding: 4px 10px; font-size: 13px; backdrop-filter: blur(4px);
      z-index: 10;
    }
    .card.edit-mode-card { border: 3px dashed #2e7d5e; }
    .card-edit-tools { 
      position: absolute; top: 8px; right: 8px; display: flex; gap: 8px; z-index: 15; 
    }
    .delete-card-btn, .pin-btn {
      background: rgba(60,60,60,0.7); border: none; color: white; width: 32px; height: 32px;
      border-radius: 50%; display: flex; align-items: center; justify-content: center;
      cursor: pointer; backdrop-filter: blur(4px);
    }
    .delete-card-btn { background: rgba(200,60,60,0.85); }
    .pin-btn { background: #3f7845; }
    .card-controls-panel {
      position: absolute; bottom: 22%; left: 12px; right: 12px;
      background: rgba(255,255,245,0.85); backdrop-filter: blur(8px);
      padding: 12px 14px; border-radius: 20px; display: flex; flex-wrap: wrap; gap: 8px;
      z-index: 20; font-size: 13px; border: 1px solid #b1cfb1;
      pointer-events: auto;
    }
    .card-controls-panel input, .card-controls-panel label, .card-controls-panel button {
      pointer-events: auto;
    }
    .card-controls-panel .bg-upload-label {
      background: #3f7845; color: white; padding: 4px 12px; border-radius: 24px; cursor: pointer;
      font-size: 12px; display: inline-flex; align-items: center; gap: 4px;
    }
    .fab-edit {
      position: fixed; bottom: 30px; right: 30px; width: 64px; height: 64px;
      border-radius: 50%; background: #2e6b3e; color: white; border: none;
      box-shadow: 0 8px 18px rgba(30,70,20,0.35); font-size: 32px;
      display: flex; align-items: center; justify-content: center; cursor: pointer; z-index: 90;
      transition: 0.2s;
    }
    .fab-edit.active-edit { background: #c53a3a; }
    .add-card-btn {
      background: #3f7845; color: white; border: none; border-radius: 50px;
      padding: 12px 24px; font-size: 18px; margin: 20px 0 10px; cursor: pointer;
    }

    #detailView {
      display: none; min-height: 100vh; background-size: cover; background-position: center;
      background-attachment: fixed; background-repeat: no-repeat; background-color: #b7d2b0;
      padding: 20px 0 80px; position: relative;
    }
    .detail-nav { display: flex; padding: 10px 28px; }
    .back-btn { background: rgba(255,255,240,0.7); backdrop-filter: blur(8px); border: none; border-radius: 40px; padding: 10px 22px; font-size: 18px; color: #1e4620; cursor: pointer; font-weight: 600; }
    .content-wrapper { display: flex; justify-content: center; margin-top: 30px; }
    .lyric-editor {
      width: 1000px; max-width: 95vw; background: rgba(255,255,255,0.7);
      backdrop-filter: blur(12px); border-radius: 32px; padding: 30px 32px;
      box-shadow: 0 20px 40px rgba(0,20,0,0.15); border: 1px solid #dceddc;
      min-height: 200px; color: #1c351c; font-size: 17px; line-height: 1.7;
      outline: none; transition: 0.2s; font-family: '微软雅黑', 'Segoe UI', sans-serif;
    }
    .lyric-editor[contenteditable="true"] { background: rgba(255,255,255,0.85); border: 2px dashed #56855e; }
    .detail-fab {
      position: fixed; bottom: 30px; right: 30px; width: 60px; height: 60px;
      border-radius: 50%; background: #2e6b3e; color: white; border: none;
      font-size: 28px; cursor: pointer; z-index: 90;
    }
    .detail-fab.active-edit { background: #c53a3a; }

    .detail-bg-panel {
      position: fixed; top: 30px; right: 30px; background: rgba(240,250,235,0.9);
      backdrop-filter: blur(8px); padding: 16px 24px; border-radius: 48px;
      box-shadow: 0 6px 18px rgba(30,50,20,0.2); display: none; align-items: center;
      gap: 20px; flex-wrap: wrap; z-index: 95; border: 1px solid #a8c9a8;
    }
    .detail-bg-panel label { background: #3f7845; color: white; padding: 8px 18px; border-radius: 40px; cursor: pointer; }
    .scale-control { display: flex; align-items: center; gap: 8px; background: rgba(255,255,240,0.5); padding: 4px 8px; border-radius: 40px; }
    .scale-btn { background: white; border: 1px solid #5e8b5e; width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; cursor: pointer; font-size: 18px; font-weight: bold; }
    .scale-value { min-width: 50px; text-align: center; font-weight: 600; }
    .align-buttons { display: flex; gap: 8px; }
    .align-btn { background: white; border: 1px solid #5e8b5e; width: 36px; height: 36px; border-radius: 50%; display: flex; align-items: center; justify-content: center; cursor: pointer; }
    .align-btn.active { background: #2e6b3e; color: white; }

    .hidden { display: none !important; }
    .auto-save-note {
      position: fixed; bottom: 100px; right: 30px; background: rgba(0,0,0,0.5);
      color: #eee; padding: 6px 14px; border-radius: 40px; font-size: 13px;
      backdrop-filter: blur(5px); z-index: 80; opacity: 0; transition: opacity 0.2s;
      pointer-events: none;
    }
    .storage-warning {
      position: fixed; bottom: 20px; left: 20px; background: #d9534f; color: white;
      padding: 6px 12px; border-radius: 24px; font-size: 12px; z-index: 200;
      opacity: 0; transition: 0.2s; pointer-events: none;
    }
  </style>
</head>
<body>

<div id="splash">
  <div class="splash-tree"><i class="fas fa-tree"></i></div>
  <div class="splash-text">
    <div class="cn">就是一棵树</div>
    <div class="en">JUST A TREE</div>
  </div>
  <div class="hint-text">点击任意位置进入</div>
</div>

<div id="app">
  <div id="homeView">
    <div class="navbar">
      <div class="nav-left">就是一棵树 | JUST A TREE</div>
      <div class="nav-right">歌词翻译集</div>
    </div>
    <div class="home-container">
      <div class="cards-grid" id="cardsGrid"></div>
      <div id="addCardContainer" style="text-align: center;"></div>
    </div>
    <button class="fab-edit" id="homeEditBtn"><i class="fas fa-tree"></i></button>
    <div class="auto-save-note" id="autoSaveNote">✓ 已自动保存</div>
    <div class="storage-warning" id="storageWarning">⚠️ 存储空间不足，背景图片已压缩</div>
  </div>

  <div id="detailView">
    <div class="detail-nav"><button class="back-btn" id="backToHomeBtn"><i class="fas fa-arrow-left"></i> 返回首页</button></div>
    <div class="content-wrapper"><div class="lyric-editor" id="lyricContent" contenteditable="false"></div></div>
    <button class="detail-fab" id="detailEditBtn"><i class="fas fa-pen"></i></button>
    <div class="detail-bg-panel" id="detailBgPanel">
      <label for="detailBgUploadInput"><i class="fas fa-image"></i> 更换背景</label>
      <input type="file" id="detailBgUploadInput" accept="image/*" style="display:none;">
      <div class="scale-control">
        <button class="scale-btn" id="scaleDownBtn"><i class="fas fa-minus"></i></button>
        <span class="scale-value" id="scaleValueDisplay">1.2x</span>
        <button class="scale-btn" id="scaleUpBtn"><i class="fas fa-plus"></i></button>
      </div>
      <div><span>X</span><input type="range" id="detailBgPosX" min="0" max="100" value="50" style="width:80px;"></div>
      <div><span>Y</span><input type="range" id="detailBgPosY" min="0" max="100" value="50" style="width:80px;"></div>
      <div class="align-buttons">
        <div class="align-btn" data-align="left"><i class="fas fa-align-left"></i></div>
        <div class="align-btn" data-align="center"><i class="fas fa-align-center"></i></div>
        <div class="align-btn" data-align="right"><i class="fas fa-align-right"></i></div>
      </div>
      <small>拖动空白处移动背景</small>
    </div>
  </div>
</div>

<script>
  (function(){
    "use strict";

    // 图片压缩工具
    async function compressImage(file, maxWidth = 900, quality = 0.7) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = (e) => {
          const img = new Image();
          img.onload = () => {
            const canvas = document.createElement('canvas');
            let width = img.width;
            let height = img.height;
            if (width > maxWidth) {
              height = (height * maxWidth) / width;
              width = maxWidth;
            }
            canvas.width = width;
            canvas.height = height;
            const ctx = canvas.getContext('2d');
            ctx.drawImage(img, 0, 0, width, height);
            const compressedDataUrl = canvas.toDataURL('image/jpeg', quality);
            resolve(compressedDataUrl);
          };
          img.onerror = () => reject(new Error('图片加载失败'));
          img.src = e.target.result;
        };
        reader.onerror = () => reject(new Error('文件读取失败'));
        reader.readAsDataURL(file);
      });
    }

    let warningTimeout = null;
    function showStorageWarning() {
      const warnEl = document.getElementById('storageWarning');
      if(!warnEl) return;
      warnEl.style.opacity = '1';
      if(warningTimeout) clearTimeout(warningTimeout);
      warningTimeout = setTimeout(() => { if(warnEl) warnEl.style.opacity = '0'; }, 2500);
    }

    let cards = [];
    let currentDetailId = null;
    let homeEditMode = false;
    let detailEditMode = false;
    const PASSWORD = '0220';
    const autoSaveNote = document.getElementById('autoSaveNote');

    function showAutoSaveHint() {
      if(autoSaveNote) {
        autoSaveNote.style.opacity = '1';
        setTimeout(() => autoSaveNote.style.opacity = '0', 800);
      }
    }

    function saveToStorage() {
      try {
        localStorage.setItem('justatree_cards_v11', JSON.stringify(cards));
        showAutoSaveHint();
      } catch(e) {
        console.warn('存储失败', e);
        if(e.name === 'QuotaExceededError') {
          showStorageWarning();
          alert('存储空间不足，部分背景图片过大可能导致丢失。建议使用较小尺寸图片。');
        }
      }
    }

    function initDefaultCards() {
      return [
        { id: '1', name: 'Blowin’ in the Wind', nameColor: '#1e4620', bgImage: '', bgScale: 1.2, bgPosX: 50, bgPosY: 50,
          contentHtml: '<p><strong>答案在风中飘</strong><br>一个人要走多少路……<br>答案，我的朋友，在风中飘荡。</p>',
          detailBg: '', detailBgScale: 1.2, detailBgPosX: 50, detailBgPosY: 50, textAlign: 'left', pinned: false, pinnedTime: 0 },
        { id: '2', name: 'Imagine', nameColor: '#2b4f2b', bgImage: '', bgScale: 1.0, bgPosX: 50, bgPosY: 50,
          contentHtml: '<p>想象没有天堂，<br>这很容易如果你尝试。<br>我们脚下没有地狱，<br>头顶只有蓝天。</p>',
          detailBg: '', detailBgScale: 1.2, detailBgPosX: 50, detailBgPosY: 50, textAlign: 'left', pinned: false, pinnedTime: 0 },
      ];
    }

    function loadFromStorage() {
      const stored = localStorage.getItem('justatree_cards_v11');
      if(stored) { 
        try { 
          cards = JSON.parse(stored); 
        } catch(e){ cards = []; } 
      }
      if(!cards || cards.length===0) cards = initDefaultCards();
      cards = cards.map(c => ({ 
        ...c, 
        pinned: c.pinned || false, 
        pinnedTime: c.pinnedTime || 0,
        detailBgScale: c.detailBgScale !== undefined ? c.detailBgScale : 1.2,
        detailBgPosX: c.detailBgPosX !== undefined ? c.detailBgPosX : 50,
        detailBgPosY: c.detailBgPosY !== undefined ? c.detailBgPosY : 50,
        textAlign: c.textAlign || 'left',
        bgScale: c.bgScale !== undefined ? c.bgScale : 1.0,
        bgPosX: c.bgPosX !== undefined ? c.bgPosX : 50,
        bgPosY: c.bgPosY !== undefined ? c.bgPosY : 50,
        contentHtml: c.contentHtml || '<p>在这里写下歌词翻译...</p>'
      }));
      sortCards();
    }

    function sortCards() {
      cards.sort((a, b) => {
        if(a.pinned && !b.pinned) return -1;
        if(!a.pinned && b.pinned) return 1;
        if(a.pinned && b.pinned) return b.pinnedTime - a.pinnedTime;
        return a.name.localeCompare(b.name, 'zh-CN');
      });
    }

    const splash = document.getElementById('splash');
    const app = document.getElementById('app');
    const homeView = document.getElementById('homeView');
    const detailView = document.getElementById('detailView');
    const cardsGrid = document.getElementById('cardsGrid');
    const addCardContainer = document.getElementById('addCardContainer');
    const homeEditBtn = document.getElementById('homeEditBtn');
    const backBtn = document.getElementById('backToHomeBtn');
    const lyricContent = document.getElementById('lyricContent');
    const detailEditBtn = document.getElementById('detailEditBtn');
    const detailBgPanel = document.getElementById('detailBgPanel');
    const detailBgUploadInput = document.getElementById('detailBgUploadInput');
    const detailBgPosX = document.getElementById('detailBgPosX');
    const detailBgPosY = document.getElementById('detailBgPosY');
    const scaleDownBtn = document.getElementById('scaleDownBtn');
    const scaleUpBtn = document.getElementById('scaleUpBtn');
    const scaleValueDisplay = document.getElementById('scaleValueDisplay');
    const alignBtns = document.querySelectorAll('.align-btn');

    let currentDetailScale = 1.2;

    function applyCardBgStyle(cardEl, cardData) {
      if(cardData.bgImage && cardData.bgImage.startsWith('data:image')) {
        cardEl.style.backgroundImage = `url(${cardData.bgImage})`;
        cardEl.style.backgroundSize = `${(cardData.bgScale||1)*100}%`;
        cardEl.style.backgroundPosition = `${cardData.bgPosX??50}% ${cardData.bgPosY??50}%`;
      } else {
        cardEl.style.backgroundImage = ''; 
        cardEl.style.backgroundColor = '#c0dbc0';
      }
    }

    function applyDetailStyle(card) {
      if(!card) return;
      currentDetailScale = card.detailBgScale || 1.2;
      if(card.detailBg && card.detailBg.startsWith('data:image')) {
        detailView.style.backgroundImage = `url(${card.detailBg})`;
        detailView.style.backgroundSize = `${currentDetailScale*100}%`;
        detailView.style.backgroundPosition = `${card.detailBgPosX??50}% ${card.detailBgPosY??50}%`;
      } else {
        detailView.style.backgroundImage = '';
        detailView.style.backgroundColor = '#b7d2b0';
      }
      lyricContent.style.textAlign = card.textAlign || 'left';
      updateAlignActive(card.textAlign);
      detailBgPosX.value = card.detailBgPosX??50;
      detailBgPosY.value = card.detailBgPosY??50;
      scaleValueDisplay.textContent = currentDetailScale.toFixed(1)+'x';
    }

    function updateAlignActive(align) {
      alignBtns.forEach(b => b.classList.toggle('active', b.dataset.align === align));
    }

    function saveCurrentDetailContent() {
      if(currentDetailId && detailEditMode) {
        const card = cards.find(c => c.id === currentDetailId);
        if(card && lyricContent.innerHTML !== card.contentHtml) {
          card.contentHtml = lyricContent.innerHTML;
          saveToStorage();
        }
      }
    }

    function escapeHtml(str) { 
      if(!str) return '';
      return str.replace(/[&<>]/g, function(m){
        if(m==='&') return '&amp;';
        if(m==='<') return '&lt;';
        if(m==='>') return '&gt;';
        return m;
      });
    }

    // ----- 重构编辑模式UI：使用DOM构建，确保每个卡片都有完整控制栏 -----
    function enterHomeEditModeUI() {
      const allCards = document.querySelectorAll('.card');
      allCards.forEach(cardEl => {
        const cardId = cardEl.dataset.id;
        const card = cards.find(c => c.id === cardId);
        if(!card) return;
        
        // 移除可能残留的旧工具和面板（避免重复）
        const oldTools = cardEl.querySelector('.card-edit-tools');
        if(oldTools) oldTools.remove();
        const oldPanel = cardEl.querySelector('.card-controls-panel');
        if(oldPanel) oldPanel.remove();
        
        // 添加编辑模式边框样式
        cardEl.classList.add('edit-mode-card');
        
        // 顶部工具按钮 (置顶/删除)
        const toolsDiv = document.createElement('div');
        toolsDiv.className = 'card-edit-tools';
        toolsDiv.innerHTML = `
          <button class="pin-btn" data-id="${cardId}"><i class="fas fa-thumbtack"></i></button>
          <button class="delete-card-btn" data-id="${cardId}"><i class="fas fa-trash"></i></button>
        `;
        cardEl.appendChild(toolsDiv);
        
        // 底部控制面板
        const panel = document.createElement('div');
        panel.className = 'card-controls-panel';
        
        // 名称输入框
        const nameInput = document.createElement('input');
        nameInput.type = 'text';
        nameInput.value = card.name;
        nameInput.style.width = '130px';
        nameInput.placeholder = '歌名';
        // 颜色选择器
        const colorInput = document.createElement('input');
        colorInput.type = 'color';
        colorInput.value = card.nameColor;
        // 背景上传按钮
        const uploadLabel = document.createElement('label');
        uploadLabel.className = 'bg-upload-label';
        uploadLabel.innerHTML = '<i class="fas fa-upload"></i> 背景';
        const fileInput = document.createElement('input');
        fileInput.type = 'file';
        fileInput.accept = 'image/*';
        fileInput.style.display = 'none';
        fileInput.dataset.cardId = cardId;
        uploadLabel.appendChild(fileInput);
        
        // 缩放滑块
        const scaleContainer = document.createElement('div');
        scaleContainer.innerHTML = `<span>缩放</span>`;
        const scaleSlider = document.createElement('input');
        scaleSlider.type = 'range';
        scaleSlider.min = '0.1';
        scaleSlider.max = '10';
        scaleSlider.step = '0.1';
        scaleSlider.value = card.bgScale || 1;
        scaleContainer.appendChild(scaleSlider);
        
        // X/Y 偏移
        const xContainer = document.createElement('div');
        xContainer.innerHTML = '<span>X</span>';
        const xSlider = document.createElement('input');
        xSlider.type = 'range';
        xSlider.min = '0';
        xSlider.max = '100';
        xSlider.value = card.bgPosX ?? 50;
        xSlider.style.width = '60px';
        xContainer.appendChild(xSlider);
        
        const yContainer = document.createElement('div');
        yContainer.innerHTML = '<span>Y</span>';
        const ySlider = document.createElement('input');
        ySlider.type = 'range';
        ySlider.min = '0';
        ySlider.max = '100';
        ySlider.value = card.bgPosY ?? 50;
        ySlider.style.width = '60px';
        yContainer.appendChild(ySlider);
        
        panel.appendChild(nameInput);
        panel.appendChild(colorInput);
        panel.appendChild(uploadLabel);
        panel.appendChild(scaleContainer);
        panel.appendChild(xContainer);
        panel.appendChild(yContainer);
        cardEl.appendChild(panel);
        
        // 绑定事件
        nameInput.addEventListener('input', (e) => {
          card.name = e.target.value;
          const footer = cardEl.querySelector('.card-footer');
          if(footer) footer.textContent = card.name;
          saveToStorage();
        });
        colorInput.addEventListener('input', (e) => {
          card.nameColor = e.target.value;
          const footer = cardEl.querySelector('.card-footer');
          if(footer) footer.style.color = card.nameColor;
          saveToStorage();
        });
        const updateCardBg = () => {
          card.bgScale = parseFloat(scaleSlider.value);
          card.bgPosX = parseFloat(xSlider.value);
          card.bgPosY = parseFloat(ySlider.value);
          applyCardBgStyle(cardEl, card);
          saveToStorage();
        };
        scaleSlider.addEventListener('input', updateCardBg);
        xSlider.addEventListener('input', updateCardBg);
        ySlider.addEventListener('input', updateCardBg);
        
        fileInput.addEventListener('change', async (e) => {
          const f = e.target.files[0];
          if(!f) return;
          try {
            const compressed = await compressImage(f, 800, 0.7);
            card.bgImage = compressed;
            applyCardBgStyle(cardEl, card);
            saveToStorage();
          } catch(err) { console.warn(err); alert('背景处理失败'); }
          fileInput.value = '';
        });
        
        // 置顶/删除按钮事件
        const pinBtn = toolsDiv.querySelector('.pin-btn');
        const delBtn = toolsDiv.querySelector('.delete-card-btn');
        pinBtn.addEventListener('click', (e) => {
          e.stopPropagation();
          togglePin(cardId);
        });
        delBtn.addEventListener('click', (e) => {
          e.stopPropagation();
          if(confirm('确定删除这张卡片吗？')) {
            deleteCard(cardId);
          }
        });
      });
    }
    
    function exitHomeEditModeUI() {
      document.querySelectorAll('.card').forEach(el => {
        el.classList.remove('edit-mode-card');
        const tools = el.querySelector('.card-edit-tools');
        if(tools) tools.remove();
        const panel = el.querySelector('.card-controls-panel');
        if(panel) panel.remove();
      });
    }

    function renderHomeCards() {
      sortCards();
      let html = '';
      cards.forEach(card => {
        html += `<div class="card" data-id="${card.id}">`;
        if(card.pinned) html += `<div class="pin-badge"><i class="fas fa-thumbtack"></i> 置顶</div>`;
        html += `<div class="card-footer" style="color: ${card.nameColor};">${escapeHtml(card.name)}</div></div>`;
      });
      cardsGrid.innerHTML = html;
      // 应用背景样式
      cards.forEach(card => {
        const el = document.querySelector(`.card[data-id="${card.id}"]`);
        if(el) applyCardBgStyle(el, card);
      });
      // 绑定点击打开详情
      document.querySelectorAll('.card').forEach(el => {
        el.addEventListener('click', (e) => { if(!homeEditMode) openDetail(el.dataset.id); });
      });
      // 根据编辑模式渲染控制面板
      if(homeEditMode) {
        enterHomeEditModeUI();
      } else {
        exitHomeEditModeUI();
      }
      renderAddButton();
      homeEditBtn.classList.toggle('active-edit', homeEditMode);
    }
    
    function renderAddButton() {
      if(homeEditMode) {
        addCardContainer.innerHTML = `<button class="add-card-btn" id="addNewCardBtn"><i class="fas fa-plus-circle"></i> 添加新卡片</button>`;
        document.getElementById('addNewCardBtn')?.addEventListener('click', addNewCard);
      } else addCardContainer.innerHTML = '';
    }
    
    async function addNewCard() {
      const newId = Date.now() + '-' + Math.random().toString(36);
      cards.push({
        id: newId, name: '新翻译', nameColor: '#1e4620', bgImage: '', bgScale: 1.0, bgPosX: 50, bgPosY: 50,
        contentHtml: '<p>在此粘贴歌词翻译……</p>', detailBg: '', detailBgScale: 1.2, detailBgPosX: 50, detailBgPosY: 50,
        textAlign: 'left', pinned: false, pinnedTime: 0
      });
      saveToStorage();
      renderHomeCards();  // 重新渲染，保持编辑模式
    }
    
    function deleteCard(cardId) {
      cards = cards.filter(c => c.id !== cardId);
      saveToStorage();
      renderHomeCards();
      if(currentDetailId === cardId) backToHome();
    }
    
    function togglePin(cardId) {
      const card = cards.find(c => c.id === cardId);
      if(!card) return;
      card.pinned = !card.pinned;
      card.pinnedTime = card.pinned ? Date.now() : 0;
      saveToStorage();
      renderHomeCards();  // 重新排序并刷新UI
    }
    
    async function handleCardBgUpload(cardId, file) {
      const card = cards.find(c => c.id === cardId);
      if(!card) return;
      try {
        const compressed = await compressImage(file, 800, 0.7);
        card.bgImage = compressed;
        saveToStorage();
        const cardEl = document.querySelector(`.card[data-id="${cardId}"]`);
        if(cardEl) applyCardBgStyle(cardEl, card);
      } catch(err) { console.warn(err); alert('图片处理失败'); }
    }
    
    function toggleHomeEdit() {
      if(!homeEditMode) {
        const pwd = prompt('请输入编辑密码：');
        if(pwd !== PASSWORD) { alert('密码错误'); return; }
        homeEditMode = true;
      } else {
        homeEditMode = false;
      }
      renderHomeCards();
    }
    
    function openDetail(cardId) {
      if(detailEditMode && currentDetailId) saveCurrentDetailContent();
      const card = cards.find(c=>c.id===cardId); 
      if(!card) return;
      currentDetailId = cardId;
      lyricContent.innerHTML = card.contentHtml;
      lyricContent.contentEditable = false;
      applyDetailStyle(card);
      homeView.style.display='none'; 
      detailView.style.display='block';
      detailEditMode = false;
      detailBgPanel.style.display='none';
      detailEditBtn.classList.remove('active-edit');
    }
    
    function backToHome() {
      if(detailEditMode) {
        saveCurrentDetailContent();
        detailEditMode = false;
        lyricContent.contentEditable = false;
        detailBgPanel.style.display='none';
        detailEditBtn.classList.remove('active-edit');
      }
      detailView.style.display='none'; 
      homeView.style.display='block';
      currentDetailId=null;
      renderHomeCards();
    }
    
    async function toggleDetailEdit() {
      const card = cards.find(c=>c.id===currentDetailId); 
      if(!card) return;
      if(!detailEditMode) {
        const pwd = prompt('请输入编辑密码：');
        if(pwd !== PASSWORD) { alert('密码错误'); return; }
        detailEditMode = true;
        lyricContent.contentEditable = true;
        detailBgPanel.style.display = 'flex';
        currentDetailScale = card.detailBgScale || 1.2;
        detailBgPosX.value = card.detailBgPosX ?? 50;
        detailBgPosY.value = card.detailBgPosY ?? 50;
        scaleValueDisplay.textContent = currentDetailScale.toFixed(1)+'x';
        updateAlignActive(card.textAlign || 'left');
        detailEditBtn.classList.add('active-edit');
      } else {
        saveCurrentDetailContent();
        detailEditMode = false;
        lyricContent.contentEditable = false;
        detailBgPanel.style.display = 'none';
        detailEditBtn.classList.remove('active-edit');
        showAutoSaveHint();
      }
    }
    
    function updateDetailBg() {
      if(!detailEditMode) return;
      const card = cards.find(c=>c.id===currentDetailId); 
      if(!card) return;
      card.detailBgScale = currentDetailScale;
      card.detailBgPosX = parseFloat(detailBgPosX.value);
      card.detailBgPosY = parseFloat(detailBgPosY.value);
      applyDetailStyle(card); 
      saveToStorage();
    }
    
    function changeScale(delta) {
      if(!detailEditMode) return;
      let ns = Math.min(10, Math.max(0.1, currentDetailScale + delta));
      currentDetailScale = Math.round(ns*10)/10;
      scaleValueDisplay.textContent = currentDetailScale.toFixed(1)+'x';
      const card = cards.find(c=>c.id===currentDetailId);
      if(card) { card.detailBgScale = currentDetailScale; applyDetailStyle(card); saveToStorage(); }
    }
    
    async function handleDetailBgUpload(file) {
      const card = cards.find(c=>c.id===currentDetailId);
      if(!card) return;
      try {
        const compressed = await compressImage(file, 1000, 0.7);
        card.detailBg = compressed;
        saveToStorage();
        applyDetailStyle(card);
      } catch(err) { console.warn(err); alert('背景处理失败'); }
    }
    
    // 事件绑定
    splash.addEventListener('click', ()=>{
      splash.style.opacity='0';
      setTimeout(()=>{ splash.style.display='none'; app.style.display='block'; homeView.style.display='block'; }, 400);
    });
    homeEditBtn.addEventListener('click', toggleHomeEdit);
    backBtn.addEventListener('click', backToHome);
    detailEditBtn.addEventListener('click', toggleDetailEdit);
    lyricContent.addEventListener('input', () => { if(detailEditMode) saveCurrentDetailContent(); });
    lyricContent.addEventListener('blur', () => { if(detailEditMode) saveCurrentDetailContent(); });
    detailBgUploadInput.addEventListener('change', async e => {
      const f = e.target.files[0]; if(!f) return;
      await handleDetailBgUpload(f);
      detailBgUploadInput.value = '';
    });
    detailBgPosX.addEventListener('input', updateDetailBg);
    detailBgPosY.addEventListener('input', updateDetailBg);
    scaleDownBtn.addEventListener('click', ()=>changeScale(-0.1));
    scaleUpBtn.addEventListener('click', ()=>changeScale(0.1));
    alignBtns.forEach(b=>b.addEventListener('click', ()=>{
      if(!detailEditMode) return;
      const card = cards.find(c=>c.id===currentDetailId); if(!card) return;
      card.textAlign = b.dataset.align;
      lyricContent.style.textAlign = b.dataset.align;
      updateAlignActive(b.dataset.align);
      saveToStorage();
    }));
    
    let dragging=false;
    detailView.addEventListener('mousedown', e=>{
      if(!detailEditMode) return;
      if(e.target.closest('.lyric-editor, button, .detail-bg-panel')) return;
      const card = cards.find(c=>c.id===currentDetailId); if(!card) return;
      dragging=true; const sx=e.clientX, sy=e.clientY, px=card.detailBgPosX, py=card.detailBgPosY;
      const move = me=>{
        if(!dragging) return;
        const nx = Math.min(100, Math.max(0, px + (me.clientX-sx)/3));
        const ny = Math.min(100, Math.max(0, py + (me.clientY-sy)/3));
        card.detailBgPosX = nx; card.detailBgPosY = ny;
        detailBgPosX.value = nx; detailBgPosY.value = ny;
        applyDetailStyle(card); saveToStorage();
      };
      const up = ()=>{ dragging=false; saveToStorage(); window.removeEventListener('mousemove',move); window.removeEventListener('mouseup',up); };
      window.addEventListener('mousemove', move); window.addEventListener('mouseup', up);
    });
    
    loadFromStorage();
    saveToStorage();
    renderHomeCards();
  })();
</script>
</body>
</html>
