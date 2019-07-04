# Fast-accurate-scientific-writing

Заметка навеяна технологией быстрого написания математических конспектов, опубликованных [здесь](https://habr.com/ru/post/445066/) и [здесь](https://habr.com/ru/post/450088/).

Далее привожу настройки среды написания текстов для получения результатов близких к описанным выше в хабра-постах.

## Настройка среды
Установка vim, zathura, vimtex, vim-plug, vim-shippets, latex, inkscape:

    sudo apt-get install vim
    sudo apt-get install zathura
    sudo apt-get install vimtex
    curl -fLo ~/.vim/autoload/plug.vim --create-dirs     https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    sudo apt-get install vim-snippets
    sudo apt-get install texlive texlive-full
    sudo apt-get install inkscape

### Настройка vim-plug
Введя команду

    vim ~/.vimrc
    
настариваем vim-plug сдедующим образом

    call plug#begin('~/.vim/plugged')
    Plug 'pearofducks/ansible-vim'
    
    " A Vim Plug for Lively Previewing LaTeX PDF Output
    Plug 'xuhdev/vim-latex-live-preview', { 'for': 'tex' }
    let g:livepreview_previewer ='zathura'
    
    Plug 'lervag/vimtex'
    let g:tex_flavor='latex'
    let g:vimtex_view_method='zathura'
    let g:vimtex_quickfix_mode=0
    set conceallevel=1
    let g:tex_conceal='abdmg'
    
    Plug 'sirver/ultisnips'
    let g:UltiSnipsExpandTrigger = '<tab>'
    let g:UltiSnipsJumpForwardTrigger = '<tab>'
    let g:UltiSnipsJumpBackwardTrigger = '<s-tab>'
    let g:UltiSnipsEditSplit="vertical"
    let g:UltiSnipsSnippetDirectories=[$HOME.'/general/path/of/snippets/']
	
    setlocal spell spelllang=en_gb
    inoremap <C-l> <c-g>u<Esc>[s1z=`]a<c-g>u 
    inoremap <C-f> <Esc>: silent exec '.!inkscape-figures create "'.getline('.').'" "/home/vasiliyeskin/figures/"'<CR><CR>:w<CR>
    nnoremap <C-f> :      silent exec '!inkscape-figures edit /home/vasiliyeskin/figures/ > /dev/null 2>&1 &'<CR><CR>:redraw!<CR>
    
    call plug#end()


