# Notas para pantallas Omnicore.

Relación de notas y ejemplos para la programación de las pantallas en los robots [Onmicore de ABB](https://new.abb.com/products/robotics/es/controladores/omnicore), usando la aplicación [AppStudio](https://new.abb.com/products/robotics/es/software-y-digital/appstudio).

## Indice

[Acceder a un componente de la pantalla](#acceder-a-un-componente-de-la-pantalla)

[Buscar objetos select en Instance](#buscar-objetos-select-en-instance)

[Generar una señal pulsada](#generar-una-señal-pulsada)

[Cambiar el valor de una salidad digital](#cambiar-el-valor-de-una-salida-digital)

[Cambiar el valor de una variable](#cambiar-el-valor-de-una-variable)

[Cómo usar los componentes en las funciones de usuario](#cómo-usar-los-componentes-en-las-funciones-de-usuario)

[Leer el valor de una variable](#leer-el-valor-de-una-variable)

[Mostrar Mensaje emergente](#mostrar-mensaje-emergente)

[Mostrar aplicación en navegador](#mostrar-aplicación-en-navegador)

[Ocultar un Layout por código](#ocultar-un-layout-por-código)


## Acceder a un componente de la pantalla

Accede al componente Input_6 y muestra el contenido de su propiedad `text`

    console.log(Instance);
    let texto = Instance.Input_6.text;
    console.log(texto);

## Buscar objetos select en Instance

     function worksWithSelects() {
       const selects = []
       const keys = Object.keys(Instance)
       for (let x=0; x < keys.length; x++) {
         if (keys[x].indexOf('Select_') > -1) {
           selects.push(Instance[keys[x]])
            }
         }    
      console.info('selects found: ', selects)
      }

Dentro del for puedes incluso hacer lo que quieras, yo te armé un array con los objetos para mayor claridad

    selects.push(Instance[keys[x]])  // esto guarda en el array el objeto.


`Keys` es un array de strings con el nombre de cada prop del objeto Instance

`keys[x] ` accede a la position x del array keys (en js, los arrays inician en 0).

`Instance[]`, en JavaScript, se puede acceder a la propiedad de un objeto usando los corchetes y el nombre (o una variable con el nombre). Por ejemplo: 

      Instance.Select_1 === Instance['Select_1']


## Cambiar el valor de una salida digital

Cambiar a valor 1

            API.RWS.SIGNAL.setSignalValue("domysenal",1,{});

Cambiar a valor 0

            API.RWS.SIGNAL.setSignalValue("domysenal",1,{});

## Cambiar el valor de una variable

    let retorno = await API.RAPID.setVariableValue ('TestModule','jorge','Valor nuevo','T_ROB1');

## Cómo usar los componentes en las funciones de usuario

Los componentes desciende de la clase TComponent, por lo cual hay que hacer referencia a ella. 

Observa esta función de usuario:

    async (header, mensaje) => {
     /* Esta función recibe dos parámetros,
     header, el mensaje para el título de la ventana, tiene que ser una cadena
     mensaje, contiene el mensaje a mostrar, puede ser una cadena o un array.
     Devuelve true en el caso que se pulse Aceptar, o false en caso contrario
    */

    TComponents.Popup_A.confirm(
      header,
      mensaje,
      function (action) {
        if (action === TComponents.Popup_A.OK) {
          console.log('ok');
        } else if (action == TComponents.Popup_A.CANCEL) {
        console.log('cancel');
        }
      }
      );
    }

## Generar una señal pulsada

Este ejemplo genera una señal pulsada. Durante 1000 milisegundo estara a nivel alto, y volverá a nivel bajo

      await API.RWS.SIGNAL.setSignalValue("R1diP3Comp3Aprox",1, {mode:"pulse",activePulse:1000,passivePulse:1000});

## Leer los archivos de un directorio

            const programas = await API.FILESYSTEM.getDirectoryContents ('$HOME/');
            Instance.Select_1.optionItems = imagesInfo.files.map((item) => `${item}|${item}`).join(';');

Este ejemplo lee los archivos del directorio home, y los carga en un componente Select

## Leer un array 

      let variableValue = await API.RAPID.getVariableValue(
     'TestModule',
     'lista',
     'T_ROB1',);
      console.log(variableValue);
      console.log(variableValue.length);
      for (let step = 0; step < variableValue.length; step++){
      console.log(variableValue[step]);}

## Leer el valor de una variable

      let variableValue = await API.RAPID.getVariableValue(
     'TestModule',
     'jorge',
     'T_ROB1',);
      console.log(variableValue);

## Mostrar Mensaje emergente

`Poup_A`, recibe tres parámetros. 

* Texto para la cabecera
* Texto que representa el mensaje, puede ser un array. Es opcional
* Un función callback. Que se ejecuta si el usuario pulsa un botón. Esta función recibe un parámetro con el botón pulsado. Es opcional.



      Popup_A.confirm(
      "XXX module not yet loaded on the controller", ['This module is required for this WebApp.', 'Do you want to load the module?'],function(action){
       if ( action === Popup_A.OK) {
        console.log("Load the module here...")
       } 
        else if (action == Popup_A.CANCEL) {
        console.log("Abort mission!");
        }
       }
      );

## Mostrar aplicación en navegador

Dirección:      
      
      https://127.0.0.1:80/fileservice/$home/WebApps/jorge/index.html

Donde `jorge` es el nombre de la aplicación.
Esto abrirá la aplicación jorge que está en un controlador virtual. 

En el caso que sea un controlador real, ser debe poner la ip del mismo, que es `192.168.15.1` cuando se conecta un pc por el puerto MGMT (en la versión de robots anterior, `IRC5`, su nombre era por puerto de servicio).

## Ocultar un Layout por código

La propiedad container es un DIV por tanto se puede cambiar su propiedad display

      if (Instance.LayoutInfobox_83.container.style.display === '')
      {
            Instance.LayoutInfobox_83.container.style.display = 'none';
      }
      else
      {
            Instance.LayoutInfobox_83.container.style.display = '';
      }
 