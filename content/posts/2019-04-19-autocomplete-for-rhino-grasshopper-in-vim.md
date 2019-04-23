+++
title = "Autocompletion for Rhino/Grasshopper python scripts in Vim"
tags = [ "code", "vim", "rhino", "grasshopper", "python" ]
categories = [ "blog" ]
date = "2019-04-19"
+++

[Steve Baer](https://stevebaer.wordpress.com/) at [McNeel](https://mcneel.com/) posted on the [McNeel forum](https://discourse.mcneel.com/) about the Python package [Rhino-Stubs](https://pypi.org/project/Rhino-stubs/) he had created and how to use it in [PyCharm](https://www.jetbrains.com/pycharm/) for autocompletion and type hints for Rhino/Grasshopper scripts. Here's the [thread](https://discourse.mcneel.com/t/autocomplete-while-editing-python-scripts-outside-of-rhino/79329) and here's [the full explanation on Steve's blog](https://stevebaer.wordpress.com/2019/02/25/autocomplete-and-type-hints-with-python-scripts-for-rhino-grasshopper/).

I've started to write a lot of Python scripts in Grasshopper which usually means you write using the built in editor if you want autocompletion. Since I already copy my scripts to a [git repository](https://github.com/tetov/py-grasshopper) for version control I jumped at the chance to do more of the scripting in an external editor. While PyCharm has an OK Vim plugin I still longed for the comfort of trusty Vim.

The Rhino-Stubs package provides Python stubs as described in [PEP-484](https://www.python.org/dev/peps/pep-0484/) and [PEP-561](https://www.python.org/dev/peps/pep-0561/), and my initial research led me to an [issue on the jedi repository](https://github.com/davidhalter/jedi/issues/839) (a widely used autocompletion engine for python). However, I also found another source of stubs, [gtalarico/ironpython-stubs](https://github.com/gtalarico/ironpython-stubs) in another format. The repo's wiki includes [instructions for integration with Atom, VS Code and Sublime Code](https://github.com/gtalarico/ironpython-stubs/wiki), and I used these instructions to integrate the stubs with Vim and Jedi through [vim-jedi](https://github.com/davidhalter/jedi-vim).

Here are the steps needed in case someone else wants to try it. These instructions assumes that you have a Linux environment with an installation of Vim that has been compiled with python support, and that you have Python 2 installed. Other autocompletion plugins might interfere with jedi-vim.

1. Install the Vim plugin [jedi-vim](https://github.com/davidhalter/jedi-vim).
2. Clone or download [gtalarico/ironpython-stubs](https://github.com/gtalarico/ironpython-stubs). The repo will be referenced in the next step.
3. Append the path to the stubs to the `PYTHONPATH` variable in your shell profile (e.g. `.bash_profile/.bashrc/.zshrc`) using the command `export PYTHONPATH=${PYTHONPATH}:/path/to/iron-python-stubs/release/stubs.min`
4. Put `let g:jedi-vim#force_py_version = '2'` in your `.vimrc` if you have Python 3 in your environment.
5. You are now ready to complete using jedi-vim, the standard keymap is `<C>-<Space>`. I use [ervandew/supertab](https://github.com/ervandew/supertab) to map completions to `<Tab>`.
<script id="asciicast-242101" src="https://asciinema.org/a/242101.js" async></script>

I've tried to use the same method with the stubs provided by Rhino-Stubs, but haven't gotten it to work. If someone figures it out, please let me know. This would be interesting because Rhino-Stubs [provides better support](https://discourse.mcneel.com/t/autocomplete-while-editing-python-scripts-outside-of-rhino/79329/3) for [RhinoInside](https://discourse.mcneel.com/t/rhino-inside-python/78987).
