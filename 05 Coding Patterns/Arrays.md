# Arrays

**Tags:** #coding #arrays #memory

---

## Declaration and Initialization

```cpp
int readings[5];                      // declare — contents undefined
int readings[5] = {0};               // declare and zero-initialize
int temps[5] = {18, 25, 31, 29, 22}; // declare and initialize

byte buffer[10] = {0};               // memory-efficient: 10 bytes
                                      // vs int buffer[10]: 20 bytes
```

---

## Indexing

Arrays in C++ are **zero-indexed**:
```cpp
temps[0] = 18;   // first element
temps[4] = 22;   // last element (size 5 → indices 0–4)
```

---

## The Bounds Warning — Critical

**C++ does not check array boundaries.**

```cpp
int data[4] = {10, 20, 30, 40};
data[4] = 99;    // index 4 does not exist — silent corruption
                 // overwrites whatever memory is at that address
                 // no error, no warning, unpredictable behavior
```

The off-by-one error:
```cpp
for (int i = 0; i <= 4; i++) {   // WRONG: accesses index 4
  Serial.println(data[i]);
}

for (int i = 0; i < 4; i++) {    // CORRECT: 0, 1, 2, 3 only
  Serial.println(data[i]);
}
```

---

## Passing Arrays to Functions

Arrays in C++ do not carry their own length information.
You MUST pass the size explicitly:

```cpp
int getAverage(int arr[], int size) {
  int sum = 0;
  for (int i = 0; i < size; i++) {
    sum += arr[i];
  }
  return sum / size;
}

// Call:
int avg = getAverage(temps, 5);
```

If you omit `size`, the function has no way to know when to stop.
It will silently read past the end of the array.

---

## Circular Buffer Pattern

Used in moving average filters:

```cpp
const int NUM = 10;
int buffer[NUM] = {0};
int index = 0;
long total = 0;

int addReading(int newVal) {
  total -= buffer[index];    // remove oldest
  buffer[index] = newVal;    // store newest
  total += newVal;           // add newest
  index = (index + 1) % NUM; // advance, wrap around
  return total / NUM;         // return average
}
```

The `%` (modulo) operator wraps the index back to 0
when it reaches `NUM`. No boundary overflow possible.

---

## Related Notes

- [[Moving Average Filter]]
- [[Data Types]]
- [[Functions]]
- [[Buffer Initialization Bug]]
