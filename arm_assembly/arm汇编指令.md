x86汇编语言有两个主要的语法分支：Intel语法和AT&T语法。
Intel 语法在DOS和Windows世界中占主导地位，而 AT&T 语法在Unix世界中占主导地位，因为 Unix 是在AT&T 贝尔实验室创建的。这里总结了Intel 语法和AT&T 语法之间的主要区别：

|          |                             AT&T                             |                            Intel                             |
| :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 参数顺序 |             源在目的地之前<br />`movl $5, %eax `             |              目的地在源之前.<br />`mov eax, 5 `              |
| 参数大小 | 助记符的后缀是一个表示操作数大小的字母：*q*代表 qword，*l*代表长 (dword)，*w*代表字，*b*代表字节<br/>  `addl $4, %esp ` | 从所使用的寄存器的名称衍生的（例如*RAX，EAX，斧头，人*暗示*Q，L，W，B*分别）<br/>`add esp, 4 ` |
|   符号   | 以“$”为前缀的[立即值](https://en.wikipedia.org/wiki/Constant_(programming))，以“%”为前缀的寄存器 | 汇编器自动检测符号的类型；即，无论它们是寄存器、常量还是其他东西 |
| 有效地址 | *DISP(BASE,INDEX,SCALE) 的*一般语法。例子:<br />`movl mem_location(%ebx,%ecx,4), %eax ` | 方括号中的算术表达式；此外，如果无法从操作数确定大小，则必须使用大小关键字，如*byte*、*word*或*dword*。Example:<br />`mov eax, [ebx + ecx*4 + mem_location]` |

GAS（GNU as）指令（所有架构通用）：https://www.sourceware.org/binutils/docs-2.12/as.info/Pseudo-Ops.html#Pseudo%20Ops



有趣的是，linux中x86架构下的汇编采用AT&T 语法，而arm架构下的汇编则采用Intel语法。嗯。。。



ADR : 装载地址
ADRL : 装载长地址
ALIGN : 对齐指针
DCx : 初始化数据存储
EQUx : 初始化数据存储
OPT : 设置汇编器选项


Rn 是基址寄存器，即存储用于传送的初始地址的ARM寄存器。Rn不能为r15。
! 是一个可选的后缀。如果有!，则最终地址将写回到Rn中。
LDR Rd, =const 使用LDR伪指令加载任何32位常数。
# 立即数前缀

LDRB r2,[r1],#1 用r1所指向的地址的内容加载到r2，然后将r1增加1
LDR和STR
带有直接偏移量、前变址直接偏移量或后变址直接偏移量的加载和存储。
op{type}{cond} Rt, [Rn {, #offset}] ; immediate offset
op{type}{cond} Rt, [Rn, #offset]! ; pre-indexed
op{type}{cond} Rt, [Rn], #offset ; post-indexed
opD{cond} Rt, Rt2, [Rn {, #offset}] ; immediate offset, doubleword
opD{cond} Rt, Rt2, [Rn, #offset]! ; pre-indexed, doubleword
opD{cond} Rt, Rt2, [Rn], #offset ; post-indexed, doubleword
op 可为以下指令之一：
	LDR 加载寄存器。
	STR 存储寄存器。
type 可以是下列项之一：
	B 无符号字节（加载时零扩展为 32 位。）
	SB 有符号字节（仅 LDR。 符号扩展为 32 位。）
	H 无符号半字（加载时零扩展为 32 位。）
	SH 有符号半字（仅 LDR。 符号扩展为 32 位。）
	- 如果是字，则省略。
cond 是一个可选的条件代码（请参阅第2-17 页的条件执行）。
Rt 是要加载或存储的寄存器。
Rn 内存地址所基于的寄存器。
offset 是偏移量。 如果省略了 offset，则该地址为 Rn 中的内容。
Rt2 为附加寄存器，在双字运算中使用，用于加载或存储。

APSR 包含下列 ALU 状态标记：
N 当运算结果为负值时设置此标记。
Z 当运算结果为零时设置此标记。
C 当运算导致进位时设置此标记。
V 当运算导致溢出时设置此标记。
EQ 设置 Z 等于
NE 清除 Z 不等于
CS 或 HS 设置 C 大于或等于（无符号 >=）
CC 或 LO 清除 C 小于（无符号 <）
MI 设置 N 负数
PL 清除 N 正数或零
VS 设置 V 溢出
VC 清除 V 无溢出
HI 设置 C 并清除 Z 大于（无符号 >）
LS 清除 C 或设置 Z 小于或等于（无符号 <=）
GE N 与 V 相同有符号 >=
LT N 与 V 不同有符号 <
GT 清除 Z，N 与 V 相同有符号 >
LE 设置 Z，N 与 V 不同有符号 <=
AL 任何始终。 通常会忽略此后缀。
NV 从不


op1{cond}{.W} label
op2{cond} Rm
其中：
op1 是下列项之一：
	B 跳转。
	BL 带链接跳转
	BLX 带链接跳转并切换指令集。
op2 是下列项之一：
	BX 跳转并切换指令集。
	BLX 带链接跳转并切换指令集。
	BXJ 跳转并转换为 Jazelle 执行。
cond 是一个可选的条件代码（请参阅第2-17 页的条件执行）。 cond 不能用于此指令的所有形式，请参阅第4-112 页的指令可用性和跳转范围。
.W 是一个可选的指令宽度说明符，用于强制要求在 Thumb-2 中使用32 位 B 指令。有关详细信息，请参阅第4-113 页的Thumb-2 中的B。
label 是一个程序相对的表达式。 有关详细信息，请参阅第3-33 页的相对寄存器和程序相对的表达式。
Rm 是一个寄存器，包含要跳转到的目标地址。

ADR{cond}{.W} Rd,label
其中：
cond 是一个可选的条件代码（请参阅第2-17 页的条件执行）。
.W 是可选的指令宽度说明符。 有关详细信息，请参阅第4-22页的Thumb-2中的 ADR。
Rd 是要加载的寄存器。
label 是一个程序相对的表达式。

汇总
助记符	 	简单说明 		页码 		体系结构
ADC，ADD	带进位加法，加法 页4-43 全部
ADR 	加载相对程序或相对寄存器地址（短范围） 页4-21 全部
ADRL 	伪指令加载相对程序或相对寄存器地址（中等范围） 页4-151 x6M
AND 	逻辑“与” 页4-49 全部
ASR 	算术右移 页4-65 全部
B 		跳转 页4-111 全部
BFC，BFI 	位域清零和插入 页4-103 T2
BIC 	位清零 页4-49 全部
BKPT 	断点 页4-128 5
BL 		带链接的跳转 页4-111 全部
BLX 	带链接的跳转，更改指令集 页4-111 T
BX 		跳转，更改指令集 页4-111 T
BXJ 	跳转，更改为 Jazelle 页4-111 J， x7M
CBZ, CBNZ 	比较，如果为（非）零，则跳转 页4-117 T2
CDP 	协处理器数据处理操作 页4-120 x6M
CDP2 	协处理器数据处理操作 页4-120 5， x6M
CHKA 	检查数组 页4-148 EE
CLREX 	清除独占 页4-37 K，x6M
CLZ 	计算前导零数目 页4-52 5， x6M
CMN，CMP 	与负值比较，比较 页4-53 全部
CPS 	更改处理器状态 页4-134 6
DBG 	调试 页4-140 7
DMB，DSB 	数据内存屏障，数据同步屏障 页4-140 7， 6M
ENTERX，LEAVEX 	将状态更改为 ThumbEE 或更改状态 ThumbEE 页4-147 EE
EOR 	异或 页4-49 全部
HB，HBL，HBLP，HBP 		处理程序跳转，跳转到指定处理程序 页4-149 EE
ISB 	指令同步屏障 页4-140 7， 6M
IT 		条件判断 页4-114 T2
LDC 	加载协处理器 页4-125 x6M
LDC2 	加载协处理器 页4-125 5， x6M
LDM 	加载多个寄存器 页4-25 全部
LDR 	加载寄存器指令 页4-10 全部
LDR 	伪指令加载寄存器伪指令 页4-155 全部
LDREX 	独占加载寄存器 页4-34 6， x6M
LDREXB，LDREXH 		独占加载寄存器，半字 页4-34 K，x6M
LDREXD 		独占加载寄存器，双字 页4-34 K，x7M
LSL，LSR 	逻辑左移，逻辑右移 页4-65 全部
MAR 	从寄存器移动到 40 位累加器 页4-143 XScale
MCR 	从寄存器移动到协处理器 页4-121 x6M
MCR2 	从寄存器移动到协处理器 页4-121 5， x6M
MCRR 	从寄存器移动到协处理器 页4-121 5E，x6M
MCRR2 	从寄存器移动到协处理器 页4-121 6， x6M
MIA，MIAPH，MIAxy 	带内部 40 位累加的乘法 页4-87 XScale
MLA 	乘加 页4-69 x6M
MLS 	乘减 页4-69 T2
MOV 	移动 页4-55 全部
MOVT 	移动到顶部 页4-58 T2
MOV32 	伪指令移动 32 位常数到寄存器 页4-153 T2
MRA 	从 40 位累加器移动到寄存器 页4-143 XScale
MRC 	从协处理器移动到寄存器 页4-123 全部
MRC2 	从协处理器移动到寄存器 页4-123 5， x6M
MRS 	从 PSR 移动到寄存器 页4-130 全部
MSR 	从寄存器移动到 PSR 页4-132 全部
MUL 	乘法 页4-69 全部
MVN 	取反移动 页4-55 全部
NOP 	无操作 页4-138 全部
ORN 	逻辑“或非” 页4-49 T2
ORR 	逻辑“或” 页4-49 全部
PKHBT，PKHTB 	组合半字 页4-108 6， x7M
PLD 	预载数据 页4-23 5E，x6M
PLDW 	预载要写入的数据 页4-23 7MP
PLI 	预载指令 页4-23 7
PUSH，POP 将寄存器推入 (PUSH) 堆栈，从堆栈弹出 (POP) 寄存器 页4-28 全部
QADD，QDADD，QDSUB，QSUB 饱和算法 页4-90 5E，x7M
QADD8，QADD16，QASX，QSUB8，QSUB16，QSAX 	并行有符号饱和算法 页4-95 6，x7M
RBIT 	反转位 页4-63 T2
REV，REV16，REVSH 	反转字节顺序 页4-63 6
RFE 	从异常中返回 页4-30 T2，x7M
ROR 	向右循环移寄存器 页4-65 全部
RSB 	反向减法 页4-43 全部
RSC 	带进位反向减法 页4-43 x6M
SADD8，SADD16，SASX 	并行有符号算法 页4-95 6， x7M
SBC 	带进位的减法 页4-43 全部
SBFX，UBFX 		有符号、无符号位域提取 页4-104 T2
SDIV 	有符号除法 页4-67 7M，7R
SEL 	根据 APSR GE 标记选择字节 页4-61 6， x7M
SETEND 	设置内存访问的端标记 页4-137 6， x7M
SEV 	设置事件 页4-138 K， 6M
SHADD8，SHADD16，SHASX，SHSUB8，SHSUB16，SHSAX	并行有符号均分算法 页4-95 6， x7M
SMC 	安全监控调用 页4-136 Z
SMLAD 	两次有符号乘加 页4-82 6， x7M
SMLAL 	有符号乘加 (64 <= 64 +32 x 32) 页4-71 x6M
SMLALxy 有符号乘加 (64 <= 64 +16 x 16) 页4-77 5E，x7M
SMLALD 	两次有符号长整数乘加 页4-84 6， x7M
SMLSD 	两次有符号乘减累加 页4-82 6， x7M
SMLSLD 	两次有符号长整数乘减累加 页4-84 6， x7M
SMMUL 	有符号高位字乘法 (32 <= TopWord(32 x 32)) 页4-80 6， x7M
SMUAD，SMUSD 	有符号双乘法，并将乘积相加或相减 页4-78 6， x7M
SMULxy 	有符号乘法 (32 <= 16 x 16) 页4-73 5E，x7M
SMULL 	有符号乘法 (64 <= 32 x 32) 页4-71 x6M
SMULWy 	有符号乘法 (32 <= 32 x 16) 页4-75 5E，x7M
SRS 	存储返回状态 页4-32 T2，x7M
SSAT 	有符号饱和 页4-92 6， x6M
SSAT16 	有符号饱和，并行半字 页4-100 6， x7M
SSUB8，SSUB16，SSAX 	并行有符号算法 页4-95 6， x7M
STC 	存储协处理器 页4-125 x6M
STC2 	存储协处理器 页4-125 5， x6M
STM 	存储多个寄存器 页4-25 全部
STR 	存储寄存器指令 页4-10 全部
STREX 	独占存储寄存器 页4-34 6， x6M
STREXB，STREXH 	独占存储寄存器，字节或半字 页4-34 K，x6M
STREXD 	独占存储寄存器，双字 页4-34 K，x7M
SUB 	减法 页4-43 全部
SUBS pc, lr 	从异常中返回，无出栈 页4-47 T2，x7M
SVC （以前为 SWI） 	超级用户调用 页4-129 全部
SWP，SWPB 	交换寄存器和内存（仅 ARM） 页4-38 所有，x7M
SXTB, SXTB16, SXTH 	有符号扩展 页4-105 6
SXTAB, SXTAB16, SXTAH 	有符号扩展，带加法 页4-105 6， x7M
TBB，TBH 	表跳转字节、半字 页4-118 T2
TEQ，TST 	相等测试、测试 页4-59 全部
UADD8，UADD16，UASX 	并行无符号算法 页4-95 6， x7M
UDIV 	无符号除法 页4-67 7M，7R
UHADD8，UHADD16，UHASX，UHSUB8，UHSUB16，UHSAX	并行无符号均分算法 页4-95 6， x7M
UMAAL 	无符号长整型乘加累加页4-86 6， x7M
UMLAL，UMULL 	无符号乘加，乘法页4-71 x6M
UQADD8，UQADD16，UQASX，UQSUB8，UQSUB16，UQSAX 	并行无符号饱和算法 页4-95 6， x7M
USAD8 	差值的绝对值无符号求和 页4-98 6， x7M
USADA8 	差值的绝对值无符号求和再累加 页4-98 6， x7M
USAT 	无符号饱和 页4-92 6， x6M
USAT16 	无符号饱和，并行半字 页4-100 6， x7M
USUB8，USUB16，USAX 	并行无符号算法 页4-95 6， x7M
UXTB, UXTB16, UXTH 	无符号扩展 页4-105 6
UXTAB, UXTAB16, UXTAH 	无符号扩展，带加法 页4-105 6， x7M
V* 请参阅第5 章 NEON 和 VFP 编程
WFE，WFI，YIELD 	等待事件，等待中断，通知 页4-138 T2， 6M

a. “体系结构”列中的条目的含义如下：
全部 这些指令可用于所有版本的 ARM 体系结构。
5 这些指令可用于 ARMv5T*、ARMv6* 和 ARMv7 体系结构。
5E 这些指令可用于 ARMv5TE、ARMv6* 和 ARMv7 体系结构。
6 这些指令可用于 ARMv6* 和 ARMv7 体系结构。
6M 这些指令可用于 ARMv6-M 和 ARMv7 体系结构。
x6M 这些指令不可用于 ARMv6-M 体系结构。
7 这些指令可用于 ARMv7 体系结构。
7M 这些指令可用于 ARMv7-M 架构。
x7M 这些指令不可用于 ARMv6-M 和 ARMv7-M 架构。
7R 这些指令可用于 ARMv7-R 架构。
7MP 这些指令可用于实现了多重处理扩展的 ARMv7 体系结构。
EE 这些指令可用于 ARM 体系结构的 ThumbEE 变体。
J 此指令可用于 ARMv5TEJ、ARMv6* 和 ARMv7 体系结构。
K 这些指令可用于 ARMv6K 和 ARMv7 体系结构。
T 这些指令可用于 ARMv4T、ARMv5T*、ARMv6* 和 ARMv7 体系结构。
T2 这些指令可用于 ARMv6T2 及更高版本的体系结构。
XScale 这些指令可用于 ARM 体系结构的 XScale 版本。
Z 此指令仅当执行安全扩展后才可用。

















