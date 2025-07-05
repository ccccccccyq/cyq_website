# Signals and Systems

!!! example
    已知如图所示的三角波信号$f(t)$：  
    1. 求出其频谱。  
    2. $f_s(t)$是对$f(t)$以等间隔 $T/8$ 进行抽样所得信号，
    分析并画出其频谱图$F_{ps}(j\omega)$。  
    3. 将$f(t)$以周期 $T$ 进行周期延拓构成周期信号$f_p(t)$，
    画出对$f_p(t)$以等间隔 $T/8$ 进行抽样所得信号的频谱 $F_{ps}(j\omega)$。

![picture](.\images\triangle_wave.png)

**Answer：**

1. 利用Fourier变换的微分特性，或直接利用三角波信号的频谱公式，可以得到：

$$
F(j\omega) = \frac{T}{2}{Sa}^2(\frac{\omega T}{4}),
  其中Sa(x) = \frac{\sin(x)}{x}.
$$

2.$f_s(t)$ 是对 $f(t)$ 以等间隔 $T_1 = T/8$ 进行抽样所得信号，
即$f_s(t) = f(t) \delta_{T_1}(t)$。因此有

$$
F_s{(j\omega)} = \frac{8}{T}\sum_{n=-\infty}^{+\infty} F[j(\omega - n\omega_s)],
  其中\omega_s = \frac{2\pi}{T_1} = \frac{16\pi}{T}.
$$

3.$f_p(t)$是对$f(t)$以周期T进行的周期延拓，
即 $f_p(t) = f(t) * \delta_{T_1}(t)$ ,所以有

$$
F_p(j\omega) = \omega_m \sum_{n=-\infty}^{+\infty} F(jn\omega_m) \delta(\omega - n\omega_m) =
  \pi \sum_{n=-\infty}^{+\infty} {Sa}^2(\frac{n\pi}{2}) \delta(\omega - \frac{2n\pi}{T})
$$

!!! example
    试求周期矩形脉冲信号的功率谱，并计算在其有效带宽 $(0 到 \frac{2\pi}{\tau})$内谐波
    分量所具有的平均功率占整个信号的平均功率的百分比。其中
    $A = 1, T_0 = \frac{1}{4}, \tau = \frac{1}{20}$。

**Answer：**
周期矩形脉冲的频谱为：

$$ C_n = \frac{A \tau}{T_0}Sa(\frac{n \omega_0 \tau}{2}) $$

将 $A = 1, T_0 = \frac{1}{4}, \tau = \frac{1}{20}$ 代入，得到：

$$ C_n = 0.2 Sa(\frac{n \omega_0}{40})  = 0.2 Sa(\frac{n \pi}{5}) $$

因此可得周期矩形脉冲信号的功率谱为：

$$ P_n = |C_n|^2 = 0.04Sa^2(\frac{n \pi}{5}) $$

显然，在有效带宽$(0 到 \frac{2\pi}{\tau})$内，包含了一个直流分量和四个谐波分量。
信号的平均功率为：

$$ P = \frac{1}{T_0} \int_{-\frac{T_0}{2}}^{\frac{T_0}{2}} |f(t)|^2 \, dt = 0.2$$

而包含在有效带宽$(0 到 \frac{2\pi}{\tau})$内各个谐波平均功率之和为：

$$ P_1 = \sum_{n=-4}^{4} |C_n|^2 = |C_0|^2 + 2 \sum_{n=1}^{4} |C_n|^2 = 0.1806 $$

$$ \frac{P_1}{P} = \frac{0.1806}{0.2} \approx 90% $$

上式表明，周期矩形脉冲信号包含在有效带宽内的各谐波平均功率之和占整个信号平均功率的90%。
因此，若用**直流分量、基波、二次、三次、四次谐波来近似周期矩形脉冲信号，可以达到很高的精度**。
同样，若该信号在通过系统时，只损失了有效带宽以外的所有谐波，则信号只有较少的失真。
