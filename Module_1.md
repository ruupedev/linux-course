# Module 2

## 1 Basic Commands

In this part we use `mkdir`, `cd` and `touch` to create the file structure.

![Creating file structure](img/module2_img1.png)

After we have the structure we used `nano` to add text to the two files.

![Adding text to files](img/module2_img2.png)

Next we use `mv` to rename, `cp` to copy and `rm` to delete. `nano` and `more` were used to edit and check the content of animals.txt

![Testing rename, copy, edit, delete](img/module2_img3.png)

## 2 Grep and Pipe

![Testing grep + parameters](img/module2_img4.png)

`grep` is used to search patterns in text. `-i` ignores case. `-n` shows line numbers. `-v` inverts the match (everything else BUT the search).

Next we used `wc` for word count. `-l` = lines. `-w` = words. `-c` = characters

![Testing worc count + parameters](img/module2_img5.png)

In the next phase we first edited the animals.txt file and then ran `cat` to print files contents paired with `|` so we could add more commands for what was selected with the `cat` command.

`grep cat` searched for lines with "cat". `wc -l` calculated the lines in the file. `sort | uniq` first sorts the lines to alphabetical order and `uniq` removes any dublicates.

![Pipe testing](img/module2_img6.png)

Next we're practicing grep, pipe and wc commands with GPL-2 license file.

![GPL testing 1](img/module2_img7.png)
![GPL testing 2](img/module2_img8.png)

`wc -l GPL-2` => word count & counts lines
`grep GNU GPL-2` => prints all lines that contain "GNU"
`grep -c GNU GPL-2` => counts all the lines that contain "GNU"
`grep license GPL-2` => shows all lines containing "license"
`grep -i license GPL-2` => shows all lines containing "license" (case INsensitive)
`grep -i and GPL-2 | wc -l` => finds lines with "and" (case insensitive) and counts them

### GPL-2 License - Main Ideas

- Freedom to use: anyone can run the software for any purpose
- Freedom to study: source code must be available so anyone can see how it works
- Freedom to modify: you can change the software however you need
- Freedom to distribute: you can share the original or modified software freely
- Copyleft: modified versions must also use GPL-2, keeping the software open source forever
- No warranty: software is provided "as is", the author is not responsible for any problems
- Protects users: nobody can take the software, make it closed source and sell it as their own
