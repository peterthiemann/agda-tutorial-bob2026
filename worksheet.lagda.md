---
title: Agda by Example — Programming and Proving with Dependent Types
author: (worksheet)
---

This worksheet follows the structure of the slides:

1. Warm-up: simple functions  
2. Inductive data types: lists  
3. Dependent types: vectors  
4. Propositions as types: equality proofs  
5. Case study: `Fin` and safe `lookup`  

Use the holes (`{!!}`) with your editor’s Agda mode (e.g. Emacs or VSCode).

---

```agda
{-# OPTIONS --safe #-}

module worksheet where

open import Data.Nat      using (ℕ; zero; suc; _+_)
open import Relation.Binary.PropositionalEquality using (_≡_; refl)
```

# 1. Warm-Up: Simple Functions (Slide: “Warm-Up: Simple Functions”)

We start with simple functions on natural numbers.

```agda
-- Warm-up definitions

double : ℕ → ℕ
double n = n + n

-- Exercise 1:
-- Implement a function to triple a number n.

triple : ℕ → ℕ
triple n = {!!}
```

Try normalizing expressions like `double (suc (suc zero))` with your editor
command (e.g. `C-c C-n` in Emacs).

# 2. Inductive Data Types: Lists (Slides: “Lists from Scratch” / “Appending Lists”)

We define a polymorphic list type and basic functions.

```agda
-- A simple polymorphic list type

data List (A : Set) : Set where
  []   : List A
  _::_ : A → List A → List A
```

## 2.1 length

```agda
-- Exercise 2.1:
-- Define the length of a list.

length : {A : Set} → List A → ℕ
length xs = {!!}
```

## 2.2 append

```agda
-- Exercise 2.2:
-- Define list append so that append xs ys
-- contains all elements from xs followed by
-- all elements from ys.

append : {A : Set} → List A → List A → List A
append xs ys = {!!}
```

## 2.3 reverse (from the “Appending Lists” slide)

```agda
-- Exercise 2.3:
-- Implement reverse using append (or define a helper like snoc).
-- Try to load the file and let Agda check totality and termination.

reverse : {A : Set} → List A → List A
reverse xs = {!!}
```

# 3. Dependent Types in Action: Vectors (Slides: “Vectors: Lists with Length in the Type”, “Mapping over Vectors”)

We now index lists by their length.

```agda
-- Vectors indexed by length

data Vec (A : Set) : ℕ → Set where
  []   : Vec A 0
  _::_ : {n : ℕ} → A → Vec A n → Vec A (suc n)
```

## 3.1 vmap (slide version)

```agda
-- Exercise 3.1:
-- Implement vmap so that it preserves the length index.

vmap :
  {A B : Set} {n : ℕ} →
  (A → B) → Vec A n → Vec B n
vmap f []        = {!!}
vmap f (x :: xs) = {!!}
```

## 3.2 vappend (exercise from the slide)

```agda
-- Exercise 3.2:
-- Define vector append so that:
--   vappend : Vec A m → Vec A n → Vec A (m + n)

vappend :
  {A : Set} {m n : ℕ} →
  Vec A m → Vec A n → Vec A (m + n)
vappend xs ys = {!!}
```

# 4. Propositions as Types: Equality (Slides: “Equality as a Type”, “A Simple Proof…”, “List Property…”)

We’ll use the standard equality type `_≡_` (already imported).

We also use the usual congruence lemma (`cong`) as on the slide.

```agda
-- Congruence of equality: if x ≡ y, then f x ≡ f y.

cong :
  {A B : Set} (f : A → B) {x y : A} →
  x ≡ y → f x ≡ f y
cong f refl = refl
```

## 4.1 zero-right-id (slide: “A Simple Proof: Right Identity of +”)

```agda
-- Exercise 4.1:
-- Prove that n + 0 ≡ n for all n.
-- Hint: use recursion on n and cong.

zero-right-id : ∀ (n : ℕ) → n + 0 ≡ n
zero-right-id n = {!!}
```


## 4.2 length-append-[] (slide: “List Property: Appending []”)

```agda
-- Exercise 4.2:
-- Prove that appending [] does not change the length.

length-append-[] :
  {A : Set} (xs : List A) →
  length (append xs []) ≡ length xs
length-append-[] xs = {!!}
```

Try to mirror the pattern from the slide:

* Base case: `xs = []` with `refl`.
* Inductive case: use `cong suc`.

## 4.3 empty-right-if (same slide)

The empty list is also a left and right identity for list append.
Do you need induction for both proofs? Experiment!

```agda
empty-right-id : {A : Set} → (xs : List A) → append xs [] ≡ xs
empty-right-id = {!!}

empty-left-id  : {A : Set} → (xs : List A) → append [] xs ≡ xs
empty-left-id = {!!}
```

# 5. Case Study: Fin and Safe Lookup (Slides: “Safe Indexing with Fin”, “Lookup That Cannot Fail”)

We now encode bounded indices with `Fin`.

```agda
-- Finite sets: numbers 0..n-1

data Fin : ℕ → Set where
  fzero : {n : ℕ} → Fin (suc n)
  fsuc  : {n : ℕ} → Fin n → Fin (suc n)
```

## 5.1 lookup (slide: “Lookup That Cannot Fail”)

```agda
-- Exercise 5.1:
-- Implement safe lookup on vectors using Fin.
-- Notice there is no case for the empty vector: Fin 0 has no values.

lookup :
  {A : Set} {n : ℕ} →
  Fin n → Vec A n → A
lookup i xs = {!!}
```


## 5.2 head of a non-empty vector (extra exercise)

```agda
-- Exercise 5.2:
-- Define a safe head for non-empty vectors.
-- Hint: you may use lookup, but you don't have to

headVec :
  {A : Set} {n : ℕ} →
  Vec A (suc n) → A
headVec xs = {!!}
```

## 5.3 vector reverse (challenge)

```agda
-- Exercise 5.3:
-- Define a reverse function on vectors.
-- As a stepping stone, try to implement a snoc function
-- that extends a vector at the right end.
vsnoc : {A : Set} {n : ℕ} → Vec A n → A → Vec A (suc n)
-- see below

vreverse : {A : Set} {n : ℕ} → Vec A n → Vec A n
vreverse [] = []
vreverse (x :: xs) = vsnoc (vreverse xs) x
```

To complete this exercise you will have to make use of the function `subst` below
that applies an equality to a type; and you will have to prove a lemma.
The required lemma will pop out and it is easy to prove by induction.

```agda
subst : {A : Set} {x y : A} → (F : A → Set) → (x ≡ y) → F x → F y
subst F refl fx = fx
```

```agda
vsnoc {A} xs x = {!!}
```

# 6. Optional: Your Own Property

As a final exercise, pick a simple property from your own code (or from this worksheet)
and try to encode it as a type and prove it.

For example, you might try:

```agda
-- Example suggestions:
-- 1. Show that the map function on lists preserves length.
-- 2. Show that map id is the identity function on lists.
-- 3. Show that map (f ∘ g) is equal to map f ∘ map g (where ∘ is function composition).

id : {A : Set} → (x : A) → A
id x = x

_∘_ : {A B C : Set} → (B → C) → (A → B) → (A → C)
(f ∘ g) x = f (g x)

-- my-property : {!!}
-- my-property = {!!}
```

That’s it! Happy programming and proving in Agda!
