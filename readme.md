# Kaspar 2028

### docs

[https://ctechfilmuniversity.github.io/project_kaspar/](https://ctechfilmuniversity.github.io/project_kaspar/)

> contains all documents used to generate the project page


### modules

> contains all code written for the project

The code is organized in submodules ie. seperate repositories. Each repository is linked using the following commands:

```sh
cd ./modules && git submodule add https://github.com/username/repo
```

When cloning the parent repository (this repo), submodules will show up as empty directories. To acutally clone all submodules run the following commands after cloning:

```sh
git submodule init && git submodule update
```

To remove a submodule if it is no longer used run the following commands:

```sh
git submodule deinit -f path/to/submodule
```

```sh
git rm -f path/to/submodule
```



### staff

> contains personal files, thoughts or materials by each individual staff member

No rules as long as the directory has your name