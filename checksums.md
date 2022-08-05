## Checksums


> What are checksums and what are they useful for?

Checksums are a way to validate that a set of bytes is what they are intended to be. One usage is to when sending data over a communication protocol, you can put a checksum in the data being transmitted, to ensure that it came across correctly. If you have a firmware image, you might want to ensure that the firmware image has not been corrupted, so you can add a checksum to the image.

There are various forms of checksums. The simplest one is simply the "sum" of the values of all of the bytes (hence the name). But if extra bytes with a value of 0 were added to the set of bytes, this simple checksum won't catch that. Similarly for swapped bytes. At one company, they had 2-byte checksum, where they had the upper-order bytes were summed into the upper-order byte of the checksum, and the lower-order bytes were summed into the lower-order byte of the checksum. That resolves some issues.

A CRC is a better solution, as it better handles any sort of deviation in the values, so it is more likely to give a different result if something in the data is messed up. For our communication protocol, we use a 1-byte CRC for the header of the packet and a 2-byte CRC for the payload of the packet. For our firmware images, we use 32-bit CRCs. There are libraries that will generate C code for CRC generation of various sizes, using different parameters. The output code is relatively small, and (depending on the input parameters chosen) can be used in conjunction with Python (or other) libraries that implement the same CRC checking.

Overall, the process is that the originator of the set of bytes will run the set of bytes through the checksum/CRC algorithm to generate the checksum/CRC, and then somehow add that checksum/CRC to the set of bytes. Then when those set of bytes are checked (such as by code receiving a packet or analyzing a firmware image for integrity), the same bytes are pass through the same algorithm. If the checksum/CRC generated on the receiver side matches the checksum/CRC that is associated (such as appended to) the set of bytes, then the bytes are correct.