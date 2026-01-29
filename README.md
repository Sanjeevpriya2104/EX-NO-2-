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
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char keyM[5][5];

void makeMatrix(char key[]) {
    int used[26] = {0}, i, j, k = 0;
    used['J'-'A'] = 1;

    for(i = 0; key[i]; i++) {
        if(isalpha(key[i]) && !used[key[i]-'A']) {
            keyM[k/5][k%5] = key[i];
            used[key[i]-'A'] = 1;
            k++;
        }
    }

    for(i = 0; i < 26; i++) {
        if(!used[i]) {
            keyM[k/5][k%5] = i + 'A';
            k++;
        }
    }
}

void findPos(char ch, int *r, int *c) {
    int i, j;
    if(ch == 'J') ch = 'I';
    for(i = 0; i < 5; i++)
        for(j = 0; j < 5; j++)
            if(keyM[i][j] == ch) {
                *r = i; *c = j;
            }
}

int main() {
    char key[50], text[50];
    int i, r1,c1,r2,c2;

    printf("Enter key: ");
    fgets(key, sizeof(key), stdin);
    printf("Enter plaintext: ");
    fgets(text, sizeof(text), stdin);

    for(i = 0; key[i]; i++)
        if(isalpha(key[i])) key[i] = toupper(key[i]);

    for(i = 0; text[i]; i++)
        if(isalpha(text[i])) text[i] = toupper(text[i]);

    makeMatrix(key);

    printf("\n5x5 Key Matrix:\n");
    for(i = 0; i < 5; i++) {
        for(int j = 0; j < 5; j++) printf("%c ", keyM[i][j]);
        printf("\n");
    }

    printf("\nCiphertext: ");
    for(i = 0; text[i]; i += 2) {
        if(!isalpha(text[i])) continue;

        if(text[i+1] == '\0' || !isalpha(text[i+1]))
            text[i+1] = 'X';

        findPos(text[i], &r1, &c1);
        findPos(text[i+1], &r2, &c2);

        if(r1 == r2)
            printf("%c%c", keyM[r1][(c1+1)%5], keyM[r2][(c2+1)%5]);
        else if(c1 == c2)
            printf("%c%c", keyM[(r1+1)%5][c1], keyM[(r2+1)%5][c2]);
        else
            printf("%c%c", keyM[r1][c2], keyM[r2][c1]);
    }

    return 0;
}
```




Output:
<img width="1415" height="773" alt="image" src="https://github.com/user-attachments/assets/0bdade57-ef7a-4b48-85cd-8734ad85e6cb" />

RESULT:

the playfair cipher program executed successfully

