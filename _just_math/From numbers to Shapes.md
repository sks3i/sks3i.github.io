---
layout: post
title: "Basic Building Blocks of Mathematics"
date: 2025-06-10
---

Mathematics is the universal language powering everything from computer vision to robotics. If you’ve ever skimmed a technical paper filled with terms like “field,” “real number space,” or “vector space,” you’ve encountered the building blocks of modern technology. In this article, we’ll explore these fundamental structures—starting with numbers, moving to vectors, and arriving at the geometry that enables robots to navigate and cameras to recognize faces. By tracing this hierarchy, we’ll uncover how simple arithmetic connects to the shapes and spaces that drive cutting-edge applications.

Now that we’ve seen how mathematics shapes technologies like computer vision and robotics, let’s start at the very beginning: the concept of a set.


## The Foundation: Sets and Algebraic Structures


### Sets: The Starting Point

Sets are a simple collection of entities (numbers, points, or anything else) and there are no rules or operations defined for this. For example, in an image, a set ${0, 1, ..., 255}$ represent pixel values. 

Sets are raw materials and to create a structure out of it, we need to add some operations. A single operation leads to semigroups and two operations leads to semirings.

#### Semigroups

From Sets,  we can add a single way to combine entities, creating a semigroup. For example, chaining image filters (blur, sharpen), forms a semigroup. Since there are inversion or identity operations, they form a semigroup. They have the following properties:

- One operation: Single way to combine entities. 
- Associative: Grouping doesn't affect the results. For example, $ (a \cdot b) \cdot c = a \cdot (b \cdot c)$.
- No identity or inverse operation. 

#### Semiring

Alternatively, we can equip a set with two ways to combine elements, creating a semiring. In computer vision, image dilation—a technique to enhance shapes like edges—uses a tropical semiring. Here, “addition” is taking the maximum pixel intensity in a neighborhood (e.g., a 3x3 kernel), and “multiplication” is adding kernel weights to pixel values.

- Two operations: Addition (commutative, associative, with a zero element) and multiplication (associative, distributes over addition).
- No inverses: No additive inverses (no negatives) or multiplicative inverses (no division).


### Refining the Single Operation: Monoids and Groups

In the one operation framework, what if we want to add "do nothing" operation or "undo" operation. Then, we will end up getting Monoids and Groups.

#### Monoids - Semigroup + "do nothing"

A monoid extends a semigroup with identity operation - "do nothing" operation. In the filter example in semigroups, if we can add "no filter" operation, then the entire operation chain is a monoid.


#### Groups - Monoids + "undo"

A group in a monoid with an inverse (undo) operation. In computer vision, image rotations is a group - -90 degree rotation is an inverse operation for 90 degree rotation. Thus, the group have following properties:
- One operation: A single way to combine elements.
- Associative: Grouping doesn’t change the result.
- Identity: A “do nothing” element exists.
- Inverses: Every element has an inverse that undoes it, e.g., a * a⁻¹ = identity.


### Refining the Two Operations: Rings & Fields

While monoids and groups refine a single operation for tasks like image transformations, semirings introduced two operations, like maximum and addition for image dilation. To make these operations even more powerful, we can add the ability to subtract, creating rings, and then enable division, forming fields. These number systems are the backbone of precise calculations in computer vision and robotics, paving the way for vectors and geometric spaces.


#### Rings - Semiring + "subtraction"

A ring equips a set with two operations—addition and multiplication—where addition allows subtraction (via negatives), and multiplication distributes over addition. Imagine a computer vision system using integer coordinates for pixels, like (100, 200). You can add coordinates to shift an image, subtract to find differences, or multiply for scaling effects, but dividing (e.g., (100, 200) ÷ 2) may produce non-integers, so division isn’t always possible. Rings have these properties:
- Two operations: Addition (commutative, associative, with zero and negatives) and multiplication (associative, distributes over addition, often with 1).
- No multiplicative inverses: Division isn’t guaranteed (e.g., 2 has no inverse in integers).

#### Fields - Semiring + "division"

Fields take rings further by ensuring multiplication is commutative (order doesn’t matter) and every non-zero element has a multiplicative inverse, allowing division. In computer vision, real numbers ($\mathbb{R}$) form a field for pixel coordinates, like (123.45, 67.89). You can add, subtract, multiply to scale, or divide to normalize intensities, all staying within real numbers. For example, bilinear interpolation—a technique to resize images—uses a field to compute new pixel values by pairing vectors. Fields have following properties:
- Two operations: Addition (commutative, associative, with zero and inverses) and multiplication (commutative, associative, with 1 and inverses for non-zero elements).
- Full inverses: Every non-zero element has a reciprocal.

Fields are the cornerstone of number systems, providing the scalars for vectors and geometric spaces that power vision and robotics.







```
Set (No operations)
├── Semigroup (1 operation: Associative)
│   └── Monoid (1 operation: Associative, with identity)
│       └── Group (1 operation: Associative, with identity and inverses)
└── Semiring (2 operations: Addition (commutative, associative, with identity), Multiplication (associative, distributive over addition))
    └── Ring (2 operations: Addition (commutative, associative, with identity and inverses), Multiplication (associative, distributive, often with identity))
        ├── Division Ring (2 operations: Addition (as in ring), Multiplication (associative, with identity and inverses, non-commutative))
        │   └── Field (2 operations: Addition (as in ring), Multiplication (associative, commutative, with identity and inverses))
        └── Module (2 operations: Addition (group), Scalar multiplication by a ring)
            └── Vector Space (2 operations: Addition (group), Scalar multiplication by a field)
                ├── Algebra (2 operations: Vector addition, Scalar multiplication, plus vector multiplication)
                ├── Affine Space (Built on vector space, uses field for displacements)
                ├── Euclidean Space (Vector space with inner product, uses field)
                └── Other Geometric Spaces (e.g., Metric Spaces)
```