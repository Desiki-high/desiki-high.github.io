### 代码统计
```bash
#!/usr/bin/env bash

read -p "Enter file extension(s) to count lines (separated by space): " extensions

if [ -z "$extensions" ]; then
  echo "Please provide at least one file extension."
  exit 1
fi

find_extensions=""
for ext in $extensions
do
  find_extensions="$find_extensions -o -name \"*.$ext\""
done
find_extensions=$(echo $find_extensions | sed 's/^ -o //')

eval "find . ${find_extensions#* } | xargs wc -l"
```