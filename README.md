# electromagnetism

Electromagnetism in Rust. Maxwell's equations, solved.

A classical electromagnetism library covering Coulomb's law, Biot-Savart, Lorentz force, EM waves, potentials, capacitance, and material properties — all built on `nalgebra` 3-vectors with full serde support.

**MIT OR Apache-2.0** licensed.

---

## Modules

| Module | What it covers |
|---|---|
| `vector` | 3D vector helpers (`Vec3`, distance, unit vector, cross, dot) |
| `constants` | ε₀, μ₀, c, k_Coulomb, e, m_e, m_p, k_B, Z₀ |
| `electrostatics` | Coulomb force, electric field (point + superposition), Gauss's law, line/plane charges |
| `magnetostatics` | Biot-Savart, infinite wire, current loop, Ampère's law, solenoid, magnetic dipole |
| `maxwell` | Maxwell's equations (differential + integral), displacement current, induced EMF, magnetic flux |
| `lorentz` | Lorentz force, cyclotron motion, Boris integrator, helical motion |
| `potential` | Scalar potential, superposition, Poisson/Laplace, charged sphere, numerical gradient |
| `capacitance` | Parallel plate, spherical, cylindrical capacitors; solenoid inductor; series/parallel combinations |
| `materials` | Permittivity, permeability, skin depth, refractive index, boundary conditions; built-in materials |
| `waves` | EM wave parameters, Poynting vector, energy density, radiation pressure, polarisation |

## Install

```toml
[dependencies]
electromagnetism = "0.1"
```

## Quick Start

### Electrostatics

```rust
use electromagnetism::*;
use nalgebra::vector;

let q1 = PointCharge::new(vector![0.0, 0.0, 0.0], 1e-6);
let q2 = PointCharge::new(vector![1.0, 0.0, 0.0], -1e-6);

let force = coulomb_force(&q1, &q2);
assert!(force.x < 0.0); // opposite charges attract

let field = electric_field_superposition(&[q1, q2], vector![0.5, 0.0, 0.0]);
```

### Magnetostatics

```rust
let wire = WireElement::new(vector![0.0, 0.0, -1.0], vector![0.0, 0.0, 1.0], 1.0);
let b_field = biot_savart_segment(&wire, vector![1.0, 0.0, 0.0]);

let b = infinite_wire_field(1.0, 0.1);
let b = solenoid_field(1000.0, 1.0);
```

### Lorentz Force & Boris Integrator

```rust
let electron = Particle::electron(
    vector![0.0, 0.0, 0.0],
    vector![1e6, 0.0, 0.0],
);
let b = vector![0.0, 0.0, 1.0];

let mut p = electron;
for _ in 0..100 {
    p = p.step_boris(&Vec3::zeros(), &b, 1e-9);
}
```

## License

MIT OR Apache-2.0
