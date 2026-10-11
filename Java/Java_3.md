# Reference Guide: OOP vs. Data-Oriented Programming (DOP) in Modern Java
*Based on Dr. Venkat Subramaniam's talk on pragmatic design trade-offs.*

---

## 1. Core Mental Model

| Feature | Object-Oriented Programming (OOP) | Data-Oriented Programming (DOP) |
| :--- | :--- | :--- |
| **Primary Tool** | Polymorphism & Dynamic Dispatch | Pattern Matching & Deconstruction |
| **Model Type** | State encapsulated with behavior | Plain, transparent, immutable data |
| **Extensibility** | Easy to add **new types** (open hierarchies) | Easy to add **new operations** (centralized functions) |
| **Safety Net** | Loose coupling via interfaces | Exhaustiveness checks via compiler |
| **Best Used For** | Core domain entities with rich, owned behavior | External DTOs, wire formats, event processing, ETL |

> **Key Rule of Thumb:**  
> *"Polymorphism is to OOP as Pattern Matching is to DOP."*

---

## 2. Modern Java Building Blocks for DOP

1. **`record`**: Immutable, transparent data carriers.
2. **`sealed interface` / `permits`**: Restricts the hierarchy to a closed, known set of implementations.
3. **Pattern Matching with `switch`**: Evaluates types directly, eliminating casting.
4. **Record Deconstruction**: Directly extracts nested values in matching patterns.
5. **Unnamed Pattern (`_`)**: Discards unused arguments without cluttering code.

---

## 3. Side-by-Side Example: Processing Trades

### Scenario
We receive trade orders (`Buy`, `Sell`, `Hold`) from an external API or queue. We need to calculate commission and log activities.

---

### Approach A: The Traditional OOP Anti-Pattern (When Forced)

Dogmatically avoiding `switch` statements often leads to forced polymorphism or heavyweight patterns (e.g., Visitor, dynamic command factories) on data you don't fully own:

```java
// Requires behavior to be pushed into the data model
public interface Trade {
    double calculateCommission();
}

public class Buy implements Trade {
    private final String ticker;
    private final int quantity;

    public Buy(String ticker, int quantity) {
        this.ticker = ticker;
        this.quantity = quantity;
    }

    @Override
    public double calculateCommission() {
        return quantity * 0.05;
    }
}

// Drawback:
// 1. Every new calculation (tax, audit logging, validation) pollutes the Trade interface.
// 2. If the data classes come from an external library/schema, you cannot add methods to them.
```

---

### Approach B: The Modern DOP Way (Records + Sealed Types + Switch)

Separate the **shape of data** from the **operations performed on data**:

#### 1. Define the Data Domain (Closed Hierarchy)
```java
public sealed interface Trade permits Buy, Sell, Hold {}

public record Buy(String ticker, int quantity, double price) implements Trade {}
public record Sell(String ticker, int quantity, double price) implements Trade {}
public record Hold(String ticker) implements Trade {}
```

#### 2. Implement Operations with Pattern Matching & Exhaustiveness
```java
public class TradeService {

    public static double computeFee(Trade trade) {
        // Compiler guarantees all permitted subtypes of Trade are handled!
        // No 'default' branch needed.
        return switch (trade) {
            // Deconstruction: unpacks values directly
            case Buy(var ticker, var qty, var price) -> qty * price * 0.001;
            case Sell(var ticker, var qty, var price) -> qty * price * 0.0015;
            // Unnamed pattern (_): ignores unused ticker
            case Hold(_) -> 0.0;
        };
    }

    public static String summarize(Trade trade) {
        return switch (trade) {
            case Buy(var ticker, var qty, _) -> "Buying %d shares of %s".formatted(qty, ticker);
            case Sell(var ticker, var qty, _) -> "Selling %d shares of %s".formatted(qty, ticker);
            case Hold(var ticker) -> "Holding position in %s".formatted(ticker);
        };
    }
}
```

---

## 4. Key Takeaways to Remember

1. **Trade-offs Over "Best Practices":**
   * If you need to easily add **new classes** without touching existing code $\rightarrow$ use **OOP Polymorphism**.
   * If you need to easily add **new behaviors/operations** over fixed data $\rightarrow$ use **DOP Pattern Matching**.

2. **Prefer Compile-Time Errors Over Runtime Surprises:**
   * Sealed switches fail at **compile time** when a new subtype is added to the sealed interface.
   * Dynamic discovery/reflection often fails silently at runtime in production.

3. **Modern Refactoring is Cheap:**
   * The Open-Closed Principle was popularized when recompiling large codebases took hours.
   * With modern IDE refactoring and sealed types, modifying a centralized switch statement is fast, localized, and safe.

--   
source: https://www.youtube.com/watch?v=EMdxiVYyMtU