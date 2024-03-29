# siv_tester
*siv_tester* is a test framework for the System Integrity Verifier (SIV) developed in course ET2595 at BTH.

**VERY IMPORTANT: The siv_tester is not compatible with Python 2 (which is officially deprecated by now)** 

The purpose of this framwork is to help the teacher evaluate the submissions. The students can also use it to test their own code before submission. **Please note that this is an automated test. The succesfull execution of your SIV in this framework does not guarantee that your submission fulfills all requirements for a passing grade.**

To get started you just need to download the file `test_siv.py` to one of the virtual machines used in the lab work (I recommend Server A). Place the file preferably in the folder where you have the SIV executable. 

The file `test_siv.py` is a Python script that implements the test framework. Open up a terminal and change directory to the folder where you installed the script. Change the access permission of the script so that is executable: `chmod +x test_siv.py`. Now you can enter the command `./test_siv.py -h` to run the script in help. You will see output as shown below.

```
usage: test_siv.py [-h] -s SIVEXEC [-e ENVDIR] [-i] [-v]

optional arguments:
  -h, --help            show this help message and exit
  -s SIVEXEC, --siv SIVEXEC
  -e ENVDIR, --env ENVDIR
  -i, --init
  -v, --verify
```
*siv_tester* expects that you provide the path to your *siv* executable (SIVEXEC) and a path to a folder where test data and logs will be produced (ENVDIR). For example,

```
./test_siv.py -s ./siv.py -e orig
```

will attemp to execute a file called `siv.py` located in the current directory. Furthermore, it will create a folder called `orig` in the current directory. The folder will store the test environment. In my testing environment, I create a `data` folder under `orig`, which the directory to be monitored. Then I instruct your SIV to place the report and DB files under `orig`:

```
./siv.py -i -D orig/data -V orig/vDB -R orig/rep.txt -H sha1
```

It's important that your *siv* has execution permission bits set (see how it was done above for `test_siv.py`). Also, there is no need for your *siv* to be written in Python - you can use C/C++, Java, Bash or Perl instead if that's what you prefere.

There are two functions in the *siv_tester* that you must modify: `populate_env()` and `change_env()`. The first one is used to populate the environment with folders, subfolders and files. The second is used to change an existing environment. When you execute the *siv_tester* it populates the environment (`populate_env`) and calls your SIV in initializaton mode. Then, it changes the environment (`change_env`) and calls your SIV in verification mode. The contents of the report generated by your SIV should match the changes introduced by `change_env()`.

I have put some code in these two functions that shows how you create/delete files and folders and how you change their properties. **Please note that these changes are not what I use when verifying your SIV - those I keep confidential!**

The *siv_tester* requires that you make some changes to the system (server A) as outlined below so that it can execute `sudo chown` without being asked for the password. In a terminal, enter `sudo visudo` and the following line after the %sudo line:

```
student  ALL=(ALL) NOPASSWD: /bin/chown
```



