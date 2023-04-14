To save the file with the same name as the original source file on the remote server, use –O (uppercase O) followed by url.

`curl –O [URL]`
<br><br>

To download the file and save it in a different name than the name of the file in the remote server, use -o (lower-case o).

`curl –o [filename] [URL]`
<br><br>

To download multiple files.

`curl -O [URL1] -O [URL2]`
<br><br>

To download a file from FTP server.

`curl -O ftp://ftp.example.com/file.zip`
