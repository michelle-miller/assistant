---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: system entity, sys-number, sys-date, sys-time

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Detalles de las entidades del sistema
{: #system-entities}

Obtenga más información sobre las entidades del sistema que IBM incluye de serie. Estas entidades de programa de utilidad incorporadas ayudan a su asistente a reconocer términos y referencias que son utilizados comúnmente por los clientes en conversaciones, como números y fechas. 
{: shortdesc}

Las entidades del sistema están disponibles para los idiomas indicados en el tema [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support).

Si su conocimiento de diálogo está en inglés o en alemán, puede probar las entidades del sistema actualizadas. Para obtener más detalles, consulte [Entidades del sistema nuevas](/docs/services/assistant?topic=assistant-beta-system-entities).

Para obtener más información sobre cómo utilizarlas, consulte [Creación de entidades](/docs/services/assistant?topic=assistant-entities#entities-enable-system-entities).

## Entidad @sys-currency
{: #system-entities-sys-currency}

La entidad del sistema @sys-currency detecta los valores de moneda contenidos en una expresión con un símbolo de moneda o términos específicos de la moneda. Se devuelve un valor numérico.

### Formatos reconocidos
{: #system-entities-sys-currency-formats}

- 20 céntimos
- Cinco dólares
- $10

### Metadatos
{: #system-entities-sys-currency-metadata}

- `.numeric_value`: el valor numérico canónico como entero o doble, en unidades base
- `.unit`: el código de moneda de la unidad base (por ejemplo, 'USD' o 'EUR')

### Devuelve
{: #system-entities-sys-currency-returns}

Para la entada `twenty dollars` o `$1,234.56`, @sys-currency devuelve estos valores:

| Atributo                   | Tipo   | Se devuelve para `twenty dollars` | Se devuelve para `$1,234.56` |
|-----------------------------|--------|-------------------------------|-------------------------:|
| @sys-currency               | serie | 20                            |                  1234.56 |
| @sys-currency.literal       | serie | twenty dollars                |                $1,234.56 |
| @sys-currency.numeric_value | número | 20                            |                  1234.56 |
| @sys-currency.location      | matriz  | [0,14]                        |                    [0,9] |
| @sys-currency.unit          | serie | USD*                          |                      USD |

*@sys-currency.unit siempre devuelve el código de moneda ISO de 3 letras.

Para la entrada `veinte euro` o <code>&euro;1.234,56</code>, en español, @sys-currency devuelve estos valores:

| Atributo                   | Tipo   | Se devuelve para `veinte euro` | Se devuelve para <code>&euro;1.234,56</code> |
|-----------------------------|--------|-----------------------------|-------------------------:|
| @sys-currency               | serie | 20                          |                  1234.56 |
| @sys-currency.literal       | serie | veinte euro                 |                &euro;1.234,56 |
| @sys-currency.numeric_value | número | 20                          |                  1234.56 |
| @sys-currency.location      | matriz  |[0,11]                       |                     [0,9]|
| @sys-currency.unit          | serie | EUR*                        |                     EUR  |
*@sys-currency.unit siempre devuelve el código de moneda ISO de 3 letras.

Se obtienen resultados equivalentes para otros idiomas y monedas nacionales soportados.

### Consejos sobre el uso de @system-currency
{: #system-entities-currencty-usage-tips}

- Los valores de moneda también se reconocen como instancias de entidades @sys-number. Si utiliza condiciones separadas para comprobar los valores y números de moneda, coloque la condición que controla la moneda por encima de la que controla los números.

  Este método alternativo no es necesario si utiliza las entidades del sistema revisadas. Para obtener más detalles, consulte [Entidades del sistema nuevas](/docs/services/assistant?topic=assistant-beta-system-entities).
  {: note}

- Si utiliza la entidad @sys-moneda como condición de nodo y el usuario especifica `$0` como valor, el valor se reconoce como moneda correctamente, pero la condición se evalúa en el número cero, no la moneda cero. Como resultado, el `null` en la condición se evalúa como falso y el nodo no se procesa. Para comprobar
los valores de moneda de forma que se haga un tratamiento correcto de los ceros, utilice en su lugar la expresión `@sys-currency >=0` en la condición del nodo.

## Entidades @sys-date y @sys-time
{: #system-entities-sys-date-time}

La entidad del sistema `@sys-date` extrae menciones como `Friday`, `today` o `November 1`. El valor de esta entidad guarda la fecha inferida correspondiente como serie de caracteres en el formato "aaaa-MM-dd", por ejemplo "2016-11-21". El sistema cumplimenta los elementos que faltan de una fecha (como el año para "November 21") con valores de la fecha actual.

**Nota:** - Solo para el entorno local inglés, el comportamiento predeterminado del sistema para la entrada la fecha es MM/DD/AAAA. Esto solo cambia por DD/MM/AAAA si los dos primeros números son mayores que 12. El valor almacenado seguirá en el formato "aaaa-MM-dd".

La entidad del sistema `@sys-time` extrae menciones como `2pm`, `at 4` o `15:30`. El valor de esta entidad almacena la hora como una serie de caracteres en el formato "HH:mm:ss". Por ejemplo, "13:00:00."

### Menciones a fecha y hora
{: #system-entities-date-time-mentions}

Las menciones a fecha y hora, como por ejemplo `now` (ahora) o `two hours from now` (en dos horas a partir de ahora) se extraen como dos menciones de entidades separadas - una `@sys-date` y una `@sys-time`. Estos dos no están vinculadas entre sí salvo que compartan la misma serie literal que abarca la mención de fecha y hora completa.

Las menciones de fecha y hora de varias palabras, como `on Monday at 4pm` (el lunes a las 4pm) también se extraen como dos menciones @sys-date y @sys-time. Cuando se mencionan juntas de forma consecutiva, también comparten una sola serie literal que abarca la mención de fecha y hora completa.

### Rangos de fecha y horas
{: #system-entities-date-time-ranges}

Las menciones de un rango de fechas, como `the weekend` (el fin de semana), `next week` (semana que viene) o `from Monday to Friday` (de lunes a viernes) se extraen como un par de menciones de la entidad `@sys-date` que muestran el principio y el fin del rango. Asimismo, las menciones de rangos de horas, como por ejemplo `from 2 to 3` (de 2 a 3), se extraen como dos entidades `@sys-time` que muestran la hora inicial y final. Las dos entidades del par comparten una serie literal que corresponde a la mención del rango de fecha o de hora.

### Fechas y horas `Last` (última) y `Next` (siguiente)
{: #system-entities-last-next}

En algunos entornos locales, se utilizan frases como "last Monday" (el lunes pasado) para especificar únicamente el lunes de la semana anterior. En cambio, en otros entornos se utiliza "last Monday" para especificar el último día que fue lunes, que podría estar dentro de esta semana o de la pasada.

Por ejemplo, para el viernes 16 de junio, en algunos entornos locales "last Monday" podría hacer referencia al 12 de junio o al 5 de junio, mientras que en otros entornos locales solo haría referencia al 5 de junio (la semana anterior). La misma lógica se aplicaría a una frase como "next Monday" (lunes que viene).

El servicio {{site.data.keyword.conversationshort}} trata las fechas "última" ("last") y "siguiente" ("next") como si hicieran referencia al último o al siguiente día más inmediato, que puede estar dentro de la misma semana o en la anterior.

Para frases de tiempo como "for the last 3 days" (durante los últimos 3 días) o "in the next 4 hours" (durante las próximas 4 horas), la lógica es equivalente. Por ejemplo, en el caso de "in the next 4 hours", esto da lugar a dos entidades `@sys-time`: una de la hora actual y una de cuatro horas después de la hora actual.

### Husos horarios
{: #system-entities-time-zones}

Las menciones de fecha u hora relativas a la hora actual se resuelven con respecto al huso horario elegido. De forma predeterminada, es UTC (GMT). Esto significa que, de forma predeterminada, los clientes de la API REST situados en husos horarios distintos de UTC verán el valor de `now` extraído según la hora UTC actual.

Opcionalmente, el cliente de la API REST puede añadir el huso horario local como variable de contexto `$timezone`. Esta variable de contexto se debe enviar con cada solicitud de cliente. Por ejemplo, el valor de `$timezone` podría ser `América/Los_Ángeles`, `EST` o `UTC`. Para ver una lista completa de los husos horarios soportados, consulte [Husos horarios soportados](/docs/services/assistant?topic=assistant-time-zones).

Si se especifica la variable `$timezone`, los valores de las menciones @sys-date y @sys-time relativas se calculan en función del huso horario del cliente en vez de UTC.

#### Ejemplos de menciones relativas a husos horarios
{: #system-entities-time-zone-examples}

- now (ahora)
- in two hours (dentro de dos horas)
- today (hou)
- tomorrow (mañana)
- 2 days from now (dentro de 2 días)

### Formatos reconocidos
{: #system-entities-sys-date-time-formats}

- November 21
- 10:30
- at 6 pm (a las 6 pm)
- this weekend (este fin de semana)

### Devuelve
{: #system-entities-sys-date-time-returns}

Para la entrada `November 21`, @sys-date devuelve estos valores:

| Atributo               | Tipo   | Se devuelve para `November 21` |
|-------------------------|--------|---------------------------:|
| @sys-date.literal       | serie |                November 21 |
| @sys-date               | serie |                20xx-11-21 *|
| @sys-date.location      | matriz |                     [0,11]  |
| @sys-date.calendar_type | serie |                  GREGORIAN |

- @sys-date siempre devuelve la fecha en este formato: aaaa-MM-dd.
- \* Devuelve la siguiente fecha coincidente. Si esta fecha ya ha pasado este año, se devuelve la fecha del próximo año.

Para la entrada `at 6 pm`, @sys-time devuelve estos valores:

| Atributo               | Tipo   | Se devuelve para `at 6 pm` |
|-------------------------|--------|-----------------------:|
| @sys-time.literal       | serie |                at 6 pm |
| @sys-time               | serie |               18:00:00 |
| @sys-time.location      | matriz |                   [0,7]|
| @sys-time.calendar_type | serie |              GREGORIAN |

- @sys-time siempre devuelve la hora en este formato: HH:mm:ss.

Para obtener información sobre el proceso de los valores de fecha y hora, consulte la [Referencia del método de fecha y hora](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-time).
{: tip}

## Entidad @sys-location
{: #system-entities-sys-location}

**BETA, para los idiomas indicados en el tema [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support)**: la entidad del sistema @sys-location extrae nombres de lugares (país, estado/provincia, ciudad, pueblo, etc.) a partir de la entrada del usuario. El valor de la entidad no es un valor estándar del sistema de la ubicación.

### Formatos reconocidos
{: #system-entities-sys-location-formats}

- Boston
- U.S.A.
- New South Wales

Para obtener información sobre el proceso de valores de Serie, consulte la [Referencia del método Series](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}

## Entidad @sys-number
{: #system-entities-sys-number}

La entidad del sistema @sys-number detecta los números que se han escrito utilizando números o palabras. En cualquiera de los casos, se devuelve un valor numérico.

### Formatos reconocidos
{: #system-entities-sys-number-formats}

- 21
- twenty one
- 3.13

### Metadatos
{: #system-entities-sys-number-metadata}

- `.numeric_value` - el valor numérico canónico como de tipo integer (entero) o double (doble)

### Devuelve
{: #system-entities-sys-number-returns}

Para la entrada `twenty` o `1,234.56`, @sys-number devuelve estos valores:

| Atributo                   | Tipo   | Se devuelve para `twenty` | Se devuelve para `1,234.56` |
|-----------------------------|--------|-------------------|------------------------:|
| @sys-number               | serie | 20                |                 1234.56 |
| @sys-number.literal       | serie | twenty            |                1,234.56 |
| @sys-number.location      | matriz |  [0,6]             |                    [0,8]|
| @sys-number.numeric_value | número | 20                |                 1234.56 |

Para la entrada `veinte` o `1.234,56`, en español, @sys-number devuelve estos valores:

| Atributo                   | Tipo   | Se devuelve para `veinte` | Se devuelve para `1.234,56` |
|-----------------------------|--------|-----------------------|------------------------:|
| @sys-number               | serie | 20                    |                 1234.56 |
| @sys-number.literal       | serie | veinte                |                1.234,56 |
| @sys-number.location      | matriz  | [0,6]                 |                   [0,8] |
| @sys-number.numeric_value | número | 20                    |                 1234.56 |

Se obtienen resultados equivalentes para otros idiomas soportados.

### Consejos de uso de @system-number
{: #system-entities-sys-number-usage-tips}

- Si utiliza la entidad @sys-number como una condición nodo y el usuario especifica cero como valor, el valor 0 se reconoce correctamente como un número. Sin embargo, el valor 0 se interpreta como un valor `nulo` para la condición, lo que da como resultado que el nodo no se esté procesando. Para comprobar los números de forma que se haga un tratamiento correcto de los ceros, en su lugar, utilice la expresión `@sys-number >= 0` en la condición del nodo.

- Si utiliza @sys-number para comparar valores numéricos en una condición, asegúrese de incluir por separado una comprobación de la presencia del propio número. Si no se encuentra ningún número, @sys-number se evalúa como nulo, lo que podría dar lugar a que la comparación se evaluara como verdadera aunque no hubiera ningún número presente.

  Por ejemplo, no utilice `@sys-number<4` únicamente porque si no se encuentra número alguno, `@sys-number` se evalúa a null. Puesto que el valor nulo es menor que 4, la condición se evalúa como verdadera, aunque no haya ningún número presente.

  En su lugar, utilice `@sys-number AND @sys-number<4`. Si no hay ningún número, la primera condición se evalúa como falsa, lo que da lugar, correctamente, a que toda la condición se evalúe como falsa.

Para obtener información sobre el proceso de valores numéricos, consulta la [Referencia del método Números](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-numbers).
{: tip}

## Entidad @sys-percentage
{: #system-entities-sys-percentage}

La entidad del sistema @sys-percentage detecta porcentajes expresados en una expresión con el símbolo de porcentaje o escritos con la palabra `percent` (por ciento). En cualquiera de los casos, se devuelve un valor numérico.

### Formatos reconocidos
{: #system-entities-sys-percentage-formats}

- 15%
- 10 percent (10 por ciento)

### Metadatos
{: #system-entities-sys-percentage-metadata}

`.numeric_value`: el valor numérico canónico como de tipo integer (entero) o double (doble)

### Devuelve
{: #system-entities-sys-percentage-returns}

Para la entrada `1,234.56%`, @sys-percentage devuelve estos valores:

| Atributo                     | Tipo   | Se devuelve para `1,234.56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | serie |                  1234.56 |
| @sys-percentage.literal       | serie |                1,234.56% |
| @sys-percentage.location      | matriz  |                    [0,9] |
| @sys-percentage.numeric_value | número |                  1234.56 |

Para la entrada `1.234,56%`, en español, @sys-currency devuelve estos valores:

| Atributo                     | Tipo   | Se devuelve para `1.234,56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | serie |                  1234.56 |
| @sys-percentage.literal       | serie |                1.234,56% |
| @sys-percentage.location      | matriz  |                    [0,9] |
| @sys-percentage.numeric_value | número |                  1234.56 |

Se obtienen resultados equivalentes para otros idiomas soportados.

### Consejos de uso de @system-percentage
{: #system-entities-sys-percentage-usage-tips}

- Los valores de porcentaje también se reconocen como instancias de entidades @sys-number. Si utiliza condiciones separadas para comprobar los valores y números de porcentaje, coloque la condición que controla el porcentaje por encima de la que controla los números.

  Este método alternativo no es necesario si utiliza las entidades del sistema revisadas. Para obtener más detalles, consulte [Entidades del sistema nuevas](/docs/services/assistant?topic=assistant-beta-system-entities).
  {: note}

- Si utiliza la entidad @sys-porcentaje como condición de nodo y el usuario especifica `0%` como valor, el valor se reconoce como porcentaje correctamente, pero la condición se evalúa en el número cero, no en el porcentaje 0%. Así, el `null` en la condición se evalúa a falso y el nodo no se procesa. Para comprobar
los porcentajes de forma que se haga un tratamiento correcto de aquellos que sean cero, en su lugar utilice la expresión `@sys-percentage >= 0` en la condición del nodo.

- Si especifica un valor como `1-2%`, los valores `1%` y `2%` se devuelven como entidades del sistema. El índice será todo el rango comprendido entre 1% y 2%, y ambas entidades tendrán el mismo índice.

## Entidad @sys-person
{: #system-entities-sys-person}

**BETA, para los idiomas indicados en el tema [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support)**: La entidad del sistema @sys-person extrae nombres de la entrada del usuario. Los nombres se reconocen de forma individual, de modo que "Joe" no se trata como "Joseph", o viceversa. El valor de la entidad no es un valor estándar del sistema del nombre.

### Formatos reconocidos
{: #system-entities-sys-person-formats}

- Ronald
- Jane Doe
- Vijay

Para obtener información sobre el proceso de valores de Serie, consulte la [Referencia del método Series](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}
