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

## Supported Hardware & NPU Matrix

Hardware NPU acceleration requires **8 GB+ RAM** and a SoC supported by LiteRT's vendor NPU delegates (`CompiledModel` API):

| NPU Vendor / Delegate | Supported SoCs | Example Supported Devices (8 GB+ RAM) |
| :--- | :--- | :--- |
| **Google Tensor** (Tensor SDK) | Tensor G5 (`Tensor_G5`), Tensor G6 (`Tensor_G6`) | Pixel 10, Pixel 10 Pro, Pixel 10 Pro XL, Pixel 10 Pro Fold, Pixel 11 series |
| **Qualcomm Hexagon** (QNN HTP) | Snapdragon 8 Gen 1 (`SM8450`), 8+ Gen 1 (`SM8475`), 8 Gen 2 (`SM8550`), 8 Gen 3 (`SM8650`), 8 Elite (`SM8750`), 8 Elite Gen 5 (`SM8850`) | Galaxy S23 / S24 / S25 / S26 (Snapdragon), OnePlus 11 / 12 / 13 / 15, Xiaomi 14 / 15 / 17 |
| **Samsung Exynos** (AI LiteCore) | Exynos 2500 (`E9955`), Exynos 2600 (`E9965`) | Galaxy Z Flip 7 (Exynos 2500), Galaxy S26 / S26+ (Exynos 2600) |
| **MediaTek APU** (NeuroPilot) | Dimensity 7300 (`MT6878`), 8300 (`MT6897`), 9000 (`MT6983`), 9200 (`MT6985`), 9300 / 9300+ (`MT6989`), 9400 (`MT6991`), 9500 (`MT6990`) | Galaxy Tab S10+ / S10 Ultra, Xiaomi 14T Pro, Vivo X100 / X200, OPPO Find X8 |

### Unsupported Hardware Examples

Devices without a LiteRT-supported NPU delegate or with insufficient memory are not supported for NPU-accelerated inference:

- **Under 8 GB RAM:** Entry-level and older devices with 4 GB or 6 GB RAM (e.g., Pixel 6a, Galaxy A15 4 GB, Redmi Note base 6 GB variants) exceed the safe background memory ceiling alongside foreground apps.
- **Older / Non-Delegate SoCs:** Earlier Tensor generations (Tensor G1–G4 on Pixel 6–9 series), older Exynos chips (Exynos 2400 and earlier, such as Galaxy S24 Exynos or Galaxy A55 Exynos 1480), mid-range Snapdragon 6/7-series chips lacking QNN HTP delegate support, and non-Android platforms (iOS/iPadOS).

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
