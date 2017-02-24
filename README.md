Como primer paso debemos de instalar Node.Js, ya que necesitaremos utilizar node packages manger (npm).
Ya instalado node.js crearemos una carpeta para que contenga el nuevo proyecto y nos dirigimos a la consola del S.O.
En la consola debemos situarnos en la ruta de nuestro proyecto y luego escribir:

    C:/proyecto> npm init
    
Al ejecutar este comando se solicitara informacion correspondiente al proyecto (nombre, autor, descripcion, version etc).
Es de suma importancia crear el archivo principal de la aplicacion ya que este es el que se ejeutara y desplegara la nueva ventana de electron.

Al completar los datos de nuestra aplicacion se generara un archivo similar a este:

    {
      "name": "hello",
      "version": "1.0.0",
      "description": "Aplicacion para saludar",
      "main": "main.js",
      "scripts": {
        "start": "electron ."
       },
      "author": "Marcelo Gallardo",
      "license": "ISC",
      "devDependencies": {
      "electron": "^1.4.15"
      }
    }

Similar debido a que de manera manual se ha agregado `"start": "electron ."` lo cual es necesario para que se ejectue la ventana de manera correcta.

Como paso siguiente debemos instalar electron vía npm.

    npm install electron --save-dev -verbose
    
Y ya esta, ahora al codigo.

Lo primero que haremos sera instanciar la ventana que contendra nuestra aplicacion. esta puede ser declarada de la siguiente manera

  ##MAIN.JS

    const electron = require('electron');
    const app = electron.app;
    const BrowserWindow = electron.BrowserWindow;
    const path = require('path');
    const url = require('url');

    //Declarar la ventana
    var mainWindow;

    app.on('ready', function(){
    
        //Instanciar la ventana
	      mainWindow = new BrowserWindow({
		    width: 1280, // Ancho de la ventana
		    height: 720, //Alto de la ventana
		    backgroundColor: '#494949' //Color del fondo (Opcional)
	    });
	    mainWindow.setMenu(null); //Quita el menu superior agregado por defecto (Opcional)
	    mainWindow.loadURL('https://facebook.com'); //Si queremos incluir un sitio externo.
      
      //Si queremos mostrar nuestro propio sistio, que se encuentra en la misma carpeta del proyecto
	    mainWindow.loadURL(url.format({
		    pathname: path.join(__dirname, 'index.html'),
		    protocol: 'file:',
		    slashes: true		
	    }));
    });

##INDEX.HTML

    <!DOCTYPE html>
    <html lang="en" ng-app="">
      <head>
	      <meta charset="UTF-8">
	      <title>Hola mundo</title>
	      <script src="angular.min.js"></script>
	      <style>
		      body{
			      color: #eee;
		      }
	      </style>
      </head>
      <body>
	        <input type="text" ng-model="nombre" placeholder="Escribe tu nombre">
	        <h1>Electron te desea un buen día, {{nombre}}</h1>
          
          <!--En caso de requerir jQuery, esta es la manera que debe ser llamado en Electron-->
          <script>window.$ = window.jQuery = require ('./ruta/hacia/jQuery/jquery-2.2.3.min.js')</script>
      </body>
      </html>
      
 Eso es todo, con eso nuestra aplicacion estara lista para ser compilada y antes de eso podemos probarla ejecutando el comando `npm start`
 
 #Compilando la aplicacion
 
Para compilar nuestra aplicacion utilizaremos `electron-packager` el cual debe ser instalado de manera global.

    npm install -g electron-packager
    
 Ahora solo falta compilar nuestra aplicacion con el siguiente comando.
 
      npm electron-packager . --asar

    
