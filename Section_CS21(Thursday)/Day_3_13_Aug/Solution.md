# Assignment 2 — Sample Solutions 

All 4 problems are classic **Greedy** and **Dynamic Programming** problems. Each solution below has:
1. The full Java code 
2. A simple, plain-language explanation

---

## Q1. 0/1 Knapsack Problem (Marks = 10)

**Problem in simple words:** You have a bag that can carry weight `W`. You have `N` items, each with its own weight and value. For each item you can only make **one choice** — take it whole, or leave it. You cannot break an item into pieces. Find the biggest total value you can carry.

### Java Code

```java
public class Knapsack01 {

    // Returns the maximum value achievable within given capacity
    public static int knapsack(int capacity, int[] weights, int[] values, int n) {
        // dp[i][w] = best value using the first i items with a bag of size w
        int[][] dp = new int[n + 1][capacity + 1];

        for (int i = 1; i <= n; i++) {
            for (int w = 0; w <= capacity; w++) {
                if (weights[i - 1] <= w) {
                    // Option A: skip this item -> dp[i-1][w]
                    // Option B: take this item  -> value + dp[i-1][w - weight]
                    dp[i][w] = Math.max(
                            values[i - 1] + dp[i - 1][w - weights[i - 1]],
                            dp[i - 1][w]
                    );
                } else {
                    // Item is too heavy to even consider, so skip it
                    dp[i][w] = dp[i - 1][w];
                }
            }
        }
        return dp[n][capacity];
    }

    public static void main(String[] args) {
        int[] weights = {2, 3, 4, 5};
        int[] values  = {3, 4, 5, 6};
        int capacity  = 5;
        int n = weights.length;

        int maxValue = knapsack(capacity, weights, values, n);
        System.out.println("Maximum Value = " + maxValue);
    }
}
```

**Output:**
```
Maximum Value = 7
```
(This comes from picking item 1 (weight 2, value 3) and item 2 (weight 3, value 4): total weight = 5, total value = 7.)

### Explanation (Simple Language)

- We build a table `dp[i][w]`, where the rows `i` represent "using only the first `i` items" and the columns `w` represent "bag size available so far".
- For every item, we ask: **"If I add this item, do I get more value than if I skip it?"**
  - If the item is heavier than the bag space left (`w`), we can't take it — copy the answer without this item.
  - Otherwise, compare two choices and keep the bigger one:
    - **Leave it**: value stays the same as using one fewer item.
    - **Take it**: add its value, but now we have less room, so we look up the best answer with the leftover capacity.
- The very last cell `dp[n][capacity]` holds the answer, because it means "using all n items, with the full bag size".
- **Time Complexity:** O(N × W)

---

## Q2. Activity Selection Problem (Marks = 10)

**Problem in simple words:** You have a list of activities, each with a start time and end time. You can only do one activity at a time (no overlaps). Pick the **maximum number** of activities you can attend in a day.

### Java Code

```java
import java.util.Arrays;
import java.util.Comparator;

class Activity {
    int activityId;
    String activityName;
    int startTime;
    int endTime;

    Activity(int id, String name, int start, int end) {
        this.activityId = id;
        this.activityName = name;
        this.startTime = start;
        this.endTime = end;
    }
}

public class ActivitySelection {

    public static void main(String[] args) {
        Activity[] activities = {
                new Activity(1, "A1", 1, 3),
                new Activity(2, "A2", 2, 4),
                new Activity(3, "A3", 3, 5),
                new Activity(4, "A4", 5, 7),
                new Activity(5, "A5", 6, 8),
                new Activity(6, "A6", 8, 9)
        };

        // Step 1: Sort activities by their finishing time (earliest finish first)
        Arrays.sort(activities, Comparator.comparingInt(a -> a.endTime));

        System.out.println("Selected Activities:");

        // Step 2: Always take the very first activity (it finishes earliest)
        int lastEndTime = activities[0].endTime;
        StringBuilder result = new StringBuilder(activities[0].activityName + " ");
        int count = 1;

        // Step 3: Go through the rest; take an activity only if it starts
        // strictly after the last one we picked has ended (no overlap)
        for (int i = 1; i < activities.length; i++) {
            if (activities[i].startTime > lastEndTime) {
                result.append(activities[i].activityName).append(" ");
                lastEndTime = activities[i].endTime;
                count++;
            }
        }

        System.out.println(result.toString().trim());
        System.out.println("Maximum Activities = " + count);
    }
}
```

**Output:**
```
Selected Activities:
A1 A4 A6
Maximum Activities = 3
```

### Explanation 

- **Greedy idea:** Always pick the activity that **finishes earliest** among what's left — this leaves the most room in the day for future activities.
- **Step 1:** Sort all activities by their end time (smallest first).
- **Step 2:** The first activity in this sorted list is always selected — nothing came before it.
- **Step 3:** For every next activity, check: does it start **after** the end time of the last activity we picked? If yes, there's no overlap, so we take it and update our "last end time". If no, we skip it (it overlaps).
- This greedy approach is proven to always give the maximum possible count — you never need to "look back" and undo a choice.
- **Time Complexity:** O(N log N) — dominated by the sorting step.

---

## Q3. Fractional Knapsack Problem (Marks = 10)

**Problem in simple words:** Same bag-and-items setup as Q1, but this time you're **allowed to break items** and take a fraction of one, e.g., half of an item. Find the maximum total value, and how much of each item was taken.

### Java Code

```java
import java.util.Arrays;
import java.util.Comparator;

class Item {
    int weight;
    int value;
    double ratio; // value per unit weight

    Item(int weight, int value) {
        this.weight = weight;
        this.value = value;
        this.ratio = (double) value / weight;
    }
}

public class FractionalKnapsack {

    public static void main(String[] args) {
        int[] weights = {10, 20, 30};
        int[] values  = {60, 100, 120};
        int capacity  = 50;

        int n = weights.length;
        Item[] items = new Item[n];
        for (int i = 0; i < n; i++) {
            items[i] = new Item(weights[i], values[i]);
        }

        // Step 1: Sort items by value-per-weight ratio, highest first
        Arrays.sort(items, Comparator.comparingDouble((Item it) -> it.ratio).reversed());

        double totalValue = 0.0;
        int remainingCapacity = capacity;

        // Step 2: Greedily fill the bag, best ratio first
        for (Item item : items) {
            if (remainingCapacity <= 0) break;

            if (item.weight <= remainingCapacity) {
                // Take the whole item
                totalValue += item.value;
                remainingCapacity -= item.weight;
                System.out.println("Took 100% of item (weight=" + item.weight
                        + ", value=" + item.value + ")");
            } else {
                // Take only a fraction — just enough to fill the bag
                double fraction = (double) remainingCapacity / item.weight;
                totalValue += item.value * fraction;
                System.out.printf("Took %.2f%% of item (weight=%d, value=%d)%n",
                        fraction * 100, item.weight, item.value);
                remainingCapacity = 0;
            }
        }

        System.out.println("Maximum Value = " + totalValue);
    }
}
```

**Output:**
```
Took 100% of item (weight=10, value=60)
Took 100% of item (weight=20, value=100)
Took 66.67% of item (weight=30, value=120)
Maximum Value = 240.0
```

### Explanation (Simple Language)

- **Greedy idea:** Since we can break items, the smartest move is always to grab the item that gives the **most value per kg** first, because it's the "best deal" per unit of bag space.
- **Step 1:** Calculate `ratio = value / weight` for every item, then sort items so the best ratio comes first.
- **Step 2:** Go through items in that order:
  - If the whole item fits in the remaining space, take all of it.
  - If it doesn't fully fit, take just enough of it (a fraction) to fill the bag completely, then stop.
- Unlike 0/1 Knapsack, this greedy method is **always optimal** here — precisely *because* fractions are allowed, so there's no risk of "wasting" leftover space.
- **Time Complexity:** O(N log N) — dominated by the sorting step.

---

## Q4. Minimum Activities to Cover a Time Range (Marks = 10)

**Problem in simple words:** You're given several activities (time intervals). Choose the **fewest** activities such that, when you put them together, they cover every single point in a required time range with no gaps.

### Java Code

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

class Interval {
    String name;
    int start;
    int end;

    Interval(String name, int start, int end) {
        this.name = name;
        this.start = start;
        this.end = end;
    }
}

public class MinimumCoverActivities {

    public static void main(String[] args) {
        Interval[] activities = {
                new Interval("A1", 1, 4),
                new Interval("A2", 2, 6),
                new Interval("A3", 4, 7),
                new Interval("A4", 6, 9),
                new Interval("A5", 8, 10)
        };

        int rangeStart = 1;
        int rangeEnd = 10;

        // Step 1: Sort activities by their start time
        Arrays.sort(activities, Comparator.comparingInt(a -> a.start));

        List<String> selected = new ArrayList<>();
        int covered = rangeStart; // how far we have successfully covered so far
        int i = 0;
        int n = activities.length;
        boolean possible = true;

        while (covered < rangeEnd) {
            int bestEnd = covered;
            String bestName = null;

            // Among all activities that are reachable (start <= covered),
            // pick the one that stretches furthest to the right
            while (i < n && activities[i].start <= covered) {
                if (activities[i].end > bestEnd) {
                    bestEnd = activities[i].end;
                    bestName = activities[i].name;
                }
                i++;
            }

            if (bestName == null) {
                // Nobody can extend the coverage further -> there's a gap
                possible = false;
                break;
            }

            selected.add(bestName);
            covered = bestEnd;
        }

        if (possible) {
            System.out.println("Selected Activities:");
            for (String s : selected) {
                System.out.println(s);
            }
            System.out.println("Minimum Activities = " + selected.size());
        } else {
            System.out.println("Complete coverage of the range is not possible with the given activities.");
        }
    }
}
```

**Output:**
```
Selected Activities:
A1
A3
A4
A5
Minimum Activities = 4
```

### Explanation 

- **Greedy idea:** Think of `covered` as "the furthest point we've successfully covered so far, with no gaps." We repeatedly ask: **"Of all the activities that start at or before `covered`, which one stretches the furthest to the right?"** That one always gives the best possible progress, so we pick it.
- We keep doing this — extending our covered region step by step — until we reach the end of the required range, or until no activity can extend it further (meaning full coverage is impossible).
- **Time Complexity:** O(N log N) — dominated by sorting; the scanning afterward is O(N) because each activity is only looked at once.



