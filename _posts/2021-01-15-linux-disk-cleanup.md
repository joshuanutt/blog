# Cleanup disks in Linux

The drives in one of my lab servers was full but I couldn't figure out what was filling it up until I discovered ```du```!

```du```,or "Disk Usage", is a standard Linux/Unix command that reports on disk space used by the given files/directories.
It is great for finding files or directories consuming tons of disk space.


# Du you know how to use ```du```?

I like using ```du``` like this:
```bash
du -h --max-depth=1
```
- ```-h```,```--human-readable```
	- Converts values to K/M/G for human readability
- ```--max-depth=1```
	- Print total for directory only if it is N or fewer levels below command line argument
	- I felt like this helped me narrow down which directory was causing the issue, it also made it easier to read the output, in my opinion.

Results:
```bash
7.7M    ./bin
0       ./boot
960K    ./dev
1.6M    ./etc
0       ./home
24M     ./lib
0       ./lib64
0       ./media
0       ./mnt
2.1G    ./opt
```

I then ```cd``` into the offending directory, ```./opt``` in this case, and run the same command.

I continued like this until I find the root cause deleted it. 
