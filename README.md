# C-and-Datastructure-Narendra
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct node {
    char* name;
    int is_directory;
    struct node* left;
    struct node* right;
} node_t;

node_t* new_node(char* name, int is_directory) {
    node_t* node = (node_t*) malloc(sizeof(node_t));
    node->name = name;
    node->is_directory = is_directory;
    node->left = NULL;
    node->right = NULL;
    return node;
}

node_t* insert(node_t* root, char* name, int is_directory) {
    if (root == NULL) {
        return new_node(name, is_directory);
    }

    int cmp = strcmp(name, root->name);

    if (cmp < 0) {
        root->left = insert(root->left, name, is_directory);
    } else if (cmp > 0) {
        root->right = insert(root->right, name, is_directory);
    } else {
        printf("Error: File or directory already exists.\n");
    }

    return root;
}

node_t* search(node_t* root, char* name) {
    if (root == NULL) {
        return NULL;
    }

    int cmp = strcmp(name, root->name);

    if (cmp < 0) {
        return search(root->left, name);
    } else if (cmp > 0) {
        return search(root->right, name);
    } else {
        return root;
    }
}

node_t* min_value_node(node_t* node) {
    node_t* current = node;

    while (current->left != NULL) {
        current = current->left;
    }

    return current;
}

node_t* delete(node_t* root, char* name) {
    if (root == NULL) {
        return root;
    }

    int cmp = strcmp(name, root->name);

    if (cmp < 0) {
        root->left = delete(root->left, name);
    } else if (cmp > 0) {
        root->right = delete(root->right, name);
    } else {
        if (root->left == NULL) {
            node_t* temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            node_t* temp = root->left;
            free(root);
            return temp;
        }

        node_t* temp = min_value_node(root->right);
        root->name = temp->name;
        root->right = delete(root->right, temp->name);
    }

    return root;
}

void print_helper(node_t* root, int level) {
    if (root == NULL) {
        return;
    }

    print_helper(root->right, level + 1);

    for (int i = 0; i < level; i++) {
        printf("    ");
    }

    printf("%s\n", root->name);

    print_helper(root->left, level + 1);
}

void print(node_t* root) {
    print_helper(root, 0);
}

void pre_order_traversal(node_t* root) {
    if (root != NULL) {
        printf("%s ", root->name);
        pre_order_traversal(root->left);
        pre_order_traversal(root->right);
    }
}

void in_order_traversal(node_t* root) {
    if (root != NULL) {
        in_order_traversal(root->left);
        printf("%s ", root->name);
        in_order_traversal(root->right);
    }
}

void post_order_traversal(node_t* root) {
    if (root != NULL) {
        post_order_traversal(root->left);
        post_order_traversal(root->right);
        printf("%s ", root->name);
    }
}

int main() {
    node_t* root = NULL;

    while (1) {
        int choice;
        char name[100];

        printf("\nEnter your choice:\n");
        printf("Insert a new file or directory.\n");
        printf("Delete an existing file or directory.\n");
        printf("Search for a file or directory by name.\n");
        printf("Display the directory structure in pre-order, in-order, and post-order.\n");
        printf("Print the directory structure in a formatted way.\n");
        printf("Exit.\n");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter the name of the file or directory to insert:\n");
                scanf("%s", name);
                root = insert(root, name, 1);
                break;
            case 2:
                printf("Enter the name of the file or directory to delete:\n");
                scanf("%s", name);
                root = delete(root, name);
                break;
            case 3:
                printf("Enter the name of the file or directory to search for:\n");
                scanf("%s", name);
                node_t* node = search(root, name);
                if (node != NULL) {
                    printf("Found: %s\n", node->name);
                } else {
                    printf("Not found.\n");
                }
                break;
            case 4:
                printf("Pre-order traversal: ");
                pre_order_traversal(root);
                printf("\n");

                printf("In-order traversal: ");
                in_order_traversal(root);
                printf("\n");

                printf("Post-order traversal: ");
                post_order_traversal(root);
                printf("\n");
                break;
            case 5:
                printf("Directory structure:\n");
                print(root);
                break;
            case 6:
                exit(0);
            default:
                printf("Invalid choice.\n");
        }
    }

    return 0;
}
