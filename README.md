# CSS3 Transitions, Animations, and Advanced JavaScript Functions

## Objectives

Create smooth CSS transitions and animations.
Use JavaScript functions for dynamic behavior.
Implement local storage for data persistence.

## Instructions
Add CSS animations to elements like buttons or images.

>[!NOTE]
> - Write a JavaScript function that:
> - Stores and retrieves user preferences using localStorage.
> - Implements an animation triggered by user actions.

## Tasks

Create a CSS animation.
Store data in localStorage.
Apply JavaScript to trigger animations.

Happy Coding! 💻✨



<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Animations Demo</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Animation Preferences</h1>
        
        <div class="preference-panel">
            <div class="theme-selector">
                <h2>Select Theme</h2>
                <button id="light-theme" class="theme-btn">Light</button>
                <button id="dark-theme" class="theme-btn">Dark</button>
                <button id="blue-theme" class="theme-btn">Blue</button>
            </div>
            
            <div class="animation-controls">
                <h2>Animation Preferences</h2>
                <label>
                    <input type="checkbox" id="bounce-animation"> Enable Bounce Animation
                </label>
                <label>
                    <input type="checkbox" id="rotate-animation"> Enable Rotate Animation
                </label>
                <label>
                    Animation Speed:
                    <input type="range" id="animation-speed" min="0.5" max="3" step="0.1" value="1">
                </label>
            </div>
        </div>
        
        <div class="demo-area">
            <h2>Demo Elements</h2>
            <button id="animate-btn" class="action-btn">Click to Animate</button>
            <div id="animated-box" class="box"></div>
            <img id="animated-image" src="https://via.placeholder.com/150" alt="Demo image" class="image">
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>




/* Base styles */
body {
    font-family: 'Arial', sans-serif;
    margin: 0;
    padding: 20px;
    transition: background-color 0.5s ease, color 0.5s ease;
}

.container {
    max-width: 800px;
    margin: 0 auto;
}

/* Theme styles */
.light-theme {
    background-color: #f5f5f5;
    color: #333;
}

.dark-theme {
    background-color: #333;
    color: #f5f5f5;
}

.blue-theme {
    background-color: #e6f2ff;
    color: #003366;
}

/* Button styles */
.theme-btn, .action-btn {
    padding: 10px 15px;
    margin: 5px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.3s ease;
}

.theme-btn:hover, .action-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.light-theme .theme-btn {
    background-color: #ddd;
    color: #333;
}

.dark-theme .theme-btn {
    background-color: #555;
    color: #fff;
}

.blue-theme .theme-btn {
    background-color: #66a3ff;
    color: #fff;
}

.action-btn {
    background-color: #4CAF50;
    color: white;
}

/* Demo elements */
.box {
    width: 100px;
    height: 100px;
    background-color: #4CAF50;
    margin: 20px 0;
    transition: all 0.3s ease;
}

.image {
    margin: 20px 0;
    transition: all 0.3s ease;
}

/* Animation classes */
.bounce {
    animation: bounce 0.5s;
}

.rotate {
    animation: rotate 1s;
}

@keyframes bounce {
    0%, 100% { transform: translateY(0) scale(1); }
    50% { transform: translateY(-20px) scale(1.1); }
}

@keyframes rotate {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

/* Layout */
.preference-panel, .demo-area {
    padding: 20px;
    margin-bottom: 20px;
    border-radius: 8px;
}

.light-theme .preference-panel,
.light-theme .demo-area {
    background-color: white;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.dark-theme .preference-panel,
.dark-theme .demo-area {
    background-color: #444;
    box-shadow: 0 2px 5px rgba(0,0,0,0.3);
}

.blue-theme .preference-panel,
.blue-theme .demo-area {
    background-color: #cce0ff;
    box-shadow: 0 2px 5px rgba(0,0,0,0.2);
}




document.addEventListener('DOMContentLoaded', function() {
    // DOM Elements
    const themeButtons = document.querySelectorAll('.theme-btn');
    const animateBtn = document.getElementById('animate-btn');
    const animatedBox = document.getElementById('animated-box');
    const animatedImage = document.getElementById('animated-image');
    const bounceCheckbox = document.getElementById('bounce-animation');
    const rotateCheckbox = document.getElementById('rotate-animation');
    const speedSlider = document.getElementById('animation-speed');
    
    // Load saved preferences
    loadPreferences();
    
    // Theme selection
    themeButtons.forEach(button => {
        button.addEventListener('click', function() {
            const theme = this.id.replace('-theme', '');
            setTheme(theme);
            savePreferences();
        });
    });
    
    // Animation button
    animateBtn.addEventListener('click', function() {
        // Clear previous animations
        animatedBox.style.animation = '';
        animatedImage.style.animation = '';
        
        // Trigger reflow to restart animations
        void animatedBox.offsetWidth;
        void animatedImage.offsetWidth;
        
        // Apply animations based on preferences
        const speed = parseFloat(speedSlider.value);
        
        if (bounceCheckbox.checked) {
            animatedBox.style.animation = `bounce ${0.5/speed}s`;
            animatedBox.classList.add('bounce');
        }
        
        if (rotateCheckbox.checked) {
            animatedImage.style.animation = `rotate ${1/speed}s`;
            animatedImage.classList.add('rotate');
        }
    });
    
    // Save preferences when checkboxes change
    bounceCheckbox.addEventListener('change', savePreferences);
    rotateCheckbox.addEventListener('change', savePreferences);
    speedSlider.addEventListener('input', savePreferences);
    
    // Theme setting function
    function setTheme(theme) {
        document.body.className = theme + '-theme';
    }
    
    // Save preferences to localStorage
    function savePreferences() {
        const preferences = {
            theme: document.body.className.replace('-theme', ''),
            bounce: bounceCheckbox.checked,
            rotate: rotateCheckbox.checked,
            speed: speedSlider.value
        };
        
        localStorage.setItem('animationPreferences', JSON.stringify(preferences));
    }
    
    // Load preferences from localStorage
    function loadPreferences() {
        const saved = localStorage.getItem('animationPreferences');
        if (saved) {
            const preferences = JSON.parse(saved);
            
            // Apply theme
            if (preferences.theme) {
                setTheme(preferences.theme);
            }
            
            // Apply animation preferences
            if (preferences.bounce !== undefined) {
                bounceCheckbox.checked = preferences.bounce;
            }
            
            if (preferences.rotate !== undefined) {
                rotateCheckbox.checked = preferences.rotate;
            }
            
            if (preferences.speed !== undefined) {
                speedSlider.value = preferences.speed;
            }
        }
    }
});
