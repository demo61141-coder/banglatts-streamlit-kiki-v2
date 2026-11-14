# Kiki TTS v2 — Streamlit demo

Bengali text-to-speech demo for the **Kiki TTS v2** voice
(a separate single-speaker VITS model, fresh from the EMTIAZZ base:
2,528 clips, 70 epochs, resumed from checkpoint_16500 after the first
session died at step 16,500 / epoch 54; best model step 18307).

Open the app, type or paste Bengali text, press **অডিও বানাও**, listen,
download the WAV.

## How the model loads

The 114 MB ONNX model is NOT in this repo (GitHub's 100 MB per-file
limit). On first run the app downloads it from Hugging Face and verifies
exact byte size AND MD5 before use, then caches it:

- Model: https://huggingface.co/demoqqq/banglatts-kiki-v2/resolve/main/model.onnx
- Tokens: https://huggingface.co/demoqqq/banglatts-kiki-v2/resolve/main/tokens_sherpa.txt

## Files

- `app.py` — Streamlit UI (Bengali): text input, sample texts, speed
  selector, Generate, audio player, WAV download.
- `tts_engine.py` — synthesis engine (sherpa-onnx): danda-based chunking
  (≤700 chars/part), 0.5 s joins, 22050 Hz mono output, Kiki orthography
  normalization (য়→য+়, ড়→ড+়, ঢ়→ঢ+়) before generation.
- `requirements.txt` — sherpa-onnx 2.0.1, streamlit, numpy.

## Notes

- The Kiki v2 vocab (92 tokens) uses ONLY decomposed forms (য+়/ড+়/ঢ+়),
  so typed Bengali is normalized before generation (see normalize_text()
  in tts_engine.py). This is the documented Kiki orthography rule.
- Model constants verified at finish: model.onnx 114,192,746 bytes
  (MD5 177bb81652e55d6babee638fbee49f99); tokens_sherpa.txt 605 bytes
  (MD5 366751e11c26877a07828e4cbd3e7d7d).
