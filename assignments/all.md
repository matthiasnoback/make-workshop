# Tabs or spaces

`Make` wants you to use tabs. The Makefile PhpStorm plugin deals with it for you. You can use spaces if you like (but you have to Google for it).

# File-based targets

Create a `Makefile` file. Add a target `copy` to it which will copy `source.txt` to `target.txt` but *only* if it doesn't exist, or if `target.txt` is outdated.

Question: how could you force `Make` to run the target, even though `target.txt` isn't outdated? Find *two* ways.

# A phony target

The new `clean` target should remove any generated files that can be recreated based on source-controlled files. So far we only have `target.txt`, so we should remove that file in the `clean` target.

Since `clean` itself is not a target file, label this target as "phony".

# Using `find` for prerequisites

If you don't know in advance which files should count as prerequisites for a certain target, you can define them dynamically, e.g. using the `find` command. For instance, if all the prerequisites for a target can be listed by running `find docker/webserver/*`, that target's prerequisites can be defined as: `$(shell find docker/webserver/*)`

# Dummy target files

Sometimes the result of a target is not a single file with a timestamp. In that case, you can create dummy target files, to still keep track of the last time you executed a target.

Let's "fake" the build of a Docker image:

- Create a `webserver` target. It will be responsible of building the `webserver Docker image. 
- The recipe for this new target should have only one line, which `touch`-es a file called `webserver`.
- Verify that the target is only executed when a file in `docker/webserver/` has been modified.
- Don't forget to add the dummy target file to the `clean` target.

In summary: the `webserver` target produces a file `webserver`. This target will only be executed if the `webserver` file is older than any of the files in `docker/webserver/`. 

# Building the Docker image

Now, instead of faking the build, we should be building the actual Docker image. You know how to do it!
Test the target, by modifying `docker/webserver/Dockerfile` in the same way you did in the Docker workshop.

# Moving the dummy target files out of the root

You can set up a dedicated directory for those dummy target files. At the top of the file, create a `.build` directory. Use the `shell` syntax we used earlier.

Then, to actually store the dummy target files in the `.build` directory, change the `webserver` target to `.build/webserver` and `touch` that file, instead of `webserver` at the end of the recipe.

Now you can't run `make webserver` anymore. Instead, run `make .build/webserver`.

# Create an alias

Finally, create a "phony" target called "build" which is basically a shortcut for `.build/webserver`, so you can `make build`.

