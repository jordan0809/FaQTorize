# FaQTorize

This repo documents the implementation of QFT-adder-based Shor's factoring algorithm ([Beauregard 2003](https://arxiv.org/pdf/quant-ph/0205095)) using CUDA-Q (C++) and Qiskit (Python).

## Usage

**C++ Implementation (CUDA-Q)**

The C++ version utilizes the [NVIDIA CUDA-Q](https://nvidia.github.io/cuda-quantum/latest/index.html) toolchain for high-performance quantum simulation.

1. **Compilation**: Navigate to the `cudaq` directory and use the provided `Makefile`:

```bash
cd cudaq
make
```
This will generate the `shor.exe` executable using the `nvq++` compiler.

2. **Execution**: Run the executable by providing the number to factor ($N$), the coprime base ($a$), and an optional number of phase qubits ($t$).
```bash
# Syntax: ./shor.exe N a [t]
# Example: Factor 15 with base 7
./shor.exe 15 7
```

- **N**: The integer you wish to factorize.
- **a**: A chosen integer coprime to $N$.
- **t** (optional): The number of qubits in the phase register. Defaults to $n$ (bit-length of $N$) if not specified.

3. **Example Output**: When running the C++ implementation, you will see the distribution of measurement outcomes followed by the period extraction and final factors:

```bash
$ ./shor.exe 27 8
Using default t = 5
Total number of qubits = 17

Measurement outcomes: 

{ 00000:3765 00001:3783 10000:3 10001:4 10010:83 10011:79 10100:337 10101:371 10110:173 10111:148 11000:165 11001:167 11010:353 11011:407 11100:76 11101:79 11110:5 11111:2 }
Simulation runtime: 60.6144s

Selected period p = 6

=== Factorization Result ===
27 = 3 x 9
```

**Python Implementation (Qiskit)**

The Python implementation provides better visualizations of the quantum circuit and the measurement distributions. All algorithmic components (Draper adders, modular exponentiation) are encapsulated within the `Shor` class in `shor_qiskit.py`.
- Interactive Demo: See `qiskit_demo.ipynb` for example usage. 

To run the Python version, ensure you have the requirements installed:
```bash
pip install -r qiskit/requirements.txt
```

**Key Performance Note**

For the C++ implementation, if you are factoring larger numbers, it is highly recommended to use an NVIDIA GPU backend (if available) for the simulation by specifying `--target nvidia`:

```bash
nvq++ shor.cpp -o shor.exe --target nvidia
```