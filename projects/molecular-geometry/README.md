# Molecular geometry analysis

The purpose of this project is to introduce you to fundamental C-language (or C++) programming techniques in the context of a scientific problem, viz. the calculation of the internal coordinates (bond lengths, bond angles, dihedral angles), moments of inertia, and rotational constants of a polyatomic molecule. A concise set of instructions for this project may be found [here](docs/project1-instructions.pdf).

We thank Dr. Yukio Yamaguchi of the University of Georgia for the original version of this project.

## Step 1: Read the Coordinate Data from Input

The input to the program is the set of Cartesian coordinates of the atoms (in bohr) and their associated atomic numbers. A sample molecule (acetaldehyde) to use as input to the program is:

```bash
    7
    6  0.000000000000     0.000000000000     0.000000000000
    6  0.000000000000     0.000000000000     2.845112131228
    8  1.899115961744     0.000000000000     4.139062527233
    1 -1.894048308506     0.000000000000     3.747688672216
    1  1.942500819960     0.000000000000    -0.701145981971
    1 -1.007295466862    -1.669971842687    -0.705916966833
    1 -1.007295466862     1.669971842687    -0.705916966833
```

The first line above is the number of atoms (an integer), while the remaining lines contain the z-values and x-, y-, and z-coordinates of each atom (one integer followed by three double-precision floating-point numbers). This [input file](./input/acetaldehyde.dat) ("acetaldehyde.dat") along with a few other test cases can be found in this repository in the [input directory](./input).

After downloading the file to your computer (to a file called “geom.dat”, for example), you must open the file, read the data from each line into appropriate variables, and finally close the file.

- [Hint #1](./hints/hint1-1.md): Opening and closing the file stream
- [Hint #2](./hints/hint1-2.md): Reading the number of atoms
- [Hint #3](./hints/hint1-3.md): Storing the z-values and the coordinates
- [Solution](./hints/step1-solution.md)

## Step 2: Bond Lengths

Calculate the interatomic distances using the expression:

$$ R_{ij} = \sqrt{ (x_i - x_j)^2 + (y_i - y_j)^2 + (z_i - z_j)} $$

where x, y, and z are Cartesian coordinates and i and j denote atomic indices.

- [Hint 1](./hints/hint2-1.md): Memory allocation
- [Hint 2](./hints/hint2-2.md): Loop structure
- [Hint 3](./hints/hint2-3.md): Printing the results
- [Hint 4](./hints/hint2-4.md): Extending the Molecule class
- [Solution](./hints/step2-solution.md)

## Step 3: Bond Angles

Calculate all possible bond angles. For example, the angle, $\phi_{ijk}$, between atoms i-j-k, where j is the central atom is given by

$$\cos \phi_{ijk} = \~\mathbf{e}_{ji} \cdot \~\mathbf{e}_{jk} $$

where the $\~\mathbf{e}_{ij}$ are unit vectors between the atoms, e.g.

$$e^x_{ij} = - \frac{x_i - x_j}{R_ij}$$
$$e^y_{ij} = - \frac{y_i - y_j}{R_ij}$$
$$e^z_{ij} = - \frac{z_i - z_j}{R_ij}$$

- [Hint 1](./hints/hint3-1.md): Memory allocation for the unit vectors
- [Hint 2](./hints/hint3-2.md): Avoiding a divide-by-zero
- [Hint 3](./hints/hint3-3.md): Memory allocation for the bond angles
- [Hint 4](./hints/hint3-4.md): Smart printing
- [Hint 5](./hints/hint3-5.md): Trigonometric functions
- [Solution](./hints/step3-solution.md)

## Step 4: Out-of-Plane Angles

Calculate all possible out-of-plane angles. For example, the angle $\theta_{ijkl}$ for atom i out of the plane containing atoms j-k-l (with k as the central atom, connected to i) is given by

$$\sin \theta_{ijkl} = \frac{\~\mathbf{e}_{kj} \times \~\mathbf{e}_{kl}}{\sin \phi_{jkl}} \cdot \~\mathbf{e}_{ki}$$

- [Hint 1](./hints/hint4-1.md): Memory allocation?
- [Hint 2](./hints/hint4-2.md): Cross products
- [Hint 3](./hints/hint4-3.md): Numerical precision
- [Hint 4](./hints/hint4-4.md): Smarter printing
- [Solution](./hints/step4-solution.md)

## Step 5: Torsion/Dihedral Angles

Calculate all possible torsional angles. For example, the torsional angle $\tau_{ijkl}$ for the atom connectivity i-j-k-l is given by

$$ \cos \tau_{ijkl} = \frac{(\~\mathbf{e}_{ij} \times \~\mathbf{e}_{jk}) \cdot (\~\mathbf{e}_{jk} \times \~\mathbf{e}_{kl})}{\sin \phi_{ijk} \sin \phi_{jkl}} $$

Can you also determine the sign of the torsional angle?

- [Hint 1](./hints/hint5-1.md): Memory allocation?
- [Hint 2](./hints/hint5-2.md): Numerical precision
- [Hint 3](./hints/hint5-3.md): Smart printing
- [Hint 4](./hints/hint5-4.md): Sign
- [Solution](./hints/step5-solution.md)

## Step 6: Center-of-Mass Translation

Find the center of mass of the molecule:

$$ X_{c.m.} = \frac{\sum\limits_i m_i x_i}{\sum\limits_i m_i}, \quad Y_{c.m.} = \frac{\sum\limits_i m_i y_i}{\sum\limits_i m_i}, \quad Z_{c.m.} = \frac{\sum\limits_i m_i z_i}{\sum\limits_i m_i} $$ 

where $m_i$ is the mass of atom i and the summation runs over all atoms in the molecule.

Translate the input coordinates of the molecule to the center-of-mass.

- [Hint 1](./hints/hint6-1.md): Atomic masses
- [Hint 2](./hints/hint6-2.md): Translating between atomic number and atomic mass
- [Solution](./hints/step6-solution.md)

## Step 7: Principal Moments of Inertia

Calculate elements of the [moment of inertia tensor](http://en.wikipedia.org/wiki/Moment_of_inertia_tensor).

Diagonal:

$$ I_{xx} = \sum\limits_i m_i (y^2_i + z^2_i), \quad I_{yy} = \sum\limits_i m_i (x^2_i + z^2_i), \quad  \quad I_{zz} = \sum\limits_i m_i (x^2_i + y^2_i) $$

Off-diagonal (add a negative sign):

$$ I_{xy} = \sum\limits_i m_i x_i y_i, \quad I_{xz} = \sum\limits_i m_i x_i z_i, \quad I_{yz} = \sum\limits_i m_i y_i z_i $$

Diagonalize the inertia tensor to obtain the principal moments of inertia:

$$ I_a \leq I_b \leq I_c $$ 

Report the moments of inertia in amu bohr<sup>2</sup>, amu $\AA^2$, and g cm<sup>2</sup>.

Based on the relative values of the principal moments, determine the [molecular rotor type](http://en.wikipedia.org/wiki/Rotational_spectroscopy): linear, oblate, prolate, asymmetric.

- [Hint 1](./hints/hint7-1.md): Diagonalization of a 3×3 matrix
- [Hint 2](./hints/hint7-2.md): Physical constants
- [Solution](./hints/step7-solution.md)

## Step 8: Rotational Constants

Compute the rotational constants in cm<sup>-1</sup> and MHz:

$$ A = \frac{h}{8 \pi^2 c I_a} $$
$$ B = \frac{h}{8 \pi^2 c I_b} $$
$$ C = \frac{h}{8 \pi^2 c I_c} $$

$$ A \geq B \geq C $$

- [Solution](./hints/step8-solution.md)

## Test Cases

- Acetaldehyde: [input coordinates](./input/acetaldehyde.dat) | [output](./output/acetaldehyde_out.txt)
- Benzene: [input coordinates](./input/benzene.dat) | [output](./output/benzene_out.txt)
- Allene: [input coordinates](./input/allene.dat) | [output](./output/allene_out.txt)

## Solution

- [C++](solution/solution.cc) and [CMake](solution/CMakeLists.txt)

## References

- [E.B. Wilson, J.C. Decius, and P.C. Cross, __Molecular Vibrations__, McGraw-Hill, 1955](https://www.amazon.com/Molecular-Vibrations-Infrared-Vibrational-Chemistry/dp/048663941X)
