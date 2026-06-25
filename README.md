<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>Zzy - Detektif Pembunuhan</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            user-select: none;
        }

        body {
            background: #0e0e1a;
            font-family: 'Segoe UI', system-ui, -apple-system, Arial, sans-serif;
            touch-action: manipulation;
            overflow: hidden;
            height: 100vh;
            width: 100vw;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        #game {
            width: 100%;
            max-width: 480px;
            height: 100vh;
            max-height: 860px;
            background: #111122;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            position: relative;
            box-shadow: 0 0 60px rgba(0, 0, 0, 0.9);
        }

        /* ===== HEADER ===== */
        #header {
            background: #18182a;
            padding: 12px 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px solid #b89a6a;
            flex-shrink: 0;
            z-index: 2;
        }

        #header h1 {
            font-size: 20px;
            color: #b89a6a;
            font-weight: 700;
            letter-spacing: 0.5px;
        }

        #header h1 span {
            color: #b0b0c8;
            font-weight: 300;
        }

        #header-right {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        #clue-counter {
            background: rgba(184, 154, 106, 0.12);
            padding: 4px 14px;
            border-radius: 20px;
            color: #b89a6a;
            font-size: 13px;
            font-weight: 600;
            border: 1px solid rgba(184, 154, 106, 0.15);
        }

        #reset-btn {
            background: transparent;
            border: 1px solid rgba(255, 255, 255, 0.08);
            color: #8a8aaa;
            border-radius: 50%;
            width: 32px;
            height: 32px;
            font-size: 16px;
            cursor: pointer;
            touch-action: manipulation;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background 0.15s;
        }

        #reset-btn:active {
            background: rgba(255, 255, 255, 0.05);
        }

        /* ===== SCENE ===== */
        #scene {
            flex: 1;
            position: relative;
            background: #1a1a30;
            overflow: hidden;
            min-height: 180px;
        }

        #scene-bg {
            position: absolute;
            inset: 0;
            background: radial-gradient(ellipse at 30% 40%, #2a2a48, #121224);
            transition: background 0.5s;
        }

        #scene-label {
            position: absolute;
            top: 12px;
            left: 50%;
            transform: translateX(-50%);
            color: rgba(255, 255, 255, 0.15);
            font-size: 12px;
            letter-spacing: 2px;
            text-transform: uppercase;
            font-weight: 600;
            pointer-events: none;
            z-index: 1;
        }

        .scene-object {
            position: absolute;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            touch-action: manipulation;
            padding: 8px 12px;
            border-radius: 12px;
            background: rgba(255, 255, 255, 0.02);
            border: 1px solid rgba(255, 255, 255, 0.05);
            min-width: 56px;
            min-height: 56px;
            text-align: center;
            z-index: 2;
            transition: transform 0.12s, background 0.15s, border-color 0.15s;
            color: #c8c8dc;
            font-weight: 500;
            font-size: 13px;
            line-height: 1.3;
        }

        .scene-object:active {
            transform: scale(0.94);
            background: rgba(255, 255, 255, 0.04);
        }

        .scene-object .icon {
            font-size: 26px;
            margin-bottom: 2px;
        }

        .scene-object .label {
            font-size: 10px;
            color: #8a8aaa;
            max-width: 72px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .scene-object.npc {
            border-color: rgba(184, 154, 106, 0.15);
            background: rgba(184, 154, 106, 0.04);
        }

        .scene-object.npc .icon {
            color: #c8b08a;
        }

        .scene-object.item {
            border-color: rgba(120, 180, 220, 0.12);
            background: rgba(120, 180, 220, 0.04);
        }

        .scene-object.item .icon {
            color: #8ab8d8;
        }

        .scene-object.door {
            border-color: rgba(160, 160, 200, 0.08);
            background: rgba(160, 160, 200, 0.03);
        }

        .scene-object.door .icon {
            color: #8888aa;
        }

        .scene-object.collected {
            opacity: 0.2;
            pointer-events: none;
            filter: grayscale(0.8);
        }

        .scene-object .check {
            position: absolute;
            top: -4px;
            right: -4px;
            background: #b89a6a;
            color: #111122;
            border-radius: 50%;
            width: 18px;
            height: 18px;
            font-size: 10px;
            font-weight: 700;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .scene-object .glow {
            position: absolute;
            inset: -4px;
            border-radius: 16px;
            background: radial-gradient(circle, rgba(184, 154, 106, 0.06), transparent 70%);
            z-index: -1;
        }

        /* ===== ACCUSE BUTTON ===== */
        #accuse-btn {
            position: absolute;
            bottom: 16px;
            right: 16px;
            background: #a04040;
            color: #fff;
            border: none;
            border-radius: 28px;
            padding: 10px 20px;
            font-weight: 700;
            font-size: 13px;
            box-shadow: 0 4px 24px rgba(160, 64, 64, 0.35);
            cursor: pointer;
            touch-action: manipulation;
            z-index: 5;
            display: none;
            letter-spacing: 0.3px;
            border: 1px solid rgba(255, 255, 255, 0.06);
            transition: transform 0.12s;
        }

        #accuse-btn:active {
            transform: scale(0.92);
        }

        #accuse-btn.show {
            display: block;
            animation: fadeUp 0.3s ease;
        }

        @keyframes fadeUp {
            0% {
                opacity: 0;
                transform: translateY(12px) scale(0.95);
            }
            100% {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }

        /* ===== INTERACTION PANEL ===== */
        #interaction {
            background: #121222;
            border-top: 1px solid #22223a;
            border-bottom: 1px solid #22223a;
            padding: 10px 14px;
            flex-shrink: 0;
            min-height: 110px;
            max-height: 160px;
            display: flex;
            flex-direction: column;
            overflow-y: auto;
        }

        #dialogue-speaker {
            color: #b89a6a;
            font-weight: 700;
            font-size: 13px;
            margin-bottom: 2px;
        }

        #dialogue-text {
            color: #c8c8dc;
            font-size: 14px;
            line-height: 1.6;
            margin-bottom: 8px;
            flex: 1;
            min-height: 32px;
        }

        #dialogue-choices {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
        }

        .choice-btn {
            background: rgba(184, 154, 106, 0.06);
            border: 1px solid rgba(184, 154, 106, 0.12);
            color: #c8c8dc;
            padding: 6px 14px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            cursor: pointer;
            touch-action: manipulation;
            transition: all 0.12s;
            flex: 1 0 auto;
            min-width: 48px;
        }

        .choice-btn:active {
            background: rgba(184, 154, 106, 0.15);
            transform: scale(0.94);
        }

        .choice-btn.primary {
            background: #b89a6a;
            color: #111122;
            border-color: #b89a6a;
        }

        .choice-btn.primary:active {
            background: #a88858;
        }

        /* ===== INVENTORY ===== */
        #inventory {
            background: #0e0e1e;
            padding: 6px 12px;
            display: flex;
            align-items: center;
            gap: 6px;
            flex-shrink: 0;
            border-top: 1px solid #18182a;
            min-height: 40px;
            overflow-x: auto;
            flex-wrap: nowrap;
        }

        #inventory-label {
            color: #5a6a7a;
            font-size: 11px;
            font-weight: 600;
            margin-right: 4px;
            white-space: nowrap;
            letter-spacing: 0.5px;
        }

        .inv-item {
            background: rgba(255, 255, 255, 0.02);
            border: 1px solid rgba(184, 154, 106, 0.08);
            border-radius: 6px;
            padding: 2px 12px;
            font-size: 12px;
            color: #b0b0c8;
            white-space: nowrap;
            height: 28px;
            display: flex;
            align-items: center;
        }

        #inventory-empty {
            color: #3a4a5a;
            font-size: 12px;
            font-style: italic;
        }

        /* ===== MODAL ===== */
        #modal-overlay {
            position: absolute;
            inset: 0;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(6px);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 10;
            padding: 24px;
        }

        #modal-overlay.show {
            display: flex;
        }

        #modal-box {
            background: #18182a;
            border: 2px solid #b89a6a;
            border-radius: 24px;
            padding: 24px 20px;
            max-width: 360px;
            width: 100%;
            box-shadow: 0 24px 64px rgba(0, 0, 0, 0.9);
            max-height: 80vh;
            overflow-y: auto;
        }

        #modal-box h2 {
            color: #b89a6a;
            font-size: 20px;
            text-align: center;
            margin-bottom: 6px;
            letter-spacing: 0.5px;
        }

        #modal-box .sub {
            color: #7a8a9a;
            font-size: 13px;
            text-align: center;
            margin-bottom: 16px;
        }

        .suspect-option {
            background: rgba(255, 255, 255, 0.02);
            border: 1px solid rgba(255, 255, 255, 0.04);
            border-radius: 12px;
            padding: 12px 14px;
            margin-bottom: 8px;
            color: #c8c8dc;
            font-size: 15px;
            font-weight: 500;
            cursor: pointer;
            touch-action: manipulation;
            transition: all 0.12s;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .suspect-option:active {
            background: rgba(184, 154, 106, 0.06);
            transform: scale(0.97);
            border-color: rgba(184, 154, 106, 0.15);
        }

        .suspect-option .badge {
            font-size: 11px;
            color: #7a8a9a;
            background: rgba(0, 0, 0, 0.3);
            padding: 2px 12px;
            border-radius: 20px;
        }

        #modal-result {
            margin-top: 16px;
            padding: 14px;
            border-radius: 14px;
            background: rgba(184, 154, 106, 0.04);
            border: 1px solid rgba(184, 154, 106, 0.08);
            color: #c8c8dc;
            font-size: 14px;
            line-height: 1.6;
            display: none;
        }

        #modal-result.show {
            display: block;
        }

        #modal-result .win {
            color: #7ddf9a;
            font-weight: 700;
        }

        #modal-result .lose {
            color: #f78b8b;
            font-weight: 700;
        }

        #modal-close {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 14px;
            background: rgba(255, 255, 255, 0.03);
            color: #8a9aaa;
            font-weight: 600;
            font-size: 15px;
            cursor: pointer;
            margin-top: 12px;
            touch-action: manipulation;
            transition: background 0.12s;
        }

        #modal-close:active {
            background: rgba(255, 255, 255, 0.06);
        }

        /* ===== TOAST ===== */
        .toast {
            position: absolute;
            top: 56px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.88);
            backdrop-filter: blur(6px);
            border: 1px solid rgba(184, 154, 106, 0.15);
            border-radius: 12px;
            padding: 10px 20px;
            color: #d0d0e0;
            font-size: 13px;
            z-index: 20;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.6);
            max-width: 85%;
            pointer-events: none;
            animation: toastIn 0.3s ease;
            text-align: center;
        }

        @keyframes toastIn {
            0% {
                opacity: 0;
                transform: translateX(-50%) translateY(-10px) scale(0.95);
            }
            100% {
                opacity: 1;
                transform: translateX(-50%) translateY(0) scale(1);
            }
        }

        /* ===== RESPONSIVE ===== */
        @media (max-width: 480px) {
            .scene-object {
                font-size: 12px;
                min-width: 48px;
                min-height: 48px;
                padding: 6px 8px;
            }
            .scene-object .icon {
                font-size: 22px;
            }
            .scene-object .label {
                font-size: 9px;
                max-width: 56px;
            }
            #header h1 {
                font-size: 17px;
            }
            #clue-counter {
                font-size: 12px;
                padding: 3px 10px;
            }
            #interaction {
                padding: 8px 12px;
                min-height: 96px;
                max-height: 130px;
            }
            #dialogue-text {
                font-size: 13px;
            }
            .choice-btn {
                font-size: 11px;
                padding: 4px 12px;
            }
            #inventory {
                padding: 4px 8px;
                min-height: 34px;
            }
            .inv-item {
                font-size: 11px;
                padding: 0 10px;
                height: 24px;
            }
            #accuse-btn {
                bottom: 12px;
                right: 12px;
                padding: 8px 16px;
                font-size: 12px;
            }
            #modal-box {
                padding: 18px 14px;
            }
            #modal-box h2 {
                font-size: 18px;
            }
            #reset-btn {
                width: 28px;
                height: 28px;
                font-size: 14px;
            }
        }

        @media (max-height: 680px) {
            #scene {
                min-height: 120px;
            }
            #interaction {
                min-height: 80px;
                max-height: 110px;
                padding: 6px 10px;
            }
            #dialogue-text {
                font-size: 12px;
                min-height: 24px;
            }
            #inventory {
                min-height: 30px;
                padding: 3px 8px;
            }
            .scene-object {
                min-width: 40px;
                min-height: 40px;
                padding: 4px 6px;
            }
            .scene-object .icon {
                font-size: 18px;
            }
            #header {
                padding: 8px 12px;
            }
            #header h1 {
                font-size: 15px;
            }
        }

        ::-webkit-scrollbar {
            width: 3px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(184, 154, 106, 0.15);
            border-radius: 10px;
        }
    </style>
</head>
<body>

    <div id="game">
        <!-- HEADER -->
        <div id="header">
            <h1><span>Z</span>zy</h1>
            <div id="header-right">
                <div id="clue-counter">Bukti: <span id="clue-count">0</span>/4</div>
                <button id="reset-btn" title="Mulai Ulang">↻</button>
            </div>
        </div>

        <!-- SCENE -->
        <div id="scene">
            <div id="scene-bg"></div>
            <div id="scene-label"></div>
            <button id="accuse-btn">Tuduh Pelaku</button>
        </div>

        <!-- INTERACTION -->
        <div id="interaction">
            <div id="dialogue-speaker"></div>
            <div id="dialogue-text">Selamat datang, detektif. Selidiki pembunuhan Mr. Blackwood. Kumpulkan bukti dan wawancarai tersangka.</div>
            <div id="dialogue-choices"></div>
        </div>

        <!-- INVENTORY -->
        <div id="inventory">
            <span id="inventory-label">INVENTARIS</span>
            <div id="inventory-items" style="display:flex;gap:4px;flex-wrap:nowrap;overflow-x:auto;flex:1;padding:2px 0;">
                <span id="inventory-empty">(kosong)</span>
            </div>
        </div>

        <!-- MODAL -->
        <div id="modal-overlay">
            <div id="modal-box">
                <h2>Siapa Pembunuhnya?</h2>
                <div class="sub">Pilih tersangka berdasarkan bukti yang terkumpul.</div>
                <div id="modal-suspect-list"></div>
                <div id="modal-result"></div>
                <button id="modal-close">Tutup</button>
            </div>
        </div>
    </div>

    <script>
        // ============================================================
        //  GAME DATA
        // ============================================================
        const DATA = {
            location: 'livingroom',
            inventory: [],
            cluesCollected: 0,
            totalClues: 4,
            talkedTo: {},
            gameOver: false,
            killerId: 'mrs_blackwood',

            suspects: [
                { id: 'mrs_blackwood', name: 'Mrs. Blackwood', desc: 'Istri korban, motif warisan' },
                { id: 'mr_green', name: 'Mr. Green', desc: 'Butler, motif hutang' },
                { id: 'ms_white', name: 'Ms. White', desc: 'Sekretaris, motif pemerasan' },
            ],

            items: {
                letter: { id: 'letter', name: 'Surat Ancaman', desc: 'Surat ancaman dari Mrs. Blackwood untuk suaminya.' },
                knife: { id: 'knife', name: 'Pisau Berdarah', desc: 'Pisau dapur dengan noda darah.' },
                will: { id: 'will', name: 'Surat Wasiat', desc: 'Wasiat yang menyebut Mrs. Blackwood sebagai pewaris tunggal.' },
                recording: { id: 'recording', name: 'Rekaman Telepon', desc: 'Rekaman percakapan Mrs. Blackwood yang mengancam.' },
            },

            locations: {
                livingroom: {
                    name: 'Ruang Tamu',
                    bg: '#1a1a30',
                    label: 'Ruang Tamu',
                    objects: [
                        { id: 'letter', type: 'item', label: 'Surat', icon: '📄', x: 22, y: 38, itemId: 'letter' },
                        { id: 'mrs_blackwood', type: 'npc', label: 'Mrs. Blackwood', icon: '👤', x: 55, y: 22,
                        npcId: 'mrs_blackwood' },
                        { id: 'mr_green', type: 'npc', label: 'Mr. Green', icon: '👤', x: 78, y: 52, npcId: 'mr_green' },
                        { id: 'door_kitchen', type: 'door', label: 'Ke Dapur', icon: '🚪', x: 8, y: 76, target: 'kitchen' },
                        { id: 'door_bedroom', type: 'door', label: 'Ke Kamar', icon: '🚪', x: 90, y: 76,
                        target: 'bedroom' },
                    ]
                },
                kitchen: {
                    name: 'Dapur',
                    bg: '#1a2a1e',
                    label: 'Dapur',
                    objects: [
                        { id: 'knife', type: 'item', label: 'Pisau', icon: '🔪', x: 30, y: 48, itemId: 'knife' },
                        { id: 'ms_white', type: 'npc', label: 'Ms. White', icon: '👤', x: 70, y: 28, npcId: 'ms_white' },
                        { id: 'door_living', type: 'door', label: 'Ke Ruang Tamu', icon: '🚪', x: 50, y: 86,
                            target: 'livingroom' },
                    ]
                },
                bedroom: {
                    name: 'Kamar Tidur',
                    bg: '#1a1a2e',
                    label: 'Kamar Tidur',
                    objects: [
                        { id: 'will', type: 'item', label: 'Wasiat', icon: '📄', x: 60, y: 38, itemId: 'will' },
                        { id: 'door_living2', type: 'door', label: 'Ke Ruang Tamu', icon: '🚪', x: 50, y: 86,
                            target: 'livingroom' },
                    ]
                }
            },

            dialogues: {
                mrs_blackwood: {
                    initial: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Saya tidak tahu apa-apa. Suami saya sudah tidak mencintai saya, tapi saya tidak membunuhnya.',
                        options: [
                            { label: 'Di mana anda tadi malam?', next: 'alibi' },
                            { label: 'Surat ancaman ini untuk anda?', next: 'letter_reaction' },
                            { label: 'Saya tahu anda punya motif.', next: 'motif' },
                        ]
                    },
                    alibi: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Saya di kamar tidur sepanjang malam. Sendirian. Tidak ada saksi.',
                        options: [
                            { label: 'Kembali', next: 'initial' },
                            { label: 'Wasiat menyebut nama anda.', next: 'will_reaction' },
                        ]
                    },
                    letter_reaction: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Itu hanya surat marah biasa. Saya tidak benar-benar bermaksud membunuhnya.',
                        options: [
                            { label: 'Jadi anda mengancamnya?', next: 'threat_admit' },
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    threat_admit: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Saya marah karena dia berselingkuh. Tapi saya tidak membunuhnya.',
                        options: [
                            { label: 'Saya punya rekaman telepon anda.', next: 'phone_reaction' },
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    phone_reaction: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Itu hanya panggilan biasa. Saya tidak tahu tentang rekaman itu.',
                        options: [
                            { label: 'Cukup. Saya tahu anda pelakunya.', next: 'confront' },
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    will_reaction: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Wasiat? Dia bilang dia akan mengubahnya. Tapi dia tidak sempat.',
                        options: [
                            { label: 'Jadi anda membunuhnya untuk warisan?', next: 'confront' },
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    motif: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Saya tahu saya akan mendapat warisan, tapi itu bukan alasan untuk membunuh.',
                        options: [
                            { label: 'Saya akan cari bukti lebih.', next: 'initial' },
                            { label: 'Rekaman telepon anda berbicara.', next: 'phone_reaction' },
                        ]
                    },
                    confront: {
                        speaker: 'Mrs. Blackwood',
                        text: '...Anda benar. Saya membunuhnya. Dia akan meninggalkan saya tanpa apa-apa. Maafkan saya.',
                        options: [
                            { label: 'Saya menangkap anda.', next: 'arrest' },
                        ]
                    },
                    arrest: {
                        speaker: 'Zzy',
                        text: 'Mrs. Blackwood, anda ditangkap atas pembunuhan suami anda. Keadilan ditegakkan.',
                        options: [
                            { label: 'Kasus selesai.', next: 'end' },
                        ]
                    },
                    end: {
                        speaker: 'KASUS TERPECAHKAN',
                        text: 'Selamat! Zzy berhasil mengungkap pembunuhan Mr. Blackwood. Mrs. Blackwood adalah pelakunya.',
                        options: []
                    }
                },
                mr_green: {
                    initial: {
                        speaker: 'Mr. Green',
                        text: 'Saya hanya pelayan. Saya tidak tahu apa-apa tentang pembunuhan ini.',
                        options: [
                            { label: 'Di mana anda tadi malam?', next: 'alibi' },
                            { label: 'Pisau di dapur milik anda?', next: 'knife' },
                        ]
                    },
                    alibi: {
                        speaker: 'Mr. Green',
                        text: 'Saya di dapur sepanjang malam, membersihkan. Tidak ada saksi.',
                        options: [
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    knife: {
                        speaker: 'Mr. Green',
                        text: 'Pisau itu dari dapur, tapi saya tidak menggunakannya untuk membunuh.',
                        options: [
                            { label: 'Kembali', next: 'initial' },
                        ]
                    }
                },
                ms_white: {
                    initial: {
                        speaker: 'Ms. White',
                        text: 'Saya sekretaris Mr. Blackwood. Dia orang baik. Saya tidak percaya dia dibunuh.',
                        options: [
                            { label: 'Di mana anda tadi malam?', next: 'alibi' },
                            { label: 'Surat ancaman ini untuk anda?', next: 'letter' },
                            { label: 'Ada rekaman telepon mencurigakan?', next: 'recording_offer' },
                        ]
                    },
                    alibi: {
                        speaker: 'Ms. White',
                        text: 'Saya di ruang kerja sampai larut. Banyak pekerjaan.',
                        options: [
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    letter: {
                        speaker: 'Ms. White',
                        text: 'Surat itu? Saya tahu Mrs. Blackwood menulisnya. Dia sangat marah pada suaminya.',
                        options: [
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    recording_offer: {
                        speaker: 'Ms. White',
                        text: 'Ya, saya punya rekaman percakapan Mrs. Blackwood yang mengancam suaminya. Saya berikan pada anda.',
                        options: [
                            { label: 'Ambil rekaman', next: 'take_recording' },
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    take_recording: {
                        speaker: 'Zzy',
                        text: 'Rekaman telepon berhasil didapat. Ini bukti kuat.',
                        options: [
                            { label: 'Simpan', next: 'initial' },
                        ],
                        onEnter: function() {
                            if (!DATA.inventory.includes('recording')) {
                                DATA.inventory.push('recording');
                                DATA.cluesCollected++;
                                updateUI();
                                showToast('Rekaman telepon ditambahkan ke inventaris.');
                            }
                        }
                    }
                }
            }
        };

        // ============================================================
        //  STATE
        // ============================================================
        let currentDialogue = null;

        // ============================================================
        //  DOM REFS
        // ============================================================
        const sceneEl = document.getElementById('scene');
        const sceneBg = document.getElementById('scene-bg');
        const sceneLabel = document.getElementById('scene-label');
        const speakerEl = document.getElementById('dialogue-speaker');
        const textEl = document.getElementById('dialogue-text');
        const choicesEl = document.getElementById('dialogue-choices');
        const inventoryItems = document.getElementById('inventory-items');
        const clueCount = document.getElementById('clue-count');
        const accuseBtn = document.getElementById('accuse-btn');
        const modalOverlay = document.getElementById('modal-overlay');
        const modalSuspectList = document.getElementById('modal-suspect-list');
        const modalResult = document.getElementById('modal-result');
        const modalClose = document.getElementById('modal-close');
        const resetBtn = document.getElementById('reset-btn');

        // ============================================================
        //  TOAST
        // ============================================================
        let toastTimeout = null;

        function showToast(msg) {
            const old = document.querySelector('.toast');
            if (old) old.remove();
            if (toastTimeout) clearTimeout(toastTimeout);

            const div = document.createElement('div');
            div.className = 'toast';
            div.textContent = msg;
            sceneEl.appendChild(div);

            toastTimeout = setTimeout(() => {
                div.style.opacity = '0';
                div.style.transition = 'opacity 0.4s';
                setTimeout(() => div.remove(), 500);
                toastTimeout = null;
            }, 2800);
        }

        // ============================================================
        //  RENDER SCENE
        // ============================================================
        function renderScene() {
            const loc = DATA.locations[DATA.location];
            if (!loc) return;

            document.querySelectorAll('.scene-object').forEach(el => el.remove());
            sceneBg.style.background = loc.bg;
            sceneLabel.textContent = loc.label || loc.name || '';

            loc.objects.forEach(obj => {
                const div = document.createElement('div');
                div.className = 'scene-object';

                if (obj.type === 'npc') div.classList.add('npc');
                else if (obj.type === 'door') div.classList.add('door');
                else if (obj.type === 'item') div.classList.add('item');

                div.style.left = obj.x + '%';
                div.style.top = obj.y + '%';
                div.style.transform = 'translate(-50%, -50%)';

                let html = `<span class="glow"></span>`;
                html += `<span class="icon">${obj.icon || '•'}</span>`;
                html += `<span class="label">${obj.label}</span>`;
                div.innerHTML = html;

                if (obj.type === 'item' && DATA.inventory.includes(obj.itemId)) {
                    div.classList.add('collected');
                }

                if (obj.type === 'npc' && DATA.talkedTo[obj.npcId]) {
                    const check = document.createElement('span');
                    check.className = 'check';
                    check.textContent = '✓';
                    div.appendChild(check);
                }

                div.dataset.type = obj.type;
                if (obj.type === 'item') div.dataset.itemId = obj.itemId;
                if (obj.type === 'npc') div.dataset.npcId = obj.npcId;
                if (obj.type === 'door') div.dataset.target = obj.target;

                div.addEventListener('click', onObjectClick);
                div.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    onObjectClick.call(div, e);
                });

                sceneEl.appendChild(div);
            });

            updateAccuseButton();
        }

        // ============================================================
        //  OBJECT CLICK
        // ============================================================
        function onObjectClick(e) {
            if (DATA.gameOver) return;
            const el = this;
            const type = el.dataset.type;

            if (type === 'item') {
                const itemId = el.dataset.itemId;
                if (DATA.inventory.includes(itemId)) {
                    showToast('Anda sudah mengambil ini.');
                    return;
                }
                DATA.inventory.push(itemId);
                DATA.cluesCollected++;
                const item = DATA.items[itemId];
                showToast(item.name + ' ditemukan.');
                updateUI();
                renderScene();
                setDialogue('Zzy', 'Saya menemukan ' + item.name + '. ' + item.desc, []);

                if (DATA.cluesCollected >= DATA.totalClues) {
                    setTimeout(() => {
                        showToast('Semua bukti terkumpul! Saatnya menuduh pelaku.');
                        setDialogue('Zzy', 'Saya punya cukup bukti. Saatnya menangkap pembunuhnya.', []);
                        updateAccuseButton();
                    }, 600);
                }
            } else if (type === 'npc') {
                const npcId = el.dataset.npcId;
                startDialogue(npcId, 'initial');
            } else if (type === 'door') {
                const target = el.dataset.target;
                if (target && DATA.locations[target]) {
                    DATA.location = target;
                    renderScene();
                    setDialogue('', 'Anda memasuki ' + DATA.locations[target].name, []);
                    modalOverlay.classList.remove('show');
                }
            }
        }

        // ============================================================
        //  DIALOGUE
        // ============================================================
        function setDialogue(speaker, text, options) {
            speakerEl.textContent = speaker || '';
            textEl.textContent = text || '';
            choicesEl.innerHTML = '';

            if (!options || options.length === 0) {
                const btn = document.createElement('button');
                btn.className = 'choice-btn primary';
                btn.textContent = 'Tutup';
                btn.addEventListener('click', () => {
                    speakerEl.textContent = '';
                    textEl.textContent = 'Ketuk objek untuk interaksi.';
                    choicesEl.innerHTML = '';
                });
                btn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    btn.click();
                });
                choicesEl.appendChild(btn);
            } else {
                options.forEach(opt => {
                    const btn = document.createElement('button');
                    btn.className = 'choice-btn';
                    btn.textContent = opt.label;
                    btn.addEventListener('click', () => {
                        if (opt.next && currentDialogue) {
                            const npcId = currentDialogue.npcId;
                            const nextNode = opt.next;
                            if (nextNode === 'end') {
                                DATA.gameOver = true;
                                setDialogue('', 'Kasus terpecahkan! Zzy berhasil menangkap pembunuh.', []);
                                showToast('KASUS TERPECAHKAN!');
                                updateAccuseButton();
                                return;
                            }
                            startDialogue(npcId, nextNode);
                        }
                    });
                    btn.addEventListener('touchend', (e) => {
                        e.preventDefault();
                        btn.click();
                    });
                    choicesEl.appendChild(btn);
                });
            }
        }

        function startDialogue(npcId, nodeId) {
            const tree = DATA.dialogues[npcId];
            if (!tree) return;
            const node = tree[nodeId];
            if (!node) return;

            DATA.talkedTo[npcId] = true;
            currentDialogue = { npcId, nodeId };

            if (node.onEnter) {
                node.onEnter();
                updateUI();
                renderScene();
            }

            const speaker = node.speaker || '';
            const text = node.text || '';
            const options = node.options || [];
            setDialogue(speaker, text, options);

            if (options.length === 0) {
                setTimeout(() => {
                    if (choicesEl.children.length === 0) {
                        setDialogue('', 'Ketuk objek untuk interaksi.', []);
                    }
                }, 3000);
            }
        }

        // ============================================================
        //  UI UPDATE
        // ============================================================
        function updateUI() {
            inventoryItems.innerHTML = '';
            if (DATA.inventory.length === 0) {
                inventoryItems.innerHTML = '<span id="inventory-empty">(kosong)</span>';
            } else {
                DATA.inventory.forEach(id => {
                    const item = DATA.items[id];
                    if (!item) return;
                    const div = document.createElement('div');
                    div.className = 'inv-item';
                    div.textContent = item.name;
                    inventoryItems.appendChild(div);
                });
            }
            clueCount.textContent = DATA.cluesCollected;
            updateAccuseButton();
        }

        function updateAccuseButton() {
            if (DATA.gameOver) {
                accuseBtn.classList.remove('show');
                return;
            }
            if (DATA.cluesCollected >= DATA.totalClues) {
                accuseBtn.classList.add('show');
            } else {
                accuseBtn.classList.remove('show');
            }
        }

        // ============================================================
        //  ACCUSE MODAL
        // ============================================================
        function openAccuseModal() {
            if (DATA.gameOver) return;
            if (DATA.cluesCollected < DATA.totalClues) {
                showToast('Kumpulkan semua bukti terlebih dahulu!');
                return;
            }
            modalOverlay.classList.add('show');
            modalResult.classList.remove('show');
            modalResult.innerHTML = '';

            const list = document.getElementById('modal-suspect-list');
            list.innerHTML = '';
            DATA.suspects.forEach(suspect => {
                const div = document.createElement('div');
                div.className = 'suspect-option';
                div.innerHTML = `
                    <span>${suspect.name}</span>
                    <span class="badge">Pilih</span>
                `;
                div.addEventListener('click', () => {
                    makeAccusation(suspect.id);
                });
                div.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    makeAccusation(suspect.id);
                });
                list.appendChild(div);
            });
        }

        function makeAccusation(suspectId) {
            const result = document.getElementById('modal-result');
            if (suspectId === DATA.killerId) {
                result.innerHTML = `
                    <div class="win">BENAR</div>
                    <p style="margin-top:8px;">${DATA.suspects.find(s=>s.id===suspectId).name} adalah pembunuhnya.</p>
                    <p style="font-size:14px;color:#8a9aaa;margin-top:6px;">Motif: ${DATA.suspects.find(s=>s.id===suspectId).desc}</p>
                    <p style="font-size:14px;color:#b89a6a;margin-top:10px;">Zzy berhasil memecahkan kasus.</p>
                `;
                result.className = 'show win';
                DATA.gameOver = true;
                showToast('KASUS TERPECAHKAN!');
                updateAccuseButton();
                document.querySelectorAll('.suspect-option').forEach(el => el.style.pointerEvents = 'none');
            } else {
                const suspect = DATA.suspects.find(s => s.id === suspectId);
                result.innerHTML = `
                    <div class="lose">SALAH</div>
                    <p style="margin-top:8px;">${suspect.name} bukan pembunuhnya.</p>
                    <p style="font-size:14px;color:#8a9aaa;margin-top:6px;">Periksa kembali bukti dan coba lagi.</p>
                `;
                result.className = 'show lose';
            }
        }

        modalClose.addEventListener('click', () => {
            modalOverlay.classList.remove('show');
        });
        modalClose.addEventListener('touchend', (e) => {
            e.preventDefault();
            modalOverlay.classList.remove('show');
        });

        accuseBtn.addEventListener('click', openAccuseModal);
        accuseBtn.addEventListener('touchend', (e) => {
            e.preventDefault();
            openAccuseModal();
        });

        // ============================================================
        //  RESET
        // ============================================================
        function resetGame() {
            DATA.location = 'livingroom';
            DATA.inventory = [];
            DATA.cluesCollected = 0;
            DATA.talkedTo = {};
            DATA.gameOver = false;
            currentDialogue = null;
            modalOverlay.classList.remove('show');
            document.querySelectorAll('.toast').forEach(el => el.remove());
            if (toastTimeout) {
                clearTimeout(toastTimeout);
                toastTimeout = null;
            }
            renderScene();
            updateUI();
            setDialogue('Zzy', 'Kasus dimulai ulang. Cari bukti dan wawancarai tersangka.', []);
            showToast('Game direset.');
        }

        resetBtn.addEventListener('click', resetGame);
        resetBtn.addEventListener('touchend', (e) => {
            e.preventDefault();
            resetGame();
        });

        // ============================================================
        //  INIT
        // ============================================================
        function init() {
            renderScene();
            updateUI();
            setDialogue('Zzy', 'Selamat datang. Ada pembunuhan yang harus dipecahkan. Cari bukti dan wawancarai tersangka.', []);
            setTimeout(() => {
                showToast('Ketuk objek untuk interaksi.');
            }, 800);
        }

        init();

        console.log('Zzy - Detektif Pembunuhan');
        console.log('Kasus: Pembunuhan Mr. Blackwood');
        console.log('Tersangka: Mrs. Blackwood, Mr. Green, Ms. White');
        console.log('Bukti: Surat Ancaman, Pisau Berdarah, Surat Wasiat, Rekaman Telepon');
        console.log('Pembunuh: Mrs. Blackwood');
    </script>
</body>
</html>
