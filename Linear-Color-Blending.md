# What has changed?
We’ve changed the way RGB colors are blended by using a gamma of 1.0, i.e. linearly.

This means that there are some visual changes in how colors and brightness are blended together, and also how transparent sources now blend linearly with the background. This means that with lighter backgrounds, the background will contribute more to the resulting color.


Some background: https://www.youtube.com/watch?v=LKnqECcg6Gw

More background: https://blog.johnnovak.net/2016/09/21/what-every-coder-should-know-about-gamma/

# Why did we change this?
There have been fairly substantial changes in the graphics/color pipeline to support SRGB format textures and render targets, which naturally leads to blend operations occurring in linear space, so proper blending was an automatic by-product. Among the issues that this fixes are:

* Dark artifacts you would normally see where colors would meet: https://blog.johnnovak.net/2016/09/21/what-every-coder-should-know-about-gamma/#colour-blending

* Hue and brightness shifts when alpha blending: https://blog.johnnovak.net/2016/09/21/what-every-coder-should-know-about-gamma/#alpha-blending--compositing


# How do I deal with this?

You can hopefully configure your content creation software to behave like OBS does. Photoshop, After Effects and others can be configured to behave like OBS 27.

This should make it easier to author your assets with the same behavior as OBS 27, and make it more intuitive to achieve the look/style you want.

### Photoshop
You can press Ctrl+Shift+K, and enable "Blend RGB Colors Using Gamma: 1.0.
<ps.jpg>

### After Effects
Go to "Project Settings" -> "Color Settings" tab and set "Blend Colors Using 1.0 Gamma"
<ae.jpg>

### Adobe Premiere
Sequence > Sequence Settings. At the bottom check "Composite in Linear Color"
<premiere.jpg>

### GIMP
Image > Precision > Linear Light
<gimp.jpg>

Other software may have a similar feature under a different name. Try searching for your software + "linear light", "gamma 1.0", "linear color" or similar.
***
If the exported images/videos have semi-transparency in them, you need to configure the image/media sources in OBS to use linear alpha. We don’t enable this by default for partial backwards compatibility with OBS 26. There is a new linear space checkbox in their source properties. (Media source will have the checkbox in the 27.0.1 release.)
<checkbox.jpg>


