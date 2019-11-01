

class: center, middle, hide-slide-number, hide-logo
background-image: url(figs/circles.svg)
background-size: cover
background-position: 0 50px
name: title-slide

.titleblock[
# Gaussian Process
]

.authorsblock[
.presenter[Darlan Cavalcante Moreira]

![logo](figs/logo.svg)



![:today]
]

.footnote[Created with [{Remark.js}](http://remarkjs.com/) using
[{Markdown}](https://daringfireball.net/projects/markdown/) +
[{MathJax}](https://www.mathjax.org/)]

---

class:middle, hide-logo, hide-slide-number

# .center[Agenda]



This is the toc

---
layout: true

# Mathematical Backgroubd

---

## Joint, marginal and conditional probabilities

- Joint and conditional probabilities

$$p(A,B) = p(A/B)p(B) = p(B/A)p(A)$$


.happy.labeled.box[
.label[
Bayes Theorem]

$$p(A/B) = \frac{p(B/A)p(A)}{p(B)}$$
]

- Sum Rule

$$p(A) = \sum_{b \in B} p(A/B)$$

$$p(A) = \sum_{b \in B} p(A, B)$$

---

## Likelihood function

- Consider the probability of \$y\$ conditioned to a vector of parameters \$\vtTheta\$
- If we reverse the interpretation of this conditional probability and see it as
  a function of \$\vtTheta\$ for a given fixed \$y\$ we then get the likelihood
  function

$$L(\vtTheta/y) = p(y/\vtTheta)$$

![:note moody](The likelihood function is not a probability density function, but just a function of \$\vtTheta\$)


---

## Gaussian Distribution

- Univariate Gaussian Distribution

$$f(x) = \frac{1}{\sigma \sqrt{2 \pi}} \exp \left(-\frac{1}{2} \left( \frac{x-\mu}{\sigma} \right)^2 \right)$$

- Multivariate Gaussian Distribution

$$\mtX \sim \stN(\vtMu, \mtSigma)$$

$$\vtMu = (\E{\vtX} = \E{\vtX\_1}, \E{\vtX\_2}, \ldots, \E{\vtX\_n})$$

$$\mtSigma\_{ij} = \E{(\vtX\_i - \vtMu\_i)(\vtX\_j - \vtMu\_j)} = \cov(\vtX\_i, \vtX\_j)$$

$$f\_\vtX(\vtX\_1, \vtX\_1, \ldots, \vtX\_n) = \frac{\exp \left( -\frac{1}{2}(\vtX-\vtMu)^T \mtSigma^{-1} (\vtX-\vtMu) \right)}{\sqrt{(2\pi)^k |\mtSigma|}}$$

---

## Gaussian Distribution

.extra-top-bottom-margin.moody.labeled.box[.label[
Properties of Multivariate (MV) gaussian]
- Sum of gaussian is gaussian
- Marginal of gaussian is gaussian
- Conditioning a Gaussian also results in a gaussian
]

- Bivariate Gaussian Distribution

$$f(x,y) = \frac{1}{2\pi\sigma\_x\sigma\_y \sqrt{1-\rho^2}} \exp \left( -\frac{1}{2(1-\rho^2)} \left[ \frac{(x-\mu\_x)}{\sigma\_x^2} + \frac{(y-\mu\_y)}{\sigma\_y^2} - \frac{2\rho(x-\mu\_x)(y-\mu\_y)}{\sigma\_x\sigma\_y} \right]\right)$$

where \$\rho\$ is the correlation between \$x\$ and \$y\$.

---
layout: true

# Bayesian Approach

---

- In the classical statistical estimation the parameter of interest \$\theta\$
  is assumed to be a deterministic, but unknown constant
- In a Bayesian approach \$\theta\$ is assumed to be a random variable whose
  realization we must estimate
- The motivation for using a Bayesian approach is twofold
  - We have some **prior knoledge** about \$\theta\$ that we can incorporate
    into the estimator
  - This requires \$\theta\$ to be a random variable with a given **prior** pdf

![:box moody, Motivating Example](We want to estimate a DC current \$A\$ from
noisy observations \$\vtX = \vtOne A + \vtW\$, where \$\vtW\$ is a noise vector.)

.columns[
.column.w-50.align-middle[
![:box happy, Classic Solution](\$\hat{A} = \overline{\vtX}\$)
]
.column.w-50.align-middle[
![:box happy, Bayesian Solution](\$\hat{A} = \E{A|\vtX}\$)
]]

---

## Prior and posterior

- The **prior** PDF corresponds to our belief about which values \$A\$ can take
  **before** anything is observed
  - It is often assumed as a uniform distribution
- The **posterior** PDF corresponds to our belief about which values \$A\$ can
  take **after** we have observed some data


.columns[
.column.w-50.align-middle[
![:captionedfigure Prior PDF](figs/prior_A.svg)
]
.column.w-50.align-middle[
![:captionedfigure Posterior PDF](figs/posterior_A.svg)
]]

---

## Gaussian PDFs

- Using the Bayes theorem we can write the posterior PDF as

$$p(\vtTheta|x) = \frac{p(x|\vtTheta)p(\vtTheta)}{p(x)} = \frac{p(x|\vtTheta)p(\vtTheta)}{\int p(x|\vtTheta)p(\vtTheta)  d\vtTheta}$$

- This requires a \$p\$ dimensional integration over theta
- The mean still needs to be evaluated requiring another integration over \$\vtTheta\$

.happy.labeled.box[.label[
Assumption]

The prior PDFs are Gaussian
]

- Everything becomes much easier and we can solve the integral
- The posterior also becomes Gaussian

![:note moody](See Kay book, chapter 10, for the proper deriations)



.footnote[Steven M. Kay, *Fundamentals of Statistical Processing: Estimation Theory*]

---

layout: false

# Bayesian Estimator

- With the priors and posterior being Gaussian distributions, theorem 10.2 in
  Kay book give us the expectation and covariance of the posterior as

$$\E{\vtY|\vtX} = \E{\vtY} + \mtC\_{yx}\mtC\_{xx}^{-1}(\vtX - \E{\vtX})$$

$$\mtC\_{\vtY|\vtX} = \mtC\_{yy} - \mtC\_{yx}\mtC\_{xx}^{-1}\mtC\_{xy}$$

<!-- - Notice how our estimation is a weighted average of the input -->

.extra-top-bottom-margin[
![:note moody](The covariance matrix of the conditional PDF does not depend on \$\vtX\$)
]

.extra-top-bottom-margin[
![:note happy](Theorem 10.2 is valid when \$\vtX\$ and \$\vtY\$ are jointly Gaussian)
]

.footnote[Steven M. Kay, *Fundamentals of Statistical Processing: Estimation Theory*]

---

# Bayesian Linear Model

- A data model corresponds to how we model the dependency of the input and output
- It should be complex enough to describe the principal features of the data,
  but simple enough to allow an estimation that is optimal (in the MSE sense)
  and easily implemented
- The Bayesian equivalent of a linear model is defined as
  $$\vtX = \mtH \vtTheta + \vtW$$
  where \$\vtX\$ is a \$N \times 1\$ data, \$\mtH\$ is a known \$N \times p\$ matrix and \$\vtTheta\$ is a \$p \times 1\$ random vector with prior PDF \$\stN(\vtMu\_\vtTheta, \mtC\_\vtTheta)\$, and \$\vtW\$ is an \$N \times 1\$ noisy vector with PDF \$\stN(\vtZero, \mtC\_W)\$ and independent of \$\vtTheta\$



.happy.labeled.box[.label[
Bayesian Estimator]
Theorem 10.3 in Kay book states that for this data model and prior Gaussian PDFs the posterior PDF \$p(\vtTheta | \vtX)\$ is Gaussian with mean and covariance given by
$$\E{\vtTheta|\vtX} = \vtMu\_\vtTheta + \mtC\_\vtTheta \mtH^T(\mtH \mtC\_\vtTheta \mtH^T + \mtC\_\vtW)^{-1}(\vtX - \mtH \vtMu\_\vtTheta)$$
$$\mtC\_{\vtTheta|\vtX} = \mtC\_\vtTheta - \mtC\_\vtTheta \mtH^T(\mtH\mtC\_\vtTheta \mtH^T + \mtC\_\vtW)^{-1} \mtH \mtC\_\vtTheta$$
]

---
# Gaussian Process

- While probability distributions describe random variables, a stochastic
  process describes the properties of **functions**
  - They can be interpreted as distribution over functions

.moody.labeled.box[.label[
Note]

We can loosely think of a function as a long vector, each entry specifying
    \$f(x)\$ at a particular input \$x\$]

- A Gaussian Process (GP) is a generalization of the Gaussian probability
  distribution

.extra-top-bottom-margin.happy.labeled.box[.label[
Definition]

Gaussian processes are distributions over functions \$f(x)\$ of which the
distribution is defined by a mean function \$m(x)\$ and positive definite
covariance function \$k(x,x')\$
]






---

# Gaussian Process

- Gaussian Processes have Bayesian priors
![:note moody](The usual assumption is that the function is always zero with covariance one, **until we see training data showing otherwise**)

---

layout: false

# A slide with Blocks
## Three available colors and labeled vs unlabeled blocks

.center[.happy.alert[lalala lalala lalala]
.moody.alert[lalala lalala lalala]
.angry.alert[lalala lalala lalala]]


<!-- Similar to before, but using HTML to create the box -->
<div class="angry labeled box">
    <div class="label">An angry labeled box</div>
    <p>bla bla bla</p>
    <p>bla bla bla</p>
</div>


[//]: # (Note that the new line after the label is important)
.moody.labeled.box[.label[A moody labeled box]

bla bla bla

bla bla bla]

[//]: # (Similar to before, but using a macro to create the box)
[//]: # (Note that after ':box' you add the classes then add a comma followed by the label)
[//]: # (The limitation of the macro version is that the content cannot have a close parentheses)
![:box happy, A box with a list](
- item 1
- item 2)

[//]: # (If a label is not provided it also works)
![:box moody](
A moody box without a label
- an item)

---

# A slide with Math, footnote and a citation

The equation is given by [.cite[Some guy (2018)]]

$$y = ax^2+bx+c$$

Oops, the equation above is .strike[wrong] imprecise. The correct equation is

$$\vtY = \mtH\vtX+\vtZ$$

![:note angry](You need to escape underlines in equations as `\_` such that they
are not interpret by the markdown parser)

.footnote[The math is typeset using [MathJax](https://www.mathjax.org/)]

---

# A slide with a Code Block

Some python code

```python
for i in range(10):
    print(i)
```

Some C++ code

```c++
#include <iostream>

int main() {
    std::cout << "Hello World" << std::endl;
}
```

---

# A slide with code inside a labeled box
## Same blocks as before, but without padding around the content

<div class="moody zeropadded labeled box">
<span class="label">Some Python Code</span>
<pre><code class="python">for i in range(10):
    print(i)</code></pre>
</div>


.moody.zeropadded.labeled.box[
.label[
Some Python Code]
```python
for i in range(10):
    print(i)
```
]



.moody.zeropadded.labeled.box[
.label[
Some C++ Code]
```c++
#include <iostream>

int main() {
    std::cout << "Hello World" << std::endl;
}
```
]

---

# A slide with two columns

Notice how the columns align and the proportion of each column.

.columns[
.column.w-30.align-middle[
Middle aligned column vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv

vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv
]

.column.w-40[
Not specifically aligned, but it is the largest column aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa

aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa

aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa
]

.column.w-30.align-bottom[
bottom aligned column zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz

zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz
]]


Some text after the columns, which can now again span the whole line

---
# A slide with two columns

Here the `align-middle` class was added to the columns and the last column has the `align-bottom` class

.columns.align-middle[
.column.w-20[
vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv vvv
]

.column.w-30[
aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa aaa
]

.column.w-20[
zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz zzz
]

.column.w-30.align-bottom[
bottom aligned column hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh hhh
]
]

Some text after the columns, which can now again span the whole line

---

# Figures with caption

For this you can use HTML


<div class="columns zeropadded box moody">
  <figure>
    <img src="https://upload.wikimedia.org/wikipedia/commons/6/6e/Emoji_u1f648.svg" alt="Mizaru">
    <figcaption><em>Mizaru</em> (see no evil)</figcaption>
  </figure>
  <figure>
    <img src="https://upload.wikimedia.org/wikipedia/commons/5/56/Emoji_u1f649.svg" alt="Kikazaru">
    <figcaption><em>Kikazaru</em> (hear no evil)</figcaption>
  </figure>
  <figure>
    <img src="https://upload.wikimedia.org/wikipedia/commons/2/2c/Emoji_u1f64a.svg" alt="Iwazaru">
    <figcaption><em>Iwazaru</em> (speak no evil)</figcaption>
  </figure>
</div>


You can also use the `captionedfigure` macro

.zeropadded.box.moody.columns[
![:captionedfigure *Mizaru* (see no evil)](https://upload.wikimedia.org/wikipedia/commons/6/6e/Emoji_u1f648.svg)

![:captionedfigure *Kikazaru* (hear no evil)](https://upload.wikimedia.org/wikipedia/commons/5/56/Emoji_u1f649.svg)

![:captionedfigure *Iwazaru* (speak no evil)](https://upload.wikimedia.org/wikipedia/commons/2/2c/Emoji_u1f64a.svg)
]

---

class: inverse, center, middle

# A slide with the inverse class
## Isn't it great?


.extra-top-bottom-margin[
Usually you want to also add the 'center' and 'middle' classes to the slide.]

.footnote[When both inverse and middle classes are added to a slide the font size of paragraph is larger and h1 and h2 font is different.]

---

class: img-caption

![image](figs/a_beautiful_picture.jpg)


This is a beautiful picture with a caption below.


---

background-image: url(figs/a_beautiful_picture.jpg)
background-size: cover

class: inverse

# A slide with a background image

We can have content as before.

- item 1
- item 2

Because this image is somewhat dark, we also add the inverse class to change the
colors. But this might not be enough.


---
background-image: url(figs/a_beautiful_picture.jpg)
background-size: cover
background-color: rgb(255, 255, 255, 0.8)

class: overlay-blend, hide-logo

# A slide with a background color combined with a background image

Now we have the same image as before, but we also set the background color and
add the overlay-blend class to the slide.

![:note angry](The background color needs to have an opacity value greater than
zero and lower than one in order for the overlay to work)

---


# A slide with a table


.auto-left-right-margin.extra-top-bottom-margin[

| Some text         | Some text              | Some text      |   Some text       |
| :---------------: | :--------------------: | :------------: | :---------------: |
| some text some    | some text              | some text      | some text         |
| some text         | some text some text    | text           | text              |
| some text some    | text                   | some text      | text              |
]


---

# You can combine columns and figs

.columns.align-middle[
.column.w-40[
.moody.labeled.box[
.label[Lala]

- Some text
- Some text
]
.happy.labeled.box[
.label[Lala]

- Some text
- Some text
]
.angry.labeled.box[
.label[Lala]

- Some text
- Some text
]
]
.column.w-40[
![:captionedfigure *Mizaru* (see no evil)](https://upload.wikimedia.org/wikipedia/commons/6/6e/Emoji_u1f648.svg)

![:captionedfigure *Kikazaru* (hear no evil)](https://upload.wikimedia.org/wikipedia/commons/5/56/Emoji_u1f649.svg)

![:captionedfigure *Iwazaru* (speak no evil)](https://upload.wikimedia.org/wikipedia/commons/2/2c/Emoji_u1f64a.svg)
]]

---

# Animations

- Simply showing an item at a time can be done in remarkjs when you separate content using `--`

--


![:box moody, Example](Like this box, that is only appearing now)

--

- If you add the `css/slide_transitions.css` in `index.html` then slide transitions will have a fade in effect.

--

.animated.bounceInLeft[
## One more thing]

.animated.bounceInRight[
- You can also use CSS animations for some extra goodies
- For instance, with `animate.css` all you need to do is to add the `animated`
  class as well as `<desired_animation_class>` to an element
  - See https://daneden.github.io/animate.css/ for available animations
  - See more in https://github.com/daneden/animate.css for other options .alert.angry[(speed, delay, etc)]
  - You can also use other css code from the internet or create you own
- .alert.angry[Make sure the element is a block element for the animation to work]
  - Or set it's display property to inline-block
]

---

class: animated, jackInTheBox

# You can also animate the transition of a specific slide

- Just add `animated` and `<desired_animation_class>` as slide classes
  - This slide has the `animated` and `jackInTheBox` classes
--

class: dontanimate
- Remember to add the `dontanimate` class to the first step to stop animating
  the whole slide
--

- See how this step didn't animate the whole slide again?
--

- All subsequent steps will not be animated
--

- You can also create a template with animation using remark `template` such
  that for specific slides you just use the template with the desired animation
  - Ex: create a template for splash slides!



---
name: more-animations-1

# JavaScript animations on specific slides

[//]: # (.animated.slideInDown[)
[//]: # (bla bla bla)
[//]: # (])

- If you need to animated something with JavaScript, you can name each slide and
then use the `showSlide` event to test for a specific name and call the
JavaScript code
  - This slide has name `more-animations-1`
  - When you name a slide, the slide DOM element gets its id attribute set to slide-<name>
--

name: more-animations-2
- Notice that you can give a name property to each individual slide step
  - This step has the name `more-animations-2` and a log message saying "Yahooo more-animations-2" is sent only on this step
--

name: more-animations-3

<div style="background-color: rgb(230, 230, 230); width: calc(200px); border: 4px solid purple;">
<div id="square" style="width: 100px; height: 100px; background-color: red; position: relative; transition: transform 0.8s;"></div>
<div id="animationStep" style="font-size: 75%; float:right; margin-top: 4px;"></div>
</div>

- In this step we added a square that will be moved with javascript.

  In the `showSlide` event for this step we capture the left/right arrow
  keypress events for the square movement. When the in the end of the movement
  we reenable the events in remarkjs

---

# Pull-left, Pull-right

.pull-right.width-30[![squares](figs/squares.svg)]


- Etiam vel neque nec dui dignissim bibendum.
- Proin neque massa, cursus ut, gravida ut, lobortis eget, lacus. Mauris ac felis vel velit tristique imperdiet.



.clear-both[
This text has a clear-both class and will definitely not be around the squares]

---
name: chartslide
# A chart example

- Notice that the chart is interactive ➤ Try clicking the lagend
  - You could also add more interaction such as new data appearing and updating
    the line, for instance

<div class="chart-container" style="width:80%; height: 400px; margin: 50px auto;">
<canvas id="myChart"></canvas>
</div>


---

# More css

Some other elements with css are:
- kbd: <kbd>Ctrl</kbd> <kbd>y</kbd>
- Blockquote:
  > Two things are infinite: the universe and human stupidity; and I'm not sure about the universe.
  > -- <cite>Albert Einstein</cite>

---

# TIPs

- You can get nice general pictures from specialized sites such as https://unsplash.com/
  - Better than just googling

---
# References

<div id="bibtex_display"></div>

---
class: middle, center, hide-slide-number, hide-logo

.typewriter[
# Thank You
]

.animated.fadeIn.delay-2s[
Scan this code for this presentation URL

![:qrcode](https://darcamo.github.io/remarkjs_template)
]

.contactblock[
.presenter[Darlan Cavalcante Moreira: [darlan@gtel.ufc.br](mailto:darlan@gtel.ufc.br)]<br />
.home[GTEL - Wireless Telecom Research Group]<br />
.webpage[https://www.gtel.ufc.br]<br />
.location[Fortaleza, Brazil]
]
