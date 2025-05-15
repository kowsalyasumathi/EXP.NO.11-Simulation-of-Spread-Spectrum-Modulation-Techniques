# EXP.NO.11-Simulation-of-Spread-Spectrum-Modulation-Techniques

11.Simulation of Spread Spectrum Modulation Techniques

# AIM
To simulate and analyze Spread Spectrum Modulation Techniques using Direct Sequence Spread Spectrum (DSSS) to observe their performance in resisting interference and enhancing communication security.

# SOFTWARE REQUIRED
Google Colab (python).

# ALGORITHMS
```
## Direct Sequence Spread Spectrum (DSSS)
Sender Side:
1)Input binary data (e.g., 1011).
2)Generate a pseudo-random noise (PN) sequence (e.g., 111000111).
3)Spread each data bit using the PN sequence via XOR (for BPSK modulation).
4)'1' → transmit PN sequence
5)'0' → transmit inverted PN sequence
6)Transmit the spread signal.
##Receiver Side:
1)Receive spread signal.
2)Use same PN sequence to despread (XOR).
3)Recover original binary data.
```
# PROGRAM
```
import numpy as np
import matplotlib.pyplot as plt

# System Parameters
data_length = 4                # Number of bits
chips_per_bit = 8              # PN sequence length
bit_rate = 1e3                 # 1 kbps
chip_rate = bit_rate * chips_per_bit
carrier_freq = 20e3            # 20 kHz carrier
sample_rate = 160e3            # 160 kHz sampling rate
samples_per_chip = int(sample_rate / chip_rate)

# Generate random binary data
def generate_data(length):
    return np.random.randint(0, 2, length)

# Generate PN sequence: ±1 chips
def generate_pn_sequence(length):
    return np.random.choice([-1, 1], length)

# BPSK mapping: 0 → -1, 1 → +1
def bpsk_modulate(bit):
    return 2 * bit - 1

# DSSS spreading
def dsss_spread(data, pn_sequence):
    spread = []
    for bit in data:
        bpsk_bit = bpsk_modulate(bit)
        spread.extend(bpsk_bit * pn_sequence)
    return np.array(spread)

# BPSK carrier modulation of spread signal
def carrier_modulate(spread_signal, carrier_freq, sample_rate, samples_per_chip):
    total_samples = len(spread_signal) * samples_per_chip
    t = np.arange(total_samples) / sample_rate
    carrier_wave = np.cos(2 * np.pi * carrier_freq * t)
    
    # Repeat each chip to match carrier sampling
    chip_samples = np.repeat(spread_signal, samples_per_chip)
    return chip_samples * carrier_wave, t

# Main function
if __name__ == "__main__":
    # Generate input
    data = generate_data(data_length)
    pn_seq = generate_pn_sequence(chips_per_bit)

    print("Original Data Bits:     ", data)
    print("PN Sequence:            ", pn_seq)
    # DSSS spreading
    spread_signal = dsss_spread(data, pn_seq)

    # BPSK Carrier modulation
    bpsk_waveform, t = carrier_modulate(spread_signal, carrier_freq, sample_rate, samples_per_chip)

    # Plot DSSS spread signal (chip values)
    plt.figure(figsize=(12, 3))
    plt.plot(spread_signal, drawstyle='steps-mid')
    plt.title("DSSS Spread Signal (Baseband)")
    plt.xlabel("Chip Index")
    plt.ylabel("Amplitude")
    plt.grid(True)
    plt.tight_layout()

    # Plot BPSK modulated carrier waveform
    plt.figure(figsize=(12, 3))
    plt.plot(t, bpsk_waveform)
    plt.title("BPSK Modulated Waveform (Carrier)")
    plt.xlabel("Time (s)")
    plt.ylabel("Amplitude")
    plt.grid(True)
    plt.tight_layout()
    plt.show()
```

# OUTPUT
```
Original Data Bits:      [1 0 1 1]
PN Sequence:             [ 1  1  1  1  1  1 -1  1]
```
![image](https://github.com/user-attachments/assets/97f8c11e-8b71-4216-aafd-0be6636b8ee3)
![image](https://github.com/user-attachments/assets/fba11b0d-40e7-4bd9-ab92-50c65d89fb53)

# RESULT / CONCLUSIONS
Thus the simulation of Spread Spectrum Modulation Techniques is successfully implemented using Direct Sequence Spread Spectrum .
