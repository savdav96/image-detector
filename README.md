# Image Detector, a simple VHDL component
## Aim of the project
This project aims to create a simple VHDL component which gets as input a list of bytes representing an image in a vritual RAM and writes in output the minimun area of the image filtering the bits greater than a specified threshold.
The memory is so composed:
* addresses 0 to 1:                           reserved for further writing.
* address  2:                                 the width of the image
* address  3:                                 the height of the image
* address  4:                                 the threshold with which compare next bits.
* adresses 5 to (4 + witdth) * (4 + height):  the actual bits of the image.

Once the memory gets completely read, the output must find solely the bits which overcome the threshold filter, find the outer edges and calculate the area of that region.

## Proposed algorithm 

As there are many methods to find the outer edges of the restricted image, we wanted to stress performances and try to find the least expensive in term of execution time.

Our choice fell on a variant of the linear search performed outside the edges of the bounds only.
The algorithm is divides into 4 separate parts. Each part performs a search of a specified bound of the image in the following order:

* upper bound
* left bound
* right bound
* lower bound

Starting for the upper bound, our component requires the next adresses of the RAM and goes on until it finds the first occurrency of a bit which is greater than the theshold. That will be set as minimum row, above which no bit satisfy the required condition.
So does the search of other bounds, being careful to make the correct coordinate vary. In fact, while in upper and lower bound search addresses of memory respectively increase and decrease, in left and right bound search the variation must be done upside down, so addresses must be chosen carefully.

Our algorithm has a linear complexity: in best cases (empty matrix and any 1x1 image) T(n) runs like constant, in the worst case (when the only one bit which satisfies condition is located in the middle of the image) T(n) takes O(n-1) steps, considering n the number of bits in RAM.


## Implementation
We used a Finite State Machine to implement this algorithm. 
