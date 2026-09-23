# 🃏 Cipher Cards

**Cipher Cards** is an interactive web app that implements and *visually explains* the **Solitaire Cipher**, a card-based encryption algorithm. Unlike a typical cipher tool that just spits out a result, this app shows you exactly what's happening to the deck of cards at every step, making it a hands-on way to understand how the algorithm actually works.

🔗 **Live demo:** [hamzaoukach.github.io/CipherCards](https://hamzaoukach.github.io/CipherCards/)

## Overview

The Solitaire Cipher uses a shuffled deck of 54 cards (52 standard cards + 2 jokers) to generate a stream of keystream values, which are then used to encrypt or decrypt a text message letter by letter. Cipher Cards brings this process to life with a visual, step-by-step breakdown of every card movement and calculation.

## Features

- ✍️ Manual message input or load from a text file
- 🖼️ Load an image, converted into a simplified binary alphabet
- 🔒 Encrypt and 🔓 decrypt messages
- 💾 Save results to a text file
- 🎴 Graphical, dynamic rendering of the card deck (via Canvas)
- 👣 Step-by-step mode that explains every transformation in detail

## How It Works

### The Deck
The deck is represented as an array of 54 integers:
- Cards 1–52 are the standard playing cards (e.g. `A♣ = 1`, `K♣ = 13`, `A♦ = 14`, `K♠ = 52`)
- `53` = Black Joker
- `54` = Red Joker

### Generating a Keystream Value
For each character encrypted or decrypted, the algorithm performs, in order:
1. Move the **black joker** down one position
2. Move the **red joker** down two positions
3. Perform a **triple cut** around the two jokers
4. Perform a **count cut** based on the value of the bottom card
5. Read the card indicated by the top card of the deck to get the keystream value

If that card is a joker, the process repeats until a usable card is found.

### Transforming Letters
Letters are mapped to numbers (`A = 1 ... Z = 26`). Given a letter value `L` and a keystream value `K`:

- **Encryption:** `((L + K − 1) mod 26) + 1`
- **Decryption:** `((L − K − 1 + 26) mod 26) + 1`

### The Seed
The initial deck order is generated from a **seed**. Using the same seed for encryption and decryption ensures both sides start from the identical shuffled deck — this is what keeps the cipher reversible.

## Step-by-Step Mode

Rather than only showing a final result, this mode walks through the cipher character by character, displaying:
- The current state of the deck
- The operation being performed
- The character being processed
- The card used to generate the keystream value, with the math behind it (e.g. `7♥ = (2 × 13) + 7 = 33`)
- The partial output so far
- A full breakdown of the letter transformation math

## Tech Stack

The entire application is a single self-contained HTML file:
- **HTML** for structure
- **CSS** for styling
- **JavaScript** for the cipher logic, animations, and interactivity
- **Canvas** for dynamically drawing the cards

No server, build step, or external framework required — just open it in a browser.

## Testing

The app includes a `runTests()` function covering three areas:

- **Unit tests** — each core operation (joker moves, triple cut, count cut, text cleaning, letter/number conversion) tested in isolation
- **Round-trip tests** — verifies `decrypt(encrypt(message, seed), seed) === message` across multiple sample messages
- **Cryptographic properties** — determinism (same seed → same output), different seeds → different outputs, ciphertext differs from plaintext, message length is preserved, and spaces stay in place

## Limitations

- Built for academic/educational purposes, not real-world security use
- Image handling is simplified
- Some visual cues (like marking the start/end of the deck) could be clearer

## Possible Improvements

- Full step history log
- Detailed export of the step-by-step explanations
- Better visualization of card positions within the deck
- Automated test suite

Project completed for M1 Informatique, Université de Bourgogne (UFR Sciences et Techniques), supervised by Vincent Vajnovszki.
