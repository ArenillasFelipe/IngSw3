# Trabajo Practico 9 - Pruebas Unitarias

- Clonacion de repositorio

![1](clonacion_repo.png)

- Revision de codigo

![2](revision_codigo.png)

- Creacion de simpleapptests

![3](creacion_simpleapptests.png)

- Agregamos paquetes

![4](add_package_nunit.png)

![5](add_package_nunit2.png)

![6](add_reference.png)

- Instalacion de extension .NET Core Test Explorer

![7](dotnet_core_install.png)

- Edicion de settings.json

![8](settings_json.png)

- Run de tests

![9](test1.png)

## Creacion de nuestros tests

- Edicion de UnitTest1.cs

- Run de los nuevos tests

![10](run_nuevos_tests.png)

- Edicion del codigo bajo prueba para que falle

- Comprobacion de la falla

![11](fail_test.png)

- Refactorizacion del codigo

![12](refactorizacion_codigo.png)

- Dotnet test

![13](dotnet_test_consola.png)

## Desarrollo de pruebas unitarias sobre una WebAPI

- Creacion de proyecto de pruebas

![14](simple_web_api_tests.png)

- Add Reference

![15](add_reference2.png)

- Edicion de settings.json

- Reemplazo de UnitTest1.cs

![16](weatherforecast_test.png)

- Resultados de los tests

![17](weatherforecast_test_run.png)

## Tests con mocks

- Resultados de los tests con mock

Primero, en la sección Arrange, se configura el entorno de la prueba. Se crea un contenedor de servicios (ServiceCollection) y se configura un objeto HttpResponseMessage simulando una respuesta exitosa del servidor HTTP con un JSON de modelo de datos ficticio.

Luego, se utiliza Moq para crear un manejador de mensajes HTTP simulado (mockHttpMessageHandler). Se configura para que, cuando se llame al método SendAsync con cualquier HttpRequestMessage y CancellationToken, devuelva la respuesta simulada.

Después, se registra una implementación de la interfaz IApiService en el contenedor de servicios. La implementación concreta de IApiService es ApiService, que se instancia con un objeto HttpClient que utiliza el mockHttpMessageHandler creado anteriormente.

En la sección Act, se resuelve la implementación de IApiService del contenedor de servicios y se llama al método GetMyModelsAsync. Esto activa el código de ApiService que utiliza el HttpClient mockeado.

Finalmente, en la sección Assert, se verifican los resultados de la llamada al método. Se comprueba que el resultado no sea nulo, que haya un solo elemento en la colección resultante y que el título del primer elemento coincida con el esperado.

- test ejecutado

![18](not_so_simple_test_run.png)