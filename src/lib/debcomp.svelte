<!-- App.svelte -->
<script>
    import { onMount } from 'svelte';
    import { GoogleGenerativeAI } from "@google/generative-ai"; // Import the AI library
    import { marked } from 'marked'; // Import the marked library

    const API_KEY = "AIzaSyDKv4gjBMYe_OszgWMz7Lcns4900oVBhP0"; // Replace with your actual API key
    const genAI = new GoogleGenerativeAI(API_KEY);

    let messages = [];
    let userInput = '';
    let isLoading = false;
    let currentSpeaker = 'Socrates'; 
    let debateActive = false; 
    let topic = 'Philosophical Debate'; // Define the topic
    let debateHistory = []; // Store the history of the debate for printing

    // Define prompts for each speaker
    const socratesPrompt = `You are Socrates, employing the Socratic method to challenge and question. Focus on insightful questions, exposing weaknesses in logic and assumptions. Respond concisely (under 100 words).`;
    const aristotlePrompt = `You are Aristotle, responding logically and providing well-reasoned arguments based on your vast knowledge and experience. Respond concisely (under 100 words).`;

    async function getAIResponse(input, speaker) {
        try {
            const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash-002" });
            const prompt = speaker === 'Socrates' ? socratesPrompt : aristotlePrompt;
            const result = await model.generateContent(`${prompt}\n${input}`);
            return result.response.text(); // Return the generated response
        } catch (error) {
            console.error("Error generating response:", error);
            return "Sorry, I encountered an error. Please try again.";
        }
    }

    // Function to get the appropriate voice based on the speaker
    function getVoiceForSpeaker(speaker) {
        const voices = window.speechSynthesis.getVoices();
        if (speaker === 'Socrates') {
            // Select an older voice for Socrates
            return voices.find(voice => voice.name.includes('Microsoft Ravi - English (India) - en-IN')); // Fallback to another voice
        } else if (speaker === 'Aristotle') {
            // Select a different voice for Aristotle
            return voices.find(voice => voice.name.includes('Microsoft Mark - English (United States) - en-US')); // Fallback to another voice
        }
        return null; // Default case
    }

    function speak(text) {
        const utterance = new SpeechSynthesisUtterance(text);
        utterance.onend = () => {
            handleNextSpeaker();
        };

        // Get the appropriate voice for the current speaker
        const voice = getVoiceForSpeaker(currentSpeaker);
        if (voice) {
            utterance.voice = voice; // Set the selected voice
        }

        // Set the rate for faster speech
        utterance.rate = 1.5; // Set the speech rate to 1.25

        window.speechSynthesis.speak(utterance);
    }

    async function handleNextSpeaker() {
        if (!debateActive) return;

        const response = await getAIResponse(userInput, currentSpeaker);
        
        debateHistory.push({
            role: currentSpeaker.toLowerCase(),
            content: response,
            timestamp: new Date()
        });

        speak(response);
        currentSpeaker = currentSpeaker === 'Socrates' ? 'Aristotle' : 'Socrates';
        userInput = response; // Use the last response as the next input
    }

    async function handleSubmit() {
        if (!userInput.trim() || isLoading || debateActive) return; 
        
        isLoading = true;
        debateActive = true; 
        
        messages = [...messages, {
            role: 'user',
            content: userInput,
            timestamp: new Date()
        }];

        await handleNextSpeaker(); // Start the debate with the first response
        isLoading = false;
    }

    function stopDebate() {
        debateActive = false; // Stop the debate
        window.speechSynthesis.cancel(); // Stop any ongoing speech
        printDebate(); // Call print function when stopping the debate
    }

    function skipCurrentSpeaker() {
        window.speechSynthesis.cancel(); // Stop the current speech
        handleNextSpeaker(); // Move to the next speaker
    }

    function printDebate() {
        const printContent = debateHistory.map(msg => `${msg.role}: ${msg.content}`).join('\n');
        const printWindow = window.open('', '', 'height=400,width=600');
        printWindow.document.write('<pre>' + printContent + '</pre>');
        printWindow.document.close();
        printWindow.print();
    }

    function renderMarkdown(content) {
        return marked(content);
    }
</script>

<main class="container mx-auto flex flex-col items-center justify-center min-h-screen px-4 py-8">
    <h1 class="text-3xl font-bold text-center mb-6">{topic}</h1> <!-- Display topic -->

    <div class="flex gap-2 mb-6"> <!-- Added margin-bottom for spacing -->
        <input
            type="text"
            bind:value={userInput}
            placeholder="Join the philosophical debate..."
            class="input-box" 
            on:keydown={e => e.key === 'Enter' && handleSubmit()}
            disabled={isLoading || debateActive} 
        />
        <button
            on:click={handleSubmit}
            disabled={isLoading || debateActive} 
            class="button-primary"
        >
            Start Debate
        </button>
        <button
            on:click={stopDebate}
            disabled={!debateActive} 
            class="button-secondary"
        >
            Stop Debate
        </button>
        <button
            on:click={skipCurrentSpeaker}
            disabled={!debateActive} 
            class="button-skip"
        >
            Skip
        </button>
    </div>

    {#if debateActive}
        <div class="text-white text-4xl text-center mx-4 my-4"> <!-- Adjusted text size -->
            {@html renderMarkdown(userInput || debateHistory[debateHistory.length - 1]?.content || '')} <!-- Show current response -->
        </div>
    {:else}
        <div class="text-center text-gray-400">Debate has ended.</div> <!-- Changed to gray for visibility -->
    {/if}

    <div class="mt-4 text-center text-sm text-gray-400"> <!-- Changed to gray for visibility -->
        Current Speaker: {currentSpeaker}
    </div>
</main>

<style>
    :global(body) {
        background-color: #121212; /* Set background to dark gray */
        color: #e0e0e0; /* Set default text color to light gray */
        font-family: 'Arial', sans-serif; /* Set a clean font */
        margin: 0; /* Remove default margin */
        display: flex; /* Use flexbox for centering */
        justify-content: center; /* Center horizontally */
        align-items: center; /* Center vertically */
        height: 100vh; /* Full viewport height */
    }
    .container {
    margin-left: 400px; /* Adjust as needed */
    margin-right: 400px; /* Adjust as needed */
    max-width: calc(100% - 40px); /* Ensure the container fits within the margins */
}

    .text-white {
        color: #e0e0e0; /* Ensure text remains light gray */
    }
    .input-box {
        flex: 1;
        padding: 12px;
        border: 1px solid #444;
        border-radius: 8px;
        background-color: #1f1f1f;
        color: #e0e0e0;
        transition: border-color 0.3s ease, box-shadow 0.3s ease;
    }
    .input-box::placeholder {
        color: #888; /* Placeholder color */
    }
    .input-box:focus {
        outline: none;
        border-color: #007bff; /* Blue border on focus */
        box-shadow: 0 0 5px rgba(0, 123, 255, 0.5); /* Subtle shadow */
    }
    .button-primary {
        background-color: #007bff; /* Primary button color */
        color: white;
        padding: 12px 20px;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        transition: background-color 0.3s ease, transform 0.2s ease;
    }
    .button-primary:hover {
        background-color: #0056b3; /* Darker blue on hover */
        transform: translateY(-2px); /* Lift effect */
    }
    .button-primary:disabled {
        background-color: #555; /* Disabled button color */
        cursor: not-allowed;
    }
    .button-secondary {
        background-color: #dc3545; /* Secondary button color */
        color: white;
        padding: 12px 20px;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        transition: background-color 0.3s ease, transform 0.2s ease;
    }
    .button-secondary:hover {
        background-color: #c82333; /* Darker red on hover */
        transform: translateY(-2px); /* Lift effect */
    }
    .button-secondary:disabled {
        background-color: #555; /* Disabled button color */
        cursor: not-allowed;
    }
    .button-skip {
        background-color: #ffc107; /* Skip button color */
        color: white;
        padding: 12px 20px;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        transition: background-color 0.3s ease, transform 0.2s ease;
    }
    .button-skip:hover {
        background-color: #e0a800; /* Darker yellow on hover */
        transform: translateY(-2px); /* Lift effect */
    }
    .button-skip:disabled {
        background-color: #555; /* Disabled button color */
        cursor: not-allowed;
    }
    .text-4xl {
        text-align: center; /* Center the text */
        margin: 0 auto; /* Center the div itself */
        font-size: 1.25rem; /* Adjust font size for large text */
        font-weight: bolder;
    }

    .bust-container {
        position: absolute; /* Position it absolutely */
        top: 20px; /* Adjust as needed */
        left: 50%; /* Center horizontally */
        transform: translateX(-50%); /* Center adjustment */
        z-index: 20; /* Higher z-index to bring it on top */
    }

    .debate-container {
        position: relative; /* Ensure it can be positioned */
        z-index: 10; /* Lower z-index than Bust */
    }
</style>
