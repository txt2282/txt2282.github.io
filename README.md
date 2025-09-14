<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Staff Team Tracker</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700;800&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc;
            color: #1f2937;
            padding: 2rem;
        }
        .container {
            max-width: 1000px;
            margin: 0 auto;
            background-color: #ffffff;
            border-radius: 1.5rem;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
            padding: 2.5rem;
        }
        [contenteditable]:focus {
            outline: 2px solid #3b82f6;
            outline-offset: 2px;
            border-radius: 0.5rem;
        }
        .status-badge {
            display: inline-block;
            padding: 0.35rem 1rem;
            border-radius: 9999px;
            font-weight: 700;
            font-size: 0.8rem;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .status-active {
            background-color: #d1fae5;
            color: #059669;
        }
        .status-warning {
            background-color: #ffedd5;
            color: #f97316;
        }
        .status-demoted {
            background-color: #fee2e2;
            color: #ef4444;
        }
        .header-bg {
            background: linear-gradient(45deg, #1d4ed8, #0ea5e9);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .action-button {
            transition: all 0.2s ease-in-out;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }
        .action-button:hover {
            transform: scale(1.05);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .table-row {
            transition: background-color 0.2s ease-in-out;
        }
        .table-row:hover {
            background-color: #f3f4f6;
        }
        .table-cell-border {
            border-right: 1px solid #e2e8f0;
        }
        .dropdown {
            position: relative;
            display: inline-block;
        }
        .dropdown-content {
            display: none;
            position: absolute;
            background-color: #f9f9f9;
            min-width: 160px;
            box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
            z-index: 1;
            border-radius: 0.5rem;
            top: 25px;
            left: 5px;
        }
        .dropdown-content a {
            color: black;
            padding: 12px 16px;
            text-decoration: none;
            display: block;
            cursor: pointer;
            font-size: 0.875rem;
        }
        .dropdown-content a:hover {
            background-color: #f1f1f1;
        }
        .dropdown:hover .dropdown-content {
            display: block;
        }
    </style>
</head>
<body>

<div id="main-app" class="container">
    <h1 class="text-5xl font-extrabold text-center mb-2 header-bg underline">Splash Staff Warning System</h1>
    <p class="text-center text-gray-500 mb-8 font-semibold italic text-lg">
        Keep track of your staff's strike count.
    </p>

    <div class="flex flex-col sm:flex-row justify-between items-center mb-6 space-y-4 sm:space-y-0">
        <div id="user-info" class="flex items-center space-x-2 text-gray-500 font-semibold text-sm">
            <span>UserID:</span>
            <span id="user-id">Loading...</span>
        </div>
        <div class="flex items-center space-x-4">
            <button id="add-member-button" class="action-button px-6 py-3 bg-green-500 text-white font-semibold rounded-full shadow-lg hover:bg-green-600 flex items-center hidden">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4" />
                </svg>
                Add Member
            </button>
            <button id="admin-login-button" class="action-button px-6 py-3 bg-purple-500 text-white font-semibold rounded-full shadow-lg hover:bg-purple-600">
                Admin Login
            </button>
            <button id="preview-mode-button" class="action-button px-6 py-3 bg-gray-300 text-gray-800 font-semibold rounded-full shadow-lg hover:bg-gray-400 hidden">
                Preview Mode
            </button>
            <button id="filter-demoted-button" class="action-button px-6 py-3 bg-gray-300 text-gray-800 font-semibold rounded-full shadow-lg hover:bg-gray-400">
                Show Demoted Staff Only
            </button>
            <button id="admin-panel-button" class="action-button px-6 py-3 bg-purple-500 text-white font-semibold rounded-full shadow-lg hover:bg-purple-600 hidden">
                Admin Panel
            </button>
            <button id="bin-button" class="action-button px-6 py-3 bg-gray-600 text-white font-semibold rounded-full shadow-lg hover:bg-gray-700 hidden">
                Bin
            </button>
        </div>
    </div>

    <!-- Admin Panel -->
    <div id="admin-panel" class="hidden my-8 p-6 bg-gray-100 rounded-xl shadow-inner">
        <h2 class="text-2xl font-bold text-gray-700 mb-4">Admin Notes</h2>
        
        <div class="mb-6 p-4 bg-gray-200 rounded-lg">
            <h3 class="text-lg font-semibold text-gray-700 mb-2">Leave a New Note</h3>
            <input type="text" id="admin-name-input" class="w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 transition-shadow mb-2" placeholder="Your Name">
            <textarea id="admin-note-input" class="w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 transition-shadow" rows="3" placeholder="Enter your note here..."></textarea>
            <button id="save-note-button" class="action-button mt-3 px-4 py-2 bg-blue-500 text-white font-semibold rounded-lg hover:bg-blue-600">
                Save Note
            </button>
        </div>

        <h3 class="text-xl font-bold text-gray-700 mt-6 mb-2">Past Notes</h3>
        <div id="notes-list" class="space-y-4">
            <!-- Notes will be dynamically added here -->
        </div>

        <h3 class="text-xl font-bold text-gray-700 mt-6 mb-2">Staff Overview</h3>
        <div class="overflow-x-auto rounded-lg shadow-md bg-white">
            <table class="w-full text-left">
                <thead class="bg-gray-200">
                    <tr>
                        <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider rounded-tl-lg">Name</th>
                        <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider rounded-tr-lg">Strikes</th>
                    </tr>
                </thead>
                <tbody id="admin-staff-list">
                    <!-- Staff members for admin panel will be dynamically added here -->
                </tbody>
            </table>
        </div>
        
        <div class="flex justify-between mt-4">
            <button id="back-button" class="action-button px-6 py-3 bg-gray-300 text-gray-800 font-semibold rounded-full shadow-lg hover:bg-gray-400">
                Back to Staff List
            </button>
        </div>
    </div>
    
    <!-- Bin Panel -->
    <div id="bin-panel" class="hidden my-8 p-6 bg-gray-100 rounded-xl shadow-inner">
        <h2 class="text-2xl font-bold text-gray-700 mb-4">Bin</h2>
        <div class="overflow-x-auto rounded-lg shadow-md bg-white">
            <table class="w-full text-left">
                <thead class="bg-gray-200">
                    <tr>
                        <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider rounded-tl-lg">Name</th>
                        <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider">Strikes</th>
                        <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider">Reasons</th>
                        <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider rounded-tr-lg">Actions</th>
                    </tr>
                </thead>
                <tbody id="bin-list">
                    <!-- Bin staff will be dynamically added here -->
                </tbody>
            </table>
        </div>
        <div class="flex justify-start mt-4">
            <button id="back-from-bin-button" class="action-button px-6 py-3 bg-gray-300 text-gray-800 font-semibold rounded-full shadow-lg hover:bg-gray-400">
                Back to Staff List
            </button>
        </div>
    </div>
    
    <!-- Custom PIN Prompt Modal -->
    <div id="pin-modal" class="fixed inset-0 bg-gray-900 bg-opacity-50 flex items-center justify-center hidden">
        <div class="bg-white p-8 rounded-xl shadow-2xl max-w-sm w-full mx-4 transform transition-transform duration-300">
            <h3 class="text-2xl font-bold mb-4 text-center">Enter Admin PIN</h3>
            <input type="password" id="pin-input" class="w-full p-3 border border-gray-300 rounded-lg text-center text-lg focus:outline-none focus:ring-2 focus:ring-purple-500 transition-shadow" placeholder="••••">
            <p id="pin-error" class="text-red-500 text-sm mt-2 text-center hidden">Incorrect PIN. Try again.</p>
            <div class="mt-6 flex justify-between">
                <button id="submit-pin-button" class="w-1/2 px-4 py-2 bg-purple-500 text-white font-semibold rounded-lg hover:bg-purple-600 transition-colors">
                    Submit
                </button>
            </div>
        </div>
    </div>
    
    <!-- Staff List Table -->
    <div id="staff-list-container" class="overflow-x-auto rounded-2xl shadow-lg">
        <p id="loading-message" class="text-center py-8 text-gray-500 text-lg">Connecting to database...</p>
        <p id="empty-message" class="text-center py-8 text-gray-500 text-lg hidden">No staff members found. Add some to get started!</p>
        <table class="w-full text-left hidden">
            <thead class="bg-gray-100">
                <tr>
                    <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider rounded-tl-2xl table-cell-border"></th>
                    <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider table-cell-border">Name</th>
                    <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider table-cell-border">Strikes</th>
                    <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider table-cell-border">Status</th>
                    <th id="actions-header" class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider table-cell-border">Actions</th>
                    <th class="p-4 font-bold text-sm text-gray-700 uppercase tracking-wider rounded-tr-2xl">Reasons (Strike 1, 2, 3)</th>
                </tr>
            </thead>
            <tbody id="staff-list">
                <!-- Staff members will be dynamically added here -->
            </tbody>
        </table>
    </div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
    import { getFirestore, collection, onSnapshot, query, addDoc, doc, setDoc, deleteDoc, getDocs, orderBy, writeBatch, updateDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
    import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
    
    setLogLevel('Debug');
    
    const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
    const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
    const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

    let app, auth, db, userId;
    let staffData = [];
    let binData = [];
    let adminNotes = [];
    let isReadOnly = true;
    let demotedFilterActive = false;

    // Hardcoded initial staff data
    const initialStaffData = [
        { name: "TxT", strikes: 0, reasons: ["", "", ""] },
        { name: "mike_3757", strikes: 0, reasons: ["", "", ""] },
        { name: "arif_ali0747_42070", strikes: 0, reasons: ["", "", ""] },
        { name: "v3locity", strikes: 0, reasons: ["", "", ""] },
        { name: "realnotflexi", strikes: 0, reasons: ["", "", ""] },
        { name: "_ahmed.abid_", strikes: 0, reasons: ["", "", ""] },
        { name: "sunnygaming0000", strikes: 0, reasons: ["", "", ""] },
        { name: "mr_games", strikes: 0, reasons: ["", "", ""] },
        { name: "uzair.abdullah", strikes: 0, reasons: ["", "", ""] },
        { name: "_yasirkhan_", strikes: 0, reasons: ["", "", ""] }
    ];

    // HTML Elements
    const userIdEl = document.getElementById('user-id');
    const staffListEl = document.getElementById('staff-list');
    const addMemberButton = document.getElementById('add-member-button');
    const filterDemotedButton = document.getElementById('filter-demoted-button');
    const adminPanelButton = document.getElementById('admin-panel-button');
    const binButton = document.getElementById('bin-button');
    const adminLoginButton = document.getElementById('admin-login-button');
    const previewModeButton = document.getElementById('preview-mode-button');
    const adminPanelEl = document.getElementById('admin-panel');
    const binPanelEl = document.getElementById('bin-panel');
    const backButton = document.getElementById('back-button');
    const backFromBinButton = document.getElementById('back-from-bin-button');
    const adminStaffListEl = document.getElementById('admin-staff-list');
    const binListEl = document.getElementById('bin-list');
    const staffListContainerEl = document.getElementById('staff-list-container');
    const actionsHeaderEl = document.getElementById('actions-header');
    const adminNameInput = document.getElementById('admin-name-input');
    const adminNoteInput = document.getElementById('admin-note-input');
    const saveNoteButton = document.getElementById('save-note-button');
    const notesListEl = document.getElementById('notes-list');
    const pinModalEl = document.getElementById('pin-modal');
    const pinInputEl = document.getElementById('pin-input');
    const pinErrorEl = document.getElementById('pin-error');
    const submitPinButton = document.getElementById('submit-pin-button');
    const loadingMessageEl = document.getElementById('loading-message');
    const emptyMessageEl = document.getElementById('empty-message');
    const staffTableEl = staffListContainerEl.querySelector('table');
    
    window.onload = () => {
        initFirebase();
    };

    function initFirebase() {
        if (!firebaseConfig || Object.keys(firebaseConfig).length === 0) {
            loadingMessageEl.textContent = 'Firebase configuration missing.';
            return;
        }

        try {
            app = initializeApp(firebaseConfig);
            auth = getAuth(app);
            db = getFirestore(app);

            onAuthStateChanged(auth, async (user) => {
                if (user) {
                    userId = user.uid;
                    userIdEl.textContent = userId;
                    // Start listening to data after user is authenticated
                    listenToData();
                } else {
                    try {
                        if (initialAuthToken) {
                            await signInWithCustomToken(auth, initialAuthToken);
                        } else {
                            await signInAnonymously(auth);
                        }
                    } catch (error) {
                        console.error("Authentication failed:", error);
                        loadingMessageEl.textContent = 'Authentication failed. Please try again later.';
                    }
                }
            });
        } catch (error) {
            console.error("Firebase initialization failed:", error);
            loadingMessageEl.textContent = 'Failed to initialize Firebase.';
        }
    }

    function listenToData() {
        const staffColRef = collection(db, 'artifacts', appId, 'users', userId, 'staff');
        const binColRef = collection(db, 'artifacts', appId, 'users', userId, 'bin');
        const notesColRef = collection(db, 'artifacts', appId, 'users', userId, 'notes');

        // Staff data listener
        onSnapshot(staffColRef, async (snapshot) => {
            if (snapshot.empty) {
                // If this is the first time, populate with initial data
                if (staffData.length === 0) {
                    const batch = writeBatch(db);
                    initialStaffData.forEach(member => {
                        const newDocRef = doc(staffColRef);
                        batch.set(newDocRef, member);
                    });
                    try {
                        await batch.commit();
                        console.log("Initial data populated successfully.");
                    } catch (error) {
                        console.error("Error populating initial data: ", error);
                    }
                }
            } else {
                staffData = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                renderStaffList();
                renderAdminStaffList();
            }
            loadingMessageEl.classList.add('hidden');
            staffListContainerEl.classList.remove('hidden');
        }, (error) => {
            console.error("Error fetching staff data: ", error);
            loadingMessageEl.textContent = 'Error fetching staff data.';
        });

        // Bin data listener
        onSnapshot(binColRef, (snapshot) => {
            binData = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
            renderBinList();
        }, (error) => {
            console.error("Error fetching bin data: ", error);
        });

        // Admin notes listener
        onSnapshot(query(notesColRef, orderBy('timestamp', 'desc')), (snapshot) => {
            adminNotes = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
            renderAdminNotes();
        }, (error) => {
            console.error("Error fetching admin notes: ", error);
        });

        switchToPreviewMode();
    }
    
    function renderStaffList() {
        staffListEl.innerHTML = '';
        
        if (isReadOnly) {
            actionsHeaderEl.classList.add('hidden');
        } else {
            actionsHeaderEl.classList.remove('hidden');
        }

        let displayData = staffData;
        if (demotedFilterActive) {
            displayData = staffData.filter(member => member.strikes >= 3);
        }

        if (displayData.length === 0) {
            emptyMessageEl.classList.remove('hidden');
            staffTableEl.classList.add('hidden');
        } else {
            emptyMessageEl.classList.add('hidden');
            staffTableEl.classList.remove('hidden');
        }

        displayData.forEach(member => {
            const isDemoted = member.strikes >= 3;
            const isWarning = member.strikes === 1 || member.strikes === 2;
            
            let statusText = 'Active';
            let statusClass = 'status-active';

            if (isDemoted) {
                statusText = 'Demoted';
                statusClass = 'status-demoted';
            } else if (isWarning) {
                statusText = 'Risk Warning';
                statusClass = 'status-warning';
            }
            
            const row = document.createElement('tr');
            row.classList.add('table-row');
            row.dataset.id = member.id;
            
            row.innerHTML = `
                <td class="p-4 border-b border-gray-200 table-cell-border">
                    <div class="dropdown ${isReadOnly ? 'hidden' : ''}">
                        <button class="px-2 py-1 text-gray-500 hover:text-gray-700 focus:outline-none">
                            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M10 6a2 2 0 110-4 2 2 0 010 4zm0 8a2 2 0 110-4 2 2 0 010 4zm-2 2a2 2 0 104 0 2 2 0 00-4 0z"></path></svg>
                        </button>
                        <div class="dropdown-content">
                            <a data-action="move-to-bin" data-id="${member.id}">Move to Bin</a>
                        </div>
                    </div>
                </td>
                <td class="p-4 border-b border-gray-200 table-cell-border">
                    <span contenteditable="${!isReadOnly}" class="name-editable block cursor-pointer hover:underline" data-id="${member.id}">${member.name}</span>
                </td>
                <td class="p-4 border-b border-gray-200 table-cell-border">${member.strikes}</td>
                <td class="p-4 border-b border-gray-200 table-cell-border">
                    <span class="status-badge ${statusClass}">${statusText}</span>
                </td>
                <td class="p-4 border-b border-gray-200 table-cell-border ${isReadOnly ? 'hidden' : ''}">
                    <button data-id="${member.id}" data-action="add" class="action-button px-3 py-1 text-xs font-semibold rounded-full bg-blue-500 text-white hover:bg-blue-600 transition-colors mr-2">
                        Add Strike
                    </button>
                    <button data-id="${member.id}" data-action="remove" class="action-button px-3 py-1 text-xs font-semibold rounded-full bg-gray-300 text-gray-800 hover:bg-gray-400 transition-colors">
                        Remove Strike
                    </button>
                </td>
                <td class="p-4 border-b border-gray-200 text-sm italic">
                    <div class="flex flex-col space-y-2">
                        <textarea class="strike-reason w-full p-1 border rounded" rows="1" data-id="${member.id}" data-strike-index="0" placeholder="Reason for Strike 1" ${member.strikes < 1 || isReadOnly ? 'disabled' : ''}>${member.reasons[0]}</textarea>
                        <textarea class="strike-reason w-full p-1 border rounded" rows="1" data-id="${member.id}" data-strike-index="1" placeholder="Reason for Strike 2" ${member.strikes < 2 || isReadOnly ? 'disabled' : ''}>${member.reasons[1]}</textarea>
                        <textarea class="strike-reason w-full p-1 border rounded" rows="1" data-id="${member.id}" data-strike-index="2" placeholder="Reason for Strike 3" ${member.strikes < 3 || isReadOnly ? 'disabled' : ''}>${member.reasons[2]}</textarea>
                    </div>
                </td>
            `;
            staffListEl.appendChild(row);
        });
    }
    
    function renderAdminStaffList() {
        adminStaffListEl.innerHTML = '';
        staffData.forEach(member => {
            const row = document.createElement('tr');
            row.classList.add('hover:bg-gray-100');
            row.innerHTML = `
                <td class="p-4 border-b border-gray-200">${member.name}</td>
                <td class="p-4 border-b border-gray-200">${member.strikes}</td>
            `;
            adminStaffListEl.appendChild(row);
        });
    }

    function renderBinList() {
        binListEl.innerHTML = '';
        binData.forEach(member => {
            const row = document.createElement('tr');
            row.classList.add('hover:bg-gray-100');
            row.innerHTML = `
                <td class="p-4 border-b border-gray-200">${member.name}</td>
                <td class="p-4 border-b border-gray-200">${member.strikes}</td>
                <td class="p-4 border-b border-gray-200">
                    <ul class="list-disc list-inside space-y-1">
                        ${member.reasons.map(reason => reason ? `<li>${reason}</li>` : '').join('')}
                    </ul>
                </td>
                <td class="p-4 border-b border-gray-200">
                    <button data-id="${member.id}" data-action="recover" class="action-button px-3 py-1 text-xs font-semibold rounded-full bg-green-500 text-white hover:bg-green-600 transition-colors">
                        Recover
                    </button>
                </td>
            `;
            binListEl.appendChild(row);
        });
    }

    function renderAdminNotes() {
        notesListEl.innerHTML = '';
        adminNotes.forEach(note => {
            const date = note.timestamp ? new Date(note.timestamp).toLocaleString() : 'N/A';
            const noteEl = document.createElement('div');
            noteEl.classList.add('p-4', 'bg-white', 'rounded-lg', 'shadow-sm');
            noteEl.innerHTML = `
                <p class="text-sm text-gray-400 italic mb-1">${note.name} - ${date}</p>
                <p class="text-gray-800">${note.note}</p>
            `;
            notesListEl.appendChild(noteEl);
        });
    }

    async function addStrike(id) {
        if (isReadOnly) return;
        const memberRef = doc(db, 'artifacts', appId, 'users', userId, 'staff', id);
        try {
            const member = staffData.find(m => m.id === id);
            if (member && member.strikes < 3) {
                await updateDoc(memberRef, { strikes: member.strikes + 1 });
            }
        } catch (e) {
            console.error("Error adding strike: ", e);
        }
    }

    async function removeStrike(id) {
        if (isReadOnly) return;
        const memberRef = doc(db, 'artifacts', appId, 'users', userId, 'staff', id);
        try {
            const member = staffData.find(m => m.id === id);
            if (member && member.strikes > 0) {
                const updatedReasons = [...member.reasons];
                updatedReasons[member.strikes - 1] = "";
                await updateDoc(memberRef, { strikes: member.strikes - 1, reasons: updatedReasons });
            }
        } catch (e) {
            console.error("Error removing strike: ", e);
        }
    }

    async function moveToBin(id) {
        if (isReadOnly) return;
        try {
            const memberToMove = staffData.find(m => m.id === id);
            if (!memberToMove) return;

            const batch = writeBatch(db);
            const staffRef = doc(db, 'artifacts', appId, 'users', userId, 'staff', id);
            const binRef = doc(db, 'artifacts', appId, 'users', userId, 'bin', id);

            batch.set(binRef, { name: memberToMove.name, strikes: memberToMove.strikes, reasons: memberToMove.reasons });
            batch.delete(staffRef);
            await batch.commit();
        } catch (e) {
            console.error("Error moving to bin: ", e);
        }
    }

    async function recoverFromBin(id) {
        try {
            const memberToRecover = binData.find(m => m.id === id);
            if (!memberToRecover) return;

            const batch = writeBatch(db);
            const binRef = doc(db, 'artifacts', appId, 'users', userId, 'bin', id);
            const staffRef = doc(db, 'artifacts', appId, 'users', userId, 'staff', id);

            batch.set(staffRef, { name: memberToRecover.name, strikes: memberToRecover.strikes, reasons: memberToRecover.reasons });
            batch.delete(binRef);
            await batch.commit();
        } catch (e) {
            console.error("Error recovering from bin: ", e);
        }
    }
    
    function handlePinSubmit() {
        if (pinInputEl.value === "2282") {
            pinModalEl.classList.add('hidden');
            pinInputEl.value = "";
            pinErrorEl.classList.add('hidden');
            switchToAdminMode();
        } else {
            pinErrorEl.classList.remove('hidden');
        }
    }

    addMemberButton.addEventListener('click', async () => {
        try {
            await addDoc(collection(db, 'artifacts', appId, 'users', userId, 'staff'), {
                name: `New Member`,
                strikes: 0,
                reasons: ["", "", ""]
            });
        } catch (e) {
            console.error("Error adding document: ", e);
        }
    });

    adminLoginButton.addEventListener('click', () => {
        pinModalEl.classList.remove('hidden');
    });

    previewModeButton.addEventListener('click', () => {
        switchToPreviewMode();
    });

    function switchToPreviewMode() {
        isReadOnly = true;
        addMemberButton.classList.add('hidden');
        adminPanelButton.classList.add('hidden');
        binButton.classList.add('hidden');
        adminLoginButton.classList.remove('hidden');
        previewModeButton.classList.add('hidden');
        filterDemotedButton.classList.remove('hidden');

        adminPanelEl.classList.add('hidden');
        binPanelEl.classList.add('hidden');
        staffListContainerEl.classList.remove('hidden');
        
        loadingMessageEl.classList.add('hidden');
        emptyMessageEl.classList.add('hidden');
        staffTableEl.classList.remove('hidden');
        
        renderStaffList();
    }
    
    function switchToAdminMode() {
        isReadOnly = false;
        adminLoginButton.classList.add('hidden');
        addMemberButton.classList.remove('hidden');
        adminPanelButton.classList.remove('hidden');
        binButton.classList.remove('hidden');
        previewModeButton.classList.remove('hidden');
        filterDemotedButton.classList.remove('hidden');

        adminPanelEl.classList.add('hidden');
        binPanelEl.classList.add('hidden');
        staffListContainerEl.classList.remove('hidden');
        
        loadingMessageEl.classList.add('hidden');
        emptyMessageEl.classList.add('hidden');
        staffTableEl.classList.remove('hidden');

        renderStaffList();
    }

    filterDemotedButton.addEventListener('click', () => {
        demotedFilterActive = !demotedFilterActive;
        filterDemotedButton.textContent = demotedFilterActive ? "Show All Staff" : "Show Demoted Staff Only";
        filterDemotedButton.classList.toggle('bg-gray-300', !demotedFilterActive);
        filterDemotedButton.classList.toggle('bg-red-500', demotedFilterActive);
        filterDemotedButton.classList.toggle('text-gray-800', !demotedFilterActive);
        filterDemotedButton.classList.toggle('text-white', demotedFilterActive);
        renderStaffList();
    });

    adminPanelButton.addEventListener('click', () => {
        adminPanelEl.classList.remove('hidden');
        binPanelEl.classList.add('hidden');
        staffListContainerEl.classList.add('hidden');
        renderAdminStaffList();
    });

    binButton.addEventListener('click', () => {
        binPanelEl.classList.remove('hidden');
        adminPanelEl.classList.add('hidden');
        staffListContainerEl.classList.add('hidden');
        renderBinList();
    });

    submitPinButton.addEventListener('click', handlePinSubmit);

    // Enter key functionality for PIN input
    pinInputEl.addEventListener('keydown', (e) => {
        if (e.key === 'Enter') {
            handlePinSubmit();
        }
    });
    
    backButton.addEventListener('click', () => {
        adminPanelEl.classList.add('hidden');
        staffListContainerEl.classList.remove('hidden');
    });

    backFromBinButton.addEventListener('click', () => {
        binPanelEl.classList.add('hidden');
        staffListContainerEl.classList.remove('hidden');
    });
    
    saveNoteButton.addEventListener('click', async () => {
        const name = adminNameInput.value.trim();
        const note = adminNoteInput.value.trim();
        if (name && note) {
            try {
                await addDoc(collection(db, 'artifacts', appId, 'users', userId, 'notes'), {
                    name,
                    note,
                    timestamp: new Date().toISOString()
                });
                adminNameInput.value = '';
                adminNoteInput.value = '';
            } catch (e) {
                console.error("Error adding note: ", e);
            }
        }
    });

    staffListEl.addEventListener('blur', async (e) => {
        if (isReadOnly) return;
        const editableSpan = e.target.closest('.name-editable');
        if (editableSpan) {
            const id = editableSpan.dataset.id;
            try {
                const docRef = doc(db, 'artifacts', appId, 'users', userId, 'staff', id);
                await updateDoc(docRef, { name: editableSpan.textContent });
            } catch (e) {
                console.error("Error updating document: ", e);
            }
        }
    }, true);

    staffListEl.addEventListener('blur', async (e) => {
        if (isReadOnly) return;
        const textarea = e.target.closest('.strike-reason');
        if (textarea) {
            const id = textarea.dataset.id;
            const strikeIndex = parseInt(textarea.dataset.strikeIndex);
            try {
                const docRef = doc(db, 'artifacts', appId, 'users', userId, 'staff', id);
                const member = staffData.find(m => m.id === id);
                if (member) {
                    const updatedReasons = [...member.reasons];
                    updatedReasons[strikeIndex] = textarea.value;
                    await updateDoc(docRef, { reasons: updatedReasons });
                }
            } catch (e) {
                console.error("Error updating document: ", e);
            }
        }
    }, true);

    staffListEl.addEventListener('click', (e) => {
        const button = e.target.closest('button');
        const link = e.target.closest('a');
        if (isReadOnly && !button) return;
        
        if (link && link.dataset.action === 'move-to-bin') {
            const id = link.dataset.id;
            moveToBin(id);
        } else if (button) {
            const id = button.dataset.id;
            const action = button.dataset.action;
            if (action === 'add') {
                addStrike(id);
            } else if (action === 'remove') {
                removeStrike(id);
            }
        }
    });

    binListEl.addEventListener('click', (e) => {
        const button = e.target.closest('button');
        if (!button || button.dataset.action !== 'recover') return;
        const id = button.dataset.id;
        recoverFromBin(id);
    });
</script>

</body>
</html>
