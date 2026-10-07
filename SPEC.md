# SPEC.md: "The Human How"; Exact technical requirements, environment setup, implementation details, etc.

## Execution Description

Runs the repository-local rocblas-bench for C = alpha*(A*B) + beta*C. dtype, M, N, K, transposeA, transposeB, alpha, beta, lda, ldb, ldc, and batch_count come from yaml. warmup_iters is passed as --cold_iters and norm_check as -v. -w and --norm_check are never passed. Sweep dimensions: dtype, M, N, K, transposeA, transposeB, alpha, beta.

## Parameters

| Parameter | CLI Flag | Tested Values | Default | Description |
| --- | --- | --- | --- | --- |
| dtype | `--dtype` | smoke=f32_r, baseline=f32_r, extended=f32_r | f32_r | From Parameter list; see Execution Description With Parameters. |
| M | `--m` | smoke=64, baseline=4096, extended=4096 | 4096 | From Parameter list; see Execution Description With Parameters. |
| N | `--n` | smoke=64, baseline=4096, extended=4096 | 4096 | From Parameter list; see Execution Description With Parameters. |
| K | `--k` | smoke=64, baseline=4096, extended=4096 | 4096 | From Parameter list; see Execution Description With Parameters. |
| transposeA | `--transposea` | smoke=N, baseline=N, extended=N | N | From Parameter list; see Execution Description With Parameters. |
| transposeB | `--transposeb` | smoke=N, baseline=N, extended=N | N | From Parameter list; see Execution Description With Parameters. |
| alpha | `--alpha` | smoke=1, baseline=1, extended=1 | 1 | From Parameter list; see Execution Description With Parameters. |
| beta | `--beta` | smoke=0, baseline=0, extended=0 | 0 | From Parameter list; see Execution Description With Parameters. |
| lda | `--lda` | smoke=64, baseline=4096, extended=4096 | 4096 | From Parameter list; see Execution Description With Parameters. |
| ldb | `--ldb` | smoke=64, baseline=4096, extended=4096 | 4096 | From Parameter list; see Execution Description With Parameters. |
| ldc | `--ldc` | smoke=64, baseline=4096, extended=4096 | 4096 | From Parameter list; see Execution Description With Parameters. |
| batch_count | `--batch-count` | smoke=1, baseline=1, extended=1 | 1 | From Parameter list; see Execution Description With Parameters. |
| warmup_iters | `--warmup-iters` | smoke=2, baseline=5, extended=10 | 5 | From Parameter list; see Execution Description With Parameters. |
| num_iterations | `--num-iterations` | smoke=2, baseline=200000, extended=480000 | 200000 | From Parameter list; see Execution Description With Parameters. |
| norm_check | `--norm-check` | smoke=1, baseline=1, extended=1 | 1 | From Parameter list; see Execution Description With Parameters. |

### Derived quantities

Matrix extents come from the Parameters table. Example: use the tested value of `dtype` from Profile Parameter Values. Do not invent a leading dimension that the spec does not state.

## Invocation

```bash
Run the repository-local rocblas-bench built from ROCm rocBLAS source by scripts/build.sh (rocm_compile_rocblas_bench)
```

## Raw Output Format

rocblas-bench stdout in rocblas_bench.txt plus one CSV row written twice

sample_index,status,function,dtype,transposeA,transposeB,M,N,K,lda,ldb,ldc,batch_count,cold_iters,hot_iters,rocblas_gflops,rocblas_gemm_us,cpu_gflops,norm_error_1,achieved_compute_tflops,kernel_time_msec,minimum_operand_traffic_gb_s,relative_l1_checked_columns,error_message
0,ok,gemm,f32_r,N,N,4096,4096,4096,4096,4096,4096,1,5,20,12000,80,8000,1e-6,12,0.08,1500,1e-6,

## Metrics

- **#1: Achieved compute throughput** — stored as `achieved_compute_tflops`.
- **#2: Kernel execution time** — stored as `kernel_time_msec`.
- **#3: Minimum operand traffic, GB/s** — stored as `minimum_operand_traffic_gb_s`.
- **#4: Relative L1 of checked columns** — stored as `relative_l1_checked_columns`.
- **#5: CPU reference GFLOPS** — stored as `cpu_gflops`.

## Framework

Runs the repository-local rocblas-bench for C = alpha*(A*B) + beta*C. dtype, M, N, K, transposeA, transposeB, alpha, beta, lda, ldb, ldc, and batch_count come from yaml. warmup_iters is passed as --cold_iters and norm_check as -v.

## Installation and Execution Summary

Build rocblas-bench with scripts/build.sh and run it for the yaml GEMM configuration, saving the transcript to rocblas_bench.txt, to measure rocBLAS GEMM throughput and time per call. warmup_iters maps to --cold_iters and norm_check to -v; -w and --norm_check are never passed

## Platform Portability

- **AMD (primary):** ```bash
Run the repository-local rocblas-bench built from ROCm rocBLAS source by scripts/build.sh (rocm_compile_rocblas_bench)
```
- **NVIDIA:** Primary target is AMD ROCm. NVIDIA notes in this section are reference only and are not the execution path.

## Model Context Protocols

- **Active:** None

## Execution-Loop Validation Contract

EXECUTION CHAIN: `run_benchmark.sh` ➔ raw output ➔ `scripts/parse_results.py` ➔ `results/benchmark.db` ➔ `scripts/validate_results.py`

This benchmark uses a lightweight, SQLite-integrated execution loop for result validation. All validation is performed by `scripts/validate_results.py`.

### Validation script usage

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional; select the installed interpreter

# After a live run:
".venv/bin/python" scripts/validate_results.py --db results/benchmark.db

# CI / no-GPU path (seeds fixture and validates it):
".venv/bin/python" scripts/validate_results.py --seed-fixture --quiet

# Override DB path via environment variable:
BENCHMARK_DB=tests/fixtures/benchmark.db \
  ".venv/bin/python" scripts/validate_results.py
```

### Run artifact contract

rocblas-bench stdout in rocblas_bench.txt plus one CSV row written twice

sample_index,status,function,dtype,transposeA,transposeB,M,N,K,lda,ldb,ldc,batch_count,cold_iters,hot_iters,rocblas_gflops,rocblas_gemm_us,cpu_gflops,norm_error_1,achieved_compute_tflops,kernel_time_msec,minimum_operand_traffic_gb_s,relative_l1_checked_columns,error_message
0,ok,gemm,f32_r,N,N,4096,4096,4096,4096,4096,4096,1,5,20,12000,80,8000,1e-6,12,0.08,1500,1e-6,

```bash
bash run_benchmark.sh --help
bash run_benchmark.sh --profile smoke --validate
bash run_benchmark.sh --profile baseline --validate
bash run_benchmark.sh --profile extended --validate
```
`run_benchmark.sh --help` prints usage and exits. The harness calls `scripts/ensure_setup.sh` when `.setup_state` is absent.

### Required integrity checks (built into `validate_results.py`)

1. Latest run exists and `runs.status = 'ok'`.
2. `run.error_message` is NULL.
3. `started_at` and `finished_at` are valid ISO-8601 UTC strings.
4. All required aggregate metrics in `runs` are non-NULL and finite.
5. All required aggregate metrics are physically sensible (positive values). Runs the repository-local rocblas-bench for C = alpha*(A*B) + beta*C. dtype, M, N, K, transposeA, transposeB, alpha, beta, lda, ldb, ldc, and batch_count come from yaml. warmup_iters is passed as --cold_iters and norm_check as -v.
6. At least 2 sample rows exist for the latest `run_id` (sweep coverage).
7. No sample has `status = 'error'`.
8. Runs the repository-local rocblas-bench for C = alpha*(A*B) + beta*C. dtype, M, N, K, transposeA, transposeB, alpha, beta, lda, ldb, ldc, and batch_count come from yaml. warmup_iters is passed as --cold_iters and norm_check as -v.

### Baseline / Threshold configuration (`config/benchmark_config.yaml`)

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

Threshold key suffixes encode comparison direction when `thresholds:` is present: `_min` → observed value must be ≥ threshold. `_max` → observed value must be ≤ threshold. Informational `baselines:` ranges are not pass/fail gates.
