# Servicios en Red - Prácticas VirtualBox

## Autor
Profesor: Alfonso Serrano  
Módulo: Servicios en Red (Semipresencial)  
Fecha: 2025  

---

# 📑 Índice

1. [Práctica 1: Instalación de Ubuntu Server en VirtualBox](#práctica-1-instalación-de-ubuntu-server-en-virtualbox)  
2. [Práctica 2: Instalación de Ubuntu Desktop en VirtualBox](#práctica-2-instalación-de-ubuntu-desktop-en-virtualbox)  
3. [Práctica 3: Configuración de red entre Server y Desktop](#práctica-3-configuración-de-red-entre-server-y-desktop)  

---

# Práctica 1: Instalación de Ubuntu Server en VirtualBox

## 🎯 Objetivo
Aprender a instalar y configurar una máquina virtual con **Ubuntu Server 22.04** utilizando VirtualBox. Esta máquina se usará en el módulo de *Servicios en Red* para realizar prácticas durante el curso.

### 1. 📥 Preparación previa
Qué necesitas:
1. [VirtualBox](https://www.virtualbox.org/wiki/Downloads) instalado en tu ordenador.  
2. [Imagen ISO de Ubuntu Server](https://ubuntu.com/download/server).  
3. Un equipo con al menos:  
   - RAM: 4 GB (se recomienda 8 GB).  
   - Espacio en disco: mínimo 20 GB libres.  
   - Procesador compatible con virtualización.  

### 2. 🖥️ Crear la máquina virtual en VirtualBox
1. Abre VirtualBox y pulsa **Nueva**.  
2. Pon un nombre descriptivo, por ejemplo: `UbuntuServer22`.  
3. Selecciona: Tipo: Linux, Versión: Ubuntu (64-bit).  
4. Elige la ubicación donde se guardará la máquina virtual.  
5. Haz clic en **Siguiente**.  

### 3. ⚙️ Configuración de la máquina virtual
**Memoria RAM**: al menos 2048 MB (mejor 4096 MB).  
**Disco duro**:  
- Crear disco virtual ahora.  
- Formato VDI.  
- Dinámico.  
- Tamaño: 20 GB o más.  

### 4. 📂 Cargar la ISO
1. Configuración → Almacenamiento.  
2. Seleccionar ISO de Ubuntu Server descargada.  

### 5. ▶️ Iniciar la instalación
- Idioma: Español.  
- Teclado: Español.  
- Red: por defecto.  
- Usuario: crea usuario y contraseña.  
- Disco: selecciona el virtual.  
- Instala solo lo básico.  

⚠️ No instales entorno gráfico.  

### 6. ✅ Finalización
- Reinicia y entra con tu usuario.  
- Prueba:  
```bash
ls
```

### 7. 📌 Consejos
- Crear **snapshot** tras la instalación.  
- Usar solo la terminal.  

---

# Práctica 2: Instalación de Ubuntu Desktop en VirtualBox

## 🎯 Objetivo
Aprender a instalar y configurar una máquina virtual con **Ubuntu Desktop 24.04** utilizando VirtualBox para trabajar con entorno gráfico.

### 1. 📥 Preparación previa
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads).  
- [Ubuntu Desktop 24.04](https://ubuntu.com/download/desktop).  
- RAM: 4 GB mínimo (8 GB recomendado).  
- Disco: 25 GB mínimo.  

### 2. 🖥️ Crear la máquina virtual
1. Nueva → Nombre: UbuntuDesktop24.  
2. Tipo: Linux, Versión: Ubuntu (64-bit).  
3. Configuración de disco y RAM igual que en Ubuntu Server.  

### 3. ⚙️ Configuración
- RAM: 4096 MB o más.  
- Disco duro: VDI, dinámico, 25 GB.  

### 4. 📂 Cargar la ISO
Seleccionar ISO de Ubuntu Desktop descargada.  

### 5. ▶️ Iniciar la instalación
- Idioma: Español.  
- Teclado: Español.  
- Selecciona **Instalar Ubuntu**.  
- Tipo: Instalación normal (incluye apps y navegador).  
- Usuario: crea usuario y contraseña.  
- Disco: selecciona virtual.  

⚠️ Esta versión incluye **GNOME** (interfaz gráfica).  

### 6. ✅ Finalización
- Inicia sesión.  
- Prueba la terminal:  
```bash
ls
```

### 7. 📌 Consejos
- Crear snapshot tras instalación.  
- Practicar tanto con entorno gráfico como con terminal.  

---

# Práctica 3: Configuración de red entre Server y Desktop

## 🎯 Objetivo
Configurar las interfaces de red en dos máquinas virtuales (**Ubuntu Server** y **Ubuntu Desktop**) para que puedan comunicarse entre ellas y, además, que el servidor tenga salida a Internet.

## 1. 🏗️ Arquitectura de red
- **Ubuntu Server**:  
  - enp0s3 → Adaptador puente (Internet).  
  - enp0s8 → Red interna (con cliente).  
- **Ubuntu Desktop**:  
  - enp0s3 → Red interna (con servidor).  

👉 Es como conectar físicamente los cables.  

## 2. ⚙️ Configuración en VirtualBox
- Servidor: Adaptador 1 = Puente, Adaptador 2 = Red interna.  
- Cliente: Adaptador 1 = Red interna.  

## 3. 📝 Configuración con Netplan

### Ubuntu Server
1. Ver interfaces:  
```bash
ip a
```  
2. Editar configuración:  
```bash
sudo nano /etc/netplan/01-netcfg.yaml
```
3. Ejemplo de archivo:  
```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      dhcp4: no
      addresses:
        - 192.168.100.1/24
```
4. Aplicar:  
```bash
sudo netplan try
sudo netplan apply
```

### Ubuntu Desktop
```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 192.168.100.2/24
```

## 4. 🔎 Comprobaciones
- Desde Server → ping 192.168.100.2  
- Desde Desktop → ping 192.168.100.1  
- Server a Internet → ping 8.8.8.8  

✅ Si responden, la red está lista.  

## 5. 📷 Diagrama de Red

![Diagrama de Red](Diagrama_Red_VM.png)
