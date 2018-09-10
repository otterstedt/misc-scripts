Replace character in file names

<pre>
find . -name "*:*" -exec rename 's|:|-|g' {} \;
</pre>
