<p align="center">
  <img src="./banner.jpg" alt="AndroidFolate" style="max-height: 220px; object-fit: cover;">
</p>

<p align="center">
  <a href="https://huggingface.co/figranium/folate"><strong>Folate (Web)</strong></a>
  &nbsp;&bull;&nbsp;
  <a href="https://www.apache.org/licenses/LICENSE-2.0"><strong>Apache 2.0 License</strong></a>
</p>

**AndroidFolate** *(Upcoming — January 2027)* is a dedicated, on-device neural UI element ranker built for autonomous Android automation within an **8 GB device RAM ceiling**. Fine-tuned from [`google/electra-large-discriminator`](https://huggingface.co/google/electra-large-discriminator) (`335M` parameters) as a discriminative cross-encoder on multi-framework Android accessibility trees and quantized to **INT8 (~335 MB)**, AndroidFolate runs via **LiteRT** with hardware NPU acceleration across **Google Tensor**, **Qualcomm Hexagon**, **Samsung Exynos**, and **MediaTek APU**—requiring no PyTorch or `transformers` at inference time.

AndroidFolate acts as a fast, zero-hallucination UI grounding engine for single-step Android actions (`click`, `type`, `scroll`, `long_click`) across **Native Android Views / Jetpack Compose**, **React Native**, **Flutter**, and embedded **WebViews**.

## Highlights

- **Multi-Framework Accessibility Grounding:** Unified serialization schema covering **Native Views / Jetpack Compose**, **React Native**, **Flutter** Semantics trees, and embedded **WebViews**.
- **All Four Mobile NPU Vendors at Launch:** Static-bucketed **INT8 LiteRT** compilation targeting **Google Tensor**, **Qualcomm Hexagon**, **Samsung Exynos**, and **MediaTek APU**.
- **8 GB Device RAM Budget:** Compact ~335 MB INT8 footprint engineered to run on-device alongside heavy foreground Android applications without triggering low-memory kills.
- **Zero-Hallucination Node Resolution:** Scores and ranks only interactive nodes present in the live `AccessibilityNodeInfo` hierarchy—never inventing coordinates or missing elements.

## Artifacts & Architecture

| Artifact | Backbone | Precision / Format | Disk Size | Target Runtime |
| :--- | :--- | :--- | :--- | :--- |
| **`model_quantized.tflite` (Planned)** | `google/electra-large-discriminator` | Static `INT8` LiteRT | ~335 MB | LiteRT NPU Delegates (Tensor / Hexagon / Exynos / APU) |
| **`androidfolate-large` (Checkpoint)** | `google/electra-large-discriminator` | `float32` Safetensors | ~1.34 GB | PyTorch / Training |

- **Backbone:** `ElectraForSequenceClassification` (`24` transformer layers, `1024` hidden size, `16` attention heads, `num_labels=1`) initialized from `google/electra-large-discriminator` (`335M` parameters).
- **Serialization Schema:** Normalizes framework-specific node attributes (`className`, `resource-id`, `testID`, `contentDescription`, `accessibilityLabel`, Flutter Semantics `label`/`hint`, ARIA labels, visible `text`, and interactive state flags) while pruning unlabeled layout containers.

## Planned Usage

Model weights (`figranium/androidfolate` on Hugging Face), serialization utilities, and LiteRT NPU inference bindings will be published for the **January 2027** release:

```bash
hf download figranium/androidfolate --local-dir ./androidfolate-int8
```

## Framework-Stratified Evaluation

*Weights and benchmark results will be published upon the January 2027 release.*

| Split | UI Framework | Steps | Avg Candidates | Top-1 | Top-3 | Top-5 | MRR | NPU Latency |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`eval_native_compose`** | Native Android Views & Jetpack Compose | — | — | *Pending* | *Pending* | *Pending* | *Pending* | *Pending* |
| **`eval_react_native`** | React Native | — | — | *Pending* | *Pending* | *Pending* | *Pending* | *Pending* |
| **`eval_flutter`** | Flutter Semantics | — | — | *Pending* | *Pending* | *Pending* | *Pending* | *Pending* |
| **`eval_webview`** | Android WebView | — | — | *Pending* | *Pending* | *Pending* | *Pending* | *Pending* |

## License & Attribution

Licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).

*Android is a trademark of Google LLC. AndroidFolate is an independent open-source project by Figranium and is not affiliated with or endorsed by Google.*
