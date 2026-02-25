# Agda Tutorial — 10‑Minute Pre‑Conference Setup Checklist

**Please complete this before arriving.**
We will start coding immediately and cannot do installations during the session.

This setup normally takes **10–20 minutes on macOS** and **20–40 minutes on Windows/Linux**.

---

## 0. What you need

- Laptop (macOS, Linux, or Windows 10/11)
- Stable internet (first Agda run downloads/compiles interfaces)
- About 4–6 GB free disk space
- VS Code (recommended)

---

## 1. Install Agda

### macOS (recommended & fastest)
Open **Terminal** and run:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew update
brew install agda
brew install agda-stdlib
```

Then register the library:

```bash
mkdir -p ~/.agda
echo "$(brew --prefix)/share/agda-stdlib/standard-library.agda-lib" >> ~/.agda/libraries
echo "standard-library" >> ~/.agda/defaults
```

---

### Windows (please use WSL — strongly recommended)

Open **PowerShell as Administrator**:

```powershell
wsl --install
```

Reboot → open **Ubuntu** and run:

```bash
sudo apt update
sudo apt install ghc cabal-install git build-essential
cabal update
cabal install Agda
echo 'export PATH="$HOME/.cabal/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Standard library:

```bash
git clone https://github.com/agda/agda-stdlib.git
cd agda-stdlib
mkdir -p ~/.agda
echo "$PWD/standard-library.agda-lib" >> ~/.agda/libraries
echo "standard-library" >> ~/.agda/defaults
```

---

### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install ghc cabal-install git build-essential
cabal update
cabal install Agda
echo 'export PATH="$HOME/.cabal/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Standard library:

```bash
git clone https://github.com/agda/agda-stdlib.git
cd agda-stdlib
mkdir -p ~/.agda
echo "$PWD/standard-library.agda-lib" >> ~/.agda/libraries
echo "standard-library" >> ~/.agda/defaults
```

---

## 2. Editor setup (VS Code)

1. Install **Visual Studio Code**
2. Install extension: **Agda Mode**
3. Run once in a terminal:

```bash
agda-mode setup
```

Restart VS Code.

---

## 3. Verify (IMPORTANT)

Run:

```bash
agda --version
```

Create a file `Hello.agda`:

```agda
module Hello where
open import Agda.Builtin.Nat

double : Nat → Nat
double n = n + n
```

Check it:

```bash
agda Hello.agda
```

You should see **no errors**.

---

## 4. Final readiness check

Before the tutorial you should be able to answer YES to all:

- [ ] `agda --version` works
- [ ] `agda Hello.agda` typechecks
- [ ] Opening `.agda` files in VS Code shows no errors

If any box is unchecked, please fix it before attending.

Thank you — this ensures we can spend the session on *dependent types*, not debugging PATH variables 🙂
