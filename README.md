# Audio Steganography

Two Python / Jupyter notebooks about **hidden information in audio**. The first finds a secret message hidden in the ultrasonic range of a set of recordings and recovers it. The second hides a text message inside an audio file and extracts it again.

Exercise 3 of an Intelligent Signal Processing course.

## Exercise 3.1: Finding a hidden ultrasonic message

`Exercise 3.1.ipynb` analyses the five recordings in `audio_files/`:

1. **FFT analysis**: computes each file's frequency spectrum and measures how much of its energy is **above 18 kHz** (ultrasonic, so humans can't hear it)
2. **Plots** the spectrum of every file
3. **Flags the suspicious file**, the one with the highest ultrasonic energy ratio (`Ex3_sound4.wav`)
4. **Demodulates** it by multiplying with a 19 kHz carrier, which shifts the hidden signal down into the audible range, then applies a 5th-order Butterworth **low-pass filter** at 4 kHz
5. **Saves** the recovered message as `extracted_secret_code.wav` and plays the original and extracted audio in the notebook

## Exercise 3.2: Pseudo-random LSB steganography

`Exercise 3.2.ipynb` hides the message *"It always seems impossible until it's done"* inside `audio_files/Ex3_sound5.wav`:

- **Pseudo-random positions**: a seeded generator picks which samples carry data, so the bits are scattered through the file
- **Multi-bit LSB embedding**: 3 least significant bits per chosen sample, so only **126 samples (0.03% of the audio)** are changed
- **Bit scrambling** with a second seed for extra security
- **Header and checksum**: a 32-bit length header and an 8-bit XOR checksum let the extractor check the message is intact

The result is saved as `Ex3_sound5_embedded.wav`, which sounds the same as the original. The notebook then extracts the message with the same seeds and confirms it matches:

```
Original:  'It always seems impossible until it's done'
Extracted: 'It always seems impossible until it's done'
Match:     ✓ SUCCESS
```

## Running the notebooks

### Quick start (one command)

**Step 1:** Run this command in the terminal first. It downloads the project from GitHub into a temporary folder, installs the required packages in a separate environment (so your main Python isn't changed), and starts Jupyter:

```bash
D=$(mktemp -d) && gh repo clone Alizea2/ISP-Audio-Steganography "$D" && cd "$D" && python3 -m venv .venv && .venv/bin/pip install -q -r requirements.txt && .venv/bin/jupyter notebook
```

**Step 2:** Jupyter usually opens in your browser by itself. If it doesn't, click the link that starts with **`http://localhost:8888/`** in the terminal output. Copy the whole link, including the `?token=...` part.

**Step 3:** Open **`Exercise 3.1.ipynb`** or **`Exercise 3.2.ipynb`** and choose **Run → Run All Cells**. The results, plots and audio players appear under the cells.

When you're done, close the browser tab and press `Ctrl + C` in the terminal to stop Jupyter.

> This needs Python 3 and the [GitHub CLI](https://cli.github.com/) (`gh`) signed in to an account that can access this repository.

### Manual setup

From inside the project folder:

```bash
pip install -r requirements.txt
jupyter notebook
```

## Project Structure

| File / Folder | Purpose |
|---------------|---------|
| `Exercise 3.1.ipynb` | Ultrasonic detection and demodulation |
| `Exercise 3.2.ipynb` | Pseudo-random LSB embedding and extraction |
| `audio_files/` | The five input recordings |
| `extracted_secret_code.wav` | Output of 3.1: the recovered hidden message |
| `Ex3_sound5_embedded.wav` | Output of 3.2: audio with the text hidden inside |
| `requirements.txt` | Python packages |

## Built With

- Python 3, [Jupyter](https://jupyter.org/)
- [NumPy](https://numpy.org/), [SciPy](https://scipy.org/) (FFT, Butterworth filter, WAV I/O), [Matplotlib](https://matplotlib.org/), [librosa](https://librosa.org/)

## Related exercises

- [ISP-Audio-Effects-App](https://github.com/Alizea2/ISP-Audio-Effects-App): Exercise 1
- [ISP-Audio-Captcha-Voice-Control](https://github.com/Alizea2/ISP-Audio-Captcha-Voice-Control): Exercise 2
- [ISP-Airport-Speech-Recognition](https://github.com/Alizea2/ISP-Airport-Speech-Recognition): Exercise 4

## Author

[@Alizea2](https://github.com/Alizea2)
