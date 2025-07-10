# 3D-Model-Generation-ML-Tool-with-Shap-E
This was coded for the internship application assignment at Red Panda Games

# 3D Asset Generation with Shap-E

This repository demonstrates a lightweight **3D model generation tool** powered by OpenAI's Shap‑E. Using a simple Python script (`model.py`), you can turn text prompts into 3D meshes in `.obj`/`.glb` formats, preview them as images, and even serve them through a minimal API.

---

## Repository Structure

```text
├── model.py              # Core pipeline: loads Shap-E, samples, decodes, and exports meshes
├── obj screenshot.png    # Rendered preview of one generated asset
├── working_video.mp4     # Short clip showcasing generation steps in action
├── README.md             # This documentation
└── requirements.txt      # Python dependencies
```

---

## What We Did

1. **Chose Shap-E Text2Mesh**: Leveraged OpenAI’s `text2mesh` model for CPU/Colab‑friendly inference.
2. **Built a CLI (`model.py`)**:

   * Parses a text prompt and device choice (CPU/GPU).
   * Loads the pretrained model, samples a latent representation, and decodes it into a mesh.
   * Exports the mesh to both `.obj` and `.glb` file formats.
3. **Visualized & Shared**:

   * Rendered one asset into `obj screenshot.png` for quick preview.
   * Recorded a short demo video (`working_video.mp4`).

---

## How It Works Internally

1. **Model Download & Setup**

   * We use `shap_e.models.download.load_model('text2mesh')` to fetch the pretrained Text2Mesh network.
   * The model is moved to the selected device (`cpu` or `cuda`).

2. **Latent Sampling**

   * Given a prompt (e.g. "a futuristic hoverboard"), we call `model.sample([...])` under `torch.no_grad()` to produce a latent representation of the desired 3D shape.
   * Optional parameters like `batch_size`, `guidance_scale`, and sampling settings (`karras_steps`, `sigma_min`, `sigma_max`, etc.) control diversity vs. fidelity.

3. **Decoding & Export**

   * The raw latent is converted into a triangular mesh via `latent.tri_mesh()`.
   * We write the mesh to disk using:

     ```python
     with open(path + '.obj', 'w') as f_obj:
         mesh.write_obj(f_obj)
     with open(path + '.glb', 'wb') as f_glb:
         mesh.write_glb(f_glb)
     ```

## Installation & Usage

1. **Clone & Install**

   ```bash
   git clone <your-repo-url>
   cd <repo>
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

2. **Run CLI**

   ```bash
   python model.py --prompt "an alien creature" --out outputs/alien.glb
   ```

3. **Optional: Start API**

   ```bash
   uvicorn api:app --host 0.0.0.0 --port 8000
   # GET  http://localhost:8000/status
   # POST http://localhost:8000/generate3d {"prompt": "a robot"}
   ```

---

## Future Enhancements

* **GPU Optimization**: Enable mixed precision & larger batch sizes on CUDA.
* **Multi-Model Support**: Plug in DreamFusion, Zero123, etc.
* **Web UI**: Build a React/Vue front-end to visualize & adjust parameters in real time.
* **Asset Pipeline**: Integrate generated models into Unity/Unreal Engine for immediate game testing.

---

*Created with \:heart: using OpenAI’s Shap‑E model.*
