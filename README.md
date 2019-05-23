# smart-colors
Material Colors Web Component

In this post, we will show you how to define a Custom Element with Smart Framework. We will create a Material Color Picker custom element.
<link rel="stylesheet" href="/wp-content/design/assets/stylesheets/prettify.css" type="text/css" /> 
<script type="text/javascript" src="/wp-content/design/assets/js/prettify.js"></script>
<script>
window.onload = function() {
prettyPrint();
}
</script>
<br/><br/>
1. Define the Custom Element. To define a new custom element with <strong>Smart Framework</strong>, we need to call the <strong>Smart</strong> function with two parameters - the tag name and the <strong>Class</strong> with the logic.
<br/><br/>
```
Smart('smart-colors', class SmartColors extends Smart.BaseElement {  
}
```
<br/><br/>
2. <strong>Smart Framework</strong> gives us useful things like Templates, Data Binding, Typed Properties, Lifecycle callbacks, Event Handling. Read more about this <a target="_blank" href="https://www.htmlelements.com/docs/base-element/">https://www.htmlelements.com/docs/base-element/</a>. In our Custom Element, we will use some of these features. We will use the 'ready' lifecycle callback. It is invoked once, when the element is attached for first time. We use this function to create and initialize the custom element. The 'properties' definition includes a property called 'color'. This property has default value - '#fff' and its type is 'string'. This means that when you try to set a 'bool', 'date', 'numeric' value, the element will throw an error with 'Invalid property type'. To raise the 'change' event, we use the '$.fireEvent' method.  
<br/><br/>
3. The full code of our custom element is below
```javascript
  // Define Custom Element
Smart('smart-colors', class SmartColors extends Smart.BaseElement {  
  // Declare properties
  static get properties() {  
    return {
      'color': 
        {
           value: '#fff',
           type: 'string'
        }
    };
  }
   
  // Ready is called when the element is in the DOM.
  ready () {
    const that = this;
    
    that._renderGrid();
    that._addHandlers();
  }
   
  // Renders the Color Panel.
  _renderGrid() {
    const that = this;
    const labelsAndPaletteContainer = document.createElement('div');

    that._renderShades();
    that._renderColorPalette();
    that._renderColorLabels();

    labelsAndPaletteContainer.classList = 'smart-labels-and-palette'
    labelsAndPaletteContainer.appendChild(that._colorLabelsContainer);
    labelsAndPaletteContainer.appendChild(that._paletteContainer);
    that.appendChild(labelsAndPaletteContainer);
  }
 
  // Rneders all colors.
  _renderColorPalette() {
    const that = this;
    const colorsArray = [
      ['#ffebee', '#ffcdd2', '#ef9a9a', '#e57373', '#ef5350', '#f44336', '#e53935', '#d32f2f', '#c62828', '#b71c1c', '#ff8a80', '#ff5252', '#ff1744', '#d50000'],
      ['#fce4ec', '#f8bbd0', '#f48fb1', '#f06292', '#ec407a', '#e91e63', '#d81b60', '#c2185b', '#ad1457', '#880e4f', '#ff80ab', '#ff4081', '#f50057', '#c51162'],
      ['#f3e5f5', '#e1bee7', '#ce93d8', '#ba68c8', '#ab47bc', '#9c27b0', '#8e24aa', '#7b1fa2', '#6a1b9a', '#4a148c', '#ea80fc', '#e040fb', '#d500f9', '#aa00ff'],
      ['#ede7f6', '#d1c4e9', '#b39ddb', '#9575cd', '#7e57c2', '#673ab7', '#5e35b1', '#512da8', '#4527a0', '#311b92', '#b388ff', '#7c4dff', '#651fff', '#6200ea'],
      ['#e8eaf6', '#c5cae9', '#9fa8da', '#7986cb', '#5c6bc0', '#3f51b5', '#3949ab', '#303f9f', '#283593', '#1a237e', '#8c9eff', '#536dfe', '#3d5afe', '#304ffe'],
      ['#e3f2fd', '#bbdefb', '#90caf9', '#64b5f6', '#42a5f5', '#2196f3', '#1e88e5', '#1976d2', '#1565c0', '#0d47a1', '#82b1ff', '#448aff', '#2979ff', '#2962ff'],
      ['#e1f5fe', '#b3e5fc', '#81d4fa', '#4fc3f7', '#29b6f6', '#03a9f4', '#039be5', '#0288d1', '#0277bd', '#01579b', '#80d8ff', '#40c4ff', '#00b0ff', '#0091ea'],
      ['#e0f7fa', '#b2ebf2', '#80deea', '#4dd0e1', '#26c6da', '#00bcd4', '#00acc1', '#0097a7', '#00838f', '#006064', '#84ffff', '#18ffff', '#00e5ff', '#00b8d4'],
      ['#e0f2f1', '#b2dfdb', '#80cbc4', '#4db6ac', '#26a69a', '#009688', '#00897b', '#00796b', '#00695c', '#004d40', '#a7ffeb', '#64ffda', '#1de9b6', '#00bfa5'],
      ['#e8f5e9', '#c8e6c9', '#a5d6a7', '#81c784', '#66bb6a', '#4caf50', '#43a047', '#388e3c', '#2e7d32', '#1b5e20', '#b9f6ca', '#69f0ae', '#00e676', '#00c853'],
      ['#f1f8e9', '#dcedc8', '#c5e1a5', '#aed581', '#9ccc65', '#8bc34a', '#7cb342', '#689f38', '#558b2f', '#33691e', '#ccff90', '#b2ff59', '#76ff03', '#64dd17'],
      ['#f9fbe7', '#f0f4c3', '#e6ee9c', '#dce775', '#d4e157', '#cddc39', '#c0ca33', '#afb42b', '#9e9d24', '#827717', '#f4ff81', '#eeff41', '#c6ff00', '#aeea00'],
      ['#fffde7', '#fff9c4', '#fff59d', '#fff176', '#ffee58', '#ffeb3b', '#fdd835', '#fbc02d', '#f9a825', '#f57f17', '#ffff8d', '#ffff00', '#ffea00', '#ffd600'],
      ['#fff8e1', '#ffecb3', '#ffe082', '#ffd54f', '#ffca28', '#ffc107', '#ffb300', '#ffa000', '#ff8f00', '#ff6f00', '#ffe57f', '#ffd740', '#ffc400', '#ffab00'],
      ['#fff3e0', '#ffe0b2', '#ffcc80', '#ffb74d', '#ffa726', '#ff9800', '#fb8c00', '#f57c00', '#ef6c00', '#e65100', '#ffd180', '#ffab40', '#ff9100', '#ff6d00'],
      ['#fbe9e7', '#ffccbc', '#ffab91', '#ff8a65', '#ff7043', '#ff5722', '#f4511e', '#e64a19', '#d84315', '#bf360c', '#ff9e80', '#ff6e40', '#ff3d00', '#dd2c00'],
      ['#efebe9', '#d7ccc8', '#bcaaa4', '#a1887f', '#8d6e63', '#795548', '#6d4c41', '#5d4037', '#4e342e', '#3e2723'],
      ['#fafafa', '#f5f5f5', '#eeeeee', '#e0e0e0', '#bdbdbd', '#9e9e9e', '#757575', '#616161', '#424242', '#212121'],
      ['#eceff1', '#cfd8dc', '#b0bec5', '#90a4ae', '#78909c', '#607d8b', '#546e7a', '#455a64', '#37474f', '#263238'],
    ]
    const paletteContainer = document.createElement('div');

    for (let index = 0, length = colorsArray.length; index < length; index++) {
      const currentRow = colorsArray[index];
      const currentUl = that._renderRow(currentRow, 'smart-color-cell', false);

      paletteContainer.appendChild(currentUl);
    }

    paletteContainer.className = 'smart-palette';
    that._paletteContainer = paletteContainer;
  }
  
  // Renders all shades.
  _renderShades() {
    const that = this;
    const shadesContainer = document.createElement('div');
    const shadesArray = [50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 'A 100', 'A 200', 'A 400', 'A 700'];
    const shadesRow = that._renderRow(shadesArray, 'smart-shade-cell', true);

    shadesContainer.className = 'smart-shades';
    shadesContainer.appendChild(shadesRow);
    that.appendChild(shadesContainer);
  }

  _renderColorLabels() {
    const that = this;
    const colorLabelsContainer = document.createElement('div');
    const colorLabelsArray = ['Red', 'Pink', 'Purple', 'Deep Purple', 'Indigo', 'Blue', 'Light Blue', 'Cyan', 'Teal', 'Green', 'Light Green', 'Lime', 'Yellow', 'Amber', 'Orange', 'Deep Orange', 'Brown', 'Grey', 'Blue Grey'];
    const colorLabelsColumn = that._renderRow(colorLabelsArray, 'smart-color-label', true);

    colorLabelsContainer.className = 'smart-color-labels';
    colorLabelsContainer.appendChild(colorLabelsColumn);
    that._colorLabelsContainer = colorLabelsContainer;
  }
  
  // Renders a single row of colors.
  _renderRow(array, cellClass, addInnerHtml) {
    const ul = document.createElement('ul');

    for (let index = 0, length = array.length; index < length; index++) {
      const currentElement = array[index];
      const li = document.createElement('li');

      if (addInnerHtml) {
        li.innerHTML = currentElement;
      }
      else {
        li.style.background = currentElement;
        li.setAttribute('data-color', currentElement);
      }

      li.className = cellClass;

      ul.appendChild(li);
    }

    return ul;
  }
  
  // Fires a 'change' event when a color is selected.
  _addHandlers() {
    const that = this;

    const cells = that.querySelectorAll('.smart-color-cell');

    for(let i = 0; i < cells.length; i++) {
      const  cell = cells[i];

      cell.addEventListener('click', function() {
        that._currentColorHex = event.target.getAttribute('data-color');
        that._currentColorRgb = event.target.style.background;
        that.color = that.getColor().hex.toString();
        that.$.fireEvent('change');
      });
    }
  }
 
  // gets the selected color.
  getColor() {
    const that = this;
    const rgb = that._currentColorRgb.match(/\d+/g);

    return {
      hex: that._currentColorHex.substring(1),
      r: parseInt(rgb[0]),
      g: parseInt(rgb[1]),
      b: parseInt(rgb[2])
    };
  }
});

```

<p class="codepen" data-height="340" data-theme-id="0" data-default-tab="js,result" data-user="boyko-markov" data-slug-hash="JqpWeb" style="height: 340px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="SMART HTML ELEMENTS">
  <span>See the Pen <a href="https://codepen.io/boyko-markov/pen/JqpWeb/">
  SMART HTML ELEMENTS</a> by Boyko Markov (<a href="https://codepen.io/boyko-markov">@boyko-markov</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
  
