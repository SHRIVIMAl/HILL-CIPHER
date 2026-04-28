## EX NO : 3 
## AIM:
 
To write a C program to implement the hill cipher substitution techniques.

## DESCRIPTION:

Each letter is represented by a number modulo 26. Often the simple scheme A = 0, B
= 1... Z = 25, is used, but this is not an essential feature of the cipher. To encrypt a message, each block of n letters is  multiplied by an invertible n × n matrix, against modulus 26. To
decrypt the message, each block is multiplied by the inverse of the m trix used for
 
encryption. The matrix used
 
for encryption is the cipher key, and it sho
 
ld be chosen
 
randomly from the set of invertible n × n matrices (modulo 26).


## ALGORITHM:

STEP-1: Read the plain text and key from the user. STEP-2: Split the plain text into groups of length three. STEP-3: Arrange the keyword in a 3*3 matrix.
STEP-4: Multiply the two matrices to obtain the cipher text of length three.
STEP-5: Combine all these groups to get the complete cipher text.

## PROGRAM 
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define SIZE 2
int mod26(int x) {
    int r = x % 26;
    return (r < 0) ? r + 26 : r;
}
int determinant(int key[SIZE][SIZE]) {
    return key[0][0]*key[1][1] - key[0][1]*key[1][0];
}
int modInverse(int det) {
    det = mod26(det);
    for (int i = 1; i < 26; i++) {
        if ((det * i) % 26 == 1)
            return i;
    }
    return -1;
}
void inverseKey(int key[SIZE][SIZE], int invKey[SIZE][SIZE]) {
    int det = determinant(key);
    int invDet = modInverse(det);
    if (invDet == -1) {
        printf("Key is not invertible!\n");
        exit(0);
    }
    invKey[0][0] = key[1][1];
    invKey[1][1] = key[0][0];
    invKey[0][1] = -key[0][1];
    invKey[1][0] = -key[1][0];
    for (int i = 0; i < SIZE; i++)
        for (int j = 0; j < SIZE; j++)
            invKey[i][j] = mod26(invKey[i][j] * invDet);
}
void encrypt(char *msg, int key[SIZE][SIZE]) {
    int len = strlen(msg);

    if (len % 2 != 0) {
        msg[len] = 'X';
        msg[len+1] = '\0';
        len++;
    }
    printf("Encrypted text: ");
    for (int i = 0; i < len; i += 2) {
        int a = msg[i] - 'A';
        int b = msg[i+1] - 'A';
        int c1 = mod26(key[0][0]*a + key[0][1]*b);
        int c2 = mod26(key[1][0]*a + key[1][1]*b);
        printf("%c%c", c1 + 'A', c2 + 'A');
    }
    printf("\n");
}
void decrypt(char *msg, int key[SIZE][SIZE]) {
    int invKey[SIZE][SIZE];
    inverseKey(key, invKey);
    int len = strlen(msg);
    printf("Decrypted text: ");
    for (int i = 0; i < len; i += 2) {
        int a = msg[i] - 'A';
        int b = msg[i+1] - 'A';
        int p1 = mod26(invKey[0][0]*a + invKey[0][1]*b);
        int p2 = mod26(invKey[1][0]*a + invKey[1][1]*b);
        printf("%c%c", p1 + 'A', p2 + 'A');
    }
    printf("\n");
}
int main() {
    char msg[100];
    int key[SIZE][SIZE];
    printf("Enter message (UPPERCASE): ");
    scanf("%s", msg);
    printf("Enter 2x2 key matrix:\n");
    for (int i = 0; i < SIZE; i++)
        for (int j = 0; j < SIZE; j++)
            scanf("%d", &key[i][j]);
    encrypt(msg, key);
    decrypt(msg, key);
    return 0;
}
```

## OUTPUT

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/3e114d6d-a081-4859-8828-af2f48c1218e" />



## RESULT
Thus the program is executed successfully.
