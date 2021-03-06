From README.md:


# tlocate
mlocate rewritten in bash with a text file instead of a database

## Pros/Cons over mlocate

### Pros: 
 * Allows nearly arbitrary filtering of results by attribute or type (see test man page for possible filters)
 * Only requires bash, GNU coreutils and sudo
 * Uses similar command syntax and configuration to mlocate
 * Allows querying for more than 1 regex
 * Better handling of btrfs subvolumes
 * (minor) Verifies that text file has not been altered
 * (minor) Searches are marginally faster (Almost unnoticeable without the time command)
 * (minor) `-p/--pretend` option shows command that would be executed which allows users to make sure they execute what they want
 * (minor) `--statistics` prints what was in the configuration file at the time of the last update
 * (minor) By default (without the `--quiet` flag), tlocate prints the date and time the text file was last updated after a query. This is automatically disabled if script is going into a pipe.

### Cons:
 * WRITTEN IN FREAKING BASH. Considering rewrite in Python. Also considering port to sh for extra portability
 * Updating text file takes longer than updating mlocate database (I'm looking into optimizations here)
 * (minor)No sanity checks on filtering e.g. `tlocate --files --or --existing` is redundant but the script allows it.
 * (minor) No logic sanity checks. e.g. `tlocate --files --and --directories` is accepted (though obviously returns no output).
 * (minor) In a similar vein, no performance sanity checks either e.g. `tlocate --existing --or --links` is more effecient than `tlocate --links --or --existing` (as bash does not evalute an or statement if the first test fails and files are more likely to exist than be links).
 * (minor) Text file takes up more space than mlocate database (could build in compression if this were a big deal)


## Differences in operation between [tm]locate:
 * Updating text file is not separate command. `tlocate -u` is the equivalent `updatedb`
 * `locate --nofolow` is equivalent to `tlocate --existing --or --links`
 * Regexes are default but can be disabled with `-x/--noregex`
 * Configuration files are very similar but entries resemble bash arrays
