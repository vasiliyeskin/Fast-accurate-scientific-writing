# Fast-accurate-scientific-writing

Заметка навеяна технологией быстрого написания математических конспектов, опубликованных [здесь](https://habr.com/ru/post/445066/) и [здесь](https://habr.com/ru/post/450088/).
Исходные тексты к тем, что были на хабре: быстрый набор - [https://castel.dev/post/lecture-notes-1/](https://castel.dev/post/lecture-notes-1/); рисование - [https://castel.dev/post/lecture-notes-2/](https://castel.dev/post/lecture-notes-2/)

Далее привожу настройки среды написания текстов для получения результатов близких к описанным выше в хабра-постах.

## Настройка среды в Ubuntu
Установка vim, zathura, vimtex, vim-plug, vim-snippets, latex, inkscape:

    sudo apt-get install vim
    sudo apt-get install zathura
    sudo apt-get install vimtex
    curl -fLo ~/.vim/autoload/plug.vim --create-dirs     https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    sudo apt-get install vim-snippets
    sudo apt-get install inkscape
    sudo apt install xdotool
   
Ставим либо texlive

    sudo apt-get install texlive texlive-full
    
либо [Miktex](https://miktex.org/howto/install-miktex-unx). По мне, так с миктехом меньше проблем.

Для управления рисунками ставим [rofi](https://github.com/davatorium/rofi) и [inkscape figure manager](https://github.com/gillescastel/inkscape-figures)
    
    sudo apt install rofi
    sudo apt install python3-pip
    sudo pip3 install pyrebase
    sudo pip3 install --upgrade setuptools
    sudo add-apt-repository ppa:deadsnakes/ppa
    sudo apt install python3.7
    sudo pip3 install pathlib
    sudo python3 -m pip install inkscape-figures

Отмечу, для корректной работы inkscape-figures необходим python версии >=3.7.
Назначте эту версию питона по-умолчанию ():

    sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1
    sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1

Ставим менеджер управления сочетаниями клавиш при редактировании рисунков

    git clone https://github.com/gillescastel/inkscape-shortcut-manager.git
    sudo apt-get install  python3-xlib
    sudo apt-get install xclip xsel
    sudo apt-get install rxvt-unicode
    
### Настройка vim-plug

Введя команду 

    vim ~/.vimrc    

настраиваем vim-plug следующим образом

    call plug#begin('~/.vim/plugged')
    Plug 'pearofducks/ansible-vim'
    
    " A Vim Plug for Lively Previewing LaTeX PDF Output
    Plug 'xuhdev/vim-latex-live-preview', { 'for': 'tex' }
    let g:livepreview_previewer ='zathura'
    
    Plug 'lervag/vimtex'
    let g:tex_flavor='latex'
    let g:vimtex_view_method='zathura'
    let g:vimtex_quickfix_mode=0

    Plug 'KeitaNakamura/tex-conceal.vim'
    set conceallevel=1
    let g:tex_conceal='abdmg'
    hi Conceal ctermbg=none

    
    Plug 'sirver/ultisnips'
    let g:UltiSnipsExpandTrigger = '<tab>'
    let g:UltiSnipsJumpForwardTrigger = '<tab>'
    let g:UltiSnipsJumpBackwardTrigger = '<s-tab>'
    let g:UltiSnipsEditSplit="vertical"
    let g:UltiSnipsSnippetDirectories=[$HOME.'/general/path/of/snippets/']
	
    let g:spellfile_URL = 'http://ftp.vim.org/vim/runtime/spell'
    setlocal spell spelllang=en_gb,ru_yo
    inoremap <C-l> <c-g>u<Esc>[s1z=`]a<c-g>u 
    inoremap <C-f> <Esc>: silent exec '.!inkscape-figures create "'.getline('.').'" "'.b:vimtex.root.'/figures/"'<CR><CR>:w<CR>
    nnoremap <C-f> : silent exec '!inkscape-figures edit "'.b:vimtex.root.'/figures/" > /dev/null 2>&1 &'<CR><CR>:redraw!<CR>

    
    call plug#end()

### Настройка тёмного стиля zathura

Открываем настройки затуры
    
    vim ~/.config/zathura/zathurarc

Вставляем содержимое файла zathurarc.settings

### Настройка проверки орфографии

При первом запуске в vim наберите 

    :setlocal spelllang=en_gb,ru_yo

и следуйте инстукциям. При этом доустановятся словари.
    

## Запуск среды и работа

Запускаем файл для работы

    vim test.tex

В vim выполняем команды для подгрузки и установки плагинов

    :PlugUpdate
    :PlugInstall
 
Апдейтим сниппеты

    :UltiSnipsEdit

В отктывшийся файл загружаем простыню сниппетов из файла tex.snippets.

запускаем автоматическую компиляцию теховского файла в pdf (на лету после обновления текста, но не рисунков)

    :LL

или

    :LLPStartPreview

Если всё сделано правильно, то откроется Zathura с результатом вашей работы. Я обычно переключаюсь в темную тему по нажатию **Ctrl+R** и расширяю отображаемый документы по ширине окна Zathura нажатием **s**.



Для автоматической работы с графикой в шапке файла (до \begin{document}) прописываем команду

    \usepackage{import}
    \usepackage{xifthen}
    \usepackage{pdfpages}
    \usepackage{transparent}
    
    \newcommand{\incfig}[1]{%
        \def\svgwidth{\columnwidth}
        \import{./figures/}{#1.pdf_tex}
    }
    \pdfsuppresswarningpagegroup=1

**В корне должена быть создана папка figures**

Для автоматического сохранения графического файла в форматы pdf и pdf_tex запускаем скрипт просматривающий изменения файла

    inkscape-figures watch

Для ускорения работы в inkscape запускаем питоновский скрипт

    python3.7 ~/inkscape-shortcut-manager/main.py


После ввода в теховском файле желаемого названия графического файла и нажатия **Ctrl+F** открывается inkscape и создаётся требуемый файл. По сохранению изменений в inscape автоматически происходит пересохранение и сопутствующих графических файлов.

Осуществляется проверка правописания. Если русский словарь не загрузится попробуйте зайдите с правами администратора в настройки vim

    sudo vim ~/.vimrc

Должно появится предложение о догрузки необходимых словарей. По нажатию **Ctrl+L** происходит исправление ошибки в предыдущем слове.


Необходимо поставить шрифт cm-super для отображения векторного шрифта вместо битмаповского.


### Vim tutorials

https://www.openvim.com/
https://www.udemy.com/course/learn-vim-for-beginners/?gclid=CjwKCAiAnvj9BRA4EiwAuUMDf-prjqXYxt6VXxuK0r-x9QWhLAPQObix50HytBjUbeVS0nsPy7tE2BoCpfEQAvD_BwE&matchtype=e&utm_campaign=LongTail_la.EN_cc.ROW&utm_content=deal4584&utm_medium=udemyads&utm_source=adwords&utm_term=_._ag_80675504082_._ad_478977506795_._kw_vim+text+editor+tutorial_._de_c_._dm__._pl__._ti_kwd-828088591612_._li_1011981_._pd__._



For the managing references install Mendeley desctop https://kifarunix.com/install-and-use-mendeley-in-ubuntu-20-04/
