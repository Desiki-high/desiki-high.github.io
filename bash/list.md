## 收藏一些实用的shell脚本

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
### 操作系统上机实验一(助教)
```bash
#!/usr/bin/env bash

if [ -d osdir ];then
 rm -rf osdir;
fi

mkdir osdir;cd osdir;

free -h > f1;

touch f2;chmod +x f2;
realpath f2 > f2;

ls -lh > f3;

echo "#include<stdio.h>

int main(){
    printf(\"%s\n\",\"Hello Wold\");
    return 0;
}
" > hello.c

gcc hello.c -o hello; ./hello
```