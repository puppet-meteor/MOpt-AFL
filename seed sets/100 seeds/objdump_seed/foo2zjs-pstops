#!/bin/sh

VERSION='$Id: foo2zjs-pstops.sh,v 1.21 2013/07/31 17:41:47 rick Exp $'

PROGNAME="$0"

usage() {
	cat <<EOF
NAME
    `basename $PROGNAME` - Add PS code for foo2*-wrapper

SYNOPSIS
    `basename $PROGNAME` [options] [file]

DESCRIPTION
    Add PS code for foo2zjs-wrapper.

OPTIONS
    -9		Use new gs (ver >= 9.00)
    -h ydimpts	For rotate -r, y dimension points
    -r		Rotate 90 clockwise
    -a		Accurate Screens code
    -c		CIEColor
    -n		Neuter CUPS cupsPSLevel2
    -w		Well Tempered Screens code
    -D lvl	Debug level
EOF

	exit 1
}

#
#       Report an error and exit
#
error() {
	echo "`basename $PROGNAME`: $1" >&2
	exit 1
}

debug() {
	if [ $DEBUG -ge $1 ]; then
	    echo "`basename $PROGNAME`: $2" >&2
	fi
}

#
#	Use gsed instead of sed on Mac OSX
#
case `uname -s` in
Darwin)		sed=gsed;;
FreeBSD)	sed=gsed;;
*)		sed=sed;;
esac

#
#       Process the options
#
DEBUG=0
ROTATE90=0
ACCURATE=0
CIECOLOR=0
NIXCUPS=0
WTS=0
GSVER=old
while getopts "9ach:nwrD:Vh?" opt
do
	case $opt in
	9)	GSVER=gs9;;
	a)	ACCURATE=1;;
	c)	CIECOLOR=1;;
	h)	YDIMpoints="$OPTARG";;
	n)	NIXCUPS=1;;
	r)	ROTATE90=1;;
	w)	WTS=1;;
	D)	DEBUG="$OPTARG";;
	V)	echo "$VERSION"; exit 0;;
	h|\?)	usage;;
	esac
done
shift `expr $OPTIND - 1`

if [ $NIXCUPS = 1 ]; then
    n='s#^[^/]*cupsPSLevel2#false#'
else
    n=
fi

if [ $ROTATE90 = 1 ]; then
	    # %%currentpagedevice /PageSize get\
	    # %%aload pop translate\
    r="1i\
	<< /Install {\
	    0 $YDIMpoints translate\
	    -90 rotate\
	} >> setpagedevice
	"
else
    r=
fi

if [ "$GSVER" = "old" ]; then
    ht='\
        <<\
            /AccurateScreens true\
            /HalftoneType 1\
            /HalftoneName (Round Dot Screen) cvn\
            /SpotFunction { 180 mul cos exch 180 mul cos add 2 div}\
            /Frequency 137\
            /Angle 37\
        >> sethalftone
    '
else
    ht='\
	/SpotDot { 180 mul cos exch 180 mul cos add 2 div } def\
	<<\
	    /HalftoneType 5\
	    /Cyan <<\
		/HalftoneType 1\
		/AccurateScreens true\
		/Frequency 150\
		/Angle 105\
		/SpotFunction /SpotDot load\
	    >>\
	    /Magenta <<\
		/HalftoneType 1\
		/AccurateScreens true\
		/Frequency 150\
		/Angle 165\
		/SpotFunction /SpotDot load\
	    >>\
	    /Yellow <<\
		/HalftoneType 1\
		/AccurateScreens true\
		/Frequency 150\
		/Angle 30\
		/SpotFunction /SpotDot load\
	    >>\
	    /Black <<\
		/HalftoneType 1\
		/AccurateScreens true\
		/Frequency 150\
		/Angle 45\
		/SpotFunction /SpotDot load\
	    >>\
	    /Default <<\
		/HalftoneType 1\
		/AccurateScreens true\
		/Frequency 150\
		/Angle 37\
		/SpotFunction /SpotDot load\
	    >>\
	>> /Default exch /Halftone defineresource sethalftone
    '
fi

if [ $WTS = 1 ]; then
    w="/%%Page:[        ]*[     (]1[    )].*\$/ i\
	<< /UseWTS true >> setuserparams \
	$ht
	"
elif [ $ACCURATE = 1 ]; then
    w="/%%Page:[        ]*[     (]1[    )].*\$/ i\
	<< /UseWTS false >> setuserparams \
	$ht
	"
else
    w=
fi

#
#	Main Program
#
$sed \
    -e "$w" \
    -e "$n" \
    -e "$r" \
    $@
