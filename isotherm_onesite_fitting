import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import curve_fit

label = ['Methane', 'Ethane', 'Ethene', 'Propane', 'Propene', "N\u2082"]

# Define a função de Langmuir-Freundlich
def lf(x, qsati1, a1, b1, qsat2, a2, b2):
    return qsat1 * a1 * (x ** b1) / (1 + a1 * (x ** b1)) + qsat2 * a2 * (x ** b2) / (1 + a2 * (x ** b2))

# Define o nome dos arquivos de dados e as cores dos pontos de dados
files = ["isoterma_sifsix-3-cu_metano.data", 
         "isoterma_sifsix-3-cu_etano.data", 
         "isoterma_sifsix-3-cu_eteno.data", 
         "isoterma_sifsix-3-cu_propano.data", 
         "isoterma_sifsix-3-cu_propeno.data", 
         "isoterma_sifsix-3-cu_N2.data"]
colors = ["fuchsia", "darkorange", "limegreen", "darkgreen", "mediumpurple", "dodgerblue"]

# Define as variáveis qsat iniciais para cada arquivo de dados
qsats = [3.5656231827, 2.8591458505, 3.2334482147, 2.0715282338, 2.6251843622, 2.2140271900]

# Define o modelo de ajuste e as variáveis de ajuste iniciais para cada arquivo de dados
models = [lf, lf, lf, lf, lf, lf]
p0s = [(qsats[i], 0.5, 0.5, qsats[i], 0.5, 0.5) for i in range(len(files))]

# Cria uma figura e um eixo
fig, ax = plt.subplots(figsize=(7, 5))

# Define o tamanho e o formato do eixo e dos ticks
ax.set_xlim([0, 1000])
ax.set_xticks([0, 200, 400, 600, 800, 1000])
ax.set_ylim([0, 6])
ax.set_yticks([0, 2, 4, 6])
ax.tick_params(axis="both", labelsize=18)

# Define os rótulos dos eixos 
ax.set_xlabel("Pressure / kPa", fontsize=20)
ax.set_ylabel("Adsorbed amount / mmol g$^{-1}$", fontsize=20)

# Define as margens do gráfico

for i in range(len(files)):
    # Lê os dados do arquivo
    data = np.loadtxt(files[i])
    x = data[:, 0]
    y = data[:, 1]

    # Plota os pontos de dados
    ax.scatter(x, y, color=colors[i], marker="o", s=50, linewidth=1)

    # Faz o ajuste da curva
    popt, pcov = curve_fit(models[i], x, y, p0=p0s[i])
    x_fit = np.linspace(0, 1000, 1000)
    y_fit = models[i](x_fit, *popt)

    # Plota a curva de ajuste
    ax.plot(x_fit, y_fit, color=colors[i], linewidth=2, linestyle="--", label=label[i])

    with open(f"{files[i]}_ajustado.txt", "w") as f:
        f.write(f"qsat1: {popt[0]}\n")
        f.write(f"a1: {popt[1]}\n")
        f.write(f"b1: {popt[2]}\n")
        f.write(f"qsat2: {popt[3]}\n")
        f.write(f"a2: {popt[4]}\n")
        f.write(f"b2: {popt[5]}\n")

plt.subplots_adjust(bottom=0.15, top=0.9)

# Define a legenda
ax.legend(fontsize=19, loc="upper left", ncol=2)

# Salvando figura em formato png
plt.savefig('isotherm_sifsix-3-cu_ajust.png')
