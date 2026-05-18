<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Ley de Signos en Acción - Juego Matemático</title>
    <style>
        * {
            box-sizing: border-box;
            user-select: none;
        }

        body {
            margin: 0;
            min-height: 100vh;
            background: linear-gradient(135deg, #1e2a3a, #0f1724);
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', 'Poppins', 'Helvetica Neue', sans-serif;
            padding: 1rem;
        }

        /* Contenedor principal - efecto glassmorfismo */
        .game-container {
            background: rgba(44, 62, 80, 0.65);
            backdrop-filter: blur(12px);
            border-radius: 3rem;
            padding: 1.8rem 2rem 2.5rem;
            box-shadow: 0 25px 45px rgba(0,0,0,0.3), inset 0 1px 1px rgba(255,255,255,0.1);
            width: 100%;
            max-width: 680px;
            transition: all 0.2s ease;
            border: 1px solid rgba(255,255,255,0.2);
        }

        /* Títulos */
        .game-title {
            text-align: center;
            font-size: 2rem;
            font-weight: 800;
            letter-spacing: 1px;
            background: linear-gradient(135deg, #FDE68A, #FCD34D, #FBBF24);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 2px 5px rgba(0,0,0,0.2);
            margin-top: 0;
            margin-bottom: 1rem;
        }

        /* Panel nivel (inicio) */
        .level-panel, .game-panel {
            transition: opacity 0.2s ease, transform 0.2s ease;
        }

        .level-buttons {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 0.8rem;
            margin: 1.8rem 0;
        }

        /* Botones globales */
        .btn {
            background: #1ABC9C;
            border: none;
            padding: 0.7rem 1.4rem;
            font-weight: bold;
            border-radius: 50px;
            font-size: 1rem;
            cursor: pointer;
            transition: transform 0.1s, background 0.2s;
            color: white;
            font-family: inherit;
            box-shadow: 0 5px 0 #0e6b58;
        }

        .btn:active {
            transform: translateY(2px);
            box-shadow: 0 2px 0 #0e6b58;
        }

        .btn-level {
            background: #3B82F6;
            box-shadow: 0 4px 0 #1e40af;
            min-width: 110px;
        }

        .btn-level:active {
            transform: translateY(2px);
        }

        .btn-answer {
            background: #F59E0B;
            box-shadow: 0 4px 0 #B45309;
        }

        .btn-new {
            background: #EF4444;
            box-shadow: 0 4px 0 #991B1B;
        }

        /* info (puntaje/vidas) */
        .info-bar {
            display: flex;
            justify-content: space-between;
            background: #1E293B;
            padding: 0.7rem 1.2rem;
            border-radius: 80px;
            margin-bottom: 2rem;
            font-weight: bold;
            font-size: 1.3rem;
            backdrop-filter: blur(4px);
        }

        .puntaje {
            background: #FBBF24;
            padding: 0.2rem 1rem;
            border-radius: 40px;
            color: #1E293B;
            display: inline-flex;
            align-items: center;
            gap: 6px;
        }

        .vidas {
            color: #FCA5A5;
            font-size: 1.7rem;
            line-height: 1;
            letter-spacing: 4px;
        }

        /* pregunta */
        .question-box {
            background: #0F172A;
            padding: 1.8rem;
            border-radius: 2rem;
            text-align: center;
            margin: 1rem 0;
            box-shadow: inset 0 0 5px #334155, 0 10px 20px rgba(0,0,0,0.2);
        }

        .question-text {
            font-size: 2.7rem;
            font-weight: 800;
            font-family: 'Courier New', monospace;
            word-break: break-word;
            color: #F8FAFC;
        }

        /* input + botón responder */
        .answer-area {
            display: flex;
            justify-content: center;
            gap: 0.8rem;
            margin: 1rem 0 1rem;
        }

        .answer-input {
            font-size: 1.5rem;
            width: 130px;
            text-align: center;
            border-radius: 60px;
            border: none;
            padding: 0.5rem;
            font-weight: bold;
            background: #F1F5F9;
            font-family: monospace;
        }

        .mensaje-feedback {
            background: #111827AA;
            border-radius: 2rem;
            padding: 0.7rem;
            text-align: center;
            font-size: 1rem;
            font-weight: 500;
            margin-top: 1rem;
            margin-bottom: 0.5rem;
        }

        .footer-note {
            text-align: center;
            font-size: 0.8rem;
            color: #9CA3AF;
            margin-top: 1.8rem;
        }

        .hidden {
            display: none;
        }

        button:disabled {
            opacity: 0.6;
            transform: none;
            cursor: not-allowed;
        }

        @media (max-width: 500px) {
            .game-container { padding: 1.2rem; }
            .question-text { font-size: 1.8rem; }
            .btn { padding: 0.5rem 1rem; }
            .info-bar { font-size: 1rem; }
        }
    </style>
</head>
<body>

<div class="game-container" id="app">
    <!-- pantalla de inicio / selección nivel -->
    <div id="levelScreen">
        <div class="game-title">⚡ LEY DE SIGNOS ⚡</div>
        <div style="text-align: center; color:#cbd5e6; margin-bottom: 0.5rem;">Selecciona un nivel:</div>
        <div class="level-buttons">
            <button class="btn btn-level" data-level="1">🎯 Fácil<br><span style="font-size:0.8rem;">sumas/restas</span></button>
            <button class="btn btn-level" data-level="2">🧠 Medio<br><span style="font-size:0.8rem;">× y ÷</span></button>
            <button class="btn btn-level" data-level="3">🔥 Difícil<br><span style="font-size:0.8rem;">combinadas</span></button>
        </div>
        <div class="footer-note">✨ aplica la ley de signos ✨</div>
    </div>

    <!-- pantalla de juego (inicialmente oculta) -->
    <div id="gameScreen" class="hidden">
        <div class="info-bar">
            <span class="puntaje">🏆 <span id="puntajeValor">0</span></span>
            <span class="vidas" id="vidasIconos">❤️❤️❤️</span>
        </div>

        <div class="question-box">
            <div class="question-text" id="preguntaTexto">Cargando...</div>
        </div>

        <div class="answer-area">
            <input type="number" id="respuestaInput" class="answer-input" placeholder="?" autocomplete="off">
            <button id="responderBtn" class="btn btn-answer">Responder</button>
        </div>

        <div id="mensajeFeedback" class="mensaje-feedback"></div>
        <div style="display: flex; justify-content: center; margin-top: 10px;">
            <button id="nuevoJuegoBtn" class="btn btn-new">🔄 Nuevo juego</button>
        </div>
        <div class="footer-note">🎓 ¡Aplica la ley de signos! | 3 vidas</div>
    </div>
</div>

<script>
    // ---------- LÓGICA DEL JUEGO (versión JavaScript) ----------
    // Generador de preguntas según nivel (igual que tu algoritmo original)
    function generarPregunta(nivel) {
        // Nivel 1: suma/resta entre -20 y 20
        if (nivel === 1) {
            const a = Math.floor(Math.random() * 41) - 20;
            const b = Math.floor(Math.random() * 41) - 20;
            const esSuma = Math.random() < 0.5;
            if (esSuma) {
                return { texto: `${a} + ${b}`, respuesta: a + b };
            } else {
                return { texto: `${a} - ${b}`, respuesta: a - b };
            }
        }
        // Nivel 2: multiplicación o división exacta (evitar 0)
        else if (nivel === 2) {
            const esMultiplica = Math.random() < 0.5;
            if (esMultiplica) {
                let a = Math.floor(Math.random() * 25) - 12;
                let b = Math.floor(Math.random() * 25) - 12;
                while (a === 0 || b === 0) {
                    a = Math.floor(Math.random() * 25) - 12;
                    b = Math.floor(Math.random() * 25) - 12;
                }
                return { texto: `${a} × ${b}`, respuesta: a * b };
            } else {
                let divisor = Math.floor(Math.random() * 25) - 12;
                while (divisor === 0) divisor = Math.floor(Math.random() * 25) - 12;
                let cociente = Math.floor(Math.random() * 25) - 12;
                let dividendo = divisor * cociente;
                return { texto: `${dividendo} ÷ ${divisor}`, respuesta: cociente };
            }
        }
        // Nivel 3: operaciones combinadas (difícil)
        else {
            const tipos = [
                (a,b,c) => ({ texto: `(${a} + ${b}) × ${c}`, respuesta: (a + b) * c }),
                (a,b,c) => ({ texto: `(${a} - ${b}) × ${c}`, respuesta: (a - b) * c }),
                (a,b,c) => ({ texto: `${a} × (${b} + ${c})`, respuesta: a * (b + c) }),
                (a,b,c) => ({ texto: `${a} × (${b} - ${c})`, respuesta: a * (b - c) }),
                (a,b,c) => ({ texto: `${a} + ${b} × ${c}`, respuesta: a + b * c }),
                (a,b,c) => ({ texto: `${a} - ${b} × ${c}`, respuesta: a - b * c }),
                (a,b,c) => {
                    let divisor = c;
                    while (divisor === 0) divisor = Math.floor(Math.random() * 21) - 10;
                    let numerador = (a + b);
                    let resp = Math.floor(numerador / divisor);
                    if (numerador % divisor !== 0) resp = Math.floor(numerador / divisor);
                    return { texto: `(${a} + ${b}) ÷ ${divisor}`, respuesta: resp };
                },
                (a,b,c) => {
                    let divisor = c;
                    while (divisor === 0) divisor = Math.floor(Math.random() * 21) - 10;
                    let numerador = (a - b);
                    let resp = Math.floor(numerador / divisor);
                    if (numerador % divisor !== 0) resp = Math.floor(numerador / divisor);
                    return { texto: `(${a} - ${b}) ÷ ${divisor}`, respuesta: resp };
                }
            ];
            let a = Math.floor(Math.random() * 21) - 10;
            let b = Math.floor(Math.random() * 21) - 10;
            let c = Math.floor(Math.random() * 21) - 10;
            while (c === 0) c = Math.floor(Math.random() * 21) - 10;
            const funcion = tipos[Math.floor(Math.random() * tipos.length)];
            return funcion(a, b, c);
        }
    }

    // ---------- CONTROL DE LA UI ----------
    let nivelSeleccionado = null;
    let vidas = 3;
    let puntaje = 0;
    let rondas = 0;
    let preguntaActual = null;
    let esperandoRespuesta = false;  // para evitar doble respuesta mientras se procesa

    // Elementos DOM
    const levelScreen = document.getElementById('levelScreen');
    const gameScreen = document.getElementById('gameScreen');
    const puntajeSpan = document.getElementById('puntajeValor');
    const vidasSpan = document.getElementById('vidasIconos');
    const preguntaTextoDiv = document.getElementById('preguntaTexto');
    const respuestaInput = document.getElementById('respuestaInput');
    const responderBtn = document.getElementById('responderBtn');
    const nuevoJuegoBtn = document.getElementById('nuevoJuegoBtn');
    const mensajeFeedbackDiv = document.getElementById('mensajeFeedback');

    function actualizarUI() {
        puntajeSpan.innerText = puntaje;
        let corazones = '';
        for (let i = 0; i < vidas; i++) corazones += '❤️';
        if (vidas === 0) corazones = '💀';
        vidasSpan.innerText = corazones;
    }

    function mostrarMensaje(texto, esError = false) {
        mensajeFeedbackDiv.innerText = texto;
        mensajeFeedbackDiv.style.color = esError ? '#FCA5A5' : '#6EE7B7';
        setTimeout(() => {
            if (mensajeFeedbackDiv.innerText === texto) {
                mensajeFeedbackDiv.innerText = '';
            }
        }, 1800);
    }

    function cargarNuevaPregunta() {
        if (vidas <= 0) return;
        preguntaActual = generarPregunta(nivelSeleccionado);
        preguntaTextoDiv.innerText = `${preguntaActual.texto} = ?`;
        respuestaInput.value = '';
        respuestaInput.disabled = false;
        responderBtn.disabled = false;
        esperandoRespuesta = false;
        respuestaInput.focus();
    }

    function verificarRespuesta() {
        if (esperandoRespuesta || vidas <= 0) return;
        const valorIngresado = parseInt(respuestaInput.value.trim());
        if (isNaN(valorIngresado)) {
            mostrarMensaje('⚠️ Ingresa un número entero', true);
            return;
        }

        esperandoRespuesta = true;
        respuestaInput.disabled = true;
        responderBtn.disabled = true;

        if (valorIngresado === preguntaActual.respuesta) {
            // Correcto
            puntaje += 10;
            actualizarUI();
            mostrarMensaje('✅ ¡Correcto! +10 puntos', false);
            rondas++;
            setTimeout(() => {
                if (vidas > 0) cargarNuevaPregunta();
                else finalizarJuego();
            }, 800);
        } else {
            // Incorrecto
            vidas--;
            actualizarUI();
            mostrarMensaje(`❌ Incorrecto. Respuesta: ${preguntaActual.respuesta}. Pierdes una vida.`, true);
            if (vidas === 0) {
                setTimeout(() => finalizarJuego(), 1300);
            } else {
                setTimeout(() => {
                    if (vidas > 0) cargarNuevaPregunta();
                    else finalizarJuego();
                }, 1300);
            }
        }
    }

    function finalizarJuego() {
        preguntaTextoDiv.innerText = 'JUEGO TERMINADO';
        respuestaInput.disabled = true;
        responderBtn.disabled = true;
        let mensajeFinal = `🏆 Puntaje final: ${puntaje} puntos\nRondas: ${rondas}`;
        if (puntaje >= 50) mensajeFinal += '\n🌟 ¡Excelente! Dominas la ley de signos.';
        else if (puntaje >= 20) mensajeFinal += '\n👍 Buen trabajo, sigue practicando.';
        else mensajeFinal += '\n📚 Estudia la ley de signos y vuelve a intentarlo.';
        mostrarMensaje(mensajeFinal, false);
    }

    function reiniciarCompleto() {
        // Volver a pantalla de niveles
        nivelSeleccionado = null;
        vidas = 3;
        puntaje = 0;
        rondas = 0;
        esperandoRespuesta = false;
        preguntaActual = null;
        actualizarUI();
        levelScreen.classList.remove('hidden');
        gameScreen.classList.add('hidden');
        respuestaInput.disabled = false;
        responderBtn.disabled = false;
        mensajeFeedbackDiv.innerText = '';
    }

    function iniciarJuego(nivel) {
        nivelSeleccionado = nivel;
        vidas = 3;
        puntaje = 0;
        rondas = 0;
        actualizarUI();
        esperandoRespuesta = false;
        levelScreen.classList.add('hidden');
        gameScreen.classList.remove('hidden');
        cargarNuevaPregunta();
    }

    // --- Eventos ---
    document.querySelectorAll('[data-level]').forEach(btn => {
        btn.addEventListener('click', (e) => {
            const nivel = parseInt(btn.getAttribute('data-level'));
            iniciarJuego(nivel);
        });
    });

    responderBtn.addEventListener('click', verificarRespuesta);
    respuestaInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') verificarRespuesta();
    });
    nuevoJuegoBtn.addEventListener('click', reiniciarCompleto);
</script>
</body>
</html>
