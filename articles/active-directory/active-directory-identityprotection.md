<properties
	pageTitle="Azure Active Directory Identity Protection | Microsoft Azure"
	description="Aprenda cómo Azure AD Identity Protection permite limitar la capacidad de un atacante para aprovechar una identidad o un dispositivo en peligro y asegurar una identidad o un dispositivo que antes fue sospechoso o que se sabe que estuvo en peligro."
	services="active-directory"
	keywords="azure active directory identity protection, detección de aplicaciones en la nube, administración de aplicaciones, seguridad, riesgo, nivel de riesgo, punto vulnerable, directiva de seguridad"
	documentationCenter=""
	authors="markusvi"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/20/2016"
	ms.author="markvi"/>

#Azure Active Directory Identity Protection 

Azure Active Directory Identity Protection es una característica de la edición Premium P2 de Azure AD que proporciona una vista consolidada de los eventos de riesgo y las vulnerabilidades potenciales que afectan a las identidades de su organización. Microsoft ha estado protegiendo identidades basadas en la nube durante más de una década, y con Azure AD Identity Protection, Microsoft proporciona estos mismos sistemas de protección a los clientes de empresa. Identity Protection aprovecha las funciones de detección de anomalías existentes de Azure AD (disponibles mediante informes de actividad anómala de Azure AD) e introduce nuevos tipos de eventos de riesgo que pueden detectar anomalías en tiempo real.



##Introducción

La mayoría de las infracciones de seguridad tienen lugar cuando los atacantes obtienen acceso a un entorno mediante el robo de identidad de un usuario. Los atacantes son cada vez más eficaces al aprovechar las brechas de otros fabricantes y el uso de ataques de suplantación de identidad sofisticados. Una vez que un atacante obtiene acceso aunque sea a una cuenta de usuario con pocos privilegios, es relativamente sencillo para obtener acceso a importantes recursos de la empresa mediante el desplazamiento lateral. Por lo tanto, es esencial para proteger todas las identidades y, cuando una identidad está en peligro, evitar de forma proactiva que se haga mal uso de la identidad que está en peligro.

Descubrir las identidades en peligro no es tarea fácil. Afortunadamente, Identity Protection puede ayudar: Identity Protection utiliza algoritmos de aprendizaje automático adaptables y heurísticos para detectar anomalías e eventos de riesgo que pueden indicar que se ha puesto en peligro una identidad.
 
Con estos datos, Identity Protection genera informes y alertas que permiten investigar estos eventos de riesgo y tomar las acciones de corrección o mitigación adecuadas.
 
Pero Azure Active Directory Identity Protection es más de una herramienta de supervisión e informes. En función de los eventos de riesgo, Identity Protection calcula un nivel de riesgo del usuario para cada usuario, lo que le permite configurar las directivas basadas en riesgos para proteger automáticamente las identidades de su organización. Estas directivas basadas en riesgos, además de otros controles de acceso condicional proporcionados por Azure Active Directory y EMS, pueden bloquear automáticamente u ofrecer acciones de corrección adaptables que incluyen el restablecimiento de contraseñas y el cumplimiento de la autenticación multifactor.

####Exploración de las funciones de Identity Protection 

**Detección de eventos de riesgo y cuentas peligrosas:**

- Detectar 6 tipos de eventos de riesgo mediante el aprendizaje automático y las reglas heurísticas

- Calcular los niveles de riesgo del usuario

- Proporcionar recomendaciones personalizadas para mejorar la posición de seguridad general al resaltar los puntos vulnerables



**Investigación de los eventos de riesgo:**

- Enviar notificaciones de los eventos de riesgo

- Investigar los eventos de riesgo con información pertinente y contextual

- Proporcionar los flujos de trabajo básicos para realizar un seguimiento de las investigaciones

- Proporcionar un fácil acceso a las acciones de corrección, como el restablecimiento de contraseñas



**Directivas de acceso condicional basadas en riesgos:**

- Directiva para mitigar inicios de sesión peligrosos al bloquear inicios de sesión o solicitar desafíos de la autenticación multifactor.

- Directiva para proteger o bloquear cuentas de usuario peligrosas

- Directiva para exigir que los usuarios se registren en la autenticación multifactor


## Detección y riesgo

### Eventos de riesgo

Los eventos de riesgo son episodios marcados como sospechosos por Identity Protection, e indican que es posible que se haya puesto en peligro una identidad. Para ver una lista completa de los eventos de riesgo, consulte [Tipos de eventos de riesgo que detecta Azure Active Directory Identity Protection](active-directory-identityprotection-risk-events-types.md).

Algunos de estos eventos de riesgo han estado disponibles mediante los informes de actividades anómalas de Azure AD en Azure Portal. En la tabla siguiente se enumeran los distintos tipos de eventos de riesgo y el correspondiente informe de **actividades anómalas de Azure AD**. Microsoft seguirá invirtiendo en este espacio y planea mejorar continuamente la precisión de la detección de eventos de riesgo existentes y agregar nuevos tipos de evento de riesgo de forma constante.



| Tipo de evento de riesgo de Identity Protection | Informe de actividades anómalas de Azure AD correspondiente |
| :-- | :-- |
| Credenciales con fugas | Usuarios con credenciales perdidas |
| Viaje imposible a ubicaciones inusuales |	Actividad de inicio de sesión irregular |
| Inicios de sesión desde dispositivos infectados | Inicios de sesión desde dispositivos posiblemente infectados |
| Inicios de sesión desde direcciones IP anónimas | Inicios de sesión desde orígenes desconocidos |
| Inicios de sesión desde direcciones IP con actividad sospechosa |	Inicios de sesión desde direcciones IP con actividad sospechosa |
| Inicios de sesión desde ubicaciones desconocidas | - | 
| Bloqueo de eventos | - |

Los siguientes informes de actividades anómalas de Azure AD no se incluyen como eventos de riesgo en Azure AD Identity Protection y, por tanto, no estarán disponibles mediante Identity Protection. Estos informes aún están disponibles en Azure Portal; sin embargo, dejarán de estar en uso en el futuro, ya que están siendo reemplazados por los eventos de riesgo de Identity Protection.

- Inicios de sesión tras varios errores
- Inicios de sesión desde varias ubicaciones geográficas

### Nivel de riesgo

El nivel de riesgo de un evento de riesgo es una indicación (Alto, Medio o Bajo) de la gravedad del evento de riesgo. El nivel de riesgo ayuda a los usuarios de Identity Protection a dar prioridad a las acciones que deben tomar para reducir el riesgo para su organización. La gravedad del evento de riesgo representa la fuerza de la señal como predicción del riesgo de la identidad, combinada con la cantidad de ruido que proporciona normalmente.

- **Alto**: evento de riesgo de gravedad alta y de alta confianza. Estos eventos son buenos indicadores de que se ha puesto en peligro la identidad del usuario; las cuentas de usuario afectadas deben corregirse inmediatamente.

- **Medio**: evento de riesgo de gravedad alta, pero confianza inferior, o viceversa. Estos eventos son potencialmente peligrosos y las cuentas de usuario afectadas deben corregirse.

- **Bajo**: evento de riesgo de baja confianza y gravedad baja. Este evento puede no requerir una acción inmediata, pero cuando se combina con otros eventos de riesgo, puede proporcionar una indicación clara de que la identidad está en peligro.


![Nivel de riesgo](./media/active-directory-identityprotection/01.png "Nivel de riesgo")

 

Los eventos de riesgo se identifican **en tiempo real**, o bien en el procesamiento posterior a que se haya producido el evento (sin conexión). Actualmente la mayoría de los eventos de riesgo en Identity Protection se calculan sin conexión y aparecen en Identity Protection en un plazo de 2 a 4 horas. Aunque se evalúan en tiempo real, los eventos de riesgo en tiempo real se mostrarán en la consola de Identity Protection en un plazo de 5 a 10 minutos.

Varios clientes heredados no admiten actualmente la detección y prevención de eventos de riesgo en tiempo real. Como resultado, los inicios de sesión desde estos clientes no se puede detectar ni prevenir en tiempo real.


## Investigación
Normalmente su viaje a través de Identity Protection empieza en el panel de Identity Protection.

![Corrección](./media/active-directory-identityprotection/1000.png "Corrección")

El panel proporciona acceso a:
 
- Informes como **Usuarios marcados en riesgo**, **Eventos de riesgo** y **Vulnerabilidades**.
- Opciones como la configuración de sus **directivas de seguridad**, **notificaciones** y el **registro de la autenticación multifactor**.
 

Normalmente es el punto de partida para la investigación, que es el proceso de revisión de las actividades, los registros y otra información pertinente relacionada con un evento de riesgo para decidir si son necesarios pasos de mitigación o de corrección, y cómo se ha puesto en peligro la identidad así como comprender cómo se utilizaba la identidad en peligro.

Puede vincular las actividades de investigación a las [notificaciones](active-directory-identityprotection-notifications.md) que Azure Active Directory Identity Protection envía por correo electrónico.

En las secciones siguientes se proporcionan más detalles y los pasos que están relacionados con una investigación.



## ¿Qué es un nivel de riesgo de usuario?

El nivel de riesgo de un usuario es una indicación (Alto, Medio o Bajo) de la probabilidad de que se haya puesto en peligro la identidad del usuario. Se calcula en función de los eventos de riesgo del usuario que están asociados a la identidad del usuario.

El estado de un evento de riesgo es **Activo** o **Cerrado**. Solo los eventos de riesgo con el estado **Activo** contribuyen al cálculo del riesgo del usuario.

El nivel de riesgo del usuario se calcula con las siguientes entradas:

- Eventos de riesgo activos que afectan al usuario
- Nivel de riesgo de estos eventos
- Si se ha tomado alguna acción de corrección

![Riesgos de usuario](./media/active-directory-identityprotection/1001.png "Riesgos de usuario")



Puede utilizar los niveles de riesgo del usuario para crear directivas de acceso condicionales para impedir que los usuarios peligrosos inicien sesión u obligarles a cambiar su contraseña de forma segura.


## Cierre manual de eventos de riesgo

En la mayoría de los casos, realizará las acciones de corrección, como restablecer una contraseña segura, para cerrar automáticamente los eventos de riesgo. Sin embargo, esto no siempre es posible. Por ejemplo, esto sucede cuando:

- Un usuario con eventos de riesgo en estado Activo se ha eliminado.
- Una investigación revela que el usuario legítimo ha realizado un evento de riesgo notificado.

Dado que los eventos de riesgo que tienen el estado **Activo** contribuyen al cálculo de riesgo del usuario, puede que tenga que reducir manualmente un nivel de riesgo al cerrar manualmente los eventos de riesgo. Durante el curso de la investigación, puede elegir realizar cualquiera de estas acciones para cambiar el estado de un evento de riesgo:

![Acciones](./media/active-directory-identityprotection/34.png "Acciones")

- **Resolver**: si, después de investigar un evento de riesgo, ha tomado una acción de corrección adecuada fuera de Identity Protection y cree que se debe considerar el evento de riesgo cerrado, marque el evento como resuelto. Los eventos resueltos establecerán el estado del evento de riesgo en Cerrado y ya no contribuirá al riesgo del usuario.

- **Marcar como falso positivo**: en algunos casos, puede investigar un evento de riesgo y detectar que se ha marcado incorrectamente como peligroso. Para ayudar a reducir el número de dichos casos, puede marcar el evento de riesgo como falso positivo. Esto ayudará a los algoritmos de aprendizaje automático a mejorar la clasificación de eventos similares en el futuro. El estado de los eventos con falsos positivos es **Cerrado** y ya no contribuyen al riesgo del usuario.

- **Omitir**: si no se ha tomado ninguna acción de corrección, pero desea que el evento de riesgo se quite de la lista activa, puede seleccionar Omitir para un evento de riesgo, cuyo estado será Cerrado. Los eventos omitidos no contribuyen al riesgo del usuario. Esta opción solo debe utilizarse en circunstancias inusuales.

- **Reactivar**: los eventos de riesgo que se cierran manualmente (al elegir **Resolver**, **Falso positivo** u **Omitir**) se pueden reactivar al establecer el estado del evento de nuevo en **Activo**. Los eventos de riesgo reactivados contribuyen al cálculo del nivel de riesgo del usuario. Los eventos de riesgo cerrados mediante una corrección (como el restablecimiento de una contraseña segura) no se pueden reactivar.




**Para abrir el cuadro de diálogo de configuración relacionado**:

1. En la hoja **Azure AD Identity Protection**, en **Investigate** (Investigar), haga clic en **Risk events** (Eventos de riesgo).

	![Restablecimiento manual de contraseña](./media/active-directory-identityprotection/1002.png "Restablecimiento manual de contraseña")

2. En la lista de **eventos de riesgo**, haga clic en uno de ellos.

	![Restablecimiento manual de contraseña](./media/active-directory-identityprotection/1003.png "Restablecimiento manual de contraseña")

2. En la hoja del riesgo, haga clic con el botón derecho en un usuario.

	![Restablecimiento manual de contraseña](./media/active-directory-identityprotection/1004.png "Restablecimiento manual de contraseña")



### Cierre manual de todos los eventos de riesgo de un usuario

En lugar de cerrar manualmente los eventos de riesgo de un usuario de uno en uno, Azure Active Directory Identity Protection también proporciona un método para cerrar todos los eventos de un usuario con un solo clic.


![Acciones](./media/active-directory-identityprotection/2222.png "Acciones")

Al hacer clic en **Dismiss all events** (Descartar todos los eventos), se cierran todos los eventos y el usuario afectado deja de correr riesgo.



## Corrección de eventos de riesgo del usuario

Una corrección es una acción para proteger una identidad o un dispositivo que antes fue sospechoso o que se sabe que estuvo en peligro. Una acción de corrección restaura la identidad o el dispositivo a un estado seguro y resuelve los anteriores eventos de riesgo asociados a la identidad o el dispositivo.

Para corregir los eventos de riesgo del usuario, puede:

- Realizar un restablecimiento de contraseña segura para corregir manualmente los eventos de riesgo del usuario.

- Configurar una directiva de seguridad de riesgo del usuario para mitigar o corregir automáticamente los eventos de riesgo del usuario.

- Recrear la imagen del dispositivo infectado.


### Restablecimiento manual de una contraseña segura

Restablecer una contraseña segura es una corrección eficaz para muchos de los eventos de riesgo y, cuando se realiza, estos eventos de riesgo cierran y vuelven a calcular automáticamente el nivel de riesgo del usuario. Puede utilizar el panel de Identity Protection para iniciar el restablecimiento de la contraseña de un usuario en peligro.

El cuadro de diálogo relacionado proporciona dos métodos diferentes de restablecer una contraseña:

**Restablecer contraseña**: seleccione **Require user to reset password** (Solicitar al usuario que restablezca la contraseña) para permitir al usuario una recuperación automática si el usuario se ha registrado para la autenticación multifactor. Durante el siguiente inicio de sesión del usuario, el usuario debe resolver un desafío de autenticación multifactor correctamente y, a continuación, debe cambiar la contraseña. Esta opción no está disponible si la cuenta de usuario aún no tiene registrada la autenticación multifactor.

**Contraseña temporal**: seleccione **Generar una contraseña temporal** para invalidar inmediatamente la contraseña existente y crear una nueva contraseña temporal para el usuario. Envíe la nueva contraseña temporal a una dirección de correo electrónico alternativa para el usuario o el administrador del usuario. Dado que la contraseña es temporal, se le pedirá al usuario que cambie la contraseña al iniciar sesión.


![Directiva](./media/active-directory-identityprotection/1005.png "Directiva")


**Para abrir el cuadro de diálogo de configuración relacionado**:

1. En la hoja **Azure AD Identity Protection**, haga clic en **Users flagged for risk** (Usuarios marcados para riesgo).

	![Restablecimiento manual de contraseña](./media/active-directory-identityprotection/1006.png "Restablecimiento manual de contraseña")


2. En la lista de usuarios, seleccione un usuario que tenga al menos un evento de riesgo.

	![Restablecimiento manual de contraseña](./media/active-directory-identityprotection/1007.png "Restablecimiento manual de contraseña")


2. En la hoja del usuario, haga clic en **Reset password** (Restablecer contraseña).

	![Restablecimiento manual de contraseña](./media/active-directory-identityprotection/1008.png "Restablecimiento manual de contraseña")





## Directiva de seguridad de riesgo del usuario

Una directiva de seguridad de riesgo del usuario es una directiva de acceso condicional que evalúa el nivel de riesgo de un usuario específico y aplica acciones de corrección y mitigación según reglas y condiciones predefinidas.


![Directiva de riesgo de usuario](./media/active-directory-identityprotection/1009.png "Directiva de riesgo de usuario")


Azure AD Identity Protection ayuda a administrar la mitigación y corrección de usuarios marcados como de riesgo al permitirle:

- Configurar los usuarios y grupos a los que se aplica la directiva:

	![Directiva de riesgo de usuario](./media/active-directory-identityprotection/1010.png "Directiva de riesgo de usuario")


- Establecer el umbral de nivel de riesgo del usuario (bajo, medio o alto) que desencadena la directiva:

	![Directiva de riesgo de usuario](./media/active-directory-identityprotection/1011.png "Directiva de riesgo de usuario")


- Establecer los controles que se aplicará cuando se desencadene la directiva:

	![Directiva de riesgo de usuario](./media/active-directory-identityprotection/1012.png "Directiva de riesgo de usuario")


- Cambiar el estado de la directiva:

	![Directiva de riesgo de usuario](./media/active-directory-identityprotection/403.png "Registro MFA")


- Revisar y evaluar el impacto de un cambio antes de activarlo:

	![Directiva de riesgo de usuario](./media/active-directory-identityprotection/1013.png "Directiva de riesgo de usuario")


Elegir un umbral **Alto** reduce el número de veces que una directiva se desencadena y minimiza el impacto en los usuarios. Sin embargo, excluye los usuarios marcados en riesgo **Bajo** y **Medio** por la directiva, lo que puede no proteger las identidades o los dispositivos que antes fueron sospechosos o que se sabe que estuvieron en peligro.

Al establecer la directiva:

- Excluya a los usuarios que es probable que generen una gran cantidad de falsos positivos (desarrolladores, analistas de seguridad).

- Excluya a los usuarios que tengan configuraciones regionales en las que no resulte práctico habilitar la directiva (por ejemplo, no tienen acceso al departamento de soporte técnico).

- Utilice un umbral **Alto** durante la puesta en servicio inicial de la directiva, o bien si debe minimizar los desafíos que ven los usuarios finales.

- Utilice un umbral **Bajo** si su organización requiere un mayor grado de seguridad. Seleccionar un umbral **Bajo** presenta desafíos adicionales de inicio de sesión del usuario, pero aumenta la seguridad.

El valor predeterminado recomendado para la mayoría de las organizaciones es configurar una regla para un umbral **Medio** con el fin de lograr un equilibrio entre la facilidad de uso y la seguridad.

Para obtener información general de la experiencia de usuario relacionada, consulte:

- [Flujo de recuperación de cuentas en peligro](active-directory-identityprotection-flows.md#compromised-account-recovery).

- [Flujo de cuentas en peligro bloqueadas](active-directory-identityprotection-flows.md#compromised-account-blocked).


**Para abrir el cuadro de diálogo de configuración relacionado**:

1. En la hoja **Azure AD Identity Protection**, en las sección **Configure** (Configurar), haga clic en **User risk policy** (Directiva de riesgo de usuario).

	![Directiva de riesgo de usuario](./media/active-directory-identityprotection/1009.png "Directiva de riesgo de usuario")






## Mitigación de los eventos de riesgo del usuario
Los administradores pueden establecer una directiva de seguridad de riesgo del usuario para bloquear a los usuarios al iniciar sesión en función del nivel de riesgo.

El bloqueo de un inicio de sesión:
 
- Evita la generación de nuevos eventos de riesgo del usuario para el usuario afectado.

- Permite a los administradores corregir manualmente los eventos de riesgo que afectan a la identidad del usuario y restaurarla a un estado seguro.



## ¿Qué es un nivel de riesgo de inicio de sesión?

Un nivel de riesgo de inicio de sesión (Alto, Medio o Bajo) es una indicación de la probabilidad de que en un determinado inicio de sesión, otra persona esté intentando autenticarse con la identidad del usuario. Se evalúa en el momento de un inicio de sesión y tiene en cuenta los eventos de riesgo y los indicadores detectados en tiempo real para ese inicio de sesión específico.

## Mitigación de eventos de riesgo de inicio de sesión 
Una mitigación es una acción para limitar la capacidad de un atacante de aprovechar una identidad o un dispositivo en peligro sin restaurar la identidad o el dispositivo a un estado seguro. Una mitigación no resuelve los anteriores eventos de riesgo de inicio de sesión asociados a la identidad o el dispositivo.

Puede usar el acceso condicional en Azure AD Identity Protection para mitigar los eventos de riesgo de inicio de sesión automáticamente. Mediante estas directivas, tenga en cuenta el nivel de riesgo del usuario o el inicio de sesión para bloquear inicios de sesión peligrosos o solicite que el usuario realice la autenticación multifactor. Estas acciones pueden evitar que un atacante pueda aprovechar una identidad robada para causar daños, y pueden darle algún tiempo para proteger la identidad.


## Directiva de seguridad de riesgo de inicio de sesión

Una directiva de riesgo de inicio de sesión es una directiva de acceso condicional que evalúa el riesgo de un inicio de sesión concreto y aplica mitigaciones de acuerdo con las reglas y condiciones predefinidas.

![Directiva de riesgo de inicio de sesión](./media/active-directory-identityprotection/1014.png "Directiva de riesgo de inicio de sesión")


Azure AD Identity Protection ayuda a administrar la mitigación de inicios de sesión peligrosos al permitir:

- Configurar los usuarios y grupos a los que se aplica la directiva:

	![Directiva de riesgo de inicio de sesión](./media/active-directory-identityprotection/1015.png "Directiva de riesgo de inicio de sesión")


- Establecer el umbral de nivel de riesgo del inicio de sesión (bajo, medio o alto) que desencadena la directiva:

	![Directiva de riesgo de inicio de sesión](./media/active-directory-identityprotection/1016.png "Directiva de riesgo de inicio de sesión")


- Establecer los controles que se aplicará cuando se desencadene la directiva:

	![Directiva de riesgo de inicio de sesión](./media/active-directory-identityprotection/1017.png "Directiva de riesgo de inicio de sesión")


- Cambiar el estado de la directiva:

	![Registro MFA](./media/active-directory-identityprotection/403.png "Registro MFA")

- Revisar y evaluar el impacto de un cambio antes de activarlo:

	![Directiva de riesgo de inicio de sesión](./media/active-directory-identityprotection/1018.png "Directiva de riesgo de inicio de sesión")

 
Elegir un umbral **Alto** reduce el número de veces que una directiva se desencadena y minimiza el impacto en los usuarios.
 
Sin embargo, excluye los usuarios marcados con un riesgo **Bajo** y **Medio** por la directiva, lo que puede no bloquear a un atacante para que no pueda aprovechar una identidad en peligro.

Al establecer la directiva:

- Excluya a los usuarios que no tengan o no puedan tener la autenticación multifactor.

- Excluya a los usuarios que tengan configuraciones regionales en las que no resulte práctico habilitar la directiva (por ejemplo, no tienen acceso al departamento de soporte técnico).

- Excluya a los usuarios que es probable que generen una gran cantidad de falsos positivos (desarrolladores, analistas de seguridad).

- Utilice un umbral **Alto** durante la puesta en servicio inicial de la directiva, o bien si debe minimizar los desafíos que ven los usuarios finales.

- Utilice un umbral **Bajo** si su organización requiere un mayor grado de seguridad. Seleccionar un umbral **Bajo** presenta desafíos adicionales de inicio de sesión del usuario, pero aumenta la seguridad.

El valor predeterminado recomendado para la mayoría de las organizaciones es configurar una regla para un umbral **Medio** con el fin de lograr un equilibrio entre la facilidad de uso y la seguridad.

 
La directiva de riesgo de inicio de sesión:

- Se aplica a todo el tráfico del explorador e inicios de sesión mediante una autenticación moderna.
- No afecta a aplicaciones que utilizan protocolos de seguridad anteriores al deshabilitar el punto de conexión WS-Trust en el IDP federado, como ADFS.

La página **Eventos de riesgo** de la consola de Identity Protection enumera todos los eventos:

- A los que se aplicó esta directiva.
- Puede revisar la actividad y determinar si la acción era apropiada o no.

Para obtener información general de la experiencia de usuario relacionada, consulte:

- [Recuperación de inicios de sesión peligrosos](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

- [Inicios de sesión peligrosos bloqueados](active-directory-identityprotection-flows.md#risky-sign-in-blocked)

- [Registro de la autenticación multifactor durante un inicio de sesión peligroso](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)





**Para abrir el cuadro de diálogo de configuración relacionado**:

1. En la hoja **Azure AD Identity Protection**, en las sección **Configure** (Configurar), haga clic en **Sign-in risk policy** (Directiva de riesgo de inicio de sesión).

	![Directiva de riesgo de usuario](./media/active-directory-identityprotection/1014.png "Directiva de riesgo de usuario")





## Directiva de registro de la autenticación multifactor

Azure Multi-Factor Authentication es un método que se usa para comprobar la identidad del usuario que requiere usar algo más que un nombre de usuario y una contraseña. Proporciona una segunda capa de seguridad a los inicios de sesión y transacciones de los usuarios. Se recomienda requerir Azure Multi-Factor Authentication en los inicios de sesión de usuario por los siguientes motivos:

- Ofrece autenticación segura con una gama de opciones de comprobación sencillas.

- Desempeña un papel fundamental en la preparación de su organización para protegerse y recuperarse frente a cuentas comprometidas.

![Directiva de riesgo de usuario](./media/active-directory-identityprotection/1019.png "Directiva de riesgo de usuario")



Para obtener más detalles, consulte [Qué es Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md).


Azure AD Identity Protection ayuda a administrar la puesta en servicio del registro de autenticación multifactor al configurar una directiva que le permite:



- Configurar los usuarios y grupos a los que se aplica la directiva:

	![Directiva de MFA](./media/active-directory-identityprotection/1020.png "Directiva de MFA")



- Establecer los controles que se aplicará cuando se desencadene la directiva:

	![Directiva de MFA](./media/active-directory-identityprotection/1021.png "Directiva de MFA")


- Cambiar el estado de la directiva:

	![Directiva de MFA](./media/active-directory-identityprotection/403.png "Directiva de MFA")

- Ver el estado actual del registro:

	![Directiva de MFA](./media/active-directory-identityprotection/1022.png "Directiva de MFA")


Para obtener información general de la experiencia de usuario relacionada, consulte:

- [Flujo de registro de autenticación multifactor](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).

- [Registro de la autenticación multifactor durante un inicio de sesión peligroso](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).





**Para abrir el cuadro de diálogo de configuración relacionado**:

1. En la hoja **Azure AD Identity Protection**, en las sección **Configure** (Configurar), haga clic en **Multi-factor authentication registration** (Registro de Multi-factor Authentication).

	![Directiva de MFA](./media/active-directory-identityprotection/1019.png "Directiva de MFA")





## Pasos siguientes

 - [Channel 9: Azure AD and Identity Show: Identity Protection Preview (Channel 9: Presentación de Azure AD e Identity: versión preliminar de Identity Protection)](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Types of risk events detected by Azure Active Directory Identity Protection (Tipos de eventos de riesgo que detecta Azure Active Directory Identity Protection)](active-directory-identityprotection-risk-events-types.md)
 - [Vulnerabilities detected by Azure Active Directory Identity Protection (Vulnerabilidades detectadas por Azure Active Directory Identity Protection)](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory Identity Protection notifications (Notificaciones de Azure Active Directory Identity Protection)](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory Identity Protection flows (Flujos de Azure Active Directory Identity Protection)](active-directory-identityprotection-flows.md)
 - [Azure Active Directory Identity Protection playbook (Guía de Azure Active Directory Identity Protection)](active-directory-identityprotection-playbook.md)
 - [Azure Active Directory Identity Protection glossary (Glosario de Azure Active Directory Identity Protection)](active-directory-identityprotection-glossary.md)

 - [Habilitación de Azure Active Directory Identity Protection](active-directory-identityprotection-enable.md)
 - [Azure Active Directory Identity Protection: desbloqueo de usuarios](active-directory-identityprotection-unblock-howto.md)

 - [Introducción a Azure Active Directory Identity Protection y Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)

<!---HONumber=AcomDC_0921_2016-->