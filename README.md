# Pulse-Code-Modulation
# NAME: DEJASHINI T P
# REG NUM: 212224060055
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Tools required
# Program

# Pulse Code Modulation
```
import numpy as np
import matplotlib.pyplot as plt
# ===== Signal Parameters =====
frequency = 2 # Hz
amplitude = 1
duration = 2 # seconds
analog_rate = 1000 # Hz for smooth analog plot
sample_rate = 10 # Hz sampling
num_levels = 8 # Quantization levels
# ===== Analog Signal =====
t = np.linspace(0, duration, int(analog_rate * duration), endpoint=False)
analog_signal = amplitude * np.sin(2 * np.pi * frequency * t)
plt.figure(figsize=(8,4))
plt.plot(t, analog_signal)
plt.title("Original Analog Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()
# ===== Sampling =====
t_samp = np.linspace(0, duration, int(sample_rate * duration), endpoint=False)
sampled_signal = amplitude * np.sin(2 * np.pi * frequency * t_samp)
plt.figure(figsize=(8,4))
plt.stem(t_samp, sampled_signal, linefmt='r-', markerfmt='ro', basefmt=' ')
plt.title("Sampled Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()
# ===== Quantization =====
max_amp = np.max(np.abs(sampled_signal))
step = 2 * max_amp / num_levels
indices = np.round((sampled_signal + max_amp) / step)
indices = np.clip(indices, 0, num_levels-1).astype(int)
quantized_signal = (indices * step) - max_amp
plt.figure(figsize=(8,4))
plt.step(t_samp, quantized_signal, where='mid', color='g')
plt.title("Quantized Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()
# ===== PCM Encoding (Binary) =====
num_bits = int(np.log2(num_levels))
binary_codes = [np.binary_repr(idx, width=num_bits) for idx in indices]
print("Binary Codes for Samples:", binary_codes)
# ===== Transmission (simulated) =====
# For demonstration, we assume noiseless channel
transmitted_codes = binary_codes.copy()
# ===== PCM Decoding =====
decoded_indices = np.array([int(code, 2) for code in transmitted_codes])
decoded_signal = (decoded_indices * step) - max_amp
plt.figure(figsize=(8,4))
plt.step(t_samp, decoded_signal, where='mid', color='purple')
plt.stem(t_samp, decoded_signal, linefmt='g:', markerfmt='go', basefmt=' ')
plt.title("Reconstructed PCM Signal (Demodulated)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()
# ===== Optional: Compare Original Analog and Reconstructed Signal =====
plt.figure(figsize=(8,4))
plt.plot(t, analog_signal, label='Original Analog', alpha=0.5)
plt.step(t_samp, decoded_signal, where='mid', color='purple', label='Reconstructed PCM')
plt.title("Original vs Reconstructed PCM Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.legend()
plt.show()
```
# Delta Modulation
```
import numpy as np
import matplotlib.pyplot as plt

# ===== Signal Parameters =====
frequency = 2        # Hz
amplitude = 1
duration = 2         # seconds
analog_rate = 1000   # Hz for smooth analog plot
sample_rate = 50     # Hz sampling (higher than PCM for better DM)
delta = 0.2          # Step size (important parameter in DM)

# ===== Analog Signal =====
t = np.linspace(0, duration, int(analog_rate * duration), endpoint=False)
analog_signal = amplitude * np.sin(2 * np.pi * frequency * t)

plt.figure(figsize=(8,4))
plt.plot(t, analog_signal)
plt.title("Original Analog Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()

# ===== Sampling =====
t_samp = np.linspace(0, duration, int(sample_rate * duration), endpoint=False)
sampled_signal = amplitude * np.sin(2 * np.pi * frequency * t_samp)

plt.figure(figsize=(8,4))
plt.stem(t_samp, sampled_signal, linefmt='r-', markerfmt='ro', basefmt=' ')
plt.title("Sampled Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()

# ===== Delta Modulation Encoding =====
dm_output = []
reconstructed = []

prev_value = 0   # Initial predicted value

for sample in sampled_signal:
    if sample >= prev_value:
        dm_output.append(1)
        prev_value += delta
    else:
        dm_output.append(0)
        prev_value -= delta
    reconstructed.append(prev_value)

dm_output = np.array(dm_output)
reconstructed = np.array(reconstructed)

print("Delta Modulated Bit Stream:")
print(dm_output)

# ===== Plot Reconstructed Signal =====
plt.figure(figsize=(8,4))
plt.step(t_samp, reconstructed, where='mid', color='purple')
plt.title("Reconstructed Signal (Delta Demodulation)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()

# ===== Comparison Plot =====
plt.figure(figsize=(8,4))
plt.plot(t, analog_signal, label='Original Analog', alpha=0.5)
plt.step(t_samp, reconstructed, where='mid', color='purple', label='Reconstructed DM')
plt.title("Original vs Reconstructed Delta Modulated Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.legend()
plt.show()
# Output Waveform
```

# Pulse-Code-Modulation Waveform
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/9c67c062-02e5-4c93-a04b-d102b6dadaef" />
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/ad1d2c0a-f76c-4f49-9bff-d75e897e82f9" />
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/3abbec2f-4091-4d63-8117-cf27ea56d44c" />



Binary Codes for Samples: ['100', '111', '110', '010', '000', '100', '111', '110', '010', '000', '100', '111', '110', '010', '000', '100', '111', '110', '010', '000']
<img width="885" height="493" alt="image" src="https://github.com/user-attachments/assets/0b5e85ee-39fd-44e2-b0fc-03a9f44baa59" />
<img width="882" height="483" alt="image" src="https://github.com/user-attachments/assets/638264b4-5019-4c68-ab3d-edc02b9d5d5a" />

# Delta Modulatio Waveform
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/ede2078f-9b84-485b-8269-a1e7b86713c0" />
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/8e788f79-b395-48de-aa7a-9abdf5ab0a76" />

Delta Modulated Bit Stream:
[1 1 1 1 1 0 1 0 1 0 0 0 0 0 0 0 0 0 0 1 0 1 1 1 1 1 1 1 1 1 1 0 1 0 0 0 0
 0 0 0 0 0 0 1 0 1 0 1 1 1 1 1 1 1 1 1 1 0 1 0 0 0 0 0 0 0 0 0 0 1 0 1 1 1
 1 1 1 1 1 1 1 0 1 0 0 0 0 0 0 0 0 0 0 1 0 1 0 1 1 1]
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/81f3e30a-3222-4315-862a-4ac24db3b787" />
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/7b2a7246-f9e5-4693-b490-f691af1e0c1d" />
# Results
The analog signal was successfully encoded and reconstructed using PCM and DM techniques in Python, verifying their working principles.
