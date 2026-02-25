# Installing Agda (macOS, Linux, and Windows)

This guide explains how to install **Agda**, a dependently typed functional programming language and proof assistant, together with the **Agda standard library** and editor support.

---

## What you will install

You need:

1. **Agda**
2. **The Agda standard library**
3. Editor integration (VS Code or Emacs recommended)

After installation you should be able to run:

```bash
agda --version
```

---

# macOS (Recommended: Homebrew — easiest method)

### 1. Install Homebrew (if not installed)

Open Terminal and run:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Restart the terminal afterwards.

---

### 2. Install Agda + Standard Library

Homebrew provides prebuilt binaries — **no manual GHC/cabal setup needed**.

```bash
brew update
brew install agda
brew install agda-stdlib
```

Verify:

```bash
agda --version
```

---

### 3. Register the standard library

Homebrew installs the library but Agda must be told where it is:

```bash
mkdir -p ~/.agda
echo "$(brew --prefix)/share/agda-stdlib/standard-library.agda-lib" >> ~/.agda/libraries
echo "standard-library" >> ~/.agda/defaults
```

---

### 4. Editor integration

Run once:

```bash
agda-mode setup
```

Then install **Agda Mode** in VS Code (or use Emacs).

You are done.

---

# Linux

These instructions work reliably on Ubuntu/Debian. Other distributions are similar.

## Ubuntu / Debian

Install prerequisites:

```bash
sudo apt update
sudo apt install ghc cabal-install git build-essential
```

Install Agda:

```bash
cabal update
cabal install Agda
```

Add cabal binaries to PATH:

```bash
echo 'export PATH="$HOME/.cabal/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Verify:

```bash
agda --version
```

Install standard library:

```bash
git clone https://github.com/agda/agda-stdlib.git
cd agda-stdlib
mkdir -p ~/.agda
echo "$PWD/standard-library.agda-lib" >> ~/.agda/libraries
echo "standard-library" >> ~/.agda/defaults
```

---

# Windows (Recommended: WSL)

Native Windows installation is fragile. Please use **WSL (Windows Subsystem for Linux)**.

## 1. Install WSL

Open PowerShell as Administrator:

```powershell
wsl --install
```

Reboot when prompted and open **Ubuntu** from the Start menu.

---

## 2. Install Agda inside WSL

```bash
sudo apt update
sudo apt install ghc cabal-install git build-essential
cabal update
cabal install Agda
echo 'export PATH="$HOME/.cabal/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Verify:

```bash
agda --version
```

Install standard library:

```bash
git clone https://github.com/agda/agda-stdlib.git
cd agda-stdlib
mkdir -p ~/.agda
echo "$PWD/standard-library.agda-lib" >> ~/.agda/libraries
echo "standard-library" >> ~/.agda/defaults
```

---

# Editor Support

## VS Code (recommended)

1. Install **Visual Studio Code**
2. Install extension: **Agda Mode**
3. Run once:

```bash
agda-mode setup
```

## Emacs

Agda ships with an Emacs mode. After installation:

```bash
agda --emacs-mode setup
```

More information can be found in the [official documentation](https://agda.readthedocs.io/en/latest/tools/emacs-mode.html).

---

# Testing Your Installation

Create a file `Hello.agda`:

```agda
module Hello where

open import Agda.Builtin.Nat

double : Nat → Nat
double n = n + n
```

Then run:

```bash
agda Hello.agda
```

If no errors appear, your installation is correct.

---

# Common Problems

### `agda: command not found`
Restart the terminal or ensure PATH contains:
```
~/.cabal/bin
```

### Standard library not found
Check:
```
~/.agda/libraries
~/.agda/defaults
```

### First load is slow

Normal — Agda compiles library interfaces the first time.

---

You are ready for the tutorial.
