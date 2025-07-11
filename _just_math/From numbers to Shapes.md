---
layout: post
title: From Numbers to Shapes
date: 2025-06-10
---

# Basic building blocks

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