<properties
	pageTitle="Copia de datos de Almacenamiento de blobs en Base de datos SQL | Microsoft Azure"
	description="En este tutorial se muestra cómo usar la actividad de copia en una canalización de Data Factory de Azure para copiar datos de Almacenamiento de blobs en Base de datos SQL de Azure."
	Keywords="blob SQL, Almacenamiento de blobs, copia de datos"
	services="data-factory"
	documentationCenter=""
	authors="spelluru"
	manager="jhubbard"
	editor="monicar"/>

<tags
	ms.service="data-factory"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article" 
	ms.date="08/01/2016"
	ms.author="spelluru"/>

# Copia de datos de Almacenamiento de blobs en Base de datos SQL mediante Data Factory 
> [AZURE.SELECTOR]
- [Introducción y requisitos previos](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Portal de Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [API DE REST](data-factory-copy-activity-tutorial-using-rest-api.md)
- [API de .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Asistente para copia](data-factory-copy-data-wizard-tutorial.md)

En este tutorial, creará una factoría de datos con una canalización para copiar datos de Almacenamiento de blobs en Base de datos SQL.

La actividad de copia realiza el movimiento de datos en Data Factory de Azure. Funciona con un servicio disponible de forma global que puede copiar datos entre varios almacenes de datos de forma segura, confiable y escalable. Consulte el artículo [Actividades de movimiento de datos](data-factory-data-movement-activities.md) para obtener más información sobre la actividad de copia.

> [AZURE.NOTE] Para información general más detallada sobre el servicio Data Factory, consulte el artículo [Introducción al servicio Data Factory de Azure][data-factory-introduction].

##Requisitos previos para el tutorial
Antes de empezar este tutorial, debe contar con lo siguiente:

- **Suscripción de Azure**. Si no tiene una suscripción, puede crear una cuenta de prueba gratuita en tan solo un par de minutos. Consulte el artículo [Evaluación gratuita][azure-free-trial] para obtener información.
- **Cuenta de almacenamiento de Azure**. Almacenamiento de blobs se usará como un almacén de datos de **origen** en este tutorial. Si no tiene una cuenta de Almacenamiento de Azure, consulte la sección [Crear una cuenta de almacenamiento][data-factory-create-storage] para ver los pasos para su creación.
- **Base de datos de SQL Azure**. Usará una base de datos SQL de Azure como un almacén de datos de **destino** en este tutorial. Si no dispone de una base de datos SQL de Azure que pueda usar en el tutorial, vea [Cómo crear y configurar una base de datos de SQL de Azure][data-factory-create-sql-database] para crear una.
- **SQL Server 2012/2014 o Visual Studio 2013**. Usará SQL Server Management Studio o Visual Studio para crear una base de datos de ejemplo y ver los datos de resultados de la base de datos.

## Obtención del nombre y la clave de la cuenta de Almacenamiento de blobs 
Para realizar este tutorial necesitará el nombre de cuenta y la clave de cuenta de su cuenta de Almacenamiento de Azure. Anote el **nombre de cuenta** y la **clave de cuenta** para su cuenta de almacenamiento de Azure.

1. Inicie sesión en el [Portal de Azure][azure-portal].
2. Haga clic en el centro **EXAMINAR** de la izquierda y seleccione **Cuentas de almacenamiento**.
3. En la hoja **Cuentas de almacenamiento**, seleccione la **cuenta de almacenamiento de Azure** que desea usar en este tutorial.
4. Seleccione **Claves de acceso** en **CONFIGURACIÓN**.
5.  Haga clic en el botón **copiar** (imagen) que se encuentra junto al cuadro de texto **Nombre de cuenta de almacenamiento** y guárdelo y péguelo en algún lugar (por ejemplo: en un archivo de texto).
6. Repita el paso anterior para copiar o anotar la **clave 1**.
7. Haga clic en **X** para cerrar todas las hojas.

## Recopilación de nombres de usuario, bases de datos y servidores SQL Server
Necesitará los nombres de servidor SQL de Azure, la base de datos y el usuario para realizar este tutorial. Anote los nombres del **servidor**, la **base de datos** y el **usuario** de su base de datos SQL de Azure.

1. En el **Portal de Azure**, haga clic en **EXAMINAR** a la izquierda y seleccione **Bases de datos SQL**.
2. En la **hoja de bases de datos SQL**, seleccione la **base de datos** que desea usar en este tutorial. Anote el **nombre de la base de datos**.
3. En la hoja **BASE DE DATOS SQL**, haga clic en la hoja **PROPIEDADES**.
4. Anote los valores de **NOMBRE DEL SERVIDOR** e **INICIO DE SESIÓN DE ADMINISTRADOR DE SERVIDOR**.
5. Haga clic en **X** para cerrar todas las hojas.

## Procedimiento para permitir que los servicios de Azure accedan a SQL Server 
Asegúrese de que la configuración **Permitir el acceso a los servicios de Azure** está **ACTIVADA** para el servidor de SQL de Azure para que el servicio de la factoría de datos pueda acceder al servidor de SQL de Azure. Para comprobar y activar esta configuración, realice los siguientes pasos:

1. Haga clic en el concentrador **EXAMINAR** a la izquierda y haga clic en **Servidores SQL**.
2. Seleccione **su servidor** y haga clic en **CONFIGURACIÓN** en la hoja **SQL SERVER**.
3. En la hoja **CONFIGURACIÓN**, haga clic en **Firewall**.
4. En la hoja **Configuración de firewall**, haga clic en **ACTIVADA** para **Permitir el acceso a los servicios de Azure**.
5. Haga clic en **X** para cerrar todas las hojas.

## Preparación del Almacenamiento de blobs y Base de datos SQL 
Ahora, prepare su almacenamiento de blobs de Azure y base de datos SQL de Azure para el tutorial realizando los pasos siguientes:

1. Inicie el Bloc de notas, pegue el texto siguiente y guárdelo como **emp.txt** en la carpeta **C:\\ADFGetStarted** del disco duro.

        John, Doe
		Jane, Doe

2. Use herramientas como el [Explorador de almacenamiento de Azure](https://azurestorageexplorer.codeplex.com/) para crear el contenedor **adftutorial** y cargar el archivo **emp.txt** en el contenedor.

    ![Explorador de almacenamiento de Azure Copia de datos de Almacenamiento de blobs en Base de datos SQL](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Use el siguiente script de SQL para crear la tabla **emp** en su Base de datos SQL de Azure.


        CREATE TABLE dbo.emp
		(
			ID int IDENTITY(1,1) NOT NULL,
			FirstName varchar(50),
			LastName varchar(50),
		)
		GO

		CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

	**Si tiene SQL Server 2012/2014 instalado en el equipo:** siga las instrucciones del artículo [Paso 2: Conectar con la base de datos SQL de la base de datos de administración de SQL de Azure con SQL Server Management Studio][sql-management-studio] para conectarse al servidor SQL de Azure y ejecutar el script de SQL. En este artículo se usa el [Portal de Azure clásico](http://manage.windowsazure.com) en lugar de [Azure Portal](https://portal.azure.com) para configurar el firewall para un servidor SQL de Azure.

	Si el cliente no tiene permiso para acceder al servidor SQL de Azure, tendrá que configurar el firewall de su servidor SQL de Azure para permitir el acceso desde su máquina (dirección IP). Consulte [este artículo](../sql-database/sql-database-configure-firewall-settings.md) para conocer los pasos que deben darse para configurar el firewall de un servidor SQL de Azure.

Ha completado los requisitos previos. Haga clic en una pestaña situada en la parte superior para realizar el tutorial con una de estas opciones:

- Portal de Azure
- Visual Studio
- PowerShell
- API de REST
- API de .NET
- Asistente para copia

<!--Link references-->
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[sql-management-studio]: http://azure.microsoft.com/documentation/articles/sql-database-manage-azure-ssms/#Step2

[data-factory-introduction]: data-factory-introduction.md
[data-factory-create-storage]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account
[data-factory-create-sql-database]: ../sql-database/sql-database-get-started.md

<!---HONumber=AcomDC_0921_2016-->