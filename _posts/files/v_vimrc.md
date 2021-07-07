filetype off    " required

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

" nerdtree : F8키 누르면 ON/OFF
nmap <F8> :NERDTreeToggle<CR> 
let g:NERDTreeWinPos = "right"

" taglist-plus : F7키 누르면 ON/OFF
nmap <F7> :TlistToggle<CR> 
let Tlist_Ctags_Cmd = "/usr/bin/ctags" 
let Tlist_Inc_Winwidth = 0 
let Tlist_Exit_OnlyWindow = 0 
let Tlist_Auto_Open = 0 
let Tlist_Use_Left_Window = 1 

" vim-airline
let g:airline#extensions#tabline#enabled = 1 " turn on buffer list 
let g:airline_theme='hybrid' 
set laststatus=2 " turn on bottom bar 

" ctrlp : 성능 향상, Ctrl + P 눌렀을 때, 파일 목록 출력됨
let g:ctrlp_custom_ignore = { 
  \ 'dir':  '\.git$\|public$\|log$\|tmp$\|vendor$', 
  \ 'file': '\v\.(exe|so|dll)$' 
\ }
