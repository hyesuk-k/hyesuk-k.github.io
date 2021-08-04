set ts=2
set sw=2

set ai
set ci
set si

set hlsearch
set ic
set scs
set nu

colorscheme default
if has ("syntax")
  syntax on
endif

set tagbsearch
set fileencoding=korea
set fileencodings=ubs-bom, utf-8, korea

set nocompatible

filetype off    " required

"set tags
set tags=./tags;/
"set tags += ./tags, tags
"set tags += ./tags,../tags,../../tags,tags

"set cscope add cscope.out
set nocsverb

if filereadable("./cscope.out")
  cs add ./cscope.out

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

call vundle#end()

filetype plugin indent on    " required 

" Brief help 
" :PluginList       - lists configured plugins 
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate 
" :PluginSearch foo - searches for foo; append `!` to refresh local cache 
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal 

" see :h vundle for more details or wiki for FAQ 

" taglist-plus : use F7 func key for ON/OFF
nmap <F9> :TlistToggle<CR> 
let Tlist_Ctags_Cmd = "/usr/bin/ctags" 
let Tlist_Inc_Winwidth = 0 
let Tlist_Exit_OnlyWindow = 0 
let Tlist_Auto_Open = 0 
let Tlist_Use_Left_Window = 1 

" nerdtree : use F8 func key for ON/OFF
nmap <F10> :NERDTreeToggle<CR> 
let g:NERDTreeWinPos = "right"

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

nmap <F4> :qa!<CR>