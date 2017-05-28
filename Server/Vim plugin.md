# Vim plugin

Vim을 편리하게 사용하기 위해 플러그인들을 설치한다.

플러그인을 관리하는 플러그인(?)이 Vundle 인가 하나만 있는 줄 알았는데, 뭔가 심오한 세계가 있는 것 같다.

찾아본 바

* Vundle
* NeoBundle
* VimPlug
* Pathogen

이 있는걸로 생각된다.

그래서 처음에 삽질끝에 Vundle을 설치하고 vim powerline 을 사용하는 데 까지 성공했으나 사용하려는 찰나에

더 빠르다는 VimPlug의 존재를 발견하게 되고 다시 삽질 중 이다.

그래서 VimPlug를 설치하려고 해본다.



주소는 

[https://github.com/junegunn/vim-plug](https://github.com/junegunn/vim-plug)

* 설치

~~~shell
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
~~~

* 간단한 설명

~~~shell
# .vimrc에 설정을 해주어야 한다
# example

call plug#begin()
Plug 'tpope/vim-sensible' -> 처럼 설치하고 싶은 플러그인을 적어주고 :PlugInstall
call plug#end()
~~~

* 적용할 땐


~~~shell
:source %
:PlugInstall
~~~




#### 자주 사용하는 플러그인

* NERDTree

윈도우 탐색기처럼 Tree형태로 볼 수 있는 듯 하다

[http://vimawesome.com/plugin/nerdtree-red](http://vimawesome.com/plugin/nerdtree-red) 참고

~~~shell
Plug 'scrooloose/nerdtree'
~~~

~~~shell
# vim 실행시 자동으로 NERDTree 실행
autocmd vimenter * NERDTree

# vim 실행 후 파일이 없을 때 자동실행
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists("s:std_in") | NERDTree | endif

# 디렉토리를 열때 자동실행
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 1 && isdirectory(argv()[0]) && !exists("s:std_in") | exe 'NERDTree' argv()[0] | wincmd p | ene | endif
~~~

* vim-powerline

zsh agnoster 처럼 vim 에도 powerline 을 만들어 준다

~~~shell
Plug 'powerline/powerline'
~~~

~~~shell
# 처음에 실행하면 윈도우가 하나일때는 활성화가 안되는데
# 그 이유는 laststatus 때문에 그렇다
:h 'laststatus' # 를 통해서 도움을 얻고
# .vimrc에 
set laststatus=2 # 를 설정해주면 윈도우가 한 개 일때고 활성화가 된다
~~~

