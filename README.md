# arvore_binaria

class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.height = 1

class AVLTree:
    def __init__(self):
        self.root = None

    def height(self, node):
        if node is None:
            return 0
        return node.height

    def balance_factor(self, node):
        if node is None:
            return 0
        return self.height(node.left) - self.height(node.right)

    def update_height(self, node):
        if node is None:
            return
        node.height = 1 + max(self.height(node.left), self.height(node.right))

    def right_rotate(self, y):
        x = y.left
        T2 = x.right

        x.right = y
        y.left = T2

        self.update_height(y)
        self.update_height(x)

        return x

    def left_rotate(self, x):
        y = x.right
        T2 = y.left

        y.left = x
        x.right = T2

        self.update_height(x)
        self.update_height(y)

        return y

    def insert(self, root, key):
        if root is None:
            return Node(key)

        if key < root.key:
            root.left = self.insert(root.left, key)
        else:
            root.right = self.insert(root.right, key)

        self.update_height(root)

        balance = self.balance_factor(root)

        
        if balance > 1:
            if key < root.left.key:
                return self.right_rotate(root)
            else:
                root.left = self.left_rotate(root.left)
                return self.right_rotate(root)

        
        if balance < -1:
            if key > root.right.key:
                return self.left_rotate(root)
            else:
                root.right = self.right_rotate(root.right)
                return self.left_rotate(root)

        return root

    def delete(self, root, key):
        if root is None:
            return root

        if key < root.key:
            root.left = self.delete(root.left, key)
        elif key > root.key:
            root.right = self.delete(root.right, key)
        else:
            if root.left is None:
                return root.right
            elif root.right is None:
                return root.left

            min_val = self.find_min_value(root.right)
            root.key = min_val
            root.right = self.delete(root.right, min_val)

        self.update_height(root)

        balance = self.balance_factor(root)

        
        if balance > 1:
            if self.balance_factor(root.left) >= 0:
                return self.right_rotate(root)
            else:
                root.left = self.left_rotate(root.left)
                return self.right_rotate(root)

        
        if balance < -1:
            if self.balance_factor(root.right) <= 0:
                return self.left_rotate(root)
            else:
                root.right = self.right_rotate(root.right)
                return self.left_rotate(root)

        return root

    def find_min_value(self, root):
        while root.left is not None:
            root = root.left
        return root.key

    def search(self, root, key):
        if root is None or root.key == key:
            return root

        if key < root.key:
            return self.search(root.left, key)

        return self.search(root.right, key)

    def insert_key(self, key):
        self.root = self.insert(self.root, key)

    def delete_key(self, key):
        self.root = self.delete(self.root, key)

    def search_key(self, key):
        return self.search(self.root, key)

    def inorder_traversal(self, root):
        result = []
        if root:
            result = self.inorder_traversal(root.left)
            result.append(root.key)
            result = result + self.inorder_traversal(root.right)
        return result

    def display(self):
        result = self.inorder_traversal(self.root)
        print(result)


if __name__ == "__main__":
    tree = AVLTree()
    tree.insert_key(40)
    tree.insert_key(70)
    tree.insert_key(30)
    tree.insert_key(50)
    tree.insert_key(60)

    print("Árvore AVL após a inserção:")
    tree.display()

    tree.delete_key(30)
    print("Árvore AVL após a exclusão do nó 30:")
    tree.display()

    tree.delete_key(40)
    print("Árvore AVL após a exclusão do nó 40:")
    tree.display()

    tree.delete_key(50)
    print("Árvore AVL após a exclusão do nó 50:")
    tree.display()
