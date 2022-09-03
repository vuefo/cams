# [cams](https://vuefo.github.io/cams)

Note, when __GitHub pages__ isn't [operating normally](https://www.githubstatus.com/), you can try accessing the latest image directly from the repo... e.g. by clicking [here](https://raw.githubusercontent.com/vuefo/cams/gh-pages/res/vv.jpg).

Otherwise not much to know, except maybe this stuff:
```
#!/usr/bin/env perl

# https://towardsdatascience.com/how-to-create-a-free-github-pages-website-53743d7524e1
# https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax

# zap the history of a git[hub] repository? should free space used by old images
# https://gist.github.com/stephenhardy/5470814?permalink_comment_id=4001713#gistcomment-4001713

# example cron job:
#*/20 * * * * bash -c 'HR=$(date +\%-H); [[ $HR -lt 8 || $HR -gt 18 ]] && exit; ~/bin/snapup.pl' 2> /dev/null

use strict; use warnings; use feature qw(say);

local $ENV{PATH} = "$ENV{HOME}/bin:$ENV{PATH}"
    if (!defined $ENV{TERM});  # presumably running from cron

chdir "$ENV{'HOME'}/cams";
`raspistill -ex auto -mm average -awb auto -drc low -o /tmp/vv.jpg`;

chomp (my $INFO = `date`);
my $cmd=qq[    convert /tmp/vv.jpg -font Fantasque-Sans-Mono-Regular \\
    -pointsize 50 -resize 1600x1200 -draw "gravity northwest \\
    translate 20,0 \\
    fill seashell2  text 0,12 '$INFO' \\
    fill seashell4  text 3,10 '$INFO' " \\
    -quality 60 \\
    res/vv.jpg
    ];
`$cmd`;

if (defined $ENV{TERM}) {
    say "skipping git actions";
} else {
    # presumably running from cron
    `git commit -m '$INFO' res/vv.jpg`;
    `git push`;
}

# https://fedsoc.org/commentary/publications/conservative-libertarian-pre-law-reading-list

# ssh pi@192.168.1.143 -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no
# \scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no ./snapup.pl pi@192.168.1.143:~/bin/
# git clone git@github.com:vuefo/cams.git
```
