# Embedded Systems 

当采用统一操作码，指令长度与各类指令的地址长度发生矛盾时，
通常采用**扩展操作码**技术加以解决。
扩展操作码是一种**指令优化技术**，即让**操作码的长度随地址数的减少而增加（即扩展）**。
根据不同的地址指令格式，如三地址、二地址、单地址指令等，
操作码的位数可以有不同的选择从而在满足需要的前提下，有效地缩短了指令长度。

!!! example
    假设一个使用虚拟内存和L1缓存的存储系统具有以下特征:

    a) 内存系统按字节寻址，访存请求每次仅传递一个字节给处理器。  
    b) 虚拟地址长度14比特，物理地址长度12比特。  
    c) 页大小64字节，使用单级页表。  
    d) TLB拥有16个条目，四路组相联。  
    e) L1缓存物理寻址，块大小4字节，共16个组，直接映射。  

    现在 CPU 发起了一次对虚拟地址 0x05a4 的单字节内存加载请求，回答以下问题。  
    (i)若请求发起时，TLB的部分内容如下表所示。则TLB是否发生命中？ 
    如果命中，此次内存访问的物理地址是多少？ 
    <table border="1" cellspacing="0" cellpadding="5" style="width: 100%; text-align: center;">
        <tr>
            <th>组号</th>
            <th>标签</th>
            <th>物理页号</th>
            <th>有效位</th>
            <th>标签</th>
            <th>物理页号</th>
            <th>有效位</th>
        </tr>
        <tr>
            <td rowspan="2">0</td>
            <td>0x0B</td>
            <td>—</td>
            <td>0</td>
            <td>0x1F</td>
            <td>—</td>
            <td>0</td>
        </tr>
        <tr>
            <td>0x07</td>
            <td>0x0D</td>
            <td>1</td>
            <td>0x02</td>
            <td>0x2F</td>
            <td>1</td>
        </tr>
        <tr>
            <td rowspan="2">1</td>
            <td>0x01</td>
            <td>0x05</td>
            <td>1</td>
            <td>0x05</td>
            <td>0x0D</td>
            <td>1</td>
        </tr>
        <tr>
            <td>0x14</td>
            <td>—</td>
            <td>0</td>
            <td>0x2A</td>
            <td>0x16</td>
            <td>1</td>
        </tr>
        <tr>
            <td rowspan="2">2</td>
            <td>0x03</td>
            <td>—</td>
            <td>0</td>
            <td>0x05</td>
            <td>0x1C</td>
            <td>1</td>
        </tr>
        <tr>
            <td>0x0B</td>
            <td>0x07</td>
            <td>1</td>
            <td>0x00</td>
            <td>0x1B</td>
            <td>1</td>
        </tr>
        <tr>
            <td rowspan="2">3</td>
            <td>0x26</td>
            <td>0x34</td>
            <td>1</td>
            <td>0x02</td>
            <td>—</td>
            <td>0</td>
        </tr>
        <tr>
            <td>0x19</td>
            <td>0x2F</td>
            <td>1</td>
            <td>0x38</td>
            <td>—</td>
            <td>0</td>
        </tr>
    </table>

    (ii)该系统的页表有多少个条目？  
    (iii)如果TLB命中，则使用1. 得到的物理地址，否则使用物理地址0x1e4。  
    如果L1缓存的内容如下表所示，则此次访存请求是否命中缓存？如果命中，访存结果是多少？

**Answer：**
!!! tip "什么是TLB？"
    **TLB (Translation Lookaside Buffer)** 是一种用于加速虚拟地址到物理地址转换的
    **硬件缓存**。在现代计算机系统中，虚拟内存通过页表将虚拟地址映射到物理地址，
    但查找页表可能很慢。而 TLB 是处理器中的一个小型缓存，用来**快速查找虚拟地址和**
    **物理地址之间的映射**。如果所需的地址映射在 TLB 中存在，就可以快速获取物理地址，
    减少延迟。这种情况称为 **TLB 命中 (TLB hit)**，而当 TLB 中没有所需的映射时，
    称为 **TLB 缺失 (TLB miss)**，此时需要通过访问页表来查找映射。

(i)
这道题目使用单级页表，Page Size = 64B，所以需要6bit的页内偏移量（offset）对其进行索引。  

对于VA:(1) offset = 6bit. VPN:(2) = 8bit.  $VA = VPN + offset$  
对于PA:(3) offset = 6bit. PFN:(4) = 6bit.  $PA = PFN + offset$  
{ .annotate }

1. Virtual Address.
2. Virtual Page Number
3. Physical Address
4. Page Frame Number

$VPN = Tag + index$
TLB为四路组相联，组数 = 16/4 = 4, 所以需要2bit的组索引（index）对其进行索引；剩余的
6bit全部作为Tag。对于虚拟地址0x05a4 = 0b0000_0101_1010_0100，我们易得
Tag = 0b00_0101 = 0x05；Index = 0b10 = 0x2。查表TLB可知TLB命中，
对应的物理页号为0x1C,与offset拼接之后的物理地址为0x724.

(ii)
根据VPN位宽，需要的页表条目数：$2^{8} = 256$ 条。

(iii)
(i)中得到的物理地址PA = 0b0111_0010_0100 = 0x724.由于缓存的公式

$$ PA = Tag + index + offset $$

我们观察L1缓存的表，可知cache line size = 4B，所以块内偏移量offset = 2bit。
由于该L1缓存是两路组相联，组数 = 32/2 = 16。所以索引到组的index = 4bit。由于该题中PA = 12bit，所以
Tag = 12bit - 4bit - 2bit = 6bit。取PA = 0x724的前6bit，即Tag = 0b01_1100 = 0x1C，
index = 0x9, offset = 0x0。所以查L1缓存的表，可知L1缓存命中，对应的访存结果为0x63。

!!! example
    根据给出的不同缓存配置，补全下表中缺失的组数量、组索引位数、标签位数和偏移位数。  

    <table border="1" cellspacing="0" cellpadding="5" style="width: 100%; text-align: center;">
        <tr>
            <th rowspan="2">编号</th>
            <th rowspan="2">地址位数<br>(Bit)</th>
            <th rowspan="2">缓存大小<br>(KB)</th>
            <th rowspan="2">块大小<br>(Byte)</th>
            <th rowspan="2">相联度</th>
            <th rowspan="2">组数量</th>
        </tr>
        <tr>
            <th>组索引位数<br>(Bit)</th>
            <th>标签位数<br>(Bit)</th>
            <th>偏移位数<br>(Bit)</th>
        </tr>
        <tr>
            <td>1</td>
            <td>32</td>
            <td>4</td>
            <td>64</td>
            <td>2</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>2</td>
            <td>32</td>
            <td>4</td>
            <td>64</td>
            <td>8</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>3</td>
            <td>32</td>
            <td>4</td>
            <td>64</td>
            <td>全相联</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>4</td>
            <td>32</td>
            <td>16</td>
            <td>64</td>
            <td>1</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>5</td>
            <td>32</td>
            <td>16</td>
            <td>128</td>
            <td>2</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>6</td>
            <td>32</td>
            <td>64</td>
            <td>64</td>
            <td>4</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>7</td>
            <td>32</td>
            <td>64</td>
            <td>64</td>
            <td>8</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>8</td>
            <td>32</td>
            <td>64</td>
            <td>128</td>
            <td>16</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </table>

**Answer:**

我们先分析第一行的空余部分：缓存大小为$4KB$，而块大小为$64B$，所以一共有
$4*1024B \div 64B = 64$个缓存块。而该缓存是二路组相联缓存，所以一共有
$64 \div 2 = 32$组。由于有$32$组，所以索引到组的index需要$log_2 32 = 5$bit。
又由于每个缓存块大小为$64B$，所以块内偏移量offset为$log_2 64 = 6$bit。所以标签需要
$32 - 5 - 6 = 21$bit。

我们在再分析一种全相联的情况（编号3）。同上，我们可以先计算出组数量。由于是全相联，所以组数量
就是1，所以索引到组的index需要$log_2 1 = 0$bit。（即不需要index）。又由于每个缓存块大
小为$64B$，所以块内偏移量offset为$log_2 64 = 6$bit。所以标签需要
$32 - 0 - 6 = 26$bit。

我们可以依此计算出其余行的答案。最后的答案如下表所示：

<table border="1" cellspacing="0" cellpadding="5" style="width: 100%; text-align: center;">

    <tr>
        <th rowspan="2">编号</th>
        <th rowspan="2">地址位数<br>(Bit)</th>
        <th rowspan="2">缓存大小<br>(KB)</th>
        <th rowspan="2">块大小<br>(Byte)</th>
        <th rowspan="2">相联度</th>
        <th rowspan="2" class="highlight">组数量</th>
    </tr>

    <tr>
        <th>组索引位数<br>(Bit)</th>
        <th>标签位数<br>(Bit)</th>
        <th>偏移位数<br>(Bit)</th>
    </tr>

    <tr>
        <td>1</td>
        <td>32</td>
        <td>4</td>
        <td>64</td>
        <td>2</td>
        <td class="highlight">32</td>
        <td>5</td>
        <td>21</td>
        <td>6</td>
    </tr>

    <tr>
        <td>2</td>
        <td>32</td>
        <td>4</td>
        <td>64</td>
        <td>8</td>
        <td class="highlight">8</td>
        <td>3</td>
        <td>23</td>
        <td>6</td>
    </tr>

    <tr>
        <td>3</td>
        <td>32</td>
        <td>4</td>
        <td>64</td>
        <td>全相联</td>
        <td class="highlight">1</td>
        <td>0</td>
        <td>26</td>
        <td>6</td>
    </tr>

    <tr>
        <td>4</td>
        <td>32</td>
        <td>16</td>
        <td>64</td>
        <td>1</td>
        <td class="highlight">256</td>
        <td>8</td>
        <td>18</td>
        <td>6</td>
    </tr>

    <tr>
        <td>5</td>
        <td>32</td>
        <td>16</td>
        <td>128</td>
        <td>2</td>
        <td class="highlight">64</td>
        <td>6</td>
        <td>19</td>
        <td>7</td>
    </tr>

    <tr>
        <td>6</td>
        <td>32</td>
        <td>64</td>
        <td>64</td>
        <td>4</td>
        <td class="highlight">256</td>
        <td>8</td>
        <td>18</td>
        <td>6</td>
    </tr>

    <tr>
        <td>7</td>
        <td>32</td>
        <td>64</td>
        <td>64</td>
        <td>8</td>
        <td class="highlight">64</td>
        <td>6</td>
        <td>20</td>
        <td>6</td>
    </tr>

    <tr>
        <td>8</td>
        <td>32</td>
        <td>64</td>
        <td>128</td>
        <td>16</td>
        <td class="highlight">32</td>
        <td>5</td>
        <td>20</td>
        <td>7</td>
    </tr>
</table>

!!! example
    考虑如下所示的C语言代码片段：

    ``` c
    for(int i = 0; i < 64; i++) {
        for(int j = 0; j < 64; j++) {
            A[i][j] = A[j][i] + 1;
        }
    }
    ```

    在内存中，数据的组织方式为`A[p][q]`与`A[p][q+1]`相邻。请从利用缓存空间局部性的角度
    ，优化以上代码，不可更改代码实现的功能。

**Answer:**

**行优先存储**。更改后的代码如下：

```c
for(int j = 0; j < 64; j++) {
    for(int i = 0; i < 64; i++) {
        A[i][j] = A[j][i] + 1;
    }
}
```