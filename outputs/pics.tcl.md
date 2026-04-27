#!/usr/bin/tclsh

#
# Program: pics.tcl -
#
# A TCL script that accumulates all the image files in a directory, builds an HTML page
# to display them, and writes the results out to a file, which is viewable in any browser
#
# Michael Walker   20Apr26   Initial Creation
#


    set file_list [list];  # Create a list for file types
    set file_list [glob "./*.jpg" "./*.jpeg" "./*.gif" "./*.png" "./*.webp" "./*.JPG" "./*.JPEG" "./*.GIF" "./*.PNG" "./*.WEBP"];  # Add all image types
    set file_list [lsort -ascii $file_list];  # Sort the list alphabetically

    set html "<html>\n<body bgcolor='#a8dcd2'>\n\n";  # Begin an HTML page
    append html "<center><b>Pictures in [pwd]</b></center>\n";  #Add a title
    append html "<table border=1 width='100%'>\n";  # Add a table

    set num_files [llength $file_list];  # Find how many file types to look for

    set row_count [expr int(sqrt($num_files))];  # Create a row counter
    if {$row_count <= 0}\
        {
        set row_count 1;  # Minimum rows is 1
        }
    if {$row_count > 6}\
        {
        set row_count 6;  # Max rows per page is 6
        }

    set pct [expr (100 / $row_count)];  # Set percentages of the rows per picture

    set num 0;
    foreach file $file_list\
        {
        # If you are at the end f the row, terminate that row with a tag
        if {[expr ($num % $row_count)] == 0}\
            {
            append html "<tr>\n";
            }

        incr num 1;

        # Calculate width percentages per picture
        if {[expr ($num % $row_count)] == 0}\
            {
            set width1 "";
            }\
        else\
            {
            set width1 " width='$pct%'";
            }

        # Caption each picture, with a maximum length
        if {[string length $file] > 22}\
            {
            set file_name "...[string range $file end-20 end]";
            }\
        else\
            {
            set file_name $file;
            }

        append html "<td$width1><center><a href=\"$file\">";  # Add a link to the picture
        append html "<img src='$file' width='100%'></a>\n";   # Add the file path
        append html "<br>$file_name</center></td>\n";         # Add the file name

        if {[expr ($num % $row_count)] == 0}\
            {
            append html "</tr>\n";  # terminate the row with a tag
            }
        }

    if {[expr ($num % $row_count)] != 0}\
        {
        append html "<td colspan=[expr ($row_count - ($num % $row_count))]>&nbsp;</td>"; # Span cols
        }

    append html "</tr>\n</table>\n\n";  # Terminate the table

    append html "</body>\n</html>\n\n";  # End the HTML page

    set fd [open "pics.html" "w"];  # Open a file for output
    puts $fd $html;  # Write out the HTML code
    close $fd;  # Close the file

#
# End the program
#