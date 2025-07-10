---
created: 2025-07-10T08:43:40-07:00
modified: 2025-07-10T15:49:38-07:00
---

# Basic building blocks

Mathematics is the universal language powering everything from computer vision to robotics. If you’ve ever skimmed a technical paper filled with terms like “field,” “real number space,” or “vector space,” you’ve encountered the building blocks of modern technology. In this article, we’ll explore these fundamental structures—starting with numbers, moving to vectors, and arriving at the geometry that enables robots to navigate and cameras to recognize faces. By tracing this hierarchy, we’ll uncover how simple arithmetic connects to the shapes and spaces that drive cutting-edge applications.

Now that we’ve seen how mathematics shapes technologies like computer vision and robotics, let’s start at the very beginning: the concept of a set.

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


## The Foundation: Sets and Algebraic Structures


### Sets: The Starting Point

Sets are simple collection of entities and there are no rules or operations defined for this. For example, in an image, a set ${0, 1, ..., 255}$ represent pixel values. 

We can increase the complexity by adding operations to the set. If we have one operation, we get Semigroups and with two operations we get Semiring.

#### Semigroups

From Sets,  we can add a single way to combine entities, creating a semigroup. For example,  chaining image filters (blur, sharpening), forms a semigroup. They have the following properties:

● One operation: Single way to combine entities. 
● Associative: Grouping doesn't affect the results. For example, $ (a \cdot b) \cdot c = a \cdot (b \cdot c)$.
● No identity or inverse operation. 

#### Semiring

From Sets, if we add two ways to combine entities,  we get semirings. They have the following properties:

●
