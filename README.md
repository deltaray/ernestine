
INTRO
-----------------------

Ernestine is call routing software for asterisk.

It is meant to be easier to use for matching dates, times and caller
id information than the standard options for Asterisk. It is especially
useful when dealing with large amounts of rules or how to route a call
depending on external information.

PREREQUESITES
----------------------------

  I always try to write programs with as few of prerequesites as possible,
however there are a few that I could not easily write this program without:

  Perl modules:

    Asterisk::AGI
    Config::General

  It also uses these modules that should already be included with perl.
  Please let me know if yours does not inclue them.
    Time::HiRes
    Scalar::Util 
    Getopt::Std

  You can install these either through Perl's CPAN like this:

    cpan Asterisk::AGI
    cpan Config::General


INSTALLATION
----------------------------

 1. Copy the ernestine program to your agi-bin directory.

 2. Copy the example ernestine.conf file to /etc/asterisk/ernestine.conf
 
 3. Copy the sample ruleset directory ernestine.d to /etc/asterisk/ernestine.d
    Then configure the 'main' ruleset file according to what you want.

 4. Create the two log files at /var/log/asterisk/ernestine.call_log and
    /var/log/asterisk/ernestine.program_log or wherever you configure
    them to be.  Make sure these two files are owned by the user that
    your asterisk server runs as. This is usually 'asterisk'.

 5. Now you can put ernestine into your dialplan. Below is an example
 where

  exten => s,1,AGI(ernestine)
  exten => s,n,Goto(internal,100,1) ; Fallback to 100 in case ernestine fails.

 You don't have to put it first or even in the default extension.  However you
will get more out of it if you do so. While you're testing your setup, you may
want to first setup ernestine on a test extension. It can be anything you wish.
Then ernestine will only be run when you dial that extension.


RULESETS
-----------------------------

The default 'fullrule' type of ruleset file has lines that looks like this:

Jan,Feb,Oct-Dec  5,10-31  Mon-Sun  00:00-24:00  8001234567 PLAY(goodbye),HANGUP

Each column is an expression that can be matched and then a route target which secifies
what to do when the line matches. Below is the default column format:

MONTH   MONTHDAY   DAYOFWEEK   TIME   CALLERID   ROUTE

* can be used to match anything for any of the columns.

MONTH is the name of the month or the number. January, Jan and 1 are
 all the same.  This column allows you to specify a comma seprarated
 list of values or a range or both.

MONTHDAY is the numeric day of the month.
    Range or comma separated values are accepted.

DAYOFWEEK is Monday through Friday.
    Range or comma separated values are accepted.

TIME  is the time of day. This must be in 24-hour time format and include
  the minutes like this:
           15:00-20:00  means 3pm to 8pm.

CALLEDNUMBER is the number that the caller dialed.

CALLERID is the caller id of the caller.

CALLERNAME is the Caller id name value.

ROUTE is the target route to run if all the expressions match.


The other type of ruleset file is a 'numberlist'. In this format, a file
is specified using Path and then if the callerid number matches any of the
numbers in the file, then the call is redirected to the value of DefaultRoute
This is useful for matching against large lists of numbers, such as known
telemarketing numbers.


