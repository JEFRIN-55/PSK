# PSK

# Aim
Write a Python program for the modulation and demodulation of PSK (Phase Shift Keying).

# Tools required
- **Python**  
- **NumPy**: For numerical operations and array handling  
- **Matplotlib**: For waveform plotting  
- **SciPy**: For filtering during demodulation using Butterworth filter

# Program
```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Butterworth low-pass filter for demodulation
def butter_lowpass_filter(data, cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return lfilter(b, a, data)

# Parameters
fs = 1000                # Sampling frequency
f_carrier = 50           # Carrier frequency
bit_rate = 10            # Data rate
T = 1                    # Total time duration
t = np.linspace(0, T, int(fs * T), endpoint=False)

# Message signal (binary data)
bits = np.random.randint(0, 2, bit_rate)
bit_duration = fs // bit_rate
message_signal = np.repeat(bits, bit_duration)

# PSK Modulation (0 -> 0 phase, 1 -> 180° phase shift)
carrier = np.sin(2 * np.pi * f_carrier * t)
psk_signal = np.sin(2 * np.pi * f_carrier * t + np.pi * message_signal)

# PSK Demodulation
demodulated = psk_signal * carrier
filtered_signal = butter_lowpass_filter(demodulated, f_carrier, fs)
decoded_bits = (filtered_signal[::bit_duration] < 0).astype(int)

# Plotting
plt.figure(figsize=(12, 8))

plt.subplot(4, 1, 1)
plt.plot(t, message_signal, label='Message Signal (Binary)', color='b')
plt.title('Message Signal')
plt.grid(True)

plt.subplot(4, 1, 2)
plt.plot(t, carrier, label='Carrier Signal', color='g')
plt.title('Carrier Signal')
plt.grid(True)

plt.subplot(4, 1, 3)
plt.plot(t, psk_signal, label='PSK Modulated Signal', color='r')
plt.title('PSK Modulated Signal')
plt.grid(True)

plt.subplot(4, 1, 4)
plt.step(np.arange(len(decoded_bits)), decoded_bits, label='Decoded Bits', color='r', marker='x')
plt.title('Decoded Bits')
plt.grid(True)
plt.legend()

plt.tight_layout()
plt.show()
```
## Output Waveforms  
- **Message Signal:** The binary data to be transmitted (0s and 1s).  
- **Carrier Signal:** A continuous sine wave at the carrier frequency.  
- **PSK Modulated Signal:** The phase-shifted carrier wave (180° shift for bit ‘1’).  
- **Decoded Bits:** The recovered binary data after demodulation and filtering.  

![image](https://github.com/user-attachments/assets/031420f2-39f6-4476-9f31-54c33e75b794)

## Results  
- The given binary message signal was successfully modulated using Phase Shift Keying (PSK).  
- The carrier wave’s phase was altered (0° for bit ‘0’, 180° for bit ‘1’) during modulation.  
- The demodulated signal was obtained by multiplying with the carrier and passing through a low-pass filter.  
- The recovered bits closely matched the original input, confirming accurate modulation and demodulation.  

# Hardware experiment output waveform.
![WhatsApp Image 2025-05-09 at 18 59 28_56907e22](https://github.com/user-attachments/assets/a862db4b-3368-4aa9-a618-36cf0b57b99a)

