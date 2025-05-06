# TutorBot - Flujo de Aprendizaje de Inglés

## Resumen
TutorBot es un Agente Tutor de Inglés construido con **n8n**, diseñado para guiar a los estudiantes desde el nivel **A1** hasta **B2** del MCER. Ofrece lecciones estructuradas, cuestionarios y exámenes de nivel, rastrea el progreso en tiempo real y se comunica en español a través de **Telegram**.

## Características

- **Evaluación Inicial**: Asigna el nivel **A1** a nuevos usuarios y genera cuestionarios diagnósticos.
- **Entrega de Lecciones**: Proporciona de 3 a 4 sesiones semanales (30–45 minutos), incluyendo repaso, nuevos temas y micro-cuestionarios.
- **Seguimiento de Progreso**: Actualiza el progreso del usuario en **Supabase** tras cada lección o cuestionario.
- **Programación de Actividades**: Reserva sesiones y envía recordatorios mediante la **API de calendario MCP**.
- **Retroalimentación y Motivación**: Ofrece refuerzo positivo y ejercicios adicionales para puntajes menores al 80%.

### Fuentes de Datos
- **Supabase SQL**: Gestiona tablas de `cefr_levels`, `lessons` y `user_progress`.
- **Base de Datos Vectorial**: Recupera contenido de libros de texto.
- **Servicio de Calendario**: Programa lecciones y cuestionarios.

## Estructura del Flujo

### Nodos Principales
1. **Telegram Trigger**: Captura mensajes de los usuarios.
2. **Get Input**: Procesa texto y datos del usuario.
3. **Get All**: Obtiene el progreso del usuario desde **Supabase**.
4. **Lección (Agent)**: Lógica principal para entrega de lecciones y seguimiento de progreso.
5. **OpenAI Chat Model**: Genera contenido de lecciones utilizando modelos GPT.
6. **Supabase Vector Store**: Recupera contenido de libros de texto.
7. **Embeddings OpenAI**: Genera incrustaciones para búsqueda vectorial.
8. **MCP Client**: Maneja operaciones de calendario.
9. **Simple Memory**: Mantiene el contexto del chat.
10. **Telegram**: Envía respuestas a los usuarios.

### Conexiones
Vincula los nodos para un flujo de datos continuo desde la entrada de **Telegram** hasta la respuesta generada.

## Configuración

### Credenciales
- **API de Telegram**: Configurar en los nodos `Telegram Trigger` y `Telegram`.
- **API de Supabase**: Establecer en `Supabase Vector Store` y `Get All`.
- **API de OpenAI**: Agregar en los nodos `OpenAI Chat Model` y `Embeddings OpenAI`.

### Entorno
- Asegurar que las tablas de **Supabase** (`cefr_levels`, `lessons`, `user_progress`) estén configuradas.
- Verificar el endpoint de la **API de calendario MCP**.

### Activación
1. Habilitar el flujo en **n8n** (`active: true`).
2. Probar la integración del webhook de **Telegram**.

## Políticas

- **Frecuencia**: 3–4 lecciones por semana, micro-cuestionarios diarios, cuestionarios semanales y exámenes de nivel.
- **Secuencia**: Sigue el orden del libro de texto sin saltos.
- **Evaluación**: Se requiere un puntaje ≥80% para aprobar cuestionarios; ≥70% para remediación.
- **Respuestas**: Siempre en español.

## Uso

1. Los usuarios interactúan vía **Telegram**, enviando mensajes para activar lecciones o cuestionarios.
2. TutorBot responde con contenido personalizado, programa sesiones y rastrea el progreso del usuario.

## Dependencias

- **n8n**
- **API de Telegram**
- **Supabase** (SQL + Base de Datos Vectorial)
- **API de OpenAI**
- **API de Calendario MCP**
