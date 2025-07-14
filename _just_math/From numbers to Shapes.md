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


## From Numbers to Shapes: Geometric Structures

Fields give us the numbers to scale and combine vectors, forming the foundation for geometric structures that bring shapes to life in computer vision and robotics. Let’s explore how vector spaces, algebras, and geometric spaces build on fields to model images, robot movements, and spatial relationships.

### Vector Spaces: Combining Numbers into Vectors

A vector space is a set of vectors—objects like coordinates or arrows—that can be added together and scaled by numbers from a field, like real numbers. Vector spaces have these properties:
- Two operations: Vector addition (commutative, associative, with a zero vector) and scalar multiplication by a field (e.g., $\mathbb{R}$).
- Full scaling: Scalars from a field allow any scaling factor, including fractions.

Vector spaces are the bridge to geometry, enabling precise calculations for image data and robot positions, and supporting bilinear maps like interpolation.

### Algebras: Multiplying Vectors

An algebra extends a vector space by adding a way to multiply vectors together, producing another vector. In computer vision, 3x3 transformation matrices form an algebra. You can add matrices, scale them by real numbers, and multiply them to chain transformations, like rotating then stretching an image. This matrix multiplication is a bilinear map, pairing two matrices to produce another. Algebras have these properties:
- Three operations: Vector addition, scalar multiplication (from a field), and vector multiplication (bilinear).

Algebras model transformations in vision and robotics, connecting arithmetic to geometry through bilinear maps.

### Affine Spaces: Points and Relative Positions

An affine space is a set of points with an associated vector space for displacements, but no fixed origin. In computer vision, image stitching for panoramas treats pixel positions as points in an affine space. Affine transformations (e.g., rotations, translations) align images using vectors for relative shifts, without a fixed reference point. In robotics, SLAM (Simultaneous Localization and Mapping) models a robot’s environment as an affine space, with positions as points and movements as vectors. Affine spaces have these properties:
- Points and displacements: Differences between points yield vectors in a vector space over a field.
- No origin: No point is special as zero, focusing on relative positions.

Affine spaces are key for navigation and alignment, using fields for precise vector calculations.

### Euclidean Spaces: Measuring Distances and Angles

A Euclidean space is a vector space over $\mathbb{R}$ with an inner product—a bilinear map—that defines distances and angles. In computer vision, feature vectors (e.g., edge descriptors) are compared using Euclidean distance (via the dot product) to recognize objects, like faces in photos. In robotics, a robot arm in $\mathbb{R}^3$ calculates distances to objects or angles for precise movements using the inner product. Euclidean spaces have these properties:
- Vector space with inner product: Enables distance and angle calculations via a bilinear map.

Euclidean spaces bring geometry to life, powering tasks like object recognition and robot navigation.

### Other Geometric Spaces: Beyond Euclidean Geometry
Fields and vector spaces also support advanced geometric spaces, like metric spaces, projective spaces, and manifolds. In computer vision, projective spaces model 3D scenes captured by cameras, accounting for perspective. Metric spaces measure distances between point clouds, like 3D scans for reconstruction. In robotics, manifolds model curved terrains, ensuring smooth navigation. These spaces extend Euclidean geometry, using fields for calculations and enabling specialized applications in vision and robotics.


From sets to geometric spaces, we’ve traced a hierarchy that transforms simple numbers into the shapes and spaces driving computer vision and robotics. Below if the figure of the hierarchy we talked about




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