# ECE 281 ICE 3: Ripple-Carry Adder with Top Level Design

This is a **template** repository.

[ICE 3 instructions](https://usafa-ece.github.io/ece281-book/ICE/ICE3.html)

Targeted toward Digilent Basys3. Make sure to install the [board files](https://github.com/Xilinx/XilinxBoardStore/tree/2018.2/boards/Digilent/basys3).

Tested on Vivado 2024.2

---
![Waveform for the Ripple Adder on various test cases](ripple_adder_waveform.png)

![Sketch of the basys3 implementation of the Ripple Adder](ripple_adder_basys3_sketch.png)

## Documentation

In the top_basys3 file in Vivado, I struggled to get the wiring to work. I asked ChatGPT for advice on what to code. I originally had
A : in std_logic;
and it told me I needed a std_logic_vector since A is multiple bits. Using this information I changed it to
A : in std_logic_vector(3 downto 0);

Later, the port maps were not working, so I pasted them to ChatGPT and asked if there was anything wrong it could tell me. It told me that I had switched my logic for the outputs S and Cout. I originally had
S => led(15), Cout => led(3 downto 0)
and I needed to have
S => led(3 downto 0), Cout => led(15).


## GitHub Actions Testbench

The workflow uses the [setup-ghdl-ci](https://github.com/ghdl/setup-ghdl-ci) GitHub action
to run a *nightly* build of [GHDL](https://ghdl.github.io/ghdl/).

First, the workflow uses GHDL to **analyze** all `.vhd` files in `src/`.

Then it **elaborates** the entity defined by `$TB_ENTITY`

Finally, the workflow **runs** the simulation. If successful then it will quietly exit with a `0` code.
If any of the `assert` statements fail then GHDL will cease the simulation and exit with non-zero code; this will also cause the workflow to fail.
Assert statements of other severity levels will be reported, but not fail the workflow.
