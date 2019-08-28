---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-08"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Creación de una aplicación cliente
{: #api-client}

Ya tiene un conocimiento de diálogo funcional y un asistente que lo utiliza. Ahora supongamos que desea desarrollar una aplicación cliente personalizada que va a interactuar con los usuarios y a comunicarse con el servicio {{site.data.keyword.conversationfull}}.
{: shortdesc}

## Configuración del servicio {{site.data.keyword.conversationshort}}
{: #api-client-setup}

La aplicación de ejemplo que se creará en esta sección implementa varias funciones de un asistente personal cognitivo. El código de la aplicación se conectará a un asistente de {{site.data.keyword.conversationshort}}, donde tiene lugar el proceso cognitivo (como por ejemplo la detección de intenciones del usuario).

Antes de continuar con este ejemplo, debe configurar el asistente necesario:

1.  Descargue el <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example.json" download="assistant-simple-example.json"> archivo JSON </a> del conocimiento de diálogo.
1.  [Importe el conocimiento](/docs/services/assistant?topic=assistant-skill-dialog-add#skill-dialog-add-task) en una instancia del servicio {{site.data.keyword.conversationshort}}.
1.  [Cree un asistente](/docs/services/assistant?topic=assistant-assistant-add) y conecte el conocimiento que ha importado.

## Obtención de información del servicio
{: #api-client-get-info}

Para acceder a las API REST del servicio {{site.data.keyword.conversationshort}}, la aplicación debe ser capaz de autenticares con {{site.data.keyword.Bluemix}} y de conectarse al asistente adecuado. Tendrá que copiar las credenciales del servicio y el ID del asistente y pegarlos en el código de la aplicación.

Para acceder a las credenciales de servicio y al ID de asistente desde la herramienta {{site.data.keyword.conversationshort}}, vaya al separador **Asistentes** y pulse el menú ![Menú](images/kebab-react.png) correspondiente al asistente al que desea conectarse. Seleccione **Ver detalles de API** para ver los detalles del asistente, incluido el ID de asistente y la clave de API.

También puede acceder a las credenciales del servicio desde el panel de control de {{site.data.keyword.Bluemix_short}}.

## Comunicación con el servicio {{site.data.keyword.conversationshort}}
{: #api-client-communicate}

Interactuar con el servicio {{site.data.keyword.conversationshort}} es sencillo. Examine el ejemplo que conecta al servicio, envía un mensaje individual e imprime la salida en la consola:

```javascript
// Ejemplo 1: configura el derivador del servicio, envía el mensaje inicial y
// recibe respuesta.

const AssistantV2 = require('ibm-watson/assistant/v2');

// Configurar el derivador del servicio de asistente.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // sustituir por la clave de API
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // sustituir por el ID de asistente
let sessionId;

// Crear sesión.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({
      message_type: 'text',
      text: '' // iniciar conversación con mensaje vacío
    });
  })
  .catch(err => {
    console.log(err); // algo ha ido mal
  });

// Enviar mensaje al asistente.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
    .then(res => {
      processResponse(res);
    })
    .catch(err => {
      console.log(err); // algo ha ido mal
    });
}

// Procesar la respuesta.
function processResponse(response) {
  // Mostrar la salida del asistente, si la hay. Solo se admite una única
  // respuesta de texto.
  if (response.output.generic) {
    if (response.output.generic.length > 0) {
      if (response.output.generic[0].response_type === 'text') {
        console.log(response.output.generic[0].text);
    }
    }
  }


// Hemos terminado, así que cerramos la sesión.
service
  .deleteSession({
    assistant_id: assistantId,
    session_id: sessionId,
  })
  .catch(err => {
    console.log(err); // algo ha ido mal
  });
}
```
{: codeblock}
{: javascript}

```python
# Ejemplo 1: configura el derivador del servicio, envía el mensaje inicial y
# recibe respuesta.

import ibm_watson

# Configura el servicio del asistente.
service = ibm_watson.AssistantV2(
    iam_apikey = '{apikey}', # sustituir por la clave de API
    version = '2019-02-28'
)

assistant_id = '{assistant_id}' # sustituir por el ID de asistente

# Crear sesión.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Iniciar conversación con mensaje vacío.
response = service.message(
    assistant_id,
    session_id
).get_result()

# Imprimir la salida del diálogo, si hay. Solo se admite una única
# respuesta de texto.
if response['output']['generic']:
    if response['output']['generic'][0]['response_type'] == 'text':
        print(response['output']['generic'][0]['text'])

# Hemos terminado, así que suprimimos la sesión.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * Ejemplo 1: configura el derivador de servicio, envía el mensaje inicial y
 * recibe respuesta.
 */

import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.assistant.v2.model.MessageOptions;
import com.ibm.watson.assistant.v2.model.MessageResponse;
import com.ibm.watson.assistant.v2.model.SessionResponse;
import com.ibm.cloud.sdk.core.service.security.IamOptions;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suprimir mensajes de registro en stdout.
    LogManager.getLogManager().reset();

    // Configurar servicio de asistente.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2019-02-28", iamOptions);
    String assistantId = "{assistant_id}"; // sustituir por ID de asistente

    // Crear sesión.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
    String sessionId = session.getSessionId();

    // Iniciar conversación con mensaje vacío.
    MessageOptions messageOptions = new MessageOptions.Builder(assistantId,
                                                        sessionId).build();
    MessageResponse response = service.message(messageOptions).execute().getResult();

    // Imprimir la salida del diálogo, si la hay. Se supone que solo hay una respuesta de texto.
    System.out.println(response.getOutput().getGeneric().get(0).getText());

    // Hemos terminado, así que suprimimos la sesión.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

El primer paso consiste en crear un derivador para el servicio {{site.data.keyword.conversationshort}}.

El derivador es un objeto que utilizará para enviar información de entrada al servicio y para recibir la salida de este. Cuando cree el derivador del servicio, especifique las credenciales de autenticación de la clave de servicio, así como la versión de la API de {{site.data.keyword.conversationshort}} que está utilizando.

En este ejemplo de Node.js, el derivador es una instancia de `AssistantV2`, almacenada en la variable `service`. Los SDK de Watson de otros lenguajes proporcionan mecanismos equivalentes para crear instancias de un derivador de servicio.
{: javascript}

En este ejemplo de Python, el derivador es una instancia de `watson_developer_cloud.AssistantV2`, almacenada en la variable `service`. Los SDK de Watson de otros lenguajes proporcionan mecanismos equivalentes para crear instancias de un derivador de servicio.
{: python}

En este ejemplo de Java, el derivador es una instancia de `Assistant`, almacenada en la variable `service`. Los SDK de Watson de otros lenguajes proporcionan mecanismos equivalentes para crear instancias de un derivador de servicio.
{: java}

Después de crear el derivador del servicio, lo utilizaremos que crear una sesión y enviar un mensaje al asistente. En este ejemplo, el mensaje está vacío; sólo queremos activar el nodo conversation_start en el diálogo, de modo que no necesitamos ningún texto de entrada. A continuación, imprimimos el texto de la respuesta en la consola y finalmente suprimimos la sesión.

Utilice el mandato `node <filename.js>` para ejecutar la aplicación de ejemplo.
{: javascript}

Utilice el mandato `python3 <filename.py>` para ejecutar la aplicación de ejemplo.
{: python}

Pegue el código de ejemplo en un archivo denominado `AssistantSimpleExample.java`. Luego puede compilarlo y ejecutarlo.
{: java}

**Nota:** Asegúrese de haber instalado Watson SDK for Node.js utilizando `npm install ibm-watson`.
{: javascript}

**Nota:** Asegúrese de que ha instalado el SDK de Watson para Python utilizando `pip install --upgrade ibm-watson` o `easy_install --upgrade ibm-watson`.
{: python}

**Nota:** Asegúrese de haber instalado el [SDK de Watson para Java ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud/java-sdk/blob/master/README.md){: new_window}.
{: java}

Si todo funciona según lo esperado, el asistente devuelve la salida del diálogo, que luego se imprime en la consola:

```
Welcome to the Watson Assistant example!
```
{: screen}

Esta salida nos indica que nos hemos comunicado correctamente con el servicio {{site.data.keyword.conversationshort}} y hemos recibido el mensaje especificado por el nodo conversation_start en el diálogo. Ahora podemos añadir una interfaz de usuario, lo que permitirá procesar entrada de usuario.

## Proceso de información de entada de usuario para detectar intenciones
{: #api-client-process-input}

Para poder procesar la entrada de usuario, debemos añadir una interfaz de usuario a nuestra aplicación cliente. En este ejemplo, vamos a simplificar y utilizaremos una entrada y salida estándares.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Para ello utilizaremos el módulo prompt-sync de Node.js. (Puede instalar prompt-sync utilizando `npm install prompt-sync`.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Se puede utilizar la función `input` de Python 3 para hacer esto. </span>
<span class="ph style-scope doc-content" data-hd-programlang="java">Se puede utilizar la función `Console.readLine()` de Java para hacer esto.</span>

```javascript
// Ejemplo 2: añade información de entrada del usuario y detecta intenciones.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Configurar el derivador del servicio de asistente.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // sustituir por la clave de API
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // sustituir por el ID de asistente
let sessionId;

// Crear sesión.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({
      message_type: 'text',
      text: ''
    }); // iniciar conversación con mensaje vacío
  })
  .catch(err => {
    console.log(err); // algo ha ido mal
  });

// Enviar mensaje al asistente.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
    .then(res => {
      processResponse(res);
    })
    .catch(err => {
      console.log(err); // algo ha ido mal
    });
}

// Procesar la respuesta.
function processResponse(response) {

  // Si se detecta una intención, registrarla en la consola.
  if (response.output.intents.length > 0) {
    console.log('Detected intent: #' + response.output.intents[0].intent);
  }

  // Mostrar la salida del asistente, si la hay. Solo se admite una única
  // respuesta de texto.
  if (response.output.generic) {
    if (response.output.generic.length > 0) {
      if (response.output.generic[0].response_type === 'text') {
        console.log(response.output.generic[0].text);
    }
    }
  }

  // Solicitar la siguiente ronda de información de entrada.
  const newMessageFromUser = prompt('>> ');
  if (newMessageFromUser === 'quit') {
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .catch(err => {
        console.log(err); // algo ha ido mal
      });
    return;
  }
  newMessageInput = {
    message_type: 'text',
    text: newMessageFromUser
  }
  sendMessage(newMessageInput);
}
```
{: codeblock}
{: javascript}

```python
# Ejemplo 2: añade información de entrada del usuario y detecta intenciones.

import ibm_watson

# Configura el servicio del asistente.
service = ibm_watson.AssistantV2(
    iam_apikey = '{apikey}', # sustituir por la clave de API
    version = '2019-02-28'
)

assistant_id = '{assistant_id}' # sustituir por el ID de asistente

# Crear sesión.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Inicializar con un valor vacío para empezar la conversación.
message_input = {
    'message_type:': 'text',
    'text': ''
    }

# Main input/output loop
while message_input['text'] != 'quit':

    # Enviar mensaje al asistente.
    response = service.message(
        assistant_id,
        session_id,
        input = message_input
    ).get_result()

    # Si se detecta una intención, imprimirla en la consola.
    if response['output']['intents']:
        print('Detected intent: #' + response['output']['intents'][0]['intent'])

    # Imprimir la salida del diálogo, si hay. Solo se admite una única
    # respuesta de texto.
    if response['output']['generic']:
        if response['output']['generic'][0]['response_type'] == 'text':
            print(response['output']['generic'][0]['text'])

    # Solicitud para la siguiente ronda de entrada.
    user_input = input('>> ')
    message_input = {
        'text': user_input
    }

# Hemos terminado, así que suprimimos la sesión.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock }
{: python }

```java
/*
 * Ejemplo 2: añade entrada de usuario y detecta intenciones.
 */

import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.assistant.v2.model.MessageInput;
import com.ibm.watson.assistant.v2.model.MessageOptions;
import com.ibm.watson.assistant.v2.model.MessageResponse;
import com.ibm.watson.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.assistant.v2.model.SessionResponse;
import com.ibm.cloud.sdk.core.service.security.IamOptions;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suprimir mensajes de registro en stdout.
    LogManager.getLogManager().reset();

    // Configurar servicio de asistente.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2019-02-28", iamOptions);
    String assistantId = "{assistant_id}"; // sustituir por ID de asistente

    // Crear sesión.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
    String sessionId = session.getSessionId();

    // Inicializar con un valor vacío para empezar la conversación.
    String inputText = "";

    // Bucle de entrada/salida principal
    do {
      // Enviar mensaje al asistente.
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute().getResult();

      // Si se detecta una intención, imprimirla en la consola.
      List<RuntimeIntent> responseIntents = response.getOutput().getIntents();
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).getIntent());
      }

      // Imprimir la salida del diálogo, si la hay. Se supone que solo hay una respuesta de texto.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // Solicitar la siguiente ronda de información de entrada.
      System.out.print(">> ");
      inputText = System.console().readLine();
    } while(!inputText.equals("quit"));

    // Hemos terminado, así que suprimimos la sesión.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock }
{: java }

Esta versión de la aplicación empieza igual que antes: enviando un mensaje vacío al asistente para empezar la conversación.

Ahora la función `processResponse()` muestra las intenciones que detecta el conocimiento de diálogo junto con el texto de salida. Luego solicita el texto que rodea la entrada de usuario.
{: javascript }

Luego muestra las intenciones que detecta el conocimiento de diálogo junto con el texto de salida. Luego solicita el texto que rodea la entrada de usuario.
{: python }

A continuación, visualiza las intenciones que el diálogo ha detectado junto con el texto de salida y, a continuación, solicita la siguiente ronda de la entrada del usuario.
{: java}

Todavía no hemos implementado una forma de lenguaje natural para finalizar la conversación, por lo que estamos utilizando el mandato literal `quit` para indicar que el programa debe suprimir la sesión y salir.

```
Welcome to the Watson Assistant example!
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>> quit
```
{: screen}

¡Ahora estamos avanzando! El servicio {{site.data.keyword.conversationshort}} reconoce correctamente nuestras intenciones y el diálogo devuelve el texto de salida correcto (cuando se proporciona) para cada intención.

Sin embargo, no sucede nada más. Cuando preguntamos la hora, no objetemos respuesta, y cuando decimos adiós, la conversación no termina. Esto se debe a que estas intenciones requieren que la app lleve a cabo acciones adicionales.

## Implementación de acciones de la app
{: #api-client-implement-actions}

Además del texto de salida que se muestra al usuario, nuestro diálogo de {{site.data.keyword.conversationshort}} utiliza la matriz `actions` en el JSON de la respuesta para indicar cuándo la aplicación debe llevar a cabo una acción, en función de las intenciones detectadas. Cuando el diálogo determina que la aplicación cliente tiene que hacer algo, devuelve un objeto de acción de `tipo` `client`. El valor `name` de la acción indica la acción específica, que puede ser `display_time` o `end_conversation`. (Las propiedades adicionales de la acción pueden especificar parámetros, credenciales y otra información relacionada con la acción, pero, para nuestro ejemplo, no necesitamos nada más que el nombre de la acción.)

Sabemos que nuestro diálogo nunca solicitará más de una acción a la vez, por lo que nuestro cliente solo tiene que comprobar la existencia de una única acción `client` en la matriz `actions`. Si encuentra una, entonces puede llevar a cabo la acción especificada. (Esta versión también elimina la visualización de las intenciones detectadas, ahora que estamos seguros de que se han definido correctamente.)

```javascript
// Ejemplo 3: implementa acciones de la app.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Configurar el derivador del servicio de asistente.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // sustituir por la clave de API
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // sustituir por el ID de asistente
let sessionId;

// Crear sesión.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({
      message_type: 'text',
      text: ''  // iniciar conversación con mensaje vacío
    });
  })
  .catch(err => {
    console.log(err); // algo ha ido mal
  });

// Enviar mensaje al asistente.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
    .then(res => {
      processResponse(res);
    })
    .catch(err => {
      console.log(err); // algo ha ido mal
    });
}

// Procesar la respuesta.
function processResponse(response) {

  let endConversation = false;

  // Comprobar si hay acciones cliente solicitadas por el asistente.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // El usuario ha preguntado qué hora es, así que devolvemos como salida la hora del sistema local.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // El usuario se ha despedido, así que hemos terminado.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Mostrar la salida del asistente, si la hay. Se supone que solo hay una respuesta de texto.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        if (response.output.generic[0].response_type === 'text') {
          console.log(response.output.generic[0].text);
    }
      }
    }
  }

  // Si no hemos terminado, solicitar la siguiente ronda de información de entrada.
  if (!endConversation) {
    const newMessageFromUser = prompt('>> ');
    newMessageInput = {
      message_type: 'text',
      text: newMessageFromUser
    }
    sendMessage(newMessageInput);
  } else {
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .then(res => {
        return;
      })
      .catch(err => {
        console.log(err); // algo ha ido mal
      });
  }
}
```
{: codeblock}
{: javascript}

```python
# Ejemplo 3: Implementa acciones de la app.

import ibm_watson
import time

# Configura el servicio del asistente.
service = ibm_watson.AssistantV2(
    iam_apikey = '{apikey}', # sustituir por la clave de API
    version = '2019-02-28'
)

assistant_id = '{assistant_id}' # sustituir por el ID de asistente

# Crear sesión.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Inicializar con valores vacíos para empezar la conversación.
message_input = {'text': ''}
current_action = ''

# Bucle de entrada/salida principal
while current_action != 'end_conversation':
    # Borrar cualquier distintivo de acción definido por la respuesta anterior.
    current_action = ''

    # Enviar mensaje al asistente.
    response = service.message(
        assistant_id,
        session_id,
        input = message_input
    ).get_result()

    # Imprimir la salida del diálogo, si hay. Solo se admite una única
    # respuesta de texto.
    if response['output']['generic']:
        if response['output']['generic'][0]['response_type'] == 'text':
            print(response['output']['generic'][0]['text'])

    # Comprobar si hay acciones cliente solicitadas por el asistente.
    if 'actions' in response['output']:
        if response['output']['actions'][0]['type'] == 'client':
            current_action = response['output']['actions'][0]['name']

    # El usuario ha preguntado qué hora es, así que devolvemos como salida la hora del sistema local.
    if current_action == 'display_time':
        print('The current time is ' + time.strftime('%I:%M:%S %p') + '.')
    # Si no hemos terminado, solicitar la siguiente ronda de información de entrada.
    if current_action != 'end_conversation':
    user_input = input('>> ')
        message_input = {
            'text': user_input
    }

# Hemos terminado, así que suprimimos la sesión.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * Ejemplo 3: implementa acciones de la app.
 */

import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.assistant.v2.model.DialogNodeAction;
import com.ibm.watson.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.assistant.v2.model.MessageInput;
import com.ibm.watson.assistant.v2.model.MessageOptions;
import com.ibm.watson.assistant.v2.model.MessageResponse;
import com.ibm.watson.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.assistant.v2.model.SessionResponse;
import com.ibm.cloud.sdk.core.service.security.IamOptions;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suprimir mensajes de registro en stdout.
    LogManager.getLogManager().reset();

    // Configurar servicio de asistente.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2019-02-28", iamOptions);
    String assistantId = "{assistant_id}"; // sustituir por ID de asistente

    // Crear sesión.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
    String sessionId = session.getSessionId();

    // Inicializar con valores vacíos para empezar la conversación.
    String inputText = "";
    String currentAction;

    // Bucle de entrada/salida principal
    do {
      // Borrar cualquier distintivo de acción establecido por la respuesta anterior.
      currentAction = "";

      // Enviar mensaje al asistente.
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute().getResult();

      // Imprimir la salida del diálogo, si la hay. Se supone que solo hay una respuesta de texto.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // Comprobar si hay alguna acción solicitada por el asistente.
      List<DialogNodeAction> responseActions = response.getOutput().getActions();
      if(responseActions != null) {
        if(responseActions.get(0).getActionType().equals("client")) {
          currentAction = responseActions.get(0).getName();
        }
      }

      // El usuario ha preguntado qué hora es, así que devolvemos como salida la hora del sistema local.
      if(currentAction.equals("display_time")) {
        DateTimeFormatter fmt = DateTimeFormatter.ofPattern("h:mm:ss a");
        LocalTime time = LocalTime.now();
        System.out.println("The current time is " + time.format(fmt) + ".");
      }

      // Si no hemos terminado, solicitar la siguiente ronda de información de entrada.
      if(!currentAction.equals("end_conversation")) {
        System.out.print(">> ");
        inputText = System.console().readLine();
      }

    } while(!currentAction.equals("end_conversation"));

    // Hemos terminado, así que suprimimos la sesión.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

Ahora la app comprueba la matriz `actions` en la respuesta para ver si hay una acción con el valor `type`=`client`. Si es así, comprueba el valor de `name` de la acción y lleva a cabo la acción adecuada (ya sea mostrando la hora del sistema local o estableciendo un distintivo interno que indique que la conversación ha terminado).

```
Welcome to the {{site.data.keyword.conversationshort}} example!
>> hello
Good day to you.
>> what time is it?
The current time is 12:40:42 PM.
>> goodbye
OK! See you later.
```
{: screen}

Correcto. Ahora la aplicación utiliza el servicio {{site.data.keyword.conversationshort}} para identificar las intenciones en la entrada de lenguaje natural, muestra las respuestas adecuadas e implementa las acciones de cliente solicitadas.

Por supuesto, una aplicación real utilizaría una interfaz de usuario más sofisticadas, como por ejemplo una ventana de conversación web. Y también implementaría acciones más complejas, posiblemente integrando una base de datos de clientes u otros sistemas empresariales. También tendría que enviar datos adicionales al asistente, como por ejemplo un ID de usuario para identificar cada usuario exclusivo. Pero los principios básicos sobre cómo la aplicación interactúa con el servicio {{site.data.keyword.conversationshort}} seguirían siendo los mismos.

Para ver algunos ejemplos más completos, consulte las [Apps de ejemplo](/docs/services/assistant?topic=assistant-sample-apps).

## Utilización de la API v1
{: #api-client-v1-api}

El uso de la API v2 es el método recomendado para crear una aplicación cliente de tiempo de ejecución que se comunique con el servicio {{site.data.keyword.conversationshort}}. Sin embargo, algunas aplicaciones antiguas pueden seguir utilizando la API v1, que incluye un método de tiempo de ejecución parecido para enviar mensajes al espacio de trabajo dentro de un conocimiento de diálogo.

Tenga en cuenta que, si la app utiliza la API v1, se comunica directamente con el espacio de trabajo y omite las funciones de orquestación y gestión de estado del asistente. Esto significa que la aplicación es la responsable de gestionar la información de estado utilizando el contexto. La aplicación debe mantener el contexto, guardando el contexto recibido con cada respuesta y enviándolo de nuevo al servicio con cada nueva solicitud de mensaje. (El método `/message` de v1 devuelve el contexto con cada respuesta).

Para obtener más información sobre el método `/message` de v1 y sobre el contexto, revise la [Consulta de API v1 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input){: new_window}.
