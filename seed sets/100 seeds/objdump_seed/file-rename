#!/usr/bin/perl -w

eval 'exec /usr/bin/perl -w -S $0 ${1+"$@"}'
    if 0; # not running under some shell
# $Revision: 331 $$Date: 2013-04-30 21:23:41 +0100 (Tue, 30 Apr 2013) $
# Robin's RCS header:
# RCSfile: rename.PL,v Revision: 1.3   Date: 2006/05/25 09:20:32 
# Larry's RCS header:
#  RCSfile: rename,v   Revision: 4.1   Date: 92/08/07 17:20:30 
#
#  Log: rename,v 
# Revision 1.5  1998/12/18 16:16:31  rmb1
# moved to perl/source
# changed man documentation to POD
#
# Revision 1.4  1997/02/27  17:19:26  rmb1
# corrected usage string
#
# Revision 1.3  1997/02/27  16:39:07  rmb1
# added -v
#
# Revision 1.2  1997/02/27  16:15:40  rmb1
# *** empty log message ***
#
# Revision 1.1  1997/02/27  15:48:51  rmb1
# Initial revision
#

use strict;
use File::Rename ();
use Pod::Usage;

main() unless caller;

sub main {
    my $options = File::Rename::Options::GetOptions
        or pod2usage;

    mod_version() if $options->{show_version};
    pod2usage( -verbose => 2 ) if $options->{show_manual};
    pod2usage( -exitval => 1 ) if $options->{show_help};

    @ARGV = map {glob} @ARGV if $^O =~ m{Win}msx;

    File::Rename::rename(\@ARGV, $options);
}

sub mod_version {
    print __FILE__ .
	' using File::Rename version '.
        $File::Rename::VERSION ."\n\n";
    exit 0
}   

1;

__END__

=head1 NAME

rename - renames multiple files

=head1 SYNOPSIS

B<rename>
S<[ B<-h>|B<-m>|B<-V> ]>
S<[ B<-v> ]>
S<[ B<-n> ]>
S<[ B<-f> ]>
S<[ B<-e>|B<-E> I<perlexpr>]*|I<perlexpr>>
S<[ I<files> ]>

=head1 DESCRIPTION

C<rename>
renames the filenames supplied according to the rule specified as the
first argument.
The I<perlexpr> 
argument is a Perl expression which is expected to modify the C<$_>
string in Perl for at least some of the filenames specified.
If a given filename is not modified by the expression, it will not be
renamed.
If no filenames are given on the command line, filenames will be read
via standard input.

For example, to rename all files matching C<*.bak> to strip the extension,
you might say

	rename 's/\e.bak$//' *.bak

To translate uppercase names to lower, you'd use

	rename 'y/A-Z/a-z/' *

=head1 OPTIONS

=over 8

=item B<-v>, B<-verbose>

Verbose: print names of files successfully renamed.

=item B<-n>, B<-nono>

No action: print names of files to be renamed, but don't rename.

=item B<-f>, B<-force>

Over write: allow existing files to be over-written.

=item B<-h>, B<-help>

Help: print SYNOPSIS and OPTIONS.

=item B<-m>, B<-man>

Manual: print manual page.

=item B<-V>, B<-version>

Version: show version number.

=item B<-e>

Expression: code to act on files name.

May be repeated to build up code (like C<perl -e>).
If no B<-e>, the first argument is used as code.

=item B<-E>

Statement: code to act on files name, as B<-e> but terminated by ';'.

=back

=head1 ENVIRONMENT

No environment variables are used.

=head1 AUTHOR

Larry Wall

=head1 SEE ALSO

mv(1), perl(1)

=head1 DIAGNOSTICS

If you give an invalid Perl expression you'll get a syntax error.

=head1 BUGS

The original
C<rename>
did not check for the existence of target filenames,
so had to be used with care.
I hope I've fixed that (Robin Barker).

=cut

