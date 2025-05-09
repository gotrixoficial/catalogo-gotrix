# catalogo-gotrix
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Catálogo Gotrix</title>
  <script src="https://unpkg.com/pdf-lib"></script>
  <style>
    :root {
      --amarelo: #FFCC00;
      --preto: #000000;
    }
    body {
      font-family: Arial, sans-serif;
      background-color: var(--preto);
      color: var(--preto);
      margin: 0;
      padding: 0;
    }
    header {
      background-color: var(--amarelo);
      padding: 20px;
      text-align: center;
    }
    header img {
      max-height: 60px;
      vertical-align: middle;
    }
    header h1 {
      margin: 10px 0 0;
      font-size: 24px;
      color: var(--preto);
    }
    main {
      padding: 20px;
    }
    p {
      color: var(--amarelo);
      font-size: 16px;
      margin-bottom: 20px;
      text-align: center;
    }
    .gallery {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 15px;
    }
    .item {
      background-color: #fff;
      padding: 10px;
      border-radius: 10px;
      text-align: center;
    }
    img {
      width: 100%;
      height: auto;
      border-radius: 6px;
    }
    input[type="checkbox"] {
      margin-top: 10px;
    }
    button {
      display: block;
      margin: 30px auto 0;
      padding: 12px 24px;
      font-size: 18px;
      background-color: var(--amarelo);
      color: var(--preto);
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
    button:hover {
      background-color: #e6b800;
    }
    @media (max-width: 600px) {
      header h1 {
        font-size: 20px;
      }
      button {
        font-size: 16px;
        padding: 10px 20px;
      }
    }
  </style>
</head>
<body>

  <header>
    <img src="catalogo/logo-gotrix.png" alt="Logo Gotrix">
    <h1>📘 Catálogo Gotrix</h1>
  </header>

  <main>
    <p>Selecione as páginas que deseja e clique no botão para gerar seu PDF personalizado.</p>

    <div class="gallery" id="imageGallery">
      <!-- As imagens são carregadas aqui -->
    </div>

    <button onclick="generatePDF()">📥 Baixar PDF com páginas selecionadas</button>
  </main>

  <script>
    // Lista de imagens do catálogo com nomes "Página X.png"
    const images = [
      'catalogo/Página 1.png',
      'catalogo/Página 2.png',
      'catalogo/Página 3.png',
      'catalogo/Página 4.png',
      'catalogo/Página 5.png',
      'catalogo/Página 6.png',
      'catalogo/Página 7.png',
      'catalogo/Página 8.png',
      'catalogo/Página 9.png',
      'catalogo/Página 10.png',
      'catalogo/Página 11.png',
      'catalogo/Página 12.png',
      'catalogo/Página 13.png',
      'catalogo/Página 14.png',
      'catalogo/Página 15.png',
      'catalogo/Página 16.png',
      'catalogo/Página 17.png',
      'catalogo/Página 18.png',
      'catalogo/Página 19.png',
      'catalogo/Página 20.png',
      'catalogo/Página 21.png',
      'catalogo/Página 22.png',
      'catalogo/Página 23.png',
      'catalogo/Página 24.png',
      'catalogo/Página 25.png',
      'catalogo/Página 26.png',
      'catalogo/Página 27.png',
      'catalogo/Página 28.png',
      'catalogo/Página 29.png',
      'catalogo/Página 30.png',
      'catalogo/Página 31.png',
      'catalogo/Página 32.png',
      'catalogo/Página 33.png',
      'catalogo/Página 34.png',
      'catalogo/Página 35.png',
      'catalogo/Página 36.png',
      'catalogo/Página 37.png',
      'catalogo/Página 38.png',
      'catalogo/Página 39.png',
      'catalogo/Página 40.png',
      'catalogo/Página 41.png',
      'catalogo/Página 42.png',
      'catalogo/Página 43.png',
      'catalogo/Página 44.png',
      'catalogo/Página 45.png',
      'catalogo/Página 46.png',
      'catalogo/Página 47.png',
      'catalogo/Página 48.png',
      'catalogo/Página 49.png',
      'catalogo/Página 50.png',
      'catalogo/Página 51.png',
      'catalogo/Página 52.png',
      'catalogo/Página 53.png',
      'catalogo/Página 54.png',
      'catalogo/Página 55.png',
      'catalogo/Página 56.png',
      'catalogo/Página 57.png',
      'catalogo/Página 58.png',
      'catalogo/Página 59.png',
      'catalogo/Página 60.png',
      'catalogo/Página 61.png',
      'catalogo/Página 62.png',
      'catalogo/Página 63.png',
      'catalogo/Página 64.png',
      'catalogo/Página 65.png'
    ];

    const gallery = document.getElementById('imageGallery');

    images.forEach((src, index) => {
      const item = document.createElement('div');
      item.className = 'item';

      const checkbox = document.createElement('input');
      checkbox.type = 'checkbox';
      checkbox.id = 'img' + index;
      checkbox.value = src;

      const label = document.createElement('label');
      label.htmlFor = checkbox.id;

      const img = document.createElement('img');
      img.src = src;
      label.appendChild(img);

      item.appendChild(label);
      item.appendChild(checkbox);
      gallery.appendChild(item);
    });

    async function generatePDF() {
      const { PDFDocument } = PDFLib;
      const pdfDoc = await PDFDocument.create();
      const selected = document.querySelectorAll('input[type=checkbox]:checked');

      for (const checkbox of selected) {
        const imgUrl = checkbox.value;
        const imgBytes = await fetch(imgUrl).then(res => res.arrayBuffer());
        const img = await pdfDoc.embedPng(imgBytes);
        const page = pdfDoc.addPage([img.width, img.height]);
        page.drawImage(img, {
          x: 0,
          y: 0,
          width: img.width,
          height: img.height
        });
      }

      const pdfBytes = await pdfDoc.save();
      const blob = new Blob([pdfBytes], { type: 'application/pdf' });
      const url = URL.createObjectURL(blob);

      const a = document.createElement('a');
      a.href = url;
      a.download = 'catalogo-gotrix.pdf';
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
    }
  </script>

</body>
</html>
