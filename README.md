# File compression system using Huffman Algorithm
A compression system that uses the Huffman algorithm to compress text(.txt) files into binary(.bin) files reducing the filesize by ~50%. 
The binary(.bin) files can then be decompressed back into the original text files losslessly.
## File Structure
```
.
├── .idea                 
├── cmake-build-debug     
├── Compressed            # generated compressed files
├── Decompressed          # decompressed output files
├── HeaderFiles           # header files
│   ├── reader.h
│   ├── encoder.h
│   └── decoder.h
└── src                   # source code
    ├── main.cpp
    ├── reader.cpp
    ├── encoder.cpp
    └── decoder.cpp
```
### Architecture:
#### Reader:
```
class Reader{
  public:
  std::string readFile(const std::string& filename);
  std::unordered_map<char,int> freqMap(const std::string &data);
};
```
Reads the file in binary stream and creates a frequency map for all the characters.  
#### Encoder:
```
class HuffmanNode{
  public:
    char ch;
    int freq;
    char minChar;
    HuffmanNode* left;
    HuffmanNode* right;
    HuffmanNode(char ch,int freq);//Single Node Constructor
    HuffmanNode(int freq,HuffmanNode* left,HuffmanNode* right);//Merged Node constructor from 2 lowest frequency nodes;
};
HuffmanNode* buildHuffmanTree(const std::unordered_map<char,int>& freqMap);
void createCode(HuffmanNode* root,std::string,std::unordered_map<char,std::string>&codes);
struct comp{
  bool operator()(HuffmanNode*a , HuffmanNode* b);
};
int convertToBinary(std::vector<unsigned char> &bytes,std::string compressedBits);
```
Creates a huffman tree using the frequency map . Then the tree is used to assign codes to characters in the original string. Necessary padding is added if required. Then the file is written as such:
1. Size of the FreqMap
2. The actual characters and their frequency i.e The FreqMap.
3. The number of padding bits
4. The codes of characters .
#### Decoder:
```
std::string convertToText(std::unordered_map<char,int> &freqMap,std::string &originalFile);
void saveToFile(const std::string &data, const std::string &filename);
```
The binary file is read back and the huffman tree is constructed again using the FreqMap. Then for each byte, the corresponding node is reached in the tree and the character is noted. The output is stored in a text file.

### Testing:
![Reduction in file size](https://github.com/HalonixTheFirst/FileCompressionSystem/blob/master/ReductionInFileSize.png)

### Conclusion:
This project demonstrates the use of Data Structures and Algorithms in practice allowing for efficient storage of data highly reducing file size.
___

### References:
[Huffman Coding Algorithm - Greedy Algo 3](https://www.geeksforgeeks.org/dsa/huffman-coding-greedy-algo-3/)  
[A tutorial on using the Huffman Coding Algorithm](https://www.youtube.com/watch?v=IuFKDrdmgIQ)
