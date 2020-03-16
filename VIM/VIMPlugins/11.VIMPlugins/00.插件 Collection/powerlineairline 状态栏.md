powerline/airline 状态栏

"""""""""""""status line"""""""""""""""""""""""
"powerline{
"set guifont=PowerlineSymbols\ for\ Powerline
"set nocompatible
"set laststatus=2
"set fillchars+=stl:\ ,stlnc:\
"let g:Powerline_symbols = 'fancy'
"}

"airline{
  let g:airline_section_b = '%{strftime("%c")}'
  let g:airline#extensions#tabline#enabled = 1
  let g:airline#extensions#tabline#left_sep = ' '
  let g:airline#extensions#tabline#left_alt_sep = '|'
  let g:airline_powerline_fonts = 1
  let g:airline_theme = 'dark'
  set laststatus=2
"}