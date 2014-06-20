---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [pysf, matplotlib]
---
{% include JB/setup %}

## Abstrace
I report about contour(..) function in matplotlib that I searched yesterday。I also test jekyll markup usages.

## A seff document of countor(..) function
We should execute info(contour) at first.

<center><b>PythonSf ワンライナー</b></center>
<pre>
import matplotlib.pyplot as plt; info(plt.contour)
 contour(*args, **kwargs)

:func:`~matplotlib.pyplot.contour` and
:func:`~matplotlib.pyplot.contourf` draw contour lines and
filled contours, respectively.  Except as noted, function
signatures and return values are the same for both versions.

:func:`~matplotlib.pyplot.contourf` differs from the MATLAB
version in that it does not draw the polygon edges.
To draw edges, add line contours with
calls to :func:`~matplotlib.pyplot.contour`.


call signatures::

  contour(Z)

make a contour plot of an <b>2D</b> array *Z*. The level values are chosen
automatically.
::

  contour(X,Y,Z)

*X*, *Y* specify the (*x*, *y*) coordinates of the surface

::

  contour(Z,N)
  contour(X,Y,Z,N)

contour *N* automatically-chosen <b>N point</b> levels <b>that could not be shown partly</b>.

::

  contour(Z,V)
  contour(X,Y,Z,V)

draw contour lines at the values specified in sequence *V*
</pre>

The result of info(contour) has too many lines:180, so I hide remaining parts below. 

<div id="div_1">
<p><input type="button" value="show the remaing part" style="WIDTH:150px"
   onClick="document.getElementById('div_2').style.display='block';
            document.getElementById('div_1').style.display='none'"></p>
</div>
<div id="div_2" style="display:none">
<p><input type="button" value="hide the remaining part" style="WIDTH:150px"
   onClick="document.getElementById('div_2').style.display='none';
            document.getElementById('div_1').style.display='block'"></p>
<pre>

::

  contourf(..., V)

fill the (len(*V*)-1) regions between the values in *V*

::

  contour(Z, **kwargs)

Use keyword args to control colors, linewidth, origin, cmap ... see
below for more details.

*X*, *Y*, and *Z* must be arrays with the same dimensions.

*Z* may be a masked array, but filled contouring may not
handle internal masked regions correctly.

``C = contour(...)`` returns a
:class:`~matplotlib.contour.QuadContourSet` object.

Optional keyword arguments:

  *colors*: [ None | string | (mpl_colors) ]
    If *None*, the colormap specified by cmap will be used.

    If a string, like 'r' or 'red', all levels will be plotted in this
    color.

    If a tuple of matplotlib color args (string, float, rgb, etc),
    different levels will be plotted in different colors in the order
    specified.

  *alpha*: float
    The alpha blending value

  *cmap*: [ None | Colormap ]
    A cm :class:`~matplotlib.cm.Colormap` instance or
    *None*. If *cmap* is *None* and *colors* is *None*, a
    default Colormap is used.

  *norm*: [ None | Normalize ]
    A :class:`matplotlib.colors.Normalize` instance for
    scaling data values to colors. If *norm* is *None* and
    *colors* is *None*, the default linear scaling is used.

  *levels* [level0, level1, ..., leveln]
    A list of floating point numbers indicating the level
    curves to draw; eg to draw just the zero contour pass
    ``levels=[0]``

  *origin*: [ None | 'upper' | 'lower' | 'image' ]
    If *None*, the first value of *Z* will correspond to the
    lower left corner, location (0,0). If 'image', the rc
    value for ``image.origin`` will be used.

    This keyword is not active if *X* and *Y* are specified in
    the call to contour.

  *extent*: [ None | (x0,x1,y0,y1) ]

    If *origin* is not *None*, then *extent* is interpreted as
    in :func:`matplotlib.pyplot.imshow`: it gives the outer
    pixel boundaries. In this case, the position of Z[0,0]
    is the center of the pixel, not a corner. If *origin* is
    *None*, then (*x0*, *y0*) is the position of Z[0,0], and
    (*x1*, *y1*) is the position of Z[-1,-1].

    This keyword is not active if *X* and *Y* are specified in
    the call to contour.

  *locator*: [ None | ticker.Locator subclass ]
    If *locator* is None, the default
    :class:`~matplotlib.ticker.MaxNLocator` is used. The
    locator is used to determine the contour levels if they
    are not given explicitly via the *V* argument.

  *extend*: [ 'neither' | 'both' | 'min' | 'max' ]
    Unless this is 'neither', contour levels are automatically
    added to one or both ends of the range so that all data
    are included. These added ranges are then mapped to the
    special colormap values which default to the ends of the
    colormap range, but can be set via
    :meth:`matplotlib.colors.Colormap.set_under` and
    :meth:`matplotlib.colors.Colormap.set_over` methods.

  *xunits*, *yunits*: [ None | registered units ]
    Override axis units by specifying an instance of a
    :class:`matplotlib.units.ConversionInterface`.

  *antialiased*: [ True | False ]
    enable antialiasing, overriding the defaults.  For
    filled contours, the default is True.  For line contours,
    it is taken from rcParams['lines.antialiased'].

contour-only keyword arguments:

  *linewidths*: [ None | number | tuple of numbers ]
    If *linewidths* is *None*, the default width in
    ``lines.linewidth`` in ``matplotlibrc`` is used.

    If a number, all levels will be plotted with this linewidth.

    If a tuple, different levels will be plotted with different
    linewidths in the order specified

  *linestyles*: [None | 'solid' | 'dashed' | 'dashdot' | 'dotted' ]
    If *linestyles* is *None*, the 'solid' is used.

    *linestyles* can also be an iterable of the above strings
    specifying a set of linestyles to be used. If this
    iterable is shorter than the number of contour levels
    it will be repeated as necessary.

    If contour is using a monochrome colormap and the contour
    level is less than 0, then the linestyle specified
    in ``contour.negative_linestyle`` in ``matplotlibrc``
    will be used.

contourf-only keyword arguments:

  *nchunk*: [ 0 | integer ]
    If 0, no subdivision of the domain. Specify a positive integer to
    divide the domain into subdomains of roughly *nchunk* by *nchunk*
    points. This may never actually be advantageous, so this option may
    be removed. Chunking introduces artifacts at the chunk boundaries
    unless *antialiased* is *False*.

Note: contourf fills intervals that are closed at the top; that
is, for boundaries *z1* and *z2*, the filled region is::

    z1 < z <= z2

There is one exception: if the lowest boundary coincides with
the minimum value of the *z* array, then that minimum value
will be included in the lowest interval.

**Examples:**

.. plot:: mpl_examples/pylab_examples/contour_demo.py

.. plot:: mpl_examples/pylab_examples/contourf_demo.py

Additional kwargs: hold = [True|False] overrides default hold state
===============================
None
</pre>
<p><input type="button" value="hide the remaining part△" style="WIDTH:150px"
   onClick="document.getElementById('div_2').style.display='none';
            document.getElementById('div_1').style.display='block';
            document.location='#div_1'"></p>
</div>

But sentensed in the upper document is difficult to understand. It is typical of tech-head's documents.  We should use this as teaching material by negarive example. I added "2D" and N point.." part to be easy with bold typeface to understand.

I don't now what part is the subject and predicate in "contour \*N\* automatically-chosen levels" sentence. I should guess the meanings from the words laid out. It is unreasonable to explain contour(..) and contourf(..) functions all together. It should explain as "conturf(..) function draw contours in zones. But you can use counterf in almost same idioms." in another section. Comparison with Matlab should be laid out in another section too.

I tryed to explain contour(..) function in my style. I will write codes of PythonSf Open preferably. You will be easy to understand them if you are familiar with scipy and matplotlib though you are unfamiliar with PythonSf.

## Explantins of contour(..) function in my style

If you want to render a contour of f(x,y) in short, you shoud make a matrix that has values of f(x,y) at a rectangular lattice in each matrix element. contur(..) function interpolate values at the points apart from the lattice.

<center><b>PythonSf one-liner</b></center>

    mt=~[(`X^2+2`Y^2)(x,y) for x,y in mitr(*[klsp(-3,3)]*2)].reshape(50,50); import matplotlib.pyplot as plt; plt.contour(mt); plt.show()

<center><b>a contour of x^2+2y^2</b></center>
![](/images/2014/05/contour_ellipse.png)

You can use an dubly enclosed list substitute for an 2D array. (In addition PyhtonSfOpen can calculate below code)

<center><b>PythonSf one-liner</b></center>

    kl=np.linspace(-3,3); mt=[[(`X^2+2*`Y^2)(x,y) for y in kl] for x in kl]; import matplotlib.pyplot as plt; plt.contour(mt); plt.show()


### specify a number of contours
You can specify a number of contours. You shoud assign the number as contour(2D_array, N) with integer N. It could not be always the N number of contours. You might get less contors.
<center><b>PythonSf one-liner</b></center>

    # render 50 counters or less
    mt=~[(`X^2+2`Y^2)(x,y) for x,y in mitr(*[klsp(-3,3)]*2)].reshape(50,50); import matplotlib.pyplot as plt; plt.contour(mt, 50); plt.show()

    # PythonSf Open で等高線 50 本弱を表示させる
    kl=np.linspace(-3,3); mt=[[(`X^2+2*`Y^2)(x,y) for y in kl] for x in kl]; import matplotlib.pyplot as plt; plt.contour(mt, 50); plt.show()

![](/images/2014/05/contour_ellipse_50line.png)

<br>

### specify values of contours

If change rate of 3-d figure is calm, then contour(..) function might render the contours well. But the change rate of the shape is vital as 1/r^2, then it badly renders contours.
<center><b>PythonSf one-liner</b></center>

    # render 50 or less contors with 1/(x^2+y^2) automatically
    mt=~[(1/(`X^2+2`Y^2))(x,y) for x,y in mitr(*[klsp(-3,3)]*2)].reshape(50,50); import matplotlib.pyplot as plt; plt.contour(mt, 50); plt.show()

    # PythonSf Open で等高線 50 本弱を表示させる
    kl=np.linspace(-3,3); mt=[[(1/(`X^2+2*`Y^2))(x,y) for y in kl] for x in kl]; import matplotlib.pyplot as plt; plt.contour(mt, 50); plt.show()

![](/images/2014/05/contour_ellipse_1_r_50line.png)

Then you should specify the value of contours with sequence:V as contour(mt, V)
<center><b>PythonSf one-liner</b></center>

    # render contours with values of sequence
    mt=~[(1/(`X^2+2`Y^2))(x,y) for x,y in mitr(*[klsp(-3,3)]*2)].reshape(50,50); import matplotlib.pyplot as plt; plt.contour(mt, [10,5,2,1, .5, .1, .01]); plt.show()

    # PythonSf Open で等高線表示値シーケンスを指定する
    kl=np.linspace(-3,3); mt=[[(1/(`X^2+2*`Y^2))(x,y) for y in kl] for x in kl]; import matplotlib.pyplot as plt; plt.contour(mt, [10,5,2,1, .5, .1, .01]); plt.show()

![](/images/2014/05/contour_ellipse_1_r_50line_with_sq.png)

### render contours with mesh grid like Matlab

You can render cntours with X,Y position parameters:mesh grid matrix values and Z matrix value as contour(X,Y,Z)
<center><b>PythonSf one-liner</b></center>

    # render contours with a value sequence and mesh grid position parameters
    mt=~[(1/(`X^2+2`Y^2))(x,y) for x,y in mitr(*[klsp(-3,3)]*2)].reshape(50,50); import matplotlib.pyplot as plt; plt.contour(klsp(-3,3)^([1]*50),([1]*50)^klsp(-3,3), mt.t, [10,5,2,1, .5, .1, .01]); plt.show()

    # PythonSf Open で mesh grid を使って等高線表示値シーケンスを指定する
    kl=np.linspace(-3,3); MX,MY=np.meshgrid(kl,kl); mt=[[(1/(`X^2+2*`Y^2))(x,y) for y in kl] for x in kl]; import matplotlib.pyplot as plt; plt.contour(MX, MY, mt, [10,5,2,1, .5, .1, .01]); plt.show()

![](/images/2014/05/contour_ellipse_1_r_50line_with_sq_mesh_grid.png)

The graph is improved to show right range for X and Y axes. 

<center><b>thanks to the developer of contour(..) function</b></center>
>Matlab influences SciPy very strongly, so it frequently uses mesh grid. Many 3-d rendering function must require mesh grid parameters. In other words, you can't use simple expressions:contour(Z). And explanations of contour(..) function in web pages uses the mesh grid parameters. But it is redundant in many cases to use mesh grid parameters, because you would render contours on a uniform lattice mostly. Mesh grid parameters might display X,Y axes scales appropriately. But you don't need the appropriate X,Y scales. Because you know them well. Because you have written the one-liner youself. You need to write it in short hand.
>
>br
>Definitely you should render right X,Y axes scales in public papers. Then you should render contours with efforts to get many information across in a glance. But 
>
>br
>I thanks the contour(..) function author who implemented the contour(..) program which works well just only with Z matrix values as contour(Z).

## a reference
[pylab_examples example code: contour_demo.py        ;;http://matplotlib.org/examples/pylab_examples/contour_demo.html
](http://matplotlib.org/examples/pylab_examples/contour_demo.html)
