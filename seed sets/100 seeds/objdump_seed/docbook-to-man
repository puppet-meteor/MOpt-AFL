#!/bin/sh
#############################################################################
#
#	docbook-to-man.sh
#
#############################################################################
# 
# Copyright (c) 1996 X Consortium
# Copyright (c) 1996 Dalrymple Consulting
# 
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
# 
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
# 
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL THE
# X CONSORTIUM OR DALRYMPLE CONSULTING BE LIABLE FOR ANY CLAIM, DAMAGES OR
# OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
# ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
# OTHER DEALINGS IN THE SOFTWARE.
# 
# Except as contained in this notice, the names of the X Consortium and
# Dalrymple Consulting shall not be used in advertising or otherwise to
# promote the sale, use or other dealings in this Software without prior
# written authorization.
# 
#############################################################################
#
# Written 5/29/96 by Fred Dalrymple
#
#############################################################################

ROOT=/usr
SGMLS=$ROOT/share/sgml
DOCBOOK=$SGMLS/docbook/dtd/4.1

if test -x /usr/bin/nsgmls; then
    PARSER=/usr/bin/nsgmls
elif test -x /usr/bin/onsgmls; then
    PARSER=/usr/bin/onsgmls
else
    echo "error: SGML parser not found" 1>&2
fi
INSTANT=/usr/bin/instant
INSTANT_OPT=${INSTANT_OPT:-"-d"}

CATALOG=$DOCBOOK/docbook.cat
DECL=$DOCBOOK/docbook.dcl

error=false

if [ $# -eq 1 ]
then	INSTANCE=$1
else	error=true
fi

$error && echo "usage:  docbook-to-man docbook-instance" 1>&2 && exit 1

(#cat /tmp/dtm.$$.psinc;
 $PARSER -gl -m$CATALOG $DECL $INSTANCE |
	$INSTANT -croff.cmap -sroff.sdata -tdocbook-to-man.ts $INSTANT_OPT |
	sed 's/^[	 ]*//
	     s/$/ /
	     s/--/\\-\\-/g
	     s/^-/\\-/
	     s/\([^A-Za-z0-9\-]\)-/\1\\-/g' )
