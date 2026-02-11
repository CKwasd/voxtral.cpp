# voxtral.cpp [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/andrijdavid/voxtral.cpp/blob/main/voxtral.cpp_colab.ipynb)

A ggml-based C++ implementation of Voxtral Realtime 4B.

## Quickstart

### 1. Download the model

Download the pre-converted GGUF model from Hugging Face:

```bash
# Default: Q4_0 quantization
./tools/download_model.sh Q4_0
```

### 2. Build

Build the project using CMake:

```bash
cmake -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j
```

### 3. Audio Preparation

The model expects **16-bit PCM WAV** files at **16kHz (mono)**. You can use `ffmpeg` to convert your audio files:

```bash
ffmpeg -i input.mp3 -ar 16000 -ac 1 -c:a pcm_s16le output.wav
```

### 4. Run Inference

```bash
./build/voxtral \
  --model models/voxtral/Q4_0.gguf \
  --audio path/to/input.wav \
  --threads 8
```

---

## Advanced Usage

### Manual Quantization

You can quantize an existing GGUF file using the native quantizer:

```bash
./build/voxtral-quantize \
  models/voxtral/voxtral.gguf \
  models/voxtral/voxtral-q6_k.gguf \
  Q6_K \
  8
```

## Testing

The test suite runs over `samples/*.wav` files.

### Numeric Parity Check

To verify numeric parity against the reference implementation:

```bash
python3 tests/test_voxtral_reference.py
```

### Custom Tolerances

You can override comparison tolerances via environment variables:
- `VOXTRAL_TEST_ATOL` (default: 1e-2)
- `VOXTRAL_TEST_RTOL` (default: 1e-2)
- `VOXTRAL_TEST_THREADS`