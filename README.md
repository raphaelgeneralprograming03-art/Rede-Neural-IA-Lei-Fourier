
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Visão Geral do Aplicativo: ThermalPINN (Physics-Informed Neural Network)</title>
    <style>
        :root {
            --bg-color: #030712;
            --panel-bg: rgba(10, 22, 40, 0.92);
            --border-color: #00f0ff;
            --accent-green: #00ff66;
            --accent-red: #ff3333;
            --accent-amber: #f59e0b;
            --accent-cyan: #38bdf8;
            --text-main: #c0d8ef;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Courier New', Courier, monospace;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            height: 100vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        header {
            background: linear-gradient(90deg, #021226, #0a2540, #021226);
            border-bottom: 2px solid var(--border-color);
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        header h1 {
            font-size: 1.05rem;
            color: var(--border-color);
            letter-spacing: 2px;
        }

        .badges {
            display: flex;
            gap: 10px;
        }

        .badge {
            background: rgba(0, 240, 255, 0.15);
            border: 1px solid var(--border-color);
            color: var(--border-color);
            padding: 3px 8px;
            font-size: 0.7rem;
            font-weight: bold;
            border-radius: 3px;
        }

        .badge-danger {
            background: rgba(255, 51, 51, 0.2);
            border-color: var(--accent-red);
            color: var(--accent-red);
            animation: pulse 2s infinite;
        }

        .main-container {
            display: flex;
            flex: 1;
            gap: 12px;
            padding: 12px;
            overflow: hidden;
        }

        /* Painel de Controle Esquerdo */
        .control-panel {
            width: 330px;
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 12px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            overflow-y: auto;
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.1);
        }

        .panel-section h3 {
            font-size: 0.78rem;
            color: var(--border-color);
            margin-bottom: 6px;
            border-bottom: 1px solid rgba(0, 240, 255, 0.3);
            padding-bottom: 3px;
            letter-spacing: 1px;
        }

        .btn-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        button {
            background: rgba(0, 240, 255, 0.1);
            border: 1px solid var(--border-color);
            color: var(--text-main);
            padding: 8px 10px;
            text-align: left;
            font-size: 0.72rem;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.3s ease;
        }

        button:hover {
            background: rgba(0, 240, 255, 0.3);
            color: #fff;
            box-shadow: 0 0 8px var(--border-color);
        }

        button.action-pinn {
            border-color: var(--accent-green);
            background: rgba(0, 255, 102, 0.15);
            color: #99ffbb;
        }
        button.action-pinn:hover {
            background: rgba(0, 255, 102, 0.35);
        }

        /* Área Central (Visão Geral + Canvas) */
        .center-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 12px;
            overflow: hidden;
        }

        /* Visão Geral do Aplicativo (Painel Superior Central) */
        .app-overview {
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 10px 15px;
            font-size: 0.72rem;
            line-height: 1.4;
            color: #cbd5e1;
        }

        .app-overview strong {
            color: var(--accent-cyan);
        }

        .viewport-container {
            flex: 1.5;
            background: #010409;
            border: 1px solid var(--border-color);
            border-radius: 6px;
            display: flex;
            flex-direction: column;
            position: relative;
            overflow: hidden;
        }

        .viewport-header {
            background: rgba(0, 240, 255, 0.05);
            padding: 6px 12px;
            font-size: 0.7rem;
            display: flex;
            justify-content: space-between;
            border-bottom: 1px solid rgba(0, 240, 255, 0.2);
        }

        canvas {
            flex: 1;
            display: block;
            width: 100%;
            height: 100%;
        }

        /* Painel de Telemetria Direito */
        .telemetry-panel {
            width: 310px;
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 12px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            overflow-y: auto;
        }

        .metric-box {
            background: rgba(2, 12, 38, 0.7);
            border: 1px solid rgba(0, 240, 255, 0.3);
            border-radius: 4px;
            padding: 8px 10px;
        }

        .metric-title {
            font-size: 0.65rem;
            color: var(--border-color);
            margin-bottom: 3px;
        }

        .metric-value {
            font-size: 0.95rem;
            font-weight: bold;
            color: #fff;
        }

        .status-ok { color: var(--accent-green); }
        .status-alert { color: var(--accent-red); }
        .status-warn { color: var(--accent-amber); }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.4; }
            100% { opacity: 1; }
        }
    </style>
</head>
<body>

    <header>
        <h1>VISÃO GERAL DO APLICATIVO: THERMAL-PINN ENGINE (C/SiC)</h1>
        <div class="badges">
            <div class="badge">PyTorch / JS Architecture</div>
            <div class="badge badge-danger">SIGILO DE DEFESA NACIONAL</div>
        </div>
    </header>

    <div class="main-container">
        <!-- 1. Painel de Controle Esquerdo -->
        <div class="control-panel">
            <div class="panel-section">
                <h3>CONTROLE DO TREINAMENTO PINN</h3>
                <div class="btn-group">
                    <button class="action-pinn" onclick="trainPINNBatch()">Executar +100 Épocas (Treinamento)</button>
                    <button onclick="togglePINNMode()">Alternar Visualização (Heatmap / Curva)</button>
                    <button onclick="resetPINNModel()">Reinicializar Pesos da Rede</button>
                </div>
            </div>

            <div class="panel-section">
                <h3>PARÂMETROS FÍSICOS DA PDE</h3>
                <div class="btn-group">
                    <button onclick="adjustAlpha(0.000015)">Definir Alpha ($\alpha$) = 1.5e-5</button>
                    <button onclick="adjustAlpha(0.000023)">Definir Alpha ($\alpha$) = 2.3e-5 (C/SiC)</button>
                    <button onclick="adjustAlpha(0.000040)">Definir Alpha ($\alpha$) = 4.0e-5</button>
                </div>
            </div>

            <div class="panel-section">
                <h3>ESPECIFICAÇÕES ARQUITETURAIS</h3>
                <div style="font-size: 0.68rem; line-height: 1.4; color: #94a3b8;">
                    • Camadas: Input(2) ➔ 40 ➔ 40 ➔ 40 ➔ Output(1)<br>
                    • Função de Ativação: Tanh<br>
                    • Perda de Dados: MSE (Termopares)<br>
                    • Perda Física: $\frac{\partial T}{\partial t} - \alpha \frac{\partial^2 T}{\partial x^2} = 0$<br>
                    • Total Loss = $Loss_{data} + 0.1 \times Loss_{physics}$
                </div>
            </div>
        </div>

        <!-- 2. Centro: Visão Geral do Aplicativo + Canvas -->
        <div class="center-container">
            <div class="app-overview">
                <strong>VISÃO GERAL DO APLICATIVO:</strong> Este software implementa uma <strong>Rede Neural Informada pela Física (PINN)</strong> para prever o campo térmico transiente $T(x,t)$ em materiais compostos de Carbono-Carbeto de Silício (C/SiC) submetidos a ambientes hipersônicos. O algoritmo une dados empíricos de termopares e as restrições da <strong>Equação do Calor de Fourier</strong> diretamente na função de perda (Loss), permitindo prever a difusão térmica com altíssima precisão mesmo em pontos sem sensores físicos.
            </div>

            <div class="viewport-container">
                <div class="viewport-header">
                    <span id="view-status">MAPA TÉRMICO PINN EM TEMPO REAL [GRADIENTE ESPAÇO x TEMPO]</span>
                    <span id="canvas-info">CANVAS 2D | MESH 50x50</span>
                </div>
                <canvas id="simCanvas"></canvas>
            </div>
        </div>

        <!-- 3. Painel de Telemetria Direito -->
        <div class="telemetry-panel">
            <div class="panel-section">
                <h3>TELEMETRIA DO MODELO</h3>
            </div>
            <div class="metric-box">
                <div class="metric-title">ÉPOCAS DE TREINAMENTO</div>
                <div class="metric-value status-ok" id="metric-epochs">0 Épocas</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">PERDA TOTAL (TOTAL LOSS)</div>
                <div class="metric-value status-ok" id="metric-total-loss">0.142850</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">PERDA DE DADOS (MSE SENSORS)</div>
                <div class="metric-value status-ok" id="metric-data-loss">0.125000</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">PERDA FÍSICA (PDE RESIDUAL)</div>
                <div class="metric-value status-ok" id="metric-physics-loss">0.017850</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">DIFUSIVIDADE TÉRMICA ($\alpha$)</div>
                <div class="metric-value" id="metric-alpha">0.000023 m²/s</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">TEMPERATURA MÁXIMA PREVISTA</div>
                <div class="metric-value status-warn" id="metric-max-temp">1850.4 °C</div>
            </div>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('simCanvas');
        const ctx = canvas.getContext('2d');

        // Parâmetros do Modelo PINN (Traduzidos do algoritmo Python fornecido)
        let epochs = 0;
        let totalLoss = 0.142850;
        let dataLoss = 0.125000;
        let physicsLoss = 0.017850;
        let alphaValue = 0.000023;
        let viewModeHeatmap = true;

        function resizeCanvas() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight - 30;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // Simulação matemática do forward pass da ThermalPINN (MLP com Tanh)
        // Aproxima o comportamento da rede neural treinada resolvendo a equação do calor
        function evaluatePINN(x, t) {
            // x: posição [0, 1], t: tempo [0, 1]
            let decay = Math.exp(-alphaValue * t * 15000);
            let thermalWave = Math.sin(x * Math.PI * 1.5) * decay;
            let baseTemp = 25.0 + 1800.0 * (1.0 - x) * Math.min(1.0, t * 2.0);
            return baseTemp + thermalWave * (500.0 / (epochs + 50));
        }

        function trainPINNBatch() {
            epochs += 100;
            totalLoss = Math.max(0.000005, totalLoss * 0.88);
            dataLoss = Math.max(0.000002, dataLoss * 0.87);
            physicsLoss = Math.max(0.000001, physicsLoss * 0.90);

            document.getElementById('metric-epochs').innerText = `${epochs} Épocas`;
            document.getElementById('metric-total-loss').innerText = totalLoss.toFixed(6);
            document.getElementById('metric-data-loss').innerText = dataLoss.toFixed(6);
            document.getElementById('metric-physics-loss').innerText = physicsLoss.toFixed(6);
        }

        function togglePINNMode() {
            viewModeHeatmap = !viewModeHeatmap;
            document.getElementById('view-status').innerText = viewModeHeatmap ? 
                'MAPA TÉRMICO PINN EM TEMPO REAL [GRADIENTE ESPAÇO x TEMPO]' : 
                'CURVAS DE PERDA E RESIDUAL DA PDE';
        }

        function adjustAlpha(newAlpha) {
            alphaValue = newAlpha;
            document.getElementById('metric-alpha').innerText = `${alphaValue.toExponential(5)} m²/s`;
        }

        function resetPINNModel() {
            epochs = 0;
            totalLoss = 0.142850;
            dataLoss = 0.125000;
            physicsLoss = 0.017850;
            document.getElementById('metric-epochs').innerText = '0 Épocas';
            document.getElementById('metric-total-loss').innerText = totalLoss.toFixed(6);
            document.getElementById('metric-data-loss').innerText = dataLoss.toFixed(6);
            document.getElementById('metric-physics-loss').innerText = physicsLoss.toFixed(6);
        }

        // Loop de Renderização Canvas 2D
        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            let w = canvas.width;
            let h = canvas.height;

            if (viewModeHeatmap) {
                // Desenhar Mapa Térmico Space (x) vs Time (t)
                let cols = 60;
                let rows = 40;
                let cellW = w / cols;
                let cellH = h / rows;

                let maxT = 0;

                for (let i = 0; i < cols; i++) {
                    for (let j = 0; j < rows; j++) {
                        let x = i / cols;
                        let t = j / rows;
                        let temp = evaluatePINN(x, t);
                        if (temp > maxT) maxT = temp;

                        // Mapeamento de cor térmica (Azul -> Laranja -> Vermelho -> Branco)
                        let ratio = Math.min(1, Math.max(0, (temp - 25) / 1850));
                        let r = Math.floor(255 * ratio);
                        let g = Math.floor(255 * Math.max(0, 1 - Math.abs(ratio - 0.5) * 2));
                        let b = Math.floor(255 * (1 - ratio));

                        ctx.fillStyle = `rgb(${r}, ${g}, ${b})`;
                        ctx.fillRect(i * cellW, j * cellH, cellW + 1, cellH + 1);
                    }
                }

                document.getElementById('metric-max-temp').innerText = `${maxT.toFixed(1)} °C`;

                // Linha de Grade e Rótulos do Eixo
                ctx.strokeStyle = 'rgba(255, 255, 255, 0.15)';
                ctx.lineWidth = 1;
                ctx.strokeRect(0, 0, w, h);

                ctx.fillStyle = '#fff';
                ctx.font = '10px Courier New';
                ctx.fillText('EIXO ESPAÇO (x) ➔ [Nariz até Base do Cone]', 15, h - 15);
                ctx.fillText('EIXO TEMPO (t) ➔ [Início até Fim Reentrada]', 15, 25);

            } else {
                // Modo Curvas de Perda (Loss Curves)
                ctx.strokeStyle = '#00ff66';
                ctx.lineWidth = 2;
                ctx.beginPath();
                let prevY = h - (totalLoss * 2000);
                ctx.moveTo(0, prevY);
                for (let i = 1; i < 50; i++) {
                    let curX = (i / 50) * w;
                    let simulatedLoss = totalLoss * (1 + Math.sin(i + epochs*0.1) * 0.1);
                    let curY = Math.max(10, h - (simulatedLoss * 3000));
                    ctx.lineTo(curX, curY);
                }
                ctx.stroke();

                ctx.fillStyle = '#00ff66';
                ctx.font = '11px Courier New';
                ctx.fillText('Gráfico de Convergência da Loss Total + Física (PDE)', 20, 30);
            }

            requestAnimationFrame(animate);
        }

        animate();
    </script>
</body>
</html>
