# Taller Automatización de Atención al Cliente 24/7
Tech Stack: Gemini y n8n

En este taller vamos a implementar un chatbot para responder preguntas sobre tu empresa utilizando RAG en Gemini y con el mínimo código necesario en n8n para un taller sin mucha carga técnica.

## Configuración Inicial
1. Reclama tu 5$ de crédito gratuito en Google Cloud (LINK TBD)
2. Genera una API Key en [Google AI Studio](https://aistudio.google.com/)
3. Empieza un free trial en [n8n](https://n8n.io/)
4. Descarga estos documentos o usa los tuyos propios
   1. [Reglas del Golf](https://share.google/ACSvD0s1CGPaLYdNo)
   2. [Resultados Financieros de Inditex 2025](https://www.inditex.com/itxcomweb/api/media/95537a57-b33e-4fcb-8822-a200d9da68c1/INDITEXResultadosEjercicio2025.pdf?t=1773210068550)
   3. [Ejemplo ficticio - L'Heretat de Menorca](https://drive.google.com/file/d/1YW4krJAdQE7mvJFqRDVxAf3C-aWRpVpr/view?usp=sharing)
   4. Tus propios documentos
  
## Primeros pasos en n8n
### Creación de la Base de Conocimiento
1. Al rellenar un formulario
2. Simple Vector Store
   a. Modo "Insert Documents"
3. Embeddings Google Gemini
4. Default Data Loader
   a. Tipo: "Binary"
   b. Modo: "All Input Data"
   c. Formato de la documentación: "Automatic"
   d. Separación de Texto: "Custom"
   e. Metadata: "filename: {{ $json.filename }}"
5. Recursive Character Text Splitter
   a. Chunk Size: 1000
   b. Chunk Overlap: 200

### Implementación del Chatbot
1. Al recibir un mensaje del chat
   1. Hazlo público con un mensaje inicial
2. AI Agent - Añade Option -> System Message
   '''
   Eres un asistente servicial. Responde a las preguntas utilizando ÚNICAMENTE la información de la herramienta de base de conocimientos. Si la respuesta no se encuentra en la base de conocimientos, di: "No tengo esa información en mi base de conocimientos". No utilices conocimientos externos ni hagas suposiciones.
   '''
   1. Google Gemini Chat Model
   2. Memória de la Conversación - Simple Memory
   3. Base de Conocimiento - Simple Vector Store
      a. Modo: "Retrieve Documents (As Tool for AI Agent)"
   4. Embeddings for Retrieval


### Agent Flow
![Sample n8n Flow](agentFlow.png "Sample n8n Flow")

### Extra: integraciones externas
#### Web
Ejecutar 
'''
<link href="https://cdn.jsdelivr.net/npm/@n8n/chat/dist/style.css" rel="stylesheet" />
<script type="module">
   import { createChat } from 'https://cdn.jsdelivr.net/npm/@n8n/chat/dist/chat.bundle.es.js';

   createChat({
      webhookUrl: 'MY_WEB_HOOK_URL'
   });
</script>
'''

#### Telegram
