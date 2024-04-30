FIND and touch
	
_You can use find command to find all your files and execute touch on every found file using -exec_

    find <path-to-dir> -type f -exec touch {} + &
_The & executes the command in the background_

If you want to filter your result only for text files, you can use

    find <path-to-dir> -type f -name "*.txt" -exec touch {} + &

Change terminal title
	
    PROMPT_COMMAND='echo -ne "SOME TITLE HERE"'
	
Create a symlink - link a folder or file somewhere else in your filesystem to a link in a directory of choice.
	
	ln -s /home/user/Documents/myfile.txt MyLink
