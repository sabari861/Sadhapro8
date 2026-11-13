<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS Measurement & Color Tool</title>

    <style>
        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #f2f4f7;
            color: #222;
        }

        header {
            background: #2563eb;
            color: white;
            text-align: center;
            padding: 20px;
        }

        .container {
            max-width: 1100px;
            margin: 30px auto;
            padding: 20px;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 25px;
        }

        .panel {
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        h2 {
            margin-top: 0;
            color: #2563eb;
        }

        .control {
            margin-bottom: 18px;
        }

        label {
            display: block;
            margin-bottom: 6px;
            font-weight: bold;
        }

        input[type="number"],
        input[type="text"] {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 6px;
        }

        input[type="color"] {
            width: 70px;
            height: 40px;
            border: none;
            cursor: pointer;
        }

        .preview-area {
            min-height: 350px;
            display: flex;
            justify-content: center;
            align-items: center;
            background: #e5e7eb;
            border-radius: 10px;
            padding: 20px;
        }

        #preview {
            width: 250px;
            height: 150px;
            background-color: #60a5fa;
            color: #ffffff;
            padding: 20px;
            border: 5px solid #1d4ed8;
            border-radius: 10px;
            text-align: center;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .color-row {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        textarea {
            width: 100%;
            height: 180px;
            padding: 15px;
            font-family: monospace;
            background: #111827;
            color: #22c55e;
            border-radius: 8px;
            resize: none;
        }

        button {
            margin-top: 10px;
            padding: 12px 20px;
            border: none;
            border-radius: 6px;
            background: #2563eb;
            color: white;
            cursor: pointer;
            font-size: 15px;
        }

        button:hover {
            background: #1d4ed8;
        }

        @media (max-width: 750px) {
            .container {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>

<body>

<header>
    <h1>CSS Measurement & Color Tool</h1>
    <p>Change the values and see the preview instantly</p>
</header>

<div class="container">

    <!-- Controls -->
    <div class="panel">
        <h2>CSS Controls</h2>

        <div class="control">
            <label for="width">Width (px)</label>
            <input type="number" id="width" value="250">
        </div>

        <div class="control">
            <label for="height">Height (px)</label>
            <input type="number" id="height" value="150">
        </div>

        <div class="control">
            <label for="padding">Padding (px)</label>
            <input type="number" id="padding" value="20">
        </div>

        <div class="control">
            <label for="margin">Margin (px)</label>
            <input type="number" id="margin" value="10">
        </div>

        <div class="control">
            <label for="border">Border Width (px)</label>
            <input type="number" id="border" value="5">
        </div>

        <div class="control">
            <label for="radius">Border Radius (px)</label>
            <input type="number" id="radius" value="10">
        </div>

        <div class="control">
            <label>Background Color</label>
            <div class="color-row">
                <input type="color" id="bgColor" value="#60a5fa">
                <input type="text" id="bgText" value="#60a5fa">
            </div>
        </div>

        <div class="control">
            <label>Text Color</label>
            <div class="color-row">
                <input type="color" id="textColor" value="#ffffff">
                <input type="text" id="textText" value="#ffffff">
            </div>
        </div>

        <div class="control">
            <label>Border Color</label>
            <div class="color-row">
                <input type="color" id="borderColor" value="#1d4ed8">
                <input type="text" id="borderText" value="#1d4ed8">
            </div>
        </div>
    </div>

    <!-- Preview -->
    <div class="panel">
        <h2>Live Preview</h2>

        <div class="preview-area">
            <div id="preview">
                CSS Preview
            </div>
        </div>

        <h2>Generated CSS</h2>

        <textarea id="cssCode" readonly></textarea>

        <button onclick="copyCSS()">Copy CSS</button>
        <button onclick="resetTool()">Reset</button>
    </div>

</div>

<script>
    const inputs = document.querySelectorAll(
        '#width, #height, #padding, #margin, #border, #radius, #bgColor, #textColor, #borderColor'
    );

    const preview = document.getElementById("preview");
    const cssCode = document.getElementById("cssCode");

    function updatePreview() {

        const width = document.getElementById("width").value;
        const height = document.getElementById("height").value;
        const padding = document.getElementById("padding").value;
        const margin = document.getElementById("margin").value;
        const border = document.getElementById("border").value;
        const radius = document.getElementById("radius").value;

        const bgColor = document.getElementById("bgColor").value;
        const textColor = document.getElementById("textColor").value;
        const borderColor = document.getElementById("borderColor").value;

        preview.style.width = width + "px";
        preview.style.height = height + "px";
        preview.style.padding = padding + "px";
        preview.style.margin = margin + "px";
        preview.style.border = border + "px solid " + borderColor;
        preview.style.borderRadius = radius + "px";
        preview.style.backgroundColor = bgColor;
        preview.style.color = textColor;

        cssCode.value =
`.preview {
    width: ${width}px;
    height: ${height}px;
    padding: ${padding}px;
    margin: ${margin}px;
    border: ${border}px solid ${borderColor};
    border-radius: ${radius}px;
    background-color: ${bgColor};
    color: ${textColor};
}`;
    }

    inputs.forEach(input => {
        input.addEventListener("input", updatePreview);
    });

    // Synchronize color picker and text field
    document.getElementById("bgColor").addEventListener("input", function() {
        document.getElementById("bgText").value = this.value;
        updatePreview();
    });

    document.getElementById("textColor").addEventListener("input", function() {
        document.getElementById("textText").value = this.value;
        updatePreview();
    });

    document.getElementById("borderColor").addEventListener("input", function() {
        document.getElementById("borderText").value = this.value;
        updatePreview();
    });

    function copyCSS() {
        navigator.clipboard.writeText(cssCode.value);
        alert("CSS copied to clipboard!");
    }

    function resetTool() {
        document.getElementById("width").value = 250;
        document.getElementById("height").value = 150;
        document.getElementById("padding").value = 20;
        document.getElementById("margin").value = 10;
        document.getElementById("border").value = 5;
        document.getElementById("radius").value = 10;

        document.getElementById("bgColor").value = "#60a5fa";
        document.getElementById("textColor").value = "#ffffff";
        document.getElementById("borderColor").value = "#1d4ed8";

        document.getElementById("bgText").value = "#60a5fa";
        document.getElementById("textText").value = "#ffffff";
        document.getElementById("borderText").value = "#1d4ed8";

        updatePreview();
    }

    updatePreview();
</script>

</body>
</html>
