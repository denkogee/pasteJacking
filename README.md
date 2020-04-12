# pasteJacking

The main purpose of the tool is automating (PasteJacking/Clipboard poisoning/whatever you name it) attack with collecting all the known tricks used in this attack in one place and one automated job as after searching I found there's no tool doing this job the right way :smile:

Now because this attack depends on what the user will paste, I implemented the Metasploit web-delivery module's idea into the tool so when the user pastes into the terminal, you gets meterpreter session on his device :smile:

### What's PasteJacking ?
In short, Pastejacking is a method that malicious websites employ to take control of your computers’ clipboard and change its content to something harmful without your knowledge. *[From The Windows club definition](https://www.thewindowsclub.com/what-is-pastejacking)*

So here what I did is automating the original attack and adding two other tricks to fool the user, using HTML and CSS *Will talk about it* then added meterpreter sessions as I said before.

### A simple scenario to make things clear:
1. The target opens an HTML page served by the tool and this page has anything that makes the user wants to copy from it and paste into the terminal. *Ex: package installation instructions*
2. Target copies anything from the page then in the background it gets replaced quickly with our liner.
3. The user pastes into the terminal and before he notices that the line he copied has been changed :
    - The line gets executed by itself in the background (Without pressing enter)
    - The terminal gets cleared.
    - The user sees the terminal is usable again.
    - You already got your meterpreter session by this time.
4. All of that happened in less than second and maybe the user thinks this is a bad program and he won't install it :smile:

### This tool uses 3 methods to trick user into copying our payload instead of the command he copies:
 + **Using javascript to hook the copy event and replace copied data.**
    - Advantages :
        1. Anything the user copies in the page will be replaced with our line.
        2. Command executed by itself once target paste it without pressing enter.
    - Disadvantages :
        1. Requires Javascript to be enabled on the target browser.


 + **Using span style attribute to hide our lines by overwriting.**
    - Advantages :
        1. Doesn't require javascript to be enabled.
        2. Works on all browsers.
    - Disadvantages :
        1. Target must select all the text in the page or the first two words to ensure that he copies our hidden malicious lines.


 + **Using span style again but this time to make our text transparent and non-markable.**
    - Advantages :
        1. Doesn't require javascript to be enabled.
    - Disadvantages :
        1. Target must select all the text in the page to ensure that he copies our hidden malicious lines.
        2. Not working on opera and chrome.

##### What's the payload user copies ?
PasteJacker gives you the option to do one of this things:
    1. Generate a msfvenom backdoor on our machine and the liner target gonna copy will download the backdoor on the its machine, through wget or certutil depends on the OS, then executes it on the background without printing anything to the terminal.
    2. Serve a liner that gets you a reverse netcat connection on the target machine running in the background of course.
    3. Serve your **custom** liner like Metasploit web-delivery payload with adding some touches to hide any possible output.
