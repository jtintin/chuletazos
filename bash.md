# --- BASH DE 0 A PRO (INSTALACIÓN, USO, REGEX, AWK, SED) ---

# --- INSTALACIÓN ---
# En sistemas basados en Debian/Ubuntu:
sudo apt update                          # actualizar lista de paquetes
sudo apt install bash                    # instalar Bash (si no está)
bash --version                           # verificar versión instalada

# En sistemas basados en RPM (RedHat, CentOS, Fedora, AlmaLinux):
sudo yum install bash                    # instalar Bash con yum
sudo dnf install bash                    # instalar Bash con dnf (Fedora/AlmaLinux)
bash --version                           # verificar versión instalada

# --- NAVEGACIÓN Y ARCHIVOS ---
pwd                                      # mostrar ruta actual
ls -l                                    # listar archivos con detalles
cd carpeta                               # entrar a carpeta
mkdir nueva_carpeta                      # crear carpeta
touch archivo.txt                        # crear archivo vacío
rm archivo.txt                           # borrar archivo
cp origen.txt destino.txt                # copiar archivo
mv archivo.txt carpeta/                  # mover archivo

# --- GREP Y EXPRESIONES REGULARES ---
grep "error" archivo.log                 # buscar palabra "error"
grep -i "warning" archivo.log            # búsqueda sin distinguir mayúsculas
grep -E "error|warning" archivo.log      # regex: error o warning
grep -E "^[0-9]{3}" archivo.log          # líneas que empiezan con 3 dígitos
grep -v "info" archivo.log               # mostrar líneas que NO contienen "info"

# --- AWK ---
awk '{print $1}' archivo.txt             # imprimir primera columna
awk '{print $1,$3}' archivo.txt          # imprimir columnas 1 y 3
awk '/error/ {print $2}' archivo.log     # imprimir segunda columna si contiene "error"
awk '{s+=$2} END {print s}' archivo.txt  # sumar valores de la segunda columna

# --- SED ---
sed 's/error/ERROR/g' archivo.log        # reemplazar "error" por "ERROR"
sed '/warning/d' archivo.log             # eliminar líneas con "warning"
sed -n '1,5p' archivo.txt                # mostrar solo líneas 1 a 5
sed -i 's/foo/bar/g' archivo.txt         # reemplazo directo en archivo

# --- BACKUP Y COMPRESIÓN ---
tar -cvf backup.tar carpeta/             # crear archivo tar
tar -xvf backup.tar                      # extraer archivo tar
gzip archivo.txt                         # comprimir archivo
gunzip archivo.txt.gz                    # descomprimir archivo

# --- HISTORIAL Y FILTROS ---
history                                  # ver historial de comandos
!!                                       # repetir último comando
!n                                       # repetir comando número n del historial
grep "ls" ~/.bash_history                # buscar comandos "ls" en historial

# --- PRO TIPS ---
find . -name "*.txt"                     # buscar archivos por nombre
grep -r "TODO" .                         # buscar recursivamente "TODO"
awk '{print NR,$0}' archivo.txt          # numerar líneas

