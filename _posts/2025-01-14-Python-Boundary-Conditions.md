---
layout: post
title: "Coding 102"
date: 2025-01-14
categories: Python
---

![Author](https://img.shields.io/badge/author-Tanishq_Sadanala-blue)

# Learning to Code in 2025: Why Loops and Boundaries Matter

If you are learning to code in 2025 or so, you are bombarded with a lot of information on the internet and also through LLMs.  
In this age of *vibe coding* it is essential for us to understand how code works **in and out**.

---

## The Challenge of Starting Out

It can be challenging for a few people to grasp what exactly is happening at the beginning stages.  
I started to learn code in 2022, and it was just the beginning of the LLMs. They were not good in code, so I just copy pasted or asked my friends how these things work.

People don’t tell you the bare basics of a concept and make a strong prejudice that you already have a strong foundation.  
As a result, it gave me an **illusion of competence**.

---

## The Case of Loops

One of them is **loops**.  
I thought I understood loops, but when I started to dig into a few LeetCode problems, I realized that I was missing the concept of **boundary conditions**.

---

## Before LeetCode

So before jumping into LeetCode, make sure that you get this right!

We all know what a loop is, but when it comes to writing correct code, the devil is in the **boundary conditions**.  
Python gives us two primary types of loops:

- `for` loop
- `while` loop

---

## 1. For Loop

```python
for i in range(10):
    print(i)
````

The `for` loop in Python uses `range()`, which auto-increments under the hood.
This code prints numbers from **0 to 9** — exactly 10 numbers.

---

## 2. While Loop

```python
i = 0
while i < 10:
    print(i)
    i += 1
```

Here, you manually handle the increment (`i += 1`).
Both `for` and `while` achieve the same thing in this case, but the boundary conditions are where errors usually sneak in.

---

## 3. Math Intervals and Loops

If you’re familiar with math intervals and boundaries, this is easy. If not, check this reference: [Intervals in Mathematics](https://en.wikipedia.org/wiki/Interval_%28mathematics%29#Including_or_excluding_endpoints).

In programming, **interval patterns** matter:

* **Closed interval** `[a, b]` → includes both ends
* **Half-open interval** `[a, b)` → includes `a`, excludes `b`

Python’s `range()` always works with `[start, stop)`.
That’s why:

```python
for i in range(5):
    print(i)
```

prints `0,1,2,3,4` and **excludes 5**.

---

## 4. Array Indexing Example

Let’s take an abstracted array:

```python
arr = [1, 2, 3, 4, 5]
```

If you want the **last element**:

```python
arr[len(arr)-1]
```

We subtract `1` because indexing starts from `0`.

Now, looping through all elements:

```python
for i in range(len(arr)):
    print(arr[i])
```

This prints all elements correctly because `range(len(arr))` → `[0, len(arr))`.

Mathematically, this is: `[0, 5)` → includes `0,1,2,3,4`.

---

## 5. A Common Confusion

Sometimes beginners write:

```python
for i in range(arr[len(arr)-1]):
    print(i)
```

But this doesn’t give the array elements — it just loops from `0` up to `arr[-1]` (in this case `5`).

So be careful:

* `range(len(arr))` → iterates over **indices**
* `for x in arr` → iterates over **values**

---

## 6. Boundary Conditions in Practice

Let’s apply this to a **LeetCode problem**:

> **Problem:**
> Input: `nums = [1,2,3,4]`
> Output: `[1,3,6,10]`
> Explanation: Running sum is `[1, 1+2, 1+2+3, 1+2+3+4]`.

A simple **1-pointer approach**:

```python
i = 0
s = 0
while i <= len(nums)-1:
    s += nums[i]
    nums[i] = s
    i += 1
return nums
```

At first glance, this looks fine. But `<= len(nums)-1` risks an **index out of bounds** error.
Safer option:

```python
while i < len(nums):
    s += nums[i]
    nums[i] = s
    i += 1
```

---

## 7. The Core Idea

It all boils down to **interval choice**:

* If you use **closed intervals** `[a, b]`, use `<=`.
* If you use **half-open intervals** `[a, b)`, use `<`.

Python’s design favors `[a, b)` patterns — hence why `range()` excludes the endpoint.

---

