# Code-VoiceToTextInPega
// Show speech status
function showStatus() {
  const MicrophoneStatusDiv = document.querySelector('[data-tour-id="MicrophoneStatus"]');
  const MicrophoneStatus = MicrophoneStatusDiv.querySelector('.field-item');
  MicrophoneStatus.innerHTML = "Listening...";
}

// Hide speech status
function hideStatus() {
  const MicrophoneStatusDiv = document.querySelector('[data-tour-id="MicrophoneStatus"]');
  const MicrophoneStatus = MicrophoneStatusDiv.querySelector('.field-item');
  MicrophoneStatus.innerHTML = "";
}

// Set interim transcript
function setInterim(val) {
  const speechInterimDiv = document.querySelector('[data-tour-id="speechInterim"]');
  const MicrophoneStatus = speechInterimDiv.querySelector('.field-item');
  MicrophoneStatus.innerHTML = val;
}

// Set final transcript
function setFinal(val) {
  const TextConverter = document.querySelector('[data-tour-id="TextConverter"]');
  const TextConverterTA = TextConverter.querySelector('textarea');
  TextConverterTA.value = val;
}

// Speech Recognition API
if ("webkitSpeechRecognition" in window) {
  const speechRecognition = new webkitSpeechRecognition();
  let final_transcript = "";

  // Speech Recognition properties
  speechRecognition.continuous = true;
  speechRecognition.interimResults = true;

  // Event: Start
  speechRecognition.onstart = () => {
    showStatus();
  };

  // Event: End
  speechRecognition.onend = () => {
    hideStatus();
  };

  // Event: Error
  speechRecognition.onerror = (event) => {
    hideStatus();
    console.error(`Error - ${event.error}`);
  };

  // Event: Result
  speechRecognition.onresult = (event) => {
    let interim_transcript = "";

    for (let i = event.resultIndex; i < event.results.length; ++i) {
      if (event.results[i].isFinal) {
        final_transcript += event.results[i][0].transcript;
      } else {
        interim_transcript += event.results[i][0].transcript;
      }
    }

    setFinal(final_transcript);
    setInterim(interim_transcript);
  };

  // Start speech
  function Speak() {
    speechRecognition.start();
  }

  // Stop speech
  function Stop() {
    speechRecognition.stop();
  }

  // Set up UI button event handlers on page load
  window.addEventListener("load", () => {
    const startBtn = document.querySelector('[data-tour-id="Speak"] button');
    const stopBtn = document.querySelector('[data-tour-id="Stop"] button');

    if (startBtn) {
      startBtn.onclick = Speak;
    }

    if (stopBtn) {
      stopBtn.onclick = Stop;
    }
  });
}
//static-content-hash-trigger-GCC
