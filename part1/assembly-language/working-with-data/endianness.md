2.5.1 大小端

让我们用刚刚写的方法来输出内存中存储的值。我们会用两种不同的方法来达到这个目的：第一个种会分开输出它的各个字节，第二种则是按照正常整体输出。参见列表 2-11：

_**Listing 2-11**.endianness.asm_

```
section .data
demo1: dq 0x1122334455667788
demo2: db 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88

section .text

_start:
    mov rdi, [demo1]
    call print_hex
    call print_newline

    mov rdi, [demo2]
    call print_hex
    call print_newline

    mov rax, 60
    xor rdi, rdi
    syscall
```

运行这个程序，输出的结果可能让我们有点儿意外，我们在 demo1 和 demo2 中得到了完全不同的结果。

```
> ./main
1122334455667788
8877665544332211
```

可以看到，多字节的数值是以相反的顺序进行存储的！

每一个字节内的数值顺序倒是比较直观，但是多个字节则是从小一直存储到大了。

这种场景只适用于内存操作：在寄存器中的字节存储是比较自然的顺序。不同的处理器对于数值的存储方式有不同的约定。

* **大端模式**下多字节数是从最大的字节开始存储的。
* **小端模式**下多字节数是从最小的字节开始存储的。

像示例中展示的一样，Intel 64 遵循的是小端的规范。一般来说，选择某种规范只是硬件工程师的一个选择。

上面的约定和字符串或者数组的存储方式没有什么关系。不过如果每一个字符都是由两个而不是一个字节编码而成的话，这些字节会被以反序存储。

小端模式有一些优点，我们可以直接丢弃掉数字的高位来把数字从大范围数转为小范围数，例如 8 字节数转为 4 字节数。

例如，demo3：dq 0x1234。然后为了把这个数字转成 dw，我们需要从 demo3 相同的地址读取一个 dword 数字。参见表 2-1，该数字在内存中的布局详情。

_**Table 2-1.**小端和大端模式下 qword 数字 0x1234 的内存布局情况_

| ADDRESS | VALUE-LE | VALUE-BE |
| :--- | :--- | :--- |
| demo3 | 0x34 | 0x00 |
| demo3 + 1 | 0x12 | 0x00 |
| demo3 + 2 | 0x00 | 0x00 |
| demo3 + 3 | 0x00 | 0x00 |
| demo3 + 4 | 0x00 | 0x00 |
| demo3 + 5 | 0x00 | 0x00 |
| demo3 + 6 | 0x00 | 0x12 |
| demo3 + 7 | 0x00 | 0x32 |

大端比较常用的是网络包的传输场景\(e.g. TCP/IP\)。同时大端还是 JAVA 虚拟机的内部数字格式。

**中端模式**这个概念比较少见。假设我们现在有一些算术过程，需要对 128 位的数值进行计算。那么其字节是像这样存储：先是 8 个倒序的低位字节，然后是 8 个倒序的高位字节：

7 6 5 4 3 2 1 0, 16 15 14 13 12 11 10 9 8
