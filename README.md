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


Sample setting
==============

```lisp
(add-to-list 'load-path "/path/to/this/file_directory")
(require 'helm-git-files)
```

Please type "M-x helm-git-files" in a git repository.


Customization
==============
My setting is as follows:

```lisp
(require 'helm-git-files)

(defvar knbs-git-recentf-list '())

(defun knbs-git-set-recentf-list (rlist)
  (let ((root (helm-git-files:root))
        glist)
    (setq knbs-git-recentf-list '())
    (when root                          ; check in git repository
      (dolist (f rlist)
        (if (eq (string-match root f) 0)
            (add-to-list 'knbs-git-recentf-list f t))))))

(setq knbs-helm-source-git-recentf
  `((name . "Git Recentf")
    (init . (lambda ()
              (require 'recentf)
              (or recentf-mode (recentf-mode 1))
              (knbs-git-set-recentf-list recentf-list)))
    (candidates . knbs-git-recentf-list)
    (keymap . ,helm-generic-files-map)
    (help-message . helm-generic-file-help-message)
    (mode-line . helm-generic-file-mode-line-string)
    (action . ,(cdr (helm-get-actions-from-type
                     helm-source-locate))))))

(defun knbs-helm-git-files ()
  "`helm' for opening files managed by Git."
  (interactive)
  (helm-other-buffer `(knbs-helm-source-git-recentf
                       helm-git-files:modified-source
                           helm-git-files:untracked-source
                           helm-git-files:all-source
                           ,@(helm-git-files:submodule-sources
                              '(modified untracked all)))
                         "*helm for git files*"))
```
