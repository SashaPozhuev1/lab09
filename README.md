<фрагмент_вставки_значка>
# lab04
## Laboratory work IV

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace - переходим в коталог.
$ pushd . - запоминаем текущий коталог.
$ source scripts/activate - выполняем сценарий.
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab03.git projects/lab04 - клонируем репозиторий предыдущей лабораторной работы.
$ cd projects/lab04 - переходм в репозиторий с новой лабораторной работой.
$ git remote remove origin - удаляем origin ( имя сервера с которого мы скопировали третью лабораторную работу ).
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04.git - добавляем add в новый репозиторий.
```

```ShellSession
$ g++ -I./include -std=c++11 -c sources/print.cpp -добавляем заголовки, выбираем стандарт 2011 года, компилируем, но не компануем его, получаем файл "print.o". 
$ ls print.o - проверяем.
$ nm print.o | grep print - выводим строки с информацией об исполняемом файле, в которых встречается "print". 
$ ar rvs print.a print.o - заменяем файл в архиве, выводим описание процедуры (добавили "print.o"), регенерировали таблицу имён архива.
$ file print.a - убедились, что print.a текущий архив.
$ g++ -I./include -std=c++11 -c examples/example1.cpp - добавили заголовочные файлы, определили стандарт, скомпилировали без компановки example1.cpp 
$ ls example1.o - посмотрели, что файл существует.
$ g++ example1.o print.a -o example1 - создаём исполняемый файл "example1". 
$ ./example1 && echo - если выполниться программа, выводим результат.
```

```ShellSession
$ g++ -I./include -std=c++11 -c examples/example2.cpp - компилируем
$ nm example2.o - выводим информацию.
$ g++ example2.o print.a -o example2 - компануем.
$ ./example2 - запускаем example2.
$ cat log.txt && echo - выводим результат.
```

```ShellSession
$ rm -rf example1.o example2.o print.o - удаление скомпилированных файлов.
$ rm -rf print.a - удаление архива.
$ rm -rf example1 example2 - удаление программ.
$ rm -rf log.txt - удаление текстового файла (получился после выполнения example2).
```

```ShellSession
$ cat > CMakeLists.txt <<EOF - заполняем CMakeLists.txt
cmake_minimum_required(VERSION 3.0) - указываем необходимую минимальную версию.
project(print) - задаём название проекта.
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11) - задаём переменную, активируем C++ 11.
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
add_library(print S/TATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp) - добавляем статичесую библиотеку.
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include) - добавляем путь к хедерам.
EOF
```

```ShellSession
$ cmake -H. -B_build - выводим информацию о выполнении, выполняем настройку.
$ cmake --build _build - создаём директорию проекта.
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp) - добавляем исполняемый файл example1.cpp.
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp) - добавляем исполняемый файл example2.cpp.
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print) - компануем исполняемый файл с библиотекой.
target_link_libraries(example2 print) - компануем исполняемый файл с библиотекой.
EOF
```

```ShellSession
$ cmake --build _build - создаём бинарное директорию проекта.
$ cmake --build _build --target print - добавляем print.
$ cmake --build _build --target example1 - добавляем example1.
$ cmake --build _build --target example2 - добавляем example2.
```

```ShellSession
$ ls -la _build/libprint.a - выводим на экран информацию об архиве (права доступа к файлу, количество ссылок на файл, имя владельца, имя группы, размер файла в байтах, временной штамп) и выдаём все файлы, включая скрытые.
$ _build/example1 && echo - выполняем example1.
hello
$ _build/example2 - выполняем example2.
$ cat log.txt && echo - выводим результат в log.txt.
hello
$ rm -rf log.txt - удаляем log.txt.
```

```ShellSession
$ git clone https://github.com/tp-labs/lab04 tmp - клониурем репозиторий.
$ mv -f tmp/CMakeLists.txt . - перемещаем CMakeLists.txt в текущий коталог.
$ rm -rf tmp - удаляем.
```

```ShellSession
$ cat CMakeLists.txt
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install запускаем cmake в _install
$ cmake --build _build --target install - делаем сборку.
$ tree _install - выводим содержимое коталога в виде дерева.
```

```ShellSession
$ git add CMakeLists.txt
$ git commit -m"added CMakeLists.txt"
$ git push origin master
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=04
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2017 Братья Вершинины
```

