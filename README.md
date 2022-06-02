# mkclean-script


I put this script together to help with processing mkv files via mkclean for enhanced file optimisation for those who are not very comfortable running/writing scripts. I also wrote it to catch errors that can occur with unicode characters that are not supported yet by mkclean 0.9.0 (e.g. é). There may be an update to mkclean in the future but at this point it is easier to catch the potential errors in a separate folder. The script also deletes all files of type *.bif, *.jpg, *.png, *.nfo (with exception of .nfo files about the tvshow or season the folder mkv file is in. This is aimed at emby folder structure). The script then checks if there are *.mp4 or *.m4v files and converts them to *.mkv. As a final step, the script also cleans the mkv file's meta data (title, date, set track 2 to default).

Software is distributed ASIS.                                                         
Raise an issue if you find a bug and I will do my best to fix it.                     

## What you need to do to run this script:

1. download the script (scripot.sh)
2. open a terminal window (on mac go to applications/utilities); I recommend iterm2 (https://iterm2.com/)
3. install home brew if not already installed (https://brew.sh/)
4. have a folder with the mkv/movie files you want to optimise/convert in their respective subfolders (be aware of other file types, see above)
  e.g. folder test contains tv show Insurgence, Seasons 1-3:
  ```
    test/Insurgence
        tvshow.nfo
        test/Insurgence/Season 1/
            Episode 1.mkv
            Episode 1.jpg
            Episode 1.bif
            Episode 2.mkv
            Episode 2.jpg
            Episode 2.bif
            season.nfo
        test/Insurgence/Season 2/
            ...
        test/Insurgence/Season 3/
            ...
```

5. place the script insdie the parent folder. In this example 'test'
6. update the location of where the script resides inside the script (l. 21), by opening it with an editor such as TextEdit or BBEdit.
7. go to your terminal window and type
```bash ```, followed by the path to the file (Note: you can also drag the script file into the terminal window and it will grab the path automatically)
8. press ```enter```

The script will now attempt to run. If it can not process a file, it will put it into the error folder and rename it. It will also generate a log file to check later.

Please check the script on your system with a test folder and files first.
