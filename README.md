## Настройка Fortran для использования в текстовом редакторе (Visual Studio Code, Sublime Text, Atom, Notepad++, etc.)

Краткое руководство по настройке Fortran в SublimeText. Как минимум, часть гайда будет работать и для VSCode, Atom, Notepad++.

### Компилятор Fortran

Существует несколько различных компиляторов Fortran, список можно посмотреть [здесь](https://fortran-lang.org/compilers/). Из них бесплатными являются только GFortran, Flang, и LFortran. Также, NVIDIA и Intel предлагают свои компиляторы бесплатно, но NVFortran (NVIDIA) доступен только для систем Linux, а Intel oneAPI доступен для систем Linux, Windows и macOS. В данном руководстве мы будем использовать GFortran. 

#### Windows
GFortran можно скачать [здесь](http://www.equation.com/servlet/equation.cmd?fa=fortran). Опять же, существует несколько способов установки GFortran, на мой взягляд этот является самым простым. Но вы также можете попробовать [TDM GCC](https://jmeubank.github.io/tdm-gcc/articles/2020-03/9.2.0-release) и [MinGW](http://mingw-w64.org/doku.php/download/mingw-builds). 

Выбираем версию *11.1.0* под вашу разрядность операционной системы и скачиваем установщих. В установщике просто соглашаемся со всем до конца установки.
После завершения установки нажать *Win+PauseBreak*  
(если эта клавиша отсутствует можно в проводнике "Этот компьютер" нажать правой кнопкой мыши и выбрать свойства, либо, в Windows 10: параметры > система > сведения об устройстве)  
выбрать в меню *Дополнительные параметры системы > Переменные среды > Системные переменные > Path (изменить)*. Выбрать *Создать* и добавить следующую строку:
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
Скопируйте и вставьте туда следующее: (не нужно если у вас Linux/Mac OS, выберите GFortran в Tools > Build System > GFortran)
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

#### Настройка Visual Studio Code 
Когда редактор открыт, в главном пользовательском интерфейсе, в колонке с кнопками слева, есть значок в форме четырех квадратов, чтобы открыть Магазин расширений. Для разработки на языке Fortran предлагаются следующие расширения:

 * [Modern Fortran](https://marketplace.visualstudio.com/items?itemName=krvajalm.linter-gfortran)
 * [FORTRAN IntelliSense](https://marketplace.visualstudio.com/items?itemName=hansec.fortran-ls)

Зависит от предыдущего расширения Modern Fortran, а также от Python и [Fortran Language Server](https://github.com/hansec/fortran-language-server), которые должны быть установлены отдельно.
* [Fortran Breakpoint Support](https://marketplace.visualstudio.com/items?itemName=ekibun.fortranbreaker)

Примечание: И Modern Fortran, и Fortran Breakpoint Support требуют ms-vscode.cpptools, для этого вам также необходимо установить расширение [C/C++ by Microsoft](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).

#### Настройка Notepad++
В данном случае некоторым придется научиться работать с командной строкой.  
Скопируйте тестовую программу в Notepad++ и сохраните ее под любым именем (например hello.f90). Запомните директорию в которую была сохранена программа. 
Откройте эту директорию в проводнике, найдите сохраненный файл.   
**Внимание, дальнейшие команды актуальны только в Windows 10**  
Нажите на пустом месте в папке *Shift+правая кнопка мыши* и выберите *Открыть окно Powershell*. В открывшемся окне либо напишите следующее:
```
gfortran.exe hello.f90
```
Вместо ```hello.f90``` будет имя вашей сохраненной программы. Лайфхак: можно не набирать ```gfortran.exe``` целиком, достаточно набрать ```gf``` и нажать TAB, сработает автодополнение.  
В директории появится скомпилированный файл ```a.exe```. Я понятия не имею почему система каждый раз называет его ```a.exe```, и как это изменить.  В командной строке наберите просто: ```a.exe```. Программа выполнится. Если же просто запустить исполняемый файл, то она тоже выполнится, но вы ничего не увидите, потому что она сразу же завершит работу. Советую запускать всегда через командную строку.

### Тестовая программа

```fortran
program hello
    print *, "Hello World!"
end program
```

Сохраните файл как *hello.f90*
*Ctrl+B* скомпилирует файл и выведет результат. В выпадающем меню выбрать *Fortran - Run* (применимо только к SublimeText; компиляция в VSCode - F5. Остальные? См. раздел Notepad++ про консоль.)

#### Sources
Original article by Fortran-lang.org [here](https://fortran-lang.org/learn/os_setup). Translated for my classmates by me. 
