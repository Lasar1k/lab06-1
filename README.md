## Laboratory work VI

Данная лабораторная работа посвещена изучению средств пакетирования на примере **CPack**

```sh
$ open https://cmake.org/Wiki/CMake:CPackPackageGenerators
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab06** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=DimaZzZz101 # Задаем переменной окружения имя пользователя гитхаб
$ export GITHUB_EMAIL=my_e-mail # Задаем переменной окружения название почти пользователя гитхаб
$ alias edit=subl # По умолчанию указываем редактор sublime-text
$ alias gsed=sed # for *-nix system 
```

```sh
$ cd ${GITHUB_USERNAME}/workspace # Переходим в рабочую директорию
$ pushd . # Добавляем текущую директорию в виртуальный стек
$ source scripts/activate # Активируем скрипт
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab05 projects/lab06 # Клонируем репозиторий, созданный lab05 в директорию с lab06
# Клонирование в «projects/lab06»…
# remote: Enumerating objects: 55, done.
# remote: Counting objects: 100% (55/55), done.
# remote: Compressing objects: 100% (29/29), done.
# remote: Total 55 (delta 17), reused 55 (delta 17), pack-reused 0
# Получение объектов: 100% (55/55), 1.93 МиБ | 4.71 МиБ/с, готово.
# Определение изменений: 100% (17/17), готово.

$ cd projects/lab06 # Переходим директорию lab06
$ git remote remove origin # Удаляем ссылку на репозиторий с lab05
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06 # Добавляем ссылку с именем origin на созданный репозиторий lab06
```

```sh
# Настройка CPack через определение CPack-переменных внутри файла CMakeLists.txt Добавление истории версий для проекта: версия 0.1.0.0:
$ gsed -i "" '/project(print)/a\
set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")
' CMakeLists.txt
$ gsed -i "" '/project(print)/a\
set(PRINT_VERSION\
  \${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
' CMakeLists.txt
$ gsed -i "" '/project(print)/a\
set(PRINT_VERSION_TWEAK 0)
' CMakeLists.txt
$ gsed -i "" '/project(print)/a\
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt
$ gsed -i "" '/project(print)/a\
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
$ gsed -i "" '/project(print)/a\
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
# Сравниваем изменения в файлах
$ git diff
# diff --git a/CMakeLists.txt b/CMakeLists.txt
# index aa7a323..71b64e3 100644
# --- a/CMakeLists.txt
# +++ b/CMakeLists.txt
# @@ -7,6 +7,13 @@ option(BUILD_EXAMPLES "Build examples" OFF)
#  option(BUILD_TESTS "Build tests" OFF)
 
#  project(print)
# +set(PRINT_VERSION_MAJOR 0)
# +set(PRINT_VERSION_MINOR 1)
# +set(PRINT_VERSION_PATCH 0)
# +set(PRINT_VERSION_TWEAK 0)
# +set(PRINT_VERSION
# +  ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})
# +set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
 
#  add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
```

```sh
$ touch DESCRIPTION && edit DESCRIPTION # Создание файла DESCRIPTION и его открытие для редактирования - описание пакета
$ touch ChangeLog.md # Создание файла ChangeLog.md - список изменений в пакете
$ export DATE="`LANG=en_US date +'%a %b %d %Y'`" # Задание переменной DATE 
# Запись в файл со списком изменений 
$ cat > ChangeLog.md <<EOF 
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF
```

```sh
# Подключение необходимых системных библиотек
$ cat > CPackConfig.cmake <<EOF
include(InstallRequiredSystemLibraries)
EOF
```

```sh
# Установка значений переменных в пакете
$ cat >> CPackConfig.cmake <<EOF
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")
EOF
```

```sh
# Добавление файлов в пакет
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF
```

```sh
# Настройки для RPM-пакета
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF
```

```sh
# Настройки для Debian-пакета
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF
```

```sh
# Подключение модуля CPack
$ cat >> CPackConfig.cmake <<EOF

include(CPack)
EOF
```

```sh
# Добавление CPackConfig.cmake в основной CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF

include(CPackConfig.cmake)
EOF
```

```sh
$ gsed -i "" 's/lab05/lab06/g' README.md #
```

```sh
$ git add . # Добавляем созданные файлы в отслеживаемые 
$ git commit -m"added cpack config" # Даем название и делаем коммит
# [master 567d0b5] added cpack config
# 5 files changed, 36 insertions(+), 1 deletion(-)
# create mode 100644 CPackConfig.cmake
# create mode 100644 ChangeLog.md
# create mode 100644 DESCRIPTION
$ git tag v0.1.0.0 # Добавляем тэг с версией проекта
$ git push origin master --tags # Загружаем на удаленный репозиторий (вместе с тэгом)
```

```sh
$ travis login --auto # Логинимся в Travis
# Username: DimaZzZz101
# Password for DimaZzZz101: *************

# Successfully logged in as DimaZzZz101!
$ travis enable # Делаем проект доступным
# Detected repository as DimaZzZz101/lab06, is this correct? |yes| yes
# DimaZzZz101/lab06: enabled :)
```

```sh
# Сборка и генерация пакета через CPack
$ cmake -H. -B_build
# -- The C compiler identification is AppleClang 12.0.0.12000032
# -- The CXX compiler identification is AppleClang 12.0.0.12000032
# -- Detecting C compiler ABI info
# -- Detecting C compiler ABI info - done
# -- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc - skipped
# -- Detecting C compile features
# -- Detecting C compile features - done
# -- Detecting CXX compiler ABI info
# -- Detecting CXX compiler ABI info - done
# -- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ - skipped
# -- Detecting CXX compile features
# -- Detecting CXX compile features - done
# -- Configuring done
# -- Generating done
# -- Build files have been written to: /Users/mrrobot/DimaZzZz101/workspace/projects/lab06/_build

$ cmake --build _build
# [ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
# [100%] Linking CXX static library libprint.a
# [100%] Built target print

$ cd _build # Переходим в директорию_build
$ cpack -G "TGZ" # Архивируем пакет
# CPack: Create package using TGZ
# CPack: Install projects
# CPack: - Run preinstall target for: print
# CPack: - Install project: print []
# CPack: Create package
# CPack: - package: /Users/mrrobot/DimaZzZz101/workspace/projects/lab06/_build/print-0.1.0.0-Darwin.tar.gz generated.
$ cd .. # Поднимаемся на директорию выше
```

```sh
# Сборка и генерация пакета через CMake
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
# -- Configuring done
# -- Generating done
# -- Build files have been written to: /Users/mrrobot/DimaZzZz101/workspace/projects/lab06/_build

$ cmake --build _build --target package
# Consolidate compiler generated dependencies of target print
# [100%] Built target print
# Run CPack packaging tool...
# CPack: Create package using TGZ
# CPack: Install projects
# CPack: - Run preinstall target for: print
# CPack: - Install project: print []
# CPack: Create package
# CPack: - package: /Users/mrrobot/DimaZzZz101/workspace/projects/lab06/_build/print-0.1.0.0-Darwin.tar.gz generated.
```

```sh
$ mkdir artifacts # Создаем директорию artifacts
$ mv _build/*.tar.gz artifacts # Перемещаем в нее заархивированный пакет
$ tree artifacts # Демонстрация дерева директории artifacts
# artifacts
# ├── print-0.1.0.0-Darwin.tar.gz
# └── screenshot.png
#
# 0 directories, 2 files
```

## Report

```sh
$ popd # Вытаскиваем элемент из виртуального стека
$ export LAB_NUMBER=06 # Задаем переменной окружения номер лабораторной работы
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER} # Клонируем репозиторий из tp-labs в директорию текущей лабораторной работы в каталоге tasks
# Клонирование в «tasks/lab06»…
# remote: Enumerating objects: 3, done.
# remote: Counting objects: 100% (3/3), done.
# remote: Compressing objects: 100% (3/3), done.
# remote: Total 117 (delta 0), reused 0 (delta 0), pack-reused 114
# Получение объектов: 100% (117/117), 1.33 МиБ | 4.95 МиБ/с, готово.
# Определение изменений: 100% (34/34), готово.

$ mkdir reports/lab${LAB_NUMBER} # Создаем директорию для отчета по lab06
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md # Копируем с переименованием файл readme->report
$ cd reports/lab${LAB_NUMBER} # Переходим в директорию с отчетами в lab06
$ edit REPORT.md # Редактируем отчет
$ gist REPORT.md # Отправляем отчет в gist
```

## Homework

После того, как вы настроили взаимодействие с системой непрерывной интеграции,</br>
обеспечив автоматическую сборку и тестирование ваших изменений, стоит задуматься</br>
о создание пакетов для измениний, которые помечаются тэгами (см. вкладку [releases](https://github.com/tp-labs/lab06/releases)).</br>
Пакет должен содержать приложение _solver_ из [предыдущего задания](https://github.com/tp-labs/lab03#задание-1)
Таким образом, каждый новый релиз будет состоять из следующих компонентов:
- архивы с файлами исходного кода (`.tar.gz`, `.zip`)
- пакеты с бинарным файлом _solver_ (`.deb`, `.rpm`, `.msi`, `.dmg`)

В качестве подсказки:
```sh
$ cat .travis.yml
os: osx
script:
...
- cpack -G DragNDrop # dmg

$ cat .travis.yml
os: linux
script:
...
- cpack -G DEB # deb

$ cat .travis.yml
os: linux
addons:
  apt:
    packages:
    - rpm
script:
...
- cpack -G RPM # rpm

$ cat appveyor.yml
platform:
- x86
- x64
build_script:
...
- cpack -G WIX # msi
```

Для этого нужно добавить ветвление в конфигурационные файлы для **CI** со следующей логикой:</br>
если **commit** помечен тэгом, то необходимо собрать пакеты (`DEB, RPM, WIX, DragNDrop, ...`) </br>
и разместить их на сервисе **GitHub**. (см. пример для [Travi CI](https://docs.travis-ci.com/user/deployment/releases))</br>

## Links

- [DMG](https://cmake.org/cmake/help/latest/module/CPackDMG.html)
- [DEB](https://cmake.org/cmake/help/latest/module/CPackDeb.html)
- [RPM](https://cmake.org/cmake/help/latest/module/CPackRPM.html)
- [NSIS](https://cmake.org/cmake/help/latest/module/CPackNSIS.html)

```
Copyright (c) 2015-2020 The ISC Authors
```
