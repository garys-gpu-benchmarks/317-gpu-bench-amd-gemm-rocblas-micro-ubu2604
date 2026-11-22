# Results: 317 GEMM / rocBLAS Microbenchmark

**Main goal:** Measure rocBLAS GEMM throughput  
**Tools:** Run the repository-local rocblas-bench built from ROCm rocBLAS source by scripts/build.sh (rocm_compile_rocblas_bench)

Reference results from the validation run on the system shown in each section (recorded 2026-11-22). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-43-generic · driver 7.1.9.31600000
- **Run:** 2026-11-22T14:37:18Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 04:02 · benchmark 00:05 · whole run 04:13
- **Source:** run 317_20261122_143718
- **Command:** `bash run_benchmark.sh --smoke --dtype f32_r --M 64 --N 64 --K 64 --transposeA N --transposeB N --alpha 1 --beta 0 --lda 64 --ldb 64 --ldc 64 --batch-count 1 --warmup-iters 2 --num-iterations 2 --norm-check 1`

| # | Metric | Result |
|---|---|---|
| 1 | Achieved compute throughput, TFLOP/s (achieved_compute_tflops) | 0.04766254545454546 |
| 2 | Kernel execution time (kernel_time_msec) | 0.011 |
| 3 | Minimum operand traffic, GB/s (minimum_operand_traffic_gb_s) | 4.468363636363636 |
| 4 | Relative L1 of checked columns (relative_l1_checked_columns) | 1.46868e-07 |
| 5 | CPU reference GFLOPS (cpu_gflops) | 34.9525 |
<!-- /profile:smoke -->
