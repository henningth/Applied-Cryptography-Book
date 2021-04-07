.. Examples of simple ciphers, Caesar cipher, substitution cipher etc.

**************
Simple Ciphers
**************

Let us begin with some examples of simple ciphers, show how these are implemented in Python, and how they can be broken. 
Another goal of this chapter is to introduce the necessary *terminology* which is used in the rest of the text.

Caesar Cipher
=============
Probably one of the first examples of protecting information was the *Caesar cipher*, 
which was used in the ancient Roman empire for military communications. In this cipher, we have an *alphabet* of letters, 
for simplicity we consider in this example the English alphabet:

.. math::
    \{A, B, C, \ldots, Z\}.

This is not the only possible alphabet, as we will see later in the book.

The main idea of the Caesar Cipher is the following. Suppose that when we want to encrypt the message ``ATTACK``. 
Then we shift each character three places forward in the alphabet. So the "A" becomes "D", "T" becomes "W" and so on. 
We then end up with the encrypted text ``DWWDFN``. The text we started with is called the *plaintext* (or *cleartext* in some 
books), while the encrypted text is called *ciphertext*.

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