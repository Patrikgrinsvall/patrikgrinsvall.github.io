Neumorphic panels
```html
<!DOCTYPE html>
<head>


<style>
    :root {
      --bg: #f1f1f1;
      --bg2: #f5f5f5;
      --radius: 20px;
      --margin: 14px;
      --shadow-inset: inset 9px 9px 9px 0 rgba(206, 206, 206, 0.3), 
                      inset -9px -9px 9px 0 rgba(255, 255, 255, 0.6);
      --shadow-convex: inset -9px -9px 9px 0 rgba(206, 206, 206, 0.3), 
                      inset 9px 9px 9px 0 rgba(255, 255, 255, 0.6);
      --shadow: -9px -9px 9px 0 rgba(255, 255, 255, 0.62), 
                -1px -1px 1px 0px rgba(255, 255, 255, 0.3), 
                1px 1px 1px 0px rgba(206, 206, 206, 0.15), 
                9px 9px 9px 0 rgba(206, 206, 206, 0.3);
    }
    
    *, *::after, *::before {
      box-sizing: border-box;
    }
    
    body {
      background: var(--bg);
      font-family: 'Open Sans', sans-serif;
      font-size: 14px;
    }
    button {
      display: flex;
      fill: #689F38;
      cursor: pointer;
    }
    button:focus,
    button:active {
      outline: none;
    }
    .container {
      display: flex;
      flex-wrap: wrap;
      padding: 15px 30px;
    }
    
    .panel {
        width: 300px;
        
    }
    .neumorph {
        border-radius: var(--radius);
        border: none;
        background: var(--bg2);
        box-shadow: var(--shadow);
        margin: var(--margin);
        padding: var(--margin);
    }
    
    .neumorph--pushed {
      box-shadow: var(--shadow-inset), var(--shadow);
    }
    .neumorph--pushed-only {
      box-shadow: var(--shadow-inset);
    }
    .neumorph--convex {
      box-shadow: var(--shadow-convex), var(--shadow);
    }
    </style>

    </head>
    <body>
    <div class="container">
      <h1>Times</h1>  
    </div>
    
    <div class="container">
      <div class="panel neumorph">Panel
        <div class="neumorph">Panel2</div>
        <div class="neumorph">Panel2</div>
        <div class="neumorph">Panel2</div>
        <div class="neumorph">Panel2</div>
        <div class="neumorph">Panel2</div>
        <div class="neumorph">Panel2</div>    
      </div>
    </div>
    
    <script>
    
    let root = document.documentElement;
    
    let shadow = {
      radius: 20,
      size: 9,
      blur: 9,
      colorL: 255,
      colorD: 206,
      opacityL: 62,
      opacityD: 30,
      colorBg: '#f1f1f1',
      colorFg: '#f5f5f5'
    }
    
    class ShadowBuilder {
    
      shadowLayers = []
    
      _create (size, blur, color, opacity) {
        return `${size}px ${size}px ${blur}px 0px rgba(${color}, ${color}, ${color}, ${opacity})`
      }
    
      add (size, blur, color, opacity) {
        this.shadowLayers.push(
          this._create(size, blur, color, opacity)
        )
      }
    
      addInset (size, blur, color, opacity) {
        this.shadowLayers.push(
          `inset ${this._create(size, blur, color, opacity)}`
        )
      }
    
      build () {
        return this.shadowLayers.join(',');
      }
    }
    
    function getShadow () {
      
      let builder = new ShadowBuilder()
      
      builder.add(-shadow.size, shadow.blur, shadow.colorL, shadow.opacityL/100)
      builder.add(shadow.size, shadow.blur, shadow.colorD, shadow.opacityD/100)
      builder.add(-1, 1, shadow.colorL, 0.3)
      builder.add( 1, 1, shadow.colorD, shadow.opacityD/100)
    
      return builder.build()
    }
    
    function getInsetShadow () {
      
        let builder = new ShadowBuilder()
        builder.addInset(shadow.size, shadow.blur, shadow.colorD, shadow.opacityD/100)
        builder.addInset(-shadow.size, shadow.blur, shadow.colorL, shadow.opacityL/100)
        return builder.build()
    }
    
    function getConvexShadow () {
      
        let builder = new ShadowBuilder()
        builder.addInset(-shadow.size, shadow.blur, shadow.colorD, shadow.opacityD/100)
        builder.addInset( shadow.size, shadow.blur, shadow.colorL, shadow.opacityL/100)
        return builder.build()
    }
    
    
    //! dat.GUI Code !//
    let gui = new dat.GUI(); 
    
    function handleElementsGui (varName, suffix) {
      return value => {
        root.style.setProperty(varName, value + (suffix||''));
      }
    }
    
    function handleShadowGui (name) {
      return value => {
        root.style.setProperty('--shadow', getShadow())
        root.style.setProperty('--shadow-inset', getInsetShadow())
        root.style.setProperty('--shadow-convex', getConvexShadow())
      }
    }
    
    var shadowShapeFolder = gui.addFolder('Shadows Shape');
    shadowShapeFolder.add(shadow, "size", 0, 20).name("Size").onChange( handleShadowGui('size') );
    shadowShapeFolder.add(shadow, "blur", 0, 20).name("Blur").onChange( handleShadowGui('blur') );
    shadowShapeFolder.add(shadow, "opacityL", 0, 100).name("Brightness bright").onChange( handleShadowGui('opacityL') );
    shadowShapeFolder.add(shadow, "opacityD", 0, 100).name("Darkness dark").onChange( handleShadowGui('opacityD') );
    shadowShapeFolder.add(shadow, "colorL", 0, 255).name("Brightness color").onChange( handleShadowGui('colorL') );
    shadowShapeFolder.add(shadow, "colorD", 0, 255).name("Darkness color").onChange( handleShadowGui('colorD') );
    
    var colorsFolder = gui.addFolder('Elements Shape and Colors');
    colorsFolder.addColor(shadow, 'colorBg').name('Background').onChange( handleElementsGui('--bg') );
    colorsFolder.addColor(shadow, 'colorFg').name('Foreground').onChange( handleElementsGui('--bg2') );
    colorsFolder.add(shadow, "radius", 0, 50).name("Corner radius").onChange( handleElementsGui('--radius', 'px'));
    
    </script>
      
    
    
      
</body>
</html>
```
