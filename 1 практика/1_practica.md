# Задание 5
Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin).
```
nano reg.sh
```
```
chmod +x reg.sh
```
```
./reg.sh 
```
В фалйе reg.sh
```
#!/bin/bash
if [ -z "$1" ]; then
    echo "Использование: $0 <путь к файлу>"
    exit 1
fi

FILE="$1"

if [ ! -f "$FILE" ]; then
    echo "Ошибка: файл '$FILE' не найден."
    exit 1
fi

chmod +x "$FILE"

sudo cp "$FILE" /usr/local/bin/

if [ $? -eq 0 ]; then
    echo "Файл '$FILE' успешно зарегистрирован и скопирован в /usr/local/bin."
else
    echo "Ошибка при копировании файла."
    exit 1
fi
```
Результат

<img width="492" alt="image" src="https://github.com/user-attachments/assets/14de14c7-934f-4afc-90c5-8809ff50b185">


# Задание 6
Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.

```
nano checker
```
```
chmod +x checker
```
```
./checker 
```
В фалйе checker.sh
```
#!/bin/bash

SOURCE_FILE="$PWD/$1"
LINE=$(head -n1 $SOURCE_FILE)

if [[ $1 == *.c && $(echo $LINE | grep -E "\/\/|\/\*" 2>/dev/null) ]]; then
echo "В первой строке есть комментарий"
elif [[ $1 == *.py && $(echo $LINE | grep -E "#" 2>/dev/null) ]]; then
echo "Found a comment"
elif [[ $1 == *.js && $(echo $LINE | grep -E "\/\/" 2>/dev/null) ]]; then
echo "В первой строке есть комментарий"
else
echo "В первой строке нет комментариев"
fi
```
В файле file.cpp
```
//comm
#include <iostream>
using namespace std;
int main(){
    cout<<"Hello";
}
```
Результат

<img width="244" alt="image" src="https://github.com/user-attachments/assets/a0558998-3500-4129-8622-e3ba7c305612">

# Задание 7
Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам). 
```
mkdir new_dir (директория)
```
```
move text1.txt text2.txt text3.txt (директория)
```
```
nano finder.sh
```
```
chmod +x finder.sh
```
```
./finder.sh (директория)
```
В файле finder.sh
```
#!/bin/bash
SEARCH_DIR=$1
if [ -z "$SEARCH_DIR" ]; then
    echo "Использование: $0 <путь_к_каталогу>"
    exit 1
fi

declare -A file_hash_map

find "$SEARCH_DIR" -type f | while read -r file; do
    hash=$(sha256sum "$file" | awk '{ print $1 }')
    if [[ -n "${file_hash_map[$hash]}" ]]; then
        echo "Дубликат найден: $file и ${file_hash_map[$hash]}"
    else
        file_hash_map["$hash"]="$file"
    fi
done
```
Результат

<img width="388" alt="image" src="https://github.com/user-attachments/assets/69179c89-1290-4fbf-a01f-bdf2d684491e">

# Задание 8
Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.
```
nano archi.sh
```
```
chmod +x archi.sh
```
```
./archi.sh txt
```
В файле archi.sh
```
#!/bin/bash
if [ $# -ne 2 ]; then
  echo "Использование: $0 <расширение_файлов> <имя_архива>"
  exit 1
fi

extension="$1"
archive_name="$2"
files=$(find . -maxdepth 1 -type f -name "*.$extension")

if [ -z "$files" ]; then
  echo "Файлов с расширением .$extension не найдено."
  exit 1
fi

tar -czvf "$archive_name.tar.gz" $files
echo "Файлы с расширением .$extension были архивированы в $archive_name.tar.gz"
```
Результат

<img width="425" alt="image" src="https://github.com/user-attachments/assets/7c085ef6-6287-4335-8e87-bbd86790642e">

# Задание 9
Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.
```
echo -e "Здесь    4 пробела.\nЗдесь кстати    тоже." > input.txt
```
```
touch output.txt
```
```
nano changer.sh
```
```
chmod +x changer.sh
```
```
chmod +x changer.sh
```
В файле changer.sh
```
#!/bin/bash
if [ $# -ne 2 ]; then
  echo "Использование: $0 <входной_файл> <выходной_файл>"
  exit 1
fi

input_file="$1"
output_file="$2"

if [ ! -f "$input_file" ]; then
  echo "Входной файл не найден: $input_file"
  exit 1
fi

sed 's/    /\t/g' "$input_file" > "$output_file"
echo "Замены произведены. Результат записан в файл: $output_file"
```
Результат

<img width="410" alt="image" src="https://github.com/user-attachments/assets/d4932299-0b78-4846-bae4-3f518002c1aa">

# Задание 10
Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром.
```
mkdir new_dir
```
```
touch new_dir/file1.txt new_dir/file2.txt new_dir/file3.txt
```
```
nano empty.sh
```
```
chmod +x empty.sh
```
```
./empty.sh new_dir
```

В файле empty.sh
```
#!/bin/bash
if [ $# -ne 1 ]; then
  echo "Использование: $0 <директория>"
  exit 1
fi

directory="$1"

if [ ! -d "$directory" ]; then
  echo "Директория $directory не существует."
  exit 1
fi

find "$directory" -type f -name "*.txt" -size 0 -print
```
Результат 

<img width="220" alt="image" src="https://github.com/user-attachments/assets/b12da2e5-7fb8-4661-ad1a-ef736fea4d5a">










