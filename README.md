# Seal-seed
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Power Total Calculator</title>
  <script src="https://cdn.jsdelivr.net/npm/tesseract.js@2.1.4/dist/tesseract.min.js"></script>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      margin-top: 40px;
    }
    #result {
      margin-top: 20px;
      font-size: 24px;
    }
  </style>
</head>
<body>
  <h1>Power Total Calculator</h1>
  <input type="file" id="imageInput" multiple accept="image/*">
  <div id="result">Total: --</div>

  <script>
    const input = document.getElementById('imageInput');
    const result = document.getElementById('result');

    input.addEventListener('change', async () => {
      const files = Array.from(input.files);
      let total = 0;

      for (const file of files) {
        const image = URL.createObjectURL(file);
        const { data: { text } } = await Tesseract.recognize(image, 'eng');
        const numbers = text.match(/\d{1,3}(?:,\d{3})+/g) || [];

        numbers.forEach(num => {
          const cleaned = parseInt(num.replace(/,/g, ''), 10);
          if (!isNaN(cleaned)) total += cleaned;
        });
      }

      result.textContent = `Total: ${total.toLocaleString()}`;
    });
  </script>
</body>
</html>
