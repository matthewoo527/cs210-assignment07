# CS210 Assignment 7

## Code
```cpp
/**
 * AI Disclosure:
 * I used ChatGPT to help me build the BinarySearchTree class, and I go through the code to understand it.
 * I also added comments to explain the code and how it works.
 */

#include <iostream>
using namespace std;

struct Node {
    int value;
    Node* left;
    Node* right;

    //Constructor
    Node(int x) {
        value = x;
        left = nullptr;
        right = nullptr;
    }
};

class BinarySearchTree {
private:
    //Empty Tree
    Node* root = nullptr;
    //Insert function for inserting nodes into the tree
    Node* insert(Node* node, int x) {
        //If no node, then create one
        if (node == nullptr)
            return new Node(x);
        //If the number is smaller, then insert at left
        if (x < node->value)
            node->left = insert(node->left, x);
        //If the number is bigger, then insert at right
        else if (x > node->value)
            node->right = insert(node->right, x);
        return node;
    }

    bool search(Node* node, int x) {
        //Return false if the node is nullptr
        if (node == nullptr) return false;
        //Return true if it is the node that we want to find
        if (node->value == x) return true;
        //it uses recursion to return true or false if it found it or not
        //If it is smaller then the node then search left
        if (x < node->value)
            return search(node->left, x);
        //Or search right and return true or false if it is larger then the node
        return search(node->right, x);
    }

    Node* remove(Node* node, int x) {
        //If the node is nullptr then return null ptr
        if (node == nullptr) return nullptr;
        //If the number we trying to remove is smaller then the node,
        //then remove left child
        if (x < node->value) {
            node->left = remove(node->left, x);
        }
        //else if it is bigger, then remove right child
        else if (x > node->value) {
            node->right = remove(node->right, x);
        }
        else {
            // No left child (also handles leaf)
            if (node->left == nullptr) {
                Node* next = node->right;
                delete node;
                return next;
            }

            // No right child
            if (node->right == nullptr) {
                Node* next = node->left;
                delete node;
                return next;
            }

            // Two children
            Node* next = node->right;
            while (next->left != nullptr)
                next = next->left;
            node->value = next->value;
            node->right = remove(node->right, next->value);
        }
        return node;
    }

    void inorder(Node* node) {
        if (node == nullptr) return;
        //Left
        inorder(node->left);
        //Root
        cout << node->value << " ";
        //Right
        inorder(node->right);
    }

    void preorder(Node* node) {
        //Root->Left->Right
        if (node == nullptr) return;
        //Print Root
        cout << node->value << " ";
        //Go Left and print
        preorder(node->left);
        //Right
        preorder(node->right);
    }

    void postorder(Node* node) {
        //Left->Right->Root
        if (node == nullptr) return;
        //Left
        postorder(node->left);
        //Right
        postorder(node->right);
        //Root
        cout << node->value << " ";
    }

    int height(Node* node) {
        //Return height as 0 if the tree is empty
        if (node == nullptr) return 0;
        int left = height(node->left);
        int right = height(node->right);
        if (left > right)
            return left + 1;
        else
            return right + 1;
    }
    //Clear function to clear the tree
    void clear(Node* node) {
        if (node == nullptr) return;
        clear(node->left);
        clear(node->right);
        delete node;
    }

public:
    //destructor
    ~BinarySearchTree() {
        clear(root);
    }

    void insert(int x) { root = insert(root, x); }
    bool search(int x) { return search(root, x); }
    void remove(int x) { root = remove(root, x); }

    void inorder() { inorder(root); cout << endl; }
    void preorder() { preorder(root); cout << endl; }
    void postorder() { postorder(root); cout << endl; }

    int height() { return height(root); }
};

int main() {
    //BST object called tree
    BinarySearchTree tree;

    int numbers[] = {70, 40, 98, 20, 50, 80, 100, 10, 30, 60, 90, 102};

    for (int x : numbers)
        tree.insert(x);

    cout << "Inorder: ";
    tree.inorder();

    cout << "Preorder: ";
    tree.preorder();

    cout << "Postorder: ";
    tree.postorder();

    cout << "Height: " << tree.height() << endl;

    // Search for 40
    if (tree.search(60) == true) {
        cout << "Search 60: Found" << endl;
    }
    else {
        cout << "Search 60: Not found" << endl;
    }

    // Search for 99
    if (tree.search(99) == true) {
        cout << "Search 99: Found" << endl;
    }
    else {
        cout << "Search 99: Not found" << endl;
    }

    tree.remove(10); // Leaf
    cout << "After removing 10: ";
    tree.inorder();

    tree.remove(50); // One child
    cout << "After removing 50: ";
    tree.inorder();

    tree.remove(40); // Two children
    cout << "After removing 40: ";
    tree.inorder();

    return 0;
}
```
## Output
```
Inorder: 10 20 30 40 50 60 70 80 90 98 100 102 
Preorder: 70 40 20 10 30 50 60 98 80 90 100 102 
Postorder: 10 30 20 60 50 40 90 80 102 100 98 70 
Height: 4
Search 60: Found
Search 99: Not found
After removing 10: 20 30 40 50 60 70 80 90 98 100 102 
After removing 50: 20 30 40 60 70 80 90 98 100 102 
After removing 40: 20 30 60 70 80 90 98 100 102
```
Output Screenshot:

<img width="398" height="131" alt="Screenshot 2026-11-15 at 18 18 19" src="https://github.com/user-attachments/assets/883fe39b-cf38-4242-9351-32bd4b3837d4" />

## Complexity
### Search
The average case complexity for search is O(log n), because it searches the left or the right subtree until the number is found.

The worst case complexity for search is O(n), because it might need to search every node to find a number.

### insert
The average case complexity for insert is O(log n), because it needs to find an empty position by following the left or right subtree, then insert the node.

The worst case complexity is O(n), because it might need to go to every node to find an empty position.

### remove
The average case complexity for remove is O(log n) because when it find the node it just remove it or replace with another node if needed.

The worst case complexity for remove is O(n) because it might need to search through the whole tree to remove a node.
