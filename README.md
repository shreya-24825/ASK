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

fs = 1000
fc = 50

# Generate random bits
bits = np.random.randint(0, 2, 10)
print("Original Bits :", bits)

t = np.arange(0, 1, 1/fs)

bit_duration = len(t) // len(bits)
message = np.repeat(bits, bit_duration)

# Carrier
carrier = np.sin(2 * np.pi * fc * t)

# ASK Modulation
ask = message * carrier

# ASK Demodulation
demod = ask * carrier

# Decode bits
decoded = []

for i in range(len(bits)):
    start = i * bit_duration
    end = (i + 1) * bit_duration

    value = np.mean(demod[start:end])
    decoded.append(1 if value > 0.2 else 0)

print("Decoded Bits  :", decoded)

# Plotting
plt.figure(figsize=(12, 10))

plt.subplot(4, 1, 1)
plt.plot(t, message)
plt.title("Message Signal (Binary)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(4, 1, 2)
plt.plot(t, carrier)
plt.title("Carrier Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(4, 1, 3)
plt.plot(t, ask)
plt.title("ASK Modulated Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(4, 1, 4)
plt.step(np.arange(len(decoded)), decoded, where='mid', marker='o')
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

fs = 1000
f1 = 30
f2 = 70

bits = np.random.randint(0, 2, 10)
print("Original bits:", bits)

t = np.arange(0, 1, 1/fs)

bit_dur = len(t) // len(bits)
message = np.repeat(bits, bit_dur)

# Carrier signals
carrier1 = np.sin(2 * np.pi * f1 * t)
carrier2 = np.sin(2 * np.pi * f2 * t)

# FSK Modulation
fsk = np.zeros(len(t))

for i in range(len(bits)):
    start = i * bit_dur
    end = (i + 1) * bit_dur

    if bits[i] == 0:
        fsk[start:end] = carrier1[start:end]
    else:
        fsk[start:end] = carrier2[start:end]

# FSK Demodulation
decoded = []

for i in range(len(bits)):
    start = i * bit_dur
    end = (i + 1) * bit_dur

    power1 = np.mean(fsk[start:end] * carrier1[start:end])
    power2 = np.mean(fsk[start:end] * carrier2[start:end])

    if power2 > power1:
        decoded.append(1)
    else:
        decoded.append(0)

print("Decoded bits:", decoded)

# Plotting

plt.figure(figsize=(12, 10))

plt.subplot(4, 1, 1)
plt.plot(t, message)
plt.title("Message Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(4, 1, 2)
plt.plot(t, carrier1)
plt.title("Carrier Signal f1 (Bit = 0)")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(4, 1, 3)
plt.plot(t, fsk)
plt.title("FSK Modulated Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(4, 1, 4)
plt.step(np.arange(len(decoded)), decoded, where='mid', marker='o')
plt.title("Decoded Bits")
plt.xlabel("Bit Index")
plt.ylabel("Bit Value")
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
