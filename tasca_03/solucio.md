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

      ```bash
      mount -rw -o remount /

![imatge_3](imatge_6.png)




