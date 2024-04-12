# Discussion 2 notes (Kevin)

## Lab 1 Hints

Essentially making a sequential pipe ( | ) operator. 
 ``` shell 
./pipe ls cat wc 
ls | cat | wc 
 ``` 
 The above program should have the same output 

 ### 3 Subcomponents needed
 1.  Read and Execute the program 
 2. Execution order should align with argument order
 3. Create a pipe between two adjacent arguments
  (ie ``` arg[v] ``` should output into the input of ``` arg[v+1] ```) 

## Looking at each subcomponent

### Read and Execute

#### Some tips 

- The program name ./pipe will be read as an argument at index 0. Make sure if we are looping through the arguments we start at index 1. 

For example 

```  shell 
./pipe ls cat wc
#if we loop over this and start at i = 0, error. make sure to start at i=1 
```


### Some notes on ```execlp ```

*(Read Docs for more info)*
``` shell
Code A 
execlp(”ls”, “ls”, “-a”, “-l”, NULL) 
Code B
```

```Code A``` will run, then ``execlp`` will run, the essentially the second ``ls`` with arguments will replace the first ``ls``. That will run then the program ends. ```Code B``` will not be ran. 


#### **What we want** 

To not replace, we must create a child process (via `` fork ``). When the `` fork `` is executes, a child process is made, all variables are the same as the original program. Processes are distinguished by return code. Child process has a return code of 0, parent return code is the process id of the child. 

**Fork example** 
``` C 
int return_code = fork()
if (return_code == 0) {
    printf("Child Process\n");
} else if (return_code > 0){
    printf("Parent Process\n");
} else {
    printf("error!");
}
```
#### Important 
When these two processes are created, say via fork(), they are seen as equal by the CPU. So child or parent may be ran first, ordering it not determined. 

### Enforcing ordering via ``waitpid``
We can do it by: 
- locks
- wait 

We will use ``wait``

We will use the API ``waitpid``

### Example

``` C 
int return_code = fork();
int pid = return_code;
int status = 0; 
waitpid(pid, &status, 0);
```

We wait on pid to finish. For our example, we will wait for the child process to finish. We reference ``status`` to provide where we should modify the return of the pid process (in this case the chlid process). Essentially, we need to know where the child process ends. 


--- 

### ``Pipe``

``Pipe`` usually takes array name as argument 

#### Examples 
``` C 
int fds[2];
pipe(fds); // Create pipe of two ends
int ret = fork();
if (ret == 0) {
    write(fds[1], "hello", 6);
    write(fds[1], "hello again", 11);
}
```

``pipe`` has an implicit waiting mechanism. Pipe waits for information to be dumped inside. If we sleep between the two writes, the second message will not be shown. 



#### Check API ``dupe `` and ``dupe2``