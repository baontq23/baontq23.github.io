# How to Add a Download Counter to A Self-Hosted Cydia Repo
This is a fork of [kirb's repo download counter](https://gist.github.com/kirb/1922421), only that this one works and that one doesn't.

## Installation
1. Click the download link at the top of the gist page to save these files to your machine.
2. Unzip them and open `counter.php` and `nginx.conf` in your favorite [non-Notepad](http://en.wikipedia.org/wiki/Byte_order_mark "TL;DR version: Notepad adds a byte order mark to files it opens, which can cause Cydia to think there's a size mismatch.") text editor.
3. In counter.php, scroll down to the line that says `$db = mysqli_connect( ...` and change the parameters to the ones you use to connect to MySQL.
4. If your database doesn't use the UTF-8 charset, change the `UTF8` in the `if (!mysqli_set_charset($db, "UTF8")) ...` line to the appropriate value.
5. Go down to the `if (!mysqli_select_db($db, "myrepodb")) ...` line and change `myrepodb` to the name of your database.
6. Run the `downloads.sql` file in your MySQL database, changing `UTF8` if needed. In phpMyAdmin, you can use the import tab to do this.
7. Move your debs to a folder called `debs` in your repo folder.
8. In `nginx.conf`: Replce the `repo` with the path to the folder that your repo is in, relative to your document root. Place the rewrite rules to the root server block in the existing `/etc/nginx/nginx.conf` file on your server.
9. Make 100,000% sure there is not a byte order mark at the start of counter.php. On Windows, open counter.php in Notepad++ and choose _Convert to UTF-8 without BOM_ from the encoding menu. On Linux and Mac, pull up a terminal, `cd` to the folder the code is in and type: `awk '{if(NR==1)sub(/^\xef\xbb\xbf/,"");print}' counter.php > counter1.php && mv counter1.php counter.php` (note that this will cause problems if the file isn't in the UTF-8 charset)
10. Upload and enjoy :)