OJO PARECE QUE HEMOS CAMBIADO DE VERSION!!!
https://gist.github.com/WGrape/9b27bb3f798f7385f699e621b326353d


admin/passM0od)e

Administration / Security / Quitar password policy


https://blog.devsense.com/2019/php-nginx-docker




Deep link
https://www.youtube.com/watch?v=nLLYWlxzHUY


Configuración LTI en moodle:
https://docs.moodle.org/403/en/LTI_External_tools


La URL que se le pone a nivel de curso sólo sirve si previamente se ha configurado a nivel de sitio
Busca en la tabla lti_types por dominio:
```sql
SELECT *
FROM {lti_types}
WHERE tooldomain = :tooldomain
  AND (course = :siteid OR course = :courseid)
  AND state = :state
```

A nivel de sitio
------------------------------------------
Parece ser que hay un flujo para el registro utilizando este sistema. La herramienta tiene que mostrar un UI y habrá una serie
de pasos, al final de la cual se registra.

Del documento anterior:

Registering an LTI Advantage tool
Moodle allows a simple way to deploy an LTI Advantage tool. If the tool provider supports that flow, you should have received a registration URL. Simply enter that url to a tool in the 'Tool URL' and press 'Add LTI Advantage'.

The tool registration UI will open and guide you to the steps for registration. Once completed, the registration panel will close and the list of tools will refresh. If the registration was successful, the newly added tool should be visible in a pending state.

The pending state allows you to review the tool configuration, the privacy and services granted, apply possible modifications (for example how the tool appears in the activity chooser), then activate the tool to make it available.

In case of errors when trying to start the registration, validate the registration URL with the tool's vendor the registration URL is for an LTI Advantage registration and not for the deprecated LTI 2 registration.

For Tool Developers looking to support the automated registration of LTI Advantage tool, there is an implementation guide available. The registration process is also being standardized by IMS Global LTI Working Group. There is also a Youtube video demo-ing and explaining the flow.
