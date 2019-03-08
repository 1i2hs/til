# Git

## How to omit certain file from the previous commits?
**Date written: 2019-03-04**

When there is a file to be omitted from the commits which also should not to be pushed to remote repository, the following command can be used:

```
$ git filter-branch -f --index-filter 'git rm --cached --ignore-unmatch <relative directory of the targe file>'
```

## How to delete files within folders with a certain name?
**Date written: 2019-03-08**

use follow command in shell to delete : 
```
find . -name '<directory_name>' -exec rm -rf {} \;
```