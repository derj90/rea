# REA en el entorno de derj90: fork y Hermes

Esta guía acompaña el fork [derj90/rea](https://github.com/derj90/rea) del proyecto original [morluto/rea](https://github.com/morluto/rea). Es documentación de preparación: **no afirma que REA esté instalado, configurado o conectado a ChatGPT ni a Hermes**.

## 1. Qué aporta cada parte

- El fork de GitHub es la copia del código que podemos consultar, estudiar y modificar mediante la conexión de GitHub.
- El clon local permite desarrollar y probar cambios en el equipo.
- El paquete publicado `rea-agents` es una vía independiente para registrar REA como servidor MCP local en Hermes. El setup del paquete publicado **no ejecuta automáticamente el código modificado de este fork**.
- El acceso de ChatGPT a los archivos de GitHub no constituye por sí mismo una conexión MCP activa con REA. Hermes debe tener la conexión cargada en su propia sesión.

## 2. Prerrequisitos en el Mac

- `git`, Node.js y npm.
- Versión compatible de Node.js: 22.x desde 22.19, 24.x desde 24.11 o 26+. No usar 23, 25 ni versiones prerelease.
- Hermes ya instalado y una copia de seguridad de cualquier configuración personal relevante.

Comprobar en Terminal:

```bash
git --version
node --version
npm --version
```

## 3. Clonar **nuestro fork** para trabajar sobre el código

```bash
mkdir -p ~/Projects
cd ~/Projects
git clone https://github.com/derj90/rea.git
cd rea
git remote -v
git remote add upstream https://github.com/morluto/rea.git
git remote -v
```

Si `upstream` ya existe, **no repetir** el último `remote add`. Usar `git remote -v` para verificar que `origin` corresponde a `derj90/rea` y `upstream` a `morluto/rea`. No guardar tokens, credenciales ni datos institucionales en el repositorio.

## 4. Configurar Hermes con una versión publicada y fija (ruta recomendada para empezar)

La documentación oficial del repositorio identifica `rea-agents@6.4.0` como la versión npm comprobada el 11 de octubre de 2026, e incluye soporte de setup para Hermes. Verificar su disponibilidad real en npm al instalar.

Primero **ver el plan sin modificar la configuración**:

```bash
npx -y rea-agents@6.4.0 setup --client hermes --dry-run --json
```

Revisar las rutas propuestas, el perfil activo, las copias de seguridad y cualquier dependencia opcional. Si todo corresponde al Hermes correcto, ejecutar la instalación interactiva:

```bash
npx rea-agents@6.4.0 setup --client hermes
```

No aprobar automáticamente la instalación de herramientas opcionales como Hopper si no son necesarias. Setup registra `mcp_servers.rea` en el `config.yaml` del perfil Hermes activo y configura la skill correspondiente según su plan. La ruta depende de `HERMES_HOME`, del perfil y de otros ajustes; no editar a ciegas un archivo supuesto.

Comprobar después:

```bash
npx -y rea-agents@6.4.0 doctor --client hermes --json
```

**Reiniciar o reconectar Hermes**, pedirle que enumere sus herramientas MCP y confirmar que estén disponibles las de REA. `doctor` comprueba configuración/prerrequisitos; por sí solo no prueba que una sesión viva ya haya cargado el servidor.

## 5. Primera prueba segura con Hermes

Elegir un proyecto propio o un directorio de JavaScript/Electron que estemos autorizados a analizar. Pedir:

> Enumera las herramientas MCP disponibles y verifica si REA está conectado. Sin modificar el objetivo, investiga su estructura, módulos y dependencias. Separa observaciones verificadas, inferencias y datos desconocidos, con sus evidencias y limitaciones. No ejecutes software del objetivo sin autorización.

El análisis estático de JavaScript no requiere Hopper, Ghidra o IDA. Para análisis nativo profundo habrá que decidir e instalar un proveedor por separado y revisar permisos y riesgos.

## 6. Si más adelante queremos usar modificaciones de **nuestro fork**

No confundir la instalación npm anterior con una instalación desde el código fuente. Seguir entonces [las instrucciones de desarrollo](../AGENTS.md) y [la guía de instalación](installation.md) de la revisión específica del fork, instalar dependencias bloqueadas con `npm ci`, construir con `npm run build:cached` y revisar un plan de setup apuntando al checkout local. Probar cambios en un entorno aislado antes de modificar la conexión estable de Hermes.

## Límites y fuentes

- Fork: https://github.com/derj90/rea
- Original: https://github.com/morluto/rea
- Guía oficial: https://github.com/morluto/rea/blob/main/docs/installation.md
- La conexión GitHub de ChatGPT permite leer y editar el repositorio conectado, pero **no proporciona automáticamente ejecución de los binarios, acceso al Mac ni un servidor MCP REA disponible dentro de este chat**.
