<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cars24india: Social Media & Review Reply Assistant</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #F8F8F8; /* Light background */
            color: #333;
            transition: background-color 0.3s ease; /* Smooth transition for body bg if needed */
        }
        .container { /* Main app container */
            max-width: 800px;
            margin: 2rem auto;
            padding: 1.5rem;
            background-color: #ffffff;
            border-radius: 0.75rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }
        .login-container { /* Login page container */
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #1A73E8; /* Blue background for login page */
            padding: 1rem;
        }
        .login-box {
            background-color: #ffffff;
            padding: 2.5rem 2rem;
            border-radius: 0.75rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            width: 100%;
            max-width: 400px;
            text-align: center;
        }
        .login-title {
            font-size: 1.875rem; /* text-3xl */
            font-weight: 700; /* bold */
            color: #1A73E8; /* Blue */
            margin-bottom: 0.5rem;
        }
        .login-subtitle {
            font-size: 1rem; /* text-base */
            color: #4B5563; /* gray-600 */
            margin-bottom: 2rem;
        }
        .login-error {
            color: #EF4444; /* red-500 */
            font-size: 0.875rem; /* text-sm */
            margin-top: 0.5rem;
            min-height: 1.25rem; /* Reserve space to prevent layout shift */
        }

        .header-bg {
            background-color: #1A73E8; /* A shade of blue for Cars24india */
            color: #ffffff;
            padding: 2rem;
            border-radius: 0.75rem 0.75rem 0 0; /* Rounded top corners */
            text-align: center;
        }
        .section-title {
            color: #1A73E8; /* Blue */
            font-weight: 700;
            margin-bottom: 1.5rem;
            text-align: center;
        }
        .button-primary {
            background-color: #1A73E8; /* Blue */
            color: white;
            font-weight: bold;
            padding: 0.75rem 1.5rem;
            border-radius: 0.5rem;
            transition: background-color 0.3s ease;
            cursor: pointer;
            width: 100%; /* Make buttons full width in their context */
        }
        .button-primary:hover {
            background-color: #155CBF; /* Darker blue on hover */
        }
        .button-secondary {
            background-color: #e0e0e0; /* Light grey */
            color: #333;
            font-weight: bold;
            padding: 0.65rem 1.3rem; /* Slightly smaller */
            border-radius: 0.5rem;
            transition: background-color 0.3s ease;
            cursor: pointer;
            border: 1px solid #ccc;
        }
        .button-secondary:hover {
            background-color: #d0d0d0; /* Darker grey on hover */
        }
        .input-field, .select-field {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid #D1D5DB; /* Light grey border */
            border-radius: 0.5rem;
            font-size: 1rem;
            line-height: 1.5;
            color: #374151; /* Darker grey text */
            transition: border-color 0.2s ease, box-shadow 0.2s ease;
            background-color: #fff; /* Ensure select has background */
        }
        .input-field:focus, .select-field:focus {
            outline: none;
            border-color: #1A73E8; /* Blue focus border */
            box-shadow: 0 0 0 3px rgba(26, 115, 232, 0.2); /* Blue focus shadow */
        }
        .output-box {
            background-color: #F3F4F6; /* Lighter grey background */
            border: 1px solid #E5E7EB; /* Light grey border */
            border-radius: 0.5rem;
            padding: 1rem;
            margin-top: 1.5rem;
            color: #4B5563; /* Medium grey text */
            white-space: pre-wrap; /* Preserve whitespace and line breaks */
            word-wrap: break-word; /* Break long words */
        }
        .analysis-output-box {
            background-color: #E9F5FF; /* Light blue background */
            border: 1px solid #BEE3F8; /* Blue border */
            border-radius: 0.5rem;
            padding: 1rem;
            margin-top: 1rem;
            margin-bottom: 1.5rem; /* Space before next section */
            color: #2A4365; /* Dark blue text */
        }
        .loading-spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #1A73E8; /* Blue spinner */
            border-radius: 50%;
            width: 30px;
            height: 30px;
            animation: spin 1s linear infinite;
            margin: 1rem auto;
            display: none;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        label {
            display: block;
            font-size: 0.875rem; /* text-sm */
            font-weight: 500; /* medium */
            color: #374151; /* text-gray-700 */
            margin-bottom: 0.5rem; /* mb-2 */
            text-align: left; /* Align labels to the left */
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div id="loginPage" class="login-container">
        <div class="login-box">
            <h2 class="login-title">Cars24india Assistant</h2>
            <p class="login-subtitle">Please enter your email to access the tool.</p>
            <form id="loginForm">
                <div class="mb-4">
                    <label for="emailInput">Email Address</label>
                    <input type="email" id="emailInput" placeholder="you@example.com" class="input-field" required>
                </div>
                <button type="submit" id="loginButton" class="button-primary">Login</button>
                <p id="loginError" class="login-error"></p>
            </form>
        </div>
    </div>

    <div id="appContainer" class="container hidden">
        <div class="header-bg">
            <h1 class="text-3xl font-bold mb-2">Cars24india</h1>
            <p class="text-lg">Social Media & Review Reply Assistant</p>
        </div>

        <section id="social-reply-assistant" class="py-8">
            <h2 class="text-2xl section-title">Generate a Reply</h2>
            <p class="text-center text-gray-600 mb-6">
                Craft quick and effective responses for various social media and review platforms.
            </p>

            <div class="mb-4">
                <label for="socialPlatform">Platform:</label>
                <select id="socialPlatform" class="select-field">
                    <option value="Twitter">Twitter (X)</option>
                    <option value="Instagram">Instagram</option>
                    <option value="Facebook">Facebook</option>
                    <option value="LinkedIn">LinkedIn</option>
                    <option value="GMB">Google My Business (GMB)</option>
                    <option value="PlayStore">Google Play Store</option>
                    <option value="AppStore">Apple App Store</option>
                </select>
            </div>

            <div class="mb-4">
                <label for="replyLanguage">Reply Language:</label>
                <select id="replyLanguage" class="select-field">
                    <option value="English">English</option>
                    <option value="Hindi (Devanagari script)">Hindi (Devanagari script)</option>
                    <option value="Tamil">Tamil</option>
                    <option value="Kannada">Kannada</option>
                    <option value="Telugu">Telugu</option>
                    <option value="Malayalam">Malayalam</option>
                    <option value="Gujarati">Gujarati</option>
                </select>
            </div>
            
            <div class="mb-4">
                <label for="customerContent" id="customerContentLabel">Customer Comment/Mention:</label>
                <textarea id="customerContent" rows="4" placeholder="Paste the customer's content (comment, mention, review) here..." class="input-field"></textarea>
            </div>

            <div class="mb-2 text-center"> 
                 <button id="analyzeContentBtn" class="button-secondary">
                    ✨ Analyze Content ✨
                </button>
            </div>
            <div id="analysisLoadingSpinner" class="loading-spinner"></div>
            <div id="analysisOutput" class="analysis-output-box hidden">
                <p class="font-semibold mb-1">Content Analysis:</p>
                <p id="suggestedSentimentText" class="mb-1"></p>
                <p id="extractedKeywordsText"></p>
            </div>
            
            <div class="mb-4">
                <label for="contentSentiment">Content Sentiment:</label>
                <select id="contentSentiment" class="select-field">
                    <option value="Negative (Complaint/Issue)">Negative (Complaint/Issue)</option>
                    <option value="Positive (Appreciation/Praise)">Positive (Appreciation/Praise)</option>
                    <option value="Neutral (Inquiry/Feedback)">Neutral (Inquiry/Feedback)</option>
                    <option value="Bug Report (App Stores)">Bug Report (App Stores)</option>
                    <option value="Feature Request (App Stores)">Feature Request (App Stores)</option>
                </select>
            </div>

            <div class="mb-6">
                <label for="replyInstructions">Reply Instructions (Optional):</label>
                <textarea id="replyInstructions" rows="3" placeholder="E.g., 'Emphasize commitment to quality', 'Mention upcoming app update', 'Ask them to fill the form for details.'" class="input-field"></textarea>
            </div>
            
            <button id="generateReplyBtn" class="button-primary w-full">
                Generate Reply ✨
            </button>
            
            <div id="generatedReplyOutput" class="output-box hidden">
                <p class="font-semibold text-blue-700 mb-2">Generated Reply:</p>
                <p id="generatedReplyText"></p>
                <div id="replyLoadingSpinner" class="loading-spinner"></div>
            </div>
        </section>

        <footer class="text-center text-sm text-gray-500 mt-8">
            <p>&copy; 2025 Cars24india Social Media & Review Reply Assistant. Powered by Gemini API. For illustrative purposes only.</p>
        </footer>
    </div>

    <script>
        // Login Page Elements
        const loginPage = document.getElementById('loginPage');
        const appContainer = document.getElementById('appContainer');
        const loginForm = document.getElementById('loginForm');
        const emailInput = document.getElementById('emailInput');
        const loginError = document.getElementById('loginError');

        // Main App Elements (existing)
        const socialPlatformSelect = document.getElementById('socialPlatform');
        const customerContentLabel = document.getElementById('customerContentLabel');
        const customerContentTextarea = document.getElementById('customerContent');
        const contentSentimentSelect = document.getElementById('contentSentiment');
        const replyLanguageSelect = document.getElementById('replyLanguage');
        
        const analyzeContentBtn = document.getElementById('analyzeContentBtn');
        const analysisOutputDiv = document.getElementById('analysisOutput');
        const suggestedSentimentText = document.getElementById('suggestedSentimentText');
        const extractedKeywordsText = document.getElementById('extractedKeywordsText');
        const analysisLoadingSpinner = document.getElementById('analysisLoadingSpinner');

        const generateReplyBtn = document.getElementById('generateReplyBtn');
        const generatedReplyOutput = document.getElementById('generatedReplyOutput');
        const generatedReplyText = document.getElementById('generatedReplyText');
        const replyLoadingSpinner = document.getElementById('replyLoadingSpinner');

        // Login Logic
        loginForm.addEventListener('submit', (event) => {
            event.preventDefault(); // Prevent actual form submission
            const email = emailInput.value.trim();
            // Simple email validation (must not be empty and contain '@')
            if (email && email.includes('@')) {
                loginPage.classList.add('hidden');
                appContainer.classList.remove('hidden');
                document.body.style.backgroundColor = '#F8F8F8'; // Reset body background for app
                loginError.textContent = ''; // Clear any previous error
            } else {
                loginError.textContent = 'Please enter a valid email address.';
            }
        });


        // --- Existing App Logic ---

        // Function to update sentiment options based on platform
        function updateSentimentOptions() {
            const platform = socialPlatformSelect.value;
            const currentSentimentValue = contentSentimentSelect.value;
            
            while (contentSentimentSelect.options.length > 0) {
                contentSentimentSelect.remove(0);
            }

            contentSentimentSelect.add(new Option("Negative (Complaint/Issue)", "Negative (Complaint/Issue)"));
            contentSentimentSelect.add(new Option("Positive (Appreciation/Praise)", "Positive (Appreciation/Praise)"));
            contentSentimentSelect.add(new Option("Neutral (Inquiry/Feedback)", "Neutral (Inquiry/Feedback)"));

            if (platform === 'PlayStore' || platform === 'AppStore') {
                contentSentimentSelect.add(new Option("Bug Report", "Bug Report (App Stores)"));
                contentSentimentSelect.add(new Option("Feature Request", "Feature Request (App Stores)"));
            }
            
            let valueFound = false;
            for(let i=0; i < contentSentimentSelect.options.length; i++) {
                if(contentSentimentSelect.options[i].value === currentSentimentValue) {
                    contentSentimentSelect.value = currentSentimentValue;
                    valueFound = true;
                    break;
                }
            }
            if (!valueFound && contentSentimentSelect.options.length > 0) {
                 contentSentimentSelect.selectedIndex = 0; 
            }
        }


        // Update labels and placeholders based on platform
        socialPlatformSelect.addEventListener('change', () => {
            const platform = socialPlatformSelect.value;
            updateSentimentOptions(); 

            switch(platform) {
                case 'Twitter':
                    customerContentLabel.textContent = 'Customer Tweet:';
                    customerContentTextarea.placeholder = "Paste the customer's tweet here (e.g., 'My car delivery is delayed! #Cars24indiaService')";
                    break;
                case 'Instagram':
                case 'Facebook':
                case 'LinkedIn':
                    customerContentLabel.textContent = 'Customer Comment/Mention:';
                    customerContentTextarea.placeholder = `Paste the customer's ${platform} comment or mention here...`;
                    break;
                case 'GMB':
                    customerContentLabel.textContent = 'Customer Review/Question (GMB):';
                    customerContentTextarea.placeholder = "Paste the customer's Google My Business review or question here...";
                    break;
                case 'PlayStore':
                    customerContentLabel.textContent = 'User Review/Feedback (Play Store):';
                    customerContentTextarea.placeholder = "Paste the user's Google Play Store review or feedback here...";
                    break;
                case 'AppStore':
                    customerContentLabel.textContent = 'User Review/Feedback (App Store):';
                    customerContentTextarea.placeholder = "Paste the user's Apple App Store review or feedback here...";
                    break;
                default:
                    customerContentLabel.textContent = 'Customer Comment/Mention:';
                    customerContentTextarea.placeholder = "Paste the customer's content here...";
            }
        });
        // Initialize on load, but only if appContainer is visible (which it won't be initially)
        // This will be effectively called once login is successful and app is shown.
        if (!appContainer.classList.contains('hidden')) {
            socialPlatformSelect.dispatchEvent(new Event('change'));
        }


        // Function to call Gemini API (generic for text or JSON)
        async function callGeminiAPI(prompt, targetSpinner, isJsonOutput = false) {
            targetSpinner.style.display = 'block'; 
            
            let chatHistory = [];
            chatHistory.push({ role: "user", parts: [{ text: prompt }] });
            
            const payload = { contents: chatHistory };
            if (isJsonOutput) {
                payload.generationConfig = {
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "OBJECT",
                        properties: {
                            "suggestedSentiment": { 
                                "type": "STRING",
                                "enum": [ 
                                    "Negative (Complaint/Issue)",
                                    "Positive (Appreciation/Praise)",
                                    "Neutral (Inquiry/Feedback)"
                                ]
                             },
                            "extractedKeywords": {
                                "type": "ARRAY",
                                "items": { "type": "STRING" }
                            }
                        },
                        required: ["suggestedSentiment", "extractedKeywords"]
                    }
                };
            }

            const apiKey = ""; 
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                
                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const textOutput = result.candidates[0].content.parts[0].text;
                    if (isJsonOutput) {
                        try {
                            return JSON.parse(textOutput);
                        } catch (e) {
                            console.error("Error parsing JSON from API:", e, "Raw text:", textOutput);
                            return { error: "Failed to parse AI analysis.", details: textOutput };
                        }
                    }
                    return textOutput; 
                } else {
                    console.error("Unexpected API response structure:", result);
                     if (result.error && result.error.message) {
                        return { error: `API Error: ${result.error.message}` };
                    }
                    return { error: "Unexpected API output structure." };
                }
            } catch (error) {
                console.error("Error calling Gemini API:", error);
                return { error: "Error calling Gemini API. Check console." };
            } finally {
                targetSpinner.style.display = 'none'; 
            }
        }

        // Event listener for Analyze Content button
        analyzeContentBtn.addEventListener('click', async () => {
            const customerText = customerContentTextarea.value.trim();
            if (!customerText) {
                suggestedSentimentText.textContent = "Please enter customer content to analyze.";
                extractedKeywordsText.textContent = "";
                analysisOutputDiv.classList.remove('hidden');
                return;
            }

            analysisOutputDiv.classList.remove('hidden');
            suggestedSentimentText.textContent = "Analyzing...";
            extractedKeywordsText.textContent = "";

            const analysisPrompt = `Analyze the following customer comment for sentiment and extract key topics.
Customer Comment: "${customerText}"
Classify sentiment strictly as one of: "Negative (Complaint/Issue)", "Positive (Appreciation/Praise)", or "Neutral (Inquiry/Feedback)".
Extract up to 5 key topics or keywords.
Return the result as a JSON object with keys "suggestedSentiment" and "extractedKeywords" (an array of strings).`;

            const analysisResult = await callGeminiAPI(analysisPrompt, analysisLoadingSpinner, true);

            if (analysisResult.error) {
                suggestedSentimentText.textContent = `Analysis Error: ${analysisResult.error}`;
                if(analysisResult.details) extractedKeywordsText.textContent = `Details: ${analysisResult.details}`;
            } else if (analysisResult.suggestedSentiment && analysisResult.extractedKeywords) {
                suggestedSentimentText.textContent = `Suggested Sentiment: ${analysisResult.suggestedSentiment}`;
                
                let sentimentMatched = false;
                for (let i = 0; i < contentSentimentSelect.options.length; i++) {
                    if (contentSentimentSelect.options[i].value === analysisResult.suggestedSentiment) {
                        contentSentimentSelect.selectedIndex = i;
                        sentimentMatched = true;
                        break;
                    }
                }
                if (!sentimentMatched) {
                    console.warn("Suggested sentiment from API doesn't match any dropdown option:", analysisResult.suggestedSentiment);
                }

                if (analysisResult.extractedKeywords.length > 0) {
                    extractedKeywordsText.textContent = `Keywords: ${analysisResult.extractedKeywords.join(', ')}`;
                } else {
                    extractedKeywordsText.textContent = "Keywords: None extracted.";
                }
            } else {
                suggestedSentimentText.textContent = "Analysis failed to return expected data.";
                extractedKeywordsText.textContent = "";
                console.error("Unexpected analysis result structure:", analysisResult);
            }
        });


        // Event listener for the Generate Reply button
        generateReplyBtn.addEventListener('click', async () => {
            const platform = socialPlatformSelect.value;
            const language = replyLanguageSelect.value; 
            const customerContent = customerContentTextarea.value;
            const sentiment = contentSentimentSelect.value; 
            const replyInstructions = document.getElementById('replyInstructions').value.trim();
            
            if (!customerContent.trim()) {
                generatedReplyText.textContent = `Please paste the customer's content for ${platform} to generate a reply.`;
                generatedReplyOutput.classList.remove('hidden');
                return;
            }
            generatedReplyOutput.classList.remove('hidden');
            generatedReplyText.textContent = "Generating reply...";

            let prompt = `You are a customer support and social media manager for Cars24india, a platform for buying and selling used cars. Your primary goal is to provide professional, empathetic, and helpful responses.
A customer/user has posted the following content on ${platform} with a sentiment of '${sentiment}':\n\n"${customerContent}"\n\n`;
            
            prompt += `Draft a concise reply in ${language}. The reply MUST adhere to the following strict guidelines:\n`;
            prompt += "1.  **Tone:** Maintain a strictly professional and courteous tone throughout the reply.\n";
            
            if (sentiment === "Negative (Complaint/Issue)" || sentiment === "Neutral (Inquiry/Feedback)" || sentiment === "Bug Report (App Stores)") {
                prompt += "2.  **Opening:** MUST begin with a sincere apology (e.g., 'We sincerely apologize for the inconvenience you've experienced.', 'We're very sorry to hear about this issue.', 'Our apologies for any trouble this has caused.').\n";
            } else {
                 prompt += "2.  **Opening:** Start with a polite and appropriate opening based on the positive sentiment (e.g., 'Thank you for your kind words!', 'We're thrilled to hear you had a great experience!').\n";
            }

            prompt += "3.  **Originality:** MUST NOT repeat significant words, phrases, or sentences from the customer's original content. Rephrase and use fresh language to show you've understood their unique situation.\n";
            prompt += "4.  **Conciseness:** Keep the reply brief and to the point, especially for platforms like Twitter.\n";

            switch(platform) {
                case 'Twitter':
                    prompt += "5.  **Twitter Specifics:** Ensure the reply is well under 280 characters. Relevant emojis or hashtags can be used sparingly if appropriate for a professional tone.\n";
                    break;
                case 'Instagram':
                    prompt += "5.  **Instagram Specifics:** Emojis can be used to add warmth if they align with a professional tone. Hashtags like #Cars24 #Cars24India can be considered if relevant.\n";
                    break;
            }
            
            let askForDetailsInstruction = "IMPORTANT: The reply should be self-contained. DO NOT ask the user to provide more details, contact via DM, or fill out any forms UNLESS explicitly instructed to do so in the 'Additional Reply Instructions' provided by the user of this tool.\n";
            const instructionToAskDetails = replyInstructions.toLowerCase().includes("ask for details") || 
                                            replyInstructions.toLowerCase().includes("request form") ||
                                            replyInstructions.toLowerCase().includes("fill the form") ||
                                            replyInstructions.toLowerCase().includes("get their details");

            if (instructionToAskDetails) {
                askForDetailsInstruction = `If the situation warrants (e.g., complaint, specific issue requiring follow-up as per the additional instructions below), and you are instructed to gather more information, politely include the following phrase: "Kindly share your detailed concern by filling out this form (https://bit.ly/CARS24Cares), and we will assist you." Do not ask for sensitive details publicly.\n`;
            }
            prompt += askForDetailsInstruction;

            if (replyInstructions) {
                prompt += `\n**Additional Reply Instructions from the social media manager (follow these carefully):** ${replyInstructions}\n`;
            }
            prompt += "\nNow, generate the reply based on all the above instructions.";

            const responseText = await callGeminiAPI(prompt, replyLoadingSpinner, false); 
            
            if (typeof responseText === 'string') {
                generatedReplyText.textContent = responseText;
            } else if (responseText.error) { 
                 generatedReplyText.textContent = `Error generating reply: ${responseText.error}`;
            } else {
                generatedReplyText.textContent = "Failed to generate reply. Unknown error.";
            }
        });

        // Ensure platform select event listener is initialized if app is already shown (e.g. after a page refresh if we were to use localStorage for login state)
        // For now, this is mainly for completeness as login state isn't persisted.
         if (!appContainer.classList.contains('hidden')) {
            socialPlatformSelect.dispatchEvent(new Event('change'));
        }

    </script>

</body>
</html>
