<properties
	pageTitle="Crear una máquina virtual con Windows en el portal clásico | Microsoft Azure"
	description="Cree una máquina virtual de Windows en el Portal de Azure clásico."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/12/2016"
	ms.author="cynthn"/>

# Creación de una máquina virtual que ejecuta Windows en el Portal de Azure clásico

> [AZURE.SELECTOR]
- [Portal de Azure clásico](virtual-machines-windows-classic-tutorial.md)
- [PowerShell: implementación clásica](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] Obtenga información acerca de cómo [realizar estos pasos con el modelo de implementación de Resource Manager](virtual-machines-windows-hero-tutorial.md).

En este tutorial se muestra lo fácil que resulta crear una máquina virtual (VM) de Azure en el Portal de Azure clásico. Vamos a usar una imagen de Windows Server como ejemplo, pero es solo una de las muchas imágenes que Azure ofrece. Tenga en cuenta que las opciones de imagen dependen de su suscripción. Por ejemplo, las imágenes de escritorio de Windows pueden estar disponibles para los suscriptores de MSDN.

En esta sección se muestra cómo utilizar la opción **De la galería** del portal clásico de Azure para crear una máquina virtual. Esta opción proporciona más opciones de configuración que **Creación rápida**. Por ejemplo, si desea conectar una máquina virtual a una red virtual, necesitará usar la opción **De la galería**.

También puede crear máquinas virtuales con sus [propias imágenes](virtual-machines-windows-classic-createupload-vhd.md). Para obtener más información sobre este y otros métodos, consulte [Diferentes formas de crear una máquina virtual de Windows](virtual-machines-windows-creation-choices.md).



## Tutorial en vídeo

Se trata del tutorial en formato de vídeo.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>Creación de la máquina virtual

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## Pasos siguientes

- Iniciar sesión en la nueva máquina virtual. Para obtener instrucciones, consulte [Inicio de sesión en una máquina virtual Windows mediante el Portal de Azure clásico](virtual-machines-windows-classic-connect-logon.md).

- Acople un disco para almacenar los datos. Puede acoplar tanto discos vacíos como discos que contienen datos. Para obtener instrucciones, consulte [Conecte un disco de datos a una máquina virtual de Windows creada con el modelo de implementación clásica](virtual-machines-windows-classic-attach-disk.md).

<!---HONumber=AcomDC_0713_2016-->