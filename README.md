# vim-liberty

1. `git clone https://github.com/OpenLiberty/liberty-language-server`

2. `cd liberty-language-server`

3. `./mvnw package`

4. `cd ..`

5. `git clone https://github.com/eclipse/lemminx`

6. `cd lemminx`

7. `.mvnw package -DskipTests`

8. Copy `liberty-language-server/lemminx-liberty/target/lemminx-liberty-1.0-SNAPSHOT-jar-with-dependencies.jar` and `lemminx/org.eclipse.lemminx/target/org.eclipse.lemminx-uber.jar` to a common folder e.g. `~/liberty-ls/jars.

9. Install vim-plug https://github.com/junegunn/vim-plug

```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

10. Edit ~/.vimrc (edit path in `let g:LanguageClient_serverCommands` with the folder in step 8)

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
\ 'liberty': ['java', '-cp', '~/liberty-ls/jars/*', 'org.eclipse.lemminx.XMLServerLauncher']
                        \ }

let g:LanguageClient_loggingLevel = 'DEBUG'
let g:LanguageClient_loggingFile =  expand('~/.vim/LibertyLanguageClient.log')
let g:LanguageClient_serverStderr = expand('~/.vim/LibertyLanguageServer.log')

nnoremap <silent> <F4> :call LanguageClient#textDocument_hover()<CR>
nnoremap <silent> <F3> :call LanguageClient#textDocument_completion()<CR>
```

11. Run `vim +PlugInstall +qall`

12. Open a Liberty server.xml with Vim e.g. https://github.com/OpenLiberty/demo-devmode/blob/master/src/main/liberty/config/server.xml
