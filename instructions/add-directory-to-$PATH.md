
Set the **$PATH** variable in the .bashrc file.

NOTE: This only applies to the current user's environment.

In the following example we’ll set the variable to add a folder named `bin` in our home directory.

Add the following line at the end of `.bashrc`:
<br><br>

```
export PATH="$HOME/bin:$PATH"
```
<br><br>
Reload the shell:

```
source ~/.bashrc
```
