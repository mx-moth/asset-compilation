# Asset compilation

Asset compilation sucks, but is necessary. Makefiles make things easier.

## Requirements

In its most basic form, you need `make`. Get it from your package manager. It should be called `make`.

This version compiles CoffeeScript and LESS. As such, CoffeeScript and LESS are required.
These can be obtained from `npm` by running `npm install coffee-script` and `npm install less`,
respectively.

Additionally, the compressor uses the YUI compressor, which needs Java.

# Running

`make` is dead simple. Really.

## Compiling

	$ make

## Compressing

	$ make production

## Automatically compiling everything as you change it

Too lazy to even do that? Run this and `make` will do it for you:

	$ make watch

You can send options to `make watch`, if you want to compress things as well for example:

	$ make watch production

This will look at all your source files, every two seconds. If you have a butt load, this may start
taxing your computer/hard drive marginally.

## Removing scripts, forcing updates, and fixing conflicts

If you want to do any of the above, run the following:

	make clean
	make

**This will delete everything in the destination directory**. Seriously. The `clean` rule is quite
literally `rm -R $(DESTINATION)`. Everything will be gone.

Luckily, the very next thing we do is `make`, which will put them all right back in there again,
good as new. The lesson to take from this is that you probably shouldn't modify files in the
destination directory. Just dont do it.

### Why would I want to do this?

When source control is used, and two people change a source file, the minified destination file will
almost always have a conflict when it is merged. If you have updated your version
of LESS or CoffeeScript; or added a new compressor, `make` wont know to rebuild old files. Sometimes,
things just go dicky. Luckily, as the destination directory should be made up only of things compiled
from source, running `make clean; make` will generate you a nice brand new set of files.

# Modifying this Makefile

Feel free to fork this and add your own rules. Ill probably include your modification if you send
a pull request my way.

The Makefile is documented, but it assumes you know how Makefiles are written. If you do not know
about Makefiles, then you may want to read up them first. They are cool.

## Adding compilation rules

The compilation rules are defined in one spot, for ease of maintaining and updating this file in
the future.

The syntax of a compilation rule is pretty simple. Say we want to compile all `.foo` files in to `.bar`
files, using the `frobnicate` compiler:

	DEST_FILES := $(DEST_FILES) $(patsubst %.foo,%.bar,$(filter %.foo,$(DEST_FILES)))
	%.bar : %.foo ; frobnicate --input="$<" --output="$@"

The first line adds a bunch of compiled destination targets by substituting `%.foo` for `%.bar`.
It then defines a rule for creating any `%.bar` file out of its respective `%.foo` file.

## Adding a compression rule

Now that we have compiled our `.bar` files, we want to compress them:

	COMPRESSED_FILES := $(patsubst %.bar,%.min.bar,$(COMPRESSED_FILES))
	%.min.bar : %.bar ; bar-compressor --input="$<" --output="$@"

# License

There isnt one. I hereby release this to the Public Domain, etc. etc. Use it at home, at school, 
at work, on the loo; I am not fussed.
