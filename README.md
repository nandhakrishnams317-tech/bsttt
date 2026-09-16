#include <stdio.h>
#include <stdlib.h>

// Structure for a BST node
struct Node {
    int data;
    struct Node *left;
    struct Node *right;
};

// Create a new node
struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));

    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;

    return newNode;
}

// Insert a node
struct Node* insert(struct Node* root, int data) {
    if (root == NULL) {
        return createNode(data);
    }

    if (data < root->data) {
        root->left = insert(root->left, data);
    }
    else if (data > root->data) {
        root->right = insert(root->right, data);
    }

    return root;
}

// Find the smallest node
struct Node* findMin(struct Node* root) {
    while (root->left != NULL) {
        root = root->left;
    }

    return root;
}

// Delete a node
struct Node* deleteNode(struct Node* root, int data) {

    // Node not found
    if (root == NULL) {
        return root;
    }

    // Search in left subtree
    if (data < root->data) {
        root->left = deleteNode(root->left, data);
    }

    // Search in right subtree
    else if (data > root->data) {
        root->right = deleteNode(root->right, data);
    }

    // Node found
    else {

        // Case 1: No child
        if (root->left == NULL && root->right == NULL) {
            free(root);
            return NULL;
        }

        // Case 2: Only right child
        else if (root->left == NULL) {
            struct Node* temp = root->right;
            free(root);
            return temp;
        }

        // Case 3: Only left child
        else if (root->right == NULL) {
            struct Node* temp = root->left;
            free(root);
            return temp;
        }

        // Case 4: Two children
        else {
            struct Node* temp = findMin(root->right);

            root->data = temp->data;

            root->right = deleteNode(root->right, temp->data);
        }
    }

    return root;
}

// Inorder traversal
void inorder(struct Node* root) {
    if (root != NULL) {
        inorder(root->left);
        printf("%d ", root->data);
        inorder(root->right);
    }
}

// Main function
int main() {

    struct Node* root = NULL;
    int n, value, deleteValue;

    printf("Enter number of nodes: ");
    scanf("%d", &n);

    printf("Enter the values:\n");

    for (int i = 0; i < n; i++) {
        scanf("%d", &value);
        root = insert(root, value);
    }

    printf("\nBST Inorder before deletion: ");
    inorder(root);

    printf("\n\nEnter value to delete: ");
    scanf("%d", &deleteValue);

    root = deleteNode(root, deleteValue);

    printf("\nBST Inorder after deletion: ");
    inorder(root);

    return 0;
}