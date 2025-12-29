# Instructor Guide (Arturo)

This file is a practical runbook to teach from this repo.

## 0. Pre-class checklist (do this the day before)

### Repo health

- `main` CI is green (Build/Test, ASan/UBSan, TSan, clang-tidy, docs)
- Benchmarks build (`./build/benchmarks` runs)
- Docs build (`mkdocs build`) works

### Environment

- Codespaces / Devcontainer builds without errors
- ROOT is available (`root-config --version`)
- Sanitizers are available (`g++ -fsanitize=address ...`)

### Student workflow dry-run

Run exactly what you expect students to run:

```bash
cmake -B build -G Ninja -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake --build build -j$(nproc)
ctest --test-dir build --output-on-failure
```

## 1. Teaching model for this course

The course is organised around three quality pillars:

1. **Correctness**: tests + sanitizers + reproducible builds
2. **Collaboration**: git hygiene, pull requests, CI
3. **Performance**: measure, then optimise

For each concept, keep the loop tight:

1. Show a failure (test/CI/sanitizer/benchmark)
2. Explain what the signal means
3. Apply a small fix
4. Re-run to confirm

## 2. Core demo targets in this repo

From the repo root:

- `analyze`: triggers representative analysis codepaths
- `event_processor`: simulates event loops
- `benchmarks`: microbenchmarks (Google Benchmark)

Build:

```bash
cmake -B build -G Ninja -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake --build build -j$(nproc)
```

Run:

```bash
./build/analyze
./build/event_processor
./build/benchmarks
```

## 3. Exercise projects

Exercises are isolated mini-projects under `exercises/`.

Suggested flow:

- Start students inside the specific `starter/` folder
- Keep them out of the root repo build unless needed
- Use sanitizers first, then performance tooling

## 4. Debugging lab (TT-E1)

Goal: make students *trust the tool*.

- First run without sanitizers (ambiguous behaviour)
- Enable ASan/UBSan and re-run
- Fix the first crash, then iterate

## 5. Parallelism lab (SD-E1)

Goal: show that "faster" can also be "wrong".

- First build without OpenMP (works, slow)
- Enable OpenMP (fast, wrong output due to race)
- Fix with reductions, then tackle false sharing

## 6. Data layout lab (SD-E2)

Goal: show that layout and math formulation matter.

- Benchmark the baseline
- Remove expensive `pow` where possible
- Improve data layout and loop structure
- Re-run benchmarks to quantify improvements

