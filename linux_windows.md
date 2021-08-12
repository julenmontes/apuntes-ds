| Mac / Chrome / Linux | Explanation | Explicación | Windows PowerShell |
|:---:|:---:|:---:|:---:|
| cd filepath | change directory, aka move into a different folder | c cambiar d irectorio, también conocido como mover a una carpeta diferente | cd filepath |
| ls | list the files and folders in your current directory | l i s t los archivos y carpetas en su actual dir ectory | ls / dir / Get-ChildItem |
| pwd | show path of working directory, aka the folder that you’re in right now | mostrar p ath de w RABAJAR d irectorio, también conocido como la carpeta que usted está en este momento | pwd / cd |
| touch filename | make a new file | hacer un nuevo archivo | ni filename |
| mkdir directory-name | make a new directory, aka a folder | m un k ae un nuevo dir ectory, también conocido como una carpeta | mkdir directory-name |
| rm filename | remove, aka delete, a file or directory | r e m ove, también conocido como eliminar, un archivo o directorio | rm filename / del filename |
| cp original-filename copied-filename | copy a file or directory | c o p y un archivo o directorio | cp / copy |
| mv original-filename new-filename | move or rename a file or directory | m o v e o cambiar el nombre de un archivo o directorio | move original-filename new-filename / ren original-filename new-filename |
| cat filename | show all the contents of a file | mostrar todo el contenido de un archivo | cat filename / type filename |
| less filename | show snippet of a file that allows you to scroll through the entire thing | muestra un fragmento de un archivo que te permite desplazarte por todo el contenido | more filename |
| head filename | show the first 10 lines of a file (change number of lines by adding -*a number* flag, e.g. head -100) | mostrar las primeras 10 líneas de un archivo (cambiar el número de líneas agregando una bandera, por ejemplo )-*a number*head -100 | gc filename -head 10 |
| tail filename | show the last 10 lines of a file (change number of lines by adding -*a number* flag, e.g. tail -100) | mostrar las últimas 10 líneas de un archivo (cambiar el número de líneas agregando una bandera, por ejemplo )-*a number*tail -100 | gc filename -tail 10 |
| wc -w -l filename | show how many words or lines in a file | mostrar cuántas w ords o líneas en un archivo | gc filename \| Measure-Object -Word –Line |
| man command | show the manual, aka the documentation that tells you what a particular command does | mostrar al hombre UAL, también conocido como la documentación que le dice lo que hace un comando en particular | help command |
| echo | print text to the command line | imprimir texto en la línea de comando | echo |
| grep “search term” filename or directory-name | search for lines that include search term in file | buscar líneas que incluyan un término de búsqueda en el archivo | findstr “search term” filename |
| curl -O url / wget url | get, a file from the web | obtener , un archivo de la w eb | wget url -OutFile new-filename |