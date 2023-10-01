# Lecture 5: Instruction-Level Parallelism(2)

## 5.1 Scoreboard for Dynamic Scheduling
- using `scoreboard` to solve the data hazard(one and big manager)
- In-order issue, out-of-order execution, completion

#### 5.1.1 Scoreboard
- Instruction Status
  - `Issue`: Instruction is issued to functional unit
  - `Read Operands`: Instruction is reading its operands
  - `Execution Complete`: Instruction has completed execution
  - `Write Result`: Instruction is writing its result

- Functional Unit Status
  1. `Busy`: FU is being used
  2. `Op`: Operation performed by FU
  3. `Fi`: Destination register of FU
  4. `Fj`, `Fk`: Source register of FU
  5. `Qj`, `Qk`: FU producing `Fj`, `Fk`
  6. `Rj`, `Rk`: Fj, Fk are ready

- Register Result Status
  - Is this register reserved(write) by some instruction?

#### 5.1.2 Stage of scoreboarding
1. Issue
  - Decode instruction and Check structural hazard(FU is busy)
    - Instruction must be issued in program order
    - If structural hazard, stall until FU is free
    - If no structural hazard, check for WAW hazard
      - If WAW hazard, stall until target register is free
  - set `Qj` and `Qk` to functional unit producing the operand
  - set `Vj` and `Vk` to the value of the operand
  - update Register Result Status

2. Read Operands
  - If `Qj` or `Qk` is not empty, check if the corresponding `Rj` or `Rk` is set
  - If yes, copy operand value to `Vj` or `Vk`
  - If no, wait until `Rj` or `Rk` is set

3. Execution
  - Execute the operation if both `Rj` and `Rk` are ready

4. Write Result
  - Write the result to register
  - Stall until no WAR hazard resolved
  - If there are instructions waiting for this result, set `Rj` and `Rk`
  - delete target register from Register Result Status and update score board's `Rj` and `Rk`


## 5.2 Scoreboard Example
- Refer to the [Lec05.pdf](../Slides/Lec05-ilp2_230927_121427.pdf)

## 5.3 TomasuloAlgorithm: Another Dynamic Algorithm
- Resolvation stations for each FU

## 5.4 TomasuloExample One: A Straight-Line Code

## 5.5 TomasuloExample Two: A Loop