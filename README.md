# CLASSICAL CRYPTOGRAPHY

## C++

### 1. Caesar Cipher

```cpp
#include <iostream>
using namespace std;

int main()
{
    string text;
    int shift;

    cout << "Enter text: ";
    cin >> text;

    cout << "Enter shift: ";
    cin >> shift;

    string result = "";

    for (char c : text)
    {
        if (c >= 'A' && c <= 'Z')
            result += (c - 'A' + shift) % 26 + 'A';
        else if (c >= 'a' && c <= 'z')
            result += (c - 'a' + shift) % 26 + 'a';
        else
            result += c;
    }

    cout << "Encrypted Text: " << result;

    return 0;
}
```

### 2. Rail Fence Cipher

```cpp
#include <iostream>
using namespace std;

int main()
{
    string text;
    int rails;

    cout << "Enter text: ";
    cin >> text;

    cout << "Enter number of rails: ";
    cin >> rails;

    string fence[100];

    int row = 0;
    int direction = 1;

    for (char c : text)
    {
        fence[row] += c;

        if (row == 0)
            direction = 1;
        else if (row == rails - 1)
            direction = -1;

        row += direction;
    }

    string result = "";

    for (int i = 0; i < rails; i++)
        result += fence[i];

    cout << "Encrypted Text: " << result;

    return 0;
}
```

### 3. Vigenere Cipher

```cpp
#include <iostream>
using namespace std;

int main()
{
    string text, key;

    cout << "Enter text: ";
    cin >> text;

    cout << "Enter key: ";
    cin >> key;

    string result = "";

    for (int i = 0; i < text.length(); i++)
    {
        char c = text[i];
        int shift = key[i % key.length()] - 'A';

        if (c >= 'A' && c <= 'Z')
            result += (c - 'A' + shift) % 26 + 'A';
        else
            result += (c - 'a' + shift) % 26 + 'a';
    }

    cout << "Encrypted Text: " << result;

    return 0;
}
```

### 4. Feistel Block Cipher

```cpp
#include <iostream>
using namespace std;

int roundFunction(int right, int key)
{
    return ((right << 6) | (right >> 2)) & 0xFF;
}

void feistelRound(int &left, int &right, int key)
{
    int temp = right;
    right = left ^ roundFunction(right, key);
    left = temp;
}

int main()
{
    int left = 0x5A;
    int right = 0xA5;
    int roundKey = 0x1F;

    feistelRound(left, right, roundKey);

    cout << "Staged Block Sides:" << endl;
    cout << "Left = " << hex << left << endl;
    cout << "Right = " << hex << right << endl;

    return 0;
}
```

## Python

### 1. Caesar Cipher

```python
def encrypt_caesar(text, shift):
    result = ""

    for c in text:
        if c.isupper():
            result += chr((ord(c) - ord('A') + shift) % 26 + ord('A'))
        elif c.islower():
            result += chr((ord(c) - ord('a') + shift) % 26 + ord('a'))
        else:
            result += c

    return result


text = "DataSecurity2026"
shift = 4

print("Encrypted Text:", encrypt_caesar(text, shift))
```

### 2. Rail Fence Cipher

```python
def encrypt_rail_fence(text, rails):
    fence = [""] * rails
    row = 0
    direction = 1

    for c in text:
        fence[row] += c

        if row == 0:
            direction = 1
        elif row == rails - 1:
            direction = -1

        row += direction

    result = ""

    for r in fence:
        result += r

    return result


text = input("Enter text: ")
rails = int(input("Enter number of rails: "))

print("Encrypted Text:", encrypt_rail_fence(text, rails))
```

### 3. Vigenere Cipher

```python
def encrypt_vigenere(text, key):
    result = ""
    key = key.upper()
    key_len = len(key)
    key_index = 0

    for c in text:
        if c.isalpha():
            shift = ord(key[key_index % key_len]) - ord('A')

            if c.isupper():
                result += chr((ord(c) - ord('A') + shift) % 26 + ord('A'))
            else:
                result += chr((ord(c) - ord('a') + shift) % 26 + ord('a'))

            key_index += 1
        else:
            result += c

    return result


text = "HELLO"
key = "KEY"

print("Encrypted Vigenere Text:", encrypt_vigenere(text, key))
```

### 4. Feistel Block Cipher

```python
def round_function(block, key):
    return ((block << 6) | (block >> 2)) & 0xFF


def feistel_round(left, right, key):
    temp = right
    right = left ^ round_function(right, key)
    left = temp

    return left, right


left = 0x5A
right = 0xA5
round_key = 0x1F

left, right = feistel_round(left, right, round_key)

print("Feistel Block after one round:")
print("Left =", hex(left))
print("Right =", hex(right))
```
