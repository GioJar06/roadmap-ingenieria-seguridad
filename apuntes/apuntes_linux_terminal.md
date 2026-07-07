# Apuntes: Linux y Terminal (Fase 1 - Track A)

---

## Índice

0. Conceptos base: shell, comandos, banderas y argumentos
1. Navegación básica
2. Archivos y carpetas
3. Permisos (`chmod`)
4. `sudo` y usuarios
5. Procesos
6. `grep` — filtro de texto
7. Redes básicas
8. Glosario de siglas
9. Atajos de teclado en Bash
10. Git — flujo de trabajo y conceptos clave
11. Escritorio remoto
12. Pendientes / próximos temas

---

## 0. Conceptos base: shell, comandos, banderas y argumentos

### ¿Qué es un shell?

El **kernel** de Linux es el núcleo que controla el hardware (memoria, procesador, disco), pero no le hablas directamente — es demasiado bajo nivel. El **shell** es el programa intermediario que:

1. Recibe lo que escribes (ej. `ls -la`)
2. Lo interpreta
3. Se lo pide al kernel en la forma que el kernel entiende
4. Te devuelve el resultado en pantalla

**Analogía:** el kernel es una fábrica con maquinaria compleja que tú no sabes operar. El shell es el operador/traductor: le dices en tu idioma qué quieres, y él sabe qué palancas mover.

**Tipos de shell:**
- **Bash** (Bourne Again SHell) — el más común, el que usamos por defecto en Ubuntu/Kali
- **Zsh** — similar a Bash, con más funciones extra
- **Fish** — enfocado en ser más amigable visualmente
- **sh** — versión básica/antigua, base de la que nacieron los demás

### Terminología: comando, bandera, argumento

```bash
rm -rf carpeta/
│  │   │
│  │   └── argumento (sobre qué actúa el comando)
│  └────── bandera(s) / opción(es) (cómo se comporta)
└───────── comando (qué programa ejecutas)
```

- **Comando**: el programa en sí (`ls`, `rm`, `chmod`, `ps`)
- **Bandera / opción / "flag"**: modifica el comportamiento del comando (`-l`, `-r`, `-f`)
- **Argumento**: el objetivo sobre el que actúa (nombre de archivo, carpeta, etc.)

**Banderas cortas vs largas:** se pueden juntar (`-rf` = `-r -f`). Las largas usan doble guion y palabra completa (`--force`, `--no-preserve-root`).

**El orden de las banderas no importa** cuando van juntas tras un solo guion: `-tulnp` es exactamente igual a `-nplut` o `-lnpu`.

**Guion como parte de un nombre vs. guion de bandera:** en `net-tools` o `p7zip-full`, el guion es parte del **nombre propio del paquete**, no una bandera — se distingue porque una bandera siempre es un argumento separado que empieza con guion (`-y`, `--force`), mientras que aquí el guion está en medio del nombre.

### Banderas comunes vistas hasta ahora

| Bandera | Comando | Significado |
|---------|---------|-------------|
| `-y` | `apt install` | "yes" — responde automáticamente sí a cualquier confirmación |
| `-c 4` | `ping` | "count" — cantidad de paquetes a enviar antes de detenerse solo |

### Cómo consultar banderas que no conoces

Imposible memorizar todas las banderas de todos los comandos — se consultan cuando se necesitan:

```bash
man rm        # abre el manual completo del comando (salir con "q")
rm --help     # resumen rápido de banderas disponibles
```

### Cómo aprender a "leer" comandos y resultados sin memorizar todo

Esta es la habilidad real que se desarrolla, no la memorización. El proceso:

1. Ves algo que no reconoces (ej. un nombre de interfaz de red raro)
2. Buscas el **patrón**, no el dato exacto — la mayoría de nombres en Linux siguen convenciones predecibles
3. Confirmas con un comando o búsqueda rápida en vez de asumir

Ejemplo real: nadie memoriza que "`enp0s31f6` = ethernet" como dato aislado. Se reconoce el patrón (`en` = Ethernet, `wl` = WiFi, `lo` = loopback) y se confirma con una herramienta si hace falta (ver sección de Redes).

---

## 1. Navegación básica

```bash
pwd          # "print working directory" - muestra en qué carpeta estás ahora
ls           # lista archivos y carpetas del directorio actual
ls -l        # formato "long" (largo): permisos, dueño, tamaño, fecha
ls -a        # "all": incluye archivos ocultos (empiezan con punto, ej. .bashrc)
ls -la       # combina ambos: detalle completo + ocultos
cd Desktop   # entra a la carpeta Desktop
cd ..        # sube un nivel (carpeta anterior/padre)
cd ~         # va directo a tu carpeta de usuario (home)
cd /         # va a la raíz del sistema de archivos
```

**Rutas absolutas vs relativas:**
- Absoluta: empieza desde la raíz `/`, ej. `/home/giojar06/practica_terminal`
- Relativa: parte de donde ya estás, ej. `cd practica_terminal`

---

## 2. Archivos y carpetas

```bash
mkdir practica            # crea una carpeta
touch archivo.txt          # crea un archivo vacío
touch a.txt b.txt          # crea varios archivos de una vez
cp archivo.txt copia.txt   # copia un archivo (deja el original)
mv archivo.txt nueva/      # mueve un archivo a otra carpeta
mv viejo.txt nuevo.txt     # renombra (mv también sirve para esto)
rm archivo.txt             # borra un archivo (¡no hay papelera de reciclaje!)
cat archivo.txt            # muestra el contenido completo en pantalla
nano archivo.txt           # abre un editor de texto simple en terminal
```

**Guardar y salir de `nano`:** `Ctrl+O` (guardar) → Enter → `Ctrl+X` (salir)

**Errores de tipeo:** la terminal es 100% literal. Un carácter mal escrito da error de "no existe" aunque el archivo esté ahí con el nombre correcto. Es normal, se agarra práctica.

### `rm -r` y otras banderas de `rm`

```bash
rm carpeta/        # ERROR: "is a directory"
rm -r carpeta/     # SÍ funciona: borra la carpeta y TODO su contenido (recursivo)
```

| Bandera | Significado |
|---------|-------------|
| `-r` | recursivo — entra a subcarpetas y borra todo dentro |
| `-f` | "force" — ignora advertencias/confirmaciones y errores como "no existe" |
| `-i` | "interactive" — pregunta confirmación antes de borrar cada archivo |
| `-v` | "verbose" — muestra qué está borrando paso a paso |
| `-d` | borra carpetas vacías (alternativa limitada a `-r`) |

**El comando más temido de Linux — `sudo rm -rf /`:** borra el sistema operativo completo sin posibilidad de recuperación. Ubuntu pide confirmación extra (`--no-preserve-root`) precisamente por lo fácil que es correrlo por accidente (ej. con una variable vacía en un script: `rm -rf $VAR/`).

### Comodines: `*` y `?`

Sirven para referirte a varios archivos a la vez sin escribirlos uno por uno.

```bash
ls *.txt          # TODOS los archivos que terminen en .txt
ls archivo*       # todo lo que EMPIECE con "archivo"
rm *.log          # borra todos los archivos .log
cp *.md carpeta/  # copia todos los archivos .md a otra carpeta
```

- `*` = cualquier cantidad de caracteres, incluyendo cero
- `?` = exactamente un carácter, cualquiera que sea (ej. `archivo?.txt` coincide con `archivo1.txt`, `archivoA.txt`, pero no con `archivo10.txt`)

### `chown` — cambiar el DUEÑO de un archivo

Diferencia clave con `chmod`: `chmod` cambia **permisos** (qué se puede hacer), `chown` cambia **quién es el dueño** del archivo.

```bash
sudo chown otro_usuario archivo.txt              # cambia solo el dueño
sudo chown otro_usuario:otro_grupo archivo.txt   # cambia dueño Y grupo a la vez
sudo chown -R usuario carpeta/                   # aplica recursivamente a toda una carpeta
```

Casi siempre requiere `sudo`, porque reasignar de quién es un archivo es una acción sensible de seguridad. Caso de uso típico: un archivo descargado con `sudo` queda con dueño `root` en vez de tu usuario, y no puedes editarlo hasta reclamarlo con `chown`.

### Redirección: `>` y `>>`

Mandan la salida de un comando a un archivo, en vez de mostrarla en pantalla.

```bash
echo "Hola mundo" > saludo.txt    # > SOBRESCRIBE el archivo completo
echo "Otra línea" >> saludo.txt   # >> AGREGA al final, sin borrar lo anterior
```

Ejemplo práctico:
```bash
ls -la > lista_archivos.txt        # guarda el resultado de ls -la en un archivo
ps aux >> registro_procesos.txt    # agrega el resultado de ps aux a un archivo existente
```

**Diferencia con el pipe `|`:**
- `|` manda la salida de un comando **a otro comando** (ej. `ps aux | grep sleep`)
- `>` / `>>` manda la salida de un comando **a un archivo**

Se pueden combinar: `ps aux | grep bash >> resumen.txt` — primero filtra con `grep`, luego agrega ese resultado ya filtrado al archivo.

---

## 3. Permisos (`chmod`)

### Los 3 tipos de permiso

| Letra | Significado | Valor numérico |
|-------|-------------|-----------------|
| `r` | read (leer) | 4 |
| `w` | write (escribir/modificar contenido) | 2 |
| `x` | execute (ejecutar como programa/script) | 1 |

`w` y `x` NO son lo mismo: escribir modifica contenido, ejecutar corre el archivo como programa.

### Los 3 grupos de "quién"

```
-rw-r--r--
 [dueño][grupo][otros]
```

Ejemplo: `-rw-r--r--` = **644** → dueño `rw-`=6, grupo `r--`=4, otros `r--`=4

```bash
chmod 700 archivo.txt   # solo dueño: todo
chmod 644 archivo.txt   # dueño lee/escribe, resto solo lee
chmod 755 archivo.txt   # dueño todo, resto lee/ejecuta
chmod 777 archivo.txt   # todos pueden todo (¡evitar!)
chmod 100 archivo.sh    # solo dueño puede ejecutar (poco común — normalmente también necesitas leer para ejecutar)
```

### Por qué importa en seguridad

"Otros" es cualquier cuenta del **mismo sistema** — no viaja con el archivo a un USB o internet. Permisos mal configurados en "otros" habilitan robo de datos o **escalación de privilegios**:
```bash
find / -perm -o+w -type f 2>/dev/null   # busca archivos peligrosamente abiertos
```

---

## 4. `sudo` y usuarios

```bash
whoami         # tu usuario actual
sudo whoami    # ejecuta como root, muestra "root"
passwd         # cambia tu contraseña de usuario (la misma que usa sudo)
```

**`sudo`** = "superuser do". Ejecuta **un solo comando** como administrador, sin cambiar tu sesión permanentemente.

**Sobre la contraseña de `sudo`:** es la misma contraseña de tu usuario del sistema, no una distinta. No se muestra en pantalla al escribirla (ni con asteriscos) — es normal, medida de seguridad. `sudo` recuerda tu autorización unos ~15 minutos antes de volver a pedirla.

**Por qué root importa en seguridad:** root puede leer/modificar/borrar cualquier archivo, matar cualquier proceso. La mayoría de ataques buscan escalar de usuario normal a root sin autorización.

---

## 5. Procesos

```bash
ps aux              # lista TODOS los procesos corriendo
ps aux | head -10   # solo los primeros 10
ps aux | grep sleep # filtra procesos que contengan "sleep"
top                 # procesos en tiempo real
kill [PID]          # termina un proceso por su PID
sleep 300 &         # ejemplo: proceso en segundo plano (&)
```

**Desglose de `ps aux`:** `ps` = process status (foto de procesos actuales) | `a` = todos los usuarios | `u` = formato con %CPU/%MEM | `x` = incluye procesos sin terminal asociada.

| Columna | Significado |
|---------|-------------|
| USER | quién ejecuta el proceso |
| PID | Process ID — identificador único |
| %CPU / %MEM | consumo de recursos |
| COMMAND | programa/comando |

**PID 1** siempre es el primer proceso del sistema (`systemd`) — todos los demás nacen de él.

---

## 6. `grep` — filtro de texto

Busca un patrón y devuelve **solo las líneas que coinciden**.

```bash
ps aux | grep firefox
cat archivo.txt | grep error
grep "root" /etc/passwd
```

| Bandera | Significado |
|---------|-------------|
| `-i` | ignora mayúsculas/minúsculas |
| `-v` | invierte: muestra lo que NO coincide |
| `-c` | cuenta coincidencias en vez de mostrarlas |
| `-r` | busca recursivamente en carpetas |

**Por qué importa en hacking ético:** análisis de escaneos de red, búsqueda de contraseñas/patrones en configuraciones, filtrado de logs.

---

## 7. Redes básicas

### Conceptos centrales

- **IP** — dirección de una computadora en una red (como dirección postal)
- **Puerto** — canal específico dentro de esa computadora para un servicio (como número de departamento en un edificio). Ej: puerto 80 = tráfico web, puerto 22 = SSH
- **Protocolo** — reglas del "idioma" de comunicación (HTTP, HTTPS, SSH, TCP, UDP...)

### TCP vs UDP (protocolos de transporte)

**TCP** — como una llamada telefónica: establece conexión confirmada, garantiza que todo llegue completo y en orden, reenvía lo perdido. Más lento pero confiable. Usado en: páginas web, emails, transferencia de archivos.

**UDP** — como gritar un mensaje sin esperar confirmación: más rápido, pero si se pierde un paquete, no se reenvía. Usado donde la velocidad importa más que la perfección: videollamadas, streaming, juegos en línea.

### Comandos de diagnóstico de red

```bash
ip a
```
Muestra tus interfaces de red y direcciones IP asignadas.

**Convención de nombres de interfaces en Linux** (no se memoriza el nombre completo, se reconoce el patrón):
- `lo` = loopback (tu máquina hablándose a sí misma, IP fija `127.0.0.1`)
- `en...` = Ethernet (cable) — el resto del nombre (`p0s31f6`) es la ubicación física en el hardware (bus/slot), específica de cada equipo
- `wl...` = Wireless LAN (WiFi)

Para confirmar el tipo sin interpretar el nombre:
```bash
nmcli device status
```

**IP privada vs pública:** una IP como `192.168.0.24` solo funciona dentro de tu red local (casa/wifi) — nadie desde internet se conecta directo a ella. Tu router tiene una IP pública distinta y traduce el tráfico hacia tu laptop (esto se llama NAT).

**Notación `/24` (CIDR):** indica el tamaño de la red — `/24` permite hasta 254 dispositivos en el mismo rango (ej. `192.168.0.1` al `192.168.0.254`).

**IPv6:** versión más nueva y larga de direcciones IP (formato con `:`, ej. `2607:f8b0:4012:810::200e`), coexiste con IPv4. Se profundizará más adelante junto con subnetting cuando se vea escaneo de redes con Nmap.

```bash
ping -c 4 google.com
```
Envía paquetes de prueba a un destino y mide tiempo de respuesta. `-c 4` = enviar solo 4 paquetes y detenerse (sin la bandera, corre indefinido hasta `Ctrl+C`). Usa el protocolo **ICMP**, no TCP/UDP.

Lectura de resultado:
- `icmp_seq` = número de secuencia del paquete
- `ttl` = Time To Live — saltos restantes antes de expirar
- `time` = latencia (ida y vuelta) en milisegundos

**Uso en hacking ético:** confirma si un objetivo "está vivo". Un ping sin respuesta no confirma que el servidor esté apagado — muchos firewalls bloquean ICMP a propósito.

```bash
sudo apt install traceroute -y
traceroute google.com
```
Muestra cada "salto" (router intermedio) entre tu máquina y el destino. Un salto con `* * *` significa que ese router no respondió (normal, no indica caída). El tiempo sube progresivamente porque cada salto añade distancia real.

**Uso en hacking ético:** entender la topología de red hacia un objetivo, a veces identificar el proveedor/datacenter donde está alojado un servidor.

```bash
sudo apt install net-tools -y
netstat -tulnp
```
Muestra qué puertos están abiertos y **escuchando** (listening) en tu propia máquina, y qué proceso los usa.

- `-t` = TCP
- `-u` = UDP
- `-l` = solo mostrar los que están escuchando
- `-n` = mostrar números en vez de resolver nombres (más rápido)
- `-p` = mostrar programa/PID asociado (requiere `sudo` para ver procesos de otros usuarios)

**"Listening" (escuchando):** el programa dejó un puerto abierto esperando que alguien se conecte — como un teléfono descolgado esperando que marquen.

**Dato de seguridad clave:** un servicio escuchando en `127.0.0.1` (loopback) solo es accesible desde la misma máquina. Uno escuchando en `0.0.0.0` es accesible desde toda la red — esta diferencia es central en auditorías de seguridad. Ejemplo real: PostgreSQL, por defecto, escucha en `127.0.0.1:5432` — solo tu propia laptop puede conectarse a esa base de datos.

**Uso en hacking ético:** exactamente lo que se revisa en un servidor recién comprometido o en auditoría propia — qué servicios corren, en qué puertos, y si están expuestos a la red de más de lo necesario.

---

## 8. Glosario de siglas

| Sigla | Significado |
|-------|-------------|
| **TCP** | Transmission Control Protocol |
| **UDP** | User Datagram Protocol |
| **IP** | Internet Protocol |
| **DNS** | Domain Name System |
| **ICMP** | Internet Control Message Protocol (protocolo que usa `ping`) |
| **seq** (en `icmp_seq`) | Sequence — número de secuencia del paquete |
| **TTL** | Time To Live |
| **CIDR** | Classless Inter-Domain Routing (notación `/24`) |
| **NAT** | Network Address Translation |
| **SQL** | Structured Query Language |
| **CUPS** | Common Unix Printing System (servicio de impresión, puerto 631) |
| **NTP** | Network Time Protocol (sincronización de hora) |
| **mDNS** | Multicast DNS (descubrimiento de dispositivos en red local, puerto 5353) |

> Nota: **PostgreSQL** no es una sigla, es el nombre propio de un sistema gestor de bases de datos de código abierto que implementa SQL.

---

## 9. Atajos de teclado en Bash

| Atajo | Qué hace |
|-------|----------|
| `Ctrl + U` | Borra toda la línea desde el cursor hacia el inicio |
| `Ctrl + K` | Borra toda la línea desde el cursor hacia el final |
| `Ctrl + C` | Cancela el comando actual / interrumpe un proceso corriendo |
| `Ctrl + L` | Limpia la pantalla (igual que escribir `clear`) |
| `Ctrl + A` | Mueve el cursor al inicio de la línea |
| `Ctrl + E` | Mueve el cursor al final de la línea |
| `Ctrl + R` | Busca en el historial de comandos anteriores |
| `Ctrl + W` | Borra la última palabra antes del cursor |
| Flecha arriba/abajo | Navega entre comandos anteriores |
| `Tab` | Autocompleta nombres de archivos/carpetas (úsalo siempre, evita errores de tipeo) |

---

## 10. Git — flujo de trabajo y conceptos clave

### Flujo básico establecido

```bash
git add .                          # prepara TODOS los cambios de la carpeta actual para el commit
git status                         # confirma qué se detectó antes de comitear (buen hábito, siempre revisar)
git commit -m "mensaje descriptivo" # guarda los cambios preparados con un mensaje
git push                           # sube los cambios a GitHub
```

**El punto `.` en `git add .`** significa "todo lo que esté en la carpeta actual y subcarpetas". Alternativas más específicas: `git add archivo.txt` (solo ese archivo) o `git add carpeta/` (solo esa carpeta).

### Autenticación con Personal Access Token (PAT)

GitHub ya no acepta la contraseña normal de la cuenta para operaciones de Git — se necesita un **token** generado en GitHub (Settings → Developer settings → Personal access tokens). Se usa en el campo de "contraseña" al hacer `git push`.

Para no repetir la autenticación cada vez:
```bash
git config --global credential.helper store
```
Nota de seguridad: esto guarda el token en texto plano en `~/.git-credentials` — razonable si eres el único usuario de la máquina.

### Moverte entre carpetas y repositorios

- Git busca automáticamente la carpeta `.git` en el directorio actual o en cualquier carpeta padre — puedes estar en una subcarpeta del repo y los comandos de Git funcionan igual.
- Cada repositorio clonado es independiente; puedes tener varios en distintas carpetas sin que se mezclen.
- Antes de hacer cambios, confirma en qué repo estás:
```bash
pwd            # tu ubicación actual
git remote -v  # a qué repositorio de GitHub está conectada esta carpeta
```

---

## 11. Escritorio remoto (acceso a la laptop desde otra computadora)

Para ver/controlar la laptop Ubuntu desde otra computadora en la misma red WiFi, se activó **GNOME Remote Desktop** (nativo de Ubuntu, en Configuración → Sistema → Escritorio remoto) — no se instaló `xrdp` por separado.

**Conexión desde Windows:** `Windows + R` → `mstsc` → IP de la laptop → usuario/contraseña del sistema.

**Consideraciones:**
- La IP local de la laptop cambia según la red a la que se conecte (casa, escuela, etc.) — solo es accesible desde dentro de la misma red donde esté conectada.
- Mientras el escritorio remoto esté activado, la laptop acepta conexiones en cualquier red donde se conecte — por seguridad, conviene desactivarlo en redes que no sean la de casa.
- Pendiente de confirmar: si la opción de escritorio remoto permanece activada tras reiniciar, o si se debe reactivar manualmente cada vez.

**Cambiar entre la sesión remota y la computadora local (desde Windows/mstsc):**
- En pantalla completa: `Ctrl + Alt + Break` (o `Ctrl+Alt+Fn+B` en laptops sin tecla Break) saca la sesión a modo ventana
- En modo ventana: `Alt+Tab` normal de Windows ya cambia entre la sesión remota y otras aplicaciones

---

## 12. Pendientes / próximos temas

- Scripting básico en Bash (variables, condicionales, loops)
- Confirmar persistencia del escritorio remoto tras reiniciar
- Subnetting e IPv6 a profundidad (junto con Nmap más adelante)

---

## Preguntas para aclarar más adelante

-
-
-
