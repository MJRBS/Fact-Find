# Fact-Find
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Mortgage Fact Find</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Calm Harmony -->
    <!-- Application Structure Plan: A multi-step wizard form is the most user-friendly structure for this questionnaire. It breaks the process into manageable sections (Personal, Address, Employment, Finances, New Mortgage, Review), reducing user fatigue. A progress bar provides clear feedback on completion status. The flow is sequential, which is logical for this type of data collection. This structure was chosen to guide the user step-by-step, making the complex task of filling out a fact find feel simpler and more organized than a single, long-scrolling page. -->
    <!-- Visualization & Content Choices: The source is a form, so the primary presentation method is interactive form elements. Goal: Collect Information -> Method: HTML forms with dropdowns, text inputs, number inputs, and toggles. Interaction: Conditional logic (e.g., showing/hiding the second applicant's fields, revealing sections based on 'Yes'/'No' answers) makes the form dynamic and less cluttered. A dynamic list for adding credit commitments provides flexibility. A final review page allows for data verification. Justification: These choices directly support the goal of efficient and accurate data collection by simplifying the user interface and guiding the user's input. Library/Method: Vanilla JavaScript for all interactions. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .form-section {
            transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out;
        }
        .form-section.hidden {
            opacity: 0;
            transform: translateX(20px);
            pointer-events: none;
            position: absolute;
        }
        .form-section:not(.hidden) {
            opacity: 1;
            transform: translateX(0);
        }
        .progress-bar-fill {
            transition: width 0.4s ease-in-out;
        }
        .form-input, .form-select {
            @apply w-full px-4 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 transition-shadow;
        }
        .form-label {
            @apply block text-sm font-medium text-gray-700 mb-1;
        }
        .form-group {
            @apply mb-6;
        }
        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800">

    <div class="container mx-auto p-4 sm:p-6 md:p-8 max-w-4xl">
        <header class="text-center mb-8">
            <h1 class="text-3xl md:text-4xl font-bold text-gray-900">Mortgage Fact Find</h1>
            <p class="text-gray-600 mt-2">Please complete the following sections to help us find the best mortgage for you.</p>
        </header>

        <div class="bg-white p-6 sm:p-8 rounded-xl shadow-lg">
            <!-- Progress Bar -->
            <div id="progress-container" class="mb-8">
                <div class="flex justify-between items-center mb-2">
                    <span id="step-name" class="text-sm font-semibold text-blue-600"></span>
                    <span class="text-sm font-semibold text-gray-600"><span id="progress-percent">0</span>% Complete</span>
                </div>
                <div class="w-full bg-gray-200 rounded-full h-2.5">
                    <div id="progress-bar" class="bg-blue-600 h-2.5 rounded-full progress-bar-fill" style="width: 0%"></div>
                </div>
            </div>

            <form id="fact-find-form">
                <!-- Step 1: Personal Details -->
                <div id="step-1" class="form-section">
                    <h2 class="text-2xl font-semibold mb-6 border-b pb-4">Section 1: Personal Details</h2>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-x-8">
                        <div>
                            <h3 class="text-lg font-semibold text-gray-800 mb-4">Applicant 1</h3>
                            <div class="form-group">
                                <label for="app1-title" class="form-label">Title</label>
                                <select id="app1-title" name="app1-title" class="form-select">
                                    <option>Mr</option><option>Mrs</option><option>Miss</option><option>Ms</option><option>Dr</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="app1-name" class="form-label">Full Name</label>
                                <input type="text" id="app1-name" name="app1-name" class="form-input" placeholder="John Doe">
                            </div>
                            <div class="form-group">
                                <label for="app1-dob" class="form-label">Date of Birth</label>
                                <input type="date" id="app1-dob" name="app1-dob" class="form-input">
                            </div>
                             <div class="form-group">
                                <label for="app1-marital" class="form-label">Marital Status</label>
                                <select id="app1-marital" name="app1-marital" class="form-select">
                                    <option>Single</option><option>Married</option><option>Civil Partnership</option><option>Divorced</option><option>Separated</option><option>Widowed</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="app1-maiden-name" class="form-label">Mother's Maiden Name</label>
                                <input type="text" id="app1-maiden-name" name="app1-maiden-name" class="form-input" placeholder="Smith">
                            </div>
                        </div>
                        <div class="mt-8 md:mt-0">
                            <h3 class="text-lg font-semibold text-gray-800 mb-4">&nbsp;</h3>
                            <div class="form-group">
                                <label for="app1-nationality" class="form-label">Nationality</label>
                                <input type="text" id="app1-nationality" name="app1-nationality" class="form-input" placeholder="British">
                            </div>
                            <div class="form-group">
                                <label for="app1-ni" class="form-label">National Insurance Number</label>
                                <input type="text" id="app1-ni" name="app1-ni" class="form-input" placeholder="QQ 12 34 56 C">
                            </div>
                            <div class="form-group">
                                <label for="app1-phone" class="form-label">Contact Number</label>
                                <input type="tel" id="app1-phone" name="app1-phone" class="form-input" placeholder="07123456789">
                            </div>
                            <div class="form-group">
                                <label for="app1-email" class="form-label">Email Address</label>
                                <input type="email" id="app1-email" name="app1-email" class="form-input" placeholder="john.doe@example.com">
                            </div>
                        </div>
                    </div>
                    <div class="form-group mt-4">
                        <label class="form-label">Do you have any dependents?</label>
                        <div class="flex items-center space-x-4">
                            <label><input type="radio" name="app1-dependents" value="no" class="mr-1" checked onchange="toggleDependents('dependents-container', this.value)"> No</label>
                            <label><input type="radio" name="app1-dependents" value="yes" class="mr-1" onchange="toggleDependents('dependents-container', this.value)"> Yes</label>
                        </div>
                    </div>
                    <div id="dependents-container" class="hidden p-4 bg-gray-50 rounded-lg mt-2 fade-in">
                        <label for="dependents-details" class="form-label">Please provide details of each dependent (name, relationship, date of birth).</label>
                        <textarea id="dependents-details" name="dependents-details" rows="3" class="form-input"></textarea>
                    </div>

                    <div class="form-group mt-8 border-t pt-6">
                        <label class="form-label">Is there a second applicant?</label>
                        <label class="inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="add-applicant-2" class="sr-only peer" onchange="toggleApplicant2()">
                            <div class="relative w-11 h-6 bg-gray-200 rounded-full peer peer-focus:ring-4 peer-focus:ring-blue-300 peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-0.5 after:start-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-blue-600"></div>
                            <span class="ms-3 text-sm font-medium text-gray-900">Add Second Applicant</span>
                        </label>
                    </div>

                    <div id="applicant-2-section" class="hidden mt-6 fade-in">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-x-8 border-t pt-6">
                            <div>
                                <h3 class="text-lg font-semibold text-gray-800 mb-4">Applicant 2</h3>
                                <div class="form-group">
                                    <label for="app2-title" class="form-label">Title</label>
                                    <select id="app2-title" name="app2-title" class="form-select">
                                        <option>Mr</option><option>Mrs</option><option>Miss</option><option>Ms</option><option>Dr</option>
                                    </select>
                                </div>
                                <div class="form-group">
                                    <label for="app2-name" class="form-label">Full Name</label>
                                    <input type="text" id="app2-name" name="app2-name" class="form-input" placeholder="Jane Smith">
                                </div>
                                <div class="form-group">
                                    <label for="app2-dob" class="form-label">Date of Birth</label>
                                    <input type="date" id="app2-dob" name="app2-dob" class="form-input">
                                </div>
                                 <div class="form-group">
                                    <label for="app2-marital" class="form-label">Marital Status</label>
                                    <select id="app2-marital" name="app2-marital" class="form-select">
                                        <option>Single</option><option>Married</option><option>Civil Partnership</option><option>Divorced</option><option>Separated</option><option>Widowed</option>
                                    </select>
                                </div>
                                <div class="form-group">
                                    <label for="app2-maiden-name" class="form-label">Mother's Maiden Name</label>
                                    <input type="text" id="app2-maiden-name" name="app2-maiden-name" class="form-input" placeholder="Jones">
                                </div>
                            </div>
                            <div class="mt-8 md:mt-0">
                                <h3 class="text-lg font-semibold text-gray-800 mb-4">&nbsp;</h3>
                                <div class="form-group">
                                    <label for="app2-nationality" class="form-label">Nationality</label>
                                    <input type="text" id="app2-nationality" name="app2-nationality" class="form-input" placeholder="British">
                                </div>
                                <div class="form-group">
                                    <label for="app2-ni" class="form-label">National Insurance Number</label>
                                    <input type="text" id="app2-ni" name="app2-ni" class="form-input" placeholder="ZY 98 76 54 A">
                                </div>
                                <div class="form-group">
                                    <label for="app2-phone" class="form-label">Contact Number</label>
                                    <input type="tel" id="app2-phone" name="app2-phone" class="form-input" placeholder="07987654321">
                                </div>
                                <div class="form-group">
                                    <label for="app2-email" class="form-label">Email Address</label>
                                    <input type="email" id="app2-email" name="app2-email" class="form-input" placeholder="jane.smith@example.com">
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Step 2: Address History -->
                <div id="step-2" class="form-section hidden">
                    <h2 class="text-2xl font-semibold mb-6 border-b pb-4">Section 2: Address History</h2>
                    <div class="form-group">
                        <label for="current-address" class="form-label">Current Full Address</label>
                        <textarea id="current-address" name="current-address" rows="3" class="form-input" placeholder="123 Main Street, Anytown"></textarea>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-x-8">
                        <div class="form-group">
                            <label for="current-postcode" class="form-label">Postcode</label>
                            <input type="text" id="current-postcode" name="current-postcode" class="form-input" placeholder="AN1 2BC">
                        </div>
                        <div class="form-group">
                            <label for="current-res-status" class="form-label">Residential Status</label>
                            <select id="current-res-status" name="current-res-status" class="form-select">
                                <option>Owner with mortgage</option><option>Owner outright</option><option>Living with parents</option><option>Rented</option><option>Other</option>
                            </select>
                        </div>
                    </div>
                    <div class="form-group">
                        <label for="time-at-address-years" class="form-label">How long have you lived here?</label>
                        <div class="flex space-x-4">
                            <input type="number" id="time-at-address-years" name="time-at-address-years" class="form-input" placeholder="Years" min="0">
                            <input type="number" id="time-at-address-months" name="time-at-address-months" class="form-input" placeholder="Months" min="0" max="11">
                        </div>
                    </div>
                     <div class="form-group mt-4">
                        <label class="form-label">Have you lived at your current address for less than 3 years?</label>
                        <div class="flex items-center space-x-4">
                            <label><input type="radio" name="less-than-3-years" value="no" class="mr-1" checked onchange="toggleDependents('previous-address-container', this.value)"> No</label>
                            <label><input type="radio" name="less-than-3-years" value="yes" class="mr-1" onchange="toggleDependents('previous-address-container', this.value)"> Yes</label>
                        </div>
                    </div>
                    <div id="previous-address-container" class="hidden p-4 bg-gray-50 rounded-lg mt-2 fade-in">
                        <label for="previous-address-details" class="form-label">Please provide your address history for the past three years.</label>
                        <textarea id="previous-address-details" name="previous-address-details" rows="4" class="form-input"></textarea>
                    </div>
                </div>
                
                <!-- Step 3: Employment & Income -->
                <div id="step-3" class="form-section hidden">
                    <h2 class="text-2xl font-semibold mb-6 border-b pb-4">Section 3: Employment & Income</h2>
                    <div id="employment-app1-section">
                        <h3 class="text-lg font-semibold text-gray-800 mb-4">Applicant 1</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-x-8">
                            <div class="form-group">
                                <label for="emp1-status" class="form-label">Employment Status</label>
                                <select id="emp1-status" name="emp1-status" class="form-select">
                                    <option>Employed</option><option>Self-employed</option><option>Retired</option><option>Unemployed</option><option>Other</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="emp1-occupation" class="form-label">Occupation</label>
                                <input type="text" id="emp1-occupation" name="emp1-occupation" class="form-input">
                            </div>
                            <div class="form-group">
                                <label for="emp1-employer" class="form-label">Employer Name</label>
                                <input type="text" id="emp1-employer" name="emp1-employer" class="form-input">
                            </div>
                            <div class="form-group">
                                <label for="emp1-time-years" class="form-label">Time with Current Employer</label>
                                <div class="flex space-x-4">
                                    <input type="number" id="emp1-time-years" name="emp1-time-years" class="form-input" placeholder="Years" min="0">
                                    <input type="number" id="emp1-time-months" name="emp1-time-months" class="form-input" placeholder="Months" min="0" max="11">
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="emp1-salary" class="form-label">Annual Basic Salary (Gross)</label>
                                <input type="number" id="emp1-salary" name="emp1-salary" class="form-input" placeholder="35000" min="0">
                            </div>
                            <div class="form-group">
                                <label for="emp1-retirement" class="form-label">Anticipated Retirement Age</label>
                                <input type="number" id="emp1-retirement" name="emp1-retirement" class="form-input" placeholder="67" min="18">
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="emp1-additional-income" class="form-label">Additional Income (bonuses, commission, etc.)</label>
                            <textarea id="emp1-additional-income" name="emp1-additional-income" rows="3" class="form-input" placeholder="e.g., Annual bonus: £5000"></textarea>
                        </div>
                    </div>

                    <div id="employment-app2-section" class="hidden mt-8 border-t pt-6 fade-in">
                        <h3 class="text-lg font-semibold text-gray-800 mb-4">Applicant 2</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-x-8">
                            <div class="form-group">
                                <label for="emp2-status" class="form-label">Employment Status</label>
                                <select id="emp2-status" name="emp2-status" class="form-select">
                                    <option>Employed</option><option>Self-employed</option><option>Retired</option><option>Unemployed</option><option>Other</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="emp2-occupation" class="form-label">Occupation</label>
                                <input type="text" id="emp2-occupation" name="emp2-occupation" class="form-input">
                            </div>
                            <div class="form-group">
                                <label for="emp2-employer" class="form-label">Employer Name</label>
                                <input type="text" id="emp2-employer" name="emp2-employer" class="form-input">
                            </div>
                            <div class="form-group">
                                <label for="emp2-time-years" class="form-label">Time with Current Employer</label>
                                <div class="flex space-x-4">
                                    <input type="number" id="emp2-time-years" name="emp2-time-years" class="form-input" placeholder="Years" min="0">
                                    <input type="number" id="emp2-time-months" name="emp2-time-months" class="form-input" placeholder="Months" min="0" max="11">
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="emp2-salary" class="form-label">Annual Basic Salary (Gross)</label>
                                <input type="number" id="emp2-salary" name="emp2-salary" class="form-input" min="0">
                            </div>
                            <div class="form-group">
                                <label for="emp2-retirement" class="form-label">Anticipated Retirement Age</label>
                                <input type="number" id="emp2-retirement" name="emp2-retirement" class="form-input" placeholder="67" min="18">
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="emp2-additional-income" class="form-label">Additional Income (bonuses, commission, etc.)</label>
                            <textarea id="emp2-additional-income" name="emp2-additional-income" rows="3" class="form-input" placeholder="e.g., Overtime: £200/month"></textarea>
                        </div>
                    </div>
                </div>
                
                <!-- Step 4: Financial Overview -->
                <div id="step-4" class="form-section hidden">
                    <h2 class="text-2xl font-semibold mb-6 border-b pb-4">Section 4: Financial Overview</h2>
                    
                    <h3 class="text-lg font-semibold text-gray-800 mb-4">Existing Mortgage</h3>
                    <div class="form-group">
                        <label class="form-label">Do you currently have a mortgage?</label>
                        <div class="flex items-center space-x-4">
                            <label><input type="radio" name="has-mortgage" value="no" class="mr-1" checked onchange="toggleDependents('existing-mortgage-details', this.value)"> No</label>
                            <label><input type="radio" name="has-mortgage" value="yes" class="mr-1" onchange="toggleDependents('existing-mortgage-details', this.value)"> Yes</label>
                        </div>
                    </div>
                    <div id="existing-mortgage-details" class="hidden grid grid-cols-1 md:grid-cols-2 gap-x-8 p-4 bg-gray-50 rounded-lg fade-in">
                        <div class="form-group"><label for="mortgage-lender" class="form-label">Lender</label><input type="text" id="mortgage-lender" name="mortgage-lender" class="form-input"></div>
                        <div class="form-group"><label for="mortgage-balance" class="form-label">Current Balance (£)</label><input type="number" id="mortgage-balance" name="mortgage-balance" class="form-input" min="0"></div>
                        <div class="form-group"><label for="mortgage-payment" class="form-label">Monthly Payment (£)</label><input type="number" id="mortgage-payment" name="mortgage-payment" class="form-input" min="0"></div>
                        <div class="form-group"><label for="mortgage-rate-type" class="form-label">Interest Rate Type</label><select id="mortgage-rate-type" name="mortgage-rate-type" class="form-select"><option>Fixed</option><option>Variable</option><option>Tracker</option></select></div>
                        <div class="form-group"><label for="mortgage-term" class="form-label">Term Remaining (Years)</label><input type="number" id="mortgage-term" name="mortgage-term" class="form-input" min="0"></div>
                        <div class="form-group"><label for="mortgage-property-value" class="form-label">Estimated Property Value (£)</label><input type="number" id="mortgage-property-value" name="mortgage-property-value" class="form-input" min="0"></div>
                    </div>

                    <h3 class="text-lg font-semibold text-gray-800 mt-8 mb-4 border-t pt-6">Credit Commitments</h3>
                    <div id="debts-container"></div>
                    <button type="button" onclick="addDebt()" class="mt-2 px-4 py-2 bg-blue-100 text-blue-700 font-semibold rounded-lg hover:bg-blue-200 transition-colors text-sm">+ Add Commitment</button>

                    <h3 class="text-lg font-semibold text-gray-800 mt-8 mb-4 border-t pt-6">Monthly Household Expenditures (£)</h3>
                    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-x-8">
                        <div class="form-group"><label for="exp-council-tax" class="form-label">Council Tax</label><input type="number" id="exp-council-tax" name="exp-council-tax" class="form-input" min="0"></div>
                        <div class="form-group"><label for="exp-utilities" class="form-label">Utilities</label><input type="number" id="exp-utilities" name="exp-utilities" class="form-input" min="0"></div>
                        <div class="form-group"><label for="exp-insurance" class="form-label">Home Insurance</label><input type="number" id="exp-insurance" name="exp-insurance" class="form-input" min="0"></div>
                        <div class="form-group"><label for="exp-groceries" class="form-label">Food/Groceries</label><input type="number" id="exp-groceries" name="exp-groceries" class="form-input" min="0"></div>
                        <div class="form-group"><label for="exp-childcare" class="form-label">Childcare/School</label><input type="number" id="exp-childcare" name="exp-childcare" class="form-input" min="0"></div>
                        <div class="form-group"><label for="exp-transport" class="form-label">Transport</label><input type="number" id="exp-transport" name="exp-transport" class="form-input" min="0"></div>
                    </div>
                    <div class="form-group">
                        <label for="exp-other" class="form-label">Other Significant Outgoings</label>
                        <textarea id="exp-other" name="exp-other" rows="3" class="form-input" placeholder="e.g., Gym: £40, Subscriptions: £25"></textarea>
                    </div>
                </div>

                <!-- Step 5: New Mortgage & Credit History -->
                <div id="step-5" class="form-section hidden">
                    <h2 class="text-2xl font-semibold mb-6 border-b pb-4">Section 5: New Mortgage & Credit History</h2>
                    <h3 class="text-lg font-semibold text-gray-800 mb-4">New Mortgage Requirements</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-x-8">
                        <div class="form-group">
                            <label for="new-purpose" class="form-label">Purpose of Mortgage</label>
                            <select id="new-purpose" name="new-purpose" class="form-select"><option>Purchase</option><option>Remortgage</option><option>Buy to Let</option></select>
                        </div>
                        <div class="form-group">
                            <label for="new-property-type" class="form-label">Property Type</label>
                            <select id="new-property-type" name="new-property-type" class="form-select"><option>House</option><option>Flat</option><option>Bungalow</option><option>Maisonette</option></select>
                        </div>
                        <div class="form-group">
                            <label for="new-loan-amount" class="form-label">Desired Loan Amount (£)</label>
                            <input type="number" id="new-loan-amount" name="new-loan-amount" class="form-input" min="0">
                        </div>
                        <div class="form-group">
                            <label for="new-term" class="form-label">Desired Mortgage Term (Years)</label>
                            <input type="number" id="new-term" name="new-term" class="form-input" min="1">
                        </div>
                         <div class="form-group">
                            <label for="new-deposit-source" class="form-label">Source of Deposit</label>
                            <select id="new-deposit-source" name="new-deposit-source" class="form-select"><option>Savings</option><option>Gift from family</option><option>Sale of property</option><option>Other</option></select>
                        </div>
                    </div>
                    <div class="form-group">
                        <label for="new-additional-info" class="form-label">Any additional information or requirements?</label>
                        <textarea id="new-additional-info" name="new-additional-info" rows="3" class="form-input"></textarea>
                    </div>

                    <h3 class="text-lg font-semibold text-gray-800 mt-8 mb-4 border-t pt-6">Credit History</h3>
                    <div class="space-y-4">
                        <div class="form-group">
                            <label class="form-label">Have you ever had a mortgage or loan application refused?</label>
                            <div class="flex items-center space-x-4">
                                <label><input type="radio" name="ch-refused" value="no" class="mr-1" checked> No</label>
                                <label><input type="radio" name="ch-refused" value="yes" class="mr-1"> Yes</label>
                            </div>
                        </div>
                        <div class="form-group">
                            <label class="form-label">Have you ever had any County Court Judgements (CCJs) or Defaults?</label>
                            <div class="flex items-center space-x-4">
                                <label><input type="radio" name="ch-ccj" value="no" class="mr-1" checked> No</label>
                                <label><input type="radio" name="ch-ccj" value="yes" class="mr-1"> Yes</label>
                            </div>
                        </div>
                        <div class="form-group">
                            <label class="form-label">Have you ever been declared bankrupt or entered into an IVA?</label>
                            <div class="flex items-center space-x-4">
                                <label><input type="radio" name="ch-iva" value="no" class="mr-1" checked> No</label>
                                <label><input type="radio" name="ch-iva" value="yes" class="mr-1"> Yes</label>
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="ch-details" class="form-label">If you answered yes to any of the above, please provide details.</label>
                            <textarea id="ch-details" name="ch-details" rows="3" class="form-input"></textarea>
                        </div>
                    </div>
                </div>
                
                <!-- Step 6: Review & Submit -->
                <div id="step-6" class="form-section hidden">
                    <h2 class="text-2xl font-semibold mb-6 border-b pb-4">Review Your Information</h2>
                    <p class="text-gray-600 mb-6">Please review all the information you have provided for accuracy. You can click "Edit" to go back to any section and make changes.</p>
                    <div id="review-container" class="space-y-6"></div>
                </div>

                <!-- Navigation -->
                <div id="navigation-buttons" class="flex justify-between items-center mt-8 pt-6 border-t">
                    <button type="button" id="prev-btn" onclick="prevStep()" class="px-6 py-2 bg-gray-200 text-gray-800 font-semibold rounded-lg hover:bg-gray-300 transition-colors disabled:opacity-50 disabled:cursor-not-allowed" disabled>Previous</button>
                    <button type="button" id="next-btn" onclick="nextStep()" class="px-6 py-2 bg-blue-600 text-white font-semibold rounded-lg hover:bg-blue-700 transition-colors">Next</button>
                    <button type="button" id="submit-btn" class="hidden px-6 py-2 bg-green-600 text-white font-semibold rounded-lg hover:bg-green-700 transition-colors">Submit Application</button>
                </div>
            </form>
        </div>
        <footer class="text-center text-sm text-gray-500 mt-8">
            <p>&copy; 2025 Your Mortgage Brokerage. All Rights Reserved.</p>
        </footer>
    </div>

    <script>
        let currentStep = 1;
        const totalSteps = 6;
        const stepNames = [
            "Personal Details",
            "Address History",
            "Employment & Income",
            "Financial Overview",
            "New Mortgage & Credit History",
            "Review & Submit"
        ];

        document.addEventListener('DOMContentLoaded', () => {
            updateProgress();
            addDebt(); // Add one initial debt row
        });

        function updateProgress() {
            const progressPercent = Math.round(((currentStep - 1) / (totalSteps -1)) * 100);
            document.getElementById('progress-bar').style.width = `${progressPercent}%`;
            document.getElementById('progress-percent').innerText = progressPercent;
            document.getElementById('step-name').innerText = `Step ${currentStep}: ${stepNames[currentStep - 1]}`;

            document.getElementById('prev-btn').disabled = currentStep === 1;
            document.getElementById('next-btn').classList.toggle('hidden', currentStep === totalSteps);
            document.getElementById('submit-btn').classList.toggle('hidden', currentStep !== totalSteps);
        }

        function showStep(step) {
            document.querySelectorAll('.form-section').forEach(section => {
                section.classList.add('hidden');
            });
            const targetStep = document.getElementById(`step-${step}`);
            if(targetStep) {
                targetStep.classList.remove('hidden');
                targetStep.classList.add('fade-in');
            }
        }

        function nextStep() {
            if (currentStep < totalSteps) {
                currentStep++;
                showStep(currentStep);
                updateProgress();
                if (currentStep === totalSteps) {
                    populateReview();
                }
            }
        }

        function prevStep() {
            if (currentStep > 1) {
                currentStep--;
                showStep(currentStep);
                updateProgress();
            }
        }
        
        function goToStep(step) {
            currentStep = step;
            showStep(currentStep);
            updateProgress();
        }

        function toggleApplicant2() {
            const checkbox = document.getElementById('add-applicant-2');
            const section = document.getElementById('applicant-2-section');
            const empSection = document.getElementById('employment-app2-section');
            section.classList.toggle('hidden', !checkbox.checked);
            empSection.classList.toggle('hidden', !checkbox.checked);
        }

        function toggleDependents(containerId, value) {
            const container = document.getElementById(containerId);
            container.classList.toggle('hidden', value !== 'yes');
        }
        
        let debtCount = 0;
        function addDebt() {
            debtCount++;
            const container = document.getElementById('debts-container');
            const debtDiv = document.createElement('div');
            debtDiv.className = 'grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-4 mb-4 p-4 border rounded-lg bg-gray-50/50 fade-in';
            debtDiv.innerHTML = `
                <div class="form-group col-span-1 sm:col-span-2 md:col-span-1">
                    <label class="form-label">Type of Debt</label>
                    <input type="text" name="debt-type-${debtCount}" class="form-input" placeholder="e.g., Credit Card">
                </div>
                <div class="form-group col-span-1 sm:col-span-2 md:col-span-1">
                    <label class="form-label">Lender/Provider</label>
                    <input type="text" name="debt-lender-${debtCount}" class="form-input">
                </div>
                <div class="form-group">
                    <label class="form-label">Current Balance (£)</label>
                    <input type="number" name="debt-balance-${debtCount}" class="form-input" min="0">
                </div>
                <div class="form-group">
                    <label class="form-label">Monthly Payment (£)</label>
                    <input type="number" name="debt-payment-${debtCount}" class="form-input" min="0">
                </div>
            `;
            container.appendChild(debtDiv);
        }

        function populateReview() {
            const form = document.getElementById('fact-find-form');
            const formData = new FormData(form);
            const reviewContainer = document.getElementById('review-container');
            reviewContainer.innerHTML = '';

            const sections = [
                { title: "Personal Details", step: 1, fields: ['app1-title', 'app1-name', 'app1-dob', 'app1-marital', 'app1-maiden-name', 'app1-nationality', 'app1-ni', 'app1-phone', 'app1-email', 'app1-dependents', 'dependents-details', 'app2-title', 'app2-name', 'app2-dob', 'app2-marital', 'app2-maiden-name', 'app2-nationality', 'app2-ni', 'app2-phone', 'app2-email'] },
                { title: "Address History", step: 2, fields: ['current-address', 'current-postcode', 'current-res-status', 'time-at-address-years', 'time-at-address-months', 'previous-address-details'] },
                { title: "Employment & Income", step: 3, fields: ['emp1-status', 'emp1-occupation', 'emp1-employer', 'emp1-time-years', 'emp1-time-months', 'emp1-salary', 'emp1-retirement', 'emp1-additional-income', 'emp2-status', 'emp2-occupation', 'emp2-employer', 'emp2-time-years', 'emp2-time-months', 'emp2-salary', 'emp2-retirement', 'emp2-additional-income'] },
                { title: "Financial Overview", step: 4, fields: ['has-mortgage', 'mortgage-lender', 'mortgage-balance', 'mortgage-payment', 'mortgage-rate-type', 'mortgage-term', 'mortgage-property-value', 'exp-council-tax', 'exp-utilities', 'exp-insurance', 'exp-groceries', 'exp-childcare', 'exp-transport', 'exp-other'] },
                { title: "New Mortgage & Credit History", step: 5, fields: ['new-purpose', 'new-property-type', 'new-loan-amount', 'new-term', 'new-deposit-source', 'new-additional-info', 'ch-refused', 'ch-ccj', 'ch-iva', 'ch-details'] }
            ];

            sections.forEach(section => {
                const sectionDiv = document.createElement('div');
                sectionDiv.className = 'p-4 border rounded-lg';
                let content = `<div class="flex justify-between items-center mb-3"><h3 class="text-lg font-semibold">${section.title}</h3><button type="button" onclick="goToStep(${section.step})" class="text-sm text-blue-600 hover:underline">Edit</button></div><dl class="grid grid-cols-1 md:grid-cols-2 gap-x-8 gap-y-2">`;
                let hasContent = false;
                
                section.fields.forEach(fieldName => {
                    const formElement = form.elements[fieldName];
                    if (!formElement) return;

                    let value = formData.get(fieldName);
                    let labelText = '';

                    if (formElement instanceof RadioNodeList) {
                        const checkedRadio = Array.from(formElement).find(radio => radio.checked);
                        if (checkedRadio) {
                           value = checkedRadio.value;
                           labelText = checkedRadio.closest('.form-group')?.querySelector('label.form-label')?.innerText || fieldName.replace(/[-_]/g, ' ');
                        } else {
                           value = null; // No radio selected in this group
                        }
                    } else {
                        labelText = formElement.closest('.form-group')?.querySelector('label.form-label')?.innerText || fieldName.replace(/[-_]/g, ' ');
                    }

                    if (value && value.trim() !== '') {
                        hasContent = true;
                        content += `<div class="flex flex-col"><dt class="text-sm text-gray-500">${labelText}</dt><dd class="font-medium">${value}</dd></div>`;
                    }
                });
                
                 if (section.title === "Financial Overview") {
                    for (let i = 1; i <= debtCount; i++) {
                        const type = formData.get(`debt-type-${i}`);
                        if (type && type.trim() !== '') {
                             hasContent = true;
                             content += `<div class="col-span-full mt-2 border-t pt-2"><dt class="font-semibold">Commitment ${i}</dt><dd class="grid grid-cols-2 gap-x-4"><div>Type: ${type}</div><div>Lender: ${formData.get(`debt-lender-${i}`)}</div><div>Balance: £${formData.get(`debt-balance-${i}`)}</div><div>Payment: £${formData.get(`debt-payment-${i}`)}</div></dd></div>`;
                        }
                    }
                }

                if(hasContent){
                    content += `</dl>`;
                    sectionDiv.innerHTML = content;
                    reviewContainer.appendChild(sectionDiv);
                }
            });
        }

    </script>
</body>
</html>
