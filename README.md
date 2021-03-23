## Laboratory work III

<a href="https://yandex.ru/efir/?stream_id=vjKAlxJ0UQrs"><img src="https://raw.githubusercontent.com/tp-labs/lab03/master/preview.png" width="640"/></a>

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```sh
$ open https://cmake.org/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [ ] 3. Выполнить инструкцию учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=DimaZzZz101 # # Устанавливаем значение переменной окружения GITHUB_USERNAME.
```

```sh
$ cd ${GITHUB_USERNAME}/workspace # Переходим в рабочую директорию.
$ pushd . # создаем стек из директорий.
# Чтобы проверить работу команды, можно ввести команду dirs -v,
# которая покажет пронумерованый список директорий.
# MBP-Dmitrij:workspace mrrobot$ dirs -v
# 0  ~/DimaZzZz101/workspace
# 1  ~/DimaZzZz101/workspace

$ source scripts/activate # Выполняем команды из файла activate.
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03 # Клонируем репозиторий lab02 в каталог lab03.
$ cd projects/lab03 # Переходим в директорию lab03.
$ git remote remove origin # Отключение от ветки lab02.
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git # Переходим в ветку lab03.
```

```sh
$ g++  -std=c++11 -I./include -c sources/print.cpp
# -std=c++11 - устанавливаем стандарт языка
# -I./include - указываем каталог для поиска заголовочных файлов
# -c sources/print.cpp - создание объектного файла print.cpp

$ ls print.o # Показать print.0

# Ищем print в бинарном файле (до вертикальной черты - данные на вход,
# после - регулярное выражение, которое ищем). 
$ nm print.o | grep print
# Вывод:
# 0000000000000000 T __Z5printRKNSt3__112basic_stringIcNS_11char_traitsIcEENS_9allocatorIcEEEERNS_13basic_ostreamIcS2_EE
# 0000000000000080 T __Z5printRKNSt3__112basic_stringIcNS_11char_traitsIcEENS_9allocatorIcEEEERNS_14basic_ofstreamIcS2_EE

$ ar rvs print.a print.o # Создаем архив print.a и кладем в него print.o.

$ file print.a # Узнаем тип файла print.a.
# Вывод: print.a: current ar archive random library

$ g++ -std=c++11 -I./include -c examples/example1.cpp
# -std=c++11 - устанавливаем стандарт языка.
# -I./include - указываем каталог для поиска заголовочных файлов.
# -c examples/example1.cpp - создание объектного файла example1.cpp.

$ ls example1.o # Показать файл example1.o.

$ g++ example1.o print.a -o example1 # Компилируем example1.o и print.a.
                                      # Имя исполняемого файла example1.

$ ./example1 && echo # Запуск скомпилированного файла и вывод результата.
# Вывод: hello
```

```sh
$ g++ -std=c++11 -I./include -c examples/example2.cpp
# -std=c++11 - устанавливаем стандарт языка.
# -I./include - указываем каталог для поиска заголовочных файлов.
# -c examples/example2.cpp - создание объектного файла example2.cpp.

$ nm example2.o # Просмотр бинарного файла example.o

$ g++ example2.o print.a -o example2 # Компилируем example2.o и print.a.
                                      # Имя исполняемого файла example2.

$ ./example2 # Запуск скомпилированного файла.

$ cat log.txt && echo # Открываем файл log.txt и выводим в консоль результат.
# Вывод: hello
```

```sh
# Последовательно удаляем файлы: example1.o, example2.o, print.o, print.a, example1, example2, log.txt.
$ rm -rf example1.o example2.o print.o
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
```

```sh
# Настройка файла CMakeLists.txt путем записи в него следующего кода.
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF
```

```sh
# Установка параметров в файле CMakeLists.txt.
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

```sh
# Создание статической библиотека с именем print, исполняемый файл которой print.cpp.
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
```

```sh
# Подлючение заголовочных файлов.
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

```sh
# Компилируем и собираем проект.
$ cmake -H. -B_build
$ cmake --build _build
```

```sh
# Подключаем исполняемые файлы.
$ cat >> CMakeLists.txt <<EOF
add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

```sh
# Линковка исполняемых файлов с проектом.
$ cat >> CMakeLists.txt <<EOF
target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

```sh
$ cmake --build _build # Инициализация сборки проекта и его записи в директорию lab03/_build.
$ cmake --build _build --target print # Запуск сборки, цель - проект print.
$ cmake --build _build --target example1 # Запуск сборки, цель - проект example1.
$ cmake --build _build --target example2 #Запуск сборки, цель - проект example2.
```

```sh
$ ls -la _build/libprint.a # Просмотр содержимого libprint.a.
# Вывод: -rw-r--r--  1 mrrobot  staff  13728 23 мар 02:12 _build/libprint.a
$ _build/example1 && echo # Запуск example1 и вывод результата.
# Вывод: hello
$ _build/example2 # Запуск example2.
$ cat log.txt && echo # Открываем файл log.txt и выводим результат.
# Вывод: hello
$ rm -rf log.txt # Удаляем файл log.txt
```

```sh
$ git clone https://github.com/tp-labs/lab03 tmp # Клонируем репозиторий заданием ЛР в директорию tmp.
$ mv -f tmp/CMakeLists.txt . # Помещаем в tmp файл CMakeLists.txt.
$ rm -rf tmp # Удаляем директорию tmp.
```

```sh
$ cat CMakeLists.txt # Открываем файл CMakeLists.txt для чтения.
$ cmake -H. -B_build -D CMAKE_INSTALL_PREFIX=_install
# -H. - Установка директории, в которую будет генерироваться файл.
# -B_build - Указание директории для собираемых файлов.
# -D - Действие аналогичны команде set.

$ cmake --build _build --target install # Указание целей и последующая сборка проекта.

$ tree _install # Демострация результата.
# Вывод: 
# _install
#├── cmake
#│   ├── print-config-noconfig.cmake
#│   └── print-config.cmake
#├── include
#│   └── print.hpp
#└── lib
#    └── libprint.a

#3 directories, 4 files
```

```sh
$ git add CMakeLists.txt # Добвляем файл CMakeLists.txt в отслеживаемые.

$ git commit -m "added CMakeLists.txt" # Делаем коммит.

$ git push origin master # Отправляем на удаленный репозиторий в ветку master.
```

## Report

```sh
$ popd # Извлекаем директорию из вершины стека
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2020 The ISC Authors
```
