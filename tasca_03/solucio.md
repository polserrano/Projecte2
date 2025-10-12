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

Primer de tot, crearem el nou entorn des de **VirtualBox**.  
La meva recomanació és posar **8192 MB de RAM (8 GB)** amb **dos processadors**, ja que és una interfície gràfica i consumeix bastant.

Abans de donar-li a `Finish` al crear la màquina, anirem a l’apartat **Hard Disk** i afegirem un disc virtual, com es mostra a la imatge següent:

![imatge_1](imatge_1)

---

### 3.2 Entrar en el GRUB

Per entrar en aquest mode, haurem d’iniciar la màquina i, un cop iniciada, **prémer la tecla Mayús + una lletra**.  
Quan aparegui el menú, seleccionarem la **segona opció** per entrar a les opcions avançades.

![imatge_2](imatge_2)

Després, apareixerà un altre pop-up, on seleccionarem de nou la **segona opció**, que correspon al **recovery mode**.  
Dins d’aquest mode, triarem l’opció **root**, que ens permetrà canviar paràmetres del sistema.

![imatge_3](imatge_3)

Ara ens trobem a la **terminal com a root**, des d’on podem executar canvis al sistema.

![imatge_4](imatge_4)

---

### 3.3 Comandes

1. El sistema arrenca amb `init=/bin/bash` en mode només lectura.  
   Per poder fer canvis, cal remuntar-lo en mode escriptura:

   ```bash
   mount -rw -o remount /
