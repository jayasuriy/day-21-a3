<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
 
        <title>states</title>
 
        <link rel="stylesheet" href="" />
        <style>
            body {
                background: lightcoral;
                color: #323232;
                font-weight: 300;
                height: 100vh;
                margin: 0;
                display: flex;
                align-items: center;
                justify-content: center;
                text-align: center;
                font-family: Helvetica neue, roboto;
            }
            .button:hover {
                background-color: aquamarine;
            }
            .btn-group .button {
                background-color: bisque;
                border: 1px solid black;
                color: black;
                padding: 15px 32px;
                text-align: center;
                text-decoration: none;
                font-size: 16px;
                cursor: pointer;
                width: 150px;
                display: block;
                margin: 4px 2px;
            }
            .button:hover {
                background-color: whitesmoke;
            }
 
            h1 {
                font-weight: 200;
                font-style: 26px;
                margin: 10px;
            }
        </style>
    </head>
 
    <body>
        <div class="btn-group">
            <button id="start" class="button">
              Start Audio
            </button>
            <button id="sus" class="button">
              Suspend Audio
            </button>
            <button id="stop" class="button">
              Stop Audio
            </button>
             
 
<p>Current context time: No context exists.</p>
 
 
        </div>
 
        <script>
            let AudioContext;
 
            const start = document.getElementById("start");
            const susres = document.getElementById("sus");
            const stop = document.getElementById("stop");
 
            const timeDisplay = document.querySelector("p");
 
            susres.setAttribute("disabled", "disabled");
            stop.setAttribute("disabled", "disabled");
 
            start.onclick = function () {
                start.setAttribute("disabled", "disabled");
                susres.removeAttribute("disabled");
                stop.removeAttribute("disabled");
 
                // Create web audio api context
                AudioContext = window.AudioContext
                      || window.webkitAudioContext;
                AudioContext = new AudioContext();
 
                // Create Oscillator and filter
                const oscillator = AudioContext.createOscillator();
                const filter = AudioContext.createBiquadFilter();
 
                // Connect oscillator to filter to speakers
                oscillator.connect(filter);
                filter.connect(AudioContext.destination);
 
                // Make audio/noise
                oscillator.type = "sine";
 
                // hertz frequency
                oscillator.frequency.value = 100;
                oscillator.start(0);
            };
 
            // Suspend/resume the audiocontext,i.e,
            // the audio can be played back
            susres.onclick = function () {
                if (AudioContext.state === "running") {
                    AudioContext.suspend().then(function () {
                        susres.textContent = "Resume Audio";
                    });
                } else if (AudioContext.state === "suspended") {
                    AudioContext.resume().then(function () {
                        susres.textContent = "Suspend Audio";
                    });
                }
            };
 
            // Close the audiocontext,i.e, the audio is
            // completely stopped after the stop button
            // is clicked by promise the audio resets
            // the response to beginning state(Create Audio)
            stop.onclick = function () {
                AudioContext.close().then(function () {
                    start.removeAttribute("disabled");
                    susres.setAttribute("disabled", "disabled");
                    stop.setAttribute("disabled", "disabled");
                });
            };
 
            function displayTime() {
                if (AudioContext && AudioContext.state !== "closed")
                {
                    timeDisplay.textContent = "audio time "
                    + AudioContext.currentTime.toFixed(3);
                }
                else
                {
                    timeDisplay.textContent = "Context not started";
                }
                requestAnimationFrame(displayTime);
            }
 
            displayTime();
        </script>
    </body>
</html>
