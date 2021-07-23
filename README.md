# ansible


***KOMENDY ADHOC:

ansible -i ./inventory.yml ubuntu -m setup | grep bios

#Instalowanie:
ansible -i ./inventory.yml ubuntu -b -m apt -a "name=apache2 state=latest"

#ODINSTALOWANIE:
ansible -i ./inventory.yml ubuntu -b -m apt -a "name=apache2 state=absent"

#Filtrowanie setupa hosta:
ansible -i ./inventory.yml ubuntu -m setup -a filter=*ipv4*

#Zapis ustawień hosta do pliku:
ansible -i ./inventory.yml allhosts -m setup -tree example_facts

#Utworzenie pliku:
ansible -i ./inventory  allhosts -a "touch /home/ansible/plik.txt"

#Usuniecie pliku:
ansible -i ./inventory  allhosts -a "rm -f /home/ansible/plik.txt"

#Zapis do pliku echo:
ansible -i ./inventory  allhosts -m shell -a " echo Witaj >> /home/ansible/plik.txt"

#Tworzenie pliku w katalogu domowym:
ansible -i ./inventory allhosts -m file -a "name=kurs.txt state=touch"

#Usuwnie:
ansible -i ./inventory allhosts -m file -a "name=kurs.txt state=absent"

#Tworzenie pliku w wybranym katalogu:
ansible -i ./inventory allhosts -m file -a "name=/home/ansible/plik.txt state=touch"

#Kopiowanie pliku z nową nazwą:
ansible -i ./inventory allhosts -m copy -a "src=./old.txt dest=/home/ansible/new.txt"

#Pokazywanie zawartości plików:
ansible -i ./inventory allhosts -a "cat /home/ansible/plik.txt"
 
#Dodawanie nowych linii:
ansible -i inventory allhosts -m lineinfile -a " path=/home/ansible/plik.txt line='dodany ciag za pomoca lineinfile' state=present "
 
#Dodawanie nowych lini przed linią ze słowem "jak":
ansible -i inventory allhosts -m lineinfile -a " path=/home/ansible/plik.txt line='dodana' state=present  insertafter='jak' "

#Usuwanie linii:
ansible -i inventory allhosts -m lineinfile -a " path=/home/ansible/plik.txt line='dodana' state=absent"

#Zapisywanie pliku URL:
ansible -i ./inventory allhosts -m get_url -a "url=http://google.com dest=/home/ansible/google.html"

#Usuwanie pliku:
ansible -i ./inventory allhosts -a "rm /home/ansible/google.html"
 
#Tworzenie folderu:
ansible -i ./inventory allhosts -m file -a "path=/home/ansible/new_dir state=directory"
 
#Tworzenie pliku
ansible -i ./inventory allhosts -m file -a "path=/home/ansible/plik.txt state=touch" 
 
#Przenoszenie pliku:
ansible -i ./inventory allhosts -a "mv /home/ansible/plik.txt /home/ansible/new_dir"
 
#Tworzenie archiwum:
ansible -i ./inventory allhosts -m archive -a "path=/home/ansible/new_dir format=zip dest=/home/ansible/archive.zip"

#Rozpowkowywanie archiwum:
ansible -i ./inventory allhosts -m unarchive -a " remote_src=yes src=/home/ansible/archive.zip dest=/home/ansible/new_dir"
 
#Instalacja unzip na centoesie:
ansible -i ./inventory centos -b -m yum -a "name=zip state=latest"

#Instalacja unzip na ubuntu:
ansible -i ./inventory ubuntu -b -m apt -a "name=zip state=latest"

#Tworzenie użytkownika (musi być -b bo to daje uprawnienia root):
ansible -i ./inventory allhosts -m user -b -a " name=ansible-user state=present "

#Usunięcie użytkownika z katalogiem domowym:
ansible -i ./inventory allhosts -m user -b -a " name=ansible-user state=absent remove=yes "

#Dodanie grup:
ansible -i ./inventory allhosts -b -m group -a "name=new-group"

#Dodanie użytkownika do gupy:
ansible -i ./inventory allhosts -b -m user -a "name=ansible-user state=present group=new-group"

#Dodanie użytkoniwka do dotatkowej grup:
ansible -i ./inventory allhosts -b -m user -a "name=ansible-user state=present group=new-group append=yes"

#sprawdzenie grupy użtkownika:
id nazwa_użtkownika 

#Usuwnie grupy: (użytkownik nie możę być domyślnie w tej grupie)
ansible -i ./inventory allhosts -b -m group -a "name=secound-group state=absent"

#Pakiety: instalacja na systemach, trzeba patrzeć na nazwy bo httpd jest na centos i apache na ubuntu
ansible -i ./inventory allhosts -b  -m package -a " name=ntpdate state=latest"

#Uruchomianie usługi:
ansible -i ./inventory ubuntu -b -m service -a "name=apache2 state=started enabled=yes"

#Odinstalowanie usługi:
ansible -i ./inventory centos -b  -m package -a " name=httpd state=absent"

#Sprawdzenie usługi:
ansible -i ./inventory centos -m shell -a "systemctl status httpd"

#Zmiana portu w httpd: 
ansible -i inventory centos -b -m lineinfile -a " path=/etc/httpd/conf/httpd.conf line='Listen 81' regexp='Listen (.*)' backrefs=yes state=present"

#Zatrzymywanie usługi:
ansible -i inventory centos -b -m service -a "name=httpd state=stopped enabled=yes"

*** PLAYBOOKI:

#Uruchamianie playbooków:
ansible-playbook -i ./inventory1.yml playbook2.yml 

#Uruchamianie playbooka z tagiem:
ansible-playbook -i ./inventory tags.yml --tags software
ansible-playbook -i ./inventory1.yml tags.yml --tags copy
ansible-playbook -i ./inventory1.yml tags.yml --tags copy,files

#Szyfrowanie hasła:
sudo ansible-vault encrypt ./vault.yml 

#Zobaczenie hasła:
ansible-vault edit ./vault.yml 
sudo ansible-vault view ./vault.yml Vault

#Oszyfrowanie:
ansible-vault decrypt ./vault.yml 

#Wysyłanie hasła do hosta:
ansible-playbook -i ./inventory1.yml  --vault-id dane@prompt ./vault2.yml 

#Wysłanie do host i zapytanie hasła:
ansible-playbook -i ./inventory1.yml  --ask-vault-pass   ./vault2.yml 








