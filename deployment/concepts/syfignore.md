# `.sfyignore`

While building your source you may want to ignore certain file patterns (as we specify in [`.gitignore`](https://git-scm.com/docs/gitignore)). You can do so by creating a `.sfyignore` file in the directory where you have the deployment script.

This `.sfyignore` file follows the same rule as specified by the [`.gitignore`](https://git-scm.com/docs/gitignore) file.

If your repository is already a git repository and has a [`.gitignore`](https://git-scm.com/docs/gitignore) file then you do not specifically need to create a `.sfyignore` file as we will automatically detect the files to ignore.

## Examples

Consider that we have a following directory structure:

```
.
├── .sfyignore
├── main.py
├── main.pyc
├── deploy.py
├── deploy.pyc
├── requirements.txt
├── notes.txt
├── logs/
│   └── local.log
└── data/
    ├── file1.csv
    ├── file2.csv
    └── vocab.txt
```

And our `.sfyignore` contains:

```
# ignore a file
notes.txt

# ignore files by pattern: https://git-scm.com/docs/gitignore#_pattern_format
*.py[cod]

# ignore a directory
logs/

# ignore a directory except for some files
data/*
!data/vocab.txt
```

When we try to build the source and deploy it, following files would be added having a directory structure as:

```
├── .sfyignore
├── main.py
├── deploy.py
├── requirements.txt
└── data/
    └── vocab.txt
```

**Note:** If we would have the `.gitignore` file instead of the `.sfyignore` file then the results would have been the same.

**Note:** It is important to note that when deploying your application, `.sfyignore` should be present at the root of the project (same path as `deploy.py`). Consider the following example:

```
.
├── .sfyignore
├── main.py
├── main.pyc
├── deploy.py
├── deploy.pyc
├── requirements.txt
└── models/
    └── .sfyignore
```

In the above example, servicefoundry would parse the `.sfyignore` file present in the root of the project and not the one in `./models/.sfyignore`.
