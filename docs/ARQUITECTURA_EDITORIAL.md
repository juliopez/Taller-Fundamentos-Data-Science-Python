# Arquitectura editorial de GitHub Pages

## Objetivo

Ofrecer una experiencia de aprendizaje autónomo para personas que no conocen al autor ni el contexto de creación de los materiales. GitHub Pages actúa como capa editorial; GitHub conserva archivos, código y versiones.

## Navegación pública principal

1. **Inicio** (`docs/index.md`): propuesta de valor, públicos y tres acciones de entrada.
2. **Rutas de aprendizaje** (`docs/rutas.md`): secuencias sugeridas, prerrequisitos, resultados y comprobaciones de avance.
3. **Biblioteca** (`docs/biblioteca.md`): acceso por tipo de recurso y propósito.
4. **Laboratorio** (`docs/laboratorio.md`): tres simuladores con objetivos y experimentos.
5. **Acerca del proyecto** (`docs/acerca.md`): procedencia, autoría, licencias y citación.

## Contenido de segundo nivel

- `taller_2025.md` y `machine_learning_2026.md`: colecciones de origen y acceso ampliado.
- `catalogo.md`: inventario documental completo, no itinerario de aprendizaje.
- `materiales.md`, `sesiones.md`, `index2.md`: páginas heredadas, excluidas del menú principal.
- `Simulador_*.html`: herramientas interactivas, accesibles desde Laboratorio.

## Convenciones editoriales

- Priorizar **qué aprenderás** sobre el nombre técnico del archivo.
- Una ruta indica **secuencia sugerida**, nunca semanas ni fechas.
- Un módulo o etapa declara propósito, conocimientos previos, recursos y una pregunta para comprobar avance.
- Un recurso conserva su nombre físico para no romper dependencias; el portal muestra un título comprensible.
- Diferenciar ejemplos sintéticos, ejercicios y evidencias empíricas.
- Enlaces del menú principal relativos a `docs/` y compartidos por todas las páginas.
- No duplicar el inventario técnico en Inicio.
- Mantener identidad visual mediante `assets/css/portal.css` y `assets/css/simuladores.css`.

## Control de calidad para futuras publicaciones

1. ¿Se entiende el propósito sin instrucciones del profesor?
2. ¿Hay un único lugar editorial donde comenzar?
3. ¿El título describe el aprendizaje y no solo el nombre del archivo?
4. ¿Están claros los conocimientos previos y el uso recomendado?
5. ¿Se distingue material histórico de itinerario actual?
6. ¿Los enlaces funcionan y el diseño se adapta a móvil?
7. ¿El contenido es accesible con teclado y tiene contraste suficiente?
8. ¿La publicación de GitHub Pages finalizó correctamente?

## Conservación

Esta reorganización no renombra ni elimina notebooks, datasets, presentaciones o documentos históricos. Los identificadores `2025`, `2026`, `Semana` y `Sesion` pueden permanecer en rutas físicas y documentación de origen. No deben utilizarse como estructura de navegación principal.

## Pendientes de validación

- Revisar visualmente el sitio desplegado en escritorio y móvil.
- Verificar el despliegue de GitHub Actions y los enlaces internos.
- Auditar individualmente la precisión pedagógica y accesibilidad de todos los archivos históricos y simuladores.
