# TareaIDS2_C++
Segunda tarea de IDS Grupo 7
# Conexión entre Atlas MongoDB y un proyecto en C++

## Prerrequisitos
Tener una cuenta en MongoDB Atlas y un cluster creado
>https://www.mongodb.com/developer/products/atlas/free-atlas-cluster/

Microsoft Visual Studio Community, cuando aparezca la ventana de "cargas de trabajo", seleccionamos "Desarrollo para el Escritorio con C++"
>https://visualstudio.microsoft.com/downloads/

Instala CMake desde
>https://cmake.org/download/

Instala Python desde
>https://www.python.org/downloads/

Importante:
Los comandos usados en esta guia pueden variar dependiendo de la version disponible en el momento de la descarga. Usted, por su cuenta debe cambiarlos para que coincida con su version descargada.

## Instalación de los drivers
### Mongo-c-driver
Dirigete a https://github.com/mongodb/mongo-c-driver/releases y descarga el tarball de la última versión, en mi caso la 1.24.1

Una vez descargado, extrae los archivos, debería aparecer una carpeta llamada mongo-c-driver-1.24.1

Mueve esta carpeta a Windows(C:) para mayor facilidad de uso.

Ahora abre tu terminal como administrador(lo puedes hacer dandole click derecho al logo de windows en la barra de tareas y seleccionando "Terminal (Administrador)"
- Escribimos el comando cd C:\mongo-c-driver-1.24.1 (Esto nos llevará a la carpeta mongo-c-driver-1.24.1 pero dentro de la terminal)
- Creamos una carpeta usando el comando mkdir cmake-build
- Entramos a esta carpeta con el comando cd C:\mongo-c-driver-1.24.1\cmake-build
- Ejecutamos el comando cmake -G "Visual Studio 17 2022" -A x64 -S "C:\mongo-c-driver-1.24.1" -B "C:\mongo-c-driver-1.24.1\cmake-build".
- Terminado el proceso, pasamos a su instalación con el comando cmake --build . --config RelWithDebInfo --target install
- Cuando termine la instalación, tendremos una carpeta llamada "mongo-c-driver" en C:\Program Files
- Moveremos esta carpeta a C:\

### Mongo-cxx-driver
Entra a https://github.com/mongodb/mongo-cxx-driver/releases y descarga la última versión que se encuentra en assets, en mi caso 3.8.0

Una vez descargada, extraemos hasta que nos quede una carpeta llamada "mongo-cxx-driver-r3.8.0", esta carpeta la movemos a C:\

Como lo hicimos con el driver anterior, entraremos a la terminal como administrador
- Escribimos el comando cd C:\mongo-cxx-driver-r3.8.0
- Creamos una carpeta con el comando mkdir build
- Entramos a la carpeta recientemente creada con cd C:\mongo-cxx-driver-r3.8.0\build
- Ejecutamos el comando cmake .. -G "Visual Studio 17 2022" -A x64 -DCMAKE_CXX_STANDARD=17 -DCMAKE_CXX_FLAGS="/Zc:__cplusplus /EHsc" -DCMAKE_PREFIX_PATH=C:\mongo-c-driver -DCMAKE_INSTALL_PREFIX=C:\mongo-cxx-driver
- Esperames a que termine el proceso
- Por ultimo ejecutamos el comando de instalación cmake --build . --target install (Nos creará una carpeta llamada mongo-cxx-driver, verifique que se encuentre en C:\)

## Configuración del entorno de desarrollo en Visual Studio
-Entramos a Microsoft Visual Studio Community y creamos un nuevo proyecto

-Seleccionamos la plantilla de "Aplicacion de Consola"

-Visual Studio habrá creado un proyecto y abierto un arhcivo .cpp que imprime un "Hola Mundo". Navega hasta el panel de Solution Explorer, haga click derecho en el nombre de la solución y haga click en propiedades

-Ve a Configuration Properties > C/C++ > General > Additional Include Directories

-Dentro agrega los siguientes directorios:
   - C:\mongo-cxx-driver\include\mongocxx\v_noabi
   - C:\mongo-cxx-driver\include\bsoncxx\v_noabi
   - C:\mongo-c-driver\include\libbson-1.0
   - C:\mongo-c-driver\include\libmongoc-1.0

-Ahora ve a Configuration Properties > C/C++ > Language, y cambia el lenguaje estandar a C++ 17

-Luego ve a Configuration Properties > C/C++ > Command Line y agrega /Zc:__cplusplus en Additional Options

-Ve a Configuration Properties > Linker > Input y en Additional Dependencies agrega
   - C:\mongo-c-driver\lib\bson-1.0.lib
   - C:\mongo-c-driver\lib\mongoc-1.0.lib
   - C:\mongo-cxx-driver\lib\bsoncxx.lib
   - C:\mongo-cxx-driver\lib\mongocxx.lib

-Por último, iremos a Configuration Properties > Debugging > Environment y agregaremos PATH=C:\mongo-cxx-driver\bin;C:\mongo-c-driver\bin

## Conectando nuestro proyecto en C++ con mongodb
-Copiamos el código que nos da Atlas MongoDB (Habilitamos View full code sample).

-Pegamos el codigo en Visual Studio, ahora cambiamos donde dice <username> y <password> por nuestro usuario y contraseña

-Hacemos click en Local Windows Debugger para compilar y ejecutar el proyecto

-Eso es todo, si en la consola te aparecen los mensajes correctos, significa que ya estas conectado a tu base de datos de MongoDB
