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

<div class="container">
    <h1 class="text-5xl font-extrabold text-center mb-2 header-bg underline">Splash Staff Warning System</h1>
    <p class="text-center text-gray-500 mb-8 font-semibold italic text-lg">
        Keep track of your staff's strike count.
    </p>

    <div id="user-info" class="mb-4 text-center text-gray-600">
        Your User ID: <span id="user-id" class="font-mono text-sm bg-gray-200 px-2 py-1 rounded-md">Loading...</span>
    </div>

    <div id="header-actions" class="flex justify-between items-center mb-6">
        <div class="flex items-center space-x-4">
            <button id="add-member-button" class="action-button px-6 py-3 bg-green-500 text-white font-semibold rounded-full shadow-lg hover:bg-green-600 flex items-center hidden">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4" />
                </svg>
                Add Member
            </button>
            <button id="save-changes-button" class="action-button px-6 py-3 bg-blue-500 text-white font-semibold rounded-full shadow-lg hover:bg-blue-600 flex items-center hidden">
                Save Changes
            </button>
        </div>
        <div class="flex items-center space-x-4">
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
        <p id="loading-message" class="text-center py-8 text-gray-500 text-lg">Loading...</p>
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
    import { getFirestore, doc, setDoc, onSnapshot, collection, query, getDocs, writeBatch, addDoc, serverTimestamp, where, getDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
    import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
    setLogLevel('debug');

    // Global variables for Firebase
    let app, auth, db, userId, appId;
    let staffData = [];
    let binData = [];
    let nextId = 1;
    let demotedFilterActive = false;
    let adminNotes = [];
    let isReadOnly = true;

    // HTML Elements
    const staffListEl = document.getElementById('staff-list');
    const addMemberButton = document.getElementById('add-member-button');
    const saveChangesButton = document.getElementById('save-changes-button');
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
    const headerActionsEl = document.getElementById('header-actions');
    const actionsHeaderEl = document.getElementById('actions-header');
    
    const adminNameInput = document.getElementById('admin-name-input');
    const adminNoteInput = document.getElementById('admin-note-input');
    const saveNoteButton = document.getElementById('save-note-button');
    const notesListEl = document.getElementById('notes-list');

    // Modal elements
    const pinModalEl = document.getElementById('pin-modal');
    const pinInputEl = document.getElementById('pin-input');
    const pinErrorEl = document.getElementById('pin-error');
    const submitPinButton = document.getElementById('submit-pin-button');

    const loadingMessageEl = document.getElementById('loading-message');
    const emptyMessageEl = document.getElementById('empty-message');
    const staffTableEl = staffListContainerEl.querySelector('table');
    const userIdEl = document.getElementById('user-id');

    // Initialize Firebase
    function initializeFirebase() {
        try {
            const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
            appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
            app = initializeApp(firebaseConfig);
            auth = getAuth(app);
            db = getFirestore(app);
            
            // Sign in anonymously and wait for the auth state to be ready
            onAuthStateChanged(auth, async (user) => {
                if (user) {
                    userId = user.uid;
                    userIdEl.textContent = userId;
                    fetchData();
                    fetchBinData();
                    fetchAdminNotes();
                    switchToPreviewMode();
                } else {
                    if (typeof __initial_auth_token !== 'undefined') {
                        await signInWithCustomToken(auth, __initial_auth_token);
                    } else {
                        await signInAnonymously(auth);
                    }
                }
            });
        } catch (error) {
            console.error("Firebase initialization failed:", error);
            loadingMessageEl.textContent = "Error: Failed to initialize the app. Please check the console for details.";
        }
    }
    
    // Switch to read-only view
    function switchToPreviewMode() {
        isReadOnly = true;
        // Hide admin-specific buttons
        addMemberButton.classList.add('hidden');
        saveChangesButton.classList.add('hidden');
        adminPanelButton.classList.add('hidden');
        binButton.classList.add('hidden');

        // Show general buttons
        adminLoginButton.classList.remove('hidden');
        previewModeButton.classList.add('hidden');
        filterDemotedButton.classList.remove('hidden');

        // Show main list and hide other panels
        adminPanelEl.classList.add('hidden');
        binPanelEl.classList.add('hidden');
        staffListContainerEl.classList.remove('hidden');
        headerActionsEl.classList.remove('hidden');
        
        loadingMessageEl.classList.add('hidden');
        emptyMessageEl.classList.add('hidden');
        staffTableEl.classList.remove('hidden');

        renderStaffList();
    }
    
    // Switch to admin view
    function switchToAdminMode() {
        isReadOnly = false;
        // Hide general buttons
        adminLoginButton.classList.add('hidden');

        // Show admin-specific buttons
        addMemberButton.classList.remove('hidden');
        saveChangesButton.classList.remove('hidden');
        adminPanelButton.classList.remove('hidden');
        binButton.classList.remove('hidden');
        previewModeButton.classList.remove('hidden');
        filterDemotedButton.classList.remove('hidden');

        // Show main list and hide other panels
        adminPanelEl.classList.add('hidden');
        binPanelEl.classList.add('hidden');
        staffListContainerEl.classList.remove('hidden');
        headerActionsEl.classList.remove('hidden');
        
        loadingMessageEl.classList.add('hidden');
        emptyMessageEl.classList.add('hidden');
        staffTableEl.classList.remove('hidden');

        renderStaffList();
    }

    async function fetchData() {
        if (!db || !userId) {
            console.error("Firebase not initialized or user not authenticated.");
            loadingMessageEl.textContent = "Error: Authentication failed. Please try again.";
            return;
        }

        // Fetch staff data
        const staffCollectionRef = collection(db, `artifacts/${appId}/users/${userId}/staff_members`);
        const staffQuery = query(staffCollectionRef);
        onSnapshot(staffQuery, (snapshot) => {
            const tempStaffData = [];
            snapshot.forEach((doc) => {
                tempStaffData.push({ id: doc.id, ...doc.data() });
            });
            staffData = tempStaffData.sort((a, b) => {
                const aNum = parseInt(a.id);
                const bNum = parseInt(b.id);
                if (!isNaN(aNum) && !isNaN(bNum)) {
                    return aNum - bNum;
                }
                return a.id.localeCompare(b.id);
            });
            nextId = staffData.length > 0 ? Math.max(...staffData.map(m => parseInt(m.id) || 0)) + 1 : 1;
            renderStaffList();
            renderAdminStaffList();
        });
    }

    async function fetchBinData() {
        if (!db || !userId) return;
        const binCollectionRef = collection(db, `artifacts/${appId}/users/${userId}/bin`);
        const binQuery = query(binCollectionRef);
        onSnapshot(binQuery, (snapshot) => {
            const tempBinData = [];
            snapshot.forEach(doc => {
                tempBinData.push({ id: doc.id, ...doc.data() });
            });
            binData = tempBinData;
            renderBinList();
        });
    }
    
    async function fetchAdminNotes() {
        if (!db || !userId) return;
        const notesCollectionRef = collection(db, `artifacts/${appId}/users/${userId}/admin_notes`);
        const notesQuery = query(notesCollectionRef);
        onSnapshot(notesQuery, (snapshot) => {
            const tempNotes = [];
            snapshot.forEach(doc => {
                tempNotes.push({ id: doc.id, ...doc.data() });
            });
            adminNotes = tempNotes;
            renderAdminNotes();
        });
    }

    async function saveData() {
        if (!db || !userId) return;

        // Save staff data
        const batch = writeBatch(db);
        const staffCollectionRef = collection(db, `artifacts/${appId}/users/${userId}/staff_members`);
        
        // This method deletes all documents and recreates them, which is not ideal for large datasets
        // but works for this application. For a production app, you would only update changed documents.
        const existingDocs = await getDocs(staffCollectionRef);
        existingDocs.forEach(doc => {
            batch.delete(doc.ref);
        });

        staffData.forEach(member => {
            const docRef = doc(staffCollectionRef, member.id);
            batch.set(docRef, member);
        });
        await batch.commit();
    }

    function renderStaffList() {
        staffListEl.innerHTML = '';
        
        // Toggle actions column based on read-only mode
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
        const sortedNotes = adminNotes.sort((a, b) => b.timestamp - a.timestamp);
        
        sortedNotes.forEach(note => {
            const date = note.timestamp ? new Date(note.timestamp.seconds * 1000).toLocaleString() : 'N/A';
            const noteEl = document.createElement('div');
            noteEl.classList.add('p-4', 'bg-white', 'rounded-lg', 'shadow-sm');
            noteEl.innerHTML = `
                <p class="text-sm text-gray-400 italic mb-1">${note.name} - ${date}</p>
                <p class="text-gray-800">${note.note}</p>
            `;
            notesListEl.appendChild(noteEl);
        });
    }

    function addStrike(id) {
        if (isReadOnly) return;
        const member = staffData.find(m => m.id === id);
        if (member && member.strikes < 3) {
            member.strikes++;
        }
    }

    function removeStrike(id) {
        if (isReadOnly) return;
        const member = staffData.find(m => m.id === id);
        if (member && member.strikes > 0) {
            const lastStrikeIndex = member.strikes - 1;
            member.reasons[lastStrikeIndex] = ""; 
            member.strikes--;
        }
    }

    async function moveToBin(id) {
        if (isReadOnly) return;
        const memberIndex = staffData.findIndex(m => m.id === id);
        if (memberIndex > -1) {
            const memberToMove = staffData[memberIndex];
            staffData.splice(memberIndex, 1);
            
            const binDocRef = doc(db, `artifacts/${appId}/users/${userId}/bin/${id}`);
            await setDoc(binDocRef, memberToMove);
            saveData();
            renderStaffList();
        }
    }

    async function recoverFromBin(id) {
        const binDocRef = doc(db, `artifacts/${appId}/users/${userId}/bin/${id}`);
        const binDocSnap = await getDoc(binDocRef);

        if (binDocSnap.exists()) {
            const recoveredMember = binDocSnap.data();
            staffData.push(recoveredMember);

            await setDoc(doc(db, `artifacts/${appId}/users/${userId}/staff_members/${id}`), recoveredMember);
            await deleteDoc(binDocRef);
            saveData();
            renderStaffList();
        } else {
            console.error("No such document in bin!");
        }
    }

    addMemberButton.addEventListener('click', () => {
        staffData.push({
            id: (nextId++).toString(),
            name: `New Member`,
            strikes: 0,
            reasons: ["", "", ""]
        });
        renderStaffList();
    });

    saveChangesButton.addEventListener('click', () => {
        saveData();
    });

    adminLoginButton.addEventListener('click', () => {
        pinModalEl.classList.remove('hidden');
    });

    previewModeButton.addEventListener('click', () => {
        switchToPreviewMode();
    });

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
    });

    binButton.addEventListener('click', () => {
        binPanelEl.classList.remove('hidden');
        adminPanelEl.classList.add('hidden');
        staffListContainerEl.classList.add('hidden');
    });

    submitPinButton.addEventListener('click', () => {
        if (pinInputEl.value === "2282") {
            pinModalEl.classList.add('hidden');
            pinInputEl.value = "";
            pinErrorEl.classList.add('hidden');
            switchToAdminMode();
        } else {
            pinErrorEl.classList.remove('hidden');
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
                const notesCollectionRef = collection(db, `artifacts/${appId}/users/${userId}/admin_notes`);
                await addDoc(notesCollectionRef, {
                    name,
                    note,
                    timestamp: serverTimestamp()
                });
                adminNameInput.value = '';
                adminNoteInput.value = '';
            } catch (e) {
                console.error("Error adding document: ", e);
            }
        } else {
            console.error("Name and note cannot be empty.");
        }
    });

    // Event listener for blur event on editable name spans
    staffListEl.addEventListener('blur', (e) => {
        if (isReadOnly) return;
        const editableSpan = e.target.closest('.name-editable');
        if (editableSpan) {
            const id = editableSpan.dataset.id;
            const member = staffData.find(m => m.id === id);
            if (member) {
                member.name = editableSpan.textContent;
            }
        }
    }, true);

    // Event listener for blur event on textarea
    staffListEl.addEventListener('blur', (e) => {
        if (isReadOnly) return;
        const textarea = e.target.closest('.strike-reason');
        if (textarea) {
            const id = textarea.dataset.id;
            const strikeIndex = parseInt(textarea.dataset.strikeIndex);
            const member = staffData.find(m => m.id === id);
            if (member) {
                member.reasons[strikeIndex] = textarea.value;
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
                renderStaffList();
            } else if (action === 'remove') {
                removeStrike(id);
                renderStaffList();
            }
        }
    });

    binListEl.addEventListener('click', (e) => {
        const button = e.target.closest('button');
        if (!button || button.dataset.action !== 'recover') return;
        const id = button.dataset.id;
        recoverFromBin(id);
    });

    initializeFirebase();
</script>

</body>
</html>
