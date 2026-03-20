# Chuletazo Git de 0 a PRO
# --- INICIALIZAR PROYECTO ---
cd ruta/del/proyecto              # entrar a la carpeta del proyecto
git init                          # inicializar repo local
git status                        # ver estado de archivos
git add .                         # añadir todos los archivos
git commit -m "Primer commit"     # guardar cambios en local

# --- CONECTAR CON GITHUB ---
# (primero crear repo vacío en GitHub, ej. https://github.com/jtintin/chuletazos.git)
git remote add origin https://github.com/jtintin/chuletazos.git
git branch -M main                # renombrar rama principal a main
git push -u origin main           # subir al remoto y enlazar rama

# --- FLUJO DE ACTUALIZACIÓN ---
git status                        # ver qué cambió
git add .                         # añadir cambios
git commit -m "mensaje descriptivo"
git push                          # subir cambios a GitHub

# --- VERIFICAR QUE TODO ESTÁ BIEN ---
git branch                        # ver rama activa (* main)
git log --oneline                 # historial de commits
git remote -v                     # confirmar conexión con GitHub
# En GitHub: ver archivos y README actualizados

# --- RAMAS (OPCIONAL) ---
git checkout -b nueva-rama        # crear y cambiar a rama nueva
git push -u origin nueva-rama     # subir rama al remoto
git checkout main                 # volver a rama principal
git merge nueva-rama              # fusionar cambios en main
git branch -d nueva-rama          # borrar rama local
git push origin --delete nueva-rama   # borrar rama remota

# --- PRO TIPS ---
git diff                          # ver diferencias antes de commit
git log --oneline --graph --decorate   # historial visual
git stash                         # guardar cambios sin commit
git stash pop                     # recuperar cambios guardados

