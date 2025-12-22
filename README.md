# gr-libs

`gr-libs` is a Python library for **Goal Recognition (GR)** in Markov Decision Processes (MDPs).  
It provides implementations of multiple online and offline GR algorithms built on top of the **Gymnasium API**.

All agents interact with Gym environments registered by the companion package **`gr-envs`**, which is a required dependency.  
See: https://github.com/MatanShamir1/gr_envs

<table> <tr> <td width="40%"> <img src="gifs/point_maze.gif" width="200"/> </td> <td> A simple example of an agent trajectory used for goal recognition. Detailed descriptions of all supported domains, environment instances, and algorithms are documented separately in [`docs/domains_and_algorithms.md`](https://github.com/ReuthMirsky/gr_libs/blob/main/Domains_and_algorithms.md) </td> </tr> </table>


## Setup

### Requirements
- Python **≥ 3.11**
- Linux (recommended)
- Windows users: use **Git Bash** or **WSL**. Microsoft Visual C++ Build Tools ≥ 14.0 is required

---

### 1. Create a Python Environment (recommended)

#### Linux / macOS
```sh
python3.12 -m venv gr_env
source gr_env/bin/activate
````

#### Windows (Git Bash)

```sh
py -3.12 -m venv gr_env
source gr_env/Scripts/activate
```

---

### 2. Upgrade Packaging Tools

```sh
pip install --upgrade pip setuptools wheel versioneer
```

---

### 3. Install `gr-libs`

`gr-libs` uses extras to install environment-specific dependencies.

```sh
pip install gr_libs[minigrid]
```

Install additional environment extras:

```sh
pip install gr_libs[maze]
pip install gr_libs[highway]
pip install gr_libs[panda]
```

---

### Editable Installation (developers)

```sh
git clone https://github.com/MatanShamir1/gr_libs.git
cd gr_libs
pip install -e .
```

---

## Configuration

### Download Trained Agents and Caches

For a quick initial run, it is recommended to download the pretrained agents and caches using the CLI:

```sh
download-gr-libs-dataset
```

Specify a target directory if needed:

```sh
download-gr-libs-dataset --extract_to /path/to/data
```

This will create:

```
trained_agents/
gr_cache/
```

### Alternative Dataset Access

#### Run from source

```sh
git clone https://github.com/MatanShamir1/gr_libs.git
cd gr_libs
pip install gdown
python download_dataset.py
```

#### Docker (dataset included)

```sh
docker pull ghcr.io/matanshamir1/gr_image:latest
docker run -it ghcr.io/matanshamir1/gr_image:latest bash
```

Install `gr-libs` inside the container using the instructions above.

---

## Running

`gr-libs` supports **Online Dynamic Goal Recognition (ODGR)** workflows.
You can either write a custom Python script or run experiments via configuration files.

---

### Method 1: Python Script

```python
import gr_libs.environment  # REQUIRED: registers Gym envs

from gr_libs.recognizers import Graql
from gr_libs.learners import TabularQLearner
from gr_libs.utils import stochastic_amplified_selection, random_subset_with_order
from gr_libs.consts import QLEARNING

recognizer = Graql(
    domain_name="minigrid",
    env_name="MiniGrid-SimpleCrossingS13N4"
)

recognizer.goals_adaptation_phase(
    dynamic_goals=[(11,1), (11,11), (1,11)],
    dynamic_train_configs=[(QLEARNING, 100000)] * 3
)

actor = TabularQLearner(
    domain_name="minigrid",
    problem_name="MiniGrid-SimpleCrossingS13N4-DynamicGoal-11x1-v0",
    algorithm=QLEARNING,
    num_timesteps=100000
)

actor.learn()

full_seq = actor.generate_observation(
    action_selection_method=stochastic_amplified_selection,
    random_optimalism=True
)

partial_seq = random_subset_with_order(
    full_seq, int(0.5 * len(full_seq)), is_consecutive=False
)

predicted_goal = recognizer.inference_phase(
    partial_seq, (11,1), 0.5
)

print(predicted_goal)
```

---

### Method 2: Configuration-Based Execution

Predefined ODGR tasks are defined in `consts.py`.

Run an experiment:

```sh
python odgr_executor.py \
  --recognizer ExpertBasedGraml \
  --domain minigrid \
  --env_name MiniGrid-SimpleCrossingS13N4 \
  --task L1
```

Add statistics collection:

```sh
--collect_stats
```

This produces:

* Summary `.pkl` and `.txt` files (accuracy, runtime)
* Trajectory visualizations (PNG / MP4)
* Pickled confidence or embedding outputs for offline analysis

---

## Scaling Experiments

Run many ODGR problems in parallel using `all_experiments.py`:

```sh
python gr_libs/all_experiments.py \
  --domains minigrid parking \
  --envs MiniGrid-SimpleCrossingS13N4 Parking-S-14-PC- \
  --tasks L1 L2 L3 L4 L5 \
  --recognizers ExpertBasedGraml Graql \
  --n 5
```

Results are aggregated into:

```
outputs/summaries/
```

Analysis and plotting tools are available in:

```
evaluation/
```

See `evaluation/README.md` for details.

---

## Developers

For contributors:

```sh
pip install pre-commit
pre-commit install
```

CI, Docker images, and GitHub Actions are documented here:
[https://github.com/MatanShamir1/gr_libs/tree/main/CI](https://github.com/MatanShamir1/gr_libs/tree/main/CI)

```
