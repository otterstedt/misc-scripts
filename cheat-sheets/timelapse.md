# Timelapse with ffmpeg

Remove corrupt images

<pre>
find . -name "*jpg" -exec jpeginfo -c {} \; | grep -E "WARNING|ERROR" | cut -d " " -f 1 | xargs rm
</pre>

Generate file list
<pre>
ls *.jpg | sed "s/^/file '/;s/$/'/" > files.txt
</pre>
Generate video file
<pre>
ffmpeg -f concat -i files.txt tl/output.mpeg
</pre>
