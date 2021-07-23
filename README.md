# Ansible


#### KOMENDY ADHOC:

```
ansible -i ./inventory.yml ubuntu -m setup | grep bios
```
##### 1. Instalowanie:
```
ansible -i ./inventory.yml ubuntu -b -m apt -a "name=apache2 state=latest"
```
##### 2. Odinstalowanie:
```
ansible -i ./inventory.yml ubuntu -b -m apt -a "name=apache2 state=absent"
```
##### 3. Filtrowanie setupa hosta:
```
ansible -i ./inventory.yml ubuntu -m setup -a filter=*ipv4*
```
##### 4. Zapis ustawień hosta do pliku:
```
ansible -i ./inventory.yml allhosts -m setup -tree example_facts
```
##### 5. Utworzenie pliku:
```
ansible -i ./inventory  allhosts -a "touch /home/ansible/plik.txt"
```
##### 6. Usuniecie pliku:
```
ansible -i ./inventory  allhosts -a "rm -f /home/ansible/plik.txt"
```
##### 7. Zapis do pliku echo:
```
ansible -i ./inventory  allhosts -m shell -a " echo Witaj >> /home/ansible/plik.txt"
```
##### 8. Tworzenie pliku w katalogu domowym:
```
ansible -i ./inventory allhosts -m file -a "name=kurs.txt state=touch"
```
##### 9. Usuwnie:
```
ansible -i ./inventory allhosts -m file -a "name=kurs.txt state=absent"
```
##### 10. Tworzenie pliku w wybranym katalogu:
```
ansible -i ./inventory allhosts -m file -a "name=/home/ansible/plik.txt state=touch"
```
##### 11. Kopiowanie pliku z nową nazwą:
```
ansible -i ./inventory allhosts -m copy -a "src=./old.txt dest=/home/ansible/new.txt"
```
##### 12. Pokazywanie zawartości plików:
```
ansible -i ./inventory allhosts -a "cat /home/ansible/plik.txt"
``` 
##### 13. Dodawanie nowych linii:
```
ansible -i inventory allhosts -m lineinfile -a " path=/home/ansible/plik.txt line='dodany ciag za pomoca lineinfile' state=present "
``` 
##### 14. Dodawanie nowych lini przed linią ze słowem "jak":
```
ansible -i inventory allhosts -m lineinfile -a " path=/home/ansible/plik.txt line='dodana' state=present  insertafter='jak' "
```
##### 15. Usuwanie linii:
```
ansible -i inventory allhosts -m lineinfile -a " path=/home/ansible/plik.txt line='dodana' state=absent"
```
##### 16. Zapisywanie pliku URL:
```
ansible -i ./inventory allhosts -m get_url -a "url=http://google.com dest=/home/ansible/google.html"
```
##### 17. Usuwanie pliku:
```
ansible -i ./inventory allhosts -a "rm /home/ansible/google.html"
``` 
##### 18. Tworzenie folderu:
```
ansible -i ./inventory allhosts -m file -a "path=/home/ansible/new_dir state=directory"
``` 
##### 19. Tworzenie pliku:
```
ansible -i ./inventory allhosts -m file -a "path=/home/ansible/plik.txt state=touch" 
``` 
##### 20. Przenoszenie pliku:
```
ansible -i ./inventory allhosts -a "mv /home/ansible/plik.txt /home/ansible/new_dir"
``` 
##### 21. Tworzenie archiwum:
```
ansible -i ./inventory allhosts -m archive -a "path=/home/ansible/new_dir format=zip dest=/home/ansible/archive.zip"
```
##### 22. Rozpowkowywanie archiwum:
```
ansible -i ./inventory allhosts -m unarchive -a " remote_src=yes src=/home/ansible/archive.zip dest=/home/ansible/new_dir"
``` 
##### 23. Instalacja unzip na centoesie:
```
ansible -i ./inventory centos -b -m yum -a "name=zip state=latest"
```
##### 24. Instalacja unzip na ubuntu:
```
ansible -i ./inventory ubuntu -b -m apt -a "name=zip state=latest"
```
##### 25. Tworzenie użytkownika (musi być -b bo to daje uprawnienia root):
```
ansible -i ./inventory allhosts -m user -b -a " name=ansible-user state=present "
```
##### 26. Usunięcie użytkownika z katalogiem domowym:
```
ansible -i ./inventory allhosts -m user -b -a " name=ansible-user state=absent remove=yes "
```
##### 27. Dodanie grup:
```
ansible -i ./inventory allhosts -b -m group -a "name=new-group"
```
##### 28. Dodanie użytkownika do gupy:
```
ansible -i ./inventory allhosts -b -m user -a "name=ansible-user state=present group=new-group"
```
##### 29. Dodanie użytkoniwka do dotatkowej grup:
```
ansible -i ./inventory allhosts -b -m user -a "name=ansible-user state=present group=new-group append=yes"
```
##### 30. Sprawdzenie grupy użtkownika:
```
id nazwa_użtkownika 
```
##### 31. Usuwnie grupy: (użytkownik nie możę być domyślnie w tej grupie):
```
ansible -i ./inventory allhosts -b -m group -a "name=secound-group state=absent"
```
##### 32. Pakiety: instalacja na systemach, trzeba patrzeć na nazwy bo httpd jest na centos i apache na ubuntu:
```
ansible -i ./inventory allhosts -b  -m package -a " name=ntpdate state=latest"
```
##### 33. Uruchomianie usługi:
```
ansible -i ./inventory ubuntu -b -m service -a "name=apache2 state=started enabled=yes"
```
##### 34. Odinstalowanie usługi:
```
ansible -i ./inventory centos -b  -m package -a " name=httpd state=absent"
```
##### 35. Sprawdzenie usługi:
```
ansible -i ./inventory centos -m shell -a "systemctl status httpd"
```
##### 36. Zmiana portu w httpd:
```
ansible -i inventory centos -b -m lineinfile -a " path=/etc/httpd/conf/httpd.conf line='Listen 81' regexp='Listen (.*)' backrefs=yes state=present"
```
##### 37. Zatrzymywanie usługi:
```
ansible -i inventory centos -b -m service -a "name=httpd state=stopped enabled=yes"
```

#### PLAYBOOKI:


##### 38. Uruchamianie playbooków:
```
ansible-playbook -i ./inventory1.yml playbook2.yml 
```
##### 39. Uruchamianie playbooka z tagiem:
```
ansible-playbook -i ./inventory tags.yml --tags software
ansible-playbook -i ./inventory1.yml tags.yml --tags copy
ansible-playbook -i ./inventory1.yml tags.yml --tags copy,files
```
##### 40. Szyfrowanie hasła:
```
sudo ansible-vault encrypt ./vault.yml 
```
##### 41. Zobaczenie hasła:
```
ansible-vault edit ./vault.yml 
sudo ansible-vault view ./vault.yml Vault
```
##### 42. Odszyfrowanie:
```
ansible-vault decrypt ./vault.yml 
```
##### 43. Wysłanie do host i zapytanie hasła:
```
ansible-playbook -i ./inventory1.yml  --vault-id dane@prompt ./vault2.yml 
```
##### 44. Wysłanie do host i zapytanie hasła:
```
ansible-playbook -i ./inventory1.yml  --ask-vault-pass   ./vault2.yml 
```







