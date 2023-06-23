# TareaIDS2_C
Segunda tarea de IDS
# Atlas-MongoDB-Conection-With-C++

## Prerrequisitos
Tener una cuenta en MongoDB Atlas y un cluster creado
>https://www.mongodb.com/developer/products/atlas/free-atlas-cluster/

Microsoft Visual Studio, durante su instalación seleccionaremos “Desktop development with C++.”
>https://visualstudio.microsoft.com/downloads/

Instala CMake desde
>https://cmake.org/download/

Instala Python desde
>https://www.python.org/downloads/


## Instalación de los drivers
### Mongo-c-driver
Dirigete a https://github.com/mongodb/mongo-c-driver/releases e instala la última version, esta será la primera opción que aparece en links, tal como lo muestra la imagen:

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/fa3efd1d-82e1-4d81-9c0e-22686f4bfc50)

Una vez descargado, extrae los archivos, debería aparecer una carpeta como la de abajo:

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/7e3dd5dd-513b-49cf-9fd5-eb4876248a72)

Mueve esta carpeta a Windows(C:), esto nos facilitará la vida más adelante.

Ahora abre tu terminal, ya sea con el atajo de teclado windows + r y escribiendo cmd o en el buscador de windows.
- Escribimos el comando cd C:\mongo-c-driver-1.24.0 (Esto nos llevará a la carpeta mongo-c-driver-1.24.0 pero dentro de la terminal)
- Creamos una carpeta usando el comando mkdir cmake-build
- Entramos a esta carpeta con el comando cd C:\mongo-c-driver-1.24.0\cmake-build
- Ejecutamos el comando cmake -G "Visual Studio 17 2022" -A x64 -S "C:\mongo-c-driver-1.24.0" -B "C:\mongo-c-driver-1.24.0\cmake-build", debería haber un output parecido a este:

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/c75193c3-7d22-465e-a86f-a5285ec6a6e1)

- Terminada la configuración y generación de archivos para la build, pasamos a su instalación con el comando cmake --build . --config RelWithDebInfo --target install

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/abc24d88-b5be-4ba2-9bb1-bbcf80ff43a4)

- Cuando terminé la instalación, tendremos una carpeta llamada mongo-c-driver en:

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/bd1a9952-8434-4035-be6a-f74fd05ce115)

- Moveremos esta carpeta a C:\

### Mongo-cxx-driver
Entra a https://github.com/mongodb/mongo-cxx-driver/releases y descarga la última versión que se encuentra en assets, debería verse algo así:

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/42e74985-5fea-4e24-a8f5-7e575a097002)

Una vez descargada, extraemos los archivos y llevaremos la carpeta generada a windows(C:)

Como lo hicimos con el driver anterior, entraremos a la terminal
- Escribimos el comando cd C:\mongo-cxx-driver-r3.8.0
- Creamos una carpeta con el comando mkdir build
- Entramos a la carpeta recientemente creada con cd C:\mongo-cxx-driver-r3.8.0\build
- Ejecutamos el comando cmake .. -G "Visual Studio 17 2022" -A x64 -DCMAKE_CXX_STANDARD=17 -DCMAKE_CXX_FLAGS="/Zc:__cplusplus /EHsc" -DCMAKE_PREFIX_PATH=C:\mongo-c-driver -DCMAKE_INSTALL_PREFIX=C:\mongo-cxx-driver
- El output será algo parecido a esto:

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/c7fd1dce-8186-4ee3-a778-d6c0475c85b2)

- Por ultimo ejecutamos el comando de instalación cmake --build . --target install (Nos creará una carpeta llamada mongo-cxx-driver)

## Configuración del entorno de desarrollo en Visual Studio
Entramos a Microsoft Visual Studio (No confundir con Visual Studio Code) y creamos un nuevo proyecto, esogeremos la siguiente opción:

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/eed72518-6305-40f1-b938-50e648b0090c)

Escribe Solution Explorer panel en el buscador, en la ventana haz click derecho donde está el nombre que le pusiste al proyecto (En nuestro caso MongoDBsetup)

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/b6795ef2-f4ac-4791-9042-45c6b272a3cd)

Ve a Configuration Properties > C/C++ > General > Additional Include Directories

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/f91da6f5-ce0a-41bd-910b-c1ceda771b20)

Haz click en la flechita hacia abajo y luego en edit

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/05a4419c-1930-4f00-8000-dfda17023456)

Luego agruega lo siguiente:
- C:\mongo-cxx-driver\include\mongocxx\v_noabi
- C:\mongo-cxx-driver\include\bsoncxx\v_noabi
- C:\mongo-c-driver\include\libbson-1.0
- C:\mongo-c-driver\include\libmongoc-1.0

Ahora ve a Configuration Properties > C/C++ > Language, y cambia a C++17

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/f91e3d35-1b43-4534-bbe8-764ea3b488e6)

Luego ve a Configuration Properties > C/C++ > Command Line y agrega /Zc:__cplusplus en Additional Options

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/28b9ac80-f945-45a4-a567-812cb88f0609)

Siguiendo con la configuración, ve a Configuration Properties > Linker > Input y en Additional Dependencies agrega
- C:\mongo-c-driver\lib\bson-1.0.lib
- C:\mongo-c-driver\lib\mongoc-1.0.lib
- C:\mongo-cxx-driver\lib\bsoncxx.lib
- C:\mongo-cxx-driver\lib\mongocxx.lib

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/cf467f38-da39-4c69-ad1a-3ab6d0951f5a)

Por último, iremos a Configuration Properties > Debugging > Environment y agregaremos PATH=C:\mongo-cxx-driver\bin;C:\mongo-c-driver\bin

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/ac256445-ce6a-4fa3-81d2-b00fbc599482)

## Conectando c++ con mongodb
Copiamos el código que nos da Atlas MongoDB (Habilitamos View full code sample) o el que se encuentra en la carpeta llamada Programa-simple.cpp que se encuentra arriba.

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/bee568f0-cf2b-43bc-8d77-c389487db626)

Copiamos y pegamos el codigo en Visual Studio, ahora cambiamos donde dice <username> y <password> por nuestra cuenta y contraseña

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/10909322-3ef2-4ecd-b3c6-cc5163b28432)

Hacemos click en Local Windows Debugger

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/54741b39-71c5-4116-bb33-c21176c725d7)

Felicidades, te has conectado a tu Base de Datos usando c++

![image](https://github.com/LazaroTupo/Atlas-MongoDB-Conection/assets/123672027/ad1aed1d-d856-4911-9359-c1d94ccf12b6)
