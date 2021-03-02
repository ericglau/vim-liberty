# vim-liberty

.vimrc
```
" Specify a directory for plugins
" - For Neovim: stdpath('data') . '/plugged'
" - Avoid using standard Vim directory names like 'plugin'
call plug#begin('~/.vim/plugged')

" Make sure you use single quotes

Plug 'autozimu/LanguageClient-neovim', {
    \ 'branch': 'next',
    \ 'do': 'bash install.sh',
    \ }

" (Optional) Multi-entry selection UI.
Plug 'junegunn/fzf'

" Initialize plugin system
call plug#end()

au BufReadPost server.xml set filetype=liberty
au Filetype liberty set syntax=xml

let g:LanguageClient_serverCommands = {
\ 'liberty': ['java', '-cp', '/Users/eric/git/vim-liberty/jars/*', 'org.eclipse.lemminx.XMLServerLauncher']
                        \ }

let g:LanguageClient_loggingLevel = 'DEBUG'
let g:LanguageClient_loggingFile =  expand('~/.vim/LanguageClient.log')
let g:LanguageClient_serverStderr = expand('~/.vim/LanguageServer.log')

nnoremap <silent> <F4> :call LanguageClient#textDocument_hover()<CR>
nnoremap <silent> <F3> :call LanguageClient#textDocument_completion()<CR>
```
