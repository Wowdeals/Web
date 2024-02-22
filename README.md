<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PZDeals</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Welcome to PZDeals</h1>
    </header>
    <main>
        <input type="text" id="linkInput" placeholder="Paste your link here">
        <input type="file" id="imageInput">
        <button onclick="submitDeal()">Submit Deal</button>
    </main>
    <footer>
        <!-- Footer content here -->
    </footer>
    <script src="script.js"></script>
</body>
</html>
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(express.static('public'));
app.use(express.json());

// Routes
app.post('/submit-deal', (req, res) => {
    const link = req.body.link;
    const image = req.body.image; // Assuming image is sent as base64 encoded string
    // Save link and image to database or perform other actions
    res.status(200).json({ message: 'Deal submitted successfully' });
});

// Start server
app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
async function submitDeal() {
    const link = document.getElementById('linkInput').value;
    const image = await getBase64Image();
    const response = await fetch('/submit-deal', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({ link, image })
    });
    const data = await response.json();
    alert(data.message);
}

function getBase64Image() {
    return new Promise((resolve, reject) => {
        const file = document.getElementById('imageInput').files[0];
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => resolve(reader.result.split(',')[1]);
        reader.onerror = error => reject(error);
    });
}
