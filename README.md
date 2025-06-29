รับบันทึกหน้าเว็บ (html)
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบบันทึกรายการสั่งสินค้า</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- SweetAlert2 -->
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
    <!-- Google Fonts: Kanit -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Kanit', sans-serif;
        }
        /* Custom styles for active tab */
        .tab-active {
            background-color: #059669; /* emerald-600 */
            color: white;
            border-bottom-color: transparent;
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #a7f3d0;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #6ee7b7;
        }
        /* Style for status tags */
        .status-red {
            background-color: #fecaca;
            color: #b91c1c;
        }
        .status-green {
            background-color: #dcfce7;
            color: #166534;
        }
    </style>
</head>
<body class="bg-emerald-50 text-gray-800">

    <div class="container mx-auto p-4 md:p-8">
        <header class="text-center mb-8">
            <h1 class="text-4xl md:text-5xl font-bold text-emerald-700">📋 ระบบบันทึกรายการสั่งสินค้า</h1>
            <p class="text-emerald-600 mt-2">จัดการรายการสั่งซื้ออาหารและยาสัตว์อย่างเป็นระบบ</p>
        </header>

        <!-- Navigation Tabs -->
        <div class="mb-6 border-b border-emerald-200">
            <nav class="-mb-px flex flex-wrap gap-2" aria-label="Tabs">
                <button onclick="changeTab('dashboard')" class="tab-button tab-active text-lg px-6 py-3 font-medium rounded-t-lg border-b-2 border-transparent">📊 แดชบอร์ด</button>
                <button onclick="changeTab('orderFeed')" class="tab-button text-lg px-6 py-3 font-medium rounded-t-lg border-b-2 border-transparent">🍖 สั่งอาหารสัตว์</button>
                <button onclick="changeTab('orderMeds')" class="tab-button text-lg px-6 py-3 font-medium rounded-t-lg border-b-2 border-transparent">💊 สั่งยาสัตว์</button>
                <button onclick="changeTab('feedStatus')" class="tab-button text-lg px-6 py-3 font-medium rounded-t-lg border-b-2 border-transparent">📝 สถานะอาหารสัตว์</button>
                <button onclick="changeTab('medsStatus')" class="tab-button text-lg px-6 py-3 font-medium rounded-t-lg border-b-2 border-transparent">💉 สถานะยาสัตว์</button>
            </nav>
        </div>

        <!-- Content Area -->
        <main>
            <!-- Dashboard Section -->
            <div id="dashboard" class="tab-content space-y-8">
                 <div>
                    <h2 class="text-2xl font-semibold text-emerald-700 mb-4">📈 สรุปภาพรวม</h2>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="bg-white p-6 rounded-xl shadow-md border-l-4 border-emerald-500">
                            <h3 class="font-bold text-lg">จำนวนการสั่งอาหารสัตว์</h3>
                            <p id="feed-order-count" class="text-4xl font-bold text-emerald-600">0</p>
                        </div>
                        <div class="bg-white p-6 rounded-xl shadow-md border-l-4 border-teal-500">
                            <h3 class="font-bold text-lg">จำนวนการสั่งยาสัตว์</h3>
                            <p id="meds-order-count" class="text-4xl font-bold text-teal-600">0</p>
                        </div>
                    </div>
                </div>
                 <div>
                    <h2 class="text-2xl font-semibold text-emerald-700 mb-4">🍖 รายการสั่งอาหารสัตว์ล่าสุด (3 รายการ)</h2>
                    <div id="latest-feed-orders" class="space-y-3">
                        <p class="text-gray-500">กำลังโหลดข้อมูล...</p>
                    </div>
                </div>
                <div>
                    <h2 class="text-2xl font-semibold text-emerald-700 mb-4">💊 รายการสั่งยาสัตว์ล่าสุด (3 รายการ)</h2>
                    <div id="latest-meds-orders" class="space-y-3">
                       <p class="text-gray-500">กำลังโหลดข้อมูล...</p>
                    </div>
                </div>
            </div>

            <!-- Order Feed Section -->
            <div id="orderFeed" class="tab-content hidden">
                 <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border border-emerald-200">
                    <h2 class="text-2xl font-semibold text-emerald-700 mb-6">📝 บันทึกการสั่งอาหารสัตว์</h2>
                    <form id="feedForm">
                        <div class="space-y-4">
                            <div>
                                <label for="feed-datetime" class="block text-sm font-medium text-gray-700">วัน-เวลาที่สั่ง</label>
                                <input type="text" id="feed-datetime" name="datetime" class="mt-1 block w-full bg-gray-100 rounded-md border-gray-300 shadow-sm focus:border-emerald-500 focus:ring-emerald-500" readonly>
                            </div>
                            <div>
                                <label for="feed-recorder" class="block text-sm font-medium text-gray-700">ผู้บันทึกรายการ</label>
                                <select id="feed-recorder" name="recorder" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-emerald-500 focus:ring-emerald-500">
                                    <option>พี่หมอ</option>
                                    <option>พี่จุ๊</option>
                                    <option>พี่ยุ้ย</option>
                                    <option>พี่อัน</option>
                                </select>
                            </div>
                            <div>
                                <label for="feed-details" class="block text-sm font-medium text-gray-700">รายละเอียดการสั่ง</label>
                                <textarea id="feed-details" name="details" rows="4" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-emerald-500 focus:ring-emerald-500" placeholder="ระบุรายการอาหารสัตว์และจำนวนที่ต้องการสั่ง..." required></textarea>
                            </div>
                        </div>
                        <div class="mt-6">
                             <button type="submit" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3 px-4 rounded-lg shadow-md transition duration-300 ease-in-out transform hover:scale-105">
                                บันทึกข้อมูล
                            </button>
                        </div>
                    </form>
                </div>
            </div>

            <!-- Order Meds Section -->
            <div id="orderMeds" class="tab-content hidden">
                <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border border-teal-200">
                    <h2 class="text-2xl font-semibold text-teal-700 mb-6">📝 บันทึกการสั่งยาสัตว์</h2>
                    <form id="medsForm">
                         <div class="space-y-4">
                            <div>
                                <label for="meds-datetime" class="block text-sm font-medium text-gray-700">วัน-เวลาที่สั่ง</label>
                                <input type="text" id="meds-datetime" name="datetime" class="mt-1 block w-full bg-gray-100 rounded-md border-gray-300 shadow-sm focus:border-teal-500 focus:ring-teal-500" readonly>
                            </div>
                            <div>
                                <label for="meds-recorder" class="block text-sm font-medium text-gray-700">ผู้บันทึกรายการ</label>
                                <select id="meds-recorder" name="recorder" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-teal-500 focus:ring-teal-500">
                                    <option>พี่หมอ</option>
                                    <option>พี่จุ๊</option>
                                    <option>พี่ยุ้ย</option>
                                    <option>พี่อัน</option>
                                </select>
                            </div>
                            <div>
                                <label for="meds-details" class="block text-sm font-medium text-gray-700">รายละเอียดการสั่ง</label>
                                <textarea id="meds-details" name="details" rows="4" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-teal-500 focus:ring-teal-500" placeholder="ระบุรายการยาและจำนวนที่ต้องการสั่ง..." required></textarea>
                            </div>
                        </div>
                        <div class="mt-6">
                            <button type="submit" class="w-full bg-teal-600 hover:bg-teal-700 text-white font-bold py-3 px-4 rounded-lg shadow-md transition duration-300 ease-in-out transform hover:scale-105">
                                บันทึกข้อมูล
                            </button>
                        </div>
                    </form>
                </div>
            </div>

            <!-- Feed Status Section -->
            <div id="feedStatus" class="tab-content hidden">
                <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border border-emerald-200">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-2xl font-semibold text-emerald-700">สถานะการสั่งอาหารสัตว์</h2>
                        <button onclick="saveFeedStatusChanges()" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg shadow-md transition duration-300">
                            บันทึกการเปลี่ยนแปลง
                        </button>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="min-w-full divide-y divide-gray-200">
                            <thead class="bg-gray-50">
                                <tr>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">วัน-เวลา</th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">รายละเอียด</th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ผู้บันทึก</th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">สถานะ</th>
                                </tr>
                            </thead>
                            <tbody id="feed-status-table" class="bg-white divide-y divide-gray-200">
                                <tr><td colspan="4" class="text-center p-4 text-gray-500">กำลังโหลดข้อมูล...</td></tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
            
            <!-- Meds Status Section -->
            <div id="medsStatus" class="tab-content hidden">
                <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border border-teal-200">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-2xl font-semibold text-teal-700">สถานะการสั่งยาสัตว์</h2>
                        <button onclick="saveMedsStatusChanges()" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg shadow-md transition duration-300">
                            บันทึกการเปลี่ยนแปลง
                        </button>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="min-w-full divide-y divide-gray-200">
                            <thead class="bg-gray-50">
                                <tr>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">วัน-เวลา</th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">รายละเอียด</th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ผู้บันทึก</th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">สถานะ</th>
                                </tr>
                            </thead>
                            <tbody id="meds-status-table" class="bg-white divide-y divide-gray-200">
                                <tr><td colspan="4" class="text-center p-4 text-gray-500">กำลังโหลดข้อมูล...</td></tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <script>
        // --- CONFIGURATION ---
        const SCRIPT_URL = "https://script.google.com/macros/s/AKfycbz4_XsTZ7sMb7XfHhv-OQHobefyC7rFlD4MDZ5CFD0K2w1xyZOHRUE20ZGIEqNl_054/exec";

        // --- Utility Functions ---
        const getCurrentDateTime = () => {
            return new Date().toLocaleString('th-TH', { year: 'numeric', month: '2-digit', day: '2-digit', hour: '2-digit', minute: '2-digit', second: '2-digit' }).replace(',', '');
        };
        
        const setFormDateTime = () => {
            const nowStr = getCurrentDateTime();
            document.getElementById('feed-datetime').value = nowStr;
            document.getElementById('meds-datetime').value = nowStr;
        };

        // --- Tab Navigation ---
        const changeTab = (tabId) => {
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.add('hidden'));
            document.getElementById(tabId).classList.remove('hidden');

            document.querySelectorAll('.tab-button').forEach(button => button.classList.remove('tab-active'));
            document.querySelector(`button[onclick="changeTab('${tabId}')"]`).classList.add('tab-active');
            
            if (tabId === 'orderFeed' || tabId === 'orderMeds') {
                setFormDateTime();
            } else if (['dashboard', 'feedStatus', 'medsStatus'].includes(tabId)) {
                 jsonpRequest(`${SCRIPT_URL}?action=getData`, 'fetchData');
            }
        };
        
        // --- Generic JSONP Handler for fetching data ---
        const jsonpRequest = (url, callbackName) => {
            Swal.fire({
                title: 'กำลังโหลดข้อมูล...',
                text: 'กรุณารอสักครู่',
                allowOutsideClick: false,
                didOpen: () => { Swal.showLoading(); }
            });

            const script = document.createElement('script');
            script.src = `${url}&callback=${callbackName}`;
            document.body.appendChild(script);
            
            script.onload = () => document.body.removeChild(script);
            script.onerror = () => {
                 document.body.removeChild(script);
                 Swal.fire('เกิดข้อผิดพลาด', 'ไม่สามารถเชื่อมต่อกับเซิร์ฟเวอร์ได้', 'error');
            }
        };

        // --- Data Display Logic ---
        window.fetchData = (response) => {
            Swal.close();
            if (response.status !== "success") {
                return Swal.fire('เกิดข้อผิดพลาด', 'ไม่สามารถดึงข้อมูลได้: ' + response.message, 'error');
            }
            
            const { feedOrders, medOrders } = response.data;
            
            // Dashboard: Counts
            document.getElementById('feed-order-count').textContent = feedOrders.length;
            document.getElementById('meds-order-count').textContent = medOrders.length;

            // Dashboard: Latest Feeds
            const latestFeedContainer = document.getElementById('latest-feed-orders');
            latestFeedContainer.innerHTML = feedOrders.length > 0 ? feedOrders.slice(0, 3).map(order => 
                `<div class="bg-white p-4 rounded-lg shadow-sm"><p class="font-semibold">${order.details}</p><p class="text-sm text-gray-500">โดย: ${order.recorder} - ${order.datetime}</p></div>`
            ).join('') : '<p class="text-gray-500">ยังไม่มีข้อมูลการสั่งอาหารสัตว์</p>';

            // Dashboard: Latest Meds
            const latestMedsContainer = document.getElementById('latest-meds-orders');
            latestMedsContainer.innerHTML = medOrders.length > 0 ? medOrders.slice(0, 3).map(order =>
                `<div class="bg-white p-4 rounded-lg shadow-sm"><p class="font-semibold">${order.details}</p><p class="text-sm text-gray-500">โดย: ${order.recorder} - ${order.datetime}</p></div>`
            ).join('') : '<p class="text-gray-500">ยังไม่มีข้อมูลการสั่งยาสัตว์</p>';
            
            // Generic function to populate a status table
            const populateStatusTable = (tableBodyId, orders) => {
                const tableBody = document.getElementById(tableBodyId);
                if (orders.length > 0) {
                    tableBody.innerHTML = orders.map(order => {
                        const isReceived = order.status === 'รับสินค้าแล้ว';
                        const statusClass = isReceived ? 'status-green' : 'status-red';
                        return `
                            <tr data-id="${order.id}">
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">${order.datetime}</td>
                                <td class="px-6 py-4 whitespace-pre-wrap break-all text-sm text-gray-800">${order.details}</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">${order.recorder}</td>
                                <td class="px-6 py-4 whitespace-nowrap">
                                    <div class="flex items-center gap-4">
                                        <span class="px-3 py-1 inline-flex text-xs leading-5 font-semibold rounded-full ${statusClass}">${order.status}</span>
                                        <select class="status-selector rounded-md border-gray-300 text-sm shadow-sm focus:border-emerald-500 focus:ring-emerald-500">
                                            <option value="กำลังดำเนินการ" ${!isReceived ? 'selected' : ''}>กำลังดำเนินการ</option>
                                            <option value="รับสินค้าแล้ว" ${isReceived ? 'selected' : ''}>รับสินค้าแล้ว</option>
                                        </select>
                                    </div>
                                </td>
                            </tr>`;
                    }).join('');
                } else {
                    tableBody.innerHTML = '<tr><td colspan="4" class="text-center p-4 text-gray-500">ยังไม่มีข้อมูล</td></tr>';
                }
            };

            // Populate both status tables
            populateStatusTable('feed-status-table', feedOrders);
            populateStatusTable('meds-status-table', medOrders);
        };
        
        // --- Callbacks for Form/Status submissions ---
        window.formSubmitCallback = (response) => {
            Swal.close();
            if(response.status === "success") {
                Swal.fire({ title: 'บันทึกสำเร็จ!', icon: 'success', timer: 2000, showConfirmButton: false })
                   .then(() => {
                        document.getElementById('feedForm').reset();
                        document.getElementById('medsForm').reset();
                        changeTab('dashboard'); 
                   });
            } else {
                Swal.fire('เกิดข้อผิดพลาด', 'ไม่สามารถบันทึกข้อมูลได้: ' + response.message, 'error');
            }
        };
        
        window.statusUpdateCallback = (response) => {
            Swal.close();
            if(response.status === "success") {
                Swal.fire({ title: 'อัปเดตสำเร็จ!', icon: 'success', timer: 2000, showConfirmButton: false })
                   .then(() => jsonpRequest(`${SCRIPT_URL}?action=getData`, 'fetchData'));
            } else {
                Swal.fire('เกิดข้อผิดพลาด', 'ไม่สามารถอัปเดตสถานะได้: ' + response.message, 'error');
            }
        };

        // --- Generic JSONP Handler for submissions ---
        const jsonpRequestForSubmit = (url, callbackName) => {
             const script = document.createElement('script');
            script.src = `${url}&callback=${callbackName}`;
            document.body.appendChild(script);
            script.onload = () => { document.body.removeChild(script); delete window[callbackName]; };
            script.onerror = () => {
                 document.body.removeChild(script);
                 delete window[callbackName];
                 Swal.fire('เกิดข้อผิดพลาด', 'ไม่สามารถส่งข้อมูลได้', 'error');
            }
        }
        
        // --- Form and Status Change Handlers ---
        const submitForm = (formId, action) => {
            const form = document.getElementById(formId);
            if (!form.checkValidity()) return form.reportValidity();

            Swal.fire({ title: 'กำลังบันทึกข้อมูล...', allowOutsideClick: false, didOpen: () => Swal.showLoading() });
            const formData = new FormData(form);
            let url = `${SCRIPT_URL}?action=${action}`;
            for (let [key, value] of formData.entries()) {
                url += `&${encodeURIComponent(key)}=${encodeURIComponent(value)}`;
            }
            const callbackName = 'cb' + new Date().getTime();
            window[callbackName] = window.formSubmitCallback;
            jsonpRequestForSubmit(url, callbackName);
        };

        const saveStatusChanges = (tableId, action) => {
            const updates = Array.from(document.querySelectorAll(`#${tableId} tr`))
                .map(row => {
                    const id = row.dataset.id;
                    if (!id) return null;
                    const selectedStatus = row.querySelector('.status-selector').value;
                    const currentStatus = row.querySelector('span').textContent.trim();
                    return selectedStatus !== currentStatus ? { id, status: selectedStatus } : null;
                })
                .filter(Boolean);

            if (updates.length === 0) return Swal.fire('ไม่มีการเปลี่ยนแปลง', '', 'info');

            Swal.fire({ title: 'กำลังอัปเดตสถานะ...', allowOutsideClick: false, didOpen: () => Swal.showLoading() });
            const updatesJSON = encodeURIComponent(JSON.stringify(updates));
            const url = `${SCRIPT_URL}?action=${action}&updates=${updatesJSON}`;
            const callbackName = 'cb' + new Date().getTime();
            window[callbackName] = window.statusUpdateCallback;
            jsonpRequestForSubmit(url, callbackName);
        };
        
        const saveFeedStatusChanges = () => saveStatusChanges('feed-status-table', 'updateFeedStatus');
        const saveMedsStatusChanges = () => saveStatusChanges('meds-status-table', 'updateMedsStatus');

        // --- Event Listeners ---
        document.addEventListener('DOMContentLoaded', () => {
            setFormDateTime();
            changeTab('dashboard'); // Initial load

            document.getElementById('feedForm').addEventListener('submit', (e) => { e.preventDefault(); submitForm('feedForm', 'addFeed'); });
            document.getElementById('medsForm').addEventListener('submit', (e) => { e.preventDefault(); submitForm('medsForm', 'addMeds'); });
        });
    </script>

</body>
</html>

ระบบ GS code
/**
 * @fileoverview Google Apps Script for Purchase Order System.
 * Handles GET requests to read from and write to Google Sheets.
 * Supports adding feed/med orders and updating their statuses.
 * Uses JSONP for cross-origin communication.
 */

// --- CONFIGURATION ---
const SPREADSHEET_ID = "1tnU8rc5gnFNEWaBndiok5Jsf8VFfoXrWX2tCGBDBKh8";
const FEED_SHEET_NAME = "FeedOrders";
const MEDS_SHEET_NAME = "MedsOrders";

/**
 * Main function to handle GET requests. Acts as a router.
 * @param {object} e The event parameter from the GET request.
 * @returns {ContentService.TextOutput} A JSONP response.
 */
function doGet(e) {
  const lock = LockService.getScriptLock();
  lock.waitLock(30000); // Wait up to 30 seconds to prevent race conditions.

  try {
    const action = e.parameter.action;
    const callback = e.parameter.callback || 'callback';
    let responseData;

    switch (action) {
      case 'addFeed':
        responseData = addOrder(e, FEED_SHEET_NAME);
        break;
      case 'addMeds':
        responseData = addOrder(e, MEDS_SHEET_NAME);
        break;
      case 'getData':
        responseData = getAllData();
        break;
      case 'updateFeedStatus':
         responseData = updateOrderStatus(e, FEED_SHEET_NAME);
        break;
      case 'updateMedsStatus':
         responseData = updateOrderStatus(e, MEDS_SHEET_NAME);
        break;
      default:
        throw new Error("Invalid action specified.");
    }

    return createJsonpOutput(callback, { status: "success", data: responseData });

  } catch (error) {
    console.error(`Error in doGet: ${error.toString()}\nStack: ${error.stack}`);
    const callback = e.parameter.callback || 'callback';
    return createJsonpOutput(callback, { status: "error", message: error.message });
  } finally {
    lock.releaseLock();
  }
}

/**
 * Creates a JSONP-formatted text output.
 * @param {string} callback - The name of the callback function.
 * @param {object} data - The JavaScript object to be returned as JSON.
 * @returns {ContentService.TextOutput} The JSONP response.
 */
function createJsonpOutput(callback, data) {
  const jsonString = JSON.stringify(data);
  return ContentService
    .createTextOutput(`${callback}(${jsonString})`)
    .setMimeType(ContentService.MimeType.JAVASCRIPT);
}

/**
 * Gets a sheet by name. If it doesn't exist, creates it with appropriate headers.
 * @param {string} sheetName - The name of the sheet.
 * @returns {GoogleAppsScript.Spreadsheet.Sheet} The sheet object.
 */
function getSheet(sheetName) {
    const ss = SpreadsheetApp.openById(SPREADSHEET_ID);
    let sheet = ss.getSheetByName(sheetName);
    if (!sheet) {
      sheet = ss.insertSheet(sheetName);
      // Both sheets will now have a Status column
      sheet.appendRow(['ID', 'DateTime', 'Recorder', 'Details', 'Status']);
    }
    return sheet;
}

/**
 * Adds a new order to the specified sheet with a default status.
 * @param {object} e - The event parameter from the GET request.
 * @param {string} sheetName - The name of the sheet to add the order to.
 * @returns {object} A success message object.
 */
function addOrder(e, sheetName) {
  const sheet = getSheet(sheetName);
  
  const id = new Date().getTime().toString() + Math.random().toString(36).substring(2, 8);
  const datetime = e.parameter.datetime;
  const recorder = e.parameter.recorder;
  const details = e.parameter.details;
  const status = "กำลังดำเนินการ"; // Default status for all new orders

  if (!datetime || !recorder || !details) {
    throw new Error("Missing required parameters (datetime, recorder, details).");
  }
  
  sheet.appendRow([id, datetime, recorder, details, status]);
  
  return { message: "Order added successfully." };
}

/**
 * Updates the status of one or more orders in a given sheet.
 * @param {object} e The event parameter containing the updates.
 * @param {string} sheetName The name of the sheet to update.
 * @returns {object} A success message object.
 */
function updateOrderStatus(e, sheetName) {
  const updatesParam = e.parameter.updates;
  if (!updatesParam) throw new Error("Missing 'updates' parameter.");
  
  const updates = JSON.parse(updatesParam);
  if (!Array.isArray(updates) || updates.length === 0) {
    return { message: "No updates provided." };
  }

  const sheet = getSheet(sheetName);
  const dataRange = sheet.getDataRange();
  const values = dataRange.getValues();
  
  // Find column indexes from header row to be more robust
  const headers = values[0];
  const idColumnIndex = headers.indexOf('ID');
  const statusColumnIndex = headers.indexOf('Status');

  if (idColumnIndex === -1 || statusColumnIndex === -1) {
    throw new Error(`Could not find 'ID' or 'Status' column in sheet: ${sheetName}`);
  }

  let updatedCount = 0;
  const updateMap = new Map(updates.map(u => [u.id.toString(), u.status]));

  for (let i = 1; i < values.length; i++) {
    const rowId = values[i][idColumnIndex].toString();
    if (updateMap.has(rowId)) {
      values[i][statusColumnIndex] = updateMap.get(rowId);
      updatedCount++;
    }
  }

  if (updatedCount > 0) {
      dataRange.setValues(values);
  }

  return { message: `Successfully updated ${updatedCount} items in ${sheetName}.` };
}

/**
 * Retrieves all data from both feed and medicine sheets.
 * @returns {object} An object containing arrays of feed and medicine orders.
 */
function getAllData() {
  const mapRowToObject = (row) => ({
    id: row[0],
    datetime: row[1],
    recorder: row[2],
    details: row[3],
    status: row[4] || 'กำลังดำเนินการ' // Fallback status
  });

  const feedSheet = getSheet(FEED_SHEET_NAME);
  const medsSheet = getSheet(MEDS_SHEET_NAME);

  const feedData = feedSheet.getLastRow() > 1 ? feedSheet.getRange(2, 1, feedSheet.getLastRow() - 1, 5).getValues() : [];
  const medsData = medsSheet.getLastRow() > 1 ? medsSheet.getRange(2, 1, medsSheet.getLastRow() - 1, 5).getValues() : [];
  
  const feedOrders = feedData.map(mapRowToObject).reverse();
  const medOrders = medsData.map(mapRowToObject).reverse();

  return { feedOrders, medOrders };
}
