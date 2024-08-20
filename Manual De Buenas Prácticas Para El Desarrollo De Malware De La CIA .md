# 🛠️ **Manual De Buenas Prácticas Para El Desarrollo De Malware De La CIA** 🛡️

**📅 Fecha:** `17 Aug 2024`

---

> El siguiente manual fue extraído de la gran filtración de la CIA **“Vault 7”** por parte de **Wikileaks** en el año 2017. Las técnicas empleadas en este documento son muy actuales y pueden seguirse al pie de la letra sin ningún problema. El manual de la CIA está muy bien escrito y detallado; no solo explica el **QUÉ HACER**, sino también el **PORQUÉ**. ¡Gózalo y **Happy Hacking**! 🎉

---

## 📋 **Consejos Generales**

- **Ofusque o encripte todas las cadenas y los datos de configuración relacionados con la funcionalidad de la herramienta.** 
  - 🔒 *Desofusque las cadenas en memoria únicamente cuando sean necesarias y bórralas cuando ya no lo sean.*
  - 🛡️ **Justificación:** Dificulta el análisis para ingenieros inversos y analistas.

- **NO descifre ni desofusque todos los datos de tipo STRING o configuración inmediatamente después de la ejecución.**
  - 🕵️ **Justificación:** Aumenta la dificultad del análisis dinámico automatizado del binario.

- **Elimine explícitamente los datos confidenciales de la memoria cuando ya no se necesiten.**
  - 🗑️ *Incluye claves de cifrado, código shell, módulos cargados, etc.*
  - 🚫 **NO confíe en el sistema operativo para hacer esto al terminar la ejecución.**
  - 🛡️ **Justificación:** Dificulta la respuesta a incidentes y la revisión forense.

- **Use una clave única para ofuscar/desofuscar STRINGS confidenciales y datos de configuración en cada implementación.**
  - 🛡️ **Justificación:** Complica el análisis de múltiples implementaciones de la misma herramienta.

- **Elimine toda la información de símbolos de depuración, rutas de compilación, nombres de usuario, etc. de la compilación final.**
  - 🚫 **Justificación:** Dificulta el análisis y la ingeniería inversa, y elimina artefactos para la atribución.

- **Elimine toda la salida de depuración (e.g., `printf()`, `OutputDebugString()`, etc.) de la compilación final.**
  - 🛡️ **Justificación:** Dificulta el análisis y la ingeniería inversa.

- **NO importe ni llame explícitamente funciones inconsistentes con la funcionalidad de la herramienta.**
  - 💻 *Ejemplo: `WriteProcessMemory`, `VirtualAlloc`, `CreateRemoteThread`, etc.*
  - 🔍 **Justificación:** Reduce el escrutinio del binario y complica el análisis estático.

- **NO genere archivos de volcado de memoria ni pantallas “azules” en caso de fallo.**
  - 🚫 **Justificación:** Evita sospechas y dificulta la respuesta a incidentes y la ingeniería inversa.

- **NO realice operaciones que hagan que la computadora no responda al usuario (e.g., picos de CPU, parpadeos de pantalla).**
  - 💻 **Justificación:** Evita atención no deseada del usuario o administrador.

- **Minimice el tamaño de los archivos binarios cargados en un destino remoto.**
  - 📦 *Idealmente, archivos menores de 150 KB.*
  - 🛡️ **Justificación:** Reduce el “tiempo en el aire” y facilita la limpieza.

- **Proporcione un medio para “desinstalar”/”eliminar” implantes y otros artefactos.**
  - 📝 *Documente los procedimientos, permisos necesarios y efectos secundarios.*
  - 🛡️ **Justificación:** Evita dejar datos no deseados y facilita la evaluación de riesgos.

- **NO deje marcas de tiempo que se correlacionen con horarios laborales de EE. UU.**
  - 🕒 *Justificación:* Evita la correlación directa con el origen en Estados Unidos.

- **NO deje datos en un binario que demuestren la participación de la CIA o el USG.**
  - 🚫 **Justificación:** Evita impactos irreversibles en las operaciones.

- **NO tenga “palabras sucias” en el binario.**
  - 🗣️ *Justificación:* Evita el escrutinio injustificado del binario.

---

## 🌐 **Redes**

- **Utilice cifrado de extremo a extremo para todas las comunicaciones de red.**
  - 🔒 **Justificación:** Protege contra el análisis del tráfico y la exposición de datos.

- **NO confíe únicamente en SSL/TLS para proteger los datos en tránsito.**
  - 🕵️ **Justificación:** Existen vectores de ataque tipo “man-in-the-middle” y fallas en el protocolo.

- **NO permita que el tráfico de red se pueda reproducir.**
  - 🔄 **Justificación:** Protege la integridad de los activos operativos.

- **Utilice protocolos de red compatibles con RFC de ITEF como capa de combinación.**
  - 🌐 **Justificación:** Los protocolos personalizados pueden llamar la atención de los analistas.

- **Realice una limpieza adecuada de las conexiones de red.**
  - 🧹 **Justificación:** Dificulta el análisis de la red y la respuesta a incidentes.

---

## 💾 **Entrada/Salida Disco**

- **DOCUMENTE explícitamente la “huella forense del disco” que podría crear la herramienta.**
  - 📝 **Justificación:** Permite evaluaciones de riesgo operativo con conocimiento de posibles artefactos forenses.

- **NO lea, escriba ni almacene en caché datos en el disco innecesariamente.**
  - 🚫 **Justificación:** Reduce la posibilidad de artefactos forenses.

- **NO escriba datos de recopilación en texto sin formato en el disco.**
  - 🔒 **Justificación:** Dificulta la respuesta a incidentes y el análisis forense.

- **Cifre todos los datos escritos en el disco.**
  - 🛡️ **Justificación:** Oculta la intención del archivo y dificulta el análisis forense.

- **Utilice un borrado seguro al eliminar archivos del disco.**
  - 🧹 *Realice al menos una pasada de ceros sobre los datos.*
  - 🛡️ **Justificación:** Dificulta la respuesta a incidentes y el análisis forense.

- **NO realice operaciones de E/S de disco que provoquen que el sistema deje de responder al usuario.**
  - 🛑 **Justificación:** Evita atención no deseada del usuario o administrador.

- **NO utilice un “encabezado/pie de página mágico” para los archivos cifrados.**
  - 🔒 **Justificación:** Evita la firma de valores mágicos.

- **NO utilice nombres de archivo ni rutas codificadas de forma rígida al escribir archivos en el disco.**
  - 🛠️ **Justificación:** Permite al operador elegir un nombre adecuado al objetivo operativo.

---

## 🕒 **Fechas/Hora**

- **Utilice GMT/UTC/Zulu como zona horaria al comparar fecha/hora.**
  - 🕒 **Justificación:** Proporciona consistencia y asegura que los disparadores se activen cuando se espera.

- **NO utilice formatos de marca de tiempo centrados en EE. UU., como MM-DD-AAAA.**
  - 📅 *Se prefiere AAAAMMDD.*
  - **Justificación:** Evita asociaciones con Estados Unidos.

---

> **Fuente:** [Wikileaks - Vault 7](https://wikileaks.org/ciav7p1/cms/page_14587109.html)
