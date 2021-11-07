
 # Домашнее задание к занятию «2.4. Инструменты Git»
### 1
Для поиска хэша и комментария коммита **aefea** воспользуемся командой:  
`git show aefea`  
Из вывода возьмем ответ:  
Хэш: **aefead2207ef7e2aa5dc81a34aedf0cad4c32545**  
Комментарий: **Update CHANGELOG.md**  
### 2
Для поиска тэга коммита воспользуемся командой:   
`git show 85024d3`  
В выводе находим ответ, что коммиту **85024d3** соответствует тэг **v0.12.23**  
### 3
Для поиска родителей коммита **b8d720** для начала запустим:  
`git show b8d720`  
В выводе по строке **Merge** понимаем, что это мерж-коммит. В этой же строке **Merge: 56cd7859e 9ea88f22f** - указаны начальные хэши родителей (коммиты слиянием которых, образовался коммит **b8d720**).  
С помощью `git show 56cd7859e 9ea88f22f` можно увидеть полую информацию о коммитах-родителях. 
Так же можно воспользоваться командами `git show "b8d720^"` и `git show "b8d720^2"` (кавычки, потому что использую git в командной строке windows).  
Ответ:  
2 родителя, т.к. **b8d720** – мерж-коммит.   
Полный хэш 1 родителя: **56cd7859e05c36c06b56d013b55a252d0bb7e158**   
Полный хэш 2 родителя: **9ea88f22fc6269854151c571162c5bcf958bee2b**   
### 4
Для поиска хэшев и комментариев между тэгами используем команду:  
`git log v0.12.23..v0.12.24 --oneline` 
В выводе получим ответ:  
**33ff1c03b (tag: v0.12.24) v0.12.24  
b14b74c49 [Website] vmc provider links  
3f235065b Update CHANGELOG.md  
6ae64e247 registry: Fix panic when server is unreachable  
5c619ca1b website: Remove links to the getting started guide's old location  
06275647e Update CHANGELOG.md  
d5f9411f5 command: Fix bug when using terraform login on Windows 
4b6d06cc5 Update CHANGELOG.md  
dd01a3507 Update CHANGELOG.md  
225466bc3 Cleanup after v0.12.23 release**  
### 5
Для поиска коммита создания функции **func providerSource** выполняем:  
`git log -S "func providerSource" --oneline`   
Первый коммит из вывода и есть коммит создания нужной нам функции:  
**8c928e835 main: Consult local directories as potential mirrors of providers**  
### 6
Для поиска именно изменений функции **globalPluginDirs** необходимо понимать в каком файле данная функция используется. Для этого сначала найдем, когда она была создана:   
`git log -S "func globalPluginDir" –oneline`  
Из вывода берем хэш коммита в котором функция появилась первый раз и запускаем  
`git show 8364383c3`
В выводе находим файл в котором данная функция прописана - **plugins.go**. Запускаем поиск изменений в функции:  
`git log -L :globalPluginDir:plugins.go`  
В выводе можем увидеть все коммиты, в которых была изменена функция:  
**commit 78b12205587fe839f10d946ea3fdc06719decb05  
commit 52dbf94834cb970b510f2fba853a5b49ad9b1a46  
commit 41ab0aef7a0fe030e84018973a64135b11abcd70  
commit 66ebff90cdfaa6938f26f908c7ebad8d547fea17  
commit 8364383c359a6b738a436d1b7745ccdce178df47**  
### 7
Чтобы определить автора функции **synchronizedWriters*** для начала найдем, когда она была создана:  
`git log -S "func synchronizedWriters" –oneline`  
Из вывода берем первый коммит **5ac311e2a** и запускаем:  
`git show 5ac311e2a`  
В выводе автором коммита числится **Martin Atkins**, скорее всего он же и автор функции, но убедимся в этом наверняка. В этом же выводе видим, что был создан файл **synchronized_writers.go** в котором и фигурирует нужная нам функция. Запустив `git blame synchronized_writers.go` получаем ошибку, т.к. файл, а с ним и функция, в дальнейшем были удалены. Чтобы посмотреть содержимое файла **synchronized_writers.go** и найти виновного переключимся на коммит **5ac311e2a** когда был создан файл с функцией:  
`git checkout 5ac311e2a`  
И теперь запускаем:  
`git blame synchronized_writers.go`
В выводе видно что все строки файла **synchronized_writers.go** писал **Martin Atkins**, следовательно он – автор функции **synchronizedWriters**.

