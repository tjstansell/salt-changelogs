# Salt detailed changelogs

This is where I'm compiling and storing detailed changelogs for saltstack.  The
release notes for salt generally include more high-level items and not
necessarily every single change.  It's also hard to correlate all release note
items with issues they pertain to.

Some of the releases are quite large and so these changelogs are quite large.
I still like the details it provides, even though this is meant to be a
starting point for generating a complete set of release notes.  This is also
like looking at changes with only a microscope... kind of hard to see the
bigger picture.

# salt-issues script

I'm including the script I use to generate these as well.  It's a simple perl
script, but it does use connection caching (so it only makes 1 ssl connection
to github's API), it caches the issues (and PR) metadata locally, so you can
re-run the report and it doesn't have to re-fetch the issue data.  This lets
me test changes to the output formatting very easily.

## How you run it

The script assumes you're in your local clone of the salt repository.  This
script is relatively generic, so it could be used for other projects outside
of saltstack, but currently has the saltstack github api endpoint hardcoded.

Before you can use it, you should really go create your own [github api
token](https://github.com/settings/tokens) or you'll pretty quickly hit the api
limitations and everything will puke.  I created mine with only the
`public_repo` scope.  Take your generated key and add it to a `GITHUB_TOKEN`
env variable.

```sh
export GITHUB_TOKEN="insert_token_here"
git fetch upstream
perl ~/bin/salt-issues v2014.7.4..v2014.7.5 >details.rst
```

That's pretty much it.  Pass it a set of *changes* like you'd pass to `git log`
or `git diff`.

## Example output

Here's an example run of when I generated the v2015.5.0 changes.  And this is
the legend for what each character means:

- `.` = fetched issue from github api
- `,` = issue was found in cache
- `+` = found new issue referenced, added to next phase
- `s` = saved full issues to cache (every 20 new issues will cause this)

```
$ perl ~/bin/salt-issues upstream/2014.7..v2015.5.0 >2015.5.0-detailed.rst
** START: upstream/2014.7..v2015.5.0

Got 979 cached issues
querying 2698 issues:
........+...........+.s.....+.......+........s....................s............+
....+....s..+..................s...+.........+........s.....+......+........+.s+
................+.+...s...+.........+.+..+.....s.........,+.+++..........s+....+
.........+...+....s.+.......+.+.....+.+.+....s.+..........+........+.s.....+.+..
.........+...s...............+.....s....+.....++.....+......s............+..+...
...s....+....+.......+.+....s....+.....+........,...s+............++......+..s+.
..................+.s+.+.+.............+....+.s..................+..s..+........
....+.,.....s.............+....+...s.....+..+....+..+...+,+....s.++....+.......+
+........s.+.+.....+.+........+...+.s.....++.++++..............s....+.+.+..+....
+..+.+.....s..+.+..+....+........+...s.+..+..,.............+..s..+......+.......
+.....s+...+..+.+..............s....................s......+...+.....+.....+.s..
..................s+.+..+.....+..+.+.........s..+.+...+....+.,.........s..+.....
.....+....+..+.+.s...............+.....s...+................+.s.+...+.+..+.....+
.+.+..+..+..s+,.,..,.................s.,+.+.+..+.............+.+.s...+..........
.......s....................s....................s....................s.........
...........s....................s....................s....................s.....
...............s....................s....................s....................s.
...................s....................s....................s..................
..s....................s....................s.......+.+........,....s...........
.+........s....................s....................s....................s......
..............s....................s....................s....................s..
..................s....................s..............+......s..................
..s....................s.................+...s+,.........+..+..+,+.......s......
.....+...+......s....+...........,+.+....s.+.+.......+..+.........s...,,+.......
.,.........s........+..+..+........s......+..++....+...+..+...s...............+.
..+..s+..+.....+....+........+.s.+..........,.+..,..+..+..s.......+....+......,.
..s....,.......,.....+.,...s......+.,+.+.....+..+.+.+...s...+...,,+....,....+..,
,....s....+....,.....+...,..,..s+.+........++,,,..,.......,..s.+..,,.+......+...
.......s.................,+...s.........,.........+++..s.....+.+.....+.......,..
s.+...++......+...++.......s..........+.+.+........s..+...,+.+....,....+..+....s
.+...+,.......,+..,.+......s+........+......+......s..+...+..+....+....+.....s..
..+...............,+.s....,........+.+......+.s+......+....+,..+......+..s+++.,+
.,.,.+..........+....+..s+.......+.+.,++.+..,....+..+..s.....+........++...,....
s+........+......+.....+.s......+.+.+.+...,+......+..s+....+..+.+..+..+..++.+...
.,+..s.......+.+...+.,+........s+.......+.+,+.,+..,,.+,+,.+.+,.,...,..s++.......
.+..........+,.+.s.+..+....,.......+......s..........+..........s......+....++,+
....+..,+..++..s.........+..........,+.s.....+............+...s....+............
....s......,.........+.+..+,.+.s......+.........+.....s....................s....
.,+.+.+.,++.....+..+..+...s...+.................s..+.+..........+.......s.....+.
............+,+..s...+.................s..+.....++++++.........,..
.
querying 359 issues:
.s..............,......s...................,.s.........+........+...s...++......
...........s....................s......,........+.....,.s....................s+.
...++...,............+.s.+,..,...,.,..+....,.......s,++.,.++.,.,.....,.,.,.+.+..
.....s.+....+..,..+,.....+,...,...s,.......,.....,........s.,.,.,.........,+....
..,..s...,..,,..,..,...........s.,......,...,.........,.s.,,+,,,,,,...........+
querying 24 issues:
........s...,+..,..+.+..,.,.
querying 3 issues:
,..
```
