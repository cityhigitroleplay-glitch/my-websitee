<!---->
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <title>WormGpt | Gemini AI Deniz</title>
    <style>
        :root {
            --bg-primary: #0a0a0f;
            --bg-secondary: #121218;
            --bg-tertiary: #1a1a25;
            --bg-input: rgba(30, 30, 42, 0.7);
            --text-primary: #e6e6ff;
            --text-secondary: #a0a0c0;
            --text-accent: #bb86fc;
            --border-primary: #2d2d40;
            --border-secondary: #3a3a50;
            --primary-gradient: linear-gradient(135deg, #5365F7, #6b7dff);
            --primary-hover: #4a5ae8;
            --accent-gradient: linear-gradient(135deg, #bb86fc, #9c6fff);
            --success: #4caf50;
            --error: #cf6679;
            --shadow-sm: 0 2px 10px rgba(0, 0, 0, 0.2);
            --shadow-md: 0 4px 20px rgba(0, 0, 0, 0.3);
            --shadow-lg: 0 8px 30px rgba(0, 0, 0, 0.4);
            --shadow-glow: 0 0 15px rgba(83, 101, 247, 0.3);
            --accent-glow: 0 0 20px rgba(107, 125, 255, 0.4);
            --loading-glow: 0 0 12px rgba(83, 101, 247, 0.6);
            --radius-sm: 8px;
            --radius-md: 12px;
            --radius-lg: 18px;
            --radius-xl: 24px;
            /* Variabel untuk Tinggi Elemen Tetap */
            --header-height: 72px;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        /* ---------------------------------------------------- */
        /* PERBAIKAN UTAMA SCROLLING */
        /* ---------------------------------------------------- */

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-primary);
            color: var(--text-primary);
            display: flex;
            flex-direction: column;
            /* Pastikan body mengisi seluruh viewport */
            height: 100vh;
            /* Tinggi yang tersedia di device, termasuk safe area */
            height: 100dvh;
            overflow: hidden; /* Body tidak perlu scroll */
            transition: background-color 0.3s ease;
        }

        #chat-container {
            /* flex: 1 akan membuat kontainer ini mengambil sisa ruang */
            flex: 1;
            display: flex;
            flex-direction: column;
            /* Hapus padding-top dan padding-bottom yang tidak perlu karena header dan input area sudah terpisah */
            overflow: hidden;
            /* Menyesuaikan dengan header fixed */
            padding-top: var(--header-height);
        }

        #chat-window {
            /* flex: 1 akan membuat ini bisa di-scroll secara independen */
            flex-grow: 1;
            overflow-y: auto; /* Ini yang membuat scroll penuh */
            padding: 24px 20px 20px 20px;
            scroll-behavior: smooth;
            background: linear-gradient(to bottom, transparent, rgba(26, 26, 46, 0.1));
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
        }

        #input-area {
            /* Hapus 'position: fixed' */
            /* Background/style dipertahankan */
            background: var(--bg-secondary);
            padding: 16px 20px calc(16px + env(safe-area-inset-bottom));
            border-top: 1px solid var(--border-primary);
            box-shadow: var(--shadow-lg);
            margin: 0;
            z-index: 100;
            flex-shrink: 0; /* Pastikan input area tidak mengecil */
        }

        /* ---------------------------------------------------- */
        /* END PERBAIKAN SCROLLING */
        /* ---------------------------------------------------- */

        header {
            background: var(--bg-secondary);
            color: var(--text-primary);
            padding: 16px 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid var(--border-primary);
            position: fixed; /* Tetap fixed */
            top: 0;
            left: 0;
            right: 0;
            z-index: 1000;
            box-shadow: var(--shadow-md);
            padding-top: max(16px, env(safe-area-inset-top));
            transform: translateZ(0);
            will-change: transform;
            height: var(--header-height);
        }

        .header-content {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .back-button {
            background: var(--bg-tertiary);
            border: 1px solid var(--border-secondary);
            color: var(--text-primary);
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 18px;
            box-shadow: 0 0 10px rgba(83, 101, 247, 0.1);
        }

        .back-button:hover {
            background: var(--primary-gradient);
            transform: scale(1.05);
            box-shadow: var(--shadow-glow);
        }

        .logo-container {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .logo-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            object-fit: cover;
            border: 2px solid #6b7dff;
            box-shadow: 0 0 15px rgba(107, 125, 255, 0.7);
        }

        .app-title {
            font-size: 1rem;
            font-weight: 700;
            letter-spacing: 0.5px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .status-indicator {
            display: inline-block;
            width: 10px;
            height: 10px;
            background-color: var(--success);
            border-radius: 50%;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }

        .model-selector {
            position: relative;
            display: inline-block;
        }

        .model-btn {
            background: var(--bg-tertiary);
            border: 1px solid var(--border-secondary);
            color: var(--text-primary);
            padding: 8px 16px;
            border-radius: var(--radius-lg);
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: all 0.2s ease;
            font-size: 0.9rem;
            min-width: 120px;
        }

        .model-btn:hover {
            background: var(--bg-input);
        }

        .model-btn i {
            transition: transform 0.2s ease;
        }

        .model-btn.open i {
            transform: rotate(180deg);
        }

        .model-dropdown {
            position: absolute;
            top: 100%;
            right: 0;
            background: var(--bg-tertiary);
            border: 1px solid var(--border-secondary);
            border-radius: var(--radius-lg);
            box-shadow: var(--shadow-md);
            width: 100%;
            min-width: 140px;
            z-index: 101;
            display: none;
            margin-top: 4px;
            overflow: hidden;
        }

        .model-dropdown.open {
            display: block;
        }

        .model-option {
            padding: 12px 16px;
            cursor: pointer;
            transition: background 0.2s ease;
            color: var(--text-secondary);
            font-size: 0.9rem;
        }

        .model-option:hover {
            background: var(--bg-input);
            color: var(--text-primary);
        }

        .model-option.active {
            background: var(--primary-gradient);
            color: white;
        }

        #chat-window::-webkit-scrollbar {
            width: 4px;
        }

        #chat-window::-webkit-scrollbar-track {
            background: var(--bg-secondary);
            border-radius: 4px;
        }

        #chat-window::-webkit-scrollbar-thumb {
            background: var(--primary-gradient);
            border-radius: 4px;
            box-shadow: var(--shadow-glow);
        }

        .message {
            margin-bottom: 12px;
            padding: 20px 24px;
            border-radius: var(--radius-lg);
            max-width: 95%; /* Dipertahankan 95% */
            line-height: 1.6;
            animation: fadeInUp 0.3s ease-out;
            box-shadow: var(--shadow-md);
            white-space: pre-wrap;
            position: relative;
            border: 1px solid var(--border-secondary);
            transition: all 0.3s ease;
            background: var(--bg-tertiary);
        }

        .message:hover {
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            transform: translateY(-2px);
        }

        .user-message {
            margin-left: auto; /* Memposisikan ke kanan */
            margin-right: 0;
            border-bottom-right-radius: var(--radius-sm);
            border-left: 3px solid #7d8bff;
        }

        .ai-message {
            margin-right: auto; /* Memposisikan ke kiri */
            margin-left: 0;
            border-bottom-left-radius: var(--radius-sm);
            border-right: 3px solid #7d8bff;
            background: var(--bg-input);
        }

        .error-message {
            background: rgba(207, 102, 121, 0.15);
            color: var(--error);
            border-color: var(--error);
            box-shadow: 0 0 15px rgba(207, 102, 121, 0.3);
        }

        .message code {
            background-color: rgba(83, 101, 247, 0.15);
            color: #C0C8FF;
            padding: 3px 8px;
            border-radius: var(--radius-sm);
            font-family: 'Consolas', 'Monaco', monospace;
            font-size: 0.9em;
            border: 1px solid rgba(83, 101, 247, 0.3);
        }

        .message table {
            width: 100%;
            border-collapse: collapse;
            margin: 16px 0;
            border-radius: var(--radius-md);
            overflow: hidden;
            box-shadow: var(--shadow-sm);
            border: 1px solid var(--border-secondary);
        }

        .message table th,
        .message table td {
            padding: 10px 14px;
            border: 1px solid var(--border-secondary);
            text-align: left;
        }

        .message table th {
            background-color: rgba(83, 101, 247, 0.2);
            font-weight: 600;
            color: var(--text-accent);
        }

        .message table tr:nth-child(even) {
            background-color: rgba(26, 26, 46, 0.3);
        }

        .message h3 {
            font-size: 1.4rem;
            font-weight: 600;
            color: #9c6fff;
            margin: 16px 0 8px 0;
            padding-bottom: 4px;
            border-bottom: 1px solid var(--border-secondary);
        }

        .message ul, .message ol {
            padding-left: 24px;
            margin: 12px 0;
        }

        .message li {
            margin-bottom: 6px;
        }

        .code-container pre code {
            background-color: transparent;
            color: inherit;
            padding: 0;
            border-radius: 0;
            font-size: inherit;
        }

        .code-container {
            position: relative;
            margin: 16px 0;
            border-radius: var(--radius-md);
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.5);
            border: 1px solid var(--border-secondary);
        }

        .code-container pre {
            background-color: #161625;
            color: var(--text-primary);
            padding: 20px;
            margin: 0;
            overflow-x: auto;
            font-family: 'Consolas', 'Monaco', monospace;
            font-size: 14px;
            border-left: 4px solid #6b7dff;
            line-height: 1.5;
        }

        .copy-button {
            position: absolute;
            top: 10px;
            right: 10px;
            background: linear-gradient(90deg, #5365F7, #6b7dff);
            color: white;
            border: none;
            padding: 6px 12px;
            border-radius: var(--radius-sm);
            cursor: pointer;
            font-size: 11px;
            font-weight: 600;
            transition: all 0.2s ease;
            opacity: 0;
            z-index: 1;
            box-shadow: 0 0 10px rgba(83, 101, 247, 0.5);
        }

        .code-container:hover .copy-button {
            opacity: 1;
        }

        .copy-button:hover {
            opacity: 1;
            transform: translateY(-2px);
            box-shadow: 0 0 15px rgba(107, 125, 255, 0.7);
        }

        .copy-button:active {
            transform: scale(0.95);
        }

        #chat-input-wrapper {
            display: flex;
            align-items: flex-end;
            gap: 12px;
            width: 100%;
            margin: 0;
            flex-shrink: 0;
        }

        #chat-input-container {
            flex-grow: 1;
            background: var(--bg-input);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(131, 144, 255, 0.15);
            border-radius: var(--radius-xl);
            padding: 16px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            box-shadow: 0 10px 40px rgba(83, 101, 247, 0.08);
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            min-height: 56px;
            max-height: 40vh;
            overflow-y: auto;
            position: relative;
            z-index: 50;
        }

        #chat-input-container:focus-within {
            border-color: #6b7dff;
            box-shadow: 0 0 0 2px rgba(83, 101, 247, 0.3), 0 10px 40px rgba(83, 101, 247, 0.25);
            transform: translateY(-2px);
        }

        .image-preview-container {
            display: none;
            width: 100px;
            height: 100px;
            border-radius: var(--radius-md);
            overflow: hidden;
            background: rgba(26, 26, 46, 0.4);
            border: 1px solid rgba(83, 101, 247, 0.2);
            box-shadow: 0 4px 20px rgba(83, 101, 247, 0.1);
            transition: all 0.3s ease;
            position: relative;
            margin-bottom: 8px;
            align-self: flex-start;
        }

        .image-preview {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
            transition: transform 0.3s ease;
            border-radius: var(--radius-md);
        }

        .image-preview-container:hover .image-preview {
            transform: scale(1.05);
        }

        .remove-image-btn {
            position: absolute;
            top: -8px;
            right: -8px;
            background: #ff5555;
            color: white;
            width: 24px;
            height: 24px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 12px;
            font-weight: 600;
            opacity: 0;
            transition: all 0.2s ease;
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .image-preview-container:hover .remove-image-btn {
            opacity: 1;
        }

        .remove-image-btn:hover {
            background: var(--error);
            transform: scale(1.1);
        }

        #chat-input {
            flex-grow: 1;
            padding: 0;
            border: none;
            background: transparent;
            color: var(--text-primary);
            outline: none;
            font-size: 16px;
            font-weight: 500;
            resize: none;
            min-height: 24px;
            max-height: 30vh;
            overflow-y: auto;
            font-family: inherit;
            line-height: 1.6;
            width: 100%;
            scrollbar-width: none;
            -ms-overflow-style: none;
        }

        #chat-input::placeholder {
            color: var(--text-secondary);
            opacity: 0.7;
        }

        #chat-input::-webkit-scrollbar {
            width: 0;
            height: 0;
        }

        .input-actions {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-top: 4px;
            pointer-events: all;
        }

        .image-upload-btn {
            background: var(--bg-tertiary);
            border: 1px solid var(--border-secondary);
            color: var(--text-primary);
            width: 44px;
            height: 44px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            font-size: 18px;
            box-shadow: 0 4px 16px rgba(83, 101, 247, 0.1);
        }

        .image-upload-btn:hover {
            background: var(--primary-gradient);
            transform: translateY(-3px) scale(1.05);
            box-shadow: var(--shadow-glow);
        }

        #send-button {
            background: var(--primary-gradient);
            color: white;
            border: none;
            padding: 0;
            border-radius: 50%;
            width: 56px;
            height: 56px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 0 0 0 rgba(83, 101, 247, 0.5);
            animation: pulseButton 2s infinite;
            font-size: 18px;
            margin-top: 2px;
        }

        @keyframes pulseButton {
            0% {
                box-shadow: 0 0 0 0 rgba(83, 101, 247, 0.7);
            }
            70% {
                box-shadow: 0 0 0 10px rgba(83, 101, 247, 0);
            }
            100% {
                box-shadow: 0 0 0 0 rgba(83, 101, 247, 0);
            }
        }

        #send-button:hover:not(:disabled) {
            transform: translateY(-4px) scale(1.08);
            box-shadow: 0 10px 30px rgba(83, 101, 247, 0.4);
        }

        #send-button:disabled {
            background-color: #33334D;
            cursor: not-allowed;
            opacity: 0.6;
            transform: scale(1);
            animation: none;
        }

        .send-button-loading i {
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(12px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .loading::after {
            content: '...';
            position: absolute;
            animation: dot-loading 1.4s infinite steps(3, end);
            color: var(--text-accent);
            font-weight: 500;
        }

        @keyframes dot-loading {
            0% { content: '.'; }
            33% { content: '..'; }
            66% { content: '...'; }
        }

        #welcome-modal {
            position: fixed;
            top: 0;
            left: 0;
            w
