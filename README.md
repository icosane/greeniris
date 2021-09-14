## Настройка Fortran в SublimeText 

Краткое руководство по настройке Fortran в SublimeText. Как минимум, часть гайда будет работать и для VSCode, Atom, Notepad++.

### Компилятор Fortran

Существует несколько различных компиляторов Fortran, список можно посмотреть [здесь](https://fortran-lang.org/compilers/). Из них бесплатными являются только GFortran, Flang, и LFortran. Также, NVIDIA и Intel предлагают свои компиляторы бесплатно, но NVFortran (NVIDIA) доступен только для систем Linux, а Intel oneAPI доступен для систем Linux, Windows и macOS. В данном руководстве мы будем использовать GFortran. 

#### Windows
GFortran можно скачать [здесь](http://www.equation.com/servlet/equation.cmd?fa=fortran). Опять же, существует несколько способов установки GFortran, на мой взягляд этот является самым простым. Но вы также можете попробовать [TDM GCC](https://jmeubank.github.io/tdm-gcc/articles/2020-03/9.2.0-release) и [MinGW](http://mingw-w64.org/doku.php/download/mingw-builds). 

Выбираем версию *11.1.0* под вашу разрядность операционной системы и скачиваем установщих. В установщике просто соглашаемся со всем до конца установки.
После завершения установки нажать *Win+PauseBreak*, выбрать в меню *Дополнительные параметры системы > Переменные среды > Системные переменные > Path (изменить)*. Выбрать *Создать* и добавить следующую строку:
```
‪C:\Users\имя пользователя\gcc\bin\gfortran.exe
```
где вместо *имя пользователя* вставить свое имя пользователя. Узнать его можно выполнив *Win+R* и набрав cmd. 
```
C\Users\ ваше имя пользователя здесь
```
#### Mac OS
Если у вас установлен Xcode, откройте окно терминала и введите:
```
xcode-select --install
```
Либо воспользуйтесь установочным [файлом](https://github.com/fxcoudert/gfortran-for-macOS/releases).
Homebrew:
```
port search gcc
sudo port install gcc10
```

### Текстовый редактор

Как и написано в заголовке, я использовал [SublimeText](https://www.sublimetext.com/). Можно также воспользоваться:

* [Atom](https://atom.io/)
* [Emacs](https://www.gnu.org/software/emacs/)
* [NotePad++](https://notepad-plus-plus.org/)
* [Vim](https://www.vim.org/) и [Neovim](https://neovim.io/)
* [Visual Studio Code](https://code.visualstudio.com/)

#### Настройка SublimeText

После запуска нажмите *Сtrl+Shift+P* и наберите *Install Package*.
После чего установите следующие пакеты:
* Fortran
* Sublimelinter
* sublimelint
(после установки каждого пакета необходимо заново повторять *Ctrl+Shift+P* и *Install Package*)

Последнее что нужно сделать
В меню (Alt) выберете Tools > Build System > New Build System
Скопируйте и вставьте туда следующее:
```
{
    "shell_cmd": "gfortran \"${file}\" -o \"${file_path}/${file_base_name}\"",
    "file_regex": "^(?xi:( ^[/] [^:]* ) : (\\d+) : (\\d+) :)",
    "working_dir": "${file_path}",
    "selector": "source.modern-fortran, source.fixedform-fortran",
    "syntax": "GFortranBuild.sublime-syntax",

    "variants":
    [
        {
            "name": "Run",
            "shell_cmd": "gfortran \"${file}\" -o \"${file_path}/${file_base_name}\" && \"${file_path}/${file_base_name}\""
        }
    ]
}
```
Сохраните файл как Fortran.sublime-build. В меню (Alt) выберете Tools > Build System > Fortran.

### Тестовая программа

```fortran
program hello
    print *, "Hello World!"
end program
```

Сохраните файл как *hello.f90*
*Ctrl+B* скомпилирует файл и выведет результат. В выпадающем меню выбрать *Fortran - Run*
