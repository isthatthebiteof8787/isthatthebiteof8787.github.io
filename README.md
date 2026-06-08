<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Бесплатный Claude AI — через Puter.js</title>

    <!-- Puter.js CDN -->
    <script src="https://js.puter.com/v2/"></script>

    <style>
        :root {
            --bg: #0d0f13;
            --surface: #16181d;
            --surface2: #1e2028;
            --border: #2a2d35;
            --text: #e1e3e8;
            --text2: #9a9da8;
            --accent: #d4a574;
            --accent2: #b8884e;
            --claude: #d97757;
            --green: #4ade80;
            --red: #f87171;
            --radius: 14px;
            --radius-sm: 8px;
            --font: 'Inter', 'Segoe UI', system-ui, -apple-system, sans-serif;
            --mono: 'JetBrains Mono', 'Fira Code', 'Cascadia Code', monospace;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: var(--bg);
            color: var(--text);
            font-family: var(--font);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 24px 16px 60px;
            line-height: 1.6;
            -webkit-font-smoothing: antialiased;
        }

        /* ── Header ────────────────────── */
        .header {
            text-align: center;
            margin-bottom: 32px;
            max-width: 720px;
            width: 100%;
        }
        .header .badge {
            display: inline-block;
            background: #1a1c22;
            border: 1px solid var(--border);
            color: var(--accent);
            font-size: 0.78rem;
            font-weight: 600;
            letter-spacing: 0.04em;
            text-transform: uppercase;
            padding: 5px 14px;
            border-radius: 99px;
            margin-bottom: 16px;
        }
        .header h1 {
            font-size: 2.1rem;
            font-weight: 700;
            letter-spacing: -0.02em;
            margin-bottom: 6px;
            color: #f0f0f4;
        }
        .header h1 span {
            color: var(--claude);
        }
        .header p {
            color: var(--text2);
            font-size: 0.95rem;
            max-width: 520px;
            margin: 0 auto;
        }

        /* ── Main card ────────────────── */
        .card {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: var(--radius);
            padding: 28px 28px 24px;
            width: 100%;
            max-width: 720px;
            display: flex;
            flex-direction: column;
            gap: 18px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.35);
        }

        /* ── Model selector ───────────── */
        .model-row {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 10px;
        }
        .model-row label {
            font-weight: 600;
            font-size: 0.88rem;
            color: var(--text2);
            letter-spacing: 0.01em;
            white-space: nowrap;
        }
        .model-select {
            flex: 1;
            min-width: 200px;
            background: var(--surface2);
            color: var(--text);
            border: 1px solid var(--border);
            border-radius: var(--radius-sm);
            padding: 10px 14px;
            font-size: 0.9rem;
            font-family: var(--font);
            cursor: pointer;
            outline: none;
            transition: border 0.2s;
        }
        .model-select:focus {
            border-color: var(--accent2);
        }
        .stream-toggle {
            display: flex;
            align-items: center;
            gap: 7px;
            font-size: 0.85rem;
            color: var(--text2);
            cursor: pointer;
            user-select: none;
            white-space: nowrap;
        }
        .stream-toggle input {
            accent-color: var(--accent2);
            width: 16px;
            height: 16px;
            cursor: pointer;
        }

        /* ── Textarea ─────────────────── */
        .prompt-area {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        .prompt-area label {
            font-weight: 600;
            font-size: 0.88rem;
            color: var(--text2);
            letter-spacing: 0.01em;
        }
        .prompt-area textarea {
            width: 100%;
            min-height: 110px;
            background: var(--surface2);
            color: var(--text);
            border: 1px solid var(--border);
            border-radius: var(--radius-sm);
            padding: 14px 16px;
            font-size: 0.92rem;
            font-family: var(--font);
            resize: vertical;
            outline: none;
            line-height: 1.55;
            transition: border 0.2s;
        }
        .prompt-area textarea:focus {
            border-color: var(--accent2);
        }
        .prompt-area textarea::placeholder {
            color: #5a5d66;
        }

        /* ── Buttons ──────────────────── */
        .btn-row {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }
        .btn {
            font-family: var(--font);
            font-weight: 600;
            font-size: 0.9rem;
            letter-spacing: 0.01em;
            padding: 11px 22px;
            border-radius: var(--radius-sm);
            border: 1px solid transparent;
            cursor: pointer;
            transition: all 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 7px;
        }
        .btn-primary {
            background: var(--accent2);
            color: #0d0f13;
            border-color: var(--accent2);
        }
        .btn-primary:hover {
            background: #c9944a;
            border-color: #c9944a;
        }
        .btn-primary:disabled {
            opacity: 0.45;
            cursor: not-allowed;
            pointer-events: none;
        }
        .btn-outline {
            background: transparent;
            color: var(--text);
            border-color: var(--border);
        }
        .btn-outline:hover {
            background: var(--surface2);
            border-color: #3a3d47;
        }
        .btn-example {
            background: transparent;
            color: var(--accent);
            border-color: transparent;
            font-weight: 500;
            font-size: 0.82rem;
            padding: 6px 12px;
        }
        .btn-example:hover {
            background: #1a1c22;
            color: #e0b87a;
        }

        /* ── Response area ────────────── */
        .response-box {
            background: var(--surface2);
            border: 1px solid var(--border);
            border-radius: var(--radius-sm);
            padding: 18px 20px;
            min-height: 140px;
            max-height: 520px;
            overflow-y: auto;
            font-size: 0.9rem;
            line-height: 1.65;
            white-space: pre-wrap;
            word-break: break-word;
            color: #d8dbe2;
            font-family: var(--font);
            transition: border 0.3s;
            position: relative;
        }
        .response-box.streaming {
            border-color: var(--accent2);
            box-shadow: 0 0 0 3px rgba(184, 136, 78, 0.08);
        }
        .response-box .placeholder {
            color: #4a4d56;
            font-style: italic;
        }
        .response-box .cursor-blink {
            display: inline-block;
            width: 2px;
            height: 1.1em;
            background: var(--accent);
            margin-left: 2px;
            vertical-align: text-bottom;
            animation: blink 0.8s infinite;
        }
        @keyframes blink {
            0%,
            100% {
                opacity: 1;
            }
            50% {
                opacity: 0;
            }
        }
        .response-box .stats {
            margin-top: 12px;
            font-size: 0.75rem;
            color: #5a5d66;
            border-top: 1px solid var(--border);
            padding-top: 10px;
            display: flex;
            gap: 16px;
            flex-wrap: wrap;
        }

        /* ── Examples strip ───────────── */
        .examples-strip {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
            margin-top: 2px;
        }
        .examples-strip .btn-example {
            font-size: 0.78rem;
            padding: 5px 11px;
            border-radius: 99px;
            border: 1px solid var(--border);
            background: var(--surface2);
            color: var(--text2);
        }
        .examples-strip .btn-example:hover {
            background: #252830;
            color: var(--text);
            border-color: #3a3d47;
        }

        /* ── Footer info ──────────────── */
        .footer-note {
            margin-top: 20px;
            font-size: 0.78rem;
            color: #4a4d56;
            text-align: center;
            max-width: 720px;
        }
        .footer-note a {
            color: var(--accent2);
            text-decoration: none;
        }
        .footer-note a:hover {
            text-decoration: underline;
        }

        /* ── Responsive ───────────────── */
        @media (max-width: 520px) {
            .header h1 {
                font-size: 1.5rem;
            }
            .card {
                padding: 18px 16px 20px;
                gap: 14px;
            }
            .btn-row {
                flex-direction: column;
            }
            .btn {
                justify-content: center;
            }
            .model-row {
                flex-direction: column;
                align-items: flex-start;
            }
            .model-select {
                width: 100%;
            }
        }
    </style>
</head>
<body>

    <!-- ═══════════ Header ═══════════ -->
    <div class="header">
        <div class="badge">Бесплатно • Без API-ключа • User-Pays</div>
        <h1>Claude AI <span>бесплатно</span></h1>
        <p>
            Используйте Claude Opus 4.8, Sonnet 4.6, Haiku 4.5 и другие модели
            через <strong>Puter.js</strong>. Без серверов, без ключей, без
            ограничений для разработчика.
        </p>
    </div>

    <!-- ═══════════ Main Card ═══════════ -->
    <div class="card" id="app">
        <!-- Выбор модели -->
        <div class="model-row">
            <label for="modelSelect">🧠 Модель:</label>
            <select id="modelSelect" class="model-select">
                <option value="claude-sonnet-4-6" selected>Claude Sonnet 4.6 (сбалансированный)</option>
                <option value="claude-opus-4-8">Claude Opus 4.8 (самый мощный)</option>
                <option value="claude-opus-4-7">Claude Opus 4.7</option>
                <option value="claude-opus-4-7-fast">Claude Opus 4.7 Fast (2.5× быстрее)</option>
                <option value="claude-opus-4-6">Claude Opus 4.6</option>
                <option value="claude-opus-4-5">Claude Opus 4.5</option>
                <option value="claude-haiku-4-5">Claude Haiku 4.5 (быстрый и лёгкий)</option>
                <option value="claude-sonnet-4-5">Claude Sonnet 4.5</option>
                <option value="claude-sonnet-4">Claude Sonnet 4</option>
            </select>
            <label class="stream-toggle" title="Потоковый вывод ответа в реальном времени">
                <input type="checkbox" id="streamToggle" checked>
                Стриминг
            </label>
        </div>

        <!-- Поле ввода -->
        <div class="prompt-area">
            <label for="promptInput">💬 Ваш запрос:</label>
            <textarea
            id="promptInput"
            placeholder="Напишите что угодно — объяснение, код, эссе, стихотворение..."
        >Объясни квантовые вычисления простыми словами</textarea>
        <!-- Примеры -->
        <div class="examples-strip">
            <button class="btn-example" data-prompt="Объясни квантовые вычисления простыми словами">⚛️ Квантовые вычисления</button>
            <button class="btn-example" data-prompt="Напиши стихотворение о программировании">🎵 Стих о коде</button>
            <button class="btn-example" data-prompt="Напиши функцию на Python для быстрой сортировки с комментариями">🐍 QuickSort</button>
            <button class="btn-example" data-prompt="Каковы плюсы и минусы использования TypeScript против JavaScript в 2026 году?">📘 TS vs JS</button>
            <button class="btn-example" data-prompt="Придумай идею для стартапа в сфере образования с использованием ИИ">💡 Идея стартапа</button>
        </div>
    </div>

    <!-- Кнопки -->
    <div class="btn-row">
        <button class="btn btn-primary" id="btnSend">🚀 Отправить</button>
        <button class="btn btn-outline" id="btnClear">🗑 Очистить ответ</button>
        <button class="btn btn-outline" id="btnStop" style="display:none;">⏹ Остановить</button>
    </div>

    <!-- Блок ответа -->
    <div class="response-box" id="responseBox">
        <span class="placeholder">Ответ Claude появится здесь…</span>
    </div>
</div>

<!-- ═══════════ Footer ═══════════ -->
<div class="footer-note">
    Работает через <a href="https://docs.puter.com/AI/" target="_blank" rel="noopener">Puter.js</a>
    — модель <strong>User-Pays</strong>: пользователи оплачивают своё использование,
    разработчик не платит. Без API-ключей.
</div>

<!-- ═══════════ Scripts ═══════════ -->
<script>
    (function() {
        // ── DOM элементы ──────────────
        const modelSelect = document.getElementById('modelSelect');
        const streamToggle = document.getElementById('streamToggle');
        const promptInput = document.getElementById('promptInput');
        const responseBox = document.getElementById('responseBox');
        const btnSend = document.getElementById('btnSend');
        const btnClear = document.getElementById('btnClear');
        const btnStop = document.getElementById('btnStop');
        const exampleButtons = document.querySelectorAll('.btn-example');

        let abortController = null; // для отмены стриминга
        let isStreaming = false;

        // ── Функция: обновить UI во время стриминга ──
        function setStreamingState(active) {
            isStreaming = active;
            btnSend.disabled = active;
            btnStop.style.display = active ? 'inline-flex' : 'none';
            if (active) {
                responseBox.classList.add('streaming');
            } else {
                responseBox.classList.remove('streaming');
            }
        }

        // ── Функция: показать ответ ──
        function showResponse(text, modelUsed, streamingUsed) {
            const modelName = modelSelect.options[modelSelect.selectedIndex]?.text || modelUsed;
            const streamNote = streamingUsed ? ' • стриминг' : '';
            responseBox.innerHTML =
                `<div>${escapeHTML(text)}</div>
                 <div class="stats">
                   <span>🧠 ${modelUsed}</span>
                   <span>📝 ${text.length.toLocaleString()} символов</span>
                   <span>⚡ ${streamNote || 'без стриминга'}</span>
                 </div>`;
        }

        function showPlaceholder() {
            responseBox.innerHTML = '<span class="placeholder">Ответ Claude появится здесь…</span>';
        }

        function showError(message) {
            responseBox.innerHTML =
                `<div style="color: var(--red);">❌ Ошибка: ${escapeHTML(message)}</div>
                 <div class="stats"><span>Попробуйте ещё раз</span></div>`;
        }

        // ── Простой escape HTML ──
        function escapeHTML(str) {
            const div = document.createElement('div');
            div.appendChild(document.createTextNode(str));
            return div.innerHTML;
        }

        // ── Отправка запроса ──────────
        async function sendRequest() {
            const prompt = promptInput.value.trim();
            if (!prompt) {
                showError('Введите запрос.');
                return;
            }

            const model = modelSelect.value;
            const useStream = streamToggle.checked;

            // Отмена предыдущего стрима
            if (abortController) {
                abortController.abort();
            }
            abortController = new AbortController();

            setStreamingState(true);
            responseBox.innerHTML = '';
            responseBox.classList.add('streaming');

            let fullText = '';

            try {
                if (useStream) {
                    // ── Стриминг ──────────────────
                    const response = await puter.ai.chat(prompt, {
                        model: model,
                        stream: true,
                        signal: abortController.signal,
                    });

                    for await (const part of response) {
                        if (abortController.signal.aborted) break;
                        const chunk = part?.text || '';
                        fullText += chunk;
                        // Обновляем ответ в реальном времени
                        responseBox.innerHTML =
                            `<div>${escapeHTML(fullText)}<span class="cursor-blink"></span></div>`;
                        // Авто-прокрутка вниз
                        responseBox.scrollTop = responseBox.scrollHeight;
                    }

                    // Убираем курсор после завершения
                    responseBox.innerHTML =
                        `<div>${escapeHTML(fullText)}</div>
                         <div class="stats">
                           <span>🧠 ${model}</span>
                           <span>📝 ${fullText.length.toLocaleString()} символов</span>
                           <span>⚡ стриминг</span>
                         </div>`;
                } else {
                    // ── Без стриминга ────────────
                    const response = await puter.ai.chat(prompt, {
                        model: model,
                        stream: false,
                        signal: abortController.signal,
                    });

                    fullText = response?.message?.content?.[0]?.text || JSON.stringify(response, null, 2);
                    showResponse(fullText, model, false);
                }
            } catch (err) {
                if (err.name === 'AbortError') {
                    // Стрим был остановлен пользователем
                    responseBox.innerHTML =
                        `<div>${escapeHTML(fullText)}</div>
                         <div class="stats">
                           <span>🧠 ${model}</span>
                           <span>⏹ Остановлено</span>
                           <span>📝 ${fullText.length.toLocaleString()} символов</span>
                         </div>`;
                } else {
                    console.error(err);
                    showError(err.message || 'Неизвестная ошибка');
                }
            } finally {
                setStreamingState(false);
                abortController = null;
                responseBox.classList.remove('streaming');
            }
        }

        // ── Остановка стриминга ───────
        function stopStreaming() {
            if (abortController) {
                abortController.abort();
                abortController = null;
            }
            setStreamingState(false);
            responseBox.classList.remove('streaming');
        }

        // ── Очистка ───────────────────
        function clearResponse() {
            stopStreaming();
            showPlaceholder();
            promptInput.focus();
        }

        // ── Примеры ───────────────────
        exampleButtons.forEach(btn => {
            btn.addEventListener('click', () => {
                const prompt = btn.getAttribute('data-prompt');
                if (prompt) {
                    promptInput.value = prompt;
                    promptInput.focus();
                    // Авто-отправка при клике на пример
                    sendRequest();
                }
            });
        });

        // ── Обработчики ───────────────
        btnSend.addEventListener('click', sendRequest);
        btnClear.addEventListener('click', clearResponse);
        btnStop.addEventListener('click', stopStreaming);

        // Отправка по Ctrl+Enter
        promptInput.addEventListener('keydown', (e) => {
            if ((e.ctrlKey || e.metaKey) && e.key === 'Enter') {
                e.preventDefault();
                sendRequest();
            }
        });

        // ── Инициализация ─────────────
        promptInput.focus();
        console.log('✅ Бесплатный Claude AI готов. Модель по умолчанию:', modelSelect.value);
        console.log('📚 Документация: https://docs.puter.com/AI/');
    })();
</script>
</body>
</html>
