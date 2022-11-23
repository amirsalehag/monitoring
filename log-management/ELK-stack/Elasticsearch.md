# *what is elasticsearch*
elasticsearch is where we store , search and analyse our data.

---
# write access for elastic user
For elastic we need to grant an access to the directory which it wants to read and write data to, we need to specify the  
directory which is mounted for its container an access:  
```
mkdir esdatadir
chmod g+rwx esdatadir
chgrp 0 esdatadir
```
because elastic user is a part of a group called "0" , we can just use that as its permission.  

---
