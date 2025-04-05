# Morris Inorder Traversal in C++

This repository contains an implementation of the Morris Inorder Traversal algorithm for binary trees in C++. The Morris Traversal method performs an inorder traversal without using recursion or an explicit stack, achieving an in-place traversal with O(1) extra space (excluding the output).

## Overview

The Morris Inorder Traversal algorithm leverages temporary "threads" to connect nodes during traversal. The core ideas of the algorithm are as follows:

- **If the left child does not exist:**  
  Process the current node (for example, by storing its value) and move to its right child.

- **If the left child exists:**  
  Find the inorder predecessor (i.e., the rightmost node in the left subtree):
  - **If the predecessor's right pointer is `nullptr`:**  
    Create a temporary thread by setting the predecessor's right pointer to the current node, then move to the left child.
  - **If the predecessor's right pointer already points to the current node:**  
    This indicates that the left subtree has been fully traversed. Remove the temporary thread, process the current node, and move to the right child.

## Code Example

The repository includes an implementation in C++ as shown below:

```cpp
#include <vector>
using namespace std;

struct Node {
    int data;
    Node *left, *right;
    
    Node(int value) : data(value), left(nullptr), right(nullptr) {}
};

vector<int> inOrder(Node* root) {
    vector<int> inorder;
    Node* curr = root;
    
    while (curr != nullptr) {
        if (curr->left == nullptr) {
            // If there is no left child, process the current node and move to the right
            inorder.push_back(curr->data);
            curr = curr->right;
        } else {
            // Find the inorder predecessor of curr in the left subtree
            Node* pre = curr->left;
            while (pre->right != nullptr && pre->right != curr) {
                pre = pre->right;
            }
            
            if (pre->right == nullptr) {
                // Create a temporary thread from predecessor to current node
                pre->right = curr;
                curr = curr->left;
            } else {
                // Remove the temporary thread, process the current node, and move to the right
                pre->right = nullptr;
                inorder.push_back(curr->data);
                curr = curr->right;
            }
        }
    }
    
    return inorder;
}
```
## Time Complexity
The Morris Inorder Traversal algorithm runs in O(n) time, where n is the number of nodes in the tree. Although the inner loop appears to traverse nodes multiple times, each edge is visited at most twice (once for creating a thread and once for removing it), ensuring linear time complexity.

## Space Complexity
The algorithm uses O(1) extra space (excluding the output vector), as it does not utilize additional data structures like a stack or recursion.
