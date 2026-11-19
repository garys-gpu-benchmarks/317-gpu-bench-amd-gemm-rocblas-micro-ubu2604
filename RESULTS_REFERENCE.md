# Results: 317 GEMM / rocBLAS Microbenchmark

**Main goal:** Measure rocBLAS GEMM throughput  
**Tools:** Run the repository-local rocblas-bench built from ROCm rocBLAS source by scripts/build.sh (rocm_compile_rocblas_bench)

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_148 · recorded 2026-11-19). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-43-generic · driver 7.1.9.31600000
- **Run:** 2026-11-19T15:29:04Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 04:29 · benchmark 00:04 · whole run 04:41
- **Source:** run 317_20261119_152904
- **Command:** `bash run_benchmark.sh --smoke --dtype f32_r --M 64 --N 64 --K 64 --transposeA N --transposeB N --alpha 1 --beta 0 --lda 64 --ldb 64 --ldc 64 --batch-count 1 --warmup-iters 2 --num-iterations 2 --norm-check 1`

| # | Metric | Result |
|---|---|---|
| 1 | Achieved compute throughput, TFLOP/s (achieved_compute_tflops) | 0.040329846153846154 |
| 2 | Kernel execution time (kernel_time_msec) | 0.013 |
| 3 | Minimum operand traffic, GB/s (minimum_operand_traffic_gb_s) | 3.7809230769230773 |
| 4 | Relative L1 of checked columns (relative_l1_checked_columns) | 1.34863e-07 |
| 5 | CPU reference GFLOPS (cpu_gflops) | 26.2144 |
<!-- /profile:smoke -->
