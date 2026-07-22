# Guía de Estudio: De 2do Semestre a Becario en Programación

**Meta final:** Hacking ético + Bases de datos
**Ritmo semanal:** 6-10 hrs/semana (fuera de clases)

---

## Cómo usar esta guía

Cada fase tiene un **checkpoint de dominio** al final de cada track. No avances a la siguiente fase hasta poder marcar todos los puntos con honestidad. Es mejor ir lento y sólido que rápido y con huecos — en hacking ético especialmente, los huecos de fundamentos se pagan caro después.

Esta guía se actualiza conforme avanzas. El detalle técnico de cada tema (comandos, ejemplos, explicaciones) vive en documentos de apuntes separados — aquí solo se lleva el rastreo de progreso y la ruta general.

---

## Orden de prioridad general (C++ → Python → Ciberseguridad)

1. **Consolidar C++** (ya tienes la base) — repaso rápido de POO, no invertir de más aquí
2. **Python** — siguiente prioridad real. Es el lenguaje que más se usa en hacking ético (scripts, automatización, retos de TryHackMe/HackTheBox). Como ya sabes POO en C++, se aprende por traducción de conceptos, no desde cero
3. **Ciberseguridad/hacking ético formal** — después de tener Python básico, porque la mayoría de recursos y retos asumen que ya programas un poco para automatizar tareas y entender scripts de explotación

## Recursos externos y dónde encajan

| Recurso | Track / Fase donde se usa | Prioridad |
|---------|---------------------------|-----------|
| `30-Days-Of-Python` (GitHub) | Track C — material de apoyo estructurado para Python | Alta, en paralelo con SQL ahora |
| Cisco NetAcad "Networking Basics" | Track A — complemento opcional para profundizar redes | Media, en paralelo ahora |
| `90DaysOfCyberSecurity` (GitHub) | Fase 2-3 — ruta estructurada principal de hacking ético, junto con TryHackMe | Alta, pero después de Python básico |
| roadmap.sh | Referencia general para ver qué falta, no un curso a seguir línea por línea | Consulta ocasional |
| `ML-For-Beginners` (GitHub) | Fuera del roadmap actual — Machine Learning es una rama distinta a la meta actual | Baja — guardar para después de consolidar hacking ético + bases de datos |

---

## Reparto semanal de horas (6-10 hrs/sem)

| Track | Horas/sem | Enfoque |
|-------|-----------|---------|
| A — Linux y Terminal | 2 hrs | Base para todo, especialmente hacking ético |
| B — Bases de Datos | 2 hrs | SQL, diseño, PostgreSQL |
| C — POO / Lenguajes | 2 hrs | Consolidar C++ → transición a Python |
| D — Inglés técnico | 1-2 hrs | Vocabulario y lectura técnica en seguridad/programación |

> Nota: no todos los tracks corren siempre a la misma intensidad. Cuando un track esté en una etapa más ligera (ej. repaso), esas horas se pueden mover a otro que lo necesite más esa semana.

---

## FASE 0 — Preparar el entorno ✅ COMPLETADA

- [x] Instalar VirtualBox + Kali Linux
- [x] Instalar VS Code
- [x] Instalar PostgreSQL
- [x] Crear cuenta en GitHub
- [x] Crear cuenta en TryHackMe (gratuita)

**Checkpoint:**
- [x] Puedo encender/apagar mi VM sin problemas
- [x] Sé crear, mover, copiar y borrar archivos y carpetas por terminal
- [x] Entiendo la diferencia entre usuario normal y `sudo`
- [x] Tengo un repositorio vacío creado en GitHub

---

## FASE 0.5 — Ubuntu como sistema principal ✅ COMPLETADA

> Se borró Windows por completo y se instaló **Ubuntu** como sistema principal en la laptop. Kali se mantiene en VirtualBox dentro de Ubuntu.

- [x] Respaldo completo antes de instalar
- [x] Ubuntu instalado y funcionando
- [x] VirtualBox + Kali recuperados dentro de Ubuntu
- [x] Git, VS Code, PostgreSQL reinstalados en Ubuntu

---

## FASE 1 — Fundamentos en paralelo

### Track A — Linux y Terminal

**Navegación y archivos** ✅
- [x] `pwd`, `ls`, `cd`, `mkdir`, `touch`, `cp`, `mv`, `rm`, `cat`, `nano`
- [x] Rutas absolutas vs relativas
- [x] Qué es un shell, terminología comando/bandera/argumento, `man`/`--help`
- [x] Comodines `*`, `?`

**Permisos y procesos** ✅
- [x] `chmod` y notación numérica (700, 644, 755, 777, 100)
- [x] `ps aux`, `top`, `kill`, procesos en segundo plano (`&`)
- [x] `grep` como filtro de texto
- [x] `chown`
- [x] Redirección `>`, `>>`
- [ ] `Ctrl+Z`, `jobs` (menor, se verá en scripting)

**Redes básicas** ✅ COMPLETADO
- [x] TCP vs UDP, qué es "listening"
- [x] `ping`, `ip a`, `traceroute`, `netstat`
- [x] Puerto, protocolo, IP pública vs privada, notación CIDR (`/24`)
- [ ] Modelo TCP/IP y OSI completo, subnetting a profundidad (pendiente, se verá con Nmap)
- [ ] Opcional: Cisco NetAcad "Networking Basics" (curso gratuito) para profundizar más allá de lo visto aquí

**Extra cubierto (fuera del plan original, valioso para el día a día)**
- [x] Atajos de teclado en Bash
- [x] Flujo completo de Git (add/commit/push, tokens, manejo de repos)
- [x] Escritorio remoto (GNOME Remote Desktop)

**Scripting en Bash** — pendiente
- [ ] Variables, condicionales, loops
- [ ] Un script propio de 10+ líneas

**Checkpoint Track A:**
- [x] Me muevo por el sistema de archivos sin usar el mouse
- [x] Entiendo permisos `rwxr-xr-x`
- [x] Sé usar `grep` para filtrar texto
- [x] Explico qué es un puerto y una IP, diferencia TCP/UDP
- [x] Sé subir cambios a mi repositorio con Git de forma autónoma
- [ ] Escribí un script Bash funcional

> Detalle completo con ejemplos: `apuntes_linux_terminal.md`

---

### Track B — Bases de Datos

**SQL básico**
- [ ] `SELECT`, `WHERE`, `ORDER BY`, `LIMIT` (SQLZoo / HackerRank)

**Relaciones y JOINs**
- [ ] `INNER JOIN`, `LEFT JOIN`, claves primarias/foráneas
- [ ] `GROUP BY`, `HAVING`, `COUNT`, `SUM`, `AVG`

**Diseño de bases de datos**
- [ ] Normalización (1FN, 2FN, 3FN)
- [ ] Diagramas Entidad-Relación
- [ ] Diseño propio (ej. sistema de biblioteca)

**Implementación**
- [ ] Base de datos propia en PostgreSQL
- [ ] Consultas complejas, subconsultas, vistas

**Checkpoint Track B:**
- [ ] Escribo un JOIN sin ver ejemplos
- [ ] Explico por qué normalizar una tabla
- [ ] Tengo una base de datos propia funcionando
- [ ] Entiendo qué es y por qué existe una clave foránea

---

### Track C — POO y lenguajes

**Consolidar C++ (ya en curso)**
- [ ] Repaso de herencia, polimorfismo, encapsulamiento, abstracción
- [ ] Proyecto pequeño con 3+ clases relacionadas (ej. Usuario, Producto, Pedido)

**Transición a Python**

Por qué Python: es el lenguaje más usado en hacking ético — scripts de automatización, herramientas como Scapy, e integraciones con Nmap. La mayoría de retos en TryHackMe/HackTheBox asumen conocimiento básico de Python. Los conceptos de POO no cambian entre lenguajes, solo la sintaxis — esta transición debería ser rápida gracias a tu base en C++.

**Recurso guía:** [30-Days-Of-Python](https://github.com/Asabeneh/30-Days-Of-Python) — úsalo como material estructurado principal en vez de ejercicios sueltos, es gratuito y ya probado.

- [ ] Sintaxis básica: variables, funciones, estructuras de control
- [ ] Clases y objetos en Python — comparar directamente con cómo se hace en C++
- [ ] Reescribe en Python un proyecto pequeño que ya hiciste en C++ (acelera el aprendizaje por comparación directa)
- [ ] Módulos comunes: `os`, `sys`, `requests`

**Checkpoint Track C:**
- [ ] Explico la diferencia entre herencia y polimorfismo con ejemplo propio
- [ ] Mi proyecto en C++ usa 3+ clases relacionadas correctamente
- [ ] Puedo escribir una clase básica en Python sin ver ejemplos
- [ ] Entiendo las diferencias clave entre C++ (estático) y Python (dinámico)

---

### Track D — Inglés técnico

Punto de partida: ya mantienes conversación fluida en inglés general. Este track es específico para vocabulario técnico de programación y seguridad, y lectura de documentación real — no es aprender inglés desde cero.

**Vocabulario técnico**
- [ ] Términos de programación: *variable, loop, function, exception, debugging, deployment*
- [ ] Términos de seguridad/hacking ético: *buffer overflow, privilege escalation, payload, exploit, vulnerability, endpoint, patch*
- [ ] Glosario propio: cada término nuevo que encuentres en TryHackMe/documentación, agrégalo con su definición en tus propias palabras

**Lectura técnica directa (sin traducir)**
- [ ] Leer documentación oficial de una herramienta (ej. Nmap docs) directamente en inglés
- [ ] Leer un write-up de TryHackMe en inglés y entenderlo sin traductor
- [ ] Familiarizarte con la estructura de un CVE (Common Vulnerabilities and Exposures)

**Escritura técnica**
- [ ] Practicar README de GitHub en inglés (estructura: qué hace, cómo instalarlo, qué aprendiste)
- [ ] Redactar un mini "write-up" en inglés de algún reto que resuelvas

**Checkpoint Track D:**
- [ ] Reconozco 15+ términos técnicos de seguridad sin necesitar traducción
- [ ] Leí un artículo o write-up técnico completo en inglés sin traductor
- [ ] Escribí un README en inglés para uno de mis proyectos

> Para el CV: con este nivel puedes ponerlo como "Inglés — avanzado (B2/C1), lectura técnica y conversación fluida". Si no tienes certificación formal (TOEFL/IELTS), es honesto ponerlo autoevaluado — muchos reclutadores lo validan en la entrevista misma.

---

## FASE 2 — Integración: donde todo se conecta

- [ ] Proyecto integrador: app con backend (Python o C++) que use POO + se conecte a tu base de datos (CRUD completo)
- [ ] Aprende OWASP Top 10 (conceptual): SQL Injection, XSS, por qué ocurren
- [ ] Haz tu proyecto vulnerable a SQL Injection a propósito, luego corrígelo
- [ ] Empieza TryHackMe: rutas "Pre Security" y "Cyber Security 101"

**Checkpoint:**
- [ ] Proyecto CRUD completo, subido a GitHub
- [ ] Explico y demuestro SQL Injection en mi propio proyecto
- [ ] Completé "Pre Security" de TryHackMe
- [ ] Sé usar Git básico: `commit`, `push`, `pull`, `branch`

---

## FASE 3 — Hacking ético formal

**Recurso guía principal:** [90DaysOfCyberSecurity](https://github.com/farhanashrafdev/90DaysOfCyberSecurity) — úsalo como ruta estructurada, complementando TryHackMe. Requiere ya tener Python básico (Track C) para aprovecharlo bien.

- [ ] TryHackMe: ruta "Jr Penetration Tester" o módulos gratuitos equivalentes
- [ ] Herramientas core: **Nmap** (escaneo), **Wireshark** (tráfico), **Burp Suite Community** (web)
- [ ] Practica en HackTheBox, nivel "Starting Point"
- [ ] Documenta cada máquina/reto resuelto (evidencia para la beca)

**Checkpoint:**
- [ ] Escaneo una red y explico cada bandera de Nmap que uso
- [ ] Resolví 5+ máquinas de nivel principiante
- [ ] Tengo un write-up publicado en GitHub o blog

---

## Portafolio y búsqueda de beca (transversal desde Fase 2)

- [ ] GitHub con 2-3 proyectos completos, cada uno con README claro
- [ ] LinkedIn actualizado con proyectos y progreso
- [ ] Al menos 1 certificado gratuito o de bajo costo (Google Cybersecurity Certificate, completions de TryHackMe)
- [ ] Perfil visible en TryHackMe/HackTheBox con progreso real

---

## Futuro (después de consolidar la meta actual)

- **Machine Learning** ([ML-For-Beginners](https://github.com/microsoft/ML-For-Beginners)) — rama interesante pero distinta a hacking ético + bases de datos. Guardar para cuando la meta actual esté sólida, no competir por tiempo ahora.

---

## Reglas de oro

1. **No saltes fases.** Fundamentos débiles se notan rápido en una entrevista técnica.
2. **Documenta todo.** Escribir lo que aprendes te obliga a entenderlo de verdad.
3. **Entender > memorizar.** Si lo puedes explicar con tus palabras, lo sabes.
4. **Revisa el checkpoint antes de avanzar.** Si fallas 2+ puntos, refuerza antes de seguir.

---

*Última actualización: seguimiento de progreso en Fase 1, Track A avanzado, Tracks B/C/D en punto de partida.*
