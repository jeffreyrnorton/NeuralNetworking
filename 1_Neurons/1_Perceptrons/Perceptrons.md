# Perceptron Example
Jeffrey Norton  
January 2017  



# McCullogh-Pitts Neurons

There are several references which are helpful in understanding McCullogh-Pitts neurons,
but [this one](https://www.lri.fr/~marc/EEAAX/Neurones/tutorial/mcpits/html/index.html)
was helpful.  The key formula is given by Hinton in his PDF notes.

1) Compute a weighted sum plus a bias.
$z = b + \sum_i x_i w_i$.
2) Output 1 if $z > 0$.  $y = 1 $ if $y \gt 0$, $0$ otherwise

## Example
The inputs $x$ consist of excitatory inputs with values ${0, 1}$,

```r
n=10
(x_excite=sample(c(0,1), n, replace=TRUE))
```

```
##  [1] 0 0 1 1 1 1 0 0 1 1
```

```r
(w_excite=runif(n, min=0, max=1))
```

```
##  [1] 0.99323000 0.05921541 0.22523429 0.96706080 0.15441237 0.21378790
##  [7] 0.01276859 0.72562983 0.65210227 0.21574856
```
inhibitory inputs with values ${0, 1}$,

```r
m=8
(x_inhibit=sample(c(-1,0), m, replace=TRUE))
```

```
## [1] -1 -1  0 -1  0 -1 -1 -1
```

```r
(w_inhibit=runif(m, min=0, max=1))
```

```
## [1] 0.85299853 0.87262106 0.66453387 0.57098256 0.91492245 0.13460784
## [7] 0.62110560 0.02358327
```
and the threshold of $0$ as described above

```r
(u = 0)
```

```
## [1] 0
```
Calculate the weighted sum as shown above with bias of $0.5$.

```r
b = 0.5
(z = b + sum(x_excite, w_excite) + sum(x_inhibit, w_inhibit))
```

```
## [1] 9.374545
```
Calculate the neuron output of $y$ using the
[Heaviside function](https://en.wikipedia.org/wiki/Heaviside_step_function)

```r
(y = ifelse(z > u, 1.0, 0.0))
```

```
## [1] 1
```

# Perceptrons
The [Wikipedia](https://en.wikipedia.org/wiki/Perceptron) entry is a good place to start.
It turns out that the definition given in the previous section matches Wikipedia's
definition of a Perceptron.

We can rewrite our equation above as $y = \sigma(b + \sum_i x_i w_i)$ where $\sigma$
is called the transfer function.  In the case above, the transfer function is the
Heaviside function.

According to Wikipedia, "modern" perceptrons use functions like the
[sigmoid function](https://en.wikipedia.org/wiki/Sigmoid_function) as the transfer 
function $\sigma$.  Examples of sigmoids include the logisitic function $\sigma(t) = \frac{1}{1+e^{-t}}$

```r
sigma <- function(t) 1/(1+exp(-t))

plot(x=seq(from=-6,to=6,length.out=100), y=sigma(seq(from=-6,to=6,length.out=100)), type="l", xlab="t", ylab=expression(paste(sigma,"(t)")) )
```

![](Perceptrons_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
  
which has y-asymptotes at 0 and 1 and 
has the property that its derivative can be expressed as a function of itself,
$\sigma'(t) = \sigma(t) (1 - \sigma(t))$.

Another sigmoid functions used in as the activation function is the hyperbolic tangent
$\tanh(t) = \frac{1-e^{-2t}}{1+e^{-2t}} = 2 \sigma(2t)-1$

See Le Cunn's paper [Efficient Backprop](http://yann.lecun.com/exdb/publis/pdf/lecun-98b.pdf) for reasons for
choosing one or the other.
