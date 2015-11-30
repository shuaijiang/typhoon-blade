# Download #
Firstly, you should download the zip package, an put it into any dir.

# Install #
```
$ zip blade-1.0.zip
$ cd blade
$ ./install
```
The installing program only make symbolic links the binaries to the ones in the unzipped dir, but not make a copy. so don't remove the unzipped dir.

Blade will be installed to ~/bin, if it's not in your PATH environment, relogin maybe required.

# Experience #
Now, you can experience blade by example.
```
$ cd example
$ blade build ...
$ blade test...
$ blade clean ...
$ blade run app:app
$ blade 
```

# Build your code #
Organize you code same as example, you can enjoy blade now.