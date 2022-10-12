set ts=2
set sw=2

set ai
set ci
set si

set expandtab

set hlsearch
set ic
set scs
set nu
set ruler

set history=1000

color desert
if has ("syntax")
	syntax on
endif

set tags=./tags;/
set tagbsearch
set fileencoding=korea
set fileencodings=ubs-bom,utf-8,korea

set nocompatible
filetype off    " required

"set tags +=./tags,../tags,../../tags,../../../tags,../../../../tags,../../../../../tags,tags
"set tags += ./tags,tags

"""set cscope

set nocsverb
":cscope add cscope.out

func! MakeCscope()
  # need enter key
    exe "!find `pwd` \\( -name '*.c' -o -name '*.cc' -o -name '*.cpp' -o -name '*.h' -o -name '*.hh' \\) -print > cscope.files"
  # need ctrl + d
    exe "!cscope -i cscope.files"
endfunc

if filereadable("./cscope.out")
    cs add ./cscope.out
elseif filereadable("../cscope.out")
    cs add ../cscope.out
elseif filereadable("../../cscope.out")
    cs add ../../cscope.out
elseif filereadable("../../../cscope.out")
    cs add ../../../cscope.out
elseif filereadable("../../../../cscope.out")
    cs add ../.././../cscope.out
elseif filereadable("../../../../../cscope.out")
    cs add ../../../../../cscope.out
elseif filereadable("../../../../../../cscope.out")
    cs add ../../../../../../cscope.out
elseif filereadable("../../../../../../../cscope.out")
    cs add ../../../../../../../cscope.out
else
    :call MakeCscope()
    cs add ./cscope.out
endif

set csverb

" set the runtime path to include Vundle and initialize

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

Plugin 'VundleVim/Vundle.vim'

Plugin 'scrooloose/nerdtree'
Plugin 'taglist-plus'
Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'
Plugin 'ctrlpvim/ctrlp.vim'
Plugin 'Yggdroot/indentLine'
Plugin 'Syntastic'  " check syntax
"Plugin 'valloric/youcompleteme'
"Plug 'neoclide/coc.nvim', {'branch': 'release'}

call vundle#end()

filetype plugin indent on    " required

" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal

" see :h vundle for more details or wiki for FAQ

" nerdtree : use F8 func key for ON/OFF
nmap <F8> :NERDTreeToggle<CR>
let g:NERDTreeWinPos = "right"

" taglist-plus : use F7 func key for ON/OFF
nmap <F9> :TlistToggle<CR>
let Tlist_Ctags_Cmd = "/usr/bin/ctags"
let Tlist_Inc_Winwidth = 0
let Tlist_Exit_OnlyWindow = 0
let Tlist_Auto_Open = 0
let Tlist_Use_Left_Window = 1

" vim-airline
let g:airline#extensions#tabline#enabled = 1 " turn on buffer list
let g:airline_theme='hybrid'
set laststatus=2 " turn on bottom bar

" ctrlp : for performance
" Ctrl + P : open file lists
let g:ctrlp_custom_ignore = {
  \ 'dir':  '\.git$\|public$\|log$\|tmp$\|vendor$',
  \ 'file': '\v\.(exe|so|dll)$'
\ }

" Syntastic
set statusline+=%#warningmsg#
set statusline+=%{SyntasticStatuslineFlag()}
set statusline+=%*

let g:syntastic_always_populate_loc_list = 1
let g:syntastic_auto_loc_list = 1
let g:syntastic_check_on_open = 1
let g:syntastic_check_on_wq = 0

let g:syntastic_cpp_compiler = 'g++'
let g:syntastic_cpp_compiler_options = "-std=c++17 -Wall -Wextra -Wpedantic"
let g:syntastic_c_compiler_options = "-std=c17 -Wall -Wextra -Wpedantic"

"==grep=="
func! GrepCHPP()
  let sm=expand("<cword>")
  exe "!grep -rn " .sm " --include=*.{cc,cpp,h,hh,hpp}"
endfunc
nmap ,ll :call GrepCHPP() <CR>

func! GrepHPP()
  let sm=expand("<cword>")
  exe "!grep -rn " .sm " --include=*.{h,hh,hpp}"
endfunc
nmap ,lh :call GrepHPP() <CR>

func! GrepCPP()
  let sm=expand("<cword>")
  exe "!grep -rn " .sm " --include=*.{c,cc,cpp}"
endfunc
nmap ,lc :call GrepHPP() <CR>

"==cscope find=="
func! CSFindDefinition()
    let sm=expand("<cword>")
    exe "cs find g " .sm
endfunc
nmap ,lg :call CSFindDefinition() <CR>

nmap <F4> :qa!<CR>
nmap <F2> :tabnew<CR>
" move to the right tab
nmap <BS> gt<CR>
