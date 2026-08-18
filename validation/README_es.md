
<!-- TOC -->

# Validación del flujo federado
> Última revisión: 2026-08-14

Para realizar la validación del flujo utilizamos una API simple con un interceptor de mock y logs para, en caso necesario, realizar un análisis.
Para ello, es necesario haber concluido todo el aprovisionamiento y configuración del Enviroment federado.


* Acceder al menú API Design > API Catalog
* Hacer clic en **+**  para crear una nueva API

![Create Api](../images/create_api.jpg)

*  Complete los campos según el ejemplo:
   * API Name
   * Api version 
   * Base path
   * Seleccione el Enviroment federado
   * Hacer clic en **SAVE AND NEXT**
 
![Create Api1](../images/create_api1.jpg)

*  Añadir un Resource:

![Create Api2](../images/create_api2.jpg)

*  Complete los campos según el ejemplo:
   * Resource name
   * Hacer clic en **SAVE** 


![Create Api3](../images/create_api3.jpg)

*  Complete los campos según el ejemplo:
   * Elige el método GET
   * Establecer el Path
   * Hacer clic en **SAVE OPERATION** 
   * Hacer clic en **SAVE AND NEXT**



![Create Api4](../images/create_api4.jpg)


*  Complete los campos según el ejemplo:
   *  Seleccione el Resource creado y la opción GET
   *  Agregar un interceptor de mock


![Create Api5](../images/create_api5.jpg)

*  Complete los campos según el ejemplo:
   * Agregar en el body el ejemplo de abajo
   * Establecer el estado **200** 

```bash
{
    "firstName": "Paulo",
    "lastName": "Silva",
    "age": 25,
    "address":
    {
        "streetAddress": "Avenida das Palmeiras",
        "city": "São Paulo",
        "state": "SP",
        "postalCode": "1024"
    },
    "phoneNumber":
    [
        {
          "type": "Fixo",
          "number": "11 0000-1111"
        },
        {
          "type": "Celular",
          "number": "11 1.1111-0000"
        }
    ]
}
```

* Acceder a la pestaña de intercepciones **Tracing**
   * Agregue dos interceptores de Log, uno antes y otro después del Mock.
   * Hacer clic en **SAVE AND NEXT**
     
![Create Api6](../images/create_api6.jpg)


* Realizar el Deploy de la API
  
![Create Api7](../images/create_api7.jpg)

* Acceder al menú API Design > API Catalog
   * Abrir la API creada
   * En Environments, haga clic en el ícono según la flecha y copie el valor
   * Verifique el valor del Path
     
![Create Api8](../images/create_api8.jpg)

* Para realizar la validación, acceda a una terminal o a una herramienta de peticiones
y ejecute un **curl** como en el ejemplo


```bash
curl -X GET https://api-support-aws.sensedia.com/mock-hybrid/mock
```

![Create Api9](../images/create_api9.jpg)



