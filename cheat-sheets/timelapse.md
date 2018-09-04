# Timelapse with ffmpeg

Remove corrupt images

find . -name "*jpg" -exec jpeginfo -c {} \; | grep -E "WARNING|ERROR" | cut -d " " -f 1 | xargs rm

Generate file list

ls *.jpg | sed "s/^/file '/;s/$/'/" > files.txt

Generate video file

ffmpeg -f concat -i files.txt tl/output.mpeg
