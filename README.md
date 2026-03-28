# Edscript-2026
Desafio do Edscript 2026
(function() {
    const DOIS_HORAS_EM_MS = 2 * 60 * 60 * 1000;
    if (!localStorage.getItem('session_start')) localStorage.setItem('session_start', Date.now());

    // 1. ESTILOS (Design Fiel às Imagens)
    const style = document.createElement('style');
    style.innerHTML = `
        /* Botões Flutuantes Inferior Direito */
        #fix-acc-container { position: fixed; bottom: 20px; right: 20px; z-index: 2147483647; display: flex; flex-direction: column; gap: 10px; align-items: flex-end; }
        .float-btn { width: 55px; height: 55px; border-radius: 50%; cursor: pointer; display: flex; align-items: center; justify-content: center; box-shadow: 0 4px 15px rgba(0,0,0,0.6); position: relative; }
        .btn-vl { background: #0046ff; border: 2px solid #00f2ff; }
        .btn-acc { background: #0046ff; border: 3px solid #fffb00; }
        .badge { position: absolute; top: -2px; right: -2px; background: #ff007a; color: white; border-radius: 50%; width: 20px; height: 20px; font-size: 11px; font-weight: bold; display: flex; align-items: center; justify-content: center; }
        
        /* Menu de Acessibilidade Neon */
        #menu-acc { position: absolute; bottom: 140px; right: 0; width: 300px; background: rgba(13, 18, 41, 0.95); border: 2px solid #00f2ff; border-radius: 20px; padding: 20px; display: none; color: white; box-shadow: 0 0 20px #00f2ff55; }
        #menu-acc.open { display: block; }
        .acc-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; background: rgba(255,255,255,0.05); padding: 10px; border-radius: 12px; }
        .acc-row label { font-size: 14px; font-weight: bold; }
        
        /* Overlay Espacial (Jogo Responsável) */
        #rg-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 2147483647; background: rgba(5,10,30,0.98); display: none; align-items: center; justify-content: center; text-align: center; font-family: sans-serif; }
        .rg-card { width: 90%; max-width: 400px; border: 2px solid #00d9ff; border-radius: 30px; padding: 30px; background: #050a1d; color: white; }
        .btn-rg { width: 100%; padding: 16px; border-radius: 50px; border: none; font-weight: bold; font-size: 16px; cursor: pointer; margin-top: 15px; }
        .btn-main { background: linear-gradient(90deg, #00ff88, #0066ff); color: white; }
        .btn-sec { background: transparent; border: 2px solid #fffb00; color: #fffb00; }
        
        /* Classes de Funcionalidade */
        body.hc-mode { filter: contrast(140%) brightness(110%) !important; }
        body.font-mode { font-size: 125% !important; }
        body.calmo-mode * { animation: none !important; transition: none !important; }
    `;
    document.head.appendChild(style);

    // 2. HTML (Ícones e Estrutura)
    const container = document.createElement('div');
    container.innerHTML = `
        <div id="fix-acc-container">
            <div id="menu-acc">
                <h3 style="color:#fffb00; margin-top:0">ACESSIBILIDADE</h3>
                ${createRow('🧩 Modo Calmo', 'calmo-mode')}
                ${createRow('🔳 Alto Contraste', 'hc-mode')}
                ${createRow('abc Fonte Grande', 'font-mode')}
                ${createRow('💬 Ling. Simples', 'simple-mode')}
            </div>
            <div class="float-btn btn-vl" onclick="toggleVlibras()"><svg viewBox="0 0 24 24" width="30" fill="white"><path d="M21,12.22C21,16.73 17.19,20.5 12.5,20.5C7.81,20.5 4,16.73 4,12.22C4,9.45 5.25,6.97 7.21,5.27L8.62,6.68C7.03,7.95 6,9.97 6,12.22C6,15.71 8.91,18.5 12.5,18.5C16.09,18.5 19,15.71 19,12.22C19,10.27 18.15,8.54 16.8,7.35L18.21,5.94C19.95,7.46 21,9.72 21,12.22Z"/></svg></div>
            <div class="float-btn btn-acc" onclick="document.getElementById('menu-acc').classList.toggle('open')">
                <div class="badge">4</div>
                <svg viewBox="0 0 24 24" width="32" fill="white"><path d="M12,2c1.1,0,2,0.9,2,2s-0.9,2-2,2s-2-0.9-2-2S10.9,2,12,2z M21,9h-6v13h-2v-6h-2v6H9V9H3V7h18V9z"/></svg>
            </div>
        </div>
        <div id="rg-overlay"><div class="rg-card" id="rg-inject"></div></div>
        <div vw class="enabled"><div vw-access-button class="active" style="display:none;"></div><div vw-plugin-wrapper></div></div>
    `;
    document.body.appendChild(container);

    function createRow(label, cls) {
        return `<div class="acc-row"><label>${label}</label><input type="checkbox" onchange="document.body.classList.toggle('${cls}')"></div>`;
    }

    // 3. LÓGICA DE ALERTA DINÂMICO
    window.checkRG = function() {
        const elapsed = Date.now() - parseInt(localStorage.getItem('session_start'));
        if (elapsed >= DOIS_HORAS_EM_MS) {
            const isEsportes = window.location.href.includes('esportes');
            const target = document.getElementById('rg-inject');
            
            if (isEsportes) {
                target.innerHTML = `
                    <h2>Fala, craque! ⚽</h2>
                    <p>Que tal variar o ritmo vendo outras opções ou uma pausa para recarregar as energias? 🥤</p>
                    <button class="btn-rg btn-main" onclick="location.href='/ptb/esportes'">Outras Opções 🏐</button>
                    <button class="btn-rg btn-sec" onclick="location.reload()">Pausar agora 🥤</button>
                    <p onclick="closeRG()" style="cursor:pointer; margin-top:15px; text-decoration:underline; font-size:12px">Continuar jogando →</p>
                `;
            } else {
                target.innerHTML = `
                    <h2>Mandou bem! 🚀</h2>
                    <p>Que tal variar o ritmo com o Mines 💣 ou uma pausa para recarregar as energias? 🥤</p>
                    <button class="btn-rg btn-main" onclick="location.href='/ptb/cassino/mines'">Jogar Mines 💣</button>
                    <button class="btn-rg btn-sec" onclick="location.reload()">Pausar agora 🥤</button>
                    <p onclick="closeRG()" style="cursor:pointer; margin-top:15px; text-decoration:underline; font-size:12px">Continuar jogando →</p>
                `;
            }
            document.getElementById('rg-overlay').style.display = 'flex';
        }
    };

    window.closeRG = () => { localStorage.setItem('session_start', Date.now()); document.getElementById('rg-overlay').style.display = 'none'; };
    
    window.toggleVlibras = () => {
        if (!window.VLibras) {
            const s = document.createElement('script'); s.src = 'https://vlibras.gov.br/app/vlibras-plugin.js';
            s.onload = () => { new window.VLibras.Widget('https://vlibras.gov.br/app'); };
            document.head.appendChild(s);
        }
    };

    setInterval(checkRG, 10000);
})();

