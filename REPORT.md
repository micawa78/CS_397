Michael Walker
CS 397, Seminar 2, Spring 2026


Assignment #3, Document Intelligence Pipeline

Part 3:

Analysis and Documentation

Tool evaluation:

	The tools I used were built into the system (I use a Mac): Preview and
	Automator. They are both almost trivial to use, and, as far as I can tell,
	nearly flawless. The PDFs were fed in, and the code and images popped out 
	the other end, with no issues.  Apple products are very good that way.

Failure:

	I couldn't find anything that didn't work.  I tried the process on a couple
	of dozen different PDFs, and they all sailed right through without difficulty.
	Of course, the number of different file formats that I use is typically kind
	of limited (code, images, archives, etc.) so I suppose there might be some
	formats that would be unrecognized, but I wasn't able to find any.

Real-world applications:

	That's hard. There really isn't any reason to take a PDF apart; if you want
	to edit it, just drop it in a PDF editor and go at it.  Of course it would be
	easier to simply use the original component files themselves, rather than
	trying to dissect the output.  Even if you had deleted the originals, you
	could simply recover them from Time Machine.

	I guess if you had deleted all your originals, and wiped out the backup 
	drive, and had destroyed the hard copy, then maybe that might be a
	justification.  Of course, if you had gone to that much trouble to destroy 
	all the code, then you probably wouldn't have left an intact PDF sitting
	around, anyway.  So I'm baffled.

Design decision:

	When I wrote the script, I had to decide just how small the thumbnails could
	get before they became unviewably small. This would be a consideration on
	directories that had many dozens or hundreds of images in them, so I made 
	an arbitrary decision to limit the size to 6 images wide. If you have better
	eyes than I do, this could be adjusted.

