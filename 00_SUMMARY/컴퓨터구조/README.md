# ๐งฑ ์ปดํจํฐ๊ตฌ์กฐ

### โ๏ธ ์ปดํจํฐ ์ฑ๋ฅ์ ๊ฒฐ์ ํ๋ ์์

<img src="images/math/cputime.png" alt="CPUTime = CPUClockCycles * ClockCycleTime \\ = InstructionCount * CyclePerInstruction * cct" width="550">

- **IC** (Instruction Count)
    - ํ๋์จ์ด๊ฐ ์คํํด์ผ ํ๋ instuction์ ์ค์  ์
    - ๊ฐ์ ํ๋ก๊ทธ๋จ์ด๋ผ๋ ๋ฌ๋ผ์ง๋ฉฐ, ISA์ ์ข๋ฅ๋ ์ปดํ์ผ๋ฌ์ ์ญ๋์ ๋ฐ๋ผ์๋ ๋ฌ๋ผ์ง๋ค
        - ๋ํ input data์ ๋ฐ๋ผ์๋ ๋ณํจ
- **CPI** (Cycle-per-Instruction)
    - Instruction์ ์ํ ์๊ฐ์ผ๋ก, instruction์ ๋ง๋ค ๋ค๋ฆ (์ผ๋ฐ์ฐ์ฐ, multiplication, floating)
    - High-level organization(Pipeline, Cache ๋ฑ) ์ค๊ณ์ ๋ฐ๋ผ ์ฌํฅ์ ๋ฐ์
- **cct** (Clock Cycle Time)
    - ํด๋ญ ํ ์ฃผ๊ธฐ์ ๊ธธ์ด
    - Low-level circuit design์ ๊ฐ๊น์ด ์์ญ



### โ๏ธ RISC & CISC

- **RISC (Reduced Instruction Set Computer)**
    - Instruction ํ๋๊ฐ ํ๋ ์ผ์ด ์์
    - `IC` ์ฆ๊ฐ
    - `CPI*cct` ๊ฐ์
    - ISA๊ฐ ๋จ์ํ์ฌ pipelining์ ์ ๋ฆฌ
        - โ `CPI` ๊ฐ์
- **CISC (Complex Instruction Set Computer)**
    - ์ํฉ์ ๋ฐ๋ผ ๊ฐ์ฅ compact ํ instruction ์ฌ์ฉ
        - ๊ฐ์์ผ๋ก ์๋ฅผ ๋ค์๋ฉด CISC๋ 'multiply' instruction์ ์ ๊ณตํ์ง๋ง RISC๋ ์ฌ๋ฌ ๊ฐ์ `add` instruction์ ์ฌ์ฉํ์ฌ ์ปดํ์ผ
    - ๋ฉ๋ชจ๋ฆฌ ๊ณต๊ฐ์ ์ ๊ฒ ์ฐจ์งํ๋ค๋ ์ด์  โ ํ์ฌ์ ์์๋ ๊ณต๊ฐ์ ๋ํ ์ ์ฝ์ด ์ ๊ธฐ์ ํฐ ์๋ฏธ X
    - `IC` ๊ฐ์, `CPI*cct` ์ฆ๊ฐ



### โ๏ธ Amdahl's Law

<img src="images/math/amdahls_law.png" alt="T_{improved} = {T_{affected} \over improvementfactor} + T_{unaffected}" width="550">


- ์์ฃผ ์ฐ์ด๋ ๊ณณ์ ํฌ์๋ฅผ ํด์ผ ํจ๊ณผ๊ฐ ์์(make common case faster)์ ์๋ฏธ
- RISC์ ์ ๋ต๊ณผ ๋ง๋ฟ์ ์๋ ๋ถ๋ถ์ผ๋ก, ๊ฐ๋จํ์ง๋ง ์์ฃผ ์ฌ์ฉ๋๋ instruction์ ์ํ์๊ฐ์ ์ค์ด์



### โ๏ธ MIPS instruction design

![MIPS formats](images/MIPS-formats.png)

- **R-format**

| ํ๋ | ์๋ฏธ                              |
| -- | --------------------------------- |
| op | operation code (opcode)           |
| rs | first source register number      |
| rt | second source register number     |
| rd | destination register number       |
| shamt | shift amount (00000 for now)   |
| funct | function code (extends opcode) |

```asm
add $t0, $s1, $s2
```

- **I-format**

| ํ๋ | ์๋ฏธ                              |
| -- | --------------------------------- |
| rs | base address                           |
| rt | destination or source register number |
| constant | -2<sup>15</sup> ~ 2<sup>15</sup> - 1 |
| address | offset added to base address in `rs`  |

```asm
lw $t0, 32($s2)
sw $t0, 16($s3)
addi $s3, $s3, 4
beq $s1, $s2 label // ์ด๋ destination(target address)์ `PC + offset * 4` ๊ฐ ๋๋ค
```

- **J-format**
    - Jump instruction์ ์ฌ์ฉ
    - Target address = `address * 4`
        - Boundary: 256MB(2<sup>28</sup>) โ Code segment์ ์ต๋ ํฌ๊ธฐ
- **Operand Types**
    - Word: 32bits
    - Halfword: 16bits
    - Byte: 8bits



### โ๏ธ Procedure Execution

- Procedure ์คํ์ ์ํ ์์ ์์
    1. Parameter ๊ฐ์ ๋ ์ง์คํฐ์ ์ ์ฅ (`$a0 - $a3`)
    2. ์คํ์ ์ํด PC ๋ณ๊ฒฝ (jump and link)
    3. ํ์ ์์ ํ๋
    4. ์์ ์ํ
    5. ๊ฒฐ๊ณผ๋ฅผ ์ ์ฅํ์ฌ calling procedure (ํธ์ถ์)๊ฐ ๊ทธ ๊ฐ์ ๋ฐ์ ์ ์๋๋ก ํจ (`$v0 - $v1`)
    6. Caller๋ก ๋์๊ฐ๊ธฐ ์ํด PC ๋ณ๊ฒฝ (jump register - `$ra`)
- **Caller saving & Caller saving**
    - ๋ ์ง์คํฐ์ ์๋ caller ๋ฐ์ดํฐ๋ฅผ ์ง์์ผ ํ  ์๋ ์๋ค.
    - Caller saving
        - Procedure ํธ์ถ ์ ์ caller๊ฐ ์ ์ฅํด๋๋ ๊ฒ
        - โ ๏ธ Callee์์ ์ฐ์ง๋ ์๋๋ฐ ์ ์ฅํ๋ค๋ฉด ๋น ํจ์จ์ 
    - Callee saving
        - ์ด๋ฏธ ์ฌ์ฉ ์ค์ธ ๋ ์ง์คํฐ๋ผ๋ฉด callee๊ฐ ์ ์ฅ ํ ํด๋น ๋ ์ง์คํฐ ์ฌ์ฌ์ฉ
        - โ ๏ธ ๊ผญ ์ ์ฅํด์ผ ํ  ๋ ์ง์คํฐ์ธ์ง ์ ์ ์์
    - MIPS์ ์ ์ฑ โ Saved registers(`$s`)์ Temporary registers(`$t`)์ ๋ถ๋ฆฌ



### โ๏ธ Single Cycle Design

![Single Cycle Design](images/single-cycle-design.png)

- Instruction ๊ฐ๋ตํ ๋ถ๋ฅ
    - ๋ฉ๋ชจ๋ฆฌ ์ฐธ์กฐ: `lw`, `sw`
    - ์ฐ์ ๋ผ๋ฆฌ(Arithmetic-logical): `add`, `sub`, `and`, `or`, `slt`
    - ์ปจํธ๋กค ํ๋ก์ฐ: `beq`, `j`
- 3๊ฐ์ง ๋ฐ๋ณต๋๋ ๋จ๊ณ
    - Instruction Fetch (**IF**): PC โ PC + 4
    - Instruction Decode (**ID**): Instruction์ ์ด๋ป๊ฒ ์ํํ  ์ง ๊ฒฐ์ 
        - ๋ ์ง์คํฐ ์์ ๋ฐ๋ผ ๋ ์ง์คํฐ read
    - Instruction Execute (**EX**): Instruction ์ํ
        - ALU๋ฅผ ์ด์ฉํ์ฌ ์ฐ์ฐ
            - ์์ ๊ณ์ฐ
            - load/store๋ฅผ ์ํ ๋ฉ๋ชจ๋ฆฌ ์ฃผ์ ๊ณ์ฐ



### โ๏ธ Pipelining & Hazzards

![Pipelining](images/pipelining.png)

- Pipelining์ ์ด์ฉํ๋ฉด ์ต๋ stage ์ ๋ฐฐ ๋งํผ ์ฑ๋ฅ์ ํฅ์์ํฌ ์ ์์
- **5 Stages**
    - IF (Instruction Fetch)
    - ID (Instruction Decode)
    - EX (Execute / Address caculation)
    - MEM(Memory Access)
    - WB(Write Back)
- **Hazards**
    1. **Structural hazards**: ๋ฆฌ์์ค๊ฐ ๋ฐ์ ๊ฒฝ์ฐ
        - Instruction ๋ฉ๋ชจ๋ฆฌ์ data memory๊ฐ ๋ฌผ๋ฆฌ์ ์ผ๋ก ๋ถ๋ฆฌ๋์ด ์์ง ์๋ ๊ฒฝ์ฐ
            - lF stage์ Instruction memory์ MEM stage์ Data memory
    2. **Data hazard**: ๋ฐ์ดํฐ read/write๋ก ์ธํด ์ด์  instruction์ด ๋๋  ๋๊น์ง ๊ธฐ๋ค๋ฆฌ๋ ๊ฒฝ์ฐ
        - **ALU Data Hazard**
            - Lose two cycles (2๊ฐ ๋ฒ๋ธ inject ํ์)
            - ํด๊ฒฐ์ฑ: ๋ค 2๊ฐ ์ฐ์ฐ์ ๋ํด **ALU**๋ก ๋ฐ์ดํฐ๋ฅผ ๋ณด๋ด๊ธฐ ์ ์ **ํฌ์๋ฉ(forwarding)**
                - WB stage์์ EX stage๋ก (2๊ฐ ๋ค์ instruction)
                - MEM stage์์ EX stage๋ก (๋ฐ๋ก ๋ค์ instruction)
                - โ Cycle lose โ
        - **Load-Use Data Hazard**
            - 2๊ฐ ๋ค์ instruction์ ๊ฒฝ์ฐ WB stage์์ EX stage๋ก forwarding ๊ฐ๋ฅ
            - โ ๏ธ ๋ฐ๋ก ๋ค์ instruction์ forwarding ๋ถ๊ฐ **(1๊ฐ cycle stall ํญ์ ํ์)**
    3. **Control hazard** (Branch Hazards): ์ด์  instruction์ ๊ฒฐ๊ณผ๊ฐ flow of control์ ๊ฒฐ์ 
        - branch outcome์ MEM stage์์์ผ ๋์ด
        - ํด๊ฒฐ์ฑ: Branch prediction
            - bit comparision์ ์ด์ฉํด์ ๋ ์ ์คํ์ด์ง์์ ๋น ๋ฅด๊ฒ ์ฒดํฌ (ID stage)
            - ์ด ๋ ๋ฐ๋ก ์์ instruction์ด Load ๋ผ๋ฉด **2๊ฐ cycle stall**์ด ํ์ํ๋ค
                - WB stage์์ ID stage๋ก forwarding (WB-EX-MEM-WB)



### โ๏ธ Cache Memeory

<img src="images/math/cache_access_time.png" alt="T_{average} = hitrate * T_{cacheAccess} + (1 - hitrate) * T_{diskAccess}" width="600">

- Cache write์ 2๊ฐ์ง ๋ฐฉ๋ฒ
    - **write-through**
        - ์บ์ ๋ฐ ๋ฉ๋ชจ๋ฆฌ์ ๋์์ ์
        - ๋จ์ : ์ํ ์๊ฐ์ด ๊ธบ
    - **write-back**
        - ์บ์๋ง ์์ ํ๊ณ  ๋ฉ๋ชจ๋ฆฌ๋ ์์ ํ์ง ์์ (์ดํ ์บ์ ์ญ์  ๋ฑ์ ์ํฉ์์ ๋ฉ๋ชจ๋ฆฌ์ write)
        - ๋จ์ : ๊ตฌํ์ด ๋ณต์กํจ (shared-bus multiprocessor system?)



### โ๏ธ Set-Associative Cache

- **Direct Map Cache**
    - Cache์ฃผ์๋ `Tag`, `Index`, `Offset`์ผ๋ก ๊ตฌ์ฑ
    - Direct Map Cache: ๊ฐ์ index๋ฅผ ๊ฐ์ง ์ฃผ์๋ผ๋ฆฌ ํ๋์ block์ ๊ฐ๋ฅดํด
    - ์ฃผ์๋ง๋ค ์บ์ ๋ฉ๋ชจ๋ฆฌ ๋ด ์์ ์ ์๋ ์ฅ์ ๊ณ ์ 
    - โ ๏ธ ๊ฐ์ index๋ฅผ ๊ฐ์ง๋ ์ฃผ์๊ฐ ์ธ์ ํ์ฌ access๋  ๋ cache miss๊ฐ ์ง์์ ์ผ๋ก ๋ฐ์ํ  ์ ์๋ค.
    - Cache size๊ฐ 2<sup>8</sup>์ด๊ณ  block size๊ฐ 2<sup>4</sup>๋ผ๋ฉด ์ด Index์ ๊ธธ์ด: 4 (2<sup>8</sup> / 2<sup>4</sup> = 2<sup>4</sup>), block ์ 16๊ฐ

![Direct Map Cache](images/direct-map-cache.jpg)

- **Two-Way Set-Associative Cache**
    - Cache๋ฅผ 2๊ฐ set์ผ๋ก ์ชผ๊ฐ์ด ๊ฐ์ index๋ฅผ ๊ฐ์ง ์ฃผ์๊ฐ 2 ๊ตฐ๋ฐ์ block์ ์์นํ  ์ ์๋ ๊ตฌ์กฐ
    - Cache size๊ฐ 2<sup>8</sup>์ด๊ณ  block size๊ฐ 2<sup>4</sup>๋ผ๋ฉด ์ด Index์ ๊ธธ์ด: 3 (2<sup>8</sup>/ 2 / 2<sup>4</sup> = 2<sup>3</sup>), set๋ณ block ์ 8๊ฐ

![Two-way Set-Associative Cache](images/two-way-sa-cache.jpg)

- **Fully-Associative Cache**
    - `Index`ํ๋๊ฐ ์ฌ๋ผ์ง๋ฉฐ, `offset`์ ์ ์ธํ ๋๋จธ์ง ์ฃผ์๊ฐ ๋ชจ๋ `tag`๊ฐ ๋จ
    - โ ๏ธ Hit rate๊ฐ ๋์์ง์ง๋ง ํ๋์จ์ด ์ค๊ณ๊ฐ ๋ ๋ณต์กํด์ง
        - 12bits address, cache size 2<sup>8</sup>, block size 2<sup>4</sup>์ด๋ฉด 16-way๊ฐ fully-associative

![Fully-Associative Cache](images/fully-a-cache.png)



### โ๏ธ Multi-Level Cache

- ๋น ๋ฅธ ์ ์ฅ์ฅ์น์ผ์๋ก ๊ฐ๊ฒฉ์ด ๋น์ธ๊ธฐ ๋๋ฌธ์ cache ์ฉ๋์๋ ํ์ค์  ์ ํ์ด ์์ (speed-cost tradeoff)
- `L1`, `L2`, `L3` ๋ก ๋จ๊ณ๋ฅผ ๋๋์ด ์บ์ ๊ตฌ์ฑ
- Average access time ๋น๊ต
    - 1-level: hit time + miss rage * miss penalty
    - 2-level: L1 hit time + L1 miss rate * (L2 hit time + L2 miss rate * L2 miss penalty)



---

### ์ฐธ๊ณ  ์๋ฃ

- ํ์๋ํ๊ต ์ปดํจํฐ๊ตฌ์กฐ ์์ ๊ฐ์์๋ฃ, ์ด์ธํ ๊ต์ (ELE3019)
  - ๊ต์ฌ - Computer Organization and Design: The Hardware and Software Interface 5/e, Patterson and Hennessy
