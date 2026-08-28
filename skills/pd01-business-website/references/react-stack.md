# Stack alterno — Next.js (uso condicionado)

Este stack se activa solo cuando `SKILL.md` lo indica explícitamente para un proyecto. No es el método por defecto de Cognit.

**Stack:** Next.js (App Router) + React + TypeScript + Tailwind CSS + shadcn/ui. Framer Motion o Motion para animaciones cuando el efecto lo requiera.

**Cuándo se activa:** exclusivamente cuando el usuario solicita explícitamente, durante la construcción del sitio, un componente o efecto de una librería que requiere React (21st.dev, Aceternity UI, Magic UI, Hover.dev, React Bits, etc.). No se activa por tipo de proyecto, por sector del cliente, ni por anticipación — es reactivo a la solicitud puntual, no una decisión previa.

**Cuándo no:** en cualquier otro caso, incluidos sitios con muchas animaciones — si el efecto se puede lograr en vanilla/GSAP, ese es el camino, aun si visualmente se ve tan bien como el de la librería React.

**Entregables (reemplazan a los de `SKILL.md` cuando aplica este stack):** proyecto Next.js completo con estructura de carpetas estándar (`app/`, `components/`, `lib/`), `package.json`, configuración de Tailwind y shadcn ya inicializada, listo para deploy en Vercel vía GitHub (deploy nativo, sin configuración adicional).

**Flujo de fases:** se mantiene el mismo proceso de tres fases de `SKILL.md` (prototipo → desarrollo completo → ajustes). En Fase 1, el prototipo puede ser una sola página/ruta con los componentes base ya montados (navbar, hero, footer) igual que en el flujo vanilla.

**Seguridad y compatibilidad:** siguen aplicando los mismos estándares de `references/security-standards.md`, adaptados al contexto de Next.js (ej. usar `next/image` en vez de `<img>` con lazy loading manual, variables de entorno para claves en vez de hardcodearlas).

**Componentes de librerías copy-paste (21st.dev, Aceternity UI, Magic UI, etc.):** en este stack sí se pueden integrar directamente tal como los entrega la librería, sin necesidad de reescribirlos.
