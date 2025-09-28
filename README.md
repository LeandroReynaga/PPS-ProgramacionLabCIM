# PPS - Programación Laboratorio CIM
## Universidad Nacional de Lomas de Zamora
📍 **Laboratorio CIM, Facultad de Ingeniería, Universidad Nacional de Lomas de Zamora**  
👨‍🎓 **Alumno:** Leandro Joel Reynaga Ríos  
📅 **Año:** 2025  

---

## Índice
1. [Introducción](#1-introducción)  
2. [Objetivo](#2-objetivo)  
3. [Descripción del Proyecto](#3-descripción-del-proyecto)   
4. [Paso a paso del proceso de encendido y ejecución](#4-Paso-a-paso-del-proceso-de-encendido-y-ejecución)  
   4.1 [Preparación](#41-Preparación)  
   4.2 [Energización de equipos](#42-energización-de-equipos)  
   4.3 [Puesta a punto del brazo](#43-puesta-a-punto-del-brazo)  
   4.4 [Ejecución de programa de “Escucha”](#44-ejecución-de-programa-de-“Escucha”)  
   4.5 [Prueba manual (opcional)](#45-prueba-manual-(opcional))  
5. [Resultado](#5-resultado)  

---

## 1. Introducción
El presente informe describe la Práctica Profesional Supervisada (PPS) realizada en el Laboratorio CIM de la Facultad de Ingeniería de la Universidad Nacional de Lomas de Zamora (UNLZ), en el marco de la carrera de Ingeniería Mecatrónica.  

El Laboratorio CIM constituye un entorno de aprendizaje en el cual distintas cátedras desarrollan actividades prácticas orientadas a la integración de conocimientos en automatización, robótica y sistemas de manufactura. En esta ocasión, el proyecto general tiene como objetivo poner en valor la cinta transportadora y los brazos robóticos, con el fin de aprovechar su uso para fines carácter académico.  

---

## 2. Objetivo  
El objetivo del proyecto fue desarrollar la programación del brazo robótico Scora-ER 14 de la marca ESHED ROBOTEC para integrarlo con la cinta transportadora, de manera que quede en estado de escucha, reciba una señal de inicio, ejecute la rutina correspondiente y, al finalizar, envíe una señal de que término para que la cinta continue con su propia rutina.  

---

## 3. Descripción del Proyecto
### 3.1 Investigación inicial
La primera etapa del proyecto comenzó con la investigación del lenguaje de programación del brazo robótico y de las especificaciones técnicas de su controladora. Para ello fue necesario revisar los manuales originales del equipo, los cuales existían solamente en formato física. Por este motivo, se procedió a su escaneo completo, lo que permitió contar con una versión digital para consultar en cualquier momento y, a su vez, iniciar la generación de documentación para tener un repositorio digital, con el fin de facilitar el trabajo de futuros proyectos en el laboratorio.  

<p align="center">
  <img width="764" height="347" alt="image" src="https://github.com/user-attachments/assets/f19d2b55-d7c7-47e7-9261-8beb4e2c291f" />
  <br>
  <em>Figura 1: Manuales de operación </em>
</p>

### 3.2 Desarrollo
Se llevó a cabo la programación de una rutina, con el objetivo de integrarlo funcionalmente con la cinta transportadora. La lógica implementada permite que el robot al detectar la señal de 24 VDC en la entrada digital 1, el sistema inicia automáticamente la ejecución del programa denominado “LRREM”. Este programa funciona como nexo lógico con el programa “LFECH”, el cual a su vez, llama al programa “JOB1”, donde se encuentra la secuencia de movimientos correspondiente al cambio de platos.  

El programa “LRREM” cumple la función de una especie de bandera booleana On/Off, ya que permite mantener la rutina en ejecución en loop continuo mientras la condición de disparo se mantenga activa. De esta manera, se logra que la secuencia se repita de forma controlada, sincronizada con la señal proveniente de la cinta transportadora.  

#### LIST LRREM
```plaintext
1  PRINTLN "Inicio LRREM"
2  WAIT IN[1]=1
3  RUN LFCOM
4  END
```

#### LIST LFCOM
```plaintext
1  PRINTLN "Comienza LFCOM. Pausa de 500ms"
2  WAIT IN[1]=0
3  DELAY 50
4  RUN LFECH
5  RUN LRREM
6  END
```

#### LIST LFECH
```plaintext
1  PRINTLN "Se ejecuto LFECH. Pausa de 500ms"
2  DELAY 50
3  PRINTLN "EJECUTANDO PROGRAMA"
4  RUN JOB1
5  DELAY 200
6  END
```

#### Vector JOB1[n]

| Descripción | Imagen |
|-------------|--------|
| JOB1[10] – Va arriba de estación 1 new <br> JOB1[11] – Va abajo de estación 1 new <br> JOB1[12] – Va arriba de estación 2 new <br> JOB1[13] – Va abajo de estación 2 new <br> JOB1[14] – Va arriba de estación 3 new <br> JOB1[15] – Va abajo de estación 3 new <br> JOB1[16] – Sale de la zona de trabajo | <p align="center"><img width="443" height="317" alt="image" src="https://github.com/user-attachments/assets/baeb19a8-aa49-40ae-a3a8-0e23f21f9d0e" /></p> |

#### LIST JOB1
```plaintext
1  SET OUT[5]=1
2  OPEN #Abre el gripper (por seguridad)
3  SPEED 30
4  MOVED JOB1[10]  #Va arriba de la estación 1
5  SPEED 10
6  MOVED JOB1[11] #Va abajo de la estación 1
7  CLOSE #Agarra el plato
8  MOVED JOB1[10] #Va arriba de la estación 1
9  SPEED 30
10  MOVED JOB1[12] #Va arriba de la estación 2 
11  SPEED 10
12  MOVED JOB1[13] #Va abajo de la estación 2
13  OPEN #Suelta el plato
14  MOVED JOB1[12] #Va arriba de la estación 2 
15  SPEED 30
16  MOVED JOB1[14] #Va arriba de la estación 3
17  SPEED 10
18  MOVED JOB1[15] #Va abajo de la estación 3
19  CLOSE #Agarra el plato
20  MOVED JOB1[14] #Va arriba de la estación 3
21  SPEED 30
22  MOVED JOB1[10] #Va arriba de la estación 1
23  SPEED 10
24  MOVED JOB1[11] #Va abajo de la estación 1
25  OPEN #Suelta el plato
26  MOVED JOB1[10] #Va arriba de la estación 1
27  SPEED 30
28  MOVED JOB1[12] #Va arriba de la estación 2 
29  SPEED 10
30  MOVED JOB1[13] #Va abajo de la estación 2
31  CLOSE #Agarra el plato
32  MOVED JOB1[12] #Va arriba de la estación 2 
33  SPEED 30
34  MOVED JOB1[14] ] #Va arriba de la estación 3
35  SPEED 10
36  MOVED JOB1[15] #Va abajo de la estación 3
37  OPEN #Suelta el plato
38  MOVED JOB1[14] #Va arriba de la estación 3
39  SPEED 30
40  MOVED JOB1[16] # Sale de la zona de trabajo
41  SET OUT[5]=0
42  END
``` 
- LRREM: Mantiene al robot en espera hasta recibir la señal de activación (IN[1]=1) para disparar nuevamente la lógica de LFCOM.
- LFCOM: Se encarga de verificar la condición de la entrada digital; espera que la señal se libere (IN[1]=0) y luego reinicia la ejecución de LFECH y LRREM.
- LFECH: Ejecuta la rutina principal, con mensajes de estado y el llamado a JOB1, que contiene la secuencia de movimientos del robot.
Con esta estructura, se logró establecer un ciclo continuo de “escucha - ejecución - retorno a espera”, asegurando que el brazo robótico y la cinta transportadora trabajen de forma coordinada.

---

## 4. Paso a paso del proceso de encendido y ejecución  
### 4.1 Preparación 
a) Subir las térmicas del laboratorio.  
<p align="center">
  <img width="591" height="443" alt="image" src="https://github.com/user-attachments/assets/7b2c428d-1505-4868-87c8-86d30a08c6c7" />
  <br>
</p>

b) Conectar el aire comprimido al brazo. Al finalizar, cerrar el suministro.    
<p align="center">
  <img width="591" height="443" alt="image" src="https://github.com/user-attachments/assets/f21a86a8-260c-4c6a-bd9c-2d772e76ac30" />
  <br>
</p>


### 4.2 Energización de equipos
a) Energizar la zapatilla (Se enciende la controladora).  
b)	Encender la computadora (No inicia automáticamente hasta el escritorio)
-  Le aparecerá una pantalla en negro, presionar “F1”.
-  Entra a la BIOS, presionar “ESC” y seleccionar “Save and Exit”.
 
c)	Encender la fuente de 24 VDC (Power Supply).  


### 4.3 Puesta a punto del brazo
a)	Una vez iniciada la computadora y estando en el escritorio.  
-  Encender los motores del brazo, para ello apreta el botón "MOTORS”.
<p align="center">
  <img width="591" height="443" alt="image" src="https://github.com/user-attachments/assets/a117dc99-1e27-491d-93e6-900cd9a3b437" />
  <br>
</p>

b)	Ahora en el escritorio podes abrir el terminal de programación "ATS"  
c)	En la consola de ATS, ejecutar los comandos iniciales:  
-  “CON” → Se encienden los motores ("COFF" si se desean apagar por seguridad, pero no es el caso)    
-  “HOME” → Ejecutar referencia obligatoria; sin HOME no se ejecutan comandos RUN.    


### 4.4 Ejecución de programa de “Escucha”  
a)	Ejecutar “RUN LRREM” para que se compile el programa de escucha del brazo robótico.  
b)	El sistema queda esperando una señal de 24 VDC en la entrada digital IN 1.    
-  Al recibirla, se encadena LRREM → LFECH → JOB1, ejecutando la rutina de trabajo.
-  Al finalizar, el programa envía la señal de finalización con la salida digital OUT 5 para que la cinta continúe su rutina.


### 4.5 Prueba manual (opcional)  
a)	Si se desea hacer una prueba rápida desde el controlador, puentear en el bloque "IMPUTS" los bornes COM- y IN1 con un solo mini pulso.  
b)	Confirmar en consola los mensajes de estado y que la rutina JOB1 se ejecuta.
<p align="center">
  <img width="575" height="210" alt="image" src="https://github.com/user-attachments/assets/b90c97ca-f74c-4e42-925d-545c6ac5264b" />
  <br>
</p>

---
## 5. Resultado  
<p align="center">
  <a href="https://youtu.be/xYAC620RMZM" target="_blank">
    <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/90369ad1-bb9f-4899-98c9-23467ea2c50d" />
  </a>
</p>
<p align="center">
  <em>Haz clic en la imagen para ver el video de la demostración.</em>
</p>

---

## 🤝 Contacto

- **Reynaga Rios Leandro Joel**  
  - 📬Email: leandro_05_01@hotmail.com  
  - GitHub: [@LeandroReynaga](https://github.com/LeandroReynaga)

---

