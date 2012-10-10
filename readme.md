      ,  ,  ,_     _,  , ,   ___, ,  , 
      |  |  |_)   /    |_|, ' |   |\ | 
     '\__| '| \  '\_  '| |   _|_, |'\| 
    `  '  `    `  ' `  '     '  ` 

Urchin is a test framework for shell.

## Install
Download Urchin like so (as root)

    wget -O /usr/local/bin https://raw.github.com/scraperwiki/urchin/master/urchin
    chmod +x /usr/local/bin/urchin

Now you can run it.

    urchin <test directory>

## Writing tests
Make a root directory for your tests. Inside it, put executable files that
exit `0` on success and something else on fail. Non-executable files and hidden
files (dotfiles) are ignored, so you can store fixtures right next to your
tests. Run urchin from inside the tests directory.

Urchin only cares about the exit code, so you can actually write your tests
in any language, not just shell.

## More about writing tests
Tests are organized recursively in directories, where the names of the files
and directories have special meanings.

    tests/
      setup
      setup_dir
      bar/
        setup
        test_that_something_works
        teardown
      baz/
        jack-in-the-box/
          setup
          test_that_something_works
          teardown
        cat-in-the-box/
          fixtures/
            thingy.pdf
          test_thingy
      teardown

Directories are processed in a depth-first order. When a particular directory
is processed, `setup_dir` is run before everything else in the directory, including
subdirectories. `teardown_dir` is run after everything else in the directory. 

A directory's `setup` file, if it exists, is run right before each test file
within the particular directory, and the `teardown` file is run right after.

Files are only run if they are executable, and files beginning with `.` are
ignored. Thus, fixtures and libraries can be included sloppily within the test
directory tree. The test passes if the file exits 0; otherwise, it fails.
