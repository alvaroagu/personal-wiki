# 🛠️ Solución Definitiva: Driver SQL Server (PDO_SQLSRV) en PHP 8.2 para Arch Linux

Esta guía detalla el procedimiento exacto para compilar e integrar los drivers de Microsoft SQL Server en una instalación específica de **PHP 8.2** (AUR), solucionando los conflictos de dependencias de símbolos y el orden de carga nativo de Arch Linux.

---

### 📦 Paso 1: Instalar el Driver ODBC de Microsoft
Antes de compilar las extensiones de PHP, necesitas el entorno de comunicación oficial de Microsoft. Instálalo desde la AUR usando tu helper (`yay` o `paru`):

```bash
yay -S msodbcsql

Paso 2:

# Crear un directorio temporal de trabajo y acceder a él
mkdir -p /tmp/php-drivers && cd /tmp/php-drivers

# Descargar las versiones de los drivers compatibles con PHP 8.2
wget [https://pecl.php.net/get/sqlsrv-5.12.0.tgz](https://pecl.php.net/get/sqlsrv-5.12.0.tgz)
wget [https://pecl.php.net/get/pdo_sqlsrv-5.12.0.tgz](https://pecl.php.net/get/pdo_sqlsrv-5.12.0.tgz)

# Extraer ambos archivos tarball
tar -xzvf sqlsrv-5.12.0.tgz
tar -xzvf pdo_sqlsrv-5.12.0.tgz

# === COMPILACIÓN DE SQLSRV ===
cd sqlsrv-5.12.0
phpize82
./configure --with-php-config=php-config82
make
sudo make install

# === COMPILACIÓN DE PDO_SQLSRV ===
cd ../pdo_sqlsrv-5.12.0
phpize82
./configure --with-php-config=php-config82 CFLAGS="-I/usr/include/php82/ext/pdo"
make
sudo make install



Paso 3: Limpiar el Archivo php.ini Principal

sudo nvim /etc/php82/php.ini

Ve al final del archivo y elimina por completo cualquier línea de extensión que hayas añadido manualmente relacionada con sqlsrv, pdo_sqlsrv o pdo.so.

Guarda los cambios y cierra el editor ejecutando :wq


Paso 4: Crear la Configuración Automatizada en conf.d

sudo nvim /etc/php82/conf.d/sqlsrv.ini

Inserta únicamente las siguientes dos líneas de código:

extension=sqlsrv.so
extension=pdo_sqlsrv.so


Paso 5: Verificar la Correcta Carga de los Módulos

php82 -m | grep -i sqlsrv

Resultado limpio esperado en consola:

pdo_sqlsrv
sqlsrv



