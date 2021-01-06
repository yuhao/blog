# Principles and Practices of Chromatic Adaptation

Colorimetry provides a scientific and quantitative way to measure and compare color. By specifying a set of primaries (e.g., RGB or XYZ) and the tristimulus values, we could tell whether two light stimuli will generate the same color perception. However, colorimetry works best for a fixed viewing condition. Thus, the change of viewing condition requires modeling various factors in viewing condition, one of which is the viewing illuminant.

The significance of the viewing illuminant is that human beings change their color perception by adapting to the color of the illuminant through a process known as *chromatic adaptation*. Chromatic adaptation is fascinating; it's rooted in fundamental neuroscience and at the same time has wide applications in practice. This article describes some interesting and perhaps confusing aspects of chromatic adaptation, and discusses a few applications of chromatic adaptation.

## 1. Principles of von Kries Chromatic Adaptation

We provide a brief account for chromatic adaptation first. There are many excellent resources that provide a comprehensive treatment of chromatic adaptation. Two references that I find helpful are [Color Imaging: Fundamentals and Applications](https://dl.acm.org/doi/book/10.5555/1386684) by Reinhard et al., and of course [Color Appearance Models](https://onlinelibrary.wiley.com/doi/book/10.1002/9781118653128) by Fairchild.

In everyday life we encounter a wide range of viewing environments, each of which has a different illuminant ranging from incandescent light to daylight, but we have relatively constant visual experience. For instance, a piece of white paper or a white wall will appear the same white color to you whether you view it under incandescent light or under noon sunlight. This is because our visual system adapts to the illuminant insofar as we could discount the (color of the) illuminant to some extent, i.e., chromatic adaptation. Note that there are other forms of adaptation. For instance, we adapt to very dark or very bright environments, giving rise to the notions of light and dark adaptation, respectively. This article focuses on chromatic adaptation.

### 1.1 The Theory

How do we adapt? [Johannes von Kries](https://en.wikipedia.org/wiki/Johannes_von_Kries) hypothesized that we adapt by scaling the responses of the three types of cone under different illuminants, each of which scales independently. In matrix form, von Kries adaptation is expressed as the following equation, where [L, M, S]<sup>T</sup> represents the unadapted cone responses of the neutral point, [L<sub>a</sub>, M<sub>a</sub>, S<sub>a</sub>]<sup>T</sup> represents the adapted cone responses, and &alpha;, &beta;, &gamma; are the scaling factors (gains) of each cone type, respectively.

<img src="https://latex.codecogs.com/svg.latex?\begin{bmatrix}&space;L_a\\&space;M_a\\&space;S_a\\&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;\alpha,~0,~0\\&space;0,~\beta,~0\\&space;0,~0,~\gamma\\&space;\end{bmatrix}&space;\times&space;\begin{bmatrix}&space;L\\&space;M\\&space;S\\&space;\end{bmatrix}" title="\begin{bmatrix} L_a\\ M_a\\ S_a\\ \end{bmatrix} = \begin{bmatrix} \alpha,~0,~0\\ 0,~\beta,~0\\ 0,~0,~\gamma\\ \end{bmatrix} \times \begin{bmatrix} L\\ M\\ S\\ \end{bmatrix}" />

How exactly does each cone type scale? That is, what are the values of &alpha;, &beta;, and &gamma;? von Kries’ theory is that the cone responses scale in such a way that a neutral point appears the same color under different illuminants. This basic assumption/observation would allow us to determine the scaling factors, as will see next.

But first of all, what is a neutral point? A neutral point is a point that reflects all wavelengths equally without loss of energy. That is, the spectral reflectance of a neutral point is universally 1. From the colorimetry perspective, the color of a neutral point is obviously the color of the illuminant itself. This is because <code>&int;&Phi;(&lambda;)**R**(&lambda;)**L**(&lambda;)d&lambda; = &int;&Phi;(&lambda;)**L**(&lambda;)d&lambda;</code> when **R**(&lambda;) is 1 for all &lambda;s. A neutral point is sometimes also called a perfectly white diffuser. Since common illuminants appear white-ish, the color of the neutral point under a given illuminant is also called the white point of the illuminant.

While in reality no material is truly a perfectly white diffuser, a piece of white paper or a white wall comes close. That’s why you will often hear people say things like “white always appears white under different lights.” What they really mean, in precise terms, is that the neutral point maintains a constant perceived color under different illuminants.

Another way to look at this neutral point color constancy assumption is that the cone responses of a neutral point, after adaptation, are constant, independent of an illuminant. Taking this perspective, it’s easy to calculate the scaling factors. Here is the idea. Let’s use [L<sub>w</sub>, M<sub>w</sub>, S<sub>w</sub>]<sup>T</sup> to denote the unadapted cone responses of the illuminant itself. The unadapted cone responses of the neutral point under this illuminant must be [L<sub>w</sub>, M<sub>w</sub>, S<sub>w</sub>]<sup>T</sup> too, based on the definition of a neutral point. The neutral point color constancy basically states that the adapted responses of the neutral point under this illuminant, [L<sub>a</sub>, M<sub>a</sub>, S<sub>a</sub>]<sup>T</sup>, should be constant, i.e., independent of [L<sub>w</sub>, M<sub>w</sub>, S<sub>w</sub>]<sup>T</sup>. Let’s say the constant adapted responses are [Q, P, R]<sup>T</sup>, where Q, P, and R are just three constants.

Out of mathematical convenience, we’ll use 1 for Q, P, and R. As a result:

&alpha; = 1/L<sub>w</sub>

&beta; = 1/M<sub>w</sub>

&gamma; = 1/S<sub>w</sub>

This gives the familiar chromatic adaptation equation below, where &Phi;<sub>w</sub> denotes the SPD of the illuminant. As we will see later, the exact numerical values of the constants are not important for practical applications.

<img src="https://latex.codecogs.com/svg.latex?\begin{bmatrix}&space;L_a\\&space;M_a\\&space;S_a\\&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;\frac{1}{L_w},&space;0,&space;0\\&space;0,&space;\frac{1}{M_w},&space;0\\&space;0,&space;0,&space;\frac{1}{S_w}\\&space;\end{bmatrix}&space;\times&space;\begin{bmatrix}&space;L\\&space;M\\&space;S\\&space;\end{bmatrix}&space;,~~where&space;\begin{bmatrix}&space;L_w\\&space;M_w\\&space;S_w\\&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;\int\Phi_w(\lambda)L(\lambda)d\lambda\\&space;\int\Phi_w(\lambda)M(\lambda)d\lambda\\&space;\int\Phi_w(\lambda)S(\lambda)d\lambda\\&space;\end{bmatrix}" title="\begin{bmatrix} L_a\\ M_a\\ S_a\\ \end{bmatrix} = \begin{bmatrix} \frac{1}{L_w}, 0, 0\\ 0, \frac{1}{M_w}, 0\\ 0, 0, \frac{1}{S_w}\\ \end{bmatrix} \times \begin{bmatrix} L\\ M\\ S\\ \end{bmatrix} ,~~where \begin{bmatrix} L_w\\ M_w\\ S_w\\ \end{bmatrix} = \begin{bmatrix} \int\Phi_w(\lambda)L(\lambda)d\lambda\\ \int\Phi_w(\lambda)M(\lambda)d\lambda\\ \int\Phi_w(\lambda)S(\lambda)d\lambda\\ \end{bmatrix}" />

The von Kries chromatic adaptation model ensures that a neutral point does have a constant color appearance under different illuminants. This is obvious from the equation below, where [L<sub>w1</sub>, M<sub>w1</sub>, S<sub>w1</sub>]<sup>T</sup> and [L<sub>w2</sub>, M<sub>w2</sub>, S<sub>w2</sub>]<sup>T</sup> are two arbitrary illuminants w1 and w2.

<img src="https://latex.codecogs.com/svg.latex?\begin{bmatrix}&space;\frac{1}{L_{w1}},&space;0,&space;0\\&space;0,&space;\frac{1}{M_{w1}},&space;0\\&space;0,&space;0,&space;\frac{1}{S_{w1}}\\&space;\end{bmatrix}&space;\times&space;\begin{bmatrix}&space;L_{w1}\\&space;M_{w1}\\&space;S_{w1}\\&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;\frac{1}{L_{w2}},&space;0,&space;0\\&space;0,&space;\frac{1}{M_{w2}},&space;0\\&space;0,&space;0,&space;\frac{1}{S_{w2}}\\&space;\end{bmatrix}&space;\times&space;\begin{bmatrix}&space;L_{w2}\\&space;M_{w2}\\&space;S_{w2}\\&space;\end{bmatrix}" title="\begin{bmatrix} \frac{1}{L_{w1}}, 0, 0\\ 0, \frac{1}{M_{w1}}, 0\\ 0, 0, \frac{1}{S_{w1}}\\ \end{bmatrix} \times \begin{bmatrix} L_{w1}\\ M_{w1}\\ S_{w1}\\ \end{bmatrix} = \begin{bmatrix} \frac{1}{L_{w2}}, 0, 0\\ 0, \frac{1}{M_{w2}}, 0\\ 0, 0, \frac{1}{S_{w2}}\\ \end{bmatrix} \times \begin{bmatrix} L_{w2}\\ M_{w2}\\ S_{w2}\\ \end{bmatrix}" />

However, other (non-neutral/white) colors, albeit adapted, are *not* perfectly adapted. This can be seen from the following inequality, where [L<sub>w1</sub>, M<sub>w1</sub>, S<sub>w1</sub>]<sup>T</sup> and [L<sub>w2</sub>, M<sub>w2</sub>, S<sub>w2</sub>]<sup>T</sup> again are two arbitrary illuminants w1 and w2, respectively, and [L<sub>1</sub>, M<sub>1</sub>, S<sub>1</sub>]<sup>T</sup> and [L<sub>2</sub>, M<sub>2</sub>, S<sub>2</sub>]<sup>T</sup> are the unadapted responses of a non-neutral material under the two illuminants, respectively. Note that the equation doesn’t hold for non-neutral material, suggesting that the color appearance of an arbitrary point won’t be constant. Intuitively, while an apple is seen as red-ish both under incandescent and daylight, you could probably still tell the slight difference.

Critically, von Kries chromatic adaptation operates in the LMS cone space. That is, the chromatic adaptation matrix is applied to the cone responses. If a color stimulus is expressed in other color spaces, e.g., CIE XYZ, we would have to first transform the color to the cone space before the precise von Kries adaptation can be applied.

Finally, it is worth noting that in practice we don’t fully adapt even for white colors either. However, von Kries chromatic adaptation works remarkably well in most cases, and is what we will assume for the rest of this article. More complicated chromatic adaptation mechanisms can be found in [Reinhard et al. 2008](https://dl.acm.org/doi/book/10.5555/1386684) and [Fairchild 2013](https://onlinelibrary.wiley.com/doi/book/10.1002/9781118653128).

### 1.2 From Chromatic Adaptation to Color Appearance Models

Hopefully it is clear by now that colorimetry itself can’t fully explain subjective color perception, which depends on not only the XYZ tristimulus values of a color stimulus but also the viewing condition. Chromatic adaptation moves one step toward subjective color perception by considering the illuminant.

Illuminant, however, is just one aspect of viewing condition. In reality, many other aspects of the viewing condition will affect the color perception. For instance, the immediate proximal field and the background of the color stimulus would also affect the color perception. The best example is perhaps the simultaneous lightness contrast effect (Figure 11.7 in [Reinhard et al. 2008](https://dl.acm.org/doi/book/10.5555/1386684)).

The previous adaptation states, i.e., the recent history of viewing conditions, also affect the color perception. A clever demonstration is the recent finding that [chromatic adaptation is irreversible](https://onlinelibrary.wiley.com/doi/abs/10.1002/col.22228).

The subjective color perception is also very much dependent on the cognitive state of the observer. For instance, when observing an orange-ish wall, the color perception will be very different if the observer thinks the wall itself is orange to begin with versus if the observer thinks that the wall is being illuminated by street lights. [Figure 11.6 in Reinhard et al.]

Color appearance model comprehensively takes into account different aspects of the viewing condition, which is beyond the scope of this article. We here focus on just the chromatic adaptation aspect of color appearance modeling.

### 1.3 Emissive vs. Reflective Media

Sometimes you will see people say that we adapt to the white point of a color space, or we adapt to the white point of a display. What do they mean? Don’t we adapt to the illuminant? Why all of a sudden we are adapting to the display or a color space?

It turns out the way we adapt changes slightly depending on whether we are looking at an emissive media, e.g., a computer display, versus a reflective media, e.g., a piece of white paper. When viewing a surface color, the observer is adapted to the surrounding illuminant. When viewing a display, the observer is (primarily) adapted to the display white point (i.e., the white point of the color space currently used by the display). 

For instance if we are viewing a display that uses the sRGB color space, we adapt to D65. Why so? Intuitively, we adapt to the display white point because that’s the prevailing/predominant color to the observer, similar to the effect of an illuminant. But what if a white pixel is not even presented to the viewer? How can we still adapt to the white point of the display? The explanation is debatable and sometimes a mystery, but [the theory](https://www.sciencedirect.com/science/article/pii/S2352154619300336) is that we could estimate the equivalent illuminant from familiar objects in the images, essentially a series of visual cues.

Since the reference white of a color space is usually chosen from the CIE Standard Illuminants, which closely resemble common light sources (see [Table 3](https://www.babelcolor.com/index_htm_files/A%20review%20of%20RGB%20color%20spaces.pdf) in an article from BabelColor), we sometimes just mix the two cases together. But they are different precisely speaking: one is adapting to the chosen reference white on a display device  (usually a standard illuminant), and the other is adapting to the light source.

One final note: when we look at a display under some illuminant, we actually have a “[mixed adaptation](https://www.osapublishing.org/oe/fulltext.cfm?uri=oe-27-3-2855&id=404429)”, which can be thought of as a weighted sum between adapting to the display white point and the illuminant. Roughly, the adaptation is weighted more toward the display white, but when the display is relatively dimmer and dimmer (e.g., viewing a smartphone display under sunlight), it might have less effect on adaptation. [Apple’s True Tone Display](https://www.pocket-lint.com/tablets/news/apple/137264-what-is-apple-true-tone-display) is partially based on the notion of mixed adaptation. True Tone displays try to provide a consistent color appearance across different lightning environments by [changing the display white according to the ambient lighting](https://onlinelibrary.wiley.com/doi/abs/10.1002/sdtp.13057).

## 2. Usecase A: Cross-Platform Color Reproduction

Suppose there is a color patch on an sRGB display (D65 white point), and we want to print out the color patch. Assuming that the print will be viewed under a D50 illuminant, what color (in terms of the XYZ tristimulus values) should we print so that the color appearance on the print when viewed under D50 matches the color appearance of the patch when viewed on the display? This is a typical real-world problem where the color appearance needs to precisely be maintained when viewed under different viewing conditions.

Let’s denote the tristimulus values of the color patch on the sRGB display \[X, Y, Z\]<sub>D65</sub>, where the subscript D65 indicates the adaptation state when the color is viewed from the display, and denote the tristimulus values of the color to be printed/calculated \[X, Y, Z\]<sub>D50</sub>, where the subscript D50, again, indicates the adaptation state when the print is viewed. We can use the basic chromatic adaptation theory described above to derive \[X, Y, Z\]<sub>D50</sub>, as shown below.

<img src="https://latex.codecogs.com/svg.latex?\begin{bmatrix}&space;\frac{1}{L_{D65}},&space;0,&space;0\\&space;0,&space;\frac{1}{M_{D65}},&space;0\\&space;0,&space;0,&space;\frac{1}{S_{D65}}\\&space;\end{bmatrix}&space;\times&space;T_{xyz2lms}&space;\times&space;\begin{bmatrix}&space;X\\&space;Y\\&space;Z\\&space;\end{bmatrix}_{D65}&space;=&space;\begin{bmatrix}&space;\frac{1}{L_{D50}},&space;0,&space;0\\&space;0,&space;\frac{1}{M_{D50}},&space;0\\&space;0,&space;0,&space;\frac{1}{S_{D50}}\\&space;\end{bmatrix}&space;\times&space;T_{xyz2lms}&space;\times&space;\begin{bmatrix}&space;X\\&space;Y\\&space;Z\\&space;\end{bmatrix}_{D50}" title="\begin{bmatrix} \frac{1}{L_{D65}}, 0, 0\\ 0, \frac{1}{M_{D65}}, 0\\ 0, 0, \frac{1}{S_{D65}}\\ \end{bmatrix} \times T_{xyz2lms} \times \begin{bmatrix} X\\ Y\\ Z\\ \end{bmatrix}_{D65} = \begin{bmatrix} \frac{1}{L_{D50}}, 0, 0\\ 0, \frac{1}{M_{D50}}, 0\\ 0, 0, \frac{1}{S_{D50}}\\ \end{bmatrix} \times T_{xyz2lms} \times \begin{bmatrix} X\\ Y\\ Z\\ \end{bmatrix}_{D50}" />

The left side of the equation is the adapted LMS cone responses of the color patch when viewing on the sRGB display. The right size of the equation is the adapted LMS cone responses when viewing on the print under the D50 illuminant. The equation holds because the color appearance of the color patch is to be maintained. T<sub>xyz2lms</sub> is the linear transformation from the XYZ color space and the LMS color space, which is necessary because the von Kries chromatic adaptation operates in the LMS cone space, whereas we are given the XYZ tristimulus.

Solving the equation gives \[X, Y, Z\]<sub>D50</sub>:

<img src="https://latex.codecogs.com/svg.latex?\begin{bmatrix}&space;X\\&space;Y\\&space;Z\\&space;\end{bmatrix}_{D50}&space;=&space;T_{xyz2lms}^{-1}&space;\times&space;\begin{bmatrix}&space;\frac{L_{D50}}{L_{D65}},&space;0,&space;0\\&space;0,&space;\frac{M_{D50}}{M_{D65}},&space;0\\&space;0,&space;0,&space;\frac{S_{D50}}{S_{D65}}\\&space;\end{bmatrix}&space;\times&space;T_{xyz2lms}&space;\times&space;\begin{bmatrix}&space;X\\&space;Y\\&space;Z\\\end{bmatrix}_{D65}" title="\begin{bmatrix} X\\ Y\\ Z\\ \end{bmatrix}_{D50} = T_{xyz2lms}^{-1} \times \begin{bmatrix} \frac{L_{D50}}{L_{D65}}, 0, 0\\ 0, \frac{M_{D50}}{M_{D65}}, 0\\ 0, 0, \frac{S_{D50}}{S_{D65}}\\ \end{bmatrix} \times T_{xyz2lms} \times \begin{bmatrix} X\\ Y\\ Z\\ \end{bmatrix}_{D65}" />

This allows us to “predict” what color to produce to match a particular color appearance. Whether \[X, Y, Z\]<sub>D50</sub> can actually be produced in the print is a completely different matter known at gamut mapping, which is beyond the scope of this article.

An interesting observation from this calculation is that even if the constant adapted responses \[Q, P, R\]<sup>T</sup> don’t take the value of \[1, 1, 1\]<sup>T</sup>, the result would still be the same because Q, P, and R will get canceled in the calculation.

Not surprisingly, chromatic adaptation is critical to color management, whose central goal is to maintain color appearance across media. In fact, ICC color management introduces the notion of working space, which, in very simple terms, is basically a color space (in the colorimetry sense) combined with a viewing (reference) illuminant, the combination of which uniquely specifies the appearance of a color. ICC specifies D50 as the reference illuminant for all its working space and the [Profile Connection Space](http://www.color.org/profile.xalter) (PCS). All ICC profiles also use D50 as the reference illuminant. [Why](https://ninedegreesbelow.com/photography/xyz-rgb.html#ICC)? “*Because the ICC is heavily oriented toward facilitating the making of prints on paper, and D50 is the preferred reference white for evaluating prints on paper.*”

Of course, using D50 as the reference illuminant in a working space or the PCS doesn’t mean that only D50 can be used as the viewing illuminant in practice. It simply means that the associated tristimulus values are pre-adapted to D50; they can certainly be freely adapted to other illuminants as needed. Remember, ICC deals with color management, and so requires a color appearance model rather than just a colorimetry model.

### 2.1 Another Perspective

A useful way to look at the chromatic adaptation process above is that we have essentially derived a new color space from the sRGB color space, where every color \[X, Y, Z\]<sub>D50</sub> in the new color space when viewed under D50 (i.e., when eyes adapt to D50) will have the same color appearance as that of \[X, Y, Z\]<sub>D65</sub> when viewed under D65 (i.e., when eyes adapt to D65), if \[X, Y, Z\]<sub>D50</sub> and \[X, Y, Z\]<sub>D65</sub> satisfy the equation above.

The color gamut of this new D50-adapted sRGB color space is still a parallelepiped in the XYZ color space, because the transformation is linear. It’s just that the primaries and the white point’s XYZ values have changed.

Specifically, the xy chromaticities of the three primaries and the white point (D65) in [D65-adapted (i.e., the original) sRGB](https://en.wikipedia.org/wiki/SRGB) is:

The xy chromaticities of the three primaries and the white point (D50) in [D50-adapted sRGB](https://ninedegreesbelow.com/photography/xyz-rgb.html#table2) is:

[Figure]

The figure above just plots the xy chromaticities of the primaries under D65 and D50 adaptations. We can see that the adaptation from D65 to D50 is not dramatic but noticeable. An interesting observation is that, in changing the illuminant from D65 to D50, the primaries move roughly along the same direction from D65 to D50.

## 3. Usecase B: Color Correction in Camera Imaging

Another interesting usecase of chromatic adaptation is to help obtain the [color correction matrices](http://www.strollswithmydog.com/determining-forward-color-matrix/) in the camera imaging pipeline under different illuminants. Note that color correction by itself is not the same as chromatic adaptation, but the latter can make the former easier.

### 3.1 What is Color Correction?

Briefly, the spectral sensitivity functions (in RGB) of any camera are most likely different from the color matching functions of any known color space. Therefore, it is necessary to convert the raw tristimulus values captured by the camera from the camera-native color space to a known color space, say XYZ, for subsequent processing. This transformation is also called color correction, and is typically done through a linear transformation, just like how most color space conversions are done.

Determining the transformation matrix, a.k.a., color correction matrix (CCM), is done as follows. First we take a picture of a set of color patches, whose spectral reflectances function (SRF) are known, under a given illuminant, say D50. A common set of color patches are the [Macbeth ColorChecker](https://en.wikipedia.org/wiki/ColorChecker), which contains 24 different patches. For each color patch i, we 1) read the corresponding raw tristimulus values \[R<sub>i</sub>, G<sub>i</sub>, B<sub>i</sub>\] in the camera-native color space (after demosaicking) and 2) calculate its XYZ tristimulus values \[X<sub>i</sub>, Y<sub>i</sub>, Z<sub>i</sub>\] using the illuminant’s SPD and the patch’s SRF.

An ideal CCM would perfectly convert each \[R<sub>i</sub>, G<sub>i</sub>, B<sub>i</sub>\] to the corresponding \[X<sub>i</sub>, Y<sub>i</sub>, Z<sub>i</sub>\], hence the equation below:

<img src="https://latex.codecogs.com/svg.latex?\begin{bmatrix}&space;X_1&space;&&space;...&space;&&space;X_{24}&space;\\&space;Y_1&space;&&space;...&space;&&space;Y_{24}&space;\\&space;Z_1&space;&&space;...&space;&&space;Z_{24}&space;\\&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;T_{00},&space;T_{01},&space;T_{02}\\&space;T_{10},&space;T_{11},&space;T_{12}\\&space;T_{20},&space;T_{21},&space;T_{22}\\&space;\end{bmatrix}&space;\times&space;\begin{bmatrix}&space;R_1&space;&&space;...&space;&&space;R_{24}&space;\\&space;G_1&space;&&space;...&space;&&space;G_{24}&space;\\&space;B_1&space;&&space;...&space;&&space;B_{24}&space;\\&space;\end{bmatrix}" title="\begin{bmatrix} X_1 & ... & X_{24} \\ Y_1 & ... & Y_{24} \\ Z_1 & ... & Z_{24} \\ \end{bmatrix} = \begin{bmatrix} T_{00}, T_{01}, T_{02}\\ T_{10}, T_{11}, T_{12}\\ T_{20}, T_{21}, T_{22}\\ \end{bmatrix} \times \begin{bmatrix} R_1 & ... & R_{24} \\ G_1 & ... & G_{24} \\ B_1 & ... & B_{24} \\ \end{bmatrix}" />

Of course color correction isn’t going to be perfect (because the camera spectral sensitivities have to be designed with a set of practical constraints), our goal here is to find the CMM to minimize the conversion error. This is formulated as a linear optimization problem (e.g., linear least squares). If the CCM is perfect, that is, the camera raw color space is precisely a linear transformation away from XYZ, then the camera is said to satisfy the [Luther Condition](https://en.wikipedia.org/wiki/Tristimulus_colorimeter#:~:text=A%20camera%20or%20colorimeter%20is,of%20the%20filters%20is%20a) and that the camera space is colorimetric (in that we can use the camera to measure color).

### 3.2 The Challenge

So far so good. The challenge, however, is that the CCM depends on the illuminant under which the color patch image is taken. This is because the illuminant dictates the raw tristimulus values taken by the camera as well as the XYZ tristimulus values. Modern cameras usually calculate (pre-calibrate) different CCMs for a set of common illuminants. When the capturing illuminant is different from the preset illuminants, the camera imaging pipeline would usually [interpolate the CCM](https://openaccess.thecvf.com/content_cvpr_2018/papers/Karaimer_Improving_Color_Reproduction_CVPR_2018_paper.pdf) using the CCMs of the pre-calculate illuminants.

So if we want to consider, say, N different illuminants, we would need to calculate N CCMs. This is conceptually easy by taking N different measurements, one for each illuminant. The interesting question is what if you don’t know the spectral reflectances of the color patches, which we need to get the “ground-truth” XYZ tristimulus values for a particular illuminant?

### 3.3 Chromatic Adaptation to the Rescue

Here is the interesting thing. The color patches on the [Macbeth ColorChecker](https://en.wikipedia.org/wiki/ColorChecker) are chosen “*to have consistent color appearance under a variety of lighting conditions.*” To what extent this is true is debatable, but for some illuminants that are not far apart (e.g., D50 and D65) we can roughly assume that color patches have similar color appearance under them. What this means is that for each patch, given the XYZ values under one illuminant, we can predict the XYZ values of a color patch under a different illuminant using chromatic adaptation without knowing the spectral reflectance of the color patch!

As an example, Section 2.1.5 in [this](https://www.babelcolor.com/index_htm_files/A%20review%20of%20RGB%20color%20spaces.pdf) article from BabelColor uses chromatic adaptation to predict the XYZ values of a color patch (Blue Flower) in the ColorChecker under D50 given the XYZ values of the same patch under D65. This is feasible because the color patch is carefully picked such that it has the same color appearance under D50 and D65. Essentially, this becomes a cross-media color reproduction problem (usecase 1), where we calculate the XYZ values viewed under D50 that would match the color appearance of the given XYZ values viewed under D65. Since the patch maintains a constant color appearance under both illuminants, the actual measured XYZ values under D50 match very closely the predicted XYZ values through chromatic adaptation, as shown in the article (\[24.60, 23.40, 34.54\]<sub>predicted</sub> vs. \[24.64, 23.41, 34.43\]<sub>measured</sub>).

A small detail to note is that the BabelColor article uses Bradford chromatic adaptation, which is conceptually the same as von Kries chromatic adaptation albeit with differences that make it more reliable. But you get the idea.

**A big caveat here**: using chromatic adaptation to estimate the XYZ values of an illuminant using the XYZ values of another illuminant is just a rough estimation, because it assumes that the color appearance of the object is the same between the two illuminants, which is sort of OK if the two illuminants are similar, e.g., D65 and D50, but generally not true; you would not produce very precise results if you want to get XYZ under A from D65. The ability of an object to maintain constant color appearance under illuminants is measured by the [Color Inconstancy Index](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1478-4408.2003.tb00184.x) (CII). As you can see, the Blue Flower patch even under the very similar D50 and D65 illuminants still has a non-zero CII since the predicted and measured XYZ under D50 are not the same.

## 4. Usecase C: White Balance in Camera Imaging

A key challenge in digital camera imaging is white balance, which arises from the fact that the capturing illuminant &Phi;<sub>c</sub> when a photo is taken by the photographer is mostly different from the viewing illuminant &Phi;<sub>v</sub> when the photo is viewed by a viewer. Why does this matter? The photographer is adapted to &Phi;<sub>c</sub> while the camera, without care, would not adapt and simply tries to accurately capture the XYZ/LMS values of the scene points (which more of less is the case if we have a good color correction mechanism); when the photo captured by the camera is presented to a user, the user will adapt to &Phi;<sub>v</sub>. Therefore, the user will not perceive the color the same as what the photographer perceives when the photo is taken.

### 4.1 White Balance Using Chromatic Adaptation

The goal of white balance is to adjust the raw camera RGB values in such a way that the photo, when presented to the viewer under &Phi;<sub>v</sub>, will have the same color appearance as that of the original scene under &Phi;<sub>c</sub>. This is illustrated in the figure below, where \[R, G, B\]<sup>T</sup> denotes the raw camera RGB values (before color correction) and \[R<sub>wb</sub>, G<sub>wb</sub>, B<sub>wb</sub>\]<sup>T</sup> denotes the white balanced RGB, which is to be calculated. D<sub>c</sub> and D<sub>v</sub> in the figure denote the chromatic adaptation matrix under &Phi;<sub>c</sub> and &Phi;<sub>v</sub>, respectively.

[Figure]

\[R<sub>wb</sub>, G<sub>wb</sub>, B<sub>wb</sub>\]<sup>T</sup> can be calculated using chromatic adaptation. The only tricky thing here is that we don’t know the cone responses of the photographer before being adapted to &Phi;<sub>c</sub>. However, we do know the raw camera RGB values, from which we can calculate the pre-adapted cone responses of the photographer using the color correction matrix, which is denoted as T<sub>cam2lms</sub> in the figure above (technically CCM is Tcam2xyz, but it’s easier to go from XYZ to LMS --- a linear transformation). The rest is formality. The equations below show how to calculate \[R<sub>wb</sub>, G<sub>wb</sub>, B<sub>wb</sub>\]<sup>T</sup> from \[R, G, B\]<sup>T</sup>.

<img src="https://latex.codecogs.com/svg.latex?D_v&space;\times&space;T_{cam2lms}&space;\times&space;\begin{bmatrix}&space;R_{wb}\\&space;G_{wb}\\&space;B_{wb}\\&space;\end{bmatrix}&space;=&space;D_c&space;\times&space;T_{cam2lms}&space;\times&space;\begin{bmatrix}&space;R\\&space;G\\&space;B\\&space;\end{bmatrix}" title="D_v \times T_{cam2lms} \times \begin{bmatrix} R_{wb}\\ G_{wb}\\ B_{wb}\\ \end{bmatrix} = D_c \times T_{cam2lms} \times \begin{bmatrix} R\\ G\\ B\\ \end{bmatrix}" />

<img src="https://latex.codecogs.com/svg.latex?\begin{bmatrix}&space;R_{wb}\\&space;G_{wb}\\&space;B_{wb}\\&space;\end{bmatrix}&space;=&space;T_{cam2lms}^{-1}&space;\times&space;D_v^{-1}&space;\times&space;D_c&space;\times&space;T_{cam2lms}&space;\times&space;\begin{bmatrix}&space;R\\&space;G\\&space;B\\&space;\end{bmatrix}" title="\begin{bmatrix} R_{wb}\\ G_{wb}\\ B_{wb}\\ \end{bmatrix} = T_{cam2lms}^{-1} \times D_v^{-1} \times D_c \times T_{cam2lms} \times \begin{bmatrix} R\\ G\\ B\\ \end{bmatrix}" />

Note that we are not considering observer metamerism here. That is, we assume that the cone responses of the photographer and the viewer are the same. If not, the matter will be much more complicated, which is beyond the scope of this article.

### 4.2 Auto White Balance, a.k.a., Estimating Capturing Illuminant

Looking at the equation above, to use the precise von Kries chromatic adaptation for white balance, we need to know four things: 1) the raw camera RGB values, 2) the CCM (i.e., T<sub>cam2lms</sub>), 3) the viewing illuminant &Phi;<sub>v</sub> (which determines D<sub>v</sub>), and 4) the capturing illuminant &Phi;<sub>c</sub> (which determines D<sub>c</sub>).

Let’s focus on the 4th item, the capturing illuminant, for now. Perhaps the most difficult part in white balancing is to estimate the capturing illuminant &Phi;<sub>c</sub>, which is usually quite difficult to know precisely. In fact, the main job of an auto white balance (AWB) is to (implicitly or explicitly) estimate the capturing illuminant. Some AWB methods use a separate sensor that measures the illuminant and keeps the illuminant as the metadata. Others use pure computational methods/heuristics. Perhaps not surprisingly people even [train Deep Neural Networks to estimate capturing illuminant](https://bmvc2019.org/wp-content/uploads/papers/0105-paper.pdf).

Alternatively, we could manually estimate the capturing illuminant -- if we are lucky. If the scene that you are capturing happens to contain a (near) neutral point such as a piece of white paper or a white wall, then the XYZ values of the illuminant should be the same as the XYZ values of the neutral point as captured by the camera, which we can estimate from the raw camera RGB values of the neutral point. This is why cameras (and smartphone cameras with the help of professional camera Apps) will allow you to manually specify a white point. This helps estimating the capturing illuminant immensely.

### 4.3 Simplified White Balance Using “von Kries-style” Scaling

Even if an AWB algorithm can sort of estimate &Phi;<sub>c</sub>, the other three things might not be known depending on where and when the white balance is performed; some might never be precisely known. This is especially true if white balance is performed in a post-processing software on an sRGB-exported image without the raw camera RGB values. In that case, we would need to recover the raw camera RGB values from the sRGB image, which is non-trivial: even if we know the CCM and invert it, there are other stages in the imaging pipeline that are not invertible (e.g., demosaic, quantization).

The precise white balance using von Kries adaptation is rarely implemented by (smartphone) cameras. Instead, what gets used are various simplified white balance algorithms, which directly scale the raw camera RGB values or the sRGB encoded values rather than converting them to the LMS cone space.

As an example, here are four steps in a simplified white balance algorithm operating in the sRGB color space.
1. Specify a neutral point in the image, a point that you think should look white. The sRGB values of that point should be \[255, 255, 255\]<sup>T</sup> after white balance.
2. Get the unadapted sRGB values for that point in the image, let’s say it’s \[190, 220, 200\]<sup>T</sup>.
3. Calculate the adaptation matrix D, which should satisfy: \[255, 255, 255\]<sup>T</sup> = D x \[190, 220, 200\]<sup>T</sup>. In this case, D would be \[\[255/190, 0, 0\]<sup>T</sup>, \[0, 255/220, 0\]<sup>T</sup>, \[0, 0, 255/200\]<sup>T</sup>\]
4. We then apply D to all other pixels in the image.

Note that directly scaling the sRGB values most likely will not generate a correct white-balanced image. See examples [here](https://www.eecs.yorku.ca/~mbrown/ICCV19_Tutorial_MSBrown.pdf#page=192). Nevertheless, if you can’t invert the entire imaging pipeline or you don’t know the CCM, scaling sRGB values sometimes is the best you can do.

As another example, here are four white-balancing steps that directly scales the raw RGB values. Here we assume that the raw RGB values are normalized between \[0, 1\], expressed in floating point numbers, i.e., before quantization.
1. Specify a neutral point in the image, a point that you think should look white. The raw RGB values of that point should be \[1, 1, 1\]<sup>T</sup> after white balance.
2. Get the unadapted rawr RGB values for that point under the current illuminant, let’s say it’s \[0.90, 0.80, 0.95\]<sup>T</sup>.
3. Calculate the adaptation matrix D, which should satisfy: \[1, 1, 1\]<sup>T</sup> = D x \[0.90, 0.80, 0.95\]<sup>T</sup>. In this case, D would be \[\[1/0.90, 0, 0\]<sup>T</sup>, \[0, 1/0.80, 0\]<sup>T</sup>, \[0, 0, 1/0.95\]<sup>T</sup>\]
4. We then apply D to all other pixels in the image.

As you can see, the simplified white balance algorithm still uses a “von Kries-style” transformation, i.e., scaling the RGB values using a diagonal matrix, but it’s merely for the engineering convenience rather than strictly following von Kries chromatic adaptation.

### 4.4 Aside: White Balance and Color Correction --- Which Comes First?

If based strictly on theories in color science, color correction should be performed before white balance, because white balance technically should operate in the LMS cone space and so we need color correction to transform data from the camera raw space to the XYZ/cone space.

However, more often than not cameras perform white balance before color correction, where white balancing is done by scaling the raw RGB values as demonstrated above. You might be wondering, isn’t this incorrect? Well, yes and no. If we still use the original CCM then the result is incorrect. Therefore, the CCM here needs to be modified a bit by taking into account the raw RGB scaling matrix. More details can be found in [this great article by Andrew Rowlands](https://www.spiedigitallibrary.org/journals/optical-engineering/volume-59/issue-11/110801/Color-conversion-matrices-in-digital-cameras-a-tutorial/10.1117/1.OE.59.11.110801.full?SSO=1).

According to Rowloads, there are two main incentives for doing white balance in RAW before converting color to the XYZ space.

First, the CCM is only approximate and contains error: “In particular, color errors that have been minimized in a nonlinear color space such as CIE LAB will be unevenly amplified, so the color conversion will no longer be optimal.” White-balancing in raw RGB minimizes the impact of color correction errors on white balancing.

Second, doing white balance in RAW allows the subsequent color correction to be less dependent on illuminant, which simplifies camera design and improves color rendering quality. Recall that the traditional CCM is heavily dependent on the illuminant (see Figure 5 in [Rowlands’ article](https://www.spiedigitallibrary.org/journals/optical-engineering/volume-59/issue-11/110801/Color-conversion-matrices-in-digital-cameras-a-tutorial/10.1117/1.OE.59.11.110801.full?SSO=1), which I copy and paste below), and online interpolation is needed if the capturing illuminant is very different from the pre-calibrated illuminants, which typically need to be a lot in order to minimize the interpolation error.

It turns out that if we were to scale the raw RGB first (e.g., white balancing in raw), the new CCM will be much more stable under different illuminants (see Figure 8 in [Rowlands’ article](https://www.spiedigitallibrary.org/journals/optical-engineering/volume-59/issue-11/110801/Color-conversion-matrices-in-digital-cameras-a-tutorial/10.1117/1.OE.59.11.110801.full?SSO=1), which I again copy and past below). Compare this figure with the figure above; you can see that the coefficients in the new CCM have much less variation across illuminant CCTs. That way, cameras can afford to have CCMs for just a few pre-calibrated illuminants without online interpolation.

According to Rowlands, traditional cameras perform white balance (in raw) before color correction while smartphone cameras and [DCRaw](https://dechifro.org/dcraw/) take the other approach, although there is [evidence](https://karaimer.github.io/camera-pipeline/) that smartphone cameras are increasingly taking the former approach too. [Adobe’s DNG converter](https://helpx.adobe.com/photoshop/using/adobe-dng-converter.html) provides both modes.

## 5. Summary

Many real-world usecases deal with color appearance, ranging from color management to digital camera imaging. In simple terms, color appearance = colorimetry + viewing condition. Illuminant is one of the most important aspects of viewing condition. Chromatic adaptation provides a way to take into account the effect of the illuminant. When we want to specify a particular color appearance, it’s important to specify both the XYZ values and the illuminant.

## Acknowledgement

[Hao Xie](https://www.linkedin.com/in/hao-xie/) from RIT meticulously went through the technical details of this article. [Mike Murdoch](https://sites.google.com/view/michaeljmurdoch/) from RIT helped me better understand Apple's True Tone display. [Andrew Rowlands](http://www.andyrowlands.com/index.html) patiently answered my questions on the interactions between white balance and color correction.
