# Git
When there is a file to be omitted from the commits which also should not to be pushed to remote repository, the following command can be used:

```
$ git filter-branch -f --index-filter 'git rm --cached --ignore-unmatch <relative directory of the targe file>'
```
