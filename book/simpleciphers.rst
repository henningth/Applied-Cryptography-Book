.. Examples of simple ciphers, Caesar cipher, substitution cipher etc.

**************
Simple Ciphers
**************

Let us begin with some examples of simple ciphers, show how these are implemented in Python, and how they can be broken. 
Another goal of this chapter is to introduce the necessary *terminology* which is used in the rest of the text.

Caesar Cipher
=============
Probably one of the first examples of protecting information was the *Caesar cipher*, 
which was used in the ancient Roman empire for military communications. A *cipher* is a method for mapping 
human-readable text into text which can't be read by humans. (We will give a more precise definition soon.)

In this cipher, we have an *alphabet* of letters, 
for simplicity we consider in this example the English alphabet:

.. math::
    \{A, B, C, \ldots, Z\}.

This is not the only possible alphabet, as we will see later in the book.

The main idea of the Caesar Cipher is the following. Suppose that when we want to encrypt the message ``ATTACK``. 
Then we shift each character three places forward in the alphabet. So the "A" becomes "D", "T" becomes "W" and so on. 
We then end up with the encrypted text ``DWWDFN``. The text we started with is called the *plaintext* (or *cleartext* in some 
books), while the encrypted text is called *ciphertext*.

To decrypt the ciphertext, we need to do the inverse operation. In cryptography, it is very important that ciphers are 
*invertible*, so that we can recover the plaintext from the ciphertext.

For the Caesar Cipher, that means that we shift each character in the ciphertext three places backwards in the alphabet.

The number three is used for both encrypting and decrypting, and this is called the *key*. So in our example, we use 
:math:`3` for encrypting and :math:`-3` for decrypting.

The key is not limited to be three, it can be any integer. For example, the key could be :math:`8`. Using this key on the plaintext 
``ATTACK``, then "A" is shifted to "I", but when we shift "T", we reach the end of the alphabet, and therefore start at the beginning. 
So "T" is shifted to "B". This is similar to a clock, where if the time is 10AM and we add 25 hours, we get to 11AM. We would get 
the same result if we added 1 hour, so the rule in this case is ``25 = 1``. In a similar way, ``26 = 2``. So in the clock example, 
if we get 24 or above, we subtract 24. This is called computation *modulo* 24. We write the addition as follows:

.. math::
    a + b \mod 24,

where :math:`a` and :math:`b` are integers. Taking :math:`a=10` and :math:`b=25` (as in the first example), the computation is:

.. math::
    \begin{align}
    10 + 25 \mod 24 &= 35 \mod 24 \\
    &= 11 \mod 24.
    \end{align}

.. Add a figure showing the clock and its relationship to the modulo operation.

Caesar cipher in Python
-----------------------
Let us now introduce to modulo operation and the Caesar cipher using Python 3, starting with the modulo operation. 

In Python, modulo is computed using the % (percent) operator. For example, computing :math:`5 \mod 3` and printing the result in the 
console, we would do::

    result = 5 % 3
    print(result)

To implement the Caesar cipher in Python, let us first make a list of the requirements for the cipher:

* The cipher uses a fixed key, which is 3.
* The cipher can encrypt and decrypt text from the standard English alphabet

In this first version, we will use an imperative approach. In the exercises, some generalization will be proposed::

    # Uses fixed key
    key = 3

    # Mapping each letter to a position in the alphabet
    letterMapping = {"A": 0, "B": 1, "C": 2, "D": 3, "E": 4, "F": 5, "G": 6, 
                        "H": 7, "I": 8, "J": 9, "K": 10, "L": 11, "M": 12, "N": 13,
                        "O": 14, "P": 15, "Q": 16, "R": 17, "S": 18, "T": 19, "U": 20,
                        "V": 21, "W": 22, "X": 23, "Y": 24, "Z": 25}

    # Inverse mapping: maps each position to a letter
    inverseLetterMapping = {0: "A", 1: "B", 2: "C", 3: "D", 4: "E", 5: "F", 6: "G", 
                        7: "H", 8: "I", 9: "J", 10: "K", 11: "L", 12: "M", 13: "N",
                        14: "O", 15: "P", 16: "Q", 17: "R", 18: "S", 19: "T", 20: "U",
                        21: "V", 22: "W", 23: "X", 24: "Y", 25: "Z"}

    # The plaintext
    plaintext = "ATTACK"

    # Convert each letter in the plaintext into its corresponding position
    ciphertext = ""
    for plainletter in plaintext:
        cipherletterPosition = ( letterMapping[plainletter] + key ) % 24
        cipherletter = inverseLetterMapping[cipherletterPosition]
        ciphertext = ciphertext + cipherletter

    # Prints the ciphertext, it should be DWWDFN
    print(ciphertext)

    # Convert each letter in the ciphertext back into the plaintext letter
    decryptedPlaintext = ""
    for cipherletter in ciphertext:
        plainletterPosition = ( letterMapping[cipherletter] - key ) % 24
        plainletter = inverseLetterMapping[plainletterPosition]
        decryptedPlaintext = decryptedPlaintext + plainletter

    # Prints the decrypted plaintext, should be ATTACK
    print(decryptedPlaintext)

Substitution Cipher
===================
The substitution cipher...

Exclusive OR (XOR)
==================
Another way

Exercises
=========
Exercise 1: 

Further Reading
===============
The book