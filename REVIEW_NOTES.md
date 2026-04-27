Michael Walker
CS 397, Seminar 2, Spring 2026

Assignment #3, Document Intelligence Pipeline


Part 2:

Reviewer Notes:

The input files used are in the ~/inputs directory, and consist of three PDF files:
	cputype.asm.pdf
	mutex.asm.pdf
	pics_tcl_all.pdf

The output files generated are in the ~/outputs directory and consist of
	cputype.asm.md	<== contains code from cputype.asm
	mutex.asm.md	<== contains code from mutex.asm
	pics.tcl.md	<== contains code from pics.tcl
	pics.html.pdf	<== contains images from pics.html

Normallly, the output files would be named with their res[ective file extensions, but the instructions said to mane them as ".md", so I did.

Known limitations:

	The script cannot handle files of unknown image types. If new types exist,
	they must be manually added to the file type list.

Questions for reviewer:

	1) TIFF files are not searched for, since they are not, strictly speaking,
	image files, even though image data could, in theory, be placed in them. 
	Should I add those to the list of files detected?

	2) Should I compile the Tcl scrit?  This would make it a tiny bit smaller,
	and faster to execute, but would lso prevent editing or modification (which
	might be either a good or bad thing, depending on your viewpoint.)

Feedback:

	I don't reallly know what to ask for; is the code understndable? Can you 
	read it easily?  Do you see any bugs?
