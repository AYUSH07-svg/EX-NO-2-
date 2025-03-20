## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER
## DATE: 20-03-2025
## REG NO: 212224040041
## NAME: AYUSH S

 

## AIM:
 

 

To write a python program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:

```
import numpy as np

def generate_playfair_matrix(key):
    key = key.replace("J", "I").upper()
    key_set = set()
    matrix = []
    
    for char in key:
        if char not in key_set and char.isalpha():
            key_set.add(char)
            matrix.append(char)
    
    for char in "ABCDEFGHIKLMNOPQRSTUVWXYZ":
        if char not in key_set:
            key_set.add(char)
            matrix.append(char)
    
    return np.array(matrix).reshape(5, 5)

def find_position(matrix, letter):
    result = np.where(matrix == letter)
    return result[0][0], result[1][0]

def preprocess_text(text):
    text = text.replace("J", "I").upper()
    text = "".join([c for c in text if c.isalpha()])
    new_text = ""
    i = 0
    while i < len(text):
        new_text += text[i]
        if i + 1 < len(text) and text[i] == text[i + 1]:
            new_text += "X"
        i += 1
    if len(new_text) % 2 != 0:
        new_text += "X"
    return new_text

def encrypt_pair(matrix, a, b):
    r1, c1 = find_position(matrix, a)
    r2, c2 = find_position(matrix, b)
    
    if r1 == r2:
        return matrix[r1][(c1 + 1) % 5] + matrix[r2][(c2 + 1) % 5]
    elif c1 == c2:
        return matrix[(r1 + 1) % 5][c1] + matrix[(r2 + 1) % 5][c2]
    else:
        return matrix[r1][c2] + matrix[r2][c1]

def decrypt_pair(matrix, a, b):
    r1, c1 = find_position(matrix, a)
    r2, c2 = find_position(matrix, b)
    
    if r1 == r2:
        return matrix[r1][(c1 - 1) % 5] + matrix[r2][(c2 - 1) % 5]
    elif c1 == c2:
        return matrix[(r1 - 1) % 5][c1] + matrix[(r2 - 1) % 5][c2]
    else:
        return matrix[r1][c2] + matrix[r2][c1]

def playfair_cipher(text, key, encrypt=True):
    matrix = generate_playfair_matrix(key)
    processed_text = preprocess_text(text)
    result = ""
    
    for i in range(0, len(processed_text), 2):
        if encrypt:
            result += encrypt_pair(matrix, processed_text[i], processed_text[i + 1])
        else:
            result += decrypt_pair(matrix, processed_text[i], processed_text[i + 1])
    
    return result

# Example Usage
key = "PLAYFAIR EXAMPLE"
plaintext = "HIDE THE GOLD IN THE TREE"
ciphertext = playfair_cipher(plaintext, key, encrypt=True)
decrypted_text = playfair_cipher(ciphertext, key, encrypt=False)

print("Ciphertext:", ciphertext)
print("Decrypted Text:", decrypted_text)
```





Output:

<img width="757" alt="Screenshot 2025-03-20 091010" src="https://github.com/user-attachments/assets/3a3969ee-5f10-4bdb-b2aa-60109b14c85a" />
