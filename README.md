# Задание №1 **"Универсальный архиватор"**

## Содержание
1. [Описание](#Описание)
2. [Правила игры](#Правила-игры)
3. [Сборка](#Сборка)
  * [Unix](#unix)
  * [Windows](#windows)
  * [Docker](#docker)
4. [Запуск](#Запуск)
5. [Описание репозитория](#Описание-репозитория)

## Описание
Вам необходимо реализовать архиватор.
Задача — достичь как можно большей степени сжатия без потерь.

Замер будет проводиться в трех номинациях:
1. Арифметическое сжатие (допустим только алгоритм с нормализацией)
2. PPM
3. BWT

Для повышения степени сжатия своего архиватора рекомендуется использовать следующие методы:
* Адаптивная (или блочно-адаптивная) реализация арифметического сжатия.
* Подбор параметров агрессивности сжатия (возможно динамически, т.е. во время
  исполнения программы).
* Начальная инициализация таблиц вероятностей наиболее подходящими значениями 
  (возможно — смена таблиц при изменении характера данных).
* Увеличение точности алгоритма арифметического сжатия
  (использование int64_t и double).
* Если вам этого кажется мало — реализуйте PPM (как минимум простой,
  рассказанный на лекции, с любыми дополнениями, которые вы можете найти
  в книге [Методы сжатия данных](http://compression.ru/book/pdf/compression_methods_part1_2-4.pdf)).

## Правила игры
* За использование чужих исходных текстов или совместное написание — 
  работы всех причастных будут сниматься с замеров, и им будет присуждаться 0 баллов.
  Подробнее — читайте [Stanford CS Honor Code](http://csmajor.stanford.edu/HonorCode.shtml).
* Если разархивированный файл не совпадает с исходным,
  компрессор падает или работает более 3 минут,
  ему присваивается значение худшего результата в замере.
* За первые три места в любой номинации (после собеседования по алгоритму)
  — дополнительные баллы.

## Сборка
### **Unix**
В примере используется пакетный менеджер APT, который по умолчанию стоит 
на Ubuntu. На macOS можно пользоваться Homebrew, и тогда на 1-2 этапах будет 
использоваться команда `brew` вместо `apt`.

1. Обновляем package list:

    ```sh
    sudo apt update
    ```
    
2. Устанавливаем всё необходимое.

    ```sh
    sudo apt install -y cmake g++ python3
    ```
    
3. Распаковываем архив.
    
4. Заходим в папку compressor, создаём папку build и заходим в неё:

    ```sh
    mkdir build
    cd build

    ```

5. Теперь конфигурируем проект с помощью CMake, указав ../src как папку
    с исходниками, а после ключа `-G` — формат проекта, который мы хотим получить.
    Например, эта команда сгенерирует самый обыкновенный Makefile:

    ```sh
    cmake -G "Unix Makefiles" ../src
    ```
    
    А эта — проект для CodeBlocks:
    
    ```sh
    cmake -G "CodeBlocks - Unix Makefiles" ../src
    ```
    
    Все поддерживаемые форматы можно найти в справке `cmake --help`.
    В дальнейшем вам **не нужно** будет конфигурировать проект повторно.
    
6. Собираем проект удобным образом, например:

    ```sh
    make  # нужно быть в папке compressor/build
    ```
    
7. Проверяем папку compressor/build на наличие исполняемого файла
   compress.
8. Тестируем шаблон:

    ```sh
    cd ..  # поднимаемся из build в папку compressor
    python3 tests/run.py
    ```
    
    После успешного выполнения тестов в папке compression появится файл
    results.csv, который можно открыть и посмотреть, что тестирование
    действительно прошло успешно.
9. Ура, всё получилось! Вы готовы создавать лучший в мире архиватор.
    
    
### **Windows**
Эпиграф:

— Надо было ставить Linux!

1. Cтавим [CMake](https://cmake.org/download/).
2. Для тестирующего скрипта ставим интерпретатор [Python 3](https://www.python.org/downloads/).
3. На всякий случай ставим [MinGW](http://www.mingw.org/) по этой
    [ссылке](https://sourceforge.net/projects/mingw/files/Installer/).
4. Запускаем Windows PowerShell.
5. Распаковываем архив.
6. Заходим в папку compressor, создаём папку build и заходим в неё:

    ```sh
    mkdir build
    cd build

    ```

8. Теперь конфигурируем проект с помощью CMake, указав ../src как папку
    с исходниками, а после ключа `-G` — формат проекта, который мы хотим получить.
    Например, эта команда сгенерирует MinGW Makefile:

    ```sh
    cmake -G "MinGW Makefiles" ../src
    ```
    
    А эта — проект для CodeBlocks:
    
    ```sh
    cmake -G "CodeBlocks - MinGW Makefiles" ../src
    ```
    
    Все поддерживаемые форматы можно найти в справке `cmake --help`.
    В дальнейшем вам **не нужно** будет конфигурировать проект повторно.
    
9. Собираем проект удобным образом, например:

    ```sh
    mingw32-make  # нужно быть в папке compressor/build
    ```
    
10. Проверяем папку compressor/build на наличие исполняемого файла
   compress.exe.
11. Тестируем шаблон:

    ```sh
    cd ..  # поднимаемся из build в папку compression
    python tests/run.py
    ```
    
    После успешного выполнения тестов в папке compression появится файл
    results.csv, который можно открыть и посмотреть, что тестирование
    действительно прошло успешно.
12. Ура, всё получилось! Вы готовы создавать лучший в мире архиватор.

### **Docker**
Чтобы прогнать тесты с докером: 
1. Установите [Docker](https://docker.com)
2. Склонируйте ваш репозиторий
3. Соберите образ и запустите контейнер:

```bash
docker build . -t compressor_tests
docker run compressor_tests
```

Так как набирать эти 2 строчки слишком долго, в корне лежит маленький Makefile,
который позволяет сократить проверку до 2х слов `make tests`
  
**Внимание:** задания будут проверяться именно этим способом, поэтому 
желательно протестировать последнюю версию вашей программы с Docker


## Запуск
Общий вид:

```sh
# Unix
compress [options]
```

```sh
# Windows
compress.exe [options]
```

Опции:

```cmd
--help                     = Вызов справки, аналогичной этой.
--input  <file>            = Входной файл для архивации/разархивации, по умолчанию `input.txt`.
--output <file>            = Выходной файл для архивации/разархивации, по умолчанию `output.txt`.
--mode   {c | d}           = Режим работы: `c` - архивация, `d` - разархивация; по умолчанию `c`.
--method {ari | ppm | bwt} = Метод архивации/разархивации файла, по умолчанию `ari`.
```

Например, если мы хотим заархивировать файл the_financier.txt в файл
the_financier.cmp методом PPM, то команда будет выглядеть так:

```sh
# Unix
compress --input the_financier.txt --output the_financier.cmp --mode c --method ppm
```

```sh
# Windows
compress.exe --input the_financier.txt --output the_financier.cmp --mode c --method ppm
```

## Описание репозитория
* `src` — папка с исходными файлами:
    * main.c — основной файл;
    * utils.c — файл, содержащий функции, чтобы парсить аргументы командной строки;
    * ari.c, ppm.c, bwt.c — файлы, которые вам нужно изменить, написав
    соответствующий алгоритм;
    * *.h — заголовочные файлы ко всем вышеуказанным;
    * CMakeLists.txt — файл, необходимый для генерации нужного вам формата
    прокета с помощью CMake.

    Вообще говоря, никто вас не обязывает использовать именно такую архитектуру
    решения. Самое главное — ваше решение должно поддерживать указанный формат
    запуска.

* `test_files` — папка с тестовыми файлами.
        
* `tests` — папка для тестирования. Содержит:
        
    * `testing` — содержит необходимые для тестирования скрипты. 
    
    * `run.py` — тестирующий скрипт на языке Python. 
        Его можно использовать для тестирования вашего архиватора следующим образом:
        
        ```sh
        python run.py [options]
        ```
        
        Опции:
        
        ```cmd
        --timeout <timeout> = Время, отведённое для тестирования каждого файла, по умолчанию 180с.
        --testdir <dir>     = Папка с тестовыми файлами, по умолчанию `test_files`.
        --outputdir <dir>   = Папка для выходных файлов, полученных в результате работы архиватора.
        --output            = Флаг для сохранения выходных файлов. Если папка не указана с помощью ключа --outputdir, то папка по умолчанию — `output`.
        ```
        
        Скрипт сжимает и распаковывает каждый файл, находящийся в указанной папке,
        вычисляет размер получившихся файлов, сравнивает содержимое и записывает
        результат по каждому файлу в results.csv.
        
        Возможные вердикты:
        * OK — файл успешно сжат, распакован и совпадает с исходным;
        * WA — файл успешно сжат и распакован, но не совпадает с исходным;
        * RE1 — ошибка выполнения при сжатии;
        * TL1 — превышен таймаут при сжатии;
        * RE2 — ошибка выполнения при распаковке;
        * TL2 — превышен таймаут при распаковке.
    
    * `config.cfg` — файл с указанием реализованных методов сжатия в формате списка 
        [JSON](http://www.json.org/).
        
        Всего допускается 3 значения: "ARI", "PPM", "BWT".
        
        Пример содержимого файла:
        
        ```json
        ["ARI", "BWT"]
        ```
        
        Этот файл необходим для тестирующего скрипта, чтобы определить,
        какие методы тестировать.

