<properties
	pageTitle="Escalar automáticamente un servicio en la nube en el portal | Microsoft Azure"
	description="(clásico) Obtenga información sobre cómo usar el portal clásico para configurar reglas de escalado automático de un rol de trabajo o un rol web de servicio en la nube en Azure."
	services="cloud-services"
	documentationCenter=""
	authors="Thraka"
	manager="timlt"
	editor=""/>

<tags
	ms.service="cloud-services"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/06/2016"
	ms.author="adegeo"/>


# Cómo escalar automáticamente un servicio en la nube

> [AZURE.SELECTOR]
- [Portal de Azure](cloud-services-how-to-scale-portal.md)
- [Portal de Azure clásico](cloud-services-how-to-scale.md)

En la página Escalar del Portal de Azure clásico, puede escalar manualmente su rol web o rol de trabajo, o puede habilitar el escalado automático basado en la carga de la CPU o en una cola de mensajes.

>[AZURE.NOTE] Este artículo se centra en los roles de trabajo y en los roles web de Servicio en la nube. Al crear una máquina virtual (clásica) directamente, esta se hospeda en un servicio en la nube. Parte de esta información se aplica a estos tipos de máquinas virtuales. Escalar un conjunto de disponibilidad de máquinas virtuales consiste simplemente en encenderlas y apagarlas basándose en las reglas de escalado que configure. Para más información sobre las máquinas virtuales y los conjuntos de disponibilidad, consulte [Administración de la disponibilidad de las máquinas virtuales](../virtual-machines/virtual-machines-windows-classic-configure-availability.md).

Debe considerar la siguiente información antes de configurar el escalado para su aplicación:

- El uso de núcleos afecta el escalado. Las instancias de rol más grandes usan más núcleos. Solo puede escalar una aplicación dentro del límite de núcleos para su suscripción. Por ejemplo, si su suscripción tiene un límite de veinte núcleos y ejecuta una aplicación con dos servicios en la nube de tamaño mediano (un total de cuatro núcleos), solo puede escalar verticalmente otras implementaciones del servicio en la nube en su suscripción con dieciséis núcleos. Consulte [Tamaños de los servicios en la nube](cloud-services-sizes-specs.md) para más información sobre los tamaños.

- Debe crear una cola y asociarla con un rol para poder escalar una aplicación según el umbral de un mensaje. Para obtener más información, consulte [Uso del servicio de almacenamiento de colas](../storage/storage-dotnet-how-to-use-queues.md).

- Puede escalar los recursos que están vinculados con su servicio en la nube. Para obtener más información acerca de los recursos de vinculación, consulte [Vinculación de un recurso a un servicio en la nube](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Para permitir una alta disponibilidad de la aplicación, debe asegurarse de que se implemente con dos o más instancias de rol. Para obtener más información, consulte [Contratos de nivel de servicio](https://azure.microsoft.com/support/legal/sla/).



## Programar escalado

De forma predeterminada, todos los roles no siguen una programación específica. Por lo tanto, cualquier configuración modificada se aplica a cualquier hora y día durante todo el año. Si quiere, puede configurar la escala automática o manual para:

- Días laborables
- Fines de semana
- Noches entre semana
- Mañanas entre semana
- Fechas específicas
- Intervalos de fechas específicas

Esto se configura en el [Portal de Azure clásico](https://manage.windowsazure.com/) en la página **Servicios en la nube** > **[Su servicio en la nube]** > **Escala** > **[Producción o almacenamiento provisional]**.

Haga clic en el botón **Configurar horas de programación** para cada rol que quiera cambiar.

![Escalado automático en un servicio en la nube basado en una programación][scale_schedules]



## Escala manual

En la página **Escala**, puede aumentar o disminuir manualmente la cantidad de instancias en ejecución en un servicio en la nube. Esto se configura para cada programación que haya creado o para todas si no ha creado una programación.

1. En el [Portal de Azure clásico](https://manage.windowsazure.com/), haga clic en **Servicios en la nube** y, a continuación, haga clic en el nombre del servicio en la nube para abrir el panel.

    > [AZURE.TIP] Si no ve el servicio en la nube, debe cambiar de **Producción** a **Almacenamiento provisional** o viceversa.

2. Haga clic en **Escalar**.

3. Seleccione la programación en la que quiere cambiar las opciones de escalado. Mantenga el valor predeterminado en *Sin horas programadas* si no dispone de ninguna programación definida.

4. Busque la sección **Escalar por métrica** y seleccione **NINGUNA**. Esta es la configuración predeterminada de todos los roles.

5. Cada rol en el servicio en la nube tiene un control deslizante para cambiar la cantidad de instancias que se van a usar.

    ![Escalar manualmente un rol de servicio en la nube][manual_scale]

    Si necesita más instancias, debe cambiar el [tamaño de la máquina virtual del servicio en la nube](cloud-services-sizes-specs.md).

6. Haga clic en **Save**. Las instancias de rol se agregaran o quitarán según las selecciones realizadas.

>[AZURE.TIP] Siempre que vea ![][tip_icon], mueva el mouse y podrá obtener ayuda acerca de lo que realiza una configuración específica.


## Escala automática: CPU

Esto escala si el porcentaje medio de uso de la CPU está por encima o por debajo de los umbrales especificados; las instancias de rol se crean o eliminan.

1. En el [Portal de Azure clásico](https://manage.windowsazure.com/), haga clic en **Servicios en la nube** y, a continuación, haga clic en el nombre del servicio en la nube para abrir el panel.

    > [AZURE.TIP] Si no ve el servicio en la nube, debe cambiar de **Producción** a **Almacenamiento provisional** o viceversa.

2. Haga clic en **Escalar**.

3. Seleccione la programación en la que quiere cambiar las opciones de escalado. Mantenga el valor predeterminado en *Sin horas programadas* si no dispone de ninguna programación definida.

4. Busque la sección **Escalar por métrica** y seleccione **CPU**.

5. Ahora puede configurar un intervalo máximo y mínimo de instancias de rol, el uso de la CPU de destino (para desencadenar una escalada vertical) y el número de instancias para escalar y reducir verticalmente.

![Escalar un rol de servicio en la nube con la carga de la CPU][cpu_scale]

>[AZURE.TIP] Siempre que vea ![][tip_icon], mueva el mouse y podrá obtener ayuda acerca de lo que realiza una configuración específica.





## Escala automática: Cola

Esto escala automáticamente si el número de mensajes en una cola está por encima o por debajo de un umbral especificado; las instancias de rol se crean o se eliminan.

1. En el [Portal de Azure clásico](https://manage.windowsazure.com/), haga clic en **Servicios en la nube** y, a continuación, haga clic en el nombre del servicio en la nube para abrir el panel.

    > [AZURE.TIP] Si no ve el servicio en la nube, debe cambiar de **Producción** a **Almacenamiento provisional** o viceversa.

2. Haga clic en **Escalar**.

3. Busque la sección **Escalar por métrica** y seleccione **CPU**.

4. Ahora puede configurar un intervalo máximo y mínimo de instancias de rol, la cola y la cantidad de mensajes de cola para procesar por cada instancia, y el número de instancias para escalar y reducir verticalmente.

![Escalar un rol de servicio en la nube con una cola de mensajes][queue_scale]

>[AZURE.TIP] Siempre que vea ![][tip_icon], mueva el mouse y podrá obtener ayuda acerca de lo que realiza una configuración específica.


## Escalado de recursos vinculados

A menudo, cuando se escala un rol, es beneficioso también escalar la base de datos que la aplicación está usando. Si vincula la base de datos al servicio en la nube, puede acceder a la configuración de escalado de ese recurso haciendo clic en el vínculo apropiado.

1. En el [Portal de Azure clásico](https://manage.windowsazure.com/), haga clic en **Servicios en la nube** y, a continuación, haga clic en el nombre del servicio en la nube para abrir el panel.

    > [AZURE.TIP] Si no ve el servicio en la nube, debe cambiar de **Producción** a **Almacenamiento provisional** o viceversa.

2. Haga clic en **Escalar**.

3. Busque la sección **recursos vinculados** y haga clic en **Administrar el escalado de esta base de datos**.

    > [AZURE.NOTE] Si no ve la sección **recursos vinculados**, probablemente no tenga ningún recurso vinculado.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png

<!---HONumber=AcomDC_0914_2016-->