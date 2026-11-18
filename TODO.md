# tpt-vfx TODO

Pure-Rust, zero-C++ VFX/SFX orchestration pipeline by TPT Solutions.
License: `MIT OR Apache-2.0`. Check items off as you go.

## Phase 0: Repo & Licensing
- [ ] `git init`, create remote, set default branch
- [ ] Cargo workspace (`tpt-vfx`, `vfx-scene`, `vfx-volume`, `vfx-rays`, `vfx-constraints`)
- [ ] `LICENSE-MIT` and `LICENSE-APACHE` (Copyright TPT Solutions)
- [ ] `license = "MIT OR Apache-2.0"` in `[workspace.package]`; SPDX headers in source files
- [ ] Update spec.txt: replace "Strict MIT" tenet and "Licensing" table row with dual-license wording
- [ ] `deny.toml` (allow MIT/Apache-2.0/BSD/ISC/Unicode; deny GPL/AGPL/LGPL)
- [ ] `README.md` (overview, architecture, license + contribution clause)
- [ ] `CONTRIBUTING.md` (dual-license contributions, no C++ FFI rule)
- [ ] `rustfmt.toml`, `clippy` config, `.gitignore`
- [ ] CI: fmt, clippy, test, cargo-deny (GitHub Actions)
- [ ] Add TPT crates as optional feature-gated deps (tpt-math, tpt-physics-engine, tpt-solver, tpt-crucible, tpt-nova)
- [ ] Verify `cargo check --workspace` passes with TPT features off

## Phase 1: The Foundation (Weeks 1-4)
### vfx-scene
- [ ] Design `.vfx` binary format (header, magic, version, endianness, alignment)
- [ ] Offset-based pointers (no absolute addresses)
- [ ] `bytemuck` Pod/Zeroable component layouts
- [ ] `memmap2` read-only loader with bounds/validation checks
- [ ] Flat ECS component arrays (nova-ecs integration)
- [ ] Writer/exporter for `.vfx` caches
- [ ] Tests: round-trip, corrupt-file rejection, large-file (multi-GB) load benchmark
### Physics -> Voxels
- [ ] Wire tpt-physics-engine SPH particle output
- [ ] Basic CPU voxel grid (dense, then sparse)
- [ ] Particle-to-grid splatting (density, velocity)
- [ ] Unit tests and benchmarks

## Phase 2: Volumetrics & WGSL (Weeks 5-8)
### vfx-volume
- [ ] Sparse Voxel Octree (insert, query, iterate, memory layout)
- [ ] MAC (staggered) grid
- [ ] Gradient / Divergence / Laplacian via tpt-math-pde-operators
- [ ] Advection schemes (Semi-Lagrangian, MacCormack)
- [ ] Pressure projection solver
- [ ] WGSL compute shaders: advection + projection
- [ ] CPU (rayon) vs GPU dispatch path
- [ ] Density + temperature output grids
### Rendering
- [ ] Basic raymarching in nova-render to visualize volumes
- [ ] Beer-Lambert absorption + phase functions

## Phase 3: Verification & Ray Tracing (Weeks 9-12)
### vfx-constraints
- [ ] Wrap tpt-physics-engine step
- [ ] Proof generation: distance/collision/interpenetration constraints
- [ ] tpt-solver-check integration (Accept/Reject)
- [ ] Fallback on Reject: sub-stepping
- [ ] Fallback on Reject: XPBD solver for the failing frame
- [ ] Broad-phase "no particle teleported through geometry" verification
- [ ] Tests: deliberately exploding sim is caught
### vfx-rays
- [ ] Parallel top-down BVH builder (tpt-math-geometry AABBs)
- [ ] BVH upload to wgpu storage buffers
- [ ] WGSL traversal: ray-mesh intersection
- [ ] WGSL: ray-volume intersection
- [ ] Path tracing integrator (materials, lights)
### Output
- [ ] OpenEXR writer via `exr` crate (verify license)
- [ ] Multi-channel/AOV support

## Phase 4: AI & Heterogeneous Compute (Weeks 13-16)
- [ ] tpt-crucible: route steps to CPU / GPU / NPU by load
- [ ] nova-splat Gaussian Splat raymarching
- [ ] nova-neural-materials integration
- [ ] AI-generated volumetric density fields
- [ ] AI volumetric assets interacting physically with physics-engine particles

## API & Docs
- [ ] `VfxContext::builder()` (GPU backend, verified math)
- [ ] `prelude` module, `VfxResult`/error types
- [ ] Typed units integration (tpt-math units)
- [ ] `FluidEmitter`, `VolumeGrid` builders
- [ ] Working example from spec section 6 (`examples/fluid_splash.rs`)
- [ ] rustdoc for all public items, mdBook/guide
- [ ] Benchmarks suite and published results

## Release
- [ ] Crate metadata, categories, keywords
- [ ] Changelog, semver policy
- [ ] Publish to crates.io

## Open Questions
- [ ] Exact TPT crate names, versions, and source (crates.io vs git)
- [ ] `exr`, `wgpu`, `memmap2` license confirmation under dual-license policy
- [ ] `.vfx` format versioning/compat strategy
- [ ] MSRV and wgpu version target
- [ ] Unsafe policy (mmap requires limited `unsafe` in vfx-scene)
- [ ] Repository URL / GitHub org
