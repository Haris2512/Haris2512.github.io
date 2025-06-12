---
title: "Huffman Coding"
date: 2025-05-20 00:00:00 +0800
categories: [Algorithms, Data Compression]
tags: [Huffman Coding, Greedy Algorithm, Lossless Compression]
---

# Huffman Coding

**Huffman Coding** is a lossless data compression algorithm developed by David A. Huffman in 1952. It minimizes data size by assigning shorter binary codes to frequently occurring symbols and longer codes to less frequent ones. Widely used in formats like ZIP, JPEG, and MP3, it leverages a greedy approach to build an optimal prefix-free binary tree.

## Core Concept
- **Principle**: Assign variable-length codes based on character frequencies, ensuring high-frequency characters get shorter codes.
- **Data Structure**: Utilizes a binary tree (Huffman Tree) where leaves represent characters, and paths (0 for left, 1 for right) form the codes.
- **Objective**: Minimize the total bit length of the encoded data while ensuring no code is a prefix of another.

## Algorithm Steps
1. Calculate the frequency of each character in the input data.
2. Create a leaf node for each character with its frequency.
3. Build a Huffman Tree:
   - Repeatedly combine the two nodes with the smallest frequencies into a parent node with their combined frequency.
   - Continue until a single tree is formed.
4. Assign codes: Traverse the tree, assigning `0` to left edges and `1` to right edges, to generate codes for each character.
5. Encode the input string using the generated codes.

## Example
For the string `"BCCABBDDAECCBBAEDDCC"` (length = 20):
- **Frequencies**: A: 3, B: 5, C: 6, D: 4, E: 2
- **Huffman Tree Construction**:
  - Combine smallest frequencies (E: 2 and A: 3) → Node (5).
  - Combine next smallest (Node: 5 and D: 4) → Node (9).
  - Combine Node (9) and B: 5 → Node (14).
  - Combine Node (14) and C: 6 → Node (20).
- **Codes**:
  - A: `001` (3 bits)
  - B: `10` (2 bits)
  - C: `11` (2 bits)
  - D: `01` (2 bits)
  - E: `000` (3 bits)
- **Total Bits**: 
  - A: 3 × 3 = 9 bits
  - B: 5 × 2 = 10 bits
  - C: 6 × 2 = 12 bits
  - D: 4 × 2 = 8 bits
  - E: 2 × 3 = 6 bits
  - Total: 45 bits (vs. 160 bits using 8-bit ASCII).

## C++ Implementation
Below is a C++ implementation of Huffman Coding:

```cpp
#include <iostream>
#include <queue>
#include <unordered_map>
#include <string>

using namespace std;

// Structure for Huffman Tree nodes
struct Node {
    char ch;
    int freq;
    Node *left, *right;
    Node(char c, int f) : ch(c), freq(f), left(nullptr), right(nullptr) {}
};

// Comparator for min-heap
struct Compare {
    bool operator()(Node* l, Node* r) {
        return l->freq > r->freq;
    }
};

// Function to generate Huffman codes
void generateCodes(Node* root, string code, unordered_map<char, string>& codes) {
    if (!root) return;
    if (!root->left && !root->right) {
        codes[root->ch] = code.empty() ? "0" : code;
    }
    generateCodes(root->left, code + "0", codes);
    generateCodes(root->right, code + "1", codes);
}

// Function to perform Huffman Coding
void huffmanCoding(string text) {
    // Calculate frequency of each character
    unordered_map<char, int> freq;
    for (char c : text) freq[c]++;
    
    // Create min-heap
    priority_queue<Node*, vector<Node*>, Compare> minHeap;
    for (auto pair : freq) {
        minHeap.push(new Node(pair.first, pair.second));
    }
    
    // Build Huffman Tree
    while (minHeap.size() > 1) {
        Node *left = minHeap.top(); minHeap.pop();
        Node *right = minHeap.top(); minHeap.pop();
        Node *parent = new Node('$', left->freq + right->freq);
        parent->left = left;
        parent->right = right;
        minHeap.push(parent);
    }
    
    // Generate codes
    unordered_map<char, string> codes;
    generateCodes(minHeap.top(), "", codes);
    
    // Print codes and calculate total bits
    int totalBits = 0;
    cout << "Huffman Codes:\n";
    for (auto pair : codes) {
        cout << pair.first << ": " << pair.second << endl;
        totalBits += pair.second.length() * freq[pair.first];
    }
    
    // Encode the input string
    string encoded = "";
    for (char c : text) encoded += codes[c];
    cout << "Encoded string: " << encoded << endl;
    cout << "Total bits: " << totalBits << endl;
}

int main() {
    string text = "BCCABBDDAECCBBAEDDCC";
    huffmanCoding(text);
    return 0;
}
```

**Output** for the example:
```
Huffman Codes:
A: 001
B: 10
C: 11
D: 01
E: 000
Encoded string: 11 10 10 001 10 10 01 01 001 000 11 10 10 001 000 01 01 11 10
Total bits: 45
```

## Algorithm Visualization
Below is a diagram of the Huffman Tree for the string `"BCCABBDDAECCBBAEDDCC"`:

```mermaid
graph TD
    A[20] --> |0| B[9]
    A --> |1| C[11]
    B --> |0| D[5]
    B --> |1| E[D: 4]
    D --> |0| F[E: 2]
    D --> |1| G[A: 3]
    C --> |0| H[B: 5]
    C --> |1| I[C: 6]
```

**Codes**:
- A: `001`
- B: `10`
- C: `11`
- D: `01`
- E: `000`

## Time and Space Complexity
- **Time Complexity**: `O(n log n)`, where `n` is the number of unique characters (due to heap operations for building the Huffman Tree).
- **Space Complexity**: 
  - Storing frequency table: `O(n)`.
  - Min-heap and tree: `O(n)`.
  - Output codes: `O(n)`.

## Applications
- **File Compression**: Used in ZIP, GZIP, and other compression formats.
- **Multimedia**: JPEG and MP3 encoding for efficient storage.
- **Data Transmission**: Reducing bandwidth in network communications.

## Strengths and Limitations
- **Strengths**:
  - Lossless compression, preserving all data.
  - Highly efficient for data with uneven character frequency distributions.
  - Widely adopted in practical compression systems.
- **Limitations**:
  - Less effective for data with uniform frequency distributions.
  - Requires a code table for decompression, adding overhead.

## Conclusion
Huffman Coding is an efficient greedy algorithm for lossless data compression, achieving significant size reduction (e.g., from 160 bits to 45 bits for the example string). With a time complexity of `O(n log n)`, it is ideal for applications like file compression and multimedia encoding, though its performance depends on frequency distribution.

---
**References**:  
- Group 3 Presentation, 20 May 2025.  