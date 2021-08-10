#!/bin/bash

# variables
pth=$PWD
maxdepth=""

# functions
function print_usage ()
{
	echo 'USAGE:'
	echo 'fif <options> "<search-string>"'
	echo ''
	echo "<options>:"
	echo "    -L <num>  Maximum depth of the search."
	echo "    -p <pth>  Starting path for the search"
	echo "    -F <pat>  Pattern to filter files to be searched"
	echo "    -A <num>  NUM lines after match are printed"
	echo "    -B <num>  NUM lines before match are printed"
	echo "    -C        Case sensitive"
}

function find_in_file
{
	file=$1
	if [ -z $caseSensitive ]; then
		result=$(cat -n $file | GREP_COLOR='30;47' grep --ignore-case -A $grepA -B $grepB --color=always -E "$search_string")
	else
		result=$(cat -n $file | GREP_COLOR='30;47' grep -A $grepA -B $grepB --color=always -E "$search_string")
	fi
	if ! [ -z "$result" ]; then
		echo "[f]=$file" | GREP_COLOR='30;47' grep --color=always -E "$grepF"
		echo "$result"
	fi
}


# check parameters;
while [[ $# -gt 0 ]]; do
	case "$1" in
		-p)
			# shift if not last parameter
			if [[ $# -eq 1 ]]; then
				echo "$1 requires a specification. Try --help."
				exit 1
			fi
			shift

			# shifted -> new $1; should be a directory/file path
			if [ -f "$1" ]; then
				# if the path points to a file just search in that file
				file=$1
			else
				pth=`( cd "$1" && pwd )`
				if ! [ -d $pth ]; then
					echo "-p expected a path to a directory.\nReceived <$1> instead."
					exit 1
				fi
			fi
			;;
		-L)
			# shift if not last parameter
			if [[ $# -eq 1 ]]; then
				echo "-L option requires a specification. Try --help."
				exit 1
			fi
			shift

			# shifted -> new $1; should be a number
			maxdepth=$1
			numrgx="^[0-9]+$"
			if ! [[ $maxdepth =~ $numrgx ]]; then
				echo "-L option requires a number. Try --help."
				exit 1
			fi
			;;
		-F)
			# shift if not last parameter
			if [[ $# -eq 1 ]]; then
				echo "-F option requires a specification. Try --help."
				exit 1
			fi
			shift

			# shifted -> new $1; should be a number
			grepF="$1"
			;;
		-A)
			# shift if not last parameter
			if [[ $# -eq 1 ]]; then
				echo "-L option requires a specification. Try --help."
				exit 1
			fi
			shift

			# shifted -> new $1; should be a number
			grepA=$1
			numrgx="^[0-9]+$"
			if ! [[ $grepA =~ $numrgx ]]; then
				echo "-A option requires a number. Try --help."
				exit 1
			fi
			;;
		-B)
			# shift if not last parameter
			if [[ $# -eq 1 ]]; then
				echo "-L option requires a specification. Try --help."
				exit 1
			fi
			shift

			# shifted -> new $1; should be a number
			grepB=$1
			numrgx="^[0-9]+$"
			if ! [[ $grepB =~ $numrgx ]]; then
				echo "-A option requires a number. Try --help."
				exit 1
			fi
			;;
		-C)
			caseSensitive=1
			;;
		-h | --help)
			print_usage
			exit 0
			;;
		*)
			if [[ $# -eq 1 ]]; then
				search_string=$1
			else
				echo "<$1> not an option. Try --help."
				exit 1
			fi
			;;
	esac
	# shift out parsed option
	shift
done


# set A and B to zero if not set
if [ -z $grepA ]; then
	grepA=0
fi
if [ -z $grepB ]; then
	grepB=0
fi
if [ -z $grepF ]; then
	grepF=""
fi



# script
if ! [ -z $file ]; then
	# if the path points to a file just search in that file
	find_in_file $file
elif [ -z $maxdepth ]; then
	# if no maxdepth is specified
	for file in $(find $pth -type f | grep -E "$grepF")
	do
		find_in_file $file
	done
else
	for file in $(find $pth -maxdepth $maxdepth -type f | grep -E "$grepF")
	do
		find_in_file $file
	done
fi

echo "done"
exit 0


