# Element


---

<a id="method__setposition.1" class="lexicon_definition"></a>
#### setposition(at::Element.Atom,  R::Tuple{Float64, Float64, Float64}) [¶](#method__setposition.1)
`setposition(at,R)`

sets the position of atom `at` to `R`


*source:*
[Element/src/atom.jl:109](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/atom.jl#L109)

---

<a id="type__atom.1" class="lexicon_definition"></a>
#### Element.Atom [¶](#type__atom.1)
`Atom(Z; R=(0.,0.,0.), params...)`

creates an atom at `R` with nuclear charge `Z`,
with any additional parameters `params`.


*source:*
[Element/src/atom.jl:10](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/atom.jl#L10)

---

<a id="type__molecule.1" class="lexicon_definition"></a>
#### Element.Molecule [¶](#type__molecule.1)
`Molecule(; atoms=[], params...)`

creates an molecules with `atoms`.


*source:*
[Element/src/molecule.jl:9](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/molecule.jl#L9)

---

<a id="type__multipolefunc.1" class="lexicon_definition"></a>
#### Element.MultipoleFunc{T} [¶](#type__multipolefunc.1)
Multipole function 

$f_{lm}(r,\theta,\phi) = u(r) Y_{lm}(\theta,\phi)$,

where $u(r)$ is a user-defined radial function.

If $r_{cut}<r<r_{max}$, then the long-range analytical
form $q Y_{lm}(\theta,\phi)/r^{l+1}$ is used, where
`q` is the multipole moment.

The function vanishes for $r>r_{max}$.

f = MultipoleFunc(u; l=0, m=0, R=(0.,0.,0.), rcut=Inf, rmax=Inf, q=0.)

`u` - a user-defined radial function

e.g.
```
u = r -> exp(-r^2)
u = Spline1D(r, exp(-r.^2))
```

`l`,`m` - angular quantum numbers

`R` - origin of spherical coordinate system


*source:*
[Element/src/multipole.jl:32](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/multipole.jl#L32)

---

<a id="type__radialfunc.1" class="lexicon_definition"></a>
#### Element.RadialFunc{T} [¶](#type__radialfunc.1)
Spherically symmetric function 

$f(r,\theta,\phi) = u(r)$

where $u(r)$ is a user-defined radial function.

The function vanishes for $r>r_{max}$.

f = RadialFunc(u; R=(0.,0.,0.), rmax=Inf)

`u` - a user-defined radial function

`R` - origin of spherical coordinate system


*source:*
[Element/src/radialfunc.jl:19](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/radialfunc.jl#L19)


---

<a id="method__analytic_continuation.1" class="lexicon_definition"></a>
#### analytic_continuation!(r::AbstractArray{T, 1},  f::AbstractArray{T, 1},  g::Function) [¶](#method__analytic_continuation.1)
`analytic_continuation!(r,f,g)`

same as `analytic_continuation` but mutates `f`.


*source:*
[Element/src/radschrod.jl:152](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/radschrod.jl#L152)

---

<a id="method__analytic_continuation.2" class="lexicon_definition"></a>
#### analytic_continuation(r,  f,  g) [¶](#method__analytic_continuation.2)
`analytic_continuation(r,f,g)`

smoothly replaces $f(r)$ with the analytic form given by $g(r)$ as $r\to 0$

the transition point is determined as the point where 
the derivatives of `f` and `g` match.

analytic continuation as $r\to\infty$ can be acheived by reversing
`r` and `f`.

e.g.
```julia
r = loggrid(-12,4.,1000)
n,l = 3,1
ev,ef = radschrod1(r,-1./r,n,l,reverse=true)
efc = analytic_continuation(r,ef,r -> r.^(l+1))
```


*source:*
[Element/src/radschrod.jl:180](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/radschrod.jl#L180)

---

<a id="method__analytic_continuation_coulomb_nuclei.1" class="lexicon_definition"></a>
#### analytic_continuation_coulomb_nuclei!(r::AbstractArray{Float64, 1},  f::AbstractArray{Float64, 2},  l::AbstractArray{Int64, 1}) [¶](#method__analytic_continuation_coulomb_nuclei.1)
`analytic_continuation_coulomb_nuclei!(r,f,l)`

analytically continues multiple functions $f(r)$  (columns of `f`) with
the analytic form $r^{l+1}$ as $r\to 0$.

`f` - a matrix whose columns are functions of `r`

`l` - corresponding `l`-quantum numbers


*source:*
[Element/src/radschrod.jl:198](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/radschrod.jl#L198)

---

<a id="method__deepcopy.1" class="lexicon_definition"></a>
#### deepcopy(at::Element.Atom,  R::Tuple{Float64, Float64, Float64}) [¶](#method__deepcopy.1)
`deepcopy(at,R)`

returns a copy of `at` at the position `R`


*source:*
[Element/src/atom.jl:131](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/atom.jl#L131)

---

<a id="method__eig.1" class="lexicon_definition"></a>
#### eig!(u::Array{Float64, 1},  v::Array{Array{Float64, 1}, 1},  aE::Function,  dx::Float64,  qmin::Integer,  qmax::Integer) [¶](#method__eig.1)
`eig!(u,v,aE,dx,qmin,qmax;Emin=0.0,Emax=1.0,Elim=10000.)`

same as `eig` except that the eigenvalues and eigenfunctions are appended to `u` 
and `v`, respectively.

returns `q`, the number of nodes in the eigenfunctions.


*source:*
[Element/src/eig.jl:174](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/eig.jl#L174)

---

<a id="method__eig.2" class="lexicon_definition"></a>
#### eig(aE::Function,  dx::Float64,  qmin::Integer,  qmax::Integer) [¶](#method__eig.2)
`eig(aE,dx,qmin,qmax;Emin=0.0,Emax=1.0,Elim=10000.)`

solves the eigenvalue equation 

$y''(x) + a_E(x) y(x) = 0$

via the shooting method and Numerov's multistep integrator (see `multistep_integrator`),
on a uniform grid $[x_1, \dots, x_n]$ with spacing `dx`
and boundary conditions $y(x_1) = 0$ and $y(x_n) = 0$,

`aE` - the function $a_E(x)$, which returns a vector $[a_E(x_1), \dots, a_E(x_n)]$
for a given value of `E`.

`qmin,qmax` - solve for eigenfunctions with `qmin < q < qmax` nodes.

`Emin,Emax` - initial `E` interval to search. this will be expanded automatically
 (up to `±Elim`) until the range includes all requested eigenvalues.

returns `(u,v,q)`

where `u` are the eigenvalues, `v` are the eigenfunctions (the jth
eigenfunction given by `v[:,j]`), and `q` are the respective number of nodes in
the eigenfunctions.

e.g. *particle in a box*
```julia
x = lingrid(0,1,100)
dx = x[2]-x[1]
v = zeros(x)
evs,efs,q = eig(E -> 2*(E-v),dx,0,4)
```


*source:*
[Element/src/eig.jl:146](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/eig.jl#L146)

---

<a id="method__enumeratelm.1" class="lexicon_definition"></a>
#### enumeratelm(lmax) [¶](#method__enumeratelm.1)
`enumeratelm(lmax)` lists all pairs `(l,m)` for `l=0,...,lmax` and `m=-l,...,l`.


*source:*
[Element/src/multipole.jl:65](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/multipole.jl#L65)

---

<a id="method__findsteps.1" class="lexicon_definition"></a>
#### findsteps!(I::Array{Tuple{Float64, Float64}, 1},  f::Function,  xl::Float64,  xr::Float64,  fmin::Int64,  fmax::Int64) [¶](#method__findsteps.1)
`findsteps!(I,f,xl,xr,[fmin,fmax];args=())` 

like `findsteps` except that the intervals $(x_l,x_r)$ are appended to `I`.


*source:*
[Element/src/findsteps.jl:35](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/findsteps.jl#L35)

---

<a id="method__findsteps.2" class="lexicon_definition"></a>
#### findsteps(f::Function,  xl::Float64,  xr::Float64) [¶](#method__findsteps.2)
`findsteps(f,xl,xr,[fmin,fmax];args=())` 

takes a function `f(x,args...)`, assumed to be a
monotonically increasing integer function of `x`.

optionally one can specify the range of `f` to search 
with `fmin`, `fmax`

performs a binary search and returns an ordered list of 
intervals $[(x_{l,1},x_{r,1}),(x_{l,2},x_{r,2}),\dots]$
within the specificed range `(xl,xr)` which contains exactly one step.

e.g.
```julia
f = (x,c) -> round(Int,x^2+c)
findsteps(f,0.0,2.0,args=1.0)
```


*source:*
[Element/src/findsteps.jl:69](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/findsteps.jl#L69)

---

<a id="method__fzero.1" class="lexicon_definition"></a>
#### fzero(f,  x1::Float64,  x2::Float64,  args...) [¶](#method__fzero.1)
`fzero(f,x1,x2,args...;tol=1e-12,maxsteps=100)`

finds the root $x_0$ of $f(x)$ in the interval $[x_1,x_2]$, such that
$f(x_0) = 0$ (up to specified tolerance `tol`).

`f` - a function f(x,args...) that returns Float64, where extra parameters 
may be passed through to `f`.

throws an error if $f(x_1)$ and $f(x_2)$ have the same sign, or 
if more than `maxsteps` iterations are taken.

uses the Dekker-Brent method (see `Numerical Recipes in C`, p. 361).


*source:*
[Element/src/fzero.jl:17](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/fzero.jl#L17)

---

<a id="method__genoh_a00.1" class="lexicon_definition"></a>
#### genOh_a00(v) [¶](#method__genoh_a00.1)
 C version: Dmitri Laikov
 F77 version: Christoph van Wuellen, http://www.ccl.net
 Python version: Richard P. Muller, 2002.
 Julia version: John C. Snyder, 2015.

 This subroutine is part of a set of subroutines that generate
 Lebedev grids [1-6] for integration on a sphere. The original 
 C-code [1] was kindly provided by Dr. Dmitri N. Laikov and 
 translated into fortran by Dr. Christoph van Wuellen.
 This subroutine was translated from C to fortran77 by hand.

 Users of this code are asked to include reference [1] in their
 publications, and in the user- and programmers-manuals 
 describing their codes.
 
   [1] V.I. Lebedev, and D.N. Laikov
       'A quadrature formula for the sphere of the 131st
        algebraic order of accuracy'
       Doklady Mathematics, Vol. 59, No. 3, 1999, pp. 477-481.

   [2] V.I. Lebedev
       'A quadrature formula for the sphere of 59th algebraic
        order of accuracy'
       Russian Acad. Sci. Dokl. Math., Vol. 50, 1995, pp. 283-286. 

   [3] V.I. Lebedev, and A.L. Skorokhodov
       'Quadrature formulas of orders 41, 47, and 53 for the sphere'
       Russian Acad. Sci. Dokl. Math., Vol. 45, 1992, pp. 587-592. 

   [4] V.I. Lebedev
       'Spherical quadrature formulas exact to orders 25-29'
       Siberian Mathematical Journal, Vol. 18, 1977, pp. 99-107. 

   [5] V.I. Lebedev
       'Quadratures on a sphere'
       Computational Mathematics and Mathematical Physics, Vol. 16,
       1976, pp. 10-24. 

   [6] V.I. Lebedev
       'Values of the nodes and weights of ninth to seventeenth 
        order Gauss-Markov quadrature formulae invariant under the 
        octahedron group with inversion'
       Computational Mathematics and Mathematical Physics, Vol. 15,
       1975, pp. 44-51.


*source:*
[Element/src/lebedev.jl:49](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/lebedev.jl#L49)

---

<a id="method__multipolemoment.1" class="lexicon_definition"></a>
#### multipolemoment(r,  rho,  l) [¶](#method__multipolemoment.1)
`multipolemoment(r,rho,l)`

computes the multipole moment of radial density `rho` in
the `l`-angular momentum channel

$q = \int dr\, r^{l+2} \rho_{lm}(r)$.

`r` (LogarithmicGrid) - logarithmic grid (`loggrid`)

`rho` (AbstractVector) - radial density $\rho_{lm}(r)$ evaluated on `r` 

`l` (Int) - l-quantum number



*source:*
[Element/src/radpoisson.jl:70](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/radpoisson.jl#L70)

---

<a id="method__multistep_integrator.1" class="lexicon_definition"></a>
#### multistep_integrator!(y::AbstractArray{Float64, 1},  a::AbstractArray{Float64, 1},  h::Float64,  y1::Float64,  y2::Float64) [¶](#method__multistep_integrator.1)
`multistep_integrator!(y,a,h,y1,y2)` is the same as `multistep_integrator`,
but overwrites `y`.


*source:*
[Element/src/eig.jl:22](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/eig.jl#L22)

---

<a id="method__multistep_integrator.2" class="lexicon_definition"></a>
#### multistep_integrator(a::AbstractArray{Float64, 1},  h::Float64,  y1::Float64,  y2::Float64) [¶](#method__multistep_integrator.2)
`multistep_integrator(a,h,y1,y2)` solves 

$y''(x) + a(x) y(x) = 0$ 

on a uniform grid via the 4th order 
[Numerov's method](https://en.wikipedia.org/wiki/Numerov%27s_method).

Assuming a uniform grid $[x_1, \dots, x_n]$ with spacing `h`

`a` - discretization of $a(x)$, $[a(x_1), \dots, a(x_n)]$

`y1,y2` - initial boundary condition, specifying $y(x_1)$ and $y(x_2)$

Returns the solution $[y(x_1), \dots, y(x_n)]$.


*source:*
[Element/src/eig.jl:48](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/eig.jl#L48)

---

<a id="method__multistep_integrator_endpoint.1" class="lexicon_definition"></a>
#### multistep_integrator_endpoint(y::AbstractArray{Float64, 1},  a::AbstractArray{Float64, 1},  h::Float64,  y1::Float64,  y2::Float64) [¶](#method__multistep_integrator_endpoint.1)
`multistep_integrator_endpoint(y,a,h,y1,y2)` 

is the same as `multistep_integrator`,
but returns only $y(x_n)$. 

`y` is used as temporary storage.


*source:*
[Element/src/eig.jl:63](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/eig.jl#L63)

---

<a id="method__multistep_integrator_node_count.1" class="lexicon_definition"></a>
#### multistep_integrator_node_count(y::AbstractArray{Float64, 1},  a::AbstractArray{Float64, 1},  h::Float64,  y1::Float64,  y2::Float64) [¶](#method__multistep_integrator_node_count.1)
`multistep_integrator_node_count(y,a,h,y1,y2)` 

is the same as `multistep_integrator`,
but returns the number of nodes in $y(x)$ (i.e. the number of sign changes). 

`y` is used as temporary storage.


*source:*
[Element/src/eig.jl:78](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/eig.jl#L78)

---

<a id="method__multistep_rule.1" class="lexicon_definition"></a>
#### multistep_rule(a_jm1::Float64,  a_j::Float64,  a_jp1::Float64,  y_jm1::Float64,  y_j::Float64,  h::Float64) [¶](#method__multistep_rule.1)
`multistep_rule(a_jm1,a_j,a_jp1,y_jm1,y_j,h)`

computes the multistep rule in Numerov's method:

$y_{j+1} = { \left(2-5h^2 a_j/6\right)y_j - \left(1+h^2 a_{j-1}/12\right)y_{j-1} 
    \over 1+h^2 a_{j+1}/12 }$


*source:*
[Element/src/eig.jl:12](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/eig.jl#L12)

---

<a id="method__radpoisson.1" class="lexicon_definition"></a>
#### radpoisson(r::Element.LogarithmicGrid{T, A<:AbstractArray{T, 1}},  rho::AbstractArray{T, 1},  l::Int64) [¶](#method__radpoisson.1)
`radpoisson(r,rho,l)`

solves the poisson equation for a radial density `rho` in
the `l`-angular momentum channel

computes the hartree potential

$v_{h,lm}(r) = \int_0^r dr_<\, r_<^2 g_l(r_<,r) \rho_{lm}(r_<) + 
	\int_r^\infty dr_>\, r_>^2 g_l(r,r_>) \rho_{lm}(r_>)$,

where $g_l(r_<,r_>) = r_<^l/r_>^{l+1}$

uses a 3rd order adams-moulton multistep integrator to compute
each integral

`r` (LogarithmicGrid) - logarithmic grid (`loggrid`)

`rho` (AbstractVector) - radial density $\rho_{lm}(r)$ evaluated on `r` 

`l` (Int) - l-quantum number

e.g.
```julia
l = 0
r = loggrid(-13,5,100)
rho = exp(-2r)
vh = radpoisson(r,rho,l)
q = multipolemoment(r,rho,l)
plot(r,vh)
plot!(r,q./r.^(l+1))  # asymptotic form of hartree potential
xlims!(0,10)
ylims!(0,0.3)
```

[`V. Blum et al., CPC 180 (2009)` p. 2185-2186]


*source:*
[Element/src/radpoisson.jl:41](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/radpoisson.jl#L41)

---

<a id="method__radschrod.1" class="lexicon_definition"></a>
#### radschrod(r::Element.LogarithmicGrid{Float64, A<:AbstractArray{T, 1}},  v::AbstractArray{Float64, 1},  nmax::Int64,  lmax::Int64) [¶](#method__radschrod.1)
`radschrod(r,v,nmax,lmax;reverse=false,Emin=-1.0,Emax=0.0,Elim=1e4)`

solves the radial Schrödinger equation

$\left\{ -{1\over 2}{d^2\over dr^2} + {l(l+1)\over 2r^2} + v(r) \right\} u(r) = \epsilon u(r)$

where $\varphi(r,\Omega) = {u(r)\over r} Y_l^m(\Omega)$.

for all eigenvalues/eigenfunctions up to `nmax,lmax` (`nmax > 0`, `lmax >= 0` and `lmax < nmax`).

the eigenfunctions are normalized such that $\int dr\, u(r)^2 = 1$

`r` - logarithmic grid (see `loggrid`)

`v` - the radial potential $v(r)$ evaluated on the grid `r`

`reverse` - if true, integrate in reverse. 

`Emin,Emax` - initial `E` interval to search. this will be expanded automatically
 (up to `±Elim`) until the range includes all requested eigenvalues.

returns `evs,efs,q`, the requested eigenvalues, eigenfunctions and a list of 
the corresponding quantum numbers `n,l`

e.g. *hydrogenic atom, Z=1*
```julia
r = loggrid(-12,5,1000)
v = -1./r
nmax,lmax = 3,2
evs,efs,q = radschrod(r,v,nmax,lmax,reverse=true)
```


*source:*
[Element/src/radschrod.jl:113](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/radschrod.jl#L113)

---

<a id="method__radschrod1.1" class="lexicon_definition"></a>
#### radschrod1(r::Element.UniformGrid{Float64, A<:AbstractArray{T, 1}},  v::AbstractArray{Float64, 1},  n::Int64,  l::Int64) [¶](#method__radschrod1.1)
`radschrod1(r,v,n,l;reverse=false,Emin=-1.0,Emax=0.0,Elim=1e4)`

solves the radial Schrödinger equation

$\left\{ -{1\over 2}{d^2\over dr^2} + {l(l+1)\over 2r^2} + v(r) \right\} u(r) = \epsilon u(r)$

where $\varphi(r,\Omega) = {u(r)\over r} Y_l^m(\Omega)$.

for a single eigenvalue/eigenfunction, specified by `n,l`. 

the eigenfunction is normalized such that $\int dr\, u(r)^2 = 1$

`r` - uniform grid (`lingrid`, `simpsgrid`) or logarithmic grid (`loggrid`)

`v` - the radial potential $v(r)$ evaluated on the grid `r`

`reverse` - if true, integrate in reverse. 

`Emin,Emax` - initial `E` interval to search. this will be expanded automatically
 (up to `±Elim`) until the range includes all requested eigenvalues.

returns `ev,ef`, the requested eigenvalue $\epsilon$ and the eigenfunction $u(r)$

e.g. *hydrogen atom 1s orbital*
```julia
r = loggrid(-12,4,500)
v = -1./r
ev,ef = radschrod1(r,v,1,0,reverse=true)
```


*source:*
[Element/src/radschrod.jl:49](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/radschrod.jl#L49)

---

<a id="method__realsphharm.1" class="lexicon_definition"></a>
#### realsphharm(l::Int64,  m::Int64,  θ::Real,  ϕ::Real) [¶](#method__realsphharm.1)
`realsphharm(l,m,θ,ϕ)`

computes the real spherical harmonic function

$ Y_{lm} = {i \over \sqrt{2}} \left(Y_l^m - (-1)^m Y_l^{-m}\right), \quad m<0 $

$ Y_{lm} = Y_l^m, \quad m=0 $

$ Y_{lm} = {1 \over \sqrt{2}} \left(Y_l^{-m} + (-1)^m Y_l^m\right), \quad m>0 $



*source:*
[Element/src/sphharm.jl:48](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/sphharm.jl#L48)

---

<a id="method__sphharm.1" class="lexicon_definition"></a>
#### sphharm(l::Int64,  m::Int64,  θ::Real,  ϕ::Real) [¶](#method__sphharm.1)
`sphharm(l,m,θ,ϕ)`

computes the spherical harmonic function (standard definition used in quantum mechanics)

$ Y_l^m(\theta,\phi) = (-1)^m \sqrt{{2l+1 \over 4\pi} {(l-m)!\over (l+m)!}} P_l^m(cos \theta) e^{im\phi} $

which are normalized such that 

$ \int Y_l^m(\theta,\phi)^* Y_{l'}^{m'}(\theta,\phi) d\Omega = \delta_{ll'} \delta_{mm'} $


*source:*
[Element/src/sphharm.jl:32](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/sphharm.jl#L32)

---

<a id="method__splineradial.1" class="lexicon_definition"></a>
#### splineradial!(at::Element.Atom) [¶](#method__splineradial.1)
`splineradial!(at; npts=100)`

splines the electronic density `rho`, hartree potential `vh`,
and Kohn-Sham potential `vs` and creates radial functions.
Additionally, the Coulomb potential `v` is created.

assumes the `rho`, `vh` and `vs` have been calculated and are
given in `at` discretized on the radial grid `r`.

*sets*

`at[:v]` (see `CoulombPotential`)

`at[:rho]`, `at[:vh]`, `at[:vs]` (see `RadialFunc`).

`npts` - the number of evenly distributed spline points to use

e.g.
```julia
at = dft(Z=1.)
splineradial!(at)
plot(at[:rho].u,0,5,label="rho")
plot!(at[:vh].u,0,5,label="vh")
plot!(at[:vs].u,0,5,label="vs")
ylims!(-2,1)
```


*source:*
[Element/src/atom.jl:170](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/atom.jl#L170)

---

<a id="type__coulombpotential.1" class="lexicon_definition"></a>
#### Element.CoulombPotential [¶](#type__coulombpotential.1)
`CoulombPotential(Z;R=(0.,0.,0.))`

represents the Coulomb potential $v(r) = -Z/|r-R|$


*source:*
[Element/src/coulomb.jl:8](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/coulomb.jl#L8)

---

<a id="type__ldaxc.1" class="lexicon_definition"></a>
#### Element.ldaxc [¶](#type__ldaxc.1)
`ldaxc` is the pw-lda exchange-correlation functional [1,2].

calls the functionals `XC_LDA_X`, `XC_LDA_C_PW_MOD` in `libxc`.

spin-polarized and spin-unpolarized versions 
can be created via `ldaxcpol()` and `ldaxcunpol()`

e.g.
```julia
xc = ldaxcunpol()

vx = xc.x.v(rho)  # exchange potential
ex = xc.x.e(rho)  # exchange energy density
vc = xc.c.v(rho)  # correlation potential
ec = xc.c.e(rho)  # correlation energy density

# in-place versions
xc.x.v(rho,vx)
xc.x.e(rho,ex)
xc.c.v(rho,vc)
xc.c.e(rho,ec)

# more efficient to compute both together
xc.x(rho,ex,vx)
xc.c(rho,ec,vc)

# compute all components
xc(rho,ex,vx,ec,vc)
```

[1] J. P. Perdew and Y. Wang. Accurate and simple analytic representation 
of the electron-gas correlation energy. Phys. Rev. B, 45:13244–13249, 1992.

[2] D. M. Ceperley and B. J. Alder. Ground state of the electron gas by 
a stochastic method. Phys. Rev. Lett., 45:566–569, 1980.


*source:*
[Element/src/xc.jl:177](https://github.com/johncsnyder/Element/tree/b7e909ca09011e9748194d62f53cb6039446f1f9/src/xc.jl#L177)

