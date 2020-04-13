# ejemplo
ejemplo
<! DOCTYPE html >
< html  lang = " en " >
	< cabeza >
		< título > three.js / editor </ título >
		< meta  charset = " utf-8 " >
		< meta  name = " viewport " content = " ancho = ancho del dispositivo, escalable por el usuario = no, escala mínima = 1.0, escala máxima = 1.0 " >
		< link  rel = " apple-touch-icon " href = " images / icon.png " >
		< link  rel = " manifest " href = " manifest.json " >
		< link  rel = " icono de acceso directo " href = " ../files/favicon.ico " />
	</ cabeza >
	< cuerpo >
		< link  rel = " stylesheet " href = " css / main.css " >

		< script  src = " ../examples / js / libs / jszip.min.js " > </ script >
		< script  src = " ../examples / js / libs / draco / draco_encoder.js " > </ script >

		< link  rel = " stylesheet " href = " js / libs / codemirror / codemirror.css " >
		< link  rel = " stylesheet " href = " js / libs / codemirror / theme / monokai.css " >
		< script  src = " js / libs / codemirror / codemirror.js " > </ script >
		< script  src = " js / libs / codemirror / mode / javascript.js " > </ script >
		< script  src = " js / libs / codemirror / mode / glsl.js " > </ script >

		< script  src = " js / libs / esprima.js " > </ script >
		< script  src = " js / libs / jsonlint.js " > </ script >
		< script  src = " js / libs / glslprep.min.js " > </ script >

		< link  rel = " stylesheet " href = " js / libs / codemirror / addon / dialog.css " >
		< link  rel = " stylesheet " href = " js / libs / codemirror / addon / show-hint.css " >
		< link  rel = " stylesheet " href = " js / libs / codemirror / addon / tern.css " >

		< script  src = " js / libs / codemirror / addon / dialog.js " > </ script >
		< script  src = " js / libs / codemirror / addon / show-hint.js " > </ script >
		< script  src = " js / libs / codemirror / addon / tern.js " > </ script >
		< script  src = " js / libs / acorn / acorn.js " > </ script >
		< script  src = " js / libs / acorn / acorn_loose.js " > </ script >
		< script  src = " js / libs / acorn / walk.js " > </ script >
		< script  src = " js / libs / ternjs / polyfill.js " > </ script >
		< script  src = " js / libs / ternjs / signal.js " > </ script >
		< script  src = " js / libs / ternjs / tern.js " > </ script >
		< script  src = " js / libs / ternjs / def.js " > </ script >
		< script  src = " js / libs / ternjs / comment.js " > </ script >
		< script  src = " js / libs / ternjs / infer.js " > </ script >
		< script  src = " js / libs / ternjs / doc_comment.js " > </ script >
		< script  src = " js / libs / tern-threejs / threejs.js " > </ script >
		< script  src = " js / libs / señales.min.js " > </ script >
		< script  src = " ../examples / js / vr / HelioWebXRPolyfill.js " > </ script >

		< script  type = " module " >

			importar * como  TRES  desde  '../build/three.module.js' ;

			importar  {  Editor  }  desde  './js/Editor.js' ;
			importar  {  Viewport  }  desde  './js/Viewport.js' ;
			importar  {  Toolbar  }  desde  './js/Toolbar.js' ;
			importar  {  Script  }  desde  './js/Script.js' ;
			importar  {  Player  }  desde  './js/Player.js' ;
			importar  {  Barra lateral  }  desde  './js/Sidebar.js' ;
			importación  {  barra de menú  }  de  './js/Menubar.js ;
			importar  {  VRButton  }  desde  '../examples/jsm/webxr/VRButton.js' ;

			ventana . URL  =  ventana . URL  ||  ventana . webkitURL ;
			ventana . BlobBuilder  =  ventana . BlobBuilder  ||  ventana . WebKitBlobBuilder  ||  ventana . MozBlobBuilder ;

			Número . prototipo . formato  =  función  ( )  {

				devuelve  esto . toString ( ) . reemplazar (  / ( \ d ) (? = ( \ d { 3 } ) + (? ! \ d ) ) / g ,  "$ 1,"  ) ;

			} ;

			//

			 editor  var =  nuevo  editor ( ) ;

			ventana . editor  =  editor ;  // Exponer el editor a la consola
			ventana . TRES  =  TRES ;  // Expone TRES a las secuencias de comandos de la aplicación y la consola
			ventana . VRButton  =  VRButton ;  // Exponer VRButton a scripts de aplicaciones

			 viewport  var =  nuevo  Viewport (  editor  ) ;
			documento . cuerpo . appendChild (  viewport . dom  ) ;

			 barra de herramientas  var =  nueva  barra de herramientas (  editor  ) ;
			documento . cuerpo . appendChild (  barra de herramientas . dom  ) ;

			var  guión  =  nueva  escritura (  editor  ) ;
			documento . cuerpo . appendChild (  script . dom  ) ;

			 jugador  var =  nuevo  jugador (  editor  ) ;
			documento . cuerpo . appendChild (  jugador . dom  ) ;

			var  barra lateral  =  nueva  barra lateral (  editor  ) ;
			documento . cuerpo . appendChild (  barra lateral . dom  ) ;

			var  barra de menú  =  nueva  barra de menú (  editor  ) ;
			documento . cuerpo . appendChild (  barra de menú . dom  ) ;

			//

			editor . el almacenamiento . init (  function  ( )  {

				editor . el almacenamiento . get (  función  (  estado  )  {

					if  (  isLoadingFromHash  )  devuelve ;

					if  (  state ! == undefined )  {

						editor . fromJSON (  estado  ) ;

					}

					var  seleccionado  =  editor . config . getKey (  'seleccionado'  ) ;

					if  (  seleccionado ! == undefined )  {

						editor . selectByUuid (  seleccionado  ) ;

					}

				}  ) ;

				//

				var  timeout ;

				función  saveState ( )  {

					if  (  editor . config . getKey (  'autosave'  )  ===  false  )  {

						volver ;

					}

					clearTimeout (  tiempo de espera  ) ;

					timeout  =  setTimeout (  function  ( )  {

						editor . señales . savingStarted . despacho ( ) ;

						timeout  =  setTimeout (  function  ( )  {

							editor . el almacenamiento . set (  editor . toJSON ( )  ) ;

							editor . señales . ahorroFinished . despacho ( ) ;

						} ,  100  ) ;

					} ,  1000  ) ;

				}

				 señales  var =  editor . señales ;

				señales . geometryChanged . agregar (  saveState  ) ;
				señales . objectAdded . agregar (  saveState  ) ;
				señales . objectChanged . agregar (  saveState  ) ;
				señales . objeto eliminado . agregar (  saveState  ) ;
				señales . material modificado . agregar (  saveState  ) ;
				señales . sceneBackgroundChanged . agregar (  saveState  ) ;
				señales . sceneFogChanged . agregar (  saveState  ) ;
				señales . sceneGraphChanged . agregar (  saveState  ) ;
				señales . scriptChanged . agregar (  saveState  ) ;
				señales . historyChanged . agregar (  saveState  ) ;

			}  ) ;

			//

			documento . addEventListener (  'dragover' ,  function  (  event  )  {

				evento . preventDefault ( ) ;
				evento . transferencia de datos . dropEffect  =  'copiar' ;

			} ,  falso  ) ;

			documento . addEventListener (  'drop' ,  function  (  event  )  {

				evento . preventDefault ( ) ;

				if  (  event . dataTransfer . types [  0  ]  ===  'text / plain'  )  return ;  // caída del Outliner

				if  (  evento . transferencia de datos . artículos  )  {

					// DataTransferItemList admite carpetas

					editor . cargador . loadItemList (  evento . transferencia de datos . elementos  ) ;

				}  más  {

					editor . cargador . loadFiles (  evento . dataTransfer . archivos  ) ;

				}

			} ,  falso  ) ;

			function  onWindowResize ( )  {

				editor . señales . windowResize . despacho ( ) ;

			}

			ventana . addEventListener (  ' resize ' ,  onWindowResize ,  false  ) ;

			onWindowResize ( ) ;

			//

			var  isLoadingFromHash  =  false ;
			var  hash  =  ventana . ubicación . picadillo ;

			if  (  hash . substr (  1 ,  5  )  ===  'archivo ='  )  {

				var  file  =  hash . substr (  6  ) ;

				if  (  confirmar (  'Se perderán los datos no guardados. ¿Estás seguro?'  )  )  {

					 cargador de  var =  nuevo  TRES . FileLoader ( ) ;
					cargador . crossOrigin  =  '' ;
					cargador . carga (  archivo ,  función  (  texto  )  {

						editor . claro ( ) ;
						editor . fromJSON (  JSON . parse (  texto  )  ) ;

					}  ) ;

					isLoadingFromHash  =  true ;

				}

			}

			// Trabajador del servicio

			if  (  'serviceWorker'  en el  navegador  )  {

				prueba  {

					Navigator . serviceWorker . registrarse (  'sw.js'  ) ;

				}  captura  (  error  )  {

				}

			}

			/ *
			window.addEventListener ('mensaje', función (evento) {
				editor.clear ();
				editor.fromJSON (event.data);
			}, falso);
			* /

		</ script >
	</ cuerpo >
</ html >
