# memoramaanimalesingles
Actividad 1: Memorama Objetivo: Asociar imagen con palabra escrita en inglés
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arrastrar y Soltar Animales</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8; /* Fondo azul gris claro */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .game-container {
            background-color: #ffffff;
            border-radius: 1.5rem; /* Esquinas redondeadas */
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            padding: 2.5rem;
            max-width: 1000px; /* Ancho máximo del contenedor */
            width: 100%;
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            align-items: center;
        }
        h1 {
            font-size: 2.5rem;
            font-weight: extrabold;
            color: #1e3a8a; /* Azul oscuro */
            margin-bottom: 1.5rem;
            text-align: center;
        }
        .words-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 1rem;
            padding: 1.5rem;
            background-color: #e0f2fe; /* Azul muy claro */
            border-radius: 1rem;
            width: 100%;
            min-height: 80px; /* Altura mínima para palabras */
        }
        .word-card {
            background-color: #60a5fa; /* Azul para la tarjeta de palabra */
            color: #ffffff;
            padding: 0.75rem 1.5rem;
            border-radius: 0.75rem;
            font-size: 1.25rem;
            font-weight: bold;
            cursor: grab;
            touch-action: none; /* Previene el scroll/zoom en móviles */
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
            display: flex; /* Para alinear el emoji y el texto */
            align-items: center;
            justify-content: center; /* Centrar el emoji */
            gap: 0.5rem; /* Espacio entre emoji y texto */
        }
        .word-card .emoji {
            font-size: 2.5em; /* Tamaño del emoji más grande */
            line-height: 1; /* Asegura que el emoji no afecte la altura de la línea */
        }
        .word-card:active {
            cursor: grabbing;
            transform: scale(1.05);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }
        .image-drop-zones {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); /* Responsive grid */
            gap: 1.5rem;
            width: 100%;
            justify-content: center;
            padding: 1.5rem;
            background-color: #ffffff;
            border-radius: 1rem;
        }
        .drop-zone {
            background-color: #f8fafc; /* Gris muy claro para la zona de caída */
            border: 2px dashed #94a3b8; /* Borde punteado */
            border-radius: 1rem;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 1rem;
            min-height: 180px;
            text-align: center;
            position: relative;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            transition: border-color 0.3s ease-in-out, background-color 0.3s ease-in-out;
        }
        .drop-zone img {
            max-width: 90%;
            max-height: 120px;
            object-fit: contain;
            border-radius: 0.75rem;
            margin-bottom: 0.5rem;
        }
        .drop-zone.hover {
            border-color: #3b82f6; /* Azul al pasar el ratón */
            background-color: #eff6ff; /* Azul claro al pasar el ratón */
        }
        .drop-zone.correct {
            border-color: #22c55e; /* Verde para correcto */
            background-color: #ecfdf5; /* Verde claro para correcto */
        }
        .drop-zone.incorrect {
            border-color: #ef4444; /* Rojo para incorrecto */
            background-color: #fef2f2; /* Rojo claro para incorrecto */
        }
        .drop-zone .matched-word {
            font-size: 1.1rem;
            font-weight: bold;
            color: #1e40af; /* Azul oscuro para la palabra emparejada */
            margin-top: 0.5rem;
            display: flex; /* Para alinear el emoji y el texto */
            align-items: center;
            justify-content: center; /* Centrar el emoji */
            gap: 0.5rem; /* Espacio entre emoji y texto */
        }
        .drop-zone .matched-word .emoji {
            font-size: 2.5em; /* Tamaño del emoji más grande */
            line-height: 1;
        }

        .message-box {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 20px 40px;
            border-radius: 10px;
            font-size: 1.8rem;
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease-in-out, visibility 0.3s ease-in-out;
        }
        .message-box.show {
            opacity: 1;
            visibility: visible;
        }

        .restart-button {
            px-6 py-3 bg-gradient-to-r from-green-500 to-green-700 text-white font-bold rounded-full shadow-lg hover:from-green-600 hover:to-green-800 transition-all duration-300 transform hover:scale-105;
        }

        /* Responsive adjustments */
        @media (max-width: 768px) {
            .game-container {
                padding: 1.5rem;
                gap: 1rem;
            }
            h1 {
                font-size: 2rem;
            }
            .word-card {
                padding: 0.6rem 1.2rem;
                font-size: 1.1rem;
            }
            .word-card .emoji {
                font-size: 2em;
            }
            .image-drop-zones {
                grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
                gap: 1rem;
                padding: 1rem;
            }
            .drop-zone {
                min-height: 150px;
                padding: 0.8rem;
            }
            .drop-zone img {
                max-height: 100px;
            }
            .drop-zone .matched-word {
                font-size: 1rem;
            }
            .drop-zone .matched-word .emoji {
                font-size: 2em;
            }
            .message-box {
                font-size: 1.5rem;
                padding: 15px 30px;
            }
        }

        @media (max-width: 480px) {
            .game-container {
                padding: 1rem;
                gap: 0.75rem;
            }
            h1 {
                font-size: 1.75rem;
            }
            .word-card {
                padding: 0.5rem 1rem;
                font-size: 1rem;
            }
            .word-card .emoji {
                font-size: 1.8em;
            }
            .image-drop-zones {
                grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
                gap: 0.75rem;
                padding: 0.75rem;
            }
            .drop-zone {
                min-height: 130px;
                padding: 0.6rem;
            }
            .drop-zone img {
                max-height: 80px;
            }
            .drop-zone .matched-word {
                font-size: 0.9rem;
            }
            .drop-zone .matched-word .emoji {
                font-size: 1.8em;
            }
            .message-box {
                font-size: 1.2rem;
                padding: 10px 20px;
            }
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen p-4 sm:p-6 lg:p-8">

    <div class="game-container">
        <h1>Arrastra y Suelta: Animales</h1>

        <div id="wordsContainer" class="words-container">
            <!-- Word cards will be dynamically inserted here -->
        </div>

        <div id="imageDropZones" class="image-drop-zones">
            <!-- Image drop zones will be dynamically inserted here -->
        </div>

        <button id="restartButton" class="px-6 py-3 bg-gradient-to-r from-green-500 to-green-700 text-white font-bold rounded-full shadow-lg hover:from-green-600 hover:to-green-800 transition-all duration-300 transform hover:scale-105">
            Reiniciar Juego
        </button>

        <div id="messageBox" class="message-box"></div>
    </div>

    <script type="module">
        // Import necessary Firebase modules
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Firebase initialization variables
        let app;
        let db;
        let auth;
        let userId = 'anonymous'; // Default to anonymous
        let isAuthReady = false; // To track Firebase auth readiness

        // Game elements
        const wordsContainer = document.getElementById('wordsContainer');
        const imageDropZones = document.getElementById('imageDropZones');
        const restartButton = document.getElementById('restartButton');
        const messageBox = document.getElementById('messageBox');

        // Game state variables
        let draggedWordData = null; // Stores data of the word being dragged
        let matchedPairsCount = 0;
        let totalPairs = 0;

        // Define the animal data for the game, including emojis and using placeholders for images
        // Please replace these placeholder image URLs with the actual images from your Genially/Canva asset if possible.
        const animals = [
            { name: "Mouse", emoji: "🐭", image: "https://placehold.co/150x150/FFD700/000000?text=Mouse" },
            { name: "Monkey", emoji: "🐒", image: "https://placehold.co/150x150/FF6347/000000?text=Monkey" },
            { name: "Unicorn", emoji: "🦄", image: "https://placehold.co/150x150/9370DB/000000?text=Unicorn" },
            { name: "Worm", emoji: "🐛", image: "https://placehold.co/150x150/7CFC00/000000?text=Worm" },
            { name: "Alligator", emoji: "🐊", image: "https://placehold.co/150x150/3CB371/000000?text=Alligator" },
            { name: "Spider", emoji: "🕷️", image: "https://placehold.co/150x150/800000/FFFFFF?text=Spider" },
            { name: "Otter", emoji: "🦦", image: "https://placehold.co/150x150/DAA520/000000?text=Otter" },
            { name: "Bee", emoji: "🐝", image: "https://placehold.co/150x150/FFD700/000000?text=Bee" },
            { name: "Lizard", emoji: "🦎", image: "https://placehold.co/150x150/6B8E23/000000?text=Lizard" },
            { name: "Owl", emoji: "🦉", image: "https://placehold.co/150x150/A9A9A9/000000?text=Owl" },
            { name: "Ant", emoji: "🐜", image: "https://placehold.co/150x150/CD853F/FFFFFF?text=Ant" }
        ];

        /**
         * Initializes Firebase and authenticates the user.
         */
        const initializeFirebase = async () => {
            try {
                const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
                const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};

                app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);

                let authAttempted = false;
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    try {
                        await signInWithCustomToken(auth, __initial_auth_token);
                        console.log("Sesión iniciada con token personalizado.");
                        authAttempted = true;
                    } catch (authError) {
                        console.warn("Fallo al iniciar sesión con token personalizado (auth/invalid-claims u otro error), intentando sesión anónima:", authError);
                    }
                }

                if (!authAttempted || !auth.currentUser) { // Si el token personalizado falló o no se intentó
                    await signInAnonymously(auth);
                    console.log("Sesión iniciada de forma anónima.");
                }

                onAuthStateChanged(auth, (user) => {
                    if (user) {
                        userId = user.uid;
                        isAuthReady = true;
                        console.log("Firebase inicializado y usuario autenticado:", userId);
                    } else {
                        userId = 'anonymous_' + crypto.randomUUID(); // Fallback para usuario anónimo
                        isAuthReady = true;
                        console.log("Firebase inicializado: usuario anónimo.");
                    }
                    startGame(); // Iniciar el juego una vez que Firebase esté listo
                });
            } catch (error) {
                console.error("Error al inicializar la aplicación Firebase:", error);
                isAuthReady = true;
                startGame(); // Intentar iniciar el juego incluso si la inicialización de Firebase falla
            }
        };

        /**
         * Shuffles an array randomly using the Fisher-Yates algorithm.
         * @param {Array} array - The array to shuffle.
         * @returns {Array} The shuffled array.
         */
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        /**
         * Displays a message box with given text.
         * @param {string} message - The message to display.
         * @param {number} duration - Duration in milliseconds.
         * @param {string} type - 'correct' or 'incorrect' for styling.
         */
        function showMessageBox(message, duration = 1500, type = '') {
            messageBox.textContent = message;
            messageBox.className = 'message-box show'; // Reset classes
            if (type === 'correct') {
                messageBox.style.backgroundColor = 'rgba(34, 197, 94, 0.8)'; // Green
            } else if (type === 'incorrect') {
                messageBox.style.backgroundColor = 'rgba(239, 68, 68, 0.8)'; // Red
            } else {
                messageBox.style.backgroundColor = 'rgba(0, 0, 0, 0.7)'; // Default black
            }

            setTimeout(() => {
                messageBox.classList.remove('show');
            }, duration);
        }

        /**
         * Creates and populates the word cards and image drop zones.
         */
        function createGameElements() {
            wordsContainer.innerHTML = '';
            imageDropZones.innerHTML = '';
            matchedPairsCount = 0;
            totalPairs = animals.length;

            const shuffledWords = shuffleArray([...animals.map(a => ({ name: a.name, emoji: a.emoji }))]); // Shuffle words with emojis
            const shuffledAnimals = shuffleArray([...animals]); // Shuffle images

            // Create draggable word cards
            shuffledWords.forEach(wordData => {
                const wordCard = document.createElement('div');
                wordCard.classList.add('word-card');

                const emojiSpan = document.createElement('span');
                emojiSpan.classList.add('emoji');
                emojiSpan.textContent = wordData.emoji;
                wordCard.appendChild(emojiSpan);

                // No longer appending the textSpan to the wordCard
                // const textSpan = document.createElement('span');
                // textSpan.textContent = wordData.name;
                // wordCard.appendChild(textSpan);

                wordCard.setAttribute('draggable', 'true');
                wordCard.dataset.name = wordData.name; // Store the animal name (for matching logic)
                wordCard.id = `word-${wordData.name}`; // Unique ID for touch handling

                // Drag event listeners for desktop
                wordCard.addEventListener('dragstart', (e) => {
                    draggedWordData = { name: wordData.name, element: wordCard };
                    e.dataTransfer.setData('text/plain', wordData.name); // For dataTransfer (optional, but good practice)
                    // Add a class for visual feedback during drag
                    setTimeout(() => wordCard.classList.add('opacity-50'), 0);
                });
                wordCard.addEventListener('dragend', () => {
                    wordCard.classList.remove('opacity-50');
                    draggedWordData = null;
                });

                // Touch event listeners for mobile
                let currentTouchCard = null;
                let initialTouchX, initialTouchY;

                wordCard.addEventListener('touchstart', (e) => {
                    e.preventDefault(); // Prevent scrolling
                    currentTouchCard = wordCard;
                    draggedWordData = { name: wordData.name, element: wordCard };
                    const touch = e.touches[0];
                    initialTouchX = touch.clientX - wordCard.getBoundingClientRect().left;
                    initialTouchY = touch.clientY - wordCard.getBoundingClientRect().top;

                    // Make the card float
                    wordCard.style.position = 'absolute';
                    wordCard.style.zIndex = '1000';
                    wordCard.style.pointerEvents = 'none'; // So we can "see through" it for drop target
                    updateTouchCardPosition(touch.clientX, touch.clientY);
                });

                wordCard.addEventListener('touchmove', (e) => {
                    e.preventDefault();
                    if (currentTouchCard) {
                        const touch = e.touches[0];
                        updateTouchCardPosition(touch.clientX, touch.clientY);
                    }
                });

                wordCard.addEventListener('touchend', (e) => {
                    if (currentTouchCard) {
                        currentTouchCard.style.position = '';
                        currentTouchCard.style.zIndex = '';
                        currentTouchCard.style.pointerEvents = '';
                        currentTouchCard = null;

                        const touch = e.changedTouches[0];
                        handleTouchDrop(touch.clientX, touch.clientY);
                    }
                });

                function updateTouchCardPosition(clientX, clientY) {
                    if (currentTouchCard) {
                        currentTouchCard.style.left = (clientX - initialTouchX) + 'px';
                        currentTouchCard.style.top = (clientY - initialTouchY) + 'px';
                    }
                }

                wordsContainer.appendChild(wordCard);
            });

            // Create image drop zones
            shuffledAnimals.forEach(animal => {
                const dropZone = document.createElement('div');
                dropZone.classList.add('drop-zone');
                dropZone.dataset.name = animal.name; // Store the correct animal name for matching
                dropZone.dataset.emoji = animal.emoji; // Store the emoji for display on match

                const img = document.createElement('img');
                img.src = animal.image;
                img.alt = animal.name;
                dropZone.appendChild(img);

                const placeholderText = document.createElement('div');
                placeholderText.textContent = "Arrastra aquí";
                placeholderText.classList.add('text-gray-500', 'text-sm');
                dropZone.appendChild(placeholderText);

                // Drag event listeners for desktop
                dropZone.addEventListener('dragover', (e) => {
                    e.preventDefault(); // Allow drop
                    dropZone.classList.add('hover');
                });
                dropZone.addEventListener('dragleave', () => {
                    dropZone.classList.remove('hover');
                });
                dropZone.addEventListener('drop', (e) => {
                    e.preventDefault();
                    dropZone.classList.remove('hover');
                    handleDrop(draggedWordData, dropZone);
                });

                imageDropZones.appendChild(dropZone);
            });
        }

        /**
         * Handles the drop logic for both desktop drag and touch end.
         * @param {Object} draggedData - The data of the word being dragged ({ name, element }).
         * @param {HTMLElement} dropZoneElement - The HTML element of the drop zone.
         */
        function handleDrop(draggedData, dropZoneElement) {
            if (!draggedData) return; // No word was being dragged

            const wordName = draggedData.name;
            const dropZoneName = dropZoneElement.dataset.name;
            const dropZoneEmoji = dropZoneElement.dataset.emoji; // Get emoji from drop zone

            if (wordName === dropZoneName) {
                // Correct match
                dropZoneElement.classList.remove('incorrect');
                dropZoneElement.classList.add('correct');
                showMessageBox("¡Bien hecho!", 1500, 'correct');

                // Create the matched word display with ONLY emoji
                const matchedWordDisplay = document.createElement('div');
                matchedWordDisplay.classList.add('matched-word');

                const emojiSpan = document.createElement('span');
                emojiSpan.classList.add('emoji');
                emojiSpan.textContent = dropZoneEmoji;
                matchedWordDisplay.appendChild(emojiSpan);

                // No longer appending the textSpan to the matched word display
                // const textSpan = document.createElement('span');
                // textSpan.textContent = wordName;
                // matchedWordDisplay.appendChild(textSpan);

                // Clear previous placeholder
                dropZoneElement.querySelector('.text-gray-500').remove();
                dropZoneElement.appendChild(matchedWordDisplay);
                dropZoneElement.style.borderStyle = 'solid'; // Change to solid border on match
                dropZoneElement.style.borderColor = '#22c55e'; // Green solid border

                // Remove draggable word card from its original place
                draggedData.element.remove();
                matchedPairsCount++;

                // Save successful match to Firestore
                if (isAuthReady && db) {
                    saveGameData('match_correct', {
                        word: wordName,
                        image: dropZoneName,
                        timestamp: new Date().toISOString(),
                        userId: userId
                    });
                }

                if (matchedPairsCount === totalPairs) {
                    showMessageBox("¡Felicidades! Has completado todos los pares.", 3000, 'correct');
                    if (isAuthReady && db) {
                        saveGameData('game_completed', {
                            timestamp: new Date().toISOString(),
                            userId: userId,
                            totalPairs: totalPairs
                        });
                    }
                }
            } else {
                // Incorrect match
                dropZoneElement.classList.add('incorrect');
                showMessageBox("Inténtalo otra vez", 1500, 'incorrect');

                // Flash red and then revert
                setTimeout(() => {
                    dropZoneElement.classList.remove('incorrect');
                }, 700);

                // Save incorrect match to Firestore
                if (isAuthReady && db) {
                    saveGameData('match_incorrect', {
                        draggedWord: wordName,
                        targetImage: dropZoneName,
                        timestamp: new Date().toISOString(),
                        userId: userId
                    });
                }
            }
        }

        /**
         * Handles touch-based drops by checking element overlap.
         * @param {number} clientX - X coordinate of the touch end.
         * @param {number} clientY - Y coordinate of the touch end.
         */
        function handleTouchDrop(clientX, clientY) {
            const dropZones = Array.from(imageDropZones.children);
            let droppedOnZone = null;

            for (const dropZone of dropZones) {
                const rect = dropZone.getBoundingClientRect();
                if (
                    clientX >= rect.left &&
                    clientX <= rect.right &&
                    clientY >= rect.top &&
                    clientY <= rect.bottom &&
                    !dropZone.querySelector('.matched-word') // Only allow drops on unmatched zones
                ) {
                    droppedOnZone = dropZone;
                    break;
                }
            }

            if (droppedOnZone && draggedWordData) {
                handleDrop(draggedWordData, droppedOnZone);
            } else if (draggedWordData && draggedWordData.element) {
                // If not dropped on a valid zone, return the word card to its original position
                // This requires storing original position or re-appending to wordsContainer
                // For simplicity, we'll just put it back to wordsContainer
                wordsContainer.appendChild(draggedWordData.element);
            }
            draggedWordData = null; // Reset after drop attempt
        }

        /**
         * Saves game data to Firestore.
         * @param {string} eventType - The type of game event (e.g., 'match_correct', 'game_completed').
         * @param {Object} data - The data associated with the event.
         */
        async function saveGameData(eventType, data) {
            if (!isAuthReady || !db) {
                console.warn("Firestore no está listo. No se pueden guardar datos.");
                return;
            }
            try {
                const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
                const gameStatsCollectionRef = collection(db, `artifacts/${appId}/public/data/drag_and_drop_game_stats`);
                await addDoc(gameStatsCollectionRef, {
                    userId: userId,
                    eventType: eventType,
                    ...data,
                    timestamp: new Date().toISOString() // Ensure timestamp is always present
                });
                console.log("Datos del juego guardados:", eventType, data);
            } catch (error) {
                console.error("Error al guardar datos del juego en Firestore:", error);
            }
        }

        /**
         * Starts a new game.
         */
        function startGame() {
            createGameElements();
        }

        // Event listener for restart button
        restartButton.addEventListener('click', startGame);

        // Initialize Firebase when the window loads
        window.onload = initializeFirebase;
    </script>
</body>
</html>
Elaborado por: Camila Bermudez
