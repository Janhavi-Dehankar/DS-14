# DS-14
DS code
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *left, *right;
};

struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = data;
    newNode->left = newNode->right = NULL;
    return newNode;
}

struct Node* insert(struct Node* root, int data) {
    if (root == NULL) return createNode(data);
    if (data < root->data)
        root->left = insert(root->left, data);
    else if (data > root->data)
        root->right = insert(root->right, data);
    return root;
}

void inorder(struct Node* root) {
    if (root) {
        inorder(root->left);
        printf("%d ", root->data);
        inorder(root->right);
    }
}

void preorder(struct Node* root) {
    if (root) {
        printf("%d ", root->data);
        preorder(root->left);
        preorder(root->right);
    }
}

void postorder(struct Node* root) {
    if (root) {
        postorder(root->left);
        postorder(root->right);
        printf("%d ", root->data);
    }
}

void BFS(struct Node* root) {
    if (root == NULL) return;
    struct Node* queue[100];
    int front = 0, rear = 0;
    queue[rear++] = root;
    while (front < rear) {
        struct Node* temp = queue[front++];
        printf("%d ", temp->data);
        if (temp->left) queue[rear++] = temp->left;
        if (temp->right) queue[rear++] = temp->right;
    }
}

void DFS(struct Node* root) {
    if (root == NULL) return;
    struct Node* stack[100];
    int top = -1;
    stack[++top] = root;
    while (top != -1) {
        struct Node* temp = stack[top--];
        printf("%d ", temp->data);
        if (temp->right) stack[++top] = temp->right;
        if (temp->left) stack[++top] = temp->left;
    }
}

struct Node* mirror(struct Node* root) {
    if (root == NULL) return NULL;
    struct Node* temp = root->left;
    root->left = mirror(root->right);
    root->right = mirror(temp);
    return root;
}

struct Node* find(struct Node* root, int key) {
    if (root == NULL || root->data == key) return root;
    if (key < root->data) return find(root->left, key);
    return find(root->right, key);
}

struct Node* findParent(struct Node* root, int key) {
    if (root == NULL || root->data == key) return NULL;
    if ((root->left && root->left->data == key) || (root->right && root->right->data == key))
        return root;
    if (key < root->data) return findParent(root->left, key);
    else return findParent(root->right, key);
}

void children(struct Node* root, int key) {
    struct Node* node = find(root, key);
    if (!node) printf("Node not found\n");
    else {
        if (node->left) printf("Left Child: %d\n", node->left->data);
        else printf("Left Child: None\n");
        if (node->right) printf("Right Child: %d\n", node->right->data);
        else printf("Right Child: None\n");
    }
}

void sibling(struct Node* root, int key) {
    struct Node* p = findParent(root, key);
    if (!p) printf("No parent, so no sibling\n");
    else if (p->left && p->right) {
        if (p->left->data == key) printf("Sibling: %d\n", p->right->data);
        else printf("Sibling: %d\n", p->left->data);
    } else printf("No sibling\n");
}

void parent(struct Node* root, int key) {
    struct Node* p = findParent(root, key);
    if (p) printf("Parent: %d\n", p->data);
    else printf("No parent\n");
}

void grandParent(struct Node* root, int key) {
    struct Node* p = findParent(root, key);
    if (!p) printf("No parent, so no grandparent\n");
    else {
        struct Node* gp = findParent(root, p->data);
        if (gp) printf("Grandparent: %d\n", gp->data);
        else printf("No grandparent\n");
    }
}

void uncle(struct Node* root, int key) {
    struct Node* p = findParent(root, key);
    if (!p) printf("No parent, so no uncle\n");
    else {
        struct Node* gp = findParent(root, p->data);
        if (!gp) printf("No grandparent, so no uncle\n");
        else if (gp->left && gp->right) {
            if (gp->left->data == p->data) printf("Uncle: %d\n", gp->right->data);
            else printf("Uncle: %d\n", gp->left->data);
        } else printf("No uncle\n");
    }
}

int main() {
    struct Node* root = NULL;
    int choice, val, key;
    while (1) {
        printf("\n1.Insert\n2.Inorder\n3.Preorder\n4.Postorder\n5.BFS\n6.DFS\n7.Mirror\n8.Children\n9.Sibling\n10.Parent\n11.Grandparent\n12.Uncle\n13.Exit\nEnter choice: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1: printf("Enter value: "); scanf("%d", &val); root = insert(root, val); break;
            case 2: inorder(root); printf("\n"); break;
            case 3: preorder(root); printf("\n"); break;
            case 4: postorder(root); printf("\n"); break;
            case 5: BFS(root); printf("\n"); break;
            case 6: DFS(root); printf("\n"); break;
            case 7: root = mirror(root); printf("Mirrored\n"); break;
            case 8: printf("Enter node: "); scanf("%d", &key); children(root, key); break;
            case 9: printf("Enter node: "); scanf("%d", &key); sibling(root, key); break;
