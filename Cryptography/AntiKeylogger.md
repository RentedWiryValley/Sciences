## How to enter sensitive information on a PC with a keylogger or key-logging keyboard

#### Preparations

- Create a text document
- Enter all typeable characters

It is crucial that all typeable characters, not just the characters that exist in the sensitive information, are in the document. It is crucial that the arrangement of the characters does not correspond to the sensitive information in any way.

#### Text entering

For every character in the sensitive information, perform the following steps:

- Select the character in the prepared document with the mouse
- Drag and drop the selection onto the text entering area of the program in which the sensitive information is to be entered

It is crucial that the selection of the character is performed with the mouse, not the keyboard.

#### Alternatives

Keyboard shortcuts of copy/paste can be used instead of mouse drag and drop. However, it is secure only when there is no concern about clipboard logging malware.

#### Optimizations

Arrange the characters in the document so that the width and the height of the block are roughly equal. It can reduce the overall distance of the mouse movement while one button is held down, which can be clumsy.

#### Mouse-logging

Mouse-loggers do not seem popular. It is probably because the data is hard to interpret. Theoretically speaking, they can exist. High-profile targets may want to consider the possibility that their PCs are infected with mouse-loggers, or their mice are built with logging.

If there are both key-logging and mouse-logging, an additional step must be added for the approach to be secure.

For every character in the sensitive information, perform the following steps:

- Securely shuffle the characters in the prepared document
- Select the character in the prepared document with the mouse
- Drag and drop the selection onto the text entering area of the program in which the sensitive information is to be entered

Linux command `shuf` can shuffle the document. A secure random source should be specified. However, since it can only shuffle lines, every line in the document must contain only one character. It makes the drag and drop very inconvenient. It would be ideal if dedicated software is developed.
