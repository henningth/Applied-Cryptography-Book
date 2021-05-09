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
So "T" is shifted to "B". This is because "T" is at position 20 in the alphabet, and adding 8, we get 28. But there are only 
26 letters in the alphabet, so we must subtract 26 from 28, getting the position 2.

This is similar to a clock, where if the time is 10AM and we add 25 hours, we get to 11AM. We would get 
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
    letterMapping = {"A": 0, "B": 1, "C": 2, "D": 3, "E": 4, 
                    "F": 5, "G": 6, "H": 7, "I": 8, "J": 9, 
                    "K": 10, "L": 11, "M": 12, "N": 13, 
                    "O": 14, "P": 15, "Q": 16, "R": 17, 
                    "S": 18, "T": 19, "U": 20, "V": 21, 
                    "W": 22, "X": 23, "Y": 24, "Z": 25}

    # Inverse mapping: maps each position to a letter
    inverseLetterMapping = {0: "A", 1: "B", 2: "C", 3: "D", 4: "E", 
                            5: "F", 6: "G", 7: "H", 8: "I", 9: "J", 
                            10: "K", 11: "L", 12: "M", 13: "N", 
                            14: "O", 15: "P", 16: "Q", 17: "R", 
                            18: "S", 19: "T", 20: "U", 21: "V", 
                            22: "W", 23: "X", 24: "Y", 25: "Z"}

    # The plaintext
    plaintext = "ATTACK"

    # Convert each letter in the plaintext into its 
    # corresponding position
    ciphertext = ""
    for plainletter in plaintext:
        cipherletterPosition = ( letterMapping[plainletter] + key ) % 26
        cipherletter = inverseLetterMapping[cipherletterPosition]
        ciphertext = ciphertext + cipherletter

    # Prints the ciphertext, it should be DWWDFN
    print(ciphertext)

    # Convert each letter in the ciphertext back into the 
    # plaintext letter
    decryptedPlaintext = ""
    for cipherletter in ciphertext:
        plainletterPosition = ( letterMapping[cipherletter] - key ) % 26
        plainletter = inverseLetterMapping[plainletterPosition]
        decryptedPlaintext = decryptedPlaintext + plainletter

    # Prints the decrypted plaintext, should be ATTACK
    print(decryptedPlaintext)

There are two for-loops in the above code. One for *encrypting* the plaintext, 
and another one for *decrypting* the ciphertext. In both of these loops, 
we find the position of each letter, add (or subtract) the key, 
and compute modulo 26, which is the alphabet size.

Note that because of the alphabet size, the number of keys is limited to 26. 
This is a serious weakness of the Caesar Cipher (see also exercise 4).

In the next section, we will study a cipher which has many more possible keys.

Exercises
---------

**Exercise 1**: Starting from the Caesar cipher Python code, try with other keys. 
Also try with *negative* key values. What happens?

**Exercise 2**: Modify the Caesar cipher Python code, so that the encryption and 
decryption is done using *functions*. Also have the user specify the key at the 
start of the program using the builtin ``input`` function in Python.

**Exercise 3**: How would you deal with spaces in the plaintext? Give some 
suggestions, and try your suggestions in Python.

**Exercise 4**: Suppose you are given the ciphertext ``GHIHQG``. How would you 
find the corresponding plaintext *without* knowing the key?
(Later in this chapter we will look into this in more detail.)

Substitution Cipher
===================
In the previous section, we saw that the Caesar Cipher only has 26 possible keys. 
We now look at a cipher which has a *much* larger number of keys, namely the 
*substitution cipher*.

As previously, we use the English alphabet with the 26 uppercase letters. The idea 
is to pair each letter in the alphabet with a letter in the same alphabet. So for 
example, one such pairing could be: 

.. Insert figure here showing the pairing

In the above, we see that "A" is paired with "M", "B" is paired with "K" and so on. 
Such a pairing is also called a *permutation*.

The substitution cipher is defined using a permutation, which is the *key*. Suppose now 
that we want to encrypt the plaintext ``ATTACK`` using the key in figure x. 
The ciphertext then becomes ``MXXMSK``. To decrypt the ciphertext, we simply 
use the inverse permutation, which in figure x corresponds to reversing the 
direction of the arrows. Using this, we can recover the original 
plaintext ``ATTACK``.

How many possible keys are there for the substitution cipher. This depends on the 
alphabet size. Assuming the English alphabet with 26 letters, for the first letter "A" there 
are 26 possible pairings (including "A" itself). Then for the next letter "B", there are **25** 
possible pairings (because one letter has already been paired). Similarly, for "C" there 
are 24 possible pairings and so on. This means that there are:

.. math ::
    26 \cdot 25 \cdot 24 \cdots 2 \cdot 1 = 26!

which equals 403.291.461.126.605.635.584.000.000.

Exclusive OR (XOR)
==================
Another way

Further Reading
===============
The book

Solutions to Exercises
======================

.. Possible to use show/hide for this part?

**Solution 1**: When using negative key values, 