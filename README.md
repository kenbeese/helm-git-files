helm-git-files
==============

Yet another helm to list git files.

It is a fork of [anything-git-files-el](http://github.com/tarao/anything-git-files-el).

Features
==============

- List all the files in a git repository which the file of the current buffer belongs to
  - List untracked files and modified files in different sources
- List files in the submodules
- Cache the list as long as the repository is not modified
- Avoid interrupting user inputs; external git command is invoked asynchronously
