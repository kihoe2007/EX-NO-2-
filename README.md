## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

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
~~~
import string
def create_playfair_table(keyword):
    keyword = keyword.upper().replace('J', 'I')
    seen = set()
    table = []
    for ch in keyword + string.ascii_uppercase:
        if ch in seen or ch == 'J':
            continue
        seen.add(ch)
        table.append(ch)
    return [table[i*5:(i+1)*5] for i in range(5)]
def positions(table):
    pos = {}
    for r, row in enumerate(table):
        for c, ch in enumerate(row):
            pos[ch] = (r, c)
    return pos
def prepare_text(text):
    text = text.upper().replace('J','I')
    text = ''.join([c for c in text if c in string.ascii_uppercase])
    res, i = [], 0
    while i < len(text):
        a = text[i]
        b = text[i+1] if i+1 < len(text) else 'X'
        if a == b:
            res.append(a+'X')
            i += 1
        else:
            res.append(a+b)
            i += 2
    return res
def encrypt_pair(pair, table, pos):
    a, b = pair
    ra, ca = pos[a]
    rb, cb = pos[b]
    if ra == rb:
        return table[ra][(ca+1)%5] + table[rb][(cb+1)%5]
    elif ca == cb:
        return table[(ra+1)%5][ca] + table[(rb+1)%5][cb]
    else:
        return table[ra][cb] + table[rb][ca]
def decrypt_pair(pair, table, pos):
    a, b = pair
    ra, ca = pos[a]
    rb, cb = pos[b]
    if ra == rb:
        return table[ra][(ca-1)%5] + table[rb][(cb-1)%5]
    elif ca == cb:
        return table[(ra-1)%5][ca] + table[(rb-1)%5][cb]
    else:
        return table[ra][cb] + table[rb][ca]
def playfair_encrypt(text, keyword):
    table = create_playfair_table(keyword)
    pos = positions(table)
    return ''.join([encrypt_pair(pair, table, pos) for pair in prepare_text(text)])
def playfair_decrypt(cipher, keyword):
    table = create_playfair_table(keyword)
    pos = positions(table)
    pairs = [cipher[i:i+2] for i in range(0, len(cipher), 2)]
    return ''.join([decrypt_pair(pair, table, pos) for pair in pairs])
key_value = "HELLOWORLD" 
plain_text = "Manikandan"
cipher_text = playfair_encrypt(plain_text, key_value)
decrypted_text = playfair_decrypt(cipher_text, key_value)
print("Plain Text:", plain_text)
print("Key Value:", key_value)
print("Encrypted Cipher Text:", cipher_text)
print("Decrypted Cipher Text:", decrypted_text)
~~~
Output:
<img width="1895" height="960" alt="Screenshot 2025-08-29 143049" src="https://github.com/user-attachments/assets/af5d4fb7-589f-4d93-ae1d-6d02b786d7a8" />
<img width="1835" height="466" alt="Screenshot 2025-08-29 143120" src="https://github.com/user-attachments/assets/290d7103-a798-4c54-88a8-3b0ef1025fce" />
