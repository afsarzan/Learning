* Red-Black Trees: The self-balancing binary search tree structure that Java 8 uses to rescue heavily collided buckets.
* Never write this by hand: In modern engineering, human error in these methods is unacceptable. Always use your IDE to generate them, use Lombok's @EqualsAndHashCode annotation, or better yet, use Java record classes which automatically generate mathematically perfect implementations of both methods for you at compile time.
* Keep it blazingly fast: Hash-based collections call hashCode() constantly. Avoid expensive operations inside hashCode() like string concatenation, complex math, or accessing a database. It should be a simple mathematical combination of primitive values.
* A Marker Interface (sometimes called a markup interface) is a design pattern in Java where an interface has exactly zero methods or fields. It is entirely empty.
* @RequiredArgsConstructor // <--- This generates the constructor invisibly!
