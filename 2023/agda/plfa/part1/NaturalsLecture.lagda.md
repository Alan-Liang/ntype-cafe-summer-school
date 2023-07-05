```agda
module plfa.part1.NaturalsLecture where
```

## 自然数是一种归纳数据类型（Inductive Datatype）

    0
    1
    2
    3
    ...


    0 : ℕ
    1 : ℕ
    2 : ℕ
    3 : ℕ
    ...


    --------
    zero : ℕ

    m : ℕ
    ---------
    suc m : ℕ


a : A : 𝒰₀ : 𝒰₁ : ...


```agda
data ℕ : Set where
  zero : ℕ
  suc  : ℕ → ℕ
```

    zero
    suc zero
    suc (suc zero)
    suc (suc (suc zero))
    ...

## 编译指令

编译器看的注释

```agda
--_ : ℕ
--_ = 1

{-# BUILTIN NATURAL ℕ #-}

_ : ℕ
_ = 1
```

## 导入模块

```agda
import Relation.Binary.PropositionalEquality as Eq
open Eq using (_≡_; refl)
open Eq.≡-Reasoning using (begin_; _≡⟨⟩_; _∎)
```

## 自然数的运算是递归函数 {#plus}

```agda
_+_ : ℕ → ℕ → ℕ
zero + n = n
suc m + n = suc (m + n)
```

```agda
_ : 2 + 3 ≡ 5
_ =
  begin
    2 + 3
  ≡⟨⟩    -- 展开为
    (suc (suc zero)) + (suc (suc (suc zero)))
  ≡⟨⟩    -- 归纳步骤
    suc ((suc zero) + (suc (suc (suc zero))))
  ≡⟨⟩    -- 归纳步骤
    suc (suc (zero + (suc (suc (suc zero)))))
  ≡⟨⟩    -- 起始步骤
    suc (suc (suc (suc (suc zero))))
  ≡⟨⟩    -- 简写为
    5
  ∎
```

```agda
_ : 2 + 3 ≡ 5
_ =
  begin
    2 + 3
  ≡⟨⟩
    suc (1 + 3)
  ≡⟨⟩
    suc (suc (0 + 3))
  ≡⟨⟩
    suc (suc 3)
  ≡⟨⟩
    5
  ∎
```

```agda
_ : 2 + 3 ≡ 5
_ = refl
```

## 乘法

```agda
_*_ : ℕ → ℕ → ℕ
zero    * n  =  zero
(suc m) * n  =  n + (m * n)
```

```agda
_ =
  begin
    2 * 3
  ≡⟨⟩    -- 归纳步骤
    3 + (1 * 3)
  ≡⟨⟩    -- 归纳步骤
    3 + (3 + (0 * 3))
  ≡⟨⟩    -- 起始步骤
    3 + (3 + 0)
  ≡⟨⟩    -- 化简
    6
  ∎
```

## 饱和减法

```agda
_∸_ : ℕ → ℕ → ℕ
m     ∸ zero   =  m
zero  ∸ suc n  =  zero
suc m ∸ suc n  =  m ∸ n
```

```agda
_ =
  begin
    3 ∸ 2
  ≡⟨⟩
    2 ∸ 1
  ≡⟨⟩
    1 ∸ 0
  ≡⟨⟩
    1
  ∎
```

```agda
_ =
  begin
    2 ∸ 3
  ≡⟨⟩
    1 ∸ 2
  ≡⟨⟩
    0 ∸ 1
  ≡⟨⟩
    0
  ∎
```

## 优先级

```agda
infixl 6  _+_  _∸_
infixl 7  _*_
```

## 柯里化

`ℕ → ℕ → ℕ` 表示 `ℕ → (ℕ → ℕ)`

而

`_+_ 2 3` 表示 `(_+_ 2) 3`。

## 交互式地编写定义

(演示)

## 更多编译指令

```agda
--{-# BUILTIN NATPLUS _+_ #-}
--{-# BUILTIN NATTIMES _*_ #-}
--{-# BUILTIN NATMINUS _∸_ #-}
```

## 标准库

本章中类似的定义可在标准库中找到：

```agda
--import Data.Nat using (ℕ; zero; suc; _+_; _*_; _^_; _∸_)
```

## Unicode

这一章使用了如下的 Unicode 符号：

    ℕ  U+2115  双线体大写 N (\bN)
    →  U+2192  右箭头 (\to, \r, \->)
    ∸  U+2238  点减 (\.-)
    ≡  U+2261  等价于 (\==)
    ⟨  U+27E8  数学左尖括号 (\<)
    ⟩  U+27E9  数学右尖括号 (\>)
    ∎  U+220E  证毕 (\qed)
