<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>E-Signature Form</title>
    <style>
        canvas { border: 1px solid black; }
        form { max-width: 500px; margin: 20px auto; }
        input, button { display: block; margin: 10px 0; width: 100%; }
    </style>
</head>
<body>
    <form id="eSignForm">
        <input type="text" id="name" placeholder="Full Name" required>
        <input type="email" id="email" placeholder="Email" required>
        <input type="tel" id="phone" placeholder="Phone Number" required>
        <label>Signature:</label>
        <canvas id="signaturePad" width="400" height="200"></canvas>
        <button type="button" onclick="clearSignature()">Clear Signature</button>
        <button type="submit">Submit</button>
    </form>

    <script>
        const canvas = document.getElementById('signaturePad');
        const ctx = canvas.getContext('2d');
        let drawing = false;

        canvas.addEventListener('mousedown', () => drawing = true);
        canvas.addEventListener('mouseup', () => drawing = false);
        canvas.addEventListener('mousemove', draw);
        canvas.addEventListener('touchstart', () => drawing = true);
        canvas.addEventListener('touchend', () => drawing = false);
        canvas.addEventListener('touchmove', drawTouch);

        function draw(event) {
            if (!drawing) return;
            ctx.lineWidth = 2;
            ctx.lineCap = 'round';
            ctx.strokeStyle = 'black';
            ctx.lineTo(event.offsetX, event.offsetY);
            ctx.stroke();
        }

        function drawTouch(event) {
            if (!drawing) return;
            event.preventDefault();
            const touch = event.touches[0];
            const rect = canvas.getBoundingClientRect();
            ctx.lineTo(touch.clientX - rect.left, touch.clientY - rect.top);
            ctx.stroke();
        }

        function clearSignature() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
        }

        document.getElementById('eSignForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const phone = document.getElementById('phone').value;
            const signature = canvas.toDataURL(); // Signature as base64 image

            // Here you’d send the data (e.g., to an email or server)
            console.log({ name, email, phone, signature });
            alert('Form submitted! Check the console for data.');
        });
    </script>
</body>
</html>
