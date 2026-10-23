<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tareas Dipz</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        dipzBlue: '#1e88e5',
                        dipzGreen: '#26a69a',
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #f0f4f8 0%, #e2eafc 100%);
            min-height: 100vh;
        }
        .bg-gradient-dipz {
            background: linear-gradient(135deg, #1e88e5 0%, #26a69a 100%);
        }
        .text-gradient-dipz {
            background: linear-gradient(135deg, #1e88e5 0%, #26a69a 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .task-card {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .task-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.08), 0 4px 6px -2px rgba(0, 0, 0, 0.04);
        }
        .task-removing {
            opacity: 0;
            transform: translateX(50px) scale(0.95);
        }
        .task-adding {
            animation: fadeInSlide 0.3s ease-out forwards;
        }
        @keyframes fadeInSlide {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
    </style>
</head>
<body class="flex flex-col min-h-screen text-slate-800 antialiased">

    <header class="bg-white/80 backdrop-blur-md sticky top-0 z-50 border-b border-slate-200/80 shadow-sm">
        <div class="max-w-4xl mx-auto px-4 py-3 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="relative group">
                    <img src="90821.png" alt="Tareas Dipz Logo" class="w-12 h-12 rounded-2xl object-cover shadow-md border-2 border-white transition-transform group-hover:scale-105" onerror="this.onerror=null; this.src='https://placehold.co/100x100/1e88e5/ffffff?text=DIPZ';">
                </div>
                <div>
                    <h1 class="text-xl md:text-2xl font-bold tracking-tight text-gradient-dipz">TAREAS DIPZ</h1>
                    <p class="text-xs text-slate-500 font-medium">Gestión rápida y efectiva de actividades</p>
                </div>
            </div>
            
            <div id="statusBadge" class="flex items-center gap-2 px-3 py-1.5 rounded-full bg-slate-100 border border-slate-200 text-xs font-semibold text-slate-600">
                <span class="w-2.5 h-2.5 rounded-full bg-amber-500 animate-pulse" id="statusDot"></span>
                <span id="statusText">Conectando...</span>
            </div>
        </div>
    </header>

    <main class="flex-grow max-w-4xl w-full mx-auto px-4 py-8">

        <div class="bg-white rounded-2xl p-4 md:p-6 shadow-xl shadow-slate-200/50 border border-slate-100 mb-8">
            <form id="addTaskForm" class="flex flex-col sm:flex-row gap-3">
                <div class="relative flex-grow">
                    <div class="absolute inset-y-0 left-0 pl-3.5 flex items-center pointer-events-none text-slate-400">
                        <i class="fa-solid fa-list-check text-lg"></i>
                    </div>
                    <input type="text" id="taskInput" required placeholder="Escribe una nueva actividad..." 
                        class="w-full pl-11 pr-4 py-3.5 bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-dipzBlue focus:border-transparent text-slate-800 placeholder-slate-400 text-sm md:text-base transition-all">
                </div>
                <button type="submit" id="addButton" disabled
                    class="bg-gradient-dipz hover:opacity-95 text-white font-semibold px-6 py-3.5 rounded-xl shadow-md shadow-dipzBlue/20 hover:shadow-lg hover:shadow-dipzBlue/30 active:scale-95 transition-all flex items-center justify-center gap-2 text-sm md:text-base disabled:opacity-50 disabled:cursor-not-allowed">
                    <i class="fa-solid fa-plus text-base"></i>
                    <span>Agregar</span>
                </button>
            </form>
        </div>

        <div class="flex flex-wrap items-center justify-between gap-4 mb-6">
            <div class="flex items-center gap-4">
                <div class="bg-white px-4 py-2 rounded-xl border border-slate-200/70 shadow-sm flex items-center gap-2">
                    <span class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Pendientes:</span>
                    <span id="pendingCount" class="text-lg font-bold text-dipzBlue">0</span>
                </div>
                <div class="bg-white px-4 py-2 rounded-xl border border-slate-200/70 shadow-sm flex items-center gap-2">
                    <span class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Completadas hoy:</span>
                    <span id="completedCount" class="text-lg font-bold text-dipzGreen">0</span>
                </div>
            </div>

            <p class="text-xs text-slate-400 italic">
                <i class="fa-solid fa-bolt text-amber-400 mr-1"></i>
                Haz clic en la casilla para completar y eliminar
            </p>
        </div>

        <div id="dogWidget" class="bg-gradient-to-r from-teal-50 to-blue-50 border border-slate-200/80 rounded-2xl p-4 mb-8 shadow-sm transition-all hover:shadow-md cursor-pointer group relative overflow-hidden" title="Haz clic para ver la motivación del perrito">
            <div class="flex flex-col sm:flex-row items-center gap-4">
                <div class="relative flex-shrink-0">
                    <img src="109488.jpg" alt="Perrito Dipz" class="w-24 h-24 sm:w-28 sm:h-28 rounded-xl object-cover border-2 border-white shadow-md transition-transform group-hover:scale-105" onerror="this.onerror=null; this.src='https://placehold.co/200x200/26a69a/ffffff?text=Perrito';">
                    <span id="moodBadge" class="absolute -top-2 -right-2 bg-emerald-500 text-white text-[10px] font-extrabold px-2 py-0.5 rounded-full shadow-sm tracking-wider uppercase">FELIZ</span>
                </div>
                <div class="flex-grow text-center sm:text-left">
                    <div class="flex items-center justify-center sm:justify-start gap-2 mb-1">
                        <span class="text-xs font-bold uppercase tracking-wider text-slate-400">Compañero Dipz</span>
                        <span class="text-xs px-2 py-0.5 rounded-md bg-white text-slate-600 border border-slate-200 font-medium">Meme de estado</span>
                    </div>
                    <h3 id="dogStatusTitle" class="text-lg font-bold text-slate-800">COMO ME VEO</h3>
                    <p id="dogStatusSubtitle" class="text-sm font-semibold text-emerald-600 mt-0.5">FELIZ - Vas al día con tus actividades.</p>
                    <p id="dogMessage" class="text-xs text-slate-500 mt-1 italic">"Completa tus pendientes para mantener alegre a tu compañero."</p>
                </div>
                <div class="flex-shrink-0 hidden sm:block">
                    <span class="inline-flex items-center justify-center w-9 h-9 rounded-full bg-white shadow-sm border border-slate-200 text-slate-400 group-hover:text-dipzBlue transition-colors">
                        <i class="fa-solid fa-face-smile text-base"></i>
                    </span>
                </div>
            </div>
        </div>

        <div id="tasksContainer" class="space-y-3">
            <div id="loadingSkeleton" class="space-y-3">
                <div class="bg-white/60 animate-pulse h-16 rounded-2xl border border-slate-200/50"></div>
                <div class="bg-white/60 animate-pulse h-16 rounded-2xl border border-slate-200/50"></div>
                <div class="bg-white/60 animate-pulse h-16 rounded-2xl border border-slate-200/50"></div>
            </div>
            
            <div id="emptyState" class="hidden bg-white/60 backdrop-blur-sm rounded-2xl p-10 text-center border border-dashed border-slate-300">
                <div class="w-16 h-16 mx-auto mb-3 rounded-full bg-slate-100 flex items-center justify-center text-slate-400">
                    <i class="fa-regular fa-calendar-check text-2xl"></i>
                </div>
                <h3 class="text-base font-semibold text-slate-700">No tienes actividades pendientes</h3>
                <p class="text-xs text-slate-500 mt-1">¡Buen trabajo! Registra una nueva tarea arriba para comenzar.</p>
            </div>
        </div>

    </main>

    <footer class="mt-auto py-6 border-t border-slate-200/60 bg-white/40 text-center text-xs text-slate-500">
        <p>© 2026 Tareas Dipz • Desarrollado para ser simple, rápido e intuitivo</p>
    </footer>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, deleteDoc, doc, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const appId = typeof __app_id !== 'undefined' ? __app_id : 'tareas-dipz-app';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
            apiKey: "demo-key",
            authDomain: "demo.firebaseapp.com",
            projectId: "demo-project",
            storageBucket: "demo.appspot.com",
            messagingSenderId: "00000000000",
            appId: "1:00000000000:web:00000000000"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        let currentUser = null;
        let completedTodayCounter = 0;

        const statusBadge = document.getElementById('statusBadge');
        const statusDot = document.getElementById('statusDot');
        const statusText = document.getElementById('statusText');
        const addTaskForm = document.getElementById('addTaskForm');
        const taskInput = document.getElementById('taskInput');
        const addButton = document.getElementById('addButton');
        const tasksContainer = document.getElementById('tasksContainer');
        const loadingSkeleton = document.getElementById('loadingSkeleton');
        const emptyState = document.getElementById('emptyState');
        const pendingCount = document.getElementById('pendingCount');
        const completedCount = document.getElementById('completedCount');

        const dogWidget = document.getElementById('dogWidget');
        const moodBadge = document.getElementById('moodBadge');
        const dogStatusTitle = document.getElementById('dogStatusTitle');
        const dogStatusSubtitle = document.getElementById('dogStatusSubtitle');
        const dogMessage = document.getElementById('dogMessage');

        function updateDogMood(pendingCountValue) {
            if (!dogWidget) return;

            if (pendingCountValue === 0) {
                moodBadge.innerText = "FELIZ";
                moodBadge.className = "absolute -top-2 -right-2 bg-emerald-500 text-white text-[10px] font-extrabold px-2 py-0.5 rounded-full shadow-sm tracking-wider uppercase animate-bounce";
                dogStatusTitle.innerText = "COMO ME VEO: FELIZ";
                dogStatusSubtitle.innerText = "¡Todo completado y bajo control!";
                dogStatusSubtitle.className = "text-sm font-bold text-emerald-600 mt-0.5";
                dogMessage.innerText = '"¡Excelente trabajo! No tienes ninguna tarea pendiente acumulada."';
            } else if (pendingCountValue <= 3) {
                moodBadge.innerText = "TRANQUILO";
                moodBadge.className = "absolute -top-2 -right-2 bg-blue-500 text-white text-[10px] font-extrabold px-2 py-0.5 rounded-full shadow-sm tracking-wider uppercase";
                dogStatusTitle.innerText = "COMO ME VEO: TRANQUILO";
                dogStatusSubtitle.innerText = `Tienes solo ${pendingCountValue} tarea(s) pendiente(s).`;
                dogStatusSubtitle.className = "text-sm font-semibold text-blue-600 mt-0.5";
                dogMessage.innerText = '"¡Vamos bien! Complétalas para mantenerme 100% feliz."';
            } else {
                moodBadge.innerText = "ESTUVISTE LLORANDO";
                moodBadge.className = "absolute -top-2 -right-2 bg-amber-500 text-white text-[10px] font-extrabold px-2 py-0.5 rounded-full shadow-sm tracking-wider uppercase";
                dogStatusTitle.innerText = "COMO ME VEO: ESTUVISTE LLORANDO";
                dogStatusSubtitle.innerText = `¡Atención! Tienes ${pendingCountValue} tareas pendientes.`;
                dogStatusSubtitle.className = "text-sm font-bold text-amber-600 mt-0.5";
                dogMessage.innerText = '"¡Siento que hay mucho pendiente! Termina algunas tareas para ponernos alegres."';
            }
        }

        const motivationalPhrases = [
            '"¿Cómo me veo? ¡Listo para ayudarte a organizar el día!"',
            '"¿Estuviste llorando? ¡No te preocupes, juntos completamos todo!"',
            '"¡Cada tarea completada es una victoria feliz!"',
            '"Un paso a la vez, verás cómo eliminamos todas las actividades."'
        ];
        let phraseIndex = 0;

        if (dogWidget) {
            dogWidget.addEventListener('click', () => {
                phraseIndex = (phraseIndex + 1) % motivationalPhrases.length;
                dogMessage.innerText = motivationalPhrases[phraseIndex];
                dogWidget.classList.add('scale-[1.01]');
                setTimeout(() => dogWidget.classList.remove('scale-[1.01]'), 150);
            });
        }

        async function initAuth() {
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (err) {
                console.error("Error al autenticar:", err);
                statusText.innerText = "Error DB";
                statusDot.className = "w-2.5 h-2.5 rounded-full bg-red-500";
            }
        }

        onAuthStateChanged(auth, (user) => {
            if (user) {
                currentUser = user;
                statusText.innerText = "En línea";
                statusDot.className = "w-2.5 h-2.5 rounded-full bg-emerald-500";
                addButton.disabled = false;
                listenToTasks();
            } else {
                currentUser = null;
                statusText.innerText = "Desconectado";
                statusDot.className = "w-2.5 h-2.5 rounded-full bg-slate-400";
                addButton.disabled = true;
            }
        });

        function getTasksCollectionRef() {
            if (!currentUser) return null;
            return collection(db, 'artifacts', appId, 'users', currentUser.uid, 'tasks');
        }

        function listenToTasks() {
            const tasksRef = getTasksCollectionRef();
            if (!tasksRef) return;

            onSnapshot(tasksRef, (snapshot) => {
                loadingSkeleton.classList.add('hidden');
                
                const tasks = [];
                snapshot.forEach((docSnapshot) => {
                    tasks.push({
                        id: docSnapshot.id,
                        ...docSnapshot.data()
                    });
                });

                tasks.sort((a, b) => {
                    const timeA = a.createdAt?.toMillis() || Date.now();
                    const timeB = b.createdAt?.toMillis() || Date.now();
                    return timeB - timeA;
                });

                renderTasks(tasks);
            }, (error) => {
                console.error("Error al obtener tareas:", error);
                loadingSkeleton.classList.add('hidden');
            });
        }

        function renderTasks(tasks) {
            pendingCount.innerText = tasks.length;
            updateDogMood(tasks.length);

            if (tasks.length === 0) {
                tasksContainer.innerHTML = '';
                tasksContainer.appendChild(emptyState);
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            tasksContainer.innerHTML = '';

            tasks.forEach((task) => {
                const taskElement = createTaskCard(task);
                tasksContainer.appendChild(taskElement);
            });
        }

        function formatDate(timestamp) {
            if (!timestamp) return 'Justo ahora';
            const date = timestamp.toDate ? timestamp.toDate() : new Date(timestamp);
            return new Intl.DateTimeFormat('es-MX', {
                hour: '2-digit',
                minute: '2-digit',
                day: 'numeric',
                month: 'short'
            }).format(date);
        }

        function createTaskCard(task) {
            const card = document.createElement('div');
            card.id = `task-${task.id}`;
            card.className = "task-card task-adding bg-white rounded-2xl p-4 border border-slate-200/80 shadow-sm flex items-center justify-between gap-3 group";

            const formattedTime = formatDate(task.createdAt);

            card.innerHTML = `
                <div class="flex items-center gap-3.5 flex-grow min-w-0">
                    <button type="button" class="complete-btn flex-shrink-0 w-6 h-6 rounded-lg border-2 border-slate-300 hover:border-dipzGreen hover:bg-emerald-50 text-emerald-600 flex items-center justify-center transition-all group-hover:border-dipzGreen" title="Completar y Eliminar">
                        <i class="fa-solid fa-check text-xs opacity-0 hover:opacity-100 transition-opacity"></i>
                    </button>
                    <div class="flex flex-col min-w-0">
                        <span class="text-sm md:text-base text-slate-800 font-medium truncate select-none">${escapeHTML(task.title)}</span>
                        <span class="text-[11px] text-slate-400 font-normal">${formattedTime}</span>
                    </div>
                </div>
                <div class="flex items-center gap-2">
                    <button type="button" class="delete-btn opacity-60 group-hover:opacity-100 text-slate-400 hover:text-red-500 p-2 rounded-lg hover:bg-red-50 transition-all" title="Eliminar directamente">
                        <i class="fa-regular fa-trash-can text-sm"></i>
                    </button>
                </div>
            `;

            const completeBtn = card.querySelector('.complete-btn');
            completeBtn.addEventListener('click', () => {
                completeBtn.className = "flex-shrink-0 w-6 h-6 rounded-lg bg-dipzGreen text-white flex items-center justify-center transition-all scale-110";
                completeBtn.innerHTML = '<i class="fa-solid fa-check text-xs"></i>';
                
                completedTodayCounter++;
                completedCount.innerText = completedTodayCounter;

                removeTaskWithAnimation(task.id);
            });

            const deleteBtn = card.querySelector('.delete-btn');
            deleteBtn.addEventListener('click', () => {
                removeTaskWithAnimation(task.id);
            });

            return card;
        }

        async function removeTaskWithAnimation(taskId) {
            const card = document.getElementById(`task-${taskId}`);
            if (card) {
                card.classList.add('task-removing');
            }

            setTimeout(async () => {
                try {
                    if (!currentUser) return;
                    const taskDocRef = doc(db, 'artifacts', appId, 'users', currentUser.uid, 'tasks', taskId);
                    await deleteDoc(taskDocRef);
                } catch (error) {
                    console.error("Error al eliminar la actividad:", error);
                }
            }, 300);
        }

        function escapeHTML(str) {
            return str.replace(/[&<>'"]/g, 
                tag => ({
                    '&': '&amp;',
                    '<': '&lt;',
                    '>': '&gt;',
                    "'": '&#39;',
                    '"': '&quot;'
                }[tag] || tag)
            );
        }

        addTaskForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            const title = taskInput.value.trim();

            if (!title || !currentUser) return;

            addButton.disabled = true;

            try {
                const tasksRef = getTasksCollectionRef();
                await addDoc(tasksRef, {
                    title: title,
                    createdAt: serverTimestamp(),
                    completed: false
                });

                taskInput.value = '';
                taskInput.focus();
            } catch (error) {
                console.error("Error al guardar la tarea:", error);
            } finally {
                addButton.disabled = false;
            }
        });

        initAuth();
    </script>
</body>
</html>
