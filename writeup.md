# 45 Exile Shuffle Crackme Writeup
## Tyler Bosford

### Overview:
**Difficulty** Rating: 2.3

**Objective**: Enter a password that, when hashed using a Fisher-Yates Shuffle, produces the correct hash result

**Code when ran:**

![Screenshot of code output](crackme_start.png)

### Initial Approach:
I approach most crackmes with the same strategy. First, I dissassemble the binary with **objdump -d** and pull all of the strings from the code utilizing **strings [crackme]**. I then check the strings file to see if there are any easy wins available. If not, I move to the assembly and begin to dissect what the code is doing. 

For this particular assignment I was able to identify from the assembly that there were only two functions of interest. The first was the main function which housed the logic for user input. The second was a hashing function, I will call hash_alg, that was called from main. 

### Ghidra
Once I gained as much knowledge as I could from the assembly I moved the code into Ghidra for further analysis. Below is a screenshot of the items I identified within Ghidra and the names I gave them in an attempt to understand the source code:

![Screenshot of ghidra main](ghidra_main.png)
**How it works**: The main function takes in user input and stores it as a c-style string up to where the user clicks enter (which would leave a \r\n), reference **line 26**. It then copies the user_input into what I am calling hash_input, which is the item that is about to be sent through the hashing algorithm. At line **38** the function creates the seed, that will be later used in the hash_alg function, by taking each ASCII value of the string 'Gommage' adding it to the previous char and multiplying it by 31 (0x1f).

### hash_alg
hash_alg utilizes the **Fisher-Yates Shuffle** which selects a random index between 0 and the length of the password (See lines 11-18 below) and swaps the value at that index with the value at the end of the string: 
![Screenshot of hash_alg in Ghidra](ghidra_hash_alg.png)
