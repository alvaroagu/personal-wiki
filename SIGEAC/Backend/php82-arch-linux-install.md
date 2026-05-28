# Guía de Instalación y Configuración de PHP 8.2 en Arch Linux

Esta guía detalla cómo instalar PHP 8.2 y resolver los problemas de dependencias con Composer (para herramientas como PhpSpreadsheet, Laravel, Excel, etc.) cuando los repositorios oficiales de Arch ya han avanzado a versiones superiores.

## 1. Instalar PHP 8.2 y extensiones base desde el AUR
Utiliza tu asistente de AUR (`yay` o `paru`) para instalar la versión exacta de PHP 8.2 y la suite de módulos iniciales que requieren la mayoría de las aplicaciones modernas:

```bash
yay -S php82 php82-{cli,curl,dom,fileinfo,gd,iconv,intl,mbstring,openssl,pdo,pgsql,phar,sqlite,tokenizer,xml,zip}
```

# 2. Instalar los módulos XML adicionales indispensables

yay -S php82-simplexml php82-xmlreader php82-xmlwriter


# 3. Limpiar restos de versiones conflictivas
sudo pacman -Rns php-legacy php-legacy-xml

# 4. Crear el Enlace Simbólico Global

sudo rm -f /usr/local/bin/php  # Borra enlaces previos si existen
sudo ln -s /usr/bin/php82 /usr/local/bin/php
hash -r                       # Recarga la tabla de hashes de la shell

# 5. Activar las extensiones en los archivos de configuración

A diferencia de los paquetes nativos de Arch, las extensiones de PHP instaladas desde el AUR se gestionan en archivos individuales dentro del directorio /etc/php82/conf.d/.

Asegúrate de que los archivos carguen la extensión de manera limpia. Abre los siguientes archivos con tu editor de texto (ej. nvim o nano) y elimina el punto y coma (;) al inicio de la línea si estuviera comentado:

/etc/php82/conf.d/15-xml.ini → Debe contener: extension=xml.so

/etc/php82/conf.d/20-gd.ini → Debe contener: extension=gd.so

/etc/php82/conf.d/20-iconv.ini → Debe contener: extension=iconv.so

composer install
