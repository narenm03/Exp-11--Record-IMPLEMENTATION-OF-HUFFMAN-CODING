# HUFFMAN-CODING

## Aim

To implement Huffman Coding using Python to generate variable-length binary codes for characters based on their frequency of occurrence.

## Software Required

1. Python 3.x
2. Anaconda
3. Jupyter Notebook

## Algorithm

### Step 1:
Get the input string from the user.

### Step 2:
Calculate the frequency of each character in the input string.

### Step 3:
Create a node for each character and insert all nodes into a priority queue.

### Step 4:
Remove the two nodes with the lowest frequencies from the priority queue.

### Step 5:
Combine the two nodes to create a new node whose frequency is the sum of their frequencies.

### Step 6:
Repeat the process until only one node remains. This forms the Huffman Tree.

### Step 7:
Traverse the Huffman Tree and assign `0` to the left branch and `1` to the right branch.

### Step 8:
Display each character, its frequency, and its corresponding Huffman Code.

## Program
```
import heapq
import math


class Node:
    def __init__(self, char, prob):
        self.char = char
        self.prob = prob
        self.left = None
        self.right = None

    def __lt__(self, other):
        return self.prob < other.prob


def generate_codes(root, code="", codes=None):
    if codes is None:
        codes = {}

    if root is None:
        return codes

    # Leaf node
    if root.char is not None:
        codes[root.char] = code
        return codes

    generate_codes(root.left, code + "0", codes)
    generate_codes(root.right, code + "1", codes)

    return codes


def huffman_coding(probabilities):

    # Create priority queue
    heap = []

    for char, prob in probabilities.items():
        heapq.heappush(heap, Node(char, prob))

    # Build Huffman Tree
    while len(heap) > 1:

        left = heapq.heappop(heap)
        right = heapq.heappop(heap)

        new_node = Node(None, left.prob + right.prob)

        new_node.left = left
        new_node.right = right

        heapq.heappush(heap, new_node)

    # Root of Huffman tree
    root = heap[0]

    # Generate Huffman codes
    codes = generate_codes(root)

    return codes


# Given probabilities
probabilities = {
    "A1": 0.40,
    "A2": 0.30,
    "A3": 0.15,
    "A4": 0.10,
    "A5": 0.05
}


# Generate Huffman codes
codes = huffman_coding(probabilities)


# Display Huffman codes
print("Symbol\tProbability\tCode\tLength")
print("--------------------------------------------")

for symbol, probability in probabilities.items():
    length = len(codes[symbol])

    print(f"{symbol}\t{probability}\t\t{codes[symbol]}\t{length}")


# Calculate average code length
average_length = 0

for symbol, probability in probabilities.items():
    length = len(codes[symbol])
    average_length += probability * length


# Calculate entropy
entropy = 0

for probability in probabilities.values():
    entropy += probability * math.log2(1 / probability)


# Calculate coding efficiency
efficiency = (entropy / average_length) * 100


# Calculate redundancy
redundancy = 1 - (entropy / average_length)


print("\nEntropy =", round(entropy, 4), "bits/symbol")

print("Average Code Length =",
      round(average_length, 4),
      "bits/symbol")

print("Coding Efficiency =",
      round(efficiency, 2),
      "%")

print("Redundancy =",
      round(redundancy * 100, 2),
      "%")
```

The Huffman Coding program is implemented using Python.

## Output
<img width="430" height="285" alt="image" src="https://github.com/user-attachments/assets/808742f3-0c5c-4bbd-b559-ba91c742b92f" />

The program uses:

- `heapq` for the priority queue
- `Counter` from `collections` to calculate character frequencies

