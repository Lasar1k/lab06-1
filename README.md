## Laboratory work II

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab02** и с лиценцией **MIT**
- [x] 2. Сгенирировать токен для доступа к сервису **GitHub** с правами **repo**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Выполнить инструкцию учебного материала
- [x] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial


```sh
# Задаем переменные окружения
$ export GITHUB_USERNAME=DimaZzZz101 # задаем имя пользователя
$ export GITHUB_EMAIL=zabotin.d@list.ru # задаем почту аккаунта в гитхаб
# задем токен с правами repo
$ export GITHUB_TOKEN=c61d37a994262a7ad31ffd5583cf4844913d657c
$ alias edit=subl # задаем стандартный текстовый редактор
```

```sh
$ cd ${GITHUB_USERNAME}/workspace # переходим директорию DimaZzZz101/workspace
$ source scripts/activate # 
```

```sh
$ mkdir ~/.config # 
$ cat > ~/.config/hub <<EOF # записываем следующий текст в hub
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF

$ git config --global hub.protocol https # указываем в файле hub протокол https
```

```sh
$ mkdir projects/lab02 && cd projects/lab02 # создаем директорию lab02 и переходим в нее
$ git init # инициализируем локальный репозиторий lab02
$ git config --global user.name ${GITHUB_USERNAME} # указываем в парметрах имя пользователя
$ git config --global user.email ${GITHUB_EMAIL} # указываем в параметрах почту аккаунта гитхаб
# check your git global settings
$ git config -e --global # проверяем установленные настройки
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git # добвляем удаленный репозиторий по ссылке
$ git pull origin main # получаем файлы из ветки main из удаленного репозитория
$ touch README.md # добавляем README файл
$ git status # проверяем на какой стадии находится README файл отслеживается/неотслеживается (не отслеживается)
$ git add README.md # отслеживаем файл 
$ git commit -m "added README.md" # делаем коммит, комментируя его 
$ git push origin main # добаляем файл README в удаленный репозиторий
```

Добавить на сервисе **GitHub** в репозитории **lab02** файл **.gitignore**
со следующем содержимом:

```sh
# предварительно создаем .gitignore файл в сервисе github
# в этот файл записываем следующие строки, чтобы гит автоматически не отслеживал их
*build*/
*install*/
*.swp
.idea/
```

```sh
$ git pull origin main # синхронизируем удаленный и локальный репозиторий
$ git log # вывести все дейтсвия производимые пользователем с репозиторием

# commit 58921ff21c082abe4bae50093b1f702b023a36ee (HEAD -> main, origin/main, origin/HEAD)
#Author: DimaZzZz101 <71131150+DimaZzZz101@users.noreply.github.com>
#Date:   Mon Mar 15 16:45:13 2021 +0300

    #Create .gitignore

#commit 9f6e3797e46fe4b914b1721ef733e6f1f7d97e11
#Author: DimaZzZz101 <zabotin.d@list.ru>
#Date:   Mon Mar 15 16:42:03 2021 +0300

    #added README.md

#commit aedf58f2e580c1569c34f983455f3887a231451b
#Author: DimaZzZz101 <71131150+DimaZzZz101@users.noreply.github.com>
#Date:   Mon Mar 15 15:41:31 2021 +0300

    #Initial commit
 
```

```sh
$ mkdir sources # создание директории sources
$ mkdir include # создание директории include
$ mkdir examples # создание директории examples
# запись в файл print.cpp следующего кода (реализация фугкций вывода)
$ cat > sources/print.cpp <<EOF 
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

```sh
# запись в файл print.hpp следующего кода (объявление функций вывода в заголовочном файле)
$ cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

```sh
# запись в файл main.cpp следующего кода (пример передачи аргументов в main)
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

```sh
# запись в файл main.cpp следующего кода (пример чтение из файла)
$ cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```sh
$ edit README.md # редактирование файла README.md
```

```sh
$ git status # проверка отслеживания фалов git-ом
$ git add README.md # добавление README.md в отслеживаемые git-ом файлы
$ git commit -m "added sources" # делаем коммит с описанием: "added sources"
$ git push origin main # добавляем изменения в ветку main
```

## Report

```sh
$ cd ~/workspace/ # переходим в директорию workspace
$ export LAB_NUMBER=02 # устанавливаем значение номера лабораторной работы
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER} # клонируем репозиторий текущей лабораторной работы
$ mkdir reports/lab${LAB_NUMBER} # создаем папку лабораторной в директории для отчетов
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md # копируем содержимое файла README.md в файл REPORT.md
$ cd reports/lab${LAB_NUMBER} # переходим в директорию текущей лабораторной работы 
$ edit REPORT.md # редактируем файл отчета
$ gist REPORT.md # загружаем файл отчета в gist
```

## Homework

### Part I

1. Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).
2. Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.
3. Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу **Hello world** на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.
4. Добавьте этот файл в локальную копию репозитория.
5. Закоммитьте изменения с *осмысленным* сообщением.
6. Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение `Hello world from @name`, где `@name` имя пользователя.
7. Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?
8. Запуште изменения в удалёный репозиторий.
9. Проверьте, что история коммитов доступна в удалёный репозитории.

### Part II

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. В локальной копии репозитория создайте локальную ветку `patch1`.
2. Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.
3. **commit**, **push** локальную ветку в удалённый репозиторий.
4. Проверьте, что ветка `patch1` доступна в удалёный репозитории.
5. Создайте pull-request `patch1 -> master`.
6. В локальной копии в ветке `patch1` добавьте в исходный код комментарии.
7. **commit**, **push**.
8. Проверьте, что новые изменения есть в созданном на **шаге 5** pull-request
9. В удалённый репозитории выполните  слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.
10. Локально выполните **pull**.
11. С помощью команды **git log** просмотрите историю в локальной версии ветки `master`.
12. Удалите локальную ветку `patch1`.

### Part III

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. Создайте новую локальную ветку `patch2`.
2. Измените *code style* с помощью утилиты [**clang-format**](http://clang.llvm.org/docs/ClangFormat.html). Например, используя опцию `-style=Mozilla`.
3. **commit**, **push**, создайте pull-request `patch2 -> master`.
4. В ветке **master** в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.
5. Убедитесь, что в pull-request появились *конфликтны*.
6. Для этого локально выполните **pull** + **rebase** (точную последовательность команд, следует узнать самостоятельно). **Исправьте конфликты**.
7. Сделайте *force push* в ветку `patch2`
8. Убедитель, что в pull-request пропали конфликтны. 
9. Вмержите pull-request `patch2 -> master`.

## Links

- [hub](https://hub.github.com/)
- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2015-2020 The ISC Authors
```
