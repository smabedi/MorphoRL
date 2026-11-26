# Autonomous Explorer RL (Godot 4 + Stable-Baselines3)

A 3D reinforcement learning simulation built with **Godot 4 (Jolt Physics)** and **PyTorch / Stable-Baselines3** via `godot-rl`. An autonomous agent uses multi-channel 3D raycast sensors and proprioceptive feedback to navigate obstacles, avoid hazards, locate a target objective, and pursue it in real time.

## Repository Structure

```text
.
├── godot-project/          # Godot 4 simulation environment, scenes, and GDScript controllers
│   ├── addons/             # godot_rl_agents plugin
│   ├── script_templates/   # AIController script templates
│   └── project.godot       # Engine, Jolt physics, and collision layer configuration
└── python-trainer/         # Python RL training pipeline and ONNX export scripts
    └── train.py            # Stable-Baselines3 PPO runner
```

## Prerequisites

* **Godot Engine 4.x (.NET edition)** with Jolt 3D physics enabled
* **Python 3.12** with an NVIDIA CUDA-capable GPU (optional; CPU is supported)

## Setup

### 1. Python Training Environment

```bash
cd python-trainer
python -m venv .venv
.venv\Scripts\activate

# Install CUDA-enabled PyTorch (adjust index URL for your CUDA version if needed)
pip install torch torchvision --index-url [https://download.pytorch.org/whl/cu128](https://download.pytorch.org/whl/cu128)

# Install Godot RL bridge, Stable-Baselines3, and ONNX export tools
pip install godot-rl stable-baselines3 tensorboard onnx onnxscript
```

### 2. Godot Environment

1. Open **Godot Engine** and import `godot-project/project.godot`.
2. Confirm under **Project -> Project Settings -> Plugins** that **Godot RL Agents** is enabled.
3. Confirm under **Project -> Project Settings -> Physics -> 3D** that **Jolt Physics** is selected, and verify the 3D physics layer names (`Environment`, `Agent`, `Target`).

## Usage

### Training a Policy

1. Start the Python trainer from `python-trainer/` (with `.venv` activated):
   ```bash
   python train.py --experiment_name=explorer_v1 --timesteps=2000000 --onnx_export_path=explorer.onnx
   ```
2. Once the terminal prints `Waiting for connection...`, switch to Godot and press **F5** to launch the training scene.

### Standalone Inference

Copy the exported `.onnx` model into `godot-project/`, assign its path on the `Sync` node (`Control Mode = Onnx Inference`), set `Speed Up = 1`, and run the scene without Python.

## License

This project is licensed under the terms of the [LICENSE](LICENSE.txt) file in this repository.