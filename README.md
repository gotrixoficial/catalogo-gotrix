# catalogo-gotrix
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Catálogo Gotrix - Selecione e Baixe</title>
  <script src="https://unpkg.com/pdf-lib"></script>
  <style>
    body {
      font-family: sans-serif;
      padding: 20px;
    }
    .gallery {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 15px;
    }
    .item {
      text-align: center;
    }
    img {
      width: 100%;
      height: auto;
      border: 1px solid #ccc;
      border-radius: 8px;
    }
    button {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 16px;
      border: none;
      border-radius: 8px;
      background-color: #00b894;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background-color: #019875;
    }
  </style>
</head>
<body>
  <h1>📘 Catálogo Gotrix</h1>
  <p>Selecione as páginas que deseja e clique no botão para gerar seu PDF personalizado.</p>

  <div class="gallery" id="imageGallery">
    <!-- As imagens são carregadas aqui -->
  </div>

  <button onclick="generatePDF()">📥 Baixar PDF com páginas selecionadas</button>

  <script>
    // Lista de imagens do catálogo (adicione os caminhos reais das suas imagens PNG)
    const images = [
      'catalogo/pagina1.png',
      'catalogo/pagina2.png',
      'catalogo/pagina3.png',
      'catalogo/pagina4.png'
      // ...adicione até a página 65
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

      item.appendChild(checkbox);
      item.appendChild(label);

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
