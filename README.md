# -DOCTYPE-html-html-lang-en-head-meta-charset-UTF-8-meta-name-viewport-content-<!DOCTYPE html>
<html>
  <head>
    <title>Catan 3D Førsteperson</title>
    <script src="https://aframe.io/releases/1.6.0/aframe.min.js"></script>
    <script src="https://unpkg.com/aframe-environment-component@1.3.3/dist/aframe-environment-component.min.js"></script>
  </head>
  <body>
    <script>
      // --- KONFIGURATION AF BRÆTTET ---
      // Her definerer vi farverne for ressourcerne for at gøre det realistisk
      const TERRAIN_COLORS = {
        'WOOD': '#228B22',   // Skov (Mørkegrøn)
        'BRICK': '#B22222',  // Ler (Rødbrun)
        'WOOL': '#7CFC00',   // Græs (Lysegrøn)
        'GRAIN': '#FFD700',  // Korn (Guld/Gul)
        'ORE': '#708090',    // Bjerge (Grå)
        'DESERT': '#F4A460'  // Ørken (Sandfarvet)
      };

      // Dette er en forenklet repræsentation af brættet fra dit billede
      // Vi bruger en simpel række/kolonne struktur, som vi "skubber" til hex-form senere
      const BOARD_DATA = [
        {type: 'DESERT', num: ''}, {type: 'GRAIN', num: '4'}, {type: 'WOOD', num: '11'},
        {type: 'WOOL', num: '8'}, {type: 'BRICK', num: '3'}, {type: 'BRICK', num: '9'},
        {type: 'ORE', num: '5'}, {type: 'GRAIN', num: '10'}, {type: 'WOOD', num: '12'},
        {type: 'WOOD', num: '6'}, {type: 'ORE', num: '2'}, {type: 'ORE', num: '10'}
      ];

      // --- A-FRAME KOMPONENT TIL AT BYGGE BRÆTTET ---
      AFRAME.registerComponent('catan-board', {
        init: function () {
          let el = this.el;
          // Størrelsen på hex-brikkerne
          const hexRadius = 1.8; 
          const hexHeight = 0.3;
          
          // Matematik for at placere dem i et "falsk" hex-mønster (nemmere end ægte hex-grid)
          let xOffset = 0;
          let zOffset = 0;
          let rowCount = 0;

          BOARD_DATA.forEach((tile, index) => {
            // Opret selve hex-brikken (en flad cylinder med 6 sider)
            let hex = document.createElement('a-cylinder');
            
            // Beregn position (dette er en simpel grid-til-hex konvertering)
            if (index % 3 === 0) {
                rowCount++;
                zOffset += hexRadius * 1.75;
                // Skift startposition for hver række for at lave "bikube"-effekten
                xOffset = (rowCount % 2 === 0) ? 0 : hexRadius * 0.9;
            } else {
                xOffset += hexRadius * 1.8;
            }

            hex.setAttribute('position', {x: xOffset - 5, y: 0, z: zOffset - 8});
            hex.setAttribute('radius', hexRadius);
            hex.setAttribute('height', hexHeight);
            hex.setAttribute('segments-radial', 6); // DETTE GØR DEN SEKSKANTET!
            hex.setAttribute('color', TERRAIN_COLORS[tile.type]);
            hex.setAttribute('shadow', {receive: true});
            
            // Tilføj en sort kant for realisme
            let border = document.createElement('a-torus');
             border.setAttribute('radius', hexRadius);
             border.setAttribute('radius-tubular', 0.05);
             border.setAttribute('rotation', '90 0 0');
             border.setAttribute('position', {x: 0, y: hexHeight/2 + 0.01, z: 0});
             border.setAttribute('color', '#333');
            hex.appendChild(border);

            // Tilføj tal-brikken (hvis det ikke er ørken)
            if (tile.
