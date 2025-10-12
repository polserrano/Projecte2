# Seguretat Lògica: recuperant accés a sistemes
**Autor:** Pol Serrano Aromí  
**Data:** 7/10/2025  

---

## 1. Índex
1. [Introducció a la tasca](#2-introducció-a-la-tasca)  
2. [Procediment pas a pas](#3-procediment-pas-a-pas)  
   - [Crear i preparar la MV](#31-crear-i-preparar-la-mv)  
   - [Entrar en el GRUB](#32-entrar-en-el-grub)  
   - [Comandes](#33-comandes)  
   - [Fortificació de l’accés a GRUB](#34-fortificació-de-laccés-a-grub)  
     - [Accedir com a root i generar un hash per el GRUB](#341-accedir-com-a-root-i-generar-un-hash-per-el-grub)  
     - [Crear l’arxiu de seguretat de GRUB](#342-crear-larxiu-de-seguretat-de-grub)  
     - [Fortificació Opcional](#343-fortificació-opcional)  
   - [Webgrafia](#34-webgrafia)  
3. [Conclusió de la tasca](#4-conclusió-de-la-tasca)  

![imatge_1](imatge_1.png)
---

## 2. Introducció a la tasca

L’objectiu d’aquesta pràctica és recuperar l’accés a un portàtil amb **Zorin OS (Linux)** on el directiu ha oblidat la contrasenya.  
Per no danyar l’equip original, treballem amb un **disc clònic en una màquina virtual**.

El procés té dues parts principals:

1. **Recuperació d’accés:**
   - Connectar el disc clònic a una màquina virtual.  
   - Accedir al sistema mitjançant el GRUB i el mode de recuperació.  
   - Localitzar l’usuari i restablir la seva contrasenya.  
   - Verificar que podem entrar al sistema.

2. **Fortificació del sistema:**
   - Investigar i aplicar mesures per protegir el GRUB amb contrasenya.  
   - Documentar el procés (amb captures i fonts d’informació).

---

## 3. Procediment pas a pas

### 3.1 Crear i preparar la MV

Primer de tot, crearem el nou entorn, desde VirtualBox, la meva recomanació és posar 8192 mb de ram (8GB) amb dos processadors per tampoc anar justos, ja que es una interfície d'entorn gràfic i consumeix bastant. Seguidament, el pas important, serà importar el disc virtual, on abans de donarli a ‘Finish’ al crar la màquina, anirem al apartat ‘Hard Disk’ i afegirem un disc virtual, com surt a la imatge.

![imatge_1](imatge_2.png)

---

### 3.2 Entrar en el GRUB

Per entrar en aquests mode, haurem de iniciar la màquina i un cop iniciada, prémerla tecla Mayús + una lletra, un cop us apareix el menu, li donarem a la segona opció, per entrar a les opcions avançades.

![imatge_2](imatge_3.png)

Un cop hagué fet click a la segona opció, us sortira un altre pop-up, on li tindrem que donar a la segona opcio, on sera basicament el recovery mode (mode de recuperació), un cop hageu fet un click, estarem en les opcions del mode de
recuperació, en la nostra pràctica, la opció més adient és la de ‘root’, per això la seleccionarem.

![imatge_3](imatge_4.png)

Un cop seleccionada la opció, oficialment, ens trobarem en la terminal amb el mode de root, això ens permetrà cambiar paràmetres i diferentes opcions. Aquests és l'aspecte de la terminal root.

![imatge_4](imatge_5.png)

---

### 3.3 Comandes

3.3.1. Per defecte el sistema que arrenca amb init=/bin/bash està muntat en mode només lectura, així que phaurem de remuntar-ho per poder fer canvis:

      mount -rw -o remount /

![imatge_3](imatge_6.png)

3.3.2. Un cop fet això, ara podrem veure que tenim accés al sistemes de fitxers amb la comanda:

      ls /
      
![imatge_3](imatge_7.png)

3.3.3. Seguidament, localitzarem el nom d’usuari, per posteriorment canviar la contrasenya, per aixo ho farem amb una comanda molt bàsica:

      cat /etc/passwd | grep ‘home’

![imatge_3](imatge_8.png)

I com podem veure gràcies a la comanda, el nom de l'usuari és: miquel.

3.3.4. Per finalitzar, canviarem el password de l'usuari amb la comanda: ‘passwd miquel’ (en el nostre cas). I podrem canviar fàcilment, com es mostra en la imatge, si no recordo malament, el password haurà de tenir un mínim de 8 caràcters, finalment com podem veure la contrasenya s’ha actualitzat correctament.

![imatge_3](imatge_9.png)

---

### 3.4 Fortificació de l’accés a GRUB

En aquesta part configurarem una contrasenya al GRUB per evitar que qualsevol usuari pugui entrar al mode de recuperació o editar les opcions d’arrencada. Això permet protegir el sistema davant accessos no autoritzats.

Per fortificar l’accés ho farem mitjançant uns varios pasos.

3.4.1. Accedir com a root i generar un hash per el GRUB

Per això primer de tot obrirem una terminal i introduirem la comanda: ‘sudo -i’, araestarem amb root, despres executarem la comanda: ‘grub-mkpasswd-pbkdf2’ ja que zorin no desa els password en text pla, ja que té un sistema en format encriptat per
motius de seguretat.

![imatge_3](imatge_10.png)

Veurem que ens donarà una serie de numeros y lletres en mayus, aixo es el password pero encriptat. Llavors aquella serie de codis encriptats els copiarem per després enganxar-los a l’arxiu que crearem per fortificar l’accés.

3.4.2. Crear l’arxiu de seguretat de GRUB

Primer de tot crear l’arxiu amb el nom: ‘arxiu_fortificacio’, llavors farem un nano, amb la comanda: ‘sudo nano /etc/grub.d/arxiu_fortificacio’ un cop dins de l’arxiu nano, introduirem el script que he pogut trobar en una pàgina.

      ‘set superusers="admin"
      password_pbkdf2 admin grub……’

![imatge_3](imatge_11.png)

Un cop actualitzat el codi, el guardarem ‘Ctrl+x’ i introduirem la comanda: ‘sudo update-grub’ per regenerar la configuració nova del grub.

Un cop creat l’arxiu, podrem veure que quan reiniciem la màquina i si algú vol entrar
al GRUB per canviar algún paràmetre ens demanarà el password que hem introduït
en l’arxiu nano.

3.4.3 Fortificació Opcional

També dins de l’arxiu nano que hem creat anteriorment podem afegir un codi molt bàsic que també he trobat en una pàgina, que bàsicament el que fa és, que només l’usuari amb el rol de admin pugui arrencar certes entrades. Llavors el codi que hem de afegir és el següent:

      menuentry "Zorin OS - Mode Recuperació" --unrestricted {
      set root=(hd0,1)
      linux /boot/vmlinuz-... root=/dev/sda1 ro recovery nomodeset
      initrd /boot/initrd.img-...
      }

![imatge_3](imatge_12.png)

I finalment, quedaría el script amb les millores opcionals algo així, un cop finalitzat, recordem, el guardarem ‘Ctrl+x’ i introduirem la comanda: ‘sudo update-grub’ per regenerar la configuració nova del grub. Amb això tindrem completament el nostre OS de Zorin completament fortificat.

---

### 4. Webgrafia

Per acabar us deixo les webs amb les que he anat reawlitzant la tasca:

   ● https://www.gnu.org/software/grub/manual/grub/grub.html#Authentication
   ● https://help.ubuntu.com/community/Grub2/Passwords
   ● https://zorin.com/help/
   ● https://www.digitalocean.com/community/tutorials/how-to-secure-grub

--- 

### 5. Conclusió de la tasca

Amb aquesta pràctica hem aconseguit recuperar l’accés a un sistema Zorin OS mitjançant el mode de recuperació del GRUB i restablir la contrasenya de l’usuari. A més, hem après a fortificar el gestor d’arrencada configurant una contrasenya al
GRUB.

En definitiva, s’ha demostrat la importància de conèixer tant els mecanismes de recuperació com les mesures de seguretat necessàries per protegir un sistema Linux davant accessos no autoritzats.
