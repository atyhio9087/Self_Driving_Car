# Self-Driving Car (Kivy + DQN)

A small interactive demo of a self-driving car trained with **Deep Q-Learning (DQN)**.  
Draw sand on the map with the mouse and watch the car learn to navigate to the goal while avoiding sand and walls. Built with **Kivy** for the UI and **PyTorch** for the DQN.

---

## Demo / What you get
- An interactive Kivy window where you can draw sand (obstacles) by dragging the mouse.
- A simple car agent with three sand sensors and a DQN brain that chooses one of three rotation actions.
- Save / load the neural network (checkpoint last_brain.pth) and plot training scores.
- Quick, visual reinforcement learning playground — great for learning and experimentation.

Run the app:

```bash
python map.py
```

---

## Screenshots

(Add your images here, for example:)

<img width="599.25" height="476.25" alt="UI with Sand" src="https://github.com/user-attachments/assets/06d6f127-73de-4f9e-bb43-3074e56bca4f" />
<img width="633.75" height="482.25" alt="Start and End Points" src="https://github.com/user-attachments/assets/dab2834b-bccd-4c24-ab07-9fb36ab1b64e" />

---

## Files in this repo

- **`map.py`** — Kivy app and game logic:
  - `Game` manages world updates and interactions between the car and environment.
  - `Car` implements car kinematics, sensors, and movement.
  - `MyPaintWidget` allows drawing sand on the map with the mouse.
  - Buttons: `clear`, `save`, `load`.
  - Main loop runs at `60Hz` via `Clock.schedule_interval`.

- **`ai.py`** — DQN implementation using PyTorch:
  - `Network` — simple feedforward NN (input → 30 hidden units → actions).
  - `ReplayMemory` — experience replay buffer.
  - `Dqn` — agent wrapper: selects actions, stores experiences, trains the model, saves/loads checkpoints.

---

## Dependencies

Tested with **Python 3.8+**. Major libraries:

- numpy  
- matplotlib  
- kivy  
- torch (PyTorch)

Install with pip:

```bash
pip install numpy matplotlib kivy torch
```

> ⚠️ Installing **Kivy** can be platform-specific. On Windows, use:  
> `pip install kivy[base] kivy_examples`  
> See [Kivy installation guide](https://kivy.org/doc/stable/gettingstarted/installation.html).

---

## How it works

- The car has **3 sensors** (front, left +30°, right -30°) that detect sand density.
- **State** to the DQN: `[signal1, signal2, signal3, orientation, -orientation]`  
  where `orientation` is the normalized angle to the goal.
- **Actions**:  
  - `0` → go straight  
  - `1` → rotate +20°  
  - `2` → rotate -20°
- **Rewards**:
  - `-1` when touching sand or hitting borders  
  - `-0.2` per time step  
  - `+0.1` if moving closer to the goal

---

## Usage / Controls

- **Draw sand**: Left-click + drag. Faster drawing = thicker strokes.  
- **Buttons**:
  - `clear` — removes all sand
  - `save` — saves the brain (`last_brain.pth`) and plots scores
  - `load` — loads the last saved brain

---

## Hyperparameters

- Hidden layer: 30 units  
- Optimizer: Adam (`lr=0.001`)  
- Replay buffer size: 100,000  
- Batch size: 100  
- Discount factor (`gamma`): 0.9  
- Action selection: Softmax with temperature = 100  

---

## Known issues

- **Index errors** may occur if sensors go out of bounds. Code sets signals = 1 at edges.  
- **Deprecation warnings**: The code uses `volatile=True`, which is old PyTorch style. Replace with `with torch.no_grad():` if you update PyTorch.  
- **Kivy blank screen**: Try resizing the window or checking your Kivy installation.  

---

## Future improvements

- Use epsilon-greedy exploration instead of high-temperature softmax.  
- Add more sensors or continuous control.  
- Replace the simple MLP with a deeper network.  
- Integrate TensorBoard logging.  
- Add automated evaluation episodes.  

---

## Requirements file

Minimal `requirements.txt`:

```
numpy
matplotlib
kivy
torch
```



