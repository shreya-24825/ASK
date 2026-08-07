# ASK
# Aim
Write a simple Python program for the modulation and demodulation of ASK and FSK.
# Tools required
Google colab
# Program
## ASK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Butterworth Low-Pass Filter
def butter_lowpass_filter(data, cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return lfilter(b, a, data)

# Parameters
fs = 1000            # Sampling frequency (Hz)
f_carrier = 50       # Carrier frequency (Hz)
bit_rate = 10        # Bits per second
T = 1                # Duration (seconds)

# Time vector
t = np.linspace(0, T, int(fs * T), endpoint=False)

# Generate random binary data
bits = np.random.randint(0, 2, bit_rate)
print("Original Bits:", bits)

# Convert bits into a digital message signal
bit_duration = fs // bit_rate
message_signal = np.repeat(bits, bit_duration)

# Carrier signal
carrier = np.sin(2 * np.pi * f_carrier * t)

# ASK Modulation
ask_signal = message_signal * carrier

# ASK Demodulation (Coherent Detection)
demodulated = ask_signal * carrier

# Low-pass filtering
filtered_signal = butter_lowpass_filter(demodulated, f_carrier, fs)

# Decode bits
decoded_bits = (filtered_signal[::bit_duration] > 0.25).astype(int)
print("Decoded Bits :", decoded_bits)

# ---------------- Plotting ----------------

plt.figure(figsize=(12, 10))

# Message Signal
plt.subplot(4, 1, 1)
plt.plot(t, message_signal, color='blue')
plt.title("Message Signal (Binary)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# Carrier Signal
plt.subplot(4, 1, 2)
plt.plot(t, carrier, color='green')
plt.title("Carrier Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# ASK Modulated Signal
plt.subplot(4, 1, 3)
plt.plot(t, ask_signal, color='red')
plt.title("ASK Modulated Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

# Decoded Bits
plt.subplot(4, 1, 4)
plt.step(np.arange(len(decoded_bits)), decoded_bits, where='mid',
         color='purple', marker='o')
plt.title("Decoded Bits")
plt.xlabel("Bit Index")
plt.ylabel("Bit Value")
plt.ylim(-0.2, 1.2)
plt.grid(True)

plt.tight_layout()
plt.show()
```
## FSK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

def butter_lowpass_filter(data, cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return lfilter(b, a, data)

fs = 1000
f1 = 30
f2 = 70
bit_rate = 10
T = 1

t = np.linspace(0, T, int(fs * T), endpoint=False)

bits = np.random.randint(0, 2, bit_rate)
bit_duration = fs // bit_rate
message_signal = np.repeat(bits, bit_duration)

carrier_f1 = np.sin(2 * np.pi * f1 * t)
carrier_f2 = np.sin(2 * np.pi * f2 * t)

fsk_signal = np.zeros_like(t)

for i, bit in enumerate(bits):
    start = i * bit_duration
    end = start + bit_duration
    freq = f2 if bit else f1
    fsk_signal[start:end] = np.sin(2 * np.pi * freq * t[start:end])

ref_f1 = np.sin(2 * np.pi * f1 * t)
ref_f2 = np.sin(2 * np.pi * f2 * t)

corr_f1 = butter_lowpass_filter(fsk_signal * ref_f1, f2, fs)
corr_f2 = butter_lowpass_filter(fsk_signal * ref_f2, f2, fs)

decoded_bits = []

for i in range(bit_rate):
    start = i * bit_duration
    end = start + bit_duration

    energy_f1 = np.sum(corr_f1[start:end] ** 2)
    energy_f2 = np.sum(corr_f2[start:end] ** 2)

    decoded_bits.append(1 if energy_f2 > energy_f1 else 0)

decoded_bits = np.array(decoded_bits)
demodulated_signal = np.repeat(decoded_bits, bit_duration)

plt.figure(figsize=(12, 12))

plt.subplot(6, 1, 1)
plt.plot(t, message_signal, color='b')
plt.title('Message Signal')
plt.grid(True)

plt.subplot(6, 1, 2)
plt.plot(t, carrier_f1, color='g')
plt.title('Carrier Signal for bit = 0 (f1)')
plt.grid(True)

plt.subplot(6, 1, 3)
plt.plot(t, carrier_f2, color='r')
plt.title('Carrier Signal for bit = 1 (f2)')
plt.grid(True)

plt.subplot(6, 1, 4)
plt.plot(t, fsk_signal, color='m')
plt.title('FSK Modulated Signal')
plt.grid(True)

plt.subplot(6, 1, 5)
plt.plot(t, demodulated_signal, color='k')
plt.title('Final Demodulated Signal')
plt.grid(True)

plt.tight_layout()
plt.show()
```
# Output Waveform
### ASK
<img width="1118" height="852" alt="image" src="https://github.com/user-attachments/assets/63bd7e00-77db-47fd-b18d-55407646ab1f" />
### FSK
<img width="1070" height="846" alt="image" src="https://github.com/user-attachments/assets/ddc73c8d-c39d-4e9e-9020-36d8a0cd5a9c" />

# Results
Thus the python program for the modulation and demodulation of ASK and FSK is written and simulated successfully.
