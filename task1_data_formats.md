<img width="1620" height="942" alt="image" src="https://github.com/user-attachments/assets/7cc3ff24-74d9-46eb-adc4-0a61442d49af" />

# Task 1: Understanding MaxText Data Formats
## Candidate: Mahek Begum
## Date: June 15, 2026

## Summary

MaxText supports multiple data input formats for training LLMs. Based on my exploration of the MaxText documentation (https://maxtext.readthedocs.io/), I identified three main dataset types.

## Data Formats Available

| Format Type | Parameter Value | Description |
|-------------|----------------|-------------|
| **Synthetic** | `SYNTHETIC` | Randomly generated placeholder data, no external files needed |
| **C4 MLPerf** | `C4MLPERF` | Real text dataset from Colossal Clean Crawled Corpus (C4), used for MLPerf benchmarks |
| **JSON / Hugging Face** | `hf` or `json` | Custom datasets loaded from JSON files or Hugging Face hub |
| **Grain / OLMo** | `olmo_grain` | NumPy pipeline format used by OLMo models |

## Detailed Tradeoffs Analysis

### Synthetic Data
- **Pros:** Fastest loading, no disk I/O, reproducible, perfect for testing infrastructure
- **Cons:** Not useful for actual model quality, unrealistic patterns
- **Use case:** Benchmarking, debugging, CI/CD pipelines

### C4 / Real Text Data
- **Pros:** Real language patterns, produces useful models
- **Cons:** Slower loading, requires storage space, download time
- **Use case:** Actual model pre-training

### JSON / Custom Data
- **Pros:** Flexible, can use proprietary data
- **Cons:** Requires preprocessing, format validation needed
- **Use case:** Fine-tuning on custom domain data

## Key Differences

| Aspect | Synthetic | C4 | JSON/HF |
|--------|-----------|-----|---------|
| Speed | ⚡ Fastest | 🐢 Slow | 🟡 Medium |
| Real language | ❌ No | ✅ Yes | ✅ Yes |
| External files | ❌ No | ✅ Yes (GCS/local) | ✅ Yes |
| Setup time | 0 minutes | 10-30 minutes | 5-15 minutes |
| Model quality | ❌ Useless | ✅ Good | ✅ Good |

## What I Learned from the Documentation

1. The `dataset_type` parameter in MaxText configs controls which data pipeline runs
2. Synthetic data is implemented in `synthetic_data_processing.py`
3. C4 MLPerf has its own module: `tfds_data_processing_c4_mlperf`
4. The benchmarking guide explicitly recommends synthetic data for performance testing

## My Choice for Tasks 2 & 3

**I will use `dataset_type: SYNTHETIC` for all training runs.**

**Reasons:**
1. The assignment explicitly says: "Use synthetic data for this task"
2. 50-step training doesn't need real language patterns
3. Synthetic data eliminates network/storage variables when comparing CPU/GPU/TPU
4. Results are perfectly reproducible across different Colab sessions

## Screenshots

| Screenshot | What it shows |
|------------|----------------|
| `task1_main_page.png` | Main MaxText documentation page |
| `task1_dataset_type_mention.png` | Dataset_type parameter documentation |
| `task1_c4_mention.png` | C4 MLPerf data format availability |
| `task1_json_mention.png` | JSON format support |
| `task1_synthetic_mention.png` | Synthetic data module existence |




## Additional Observations

- MaxText uses JAX for high performance across TPU/GPU/CPU
- The framework is designed for scalability from single host to large clusters
- Data preprocessing is separate from training loop, allowing efficient pipelining
