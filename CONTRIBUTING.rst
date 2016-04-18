Development process
-------------------

Here's the long and short of it:

1. If you are a first-time contributor:

   * Go to `https://github.com/berkeley-stat222/mousestyles
     <http://github.com/berkeley-stat222/mousestyles>`_ and click the
     "fork" button to create your own copy of the project.

   * Clone the project to your local computer::

      git clone git@github.com:*your-Github-username*/mousestyles.git

   * Add the upstream repository::

      git remote add upstream git@github.com:berkeley-stat222/mousestyles.git

   * Now, you have remote repositories named:

      - ``upstream``, which refers to the ``berkeley-stat222/mousestyles`` repository
      - ``origin``, which refers to your personal fork

2. Develop your contribution:

   * Pull the latest changes from upstream::

      git checkout master
      git pull upstream master

   * Create a branch for the feature you want to work on. Since the
     branch name will appear in the merge message, use a sensible name based on the feature you want to improve or add such as '*feature*-issue#\ *number*' (e.g. ``git checkout -b contributing-issue#14``)::

      git checkout -b contributing-issue#14

     See `tools/bash-and-git.sh`,
     which contains useful configurations for visualizing and
     keeping track of branches more easily.

   * Commit locally as you progress (``git add`` and ``git
     commit``). It is highly recommended that you use the provided
     pre-commit hook (described in the `tools README <https://https://github.com/berkeley-stat222/mousestyles/blob/master/tools/README.md>`_), which automatically runs tests and style checks before every commit. This way, you can see if Travis (described below) will fail before you push.

3. To submit your contribution:

   * Push your changes back to your fork on GitHub::

      git push origin contributing-issue#14

   * Go to GitHub. The new branch will show up with a green Pull Request
     button - click it.

   * If you want, comment on the `issue posting in Github
     <https://github.com/berkeley-stat222/mousestyles/issues/issue#*number*>`_ to explain your changes or
     to ask for review.


4. Review process:

    * Reviewers (the other developers and interested community
      members) will write inline and/or general comments on your Pull
      Request (PR) to help you improve its implementation,
      documentation and style.  Every single developer working on the
      project has their code reviewed, and we've come to see it as
      friendly conversation from which we all learn and the overall
      code quality benefits.  Therefore, please don't let the review
      discourage you from contributing: its only aim is to improve the
      quality of the project, not to criticize (we are, after all,
      very grateful for the time you're donating!). See `here
      <https://github.com/thoughtbot/guides/tree/master/code-review>`_
      for some nice code review guidelines.

    * To update your pull request, make your changes on your local repository
      and commit. As soon as those changes are pushed up (to the same branch as
      before) the pull request will update automatically.

    * `Travis-CI <http://travis-ci.org/>`__, a continuous integration service,
      is triggered after each Pull Request update to build the code, run unit
      tests, measure code coverage and check coding style (PEP8) of your
      branch. The Travis tests must pass before your PR can be merged. If
      Travis fails, you can find out why by clicking on the "failed" icon (red
      cross) and inspecting the build and test log.

5. Document changes

    Before merging your commits, you must add a description of your changes
    to the release notes of the upcoming version in
    ``doc/release/release_dev.rst``.

.. note:: To reviewers: if it is not obvious, add a short explanation of what a branch
   did to the merge message and, if closing a bug, also add "Closes #14"
   where 14 is the issue number.


Divergence between ``upstream master`` and your feature branch
--------------------------------------------------------------

Do *not* ever merge the main branch into yours. If GitHub indicates that the
branch of your Pull Request can no longer be merged automatically, rebase
onto master::

   git checkout master
   git pull upstream master
   git checkout contributing-issue#14
   git rebase master

If any conflicts occur in (e.g. conflict-file1 and conflict-file2), fix the according files and continue::

   git add conflict-file1 conflict-file2
   git rebase --continue

Help in resolving merge conflicts is provided `here <https://help.github.com/articles/resolving-a-merge-conflict-from-the-command-line/>`__.

In some cases, you'll want to edit your history while rebasing. This
can be accomplished with the ``-i`` or ``--interactive`` option of
``git rebase``. Running ``rebase`` with this option will open a text
editor, where you can choose to remove or edit some commits, or squash
several together. This allows you to (for example) edit commit
messages, or merge together repetitive small commits like "fixed
typo." See this `tutorial
<https://robots.thoughtbot.com/git-interactive-rebase-squash-amend-rewriting-history>`_
for more details.

Note: you should only rebase your own branches and must generally not
rebase any branch which you collaborate on with someone else.

Finally, you must push your rebased branch::

   git push --force origin contributing-issue#14

(If you are curious, here's a further discussion on the
`dangers of rebasing <http://tinyurl.com/lll385>`__.
Also see this `LWN article <http://tinyurl.com/nqcbkj>`__.)


Guidelines
----------

* All code should have tests (see `test coverage`_ below for more details).
* All code should be documented, to the same
  `standard <http://github.com/numpy/numpy/blob/master/doc/HOWTO_DOCUMENT.rst.txt#docstring-standard>`__
  as NumPy and SciPy.
* No changes are ever committed without review.  Ask on the
  `mailing list <http://groups.google.com/group/mousestyles>`_ if
  you get no response to your pull request.
  **Never merge your own pull request.**

Stylistic Guidelines
--------------------

* Set up your editor to remove trailing whitespace.  Follow `PEP08
  <www.python.org/dev/peps/pep-0008/>`__.  Check code with pyflakes / flake8.

* Use numpy data types instead of strings (``np.uint8`` instead of
  ``"uint8"``).

* Use the following import conventions::

   import numpy as np
   import scipy as sp
   import matplotlib as mpl
   import matplotlib.pyplot as plt

   cimport numpy as cnp # in Cython code

Commit message codes
---------------------

Please prefix all commit summaries with one (or more) of the following labels.
This should help others to easily classify the commits into meaningful
categories:

* *BUG* : bug fix
* *RFT* : refactoring
* *ENH* : new feature or extended functionality
* *BKW* : addresses backward-compatibility
* *OPT* : optimization
* *BRK* : breaks something and/or tests fail
* *DOC*: for all kinds of documentation related commits
* *TST* : for adding or changing tests
* *DAT* : for adding or changing data files
* *STY* : PEP8 conformance, whitespace changes etc that do not affect
  function.

So your commit message might look something like this::

    TST: relax test threshold slightly

    Attempted fix for failure on windows test run when arrays are in fact
    very close (within 6 dp).

Keeping up a habit of doing this is useful because it makes it much easier to
see at a glance which changes are likely to be important when you are looking
for sources of bugs, fixes, large refactorings or new features.

Pull request codes
------------------

When you submit a pull request to github, github will ask you for a summary.  If
your code is not ready to merge, but you want to get feedback, please consider
using ``WIP - experimental optimization`` or similar for the title of your pull
request. That way we will all know that it's not yet ready to merge and that
you may be interested in more fundamental comments about design.

When you think the pull request is ready to merge, change the title (using the
*Edit* button) to something like ``MRG - optimization``.

Test coverage
-------------

Tests for a module should ideally cover all code in that module,
i.e., statement coverage should be at 100%.

To measure the test coverage, install
`coverage.py <http://nedbatchelder.com/code/coverage/>`__
(e.g., using ``pip install coverage``) and then run::

  $ make coverage

This will print a report with one line for each file in `mousestyles`,
detailing the test coverage::

  Name                 Stmts   Miss Branch BrMiss  Cover   Missing
  ----------------------------------------------------------------
  mousestyles                 43      6     10      1    87%   72, 77-88
  mousestyles.core            55      0     30      4    95%
  mousestyles.data            45      0      2      0   100%
  mousestyles.eda             22      0      8      0   100%
  mousestyles.irr             52      0     20      2    97%
  mousestyles.stratified      44      0     16      4    93%
  ----------------------------------------------------------------
  TOTAL                  261      6     86     11    95%
  ----------------------------------------------------------------------
  Ran 35 tests in 37.199s

  OK

Bugs
----

Please `report bugs on GitHub <https://github.com/berkeley-stat222/mousestyles/issues>`_.
