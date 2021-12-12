Ex1.
----
[nastya@localhost ~]$ sudo groupadd -g 4000 sales
[nastya@localhost ~]$ sudo useradd -g sales bob
[nastya@localhost ~]$ sudo useradd -g sales alice
[nastya@localhost ~]$ sudo useradd -g sales eve
[nastya@localhost ~]$ sudo passwd bob
[nastya@localhost ~]$ sudo passwd alice
[nastya@localhost ~]$ sudo passwd eve
----------------------------------------------------------
Заставьте пользователей сменить пароль после первого логина:
[nastya@localhost ~]$ sudo chage -d 0 bob
[nastya@localhost ~]$ sudo chage -d 0 alice
[nastya@localhost ~]$ sudo chage -d 0 eve
--------------------------------------------
Заставить пользователя менять свой пароль каждые n дней:
[nastya@localhost ~]$ sudo chage -M 30 alice
[nastya@localhost ~]$ sudo chage -M 30 eve
[nastya@localhost ~]$ sudo chage -M 15 bob
--------------------------------------------
Установка срока истечения действия аккаунта пользователя:
[nastya@localhost ~]$ sudo chage -E `date -d "90 days" +"%Y-%m-%d"` bob
[nastya@localhost ~]$ sudo chage -E `date -d "90 days" +"%Y-%m-%d"` alice
[nastya@localhost ~]$ sudo chage -E `date -d "90 days" +"%Y-%m-%d"` eve
-----------------------------------


Ex2.
[nastya@localhost ~]$ sudo useradd glen
[nastya@localhost ~]$ sudo useradd antony
[nastya@localhost ~]$ sudo useradd lesly
---
Посмотреть список пользователей:
[nastya@localhost ~]$ less /etc/passwd
------
[nastya@localhost ~]$ sudo mkdir /home/students

[nastya@localhost students]$ sudo groupadd students
---
[nastya@localhost ~]$ sudo usermod -a -G students glen
[nastya@localhost ~]$ sudo usermod -a -G students antony
[nastya@localhost ~]$ sudo usermod -a -G students lesly
[nastya@localhost ~]$ sudo chown :students /home/students
--
[nastya@localhost ~]$ sudo chmod g+s /home/students
После этого подкаталоги/файлы будут наследовать группу и бит setgid.

[nastya@localhost ~]$ sudo chmod -R u=rwX,g=rwX,o-rwx /home/students 
запрещаем остальным пользователям доступ к директории и ее файлам(o-rwx)

---------------------------------
ex3.

[nastya@localhost share]$ mkdir cases
[nastya@localhost cases]$ sudo touch murders.txt moriarty.txt

[nastya@localhost ~]$ sudo groupadd bakerstreet
[nastya@localhost ~]$ sudo groupadd scotlandyard

[nastya@localhost ~]$ sudo useradd holmes
[nastya@localhost ~]$ sudo useradd watson
[nastya@localhost ~]$ sudo useradd lestrade
[nastya@localhost ~]$ sudo useradd gregson
[nastya@localhost ~]$ sudo useradd jones
[nastya@localhost ~]$ sudo usermod -a -G bakerstreet holmes
[nastya@localhost ~]$ sudo usermod -a -G bakerstreet watson
[nastya@localhost ~]$ sudo usermod -a -G scotlandyard lestrade
[nastya@localhost ~]$ sudo usermod -a -G scotlandyard gregson
[nastya@localhost ~]$ sudo usermod -a -G scotlandyard jones

Создать пользователям безопасный пароль:
[nastya@localhost ~]$ sudo apt install pwgen
[nastya@localhost ~]$ pwgen -n 10
[nastya@localhost ~]$ sudo passwd holmes
[nastya@localhost ~]$ sudo passwd watson
[nastya@localhost ~]$ sudo passwd lestrade
[nastya@localhost ~]$ sudo passwd gregson
[nastya@localhost ~]$ sudo passwd jones
--
[nastya@localhost share]$ sudo chown :bakerstreet cases
[nastya@localhost share]$ sudo chmod -R 000 cases
[nastya@localhost share]$ sudo setfacl -R -m g:scotlandyard:rwX cases
[nastya@localhost share]$ sudo setfacl -R -m g:bakerstreet:rwX cases
[nastya@localhost share]$ sudo setfacl -R -m u:jones:rX cases
[nastya@localhost share]$ sudo setfacl -m d:g:bakerstreet:rwX cases
[nastya@localhost share]$ sudo setfacl -m d:g:scotlandyard:rwX cases
[nastya@localhost share]$ sudo setfacl -m d:u:jones:rX cases
----------------

Проверка всех настроек
------------
[holmes@localhost cases]$ vim murders.txt
[holmes@localhost cases]$ touch t.txt
[holmes@localhost cases]$ cat murders.txt
!!!!!!!!!!!!!!!!!!!!11116688
[holmes@localhost cases]$ rm t.txt
----
[jones@localhost share]$ cd cases
[jones@localhost cases]$ cat murders.txt
!!!!!!!!!!!!!!!!!!!!11116688
[jones@localhost cases]$ touch m.txt
touch: cannot touch ‘m.txt’: Permission denied
--
[gregson@localhost share]$ cat cases/murders.txt
!!!!!!!!!!!!!!!!!!!!11116688
[gregson@localhost share]$ cd cases
[gregson@localhost cases]$ mkdir tk
[gregson@localhost cases]$ touch r.txt
----
[watson@localhost share]$ cd cases
[watson@localhost cases]$ vim murders.txt
[watson@localhost cases]$ touch task.txt
[watson@localhost cases]$ cat murders.txt
hello!
[watson@localhost cases]$ rm task.txt
---
[lestrade@localhost cases]$ rm r.txt
rm: remove write-protected regular empty file ‘r.txt’?
[lestrade@localhost cases]$ rmdir tk



