#! /usr/bin/env bash

#########################################################################################
# I put this script together to help with processing mkv files via mkclean for enhanced #
# file optimisation. I also wrote it to catch errors that can occur with unicode charac #
# ers. There may be an update to mkclean in the future but at this point it is easier   #
# to catch the potential errors in a separate folder.                                   #
# Software is distributed ASIS.                                                         #
# Raise an issue if you find a bug and I will do my best to fix it.                     #
#                                                                                       #
# To run this code on a mac you need to have:                                           #
# - installed home brew (https://brew.sh/)                                              #
# - set the correct directories below and use the command line                          #
#########################################################################################


brew install mkclean

#### put your folder with the location of the script and mkv files here

location=/Volumes/

errordir="$location/Error"

function pause(){
	
	echo "please press any key to continue";
	read -p "$*";
}

{
for D in $location/* 
	do
	exclusion=$(basename $D)
	[[ $exclusion =~ ^(Error)$ ]] && continue
	if [ -d "${D}" ]; then
		currentDate=`date`
		echo ""
		echo "$currentDate"
		echo "--------------------------------------------------------------------"
		echo "Entering $D, processing"
			echo ""
			echo "STEP 1 ------------------------------------"
			echo "NOTE: clearing extra files from $D"
			find "$D" -name "*.bif" -exec rm -f {} \;
			find "$D" -name "*.jpg" -exec rm -f {} \;
			find "$D" -name "*.png" -exec rm -f {} \;
			find "$D" -name "*.nfo" ! -name "season.nfo" ! -name "tvshow.nfo" -type f -exec rm -f {} \;
			echo ""
			echo "NOTE: clearing done"
			echo ""
			echo ""
			echo "STEP 2 ------------------------------------"
			echo "processing file types other than mkv files"
			echo ""
			echo ""	
			echo "STEP 2.1 ----------------------------------"
			echo "NOTE: moving on to processing mp4 files"
			if find "$D" -name "*.mp4" ! -name "*clean.*" | read; then
				echo ""
				echo "NOTE: mp4 files found"
				find "$D" -name "*.mp4" ! -name "*clean.*" -print0 |while read -d $'\0' file;
				do
					echo ""
					echo "Processing "$file""
					mkvmerge -o "/${file%.mp4}.mkv" "$file"
					rm -f "$file";
				done
				echo ""
				echo "NOTE: mp4 conversion in $D done"
			else
				echo ""
				echo "NOTE: no mp4 files to process, moving on"
			fi
			echo ""
			echo ""	
			echo "STEP 2.2 ----------------------------------"
			echo "NOTE: moving on to processing m4v files"
			if find "$D" -name "*.m4v" ! -name "*clean.*" | read; then
				echo ""
				echo "NOTE: m4v files found"
				find "$D" -name "*.m4v" ! -name "*clean.*" -print0 |while read -d $'\0' file;
				do
					echo ""
					echo "Processing "$file""
					mkvmerge -o "/${file%.m4v}.mkv" "$file"
					rm -f "$file";
				done
				echo ""
				echo "NOTE: m4v conversion in $D done"
			else
				echo ""
				echo "NOTE: no m4v files to process, moving on"
			fi
			echo ""
			echo ""
			echo "STEP 3 ------------------------------------"
			echo "NOTE: now moving on to processing mkv files"
			if find "$D" -name "*.mkv" ! -name "*clean.*" | read; then
				echo ""
				echo "NOTE: mkv files found"
				find "$D" -name "*.mkv" ! -name "*clean.*" -print0 | while read -d $'\0' file
				do
					echo ""
					echo "Processing "$file""
					mkclean --optimize "$file"
					if [ $? == 254 ]
					then
						[ -d /"$errordir" ] || mkdir -p -- "$errordir"
						echo "something went wrong"
						echo ""
						echo $file
						mv "$file" "${errordir}/Error.${file##*/}"
            pause
					else
						echo $?
						rm -f "$file"
					fi	
				done
				echo ""
				echo "NOTE: mkv processing in $D done"
				echo ""
				echo ""
				echo "STEP 3.1 ----------------------------------"
				echo "NOTE: renaming previously processed mkv files"
				if find "$D" -name "*clean.*" | read; then
					echo ""
					echo "NOTE: mkv files found to rename"
					find "$D" -name "*clean.*" -print0| while read -d $'\0' file
					do
						echo "" 
						echo "Processing "$file""
						mv "$file" "${file/clean.}";
						echo "Processing "{$file/clean.}""
					done
				else
					echo "no renaming needed"
				fi
				echo ""
				echo "NOTE: renaming done"
				echo ""
				echo ""
				echo "STEP 3.2 ----------------------------------"
				echo "NOTE: final step, make sure that all the metadata is ok"
				find "$D" -name "*.mkv" ! -name "*ERROR.*" -print0 | while read -d $'\0' file
				do
					echo ""
					echo "Processing "$file""
					echo ""
					mkvpropedit "$file" -d title;
					echo ""
					mkvpropedit "$file" -d date;
					echo ""
					mkvpropedit "$file" --edit track:2 --set flag-default=1;
					echo ""
					mkvpropedit "$file" --edit track:3 --set flag-default=0;
					echo ""
					mkvpropedit "$file" --edit track:4 --set flag-default=0;
					echo ""
					mkvpropedit "$file" --edit track:5 --set flag-default=0;
					echo ""
					mkvpropedit "$file" --edit track:6 --set flag-default=0;
					echo ""
					mkvpropedit "$file" --edit track:7 --set flag-default=0;
					echo ""
					mkvpropedit "$file" --edit track:8 --set flag-default=0;
				done
			else
				echo ""
				echo "NOTE: no mkv files to process, moving on"
		fi
	echo ""
	echo "exiting $D, done processing"
	echo "--------------------------------------------------------------------"
	echo ""
	fi
done
} | tee -a $location/output.txt
