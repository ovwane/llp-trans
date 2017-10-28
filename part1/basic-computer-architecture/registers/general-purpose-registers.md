1.3.1 General Purpose Registers

程序员大部分时间都在和通用寄存器打交道。通用寄存器本身可互换并且可被不同的命令所用。

从 r0，r1 到 r15 都是 64-bit 的寄存器。前八个寄存器可以有选择性地进行命名，一般其别名的含义会指代其在指令中承担的职责。例如 r1 也被称为 rcx，c 代表着循环\(cycle\)。汇编中的 loop 指令即会使用 rcx 作为循环的计数器，loop 指令本身不接收显式的操作数。当然，这种类型的特殊寄存器在指令中如何使用在文档的指令介绍中都会有所反映\(e.g. 例如这里说的作为 loop 指令的计数器\)。表格 1-2 列出了所有的通用寄存器；同时也可以参考图 1-3。

| Name | Alias | Description |
| :--- | :--- | :--- |
| r0 | rax | Kind of an “accumulator,” used in arithmetic instructions. For example, an instruction div is used to divide two integers. It accepts one operand and uses rax implicitly as thesecondone.Afterexecutingdiv rcxabig128-bitwidenumber,storedinpartsin two registers rdx and rax is divided by rcx and the result is stored again in rax. |
| r3 | rbx |  |
| r1 | rcx |  |
| r2 | rdx |  |
| r4 | rsp |  |
| r5 | rbp |  |
| r6 | rsi |  |
| r7 | rdi |  |
| r8 |  |  |
| r9 ... r15 | no |  |


