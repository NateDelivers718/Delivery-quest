# Delivery-quest
NYC delivery adventure game
<!-- delivery-game.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>NYC Delivery Adventure</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        /* CSS Styles */
        body {
            background-color: #f2f2f2;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
        }
        #game-container {
            max-width: 800px;
            margin: auto;
            background-color: #ffffff;
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 0 10px #ccc;
        }
        h1 {
            text-align: center;
            color: #333;
        }
        #scene-image {
            width: 100%;
            border-radius: 10px;
            margin-bottom: 20px;
        }
        #story {
            font-size: 1.2em;
            color: #555;
        }
        .option-button {
            display: block;
            width: 100%;
            margin: 10px 0;
            padding: 15px;
            font-size: 1em;
            color: #fff;
            background-color: #007BFF;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        .option-button:hover {
            background-color: #0056b3;
        }
        @media (max-width: 600px) {
            #game-container {
                padding: 15px;
            }
            #story {
                font-size: 1.1em;
            }
            .option-button {
                padding: 12px;
                font-size: 0.9em;
            }
        }
    </style>
</head>
<body>

    <div id="game-container">
        <h1>NYC Delivery Adventure</h1>
        <img id="scene-image" src="" alt="Scene Image">
        <p id="story"></p>
        <div id="options"></div>
    </div>

    <script>
        // JavaScript Code

        const scenes = {
            start: {
                text: "Welcome to your first day as a delivery driver in bustling NYC! Your van is loaded, and your GPS is set. What's your first move?",
                image: "",
                options: [
                    { text: "Review customer notes before starting.", nextScene: "reviewNotes" },
                    { text: "Head straight to the first address.", nextScene: "headToFirstAddress" },
                    { text: "Grab a coffee to start the day.", nextScene: "grabCoffee" },
                ]
            },
            reviewNotes: {
                text: "Smart choice! You notice that the first customer requested a contactless delivery and mentioned a side entrance.",
                image: "",
                options: [
                    { text: "Proceed to the first delivery with notes in mind.", nextScene: "firstDelivery" },
                    { text: "Call the customer for clarification.", nextScene: "callCustomer" },
                    { text: "Ignore the notes and proceed.", nextScene: "ignoreNotes" },
                ]
            },
            headToFirstAddress: {
                text: "You head out but realize traffic is heavier than usual. Do you:",
                image: "",
                options: [
                    { text: "Stick to the GPS route.", nextScene: "gpsRoute" },
                    { text: "Take a shortcut you know.", nextScene: "shortcut" },
                    { text: "Pull over to check for alternative routes.", nextScene: "checkRoutes" },
                ]
            },
            grabCoffee: {
                text: "A quick caffeine boost sounds good. However, you notice a parking enforcement officer nearby.",
                image: "",
                options: [
                    { text: "Park illegally for a moment.", nextScene: "parkingTicket" },
                    { text: "Find legal parking further away.", nextScene: "legalParking" },
                    { text: "Skip the coffee and start deliveries.", nextScene: "startDeliveries" },
                ]
            },
            firstDelivery: {
                text: "You arrive at the first address. Remembering the notes, you look for the side entrance.",
                image: "",
                options: [
                    { text: "Leave the package at the main door.", nextScene: "wrongDropOff" },
                    { text: "Deliver to the side entrance as requested.", nextScene: "correctDropOff" },
                    { text: "Call the customer to confirm.", nextScene: "confirmWithCustomer" },
                ]
            },
            correctDropOff: {
                text: "You leave the package at the side entrance and mark the delivery as complete. Great job following instructions!",
                image: "",
                options: [
                    { text: "Proceed to the next delivery.", nextScene: "nextDelivery" },
                ]
            },
            wrongDropOff: {
                text: "Later, the customer reports a missing package. It was taken from the main door. Remember to follow customer notes!",
                image: "",
                options: [
                    { text: "Return to the last delivery.", nextScene: "returnToDelivery" },
                    { text: "Proceed to the next delivery.", nextScene: "nextDelivery" },
                ]
            },
            // Additional scenes...

            nextDelivery: {
                text: "Your next delivery is in a busy part of Manhattan. The customer notes mention not to ring the doorbell because of a sleeping baby.",
                image: "",
                options: [
                    { text: "Leave the package and ring the bell.", nextScene: "ringBell" },
                    { text: "Quietly leave the package and depart.", nextScene: "quietDelivery" },
                    { text: "Call the customer upon arrival.", nextScene: "callUponArrival" },
                ]
            },
            quietDelivery: {
                text: "You carefully place the package at the door and tiptoe away. The customer appreciates your attentiveness.",
                image: "",
                options: [
                    { text: "Continue to your next delivery.", nextScene: "anotherDelivery" },
                ]
            },
            parkingTicket: {
                text: "While you're grabbing coffee, you get a parking ticket. This will affect your performance report.",
                image: "",
                options: [
                    { text: "Accept it and move on.", nextScene: "acceptTicket" },
                    { text: "Try to dispute it.", nextScene: "disputeTicket" },
                ]
            },
            // More scenes and outcomes...

            gameEnd: {
                text: "Congratulations on completing your deliveries for the day! You learned the importance of following customer notes and navigating NYC's challenges.",
                image: "",
                options: [
                    { text: "Play Again", nextScene: "start" },
                ]
            },
        };

        function displayScene(sceneKey) {
            const scene = scenes[sceneKey];
            document.getElementById('story').innerText = scene.text;
            if (scene.image) {
                document.getElementById('scene-image').src = scene.image;
                document.getElementById('scene-image').style.display = 'block';
            } else {
                document.getElementById('scene-image').style.display = 'none';
            }
            const optionsDiv = document.getElementById('options');
            optionsDiv.innerHTML = '';
            scene.options.forEach(option => {
                const button = document.createElement('button');
                button.innerText = option.text;
                button.className = 'option-button';
                button.onclick = () => {
                    if (scenes[option.nextScene]) {
                        displayScene(option.nextScene);
                    } else {
                        displayEndScene(option.nextScene);
                    }
                };
                optionsDiv.appendChild(button);
            });
        }

        function displayEndScene(message) {
            document.getElementById('story').innerText = message;
            document.getElementById('scene-image').style.display = 'none';
            document.getElementById('options').innerHTML = '';
            const restartButton = document.createElement('button');
            restartButton.innerText = "Play Again";
            restartButton.className = 'option-button';
            restartButton.onclick = () => displayScene('start');
            document.getElementById('options').appendChild(restartButton);
        }

        // Start the game
        displayScene('start');
    </script>

</body>
</html>
anotherDelivery: {
    text: "Your next delivery is in Brooklyn. The customer notes mention a gate code: 2468.",
    image: "",
    options: [
        { text: "Use the gate code to enter.", nextScene: "gateEntry" },
        { text: "Climb over the fence.", nextScene: "climbFence" },
        { text: "Leave the package outside.", nextScene: "leaveOutside" },
    ]
},
gateEntry: {
    text: "You enter the gate code and gain access. You deliver the package safely to the customer's doorstep.",
    image: "",
    options: [
        { text: "Proceed to the next delivery.", nextScene: "nextDelivery2" },
    ]
},
climbFence: {
    text: "You attempt to climb the fence, but injure yourself in the process. Always use provided access codes!",
    image: "",
    options: [
        { text: "Report the injury.", nextScene: "reportInjury" },
        { text: "Continue working through the pain.", nextScene: "workInjury" },
    ]
},
leaveOutside: {
    text: "You leave the package outside the gate. Later, it's reported missing. Always follow delivery instructions!",
    image: "",
    options: [
        { text: "Proceed to the next delivery.", nextScene: "nextDelivery2" },
    ]
},
let score = 0;

function displayScene(sceneKey) {
    const scene = scenes[sceneKey];
    document.getElementById('story').innerText = scene.text;
    // Rest of the code...

    // Adjust score based on choices
    // Example:
    if (sceneKey === 'correctDropOff') {
        score += 10;
    } else if (sceneKey === 'wrongDropOff') {
        score -= 5;
    }

    // Rest of the function...
}

function displayEndScene(message) {
    document.getElementById('story').innerText = message + "\nYour total score is: " + score;
    // Rest of the code...
}
