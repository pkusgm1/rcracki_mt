[rcracki_mt README]

USAGE
================
example: rcracki_mt -h 5d41402abc4b2a76b9719d911017c592 -t 4 -o save.txt C:\md5

Start rcracki_mt without any arguments to view usage information in short. This README describes the various 
options in more detail. Many options can be set to a default value by editing rcracki_mt.ini. Command line 
arguments get priority over settings in the ini file.

INPUT
----------------
rcracki_mt takes one hash on the command line (using -h) or an input file containing the hashes. rcracki_mt supports 
three formats for the input file. Use one of the following options to specify the format followed by the filename:

-l:	specify a list of hashes (one hash per line)
-f:	specify a pwdump file
-c:	specify a .lst file (format in which Cain stores hashes and results)

SELECTING RAINBOW TABLES
----------------
Any command line argument that is not an option will be interpreted as a directory to search for rainbow tables, 
multiple directories can be specified. rcracki_mt recursively scans all specified directories for *.rti (indexed) 
and *.rt (old/original) files. You can use .rt & .rti files at once, but this hasn't been tested thoroughly.

You can set default locations to search for rainbow tables in rcracki_mt.ini. You need to use these in combination 
with the command line argument -a [algorithm]. See the comments in the ini file for examples.

SESSIONS & RESUMING
----------------
Rcracki_mt has session support, which means that it stores its progress. This allows you to interrupt the session 
and resume later on. This also allows sessions that stopped because of a crash (application or even system) to 
resume. To use this feature, start rcracki_mt with all the options you'd like, then specify a session name with:

-s session_name:	specify a session name

Now during cracking, all your valuable precalculations are stored to disk, as well as progress (which files have 
been checked) and cracked hashes. If you decide to interrupt the session (using CTRL+C), you can resume it using 
the -r option. For example:

rcracki_mt -r -s my_personal_hashes

While resuming rcracki_mt you can/have to specify the less important options again, like number of threads and 
showing debug information. Usually you will have these settings set to a default value in the .ini file anyway. 
Session are deleted after the run is completed. You can choose to keep the precalculation work on disk, for example 
if you want to reuse your session later on. Use the '-k' option to enable this feature.

Rcracki_mt has a default session which gets overwritten every time you start a new job without specifying a session 
name. It might be interesting to always keep precalculation work by enabling this feature in rcracki_mt.ini. But 
pay attention, these precalculations can become quite large on disk. Currently there is a maximum of around 500 GB 
of storage for these precalculations. You can always decide to manually remove the .precalc and .precalc.index 
files from disk. Always remove both at the same time, you will screw up your results if you don't. A possible 
'todo' for development is to do some verification before using stored precalculations.

OPTIONAL
----------------
-t:	Number of threads to use (for precalculation and false alarm checking)
Note: In Windows the crack threads run with lower priority.

-o:	specify an output file to store found hashes in a colon (:) separated format.
	Hashes are saved immediately when found. Especially useful if you have a large list of hashes.

-v:	Show more information during cracking, for debugging purposes. Please use this flag if you want to show 
output and report a bug.


EXTRA FEATURES
----------------
You can pause a running rcracki_mt by using 'P'. It might not pause right away, it actually pauses after doing 
precalculation or false alarm checking for one hash. Resume by pressing 'P' again. This pause option is different 
from the session/resume feature, as this just pauses a running job, you don't stop rcracki_mt this way.

If you are trying to crack a pwdump or Cain (.lst) file, containing both LM and NTLM hashes, rcracki_mt will try 
and crack the LM hashes. The result will be an uppercase password, which rcracki_mt will then try to correct with 
the right casing, using the NTLM hashes. If this fails it will try and perform Unicode correction, using a built-in 
mapping. If you happen to have an LM hash coupled with the wrong NTLM hash, this attempt to perform Unicode 
correction might take 'forever'. You can press 'S' to skip this step for the current hash.


HISTORY AND AUTHORS
================
rcracki_mt originally started as a modification of a modification (rcracki) of the original RainbowCrack (rcrack). 
These programs are all used to perform a rainbow table attack on password hashes, implementing Philippe Oechslin's 
faster time-memory trade-off technique.

Original rcrack code was written by Zhu Shuanglei <shuanglei@hotmail.com>.

Martin Westergaard J?rgensen <martinwj2005@gmail.com> wrote rcracki (improved) to support the rainbow tables 
generated by the distributed project www.freerainbowtables.com. These tables are perfected and indexed, making them 
faster and smaller. Rcracki also supported hybrid tables.

Dani?l Niggebrugge <neinbrucke> further enhanced this version and made it multi threaded, creating rcracki_mt. More 
features were added over time, making it less of an unofficial version with every release.

James Nobis - <quel> improved *nix compatibility and 64-bit compatability and
continues work on the project.


SUPPORTED HASH ALGORITHMS
================
Hash types supported by rcracki_mt are: LM, NTLM, MD2, MD4, MD5, DoubleMD5, SHA1, RIPEMD160, MSCACHE, MySQL323, 
MySQLSHA1, PIX, LMCHALL, HALFLMCHALL, NTLMCHALL, ORACLE

Actual indexed&perfected tables that were generated by the Free Rainbow Tables project: LM, MD5, NTLM, FASTLM, HALFLMCHALL, SHA1


SUPPORTED PLATFORMS
================
Rcracki_mt is released both as win32 binary and as source package. Rcracki_mt should work on any Microsoft Windows system, but is only tested on a 32 bit Windows XP. 

The source should work on Linux distributions.  It has been tested on:
32-bit Ubuntu
32-bit Debian GNU/Linux
64-bit Debian GNU/Linux

The source should also work on other platforms and has been tested on:
32-bit MacOSX

32-bit FreeBSD
64-bit FreeBSD
32-bit NetBSD
32-bit OpenBSD - you must install and use eg++ from ports
64-bit OpenBSD

Only compilation has been tested on:
64-bit MacOSX

Please note that to compile under the BSDs you must use gmake.

OpenBSD threading is a work in progress.

'OPTIONAL' TODO
================
- verification of an endpoint when restoring a chainwalkset from disk.
- read multiple chainwalksets from disk at once to try and speed up this process.
- read next table (part) from disk while doing cryptanalysis


LINKS
================
rcracki_mt @ SourceForge:		https://sourceforge.net/projects/rcracki/
Original rcrack:			http://www.antsight.com/zsl/rainbowcrack/
Free Rainbow Tables:			http://www.freerainbowtables.com/
My personal blog:			http://blog.distracted.nl/
Download free rainbow tables:		http://tbhost.eu/
Download free rainbow tables (mirror):	http://freerainbowtables.mirror.garr.it/mirrors/freerainbowtables/


THANKS
================
the_drag0n				Writing part of this README
<james.dickson@comhem.se>		Patch  to support Cain .lst files
Joao Inacio <jcinacio at gmail.com>	Supplying some faster algorithm implementations


FAQ
================
Q: Why do I get this message all the time? "this table contains hashes with length 8 only"
A: You are probably trying to crack LM hashes. You have to split up the hash in 2 parts of 16 hex characters each.

Q: rcracki_mt is so slow when I'm cracking 5000 hashes, why is that?
A: Rainbow table attacks are only useful for a certain amount of hashes, mainly because of the precalculations that 
are needed for every hash you are cracking. At a certain point it is faster to brute force the same key space then 
to try and use rainbow tables. Especially if you use a GPU enabled brute forcer, this limit might be reached very 
soon. Play around with these to find you limits.

Q: How can I speed up rcracki_mt?
A: This depends on quite some factors. If your jobs usually comprise of disk access time, you can try and speed up 
your storage. For example by using RAID and/or by using solid state disks. If you are trying to crack many hashes 
at the same time, you might be better off with buying a faster CPU.
