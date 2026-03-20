# ---LARAVEL DE 0 A PRO ---
# --- INICIALIZAR PROYECTO ---
laravel new proyecto              # crear nuevo proyecto Laravel
php artisan serve                 # levantar servidor local
composer install                  # instalar dependencias
cp .env.example .env              # copiar archivo de entorno
php artisan key:generate          # generar APP_KEY

# --- MIGRACIONES ---
php artisan migrate               # ejecutar migraciones
php artisan migrate:rollback      # revertir última migración
php artisan migrate:refresh       # rollback + migrate
php artisan migrate:fresh         # borrar tablas y migrar de nuevo
php artisan migrate --seed        # migrar + ejecutar seeders

# --- SEEDERS Y FACTORIES ---
php artisan make:seeder UserSeeder        # crear seeder
php artisan db:seed --class=UserSeeder    # ejecutar seeder específico
php artisan db:seed                       # ejecutar todos los seeders
php artisan make:factory UserFactory      # crear factory
php artisan tinker                        # abrir consola interactiva
User::factory()->count(10)->create();     # generar datos con factory

# --- MODELOS Y CONTROLADORES ---
php artisan make:model Producto -m        # crear modelo + migración
php artisan make:controller ProductoController --resource
php artisan make:request ProductoRequest  # crear request validación

# --- CRUD BÁSICO ---
# En controlador resource:
# index() -> listar
# create() -> formulario
# store() -> guardar
# show() -> mostrar
# edit() -> formulario edición
# update() -> actualizar
# destroy() -> borrar

# --- RUTAS ---
# En routes/web.php
Route::resource('productos', ProductoController::class);

# --- CACHÉ Y CONFIGURACIÓN ---
php artisan config:cache          # cachear configuración
php artisan route:cache           # cachear rutas
php artisan view:clear            # limpiar vistas compiladas
php artisan cache:clear           # limpiar caché general

# --- DEBUG Y UTILIDADES ---
php artisan tinker                # consola interactiva
php artisan migrate:status        # ver estado de migraciones
php artisan route:list            # ver todas las rutas
php artisan schedule:list         # ver tareas programadas

# --- PRO TIPS ---
composer dump-autoload            # regenerar autoload
php artisan optimize              # optimizar proyecto
php artisan down                  # poner app en mantenimiento
php artisan up                    # sacar app de mantenimiento

