# APNG
en:## An Animated PNG reading library
ru:## Библиотека для чтения анимированных PNG-картинок

en:The goal of this library is to provide a very simple API for reading and using frames from an APNG file.
ru:Данная библиотека предназначена для предоставления очень простого API для чтения и использования кадров из файлов APNG

[![Build Status](https://travis-ci.org/DX-MON/APNG.svg?branch=master)](https://travis-ci.org/DX-MON/APNG)

en:## Building the library
ru:## Сборка  библиотеки

en:This library can be built by running:
ru:Данная библиотека может быть собрана следующим образом:
`make && make install`

en:## Testing your build
ru:## Тестирование вашей сборки

en:This library's tests depend on [crunch](https://github.com/DX-MON/crunch) being installed and in your PATH.
ru:Для тестов данной библиотеки необходимо, чтобы [crunch](https://github.com/DX-MON/crunch) был установлен и указан в переменной окружения PATH.
en:Provided this is met, the tests can be built by running `make test`.
ru:Когда это условие соблюдено, тесты могут быть собраны командой `make test`.
en:To run the tests (and build them if they aren't), run `make check`.
ru:Для запуска тестов (и сборки, если такая не производилась) запустите `make check`.

en:## The API
ru:## API

en:The main type in the library is apng_t, which allows loading and interogating an APNG file.
ru:Основным типом данных в данной библиотеке является apng_t, который предоставляет загрузку и опрос APNG файла.
en:There are several ways to present the APNG data to apng_t - using any of the built in stream_t types, or your own.
ru:Есть несколько представлений данных APNG в apng_t: через встроенные типы stream_t или ваш собственный.

en:The three available built-in stream_t types are:
ru:Встроеннные типы stream_t следующие:
* en:fileStream_t, which takes the file to open and the mode to open it with using open()'s constants
* ru:fileStream_t, который принимает путь до файла и режим открытия (см. константы системного вызова open())
* en:memoryStream_t, which takes a buffer and the length of that buffer
* ru:memoryStream_t, который прнимает указатель на данные (буфер) и длину данных (в байтах)
* en:zlibStream_t, which takes some other stream_t that represents a ZLib stream, and whether the stream should be used in inflate or deflate mode
* ru:zlibStream_t, which takes some other stream_t that represents a ZLib stream, and whether the stream should be used in inflate or deflate mode

en:Using fileStream_t as an example, here's a typical way to open an APNG file and have the library read it in:
ru:Например, при использовании типа fileStream_t типовой способ открытия и чтения файла APNG следующий:
`apng_t pngFile(fileStream_t("myAPNG.png", O_RDONLY));`
en:At this point, the frame data will be available by calling pngFile.frames(), with other properties such as the width and the height of the display area available by calling pngFile.width() and pngFile.height().
ru:После данного вызова кадры можно получить вызовом `calling pngFile.frames()`. Другие свойства, такие как ширина и высота анимации в пикселах, доступны с помощью вызовов `pngFile.width()` и `pngFile.height()` соответственно.

en:To free all resources consumed by this operation, simply let apng_t go out of scope, or if you used new to allocate your instance, just call delete on the instance, though you should have used std::unique_ptr<>.
ru:Все используемые системные ресурсы освобождаются, как только переменная типа apng_t выходит за пределы области видимости. Или, если вы создавали переменную посредством операции new, просто вызовите delete, хотя вам, наверное, следовало бы использовать std::uniqye_ptr<> в таких случаях.
en:*DO NOTE*: all frame data returned by frames() will be invalidated and you must stop using the pointers, after allowing apng_t to go out of scope.
ru:*Имейте в виду:* все данные фреймов, полученные вызовом frames() будут некорректны, и вы не должны обращаться к соответствующим указателям после выхода apng_t из области видимости.
