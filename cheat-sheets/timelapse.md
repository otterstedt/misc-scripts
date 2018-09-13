# Timelapse with ffmpeg

Remove corrupt images

<pre>
find . -name "*jpg" -exec jpeginfo -c {} \; | grep -E "WARNING|ERROR" | cut -d " " -f 1 | xargs rm
</pre>

Replace ':'

<pre>
find . -name "*:*" -exec rename 's|:|-|g' {} \;
</pre>

Generate file list
<pre>
ls *.jpg | sed "s/^/file '/;s/$/'/" > files.txt
</pre>
Generate video file
<pre>
ffmpeg -f concat -i files.txt tl/output.mpeg
</pre>

# Image manipulation

Image overlay
<pre>
composite <-blend 30> 1.jpg 2.jpg res.jpg
</pre>

# Static maps

Tileserver GL
<pre>
http://localhost:8080/styles/klokantech-basic/static/-73.655,45.496,13@0,0/160x160.png
</pre>
