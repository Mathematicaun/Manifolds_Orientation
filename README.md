# Diffiomorphism from a Torus $(T^2)$ to Sphere $(S^2)$

This repository explores the mathematical formulations and transformations of the torus and the sphere. Below, we detail the parametric equations, transformations, and how specific limits transition between these shapes.

## Torus

The torus is represented parametrically as:

$$
\begin{aligned}
    x(u, v) &= (R + r \cos v) \cos u, \\
    y(u, v) &= (R + r \cos v) \sin u, \\
    z(u, v) &= r \sin v,
\end{aligned}
$$

where:
- $R$ is the major radius (distance from the center of the hole to the center of the tube),
- $r$ is the minor radius (radius of the tube),
- $u, v \in [0, 2\pi)$ are the parametric angles.

## Sphere

The sphere can be represented parametrically as:

$$
\begin{aligned}
    x(\theta, \phi) &= r \sin \phi \cos \theta, \\
    y(\theta, \phi) &= r \sin \phi \sin \theta, \\
    z(\theta, \phi) &= r \cos \phi,
\end{aligned}
$$

where:
- $r$ is the radius of the sphere,
- $\theta \in [0, 2\pi)$ is the azimuthal angle,
- $\phi \in [0, \pi]$ is the polar angle.

## Transition from Torus to Sphere

### Limit as $R \to 0$
When the major radius $R$ tends to $0$, the torus collapses into a sphere-like structure:

$$
\begin{aligned}
    x(u, v) &= r \cos u \cos v, \\
    y(u, v) &= r \sin u \cos v, \\
    z(u, v) &= r \sin v,
\end{aligned}
$$

This corresponds to a sphere where $r$ remains constant, and the parameters $u$ and $v$ map directly to spherical coordinates.

## Special Cases

1. **Degenerate Sphere:** When $r \to 0$, both the sphere and the torus reduce to a single point.
2. **Flat Ring:** If $r$ is constant and $R \to \infty$, the torus becomes an infinitely thin flat ring.

## Applications

- **Torus:** Used in topology, geometry, and physics to study periodic systems.
- **Sphere:** Central to many fields, including astronomy, computer graphics, and geodesy.

## Visualization

Using Python and libraries like Matplotlib, we visualize these transitions and structures. Below is an example script to generate a 3D animation of the transformation from a sphere to a torus:

```python
from matplotlib.animation import FuncAnimation
from Torus import F
import numpy as np
import cupy as cp
import matplotlib.pyplot as plt
from matplotlib.colors import Normalize

import cupy as cp
import sympy as sp

class F:
    def __init__(self, a, b, R, r):
        self.a = a
        self.b = b
        self.R = R
        self.r = r
        self._initialize_symbols()

    def _initialize_symbols(self):
        self.a_sym, self.b_sym, self.R_sym, self.r_sym = sp.symbols('a b R r')
        self.chi_sym = (self.r_sym * sp.cos(2 * sp.pi * self.a_sym) + self.R_sym) * sp.cos(2 * sp.pi * self.b_sym)
        self.psi_sym = (self.r_sym * sp.cos(2 * sp.pi * self.a_sym) + self.R_sym) * sp.sin(2 * sp.pi * self.b_sym)
        self.zet_sym = self.r_sym * sp.sin(2 * sp.pi * self.a_sym)
        self.chia_sym = sp.diff(self.chi_sym, self.a_sym)
        self.psia_sym = sp.diff(self.psi_sym, self.a_sym)
        self.zeta_sym = sp.diff(self.zet_sym, self.a_sym)
        self.chib_sym = sp.diff(self.chi_sym, self.b_sym)
        self.psib_sym = sp.diff(self.psi_sym, self.b_sym)
        self.zetb_sym = sp.diff(self.zet_sym, self.b_sym)
        self.chiR_sym = sp.diff(self.chi_sym, self.R_sym)
        self.psiR_sym = sp.diff(self.psi_sym, self.R_sym)
        self.zetR_sym = sp.diff(self.zet_sym, self.R_sym)
        self.chir_sym = sp.diff(self.chi_sym, self.r_sym)
        self.psir_sym = sp.diff(self.psi_sym, self.r_sym)
        self.zetr_sym = sp.diff(self.zet_sym, self.r_sym)

    def chi(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.chi_sym, 'cupy')(self.a, self.b, self.R, self.r)
    
    def psi(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.psi_sym, 'cupy')(self.a, self.b, self.R, self.r)
    
    def zet(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.zet_sym, 'cupy')(self.a, self.b, self.R, self.r)

    def chia(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.chia_sym)(self.a, self.b, self.R, self.r)

    def psia(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.psia_sym)(self.a, self.b, self.R, self.r)

    def zeta(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.zeta_sym)(self.a, self.b, self.R, self.r)
    
    def chib(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.chib_sym)(self.a, self.b, self.R, self.r)
    
    def psib(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.psib_sym)(self.a, self.b, self.R, self.r)
    
    def zetb(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.zetb_sym)(self.a, self.b, self.R, self.r)
    
    def chiR(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.chiR_sym)(self.a, self.b, self.R, self.r)

    def psiR(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.psiR_sym)(self.a, self.b, self.R, self.r)

    def zetR(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.zetR_sym)(self.a, self.b, self.R, self.r)
    
    def chir(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.chir_sym)(self.a, self.b, self.R, self.r)

    def psir(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.psir_sym)(self.a, self.b, self.R, self.r)

    def zetr(self):
        return sp.lambdify((self.a_sym, self.b_sym, self.R_sym, self.r_sym), self.zetr_sym)(self.a, self.b, self.R, self.r)

fig = plt.figure()
ax = fig.add_subplot(aspect='equal', projection='3d')
fig.set_facecolor('black')
ax.set_facecolor('black')

i = cp.linspace(0, 1, 10**2)
j = cp.linspace(0, 1, 3)

a, b = cp.meshgrid(i, i)
c, d = cp.meshgrid(j, j)

def diffeomorphism(t):
    r = 1/(1 + np.exp(6*(2*t-1)))+1
    R = 3*r*t
    man = F(a, b, R, r)
    vec = F(c, d, R, r)

    x1 = cp.asnumpy(vec.chi())#
    y1 = cp.asnumpy(vec.psi())# position of point on the manifold for the vector field, for len(j), to not be compact and a lot of point.
    z1 = cp.asnumpy(vec.zet())#

    x = cp.asnumpy(man.chi())
    y = cp.asnumpy(man.psi())
    z = cp.asnumpy(man.zet())

    eaxx = cp.asnumpy(vec.chia())
    eayy = cp.asnumpy(vec.psia())
    eazz = cp.asnumpy(vec.zeta())

    ebxx = cp.asnumpy(vec.chib())
    ebyy = cp.asnumpy(vec.psib())
    ebzz = cp.asnumpy(vec.zetb())

    erxx = cp.asnumpy(vec.chir())
    eryy = cp.asnumpy(vec.psir())
    erzz = cp.asnumpy(vec.zetr())

    eRxx = cp.asnumpy(vec.chiR())
    eRyy = cp.asnumpy(vec.psiR())
    eRzz = cp.asnumpy(vec.zetR())

    eax = cp.asnumpy(man.chia())
    eay = cp.asnumpy(man.psia())
    eaz = cp.asnumpy(man.zeta())

    ebx = cp.asnumpy(man.chib())
    eby = cp.asnumpy(man.psib())
    ebz = cp.asnumpy(man.zetb())

    erx = cp.asnumpy(man.chir())
    ery = cp.asnumpy(man.psir())
    erz = cp.asnumpy(man.zetr())

    eRx = cp.asnumpy(man.chiR())
    eRy = cp.asnumpy(man.psiR())
    eRz = cp.asnumpy(man.zetR())

    # ax.set(xlim=[r+R, -(r+R)], ylim=[r+R, -(r+R)], zlim=[r+.5, -(r+.5)])

    maga = np.sqrt(eax**2 + eay**2 + eaz**2)
    magb = np.sqrt(ebx**2 + eby**2 + ebz**2)
    magr = np.sqrt(erx**2 + ery**2 + erz**2)
    magR = np.sqrt(eRx**2 + eRy**2 + eRz**2)

    norma = Normalize(vmin=np.min(maga), vmax=np.max(maga))
    normb = Normalize(vmin=np.min(magb), vmax=np.max(magb))
    normr = Normalize(vmin=np.min(magr), vmax=np.max(magr))
    normR = Normalize(vmin=np.min(magR), vmax=np.max(magR))

    colora = plt.get_cmap('jet')(norma(maga))
    colora.reshape(-1, 4)
    colorb = plt.get_cmap('jet')(normb(magb))
    colora.reshape(-1, 4)
    colorr = plt.get_cmap('jet')(normr(magr))
    colora.reshape(-1, 4)
    colorR = plt.get_cmap('jet')(normR(magR))
    colora.reshape(-1, 4)

    ax.clear()
    ax.axis('off')

    ax.plot_surface(x, y, z, facecolors=colorb, alpha=.4, linewidth=.1) # choose which color to descrive the norm of which vector (ea, colora), (eb, colorb), (er, colorr), (eR, colorR)

    ax.quiver(x1, y1, z1, eaxx, eayy, eazz, normalize=True, arrow_length_ratio=.5, color='white')
    ax.quiver(x1, y1, z1, ebxx, ebyy, ebzz, normalize=True, arrow_length_ratio=.5, color='white')
    ax.quiver(x1, y1, z1, erxx, eryy, erzz, normalize=True, arrow_length_ratio=.5, color='white')
    ax.quiver(x1, y1, z1, eRxx, eRyy, eRzz, normalize=True, arrow_length_ratio=.5, color='white')

# fig.savefig('stress.pdf')
ani = FuncAnimation(fig, diffeomorphism, cp.linspace(0, 1, 10**2), blit=False, repeat=True, interval=0)
plt.show()
