# AI-Enhanced Hardware Verification Platform

## Overview

This project demonstrates AI-based verification acceleration for SystemVerilog UVM testbenches. It uses multiple AI/ML algorithms to optimize stimulus generation, minimize test time, and accelerate functional coverage closure without modifying the existing testbench.

## Key Features

- **Multiple AI Algorithms**: Reinforcement Learning (PPO, DQN, Q-Learning), Artificial Neural Networks, Bayesian Optimization, Genetic Algorithm
- **Black-Box Integration**: AI operates at simulation boundary without modifying testbenches
- **Real-Time Optimization**: Dynamic seed/constraint adjustment during simulation
- **Coverage Gap Detection**: Identifies uncovered scenarios and generates targeted stimuli
- **Comparison Framework**: Built-in baseline comparison for measuring AI effectiveness

## Directory Structure

```
ALU_Testbench_UVM_8Bit/
├── DUT/                      # Design Under Test
│   ├── ALU_DUT.sv           # 8-bit ALU implementation
│   └── ALU_interface.sv     # Interface definition
├── Testbench/               # UVM Testbench (unchanged)
│   ├── ALU_pkg.sv          # Package definition
│   ├── ALU_Sequence_Item.sv # Transaction
│   ├── ALU_Sequence.sv     # Sequence
│   ├── ALU_Driver.sv       # Driver
│   ├── ALU_monitor.sv      # Monitor
│   ├── ALU_Coverage_Collector.sv # Functional coverage
│   ├── ALU_Scoreboard.sv   # Scoreboard
│   ├── ALU_Agent.sv        # Agent
│   ├── ALU_Env.sv          # Environment
│   ├── Test.sv             # Test
│   └── ALU_Top.sv          # Top module
├── ai_engine/              # AI/ML Engine
│   ├── core/               # Core framework
│   │   ├── base_agent.py   # Base agent class
│   │   ├── coverage_tracker.py # Coverage analysis
│   │   └── stimulus_optimizer.py # Stimulus generation
│   ├── rl/                 # Reinforcement Learning
│   │   ├── ppo_agent.py    # PPO implementation
│   │   ├── dqn_agent.py    # DQN implementation
│   │   └── q_learning_agent.py # Q-Learning
│   ├── supervised/         # Supervised Learning
│   │   ├── neural_network.py # ANN predictor
│   │   ├── regressor.py    # Regression models
│   │   └── classifier.py   # Test selection
│   ├── bayesian/           # Bayesian Optimization
│   │   └── bayesian_optimizer.py
│   ├── genetic/            # Genetic Algorithm
│   │   └── genetic_optimizer.py
│   ├── utils/              # Utilities
│   │   ├── log_parser.py   # Log parsing
│   │   ├── coverage_parser.py # Coverage parsing
│   │   ├── seed_manager.py # Seed management
│   │   └── config_manager.py # Configuration
│   └── interface/          # Simulation interface
│       └── sim_controller.py # VCS integration
├── scripts/                # Run scripts
│   ├── run_simulation.py   # Main runner
│   ├── simple_sim.sh       # Basic simulation
│   └── ai_sim.sh          # AI simulation
├── config/                 # Configuration files
│   └── ai_config.yaml     # AI parameters
├── docs/                   # Documentation
│   ├── architecture.md    # System architecture
│   ├── algorithms.md      # Algorithm details
│   ├── usage.md           # Usage guide
│   └── comparison.md      # Comparison results
├── Makefile               # Build automation
└── README.md              # This file
```

## Quick Start

### Prerequisites

- Synopsys VCS (or compatible simulator)
- Python 3.8+
- NumPy, SciPy, scikit-learn

```bash
# Install Python dependencies
pip3 install numpy scipy scikit-learn

# Optional (for advanced RL)
pip3 install stable-baselines3 gymnasium torch
```

### Running Simulations

#### Basic Simulation (No AI)
```bash
make baseline
# or
./scripts/simple_sim.sh
```

#### With AI Optimization
```bash
# Using PPO
make ai_ppo

# Using Genetic Algorithm
make ai_genetic

# Using Bayesian Optimization
make ai_bayesian
```

#### Run Specific Algorithm
```bash
make run_ai ALGORITHM=genetic ITERATIONS=200
make run_ai ALGORITHM=dqn ITERATIONS=100
```

### Comparison

```bash
# Compare all algorithms
make ai_compare
```

## Configuration

Edit `config/ai_config.yaml` to customize:

- Algorithm parameters
- Simulation settings
- Coverage goals
- Stimulus ranges

```yaml
algorithm: ppo
optimization:
  coverage_goal: 0.95
  max_iterations: 1000
stimulus:
  a_range: [0, 255]
  b_range: [0, 255]
```

## Supported Algorithms

### Reinforcement Learning

| Algorithm | Best For |Features |
|-----------|----------|-----|
| PPO | Complex coverage | Stable, sample efficient |
| DQN | Discrete actions | Good exploration |
| Q-Learning | Simple scenarios | Fast, low overhead |

### Supervised Learning

| Algorithm | Best For |Features |
|-----------|----------|-----|
| Neural Network | Pattern prediction | Fast inference |
| Regression | Coverage prediction | Simple |
| Random Forest | Test selection | Handles complexity |

### Optimization

| Algorithm | Best For |Features |
|-----------|----------|-----|
| Bayesian | Expensive evaluation | Sample efficient |
| Genetic | Large search space | Global search |

## AI Architecture

```
┌─────────────────────────────────────────────────────┐
│              Simulation Environment                 │
│  ┌─────────┐    ┌─────────┐    ┌─────────────┐    │
│  │  DUT    │◄──►│  UVM    │◄──►│  Coverage   │    │
│  │ (ALU)   │    │TB (unchanged) │  Collector  │    │
│  └─────────┘    └─────────┘    └─────────────┘    │
│        ▲            ▲                  │           │
│        │            │                  ▼           │
│  ┌─────┴────────────┴──────────────────┴────────┐ │
│  │           Simulation Interface               │ │
│  │  (Log parsing, Seed injection, Coverage)     │ │
│  └────────────────────┬──────────────────────────┘ │
│                       │                            │
└───────────────────────┼────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────┐
│                AI Engine                            │
│  ┌─────────────────────────────────────────────┐   │
│  │           Coverage Tracker                  │   │
│  │  - State encoding                           │   │
│  │  - Gap detection                            │   │
│  │  - Progress monitoring                      │   │
│  └────────────────────┬────────────────────────┘   │
│                       │                            │
│                       ▼                            │
│  ┌─────────────────────────────────────────────┐   │
│  │           AI Agent                           │   │
│  │  (PPO/DQN/GA/Bayesian/etc.)                 │   │
│  │  - Policy/Value networks                    │   │
│  │  - Action selection                         │   │
│  │  - Learning updates                         │   │
│  └────────────────────┬────────────────────────┘   │
│                       │                            │
│                       ▼                            │
│  ┌─────────────────────────────────────────────┐   │
│  │      Stimulus Optimizer                     │   │
│  │  - Action mapping                           │   │
│  │  - Constraint handling                      │   │
│  │  - Target generation                        │   │
│  └─────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────┘
```

## Results

### Coverage Comparison (Example)

| Method | Iterations | Best Coverage | Improvement |
|--------|------------|---------------|-------------|
| Baseline (Random) | 100 | 68.5% | - |
| PPO | 100 | 89.2% | +20.7% |
| Genetic Algorithm | 100 | 91.3% | +22.8% |
| Bayesian | 100 | 88.7% | +20.2% |

Note: Results vary based on specific hardware and random seeds.

## Adapting to Other Designs

The AI engine is design-agnostic. To adapt to a different DUT:

1. **Update stimulus ranges** in `config/ai_config.yaml`:
```yaml
stimulus:
  a_range: [0, 255]   # Match your DUT inputs
  b_range: [0, 255]
  # Add other signals as needed
```

2. **Update coverage bins** in `core/coverage_tracker.py`:
```python
OPCODE = ['YOUR_OPS']  # Match your coverage model
CORNER_CASES = ['YOUR_CASES']
```

3. **Regenerate stimuli mapping** if needed:
- Update `action_space` in config
- Modify `StimulusSpace` in `core/base_agent.py`

## Extending to Other Methodologies

The framework can be adapted for:

- **Other HDLs**: VHDL (via VHPI/FLI)
- **Other Testbenches**: e.g., UVM, OSVVM, UVVM, cocotb
- **Other Simulators**: Questa, ModelSim, NCsim

## Troubleshooting

### Compilation Errors
- Ensure VCS is in PATH
- Check UVM version compatibility

### AI Not Improving Coverage
- Increase iterations
- Adjust learning rate
- Try different algorithm

### Performance Issues
- Enable parallel simulation: `make parallel`
- Reduce batch size in config

## License

This project is provided as-is for educational and research purposes.

## References

- [Stable Baselines3](https://stable-baselines3.readthedocs.io/)
- [Gymnasium](https://gymnasium.farama.org/)
- [UVM User Guide](https://verificationacademy.com/)
- [VCS User Guide](https://www.synopsys.com/)

## Contact

For questions or issues, please refer to the documentation in the `docs/` directory.
Details of the implementtion will not be provided out of copyright reason.