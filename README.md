# SSDPL:A completely deep neural network-based method for single-station seismic detection, phase picking, and epicenter location
> [!NOTE]
> The code is built with PyTorch.
> 
> Please install the necessary Python Package before executing the code.
# How to check the seismic waveforms and labels of trainning sets
```
import numpy as np
import matplotlib.pyplot as plt
from Read_data import read
from matplotlib.ticker import FormatStrFormatter

file_name = "../STEAD/merged/merge.hdf5"
csv_file = "../STEAD/merged/merge.csv"

training_data = read(input_hdf5=file_name, input_csv=csv_file, batch_size=128, augmentation=True, mode='test')
batch_x, batch_d, batch_p, batch_s, batch_dist, batch_p_travel, batch_deep, batch_azi = training_data.__getitem__(1)

for k in range(0, 63):
    print("-------------------")
    print(batch_dist[k, 0], batch_azi[k, :])
    print(np.degrees(np.arctan2(batch_azi[k, 1], batch_azi[k, 0])))
    fig, axs = plt.subplots(4, 1, figsize=(10, 6))
    fig.subplots_adjust(hspace=0.1)

    axs[0].plot(batch_x[k, 0, :], 'black', label='E component')
    axs[0].set_xticklabels([])
    axs[0].yaxis.set_major_formatter(FormatStrFormatter('%.2f'))
    axs[0].set_ylabel('Amplitude')
    axs[0].legend()
    axs[1].plot(batch_x[k, 1, :], 'black', label='N component')
    axs[1].set_xticklabels([])
    axs[1].yaxis.set_major_formatter(FormatStrFormatter('%.2f'))
    axs[1].set_ylabel('Amplitude')
    axs[1].legend()
    axs[2].plot(batch_x[k, 2, :], 'black', label=' component')
    axs[2].set_xticklabels([])
    axs[2].yaxis.set_major_formatter(FormatStrFormatter('%.2f'))
    axs[2].set_ylabel('Amplitude')
    axs[2].legend()
    axs[3].plot(batch_d[k, 0, :], '#65ab7c', linestyle='--', linewidth=2, label='Label event')
    axs[3].plot(batch_p[k, 0, :], '#665fd1', linestyle='--', linewidth=2, label='Label p')
    axs[3].plot(batch_s[k, 0, :], '#c44240', linestyle='--', linewidth=2, label='Label s')
    axs[3].set_ylabel('Probability')
    axs[3].set_ylim(0, 1.1)
    axs[3].set_xlabel('Time (s)')
    axs[3].legend(loc='upper right')

    labels = ['(I)', '(II)', '(III)']
    for ax, label in zip(axs.flat, labels):
        ax.text(0.02, 0.85, label, transform=ax.transAxes, fontsize=10, va='top',
                bbox=dict(boxstyle="round,pad=0.3", facecolor="white", edgecolor="black", linewidth=1))

    plt.show()
```
