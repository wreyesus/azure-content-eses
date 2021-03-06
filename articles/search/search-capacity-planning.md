<properties
	pageTitle="Escalado de niveles de recursos para cargas de trabajo de indexación y consulta en Búsqueda de Azure | Microsoft Azure"
	description="El planeamiento de la capacidad en Búsqueda de Azure se basa en combinaciones de recursos informáticos de partición y réplica, en las que el precio de cada recurso se factura en unidades de búsqueda facturables."
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="jhubbard"
	editor=""
    tags="azure-portal"/>

<tags
	ms.service="search"
	ms.devlang="NA"
	ms.workload="search"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.date="08/29/2016"
	ms.author="heidist"/>

# Escalado de niveles de recursos para cargas de trabajo de indexación y consulta en Búsqueda de Azure

Después de [elegir una SKU](search-sku-tier.md) y [aprovisionar un servicio de búsqueda](search-create-service-portal.md), el siguiente paso consiste en configurar, si lo desea, los recursos del servicio.

En Búsqueda de Azure, un servicio se asigna inicialmente a un nivel mínimo de recursos que consta de una partición y una réplica. En los niveles donde se admita, puede ajustar de forma incremental los recursos informáticos aumentando las particiones si necesita más almacenamiento y E/S, o bien las réplicas para mejorar el rendimiento y aumentar los volúmenes de las consultas. Un único servicio debe tener recursos suficientes para controlar todas las cargas de trabajo (indexación y consultas). No se pueden subdividir las cargas de trabajo entre varios servicios.

La configuración de escala está disponible cuando se aprovisiona un servicio facturable en un [nivel Básico](http://aka.ms/azuresearchbasic) o uno de los [niveles Estándar](search-limits-quotas-capacity.md). En todas las SKU facturables, la capacidad se adquiere en incrementos de *unidades de búsqueda* (SU), en las que cada partición y réplica cuentan como una SU. Si se mantiene por debajo de los límites máximos, se utilizarán menos SU y el importe de las facturas será proporcionalmente más bajo. Mientras el servicio esté aprovisionado, le seguiremos cobrando. Si va a estar un tiempo sin utilizar un servicio, la única forma de evitar que le cobremos será eliminando el servicio y creándolo de nuevo cuando lo necesite más adelante.

Para aumentar o cambiar la asignación de réplicas y particiones, se recomienda usar el Portal. El Portal aplicará límites a las combinaciones permitidas que se mantengan por debajo de los límites máximos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/) y seleccione su servicio de búsqueda.
2. En Configuración, abra la hoja Escala y utilice los controles deslizantes para aumentar o disminuir el número de particiones y réplicas.

Como norma general, las aplicaciones de búsqueda necesitan más réplicas que particiones, sobre todo cuando las operaciones de servicio están orientadas a las cargas de trabajo de consulta. En la siguiente sección sobre [alta disponibilidad](#HA) se explica el motivo.

> [AZURE.NOTE] Una vez que se aprovisiona un servicio, no se puede actualizar en contexto a una SKU superior. Tendrá que crear un nuevo servicio de búsqueda en el nivel nuevo y volver a cargar los índices. Consulte [Creación de un servicio Búsqueda de Azure mediante el Portal de Azure](search-create-service-portal.md) para obtener ayuda con el proceso de aprovisionamiento de servicios.

## Terminología: particiones y réplicas

Las particiones y réplicas son los principales recursos que respaldan a un servicio de búsqueda.

Las **particiones** proporcionan almacenamiento de índices y E/S para realizar operaciones de lectura y escritura (por ejemplo, volver a generar o actualizar un índice).

Las **réplicas** son instancias del servicio de búsqueda y se utilizan principalmente para equilibrar la carga de las operaciones de consulta. Cada réplica siempre hospeda una copia de un índice. Si hay 12 réplicas, tendrá 12 copias de todos los índices cargados en el servicio.

> [AZURE.NOTE] No existe ninguna manera de manipular o administrar directamente los índices que se ejecutan en una réplica. Una copia de cada índice de cada réplica forma parte de la arquitectura del servicio.

<a id="HA"></a>
## Alta disponibilidad

Como es sencillo y relativamente rápido escalar verticalmente, normalmente se recomienda que comience con una partición y una o dos réplicas, y que después escale verticalmente conforme se crean volúmenes de consulta hasta llegar a la cantidad máxima de réplicas y particiones que admita la SKU. Una partición proporciona suficiente almacenamiento y E/S (15 millones de documentos por partición) para aprovisionar muchos servicios en los niveles Básico y S1.

Las cargas de trabajo de consulta se ejecutan principalmente en réplicas. Es probable que necesite más réplicas si requiere más rendimiento o alta disponibilidad.

Las recomendaciones generales para alta disponibilidad son:

- 2 réplicas para alta disponibilidad de cargas de trabajo de solo lectura (consultas)
- Tres o más réplicas para lograr una alta disponibilidad en las cargas de trabajo de lectura y escritura (se agregan, actualizan o eliminan consultas e indexación como documentos individuales).

Los Acuerdos de Nivel de Servicio (SLA) de Búsqueda de Azure están destinados a las operaciones de consulta y actualizaciones de índices que constan de procesos de incorporación, actualización o eliminación de documentos.

**Disponibilidad de los índices durante un proceso de regeneración**

La alta disponibilidad para Búsqueda de Azure se refiere a las consultas y actualizaciones de índices que no requieren volver a generar un índice. Si hay que volver a crear el índice —por ejemplo, si agrega o elimina un campo, modifica un tipo de datos o le cambia el nombre a un campo—, tendrá que eliminar el índice, crearlo de nuevo y volver a cargar los datos.

Para mantener la disponibilidad del índice durante una regeneración, debe contar con una segunda copia del índice que ya esté en la fase de producción del mismo servicio (con otro nombre) o un índice con el mismo nombre en un servicio diferente. Luego, tendrá que proporcionar la lógica de conmutación por error o redireccionamiento en el código.

## Recuperación ante desastres

En la actualidad no hay ningún mecanismo integrado para la recuperación ante desastres. La adición de particiones o réplicas sería la estrategia equivocada para cumplir los objetivos de recuperación ante desastres. El enfoque más común es agregar redundancia en el nivel de servicio mediante el aprovisionamiento de un segundo servicio de búsqueda en otra región. Al igual que con la disponibilidad durante la regeneración de índices, la lógica de conmutación por error o redireccionamiento debe proporcionarse en el código.

## Aumento del rendimiento de las consultas con réplicas

La latencia de consultas es un indicador de que se necesitan más réplicas. Por lo general, el primer paso para mejorar el rendimiento de las consultas consiste en agregar más réplicas. Conforme agrega réplicas, se ponen en línea copias adicionales del índice para admitir mayores cargas de trabajo de consultas y equilibrar la carga de las solicitudes por las diversas réplicas.

Tenga en cuenta que no ofrecemos estimaciones finales sobre consultas por segundo (QPS); el rendimiento de las consultas depende de la complejidad de la consulta y las cargas de trabajo competitivas. De media, una réplica de las SKU de los niveles Básico o S1 puede dar servicio a unas 15 QPS, pero el rendimiento será un algo mayor o menor en función de la complejidad de la consulta (las consultas por facetas son más complejas) y la latencia de red. Además, es importante reconocer que, aunque la adición de réplicas agregará definitivamente escala y rendimiento, el resultado final no será estrictamente lineal: la adición de 3 réplicas no garantiza el triple rendimiento.

Para obtener información sobre las QPS, incluidos los métodos para estimar el valor de QPS en las cargas de trabajo, consulte [Administración del servicio de búsqueda en Microsoft Azure](search-manage.md).

## Aumento del rendimiento de la indexación con particiones

Las aplicaciones de búsqueda que requieren una actualización de datos casi en tiempo real necesitan una proporción mayor de particiones que de réplicas. Al agregarse particiones, se distribuyen las operaciones de lectura y escritura entre un número mayor de recursos de proceso. Además, se cuenta con más espacio en disco para almacenar documentos e índices adicionales.

Las consultas en índices de mayor tamaño tardan más tiempo en realizarse. Por lo tanto, es posible que con cada aumento incremental de las particiones sea necesario también un aumento menor, pero proporcional, de las réplicas. Como se indicó antes, la complejidad y el volumen de las consultas afectarán a la rapidez con que se ejecuta la consulta.

## Nivel básico: combinaciones de particiones y réplicas

Un servicio del nivel Básico puede tener exactamente 1 partición y hasta 3 réplicas para un límite máximo de 3 SU. El único recurso que puede ajustarse son las réplicas. Tal y como se indicó anteriormente, se necesita un mínimo de 2 réplicas para lograr una alta disponibilidad en las consultas.

<a id="chart"></a>
## Niveles Estándar: combinaciones de particiones y réplicas

En esta tabla se muestran las unidades de búsqueda necesarias para admitir combinaciones de réplicas y particiones con el límite de 36 unidades de búsqueda (SU); se excluyen los niveles Básico y S3 HD.

- |- |- |- |- |- |- |
---|----|---|---|---|---|---|
**12 réplicas**|12 unidades de búsqueda|24 unidades de búsqueda|36 unidades de búsqueda|N/D|N/D|N/D|
**6 réplicas**|6 unidades de búsqueda|12 unidades de búsqueda|18 unidades de búsqueda|24 unidades de búsqueda|36 unidades de búsqueda|N/D|
**5 réplicas**|5 unidades de búsqueda|10 unidades de búsqueda|15 unidades de búsqueda|20 unidades de búsqueda|30 unidades de búsqueda|N/D|
**4 réplicas**|4 unidades de búsqueda|8 SU<|12 unidades de búsqueda|16 unidades de búsqueda|24 unidades de búsqueda|N/|
**3 réplicas**|3 unidades de búsqueda|6 unidades de búsqueda|9 unidades de búsqueda|12 unidades de búsqueda|18 unidades de búsqueda|36 unidades de búsqueda|
**2 réplicas**|2 unidades de búsqueda|4 unidades de búsqueda|6 unidades de búsqueda|8 unidades de búsqueda|12 unidades de búsqueda|24 unidades de búsqueda|
**1 réplica**|1 unidad de búsqueda|2 unidades de búsqueda|3 unidades de búsqueda|4 unidades de búsqueda|6 unidades de búsqueda|12 unidades de búsqueda|
N/D|**1 partición**|**2 particiones**|**3 particiones**<|**4 particiones**|**6 particiones**|**12 particiones**|

En el sitio web de Azure se explican con detalle la capacidad, los precios y las unidades de búsqueda. Para más información, consulte [Detalles de precios](https://azure.microsoft.com/pricing/details/search/).

> [AZURE.NOTE] El número de réplicas y particiones debe dividirse en 12 uniformemente (de manera específica, 1, 2, 3, 4, 6, 12). Esto se debe a que Búsqueda de Azure divide previamente cada índice en 12 particiones para que se pueda repartir en porciones iguales entre todas las particiones. Por ejemplo, si su servicio tiene tres particiones y crea un nuevo índice, cada partición contendrá 4 particiones del índice. La manera en que la Búsqueda de Azure particiona un índice es un detalle de implementación, sujeto a cambios en la futura versión. Aunque el número es 12 hoy, no debe esperar que ese número se siempre 12 en el futuro.

## Nivel S3 Alta densidad: combinaciones de particiones y réplicas

Este nivel tiene 1 partición y hasta 12 réplicas para un límite máximo de 12 SU. El único recurso que puede ajustarse son las réplicas.

## Cálculo de las unidades de búsqueda para combinaciones de recursos específicas: R X P = SU

La fórmula para calcular cuántas SU se necesitan es réplicas multiplicadas por particiones. Por ejemplo, 3 réplicas multiplicadas por 3 particiones se factura como 9 unidades de búsqueda.

Ambos niveles comienzan con una réplica y una partición, que cuentan como una unidad de búsqueda (SU). Este es el único caso en que una réplica y una partición se cuentan como una unidad de búsqueda. Cada recurso adicional, ya sea una réplica o una partición, se cuenta como una SU independiente.

El nivel determina el costo por SU. El costo por SU es menor para el nivel básico que para el nivel Estándar. Las tarifas de cada nivel pueden consultarse en [Buscar Precios](https://azure.microsoft.com/pricing/details/search/).

<!---HONumber=AcomDC_0914_2016-->