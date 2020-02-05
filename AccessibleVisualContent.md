When presenting results and concepts make sure you make your graphics and visual assetts accessible. It is an issue of social justice, as you do not want to cut out any members of your audience due to physical deficiencies or impairments, and it is beneficial to you to be able to communicate your message to the wudest audience possible. 

# The choice of color and fonts will affect the perception and accessibility of the information you are sharing. 


# Color blindness

Colour (color) blindness (colour vision deficiency, or CVD) refers to conditions that modify the perception of color; it affects approximately 1 in 12 men (8%) and 1 in 200 women in the world. 

1) choose color-blind compliant palettes when making plots.

Many coding languages have graphic packages that offer color-blind compliant color palettes, 

- R: http://www.cookbook-r.com/Graphs/Colors_(ggplot2)/#a-colorblind-friendly-palette

- Python: while the default colors in [matplotlib](https://matplotlib.org/) v.3 he are perceptually homogeneous and well designed, they are not generally colorblind friendly. 
There is [the intent](https://github.com/matplotlib/matplotlib/issues/9460) to move to a colorblind-friendly palette. In the meantime, the [seaborn package offers a color-blind palette](https://seaborn.pydata.org/tutorial/color_palettes.html#qualitative-color-palettes).

2) Download apps to simulate different types of color blindness to assure your graphics and slides are readable for all people, siuch as the [colororacle](http://colororacle.org/)

3) choose color-blind compliant palettes when designing slides. 

# Make sure that your slides are readable to dyslexic and vision impaired members of the audience, and overall clear and accessible

Follow [these recommandations](https://www.ifla.org/files/assets/hq/officers/documents/wbu-visual-presentations-guidelines-summary.pdf) from the World Blind Union about fonts, font size, color, brightness, and contrast choices. Follow [these recommandation about graphic design](https://www.presentationzen.com/chapter6_spread.pdf)

Here are some general recommandations about visualizations and slides. 

Limit the number of word in your slides. 

Limit animation to essential and simple items (e.g. fade in only when helpful to highlight differences, slide transitions only in correspondence of a change of topic)

Simplify your plots and assign only one graphical element (color, shape, texture) to each data feature.

Do not use: 
  - pie charts. They are perceptually inconsistent with the mathematical proportions that they represents. Dounught cuarts and bar charts are good alternatives
  - 3D graphice. Deformation, obstruction, and distraction are all issues with 3D graphics. Only use perspective where it is strictly nevessary to represent physical shapes and proportions (e.g. in a model)
  - rainbow color schemes: they are perceptually dishomogeneous leading to percieve edgese where there are smooth transitions, miss subtle changes in the data, and focus on portions of the data. Black and white or shaes of gray are almost always a great choice. Viridis, Gist, and other "perceptually uniform" color maps are better choices than rainbow, always and forever.
 
Use color functionally rather than esthetically to highlight features in data.

Learn the difference between and different applications of perceptually uniform, sequencial, and diverging color schemes:
  - use sequencial color schemes (color schemes base on one hue that fades to white or black progressively) where the data has a natural "low" and a natural "high" point
  - use diverging color schemes (color schemes where two different hues start at a common point) only when the data you are representing has a natural 0 point or center, and make sure that the convergence of the colors corresponds with this point in the data. 
  - use perceptually uniform color schemes when no region of your data is of particular relevance compared to the rest of the feature space.
  
See more recommandations about visualizations in [these lecture slides](https://slides.com/federicabianco/dsps2019_vii)

Resources:

Edwaed tufte (anything)

Tamara Munzner [Visualization Analysis & Design, 2014](http://www.cs.ubc.ca/~tmm/talks/minicourse14/vad15london.pdf)

[color maps](http://www.kennethmoreland.com/color-maps/)

[Kelly colors](https://medium.com/@rjurney/kellys-22-colours-of-maximum-contrast-58edb70c90d1)

[7 Great Visualizations from History)[https://web.archive.org/web/20171114145335/http://data-informed.com/7-great-visualizations-history/)


(Using preattemptive processing elements)[https://pdfs.semanticscholar.org/0456/bc9cdf02c3a446e252cf2e6b83145e17749a.pdf]
