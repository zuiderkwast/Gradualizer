---
title: The Gradualizer
author: Josef Svenningsson
date: A long time ago, in a galaxy far, far away...
---

# Introduction

This document describes an new gradual type system for Erlang, and The
Gradualizer, which is a tool which checks types for Erlang programs.

# Type System

Type rules as if Erlang was typed

The judgment is somewhat non-standard. It consists of four parts,
*Γ, E , T* and *Δ*. *Γ* is the environment and contains
the type of the free variables and functions. *E* and *T* are the
expression and its type respectively. *Δ* is the variables bound
in the expression. *Γ* and *E* should be seen as input to the
typechecking algorithm, and *T* and *Δ* as outputs.


     m : f : (S) → T ∈ Γ & Γ ⊢ E : S ⊣ Δ
    —————————————————————————————————————
             Γ ⊢ m : f(E) : T ⊣ Δ

       V : T ∈ Γ
    ———————————————
     Γ ⊢ V : T ⊣ Δ

     Γ ⊢ E_1 : T_1 ⊣ Δ_1 & … & Γ ⊢ E_n : T_n ⊣ Δ_n
    ———————————————————————————————————————————————————————
     Γ ⊢ { E_1, … , E_n } : {T_1, …, T_n } ⊣ Δ_1 ∪ … ∪ Δ_n

     Γ ⊢ E_1 : S ⊣ ϒ & Γ ∪ ϒ ⊢ E_2 : T ⊣ Δ
    ———————————————————————————————————————
         Γ ⊢ E_1 , E_2 : T ⊣ Δ ∪ ϒ

           Γ ⊢ E : T ⊣ Δ
    ——————————————————————————
     Γ ⊢ V = E : T ⊣ Δ(V : T)

## Subtyping

    —————————
      T ⊑ T

       ∀ i . T_i ⊑ S
    ———————————————————
     T_1 | … | T_n ⊑ S

       ∃ i . T ⊑ S_i
    ————————————————————
     T ⊑ S_1 | … | S_n

# Gradual Type System

In gradual type systems there is particular type which means
"dynamically typed", and which is used to indicate which part of the
program should not be statically typed. In Erlang there is the type
*any()* which already has this role, in the sense that any expression
has the type *any()*. So we will reuse *any()* in our gradual type
system to mean that an expression is untyped.

## Compatibility relation

    any() ~ T

    T ~ any()

    R ~ T & S ~ U ⇒ {R,S} ~ {T,U}

## Type Rules
