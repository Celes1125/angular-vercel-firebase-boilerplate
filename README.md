# 🚀 Angular + Vercel + Firebase Boilerplate

Plantilla lista para desplegar Angular como SPA en Vercel, con buenas prácticas de seguridad y despliegue continuo.

---

## 🔒 Seguridad y buenas prácticas

- **Ignorar `.env`**: nunca subas tus claves al repositorio.
- **.gitignore reforzado**: incluye `dist/`, `node_modules/` y `.env*`.
- **Variables de entorno**: usa siempre `.env.production` y súbelas a Vercel en vez de hardcodearlas en el código.
- **Credenciales sensibles**: Firebase, API keys, etc., siempre por entorno seguro.

---

## 📂 Estructura del proyecto

```
/
├── src/
│   ├── app/              # Código Angular
│   ├── assets/           # Archivos estáticos
│   ├── index.html        # Entry point SPA
│   └── main.ts           # Bootstrap Angular
├── angular.json          # Configuración Angular
├── vercel.json           # Configuración Vercel (rewrites SPA)
├── .gitignore            # Ignorar dist, node_modules, envs
├── .env.production.example
└── README.md             # Esta guía
```

---

## ⚙️ Angular build y carpeta `dist`

- Al ejecutar `npm run build`, Angular genera la carpeta **`dist/`**.  
- Desde Angular 17+, la salida puede ir a `dist/browser`. Para Vercel, basta configurar la **Output Directory** como `dist` o `dist/browser` según tu versión.  
- Importante: nunca subas `dist` al repo, ya que Vercel la genera en cada build.

---

## 🌍 Configuración en Vercel

### 1. **Output Directory**
En el dashboard de Vercel → Project Settings → Build & Output → Output Directory:  
```
dist
```
o, si Angular sigue generando,  
```
dist/browser
```

### 2. **Rewrites SPA**
Tu `vercel.json` asegura que cualquier ruta vaya a `index.html`:  

```json
{
  "rewrites": [
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

Esto es clave para las SPA (Single Page Applications).

---

## 🔐 Variables de entorno

Ejemplo de `.env.production.example`:

```bash
# API
DOMAIN=miapp.com
NG_APP_API_URL=https://$DOMAIN/api

# Firebase
NG_APP_FIREBASE_API_KEY=your_api_key_here
NG_APP_FIREBASE_AUTH_DOMAIN=$DOMAIN
NG_APP_FIREBASE_PROJECT_ID=your_project_id
NG_APP_FIREBASE_APP_ID=your_app_id
```

### Cómo usarlas en Angular
En tu código Angular, accedes con `import.meta.env` (Angular 17+) o usando `environment.ts` con placeholders.

### Cómo cargarlas en Vercel
1. Ve a **Project → Settings → Environment Variables**.  
2. Carga cada variable con el mismo nombre (`NG_APP_API_URL`, etc.).  
3. Marca que estén disponibles en `Production` (y `Preview` si quieres).  

---

## 🧹 Limpieza y caché

Antes de un deploy limpio:  
```bash
rm -rf dist
npm run build
```

Si Vercel guarda builds previas, puedes limpiar la caché desde su dashboard en **Deployments → Redeploy → Clear cache**.

---

## 🗂️ Monorepo vs Repo único

### Repo único
- Tienes solo el frontend Angular.  
- Es más simple y directo.  
- Ideal si tu proyecto es pequeño o mediano.

### Monorepo
- Un solo repo que incluye **varios proyectos** (ej: frontend Angular + backend NestJS + librerías compartidas).  
- Beneficios:
  - Compartir código fácilmente.  
  - Manejo centralizado de dependencias.  
- Desventajas:
  - Más complejo de configurar.  
  - Necesita herramientas (Nx, Turborepo) para organizar builds.  

👉 Recomendación: si solo tienes Angular en Vercel, **repo único** es lo ideal. Si en el futuro sumas backend, microservicios o librerías compartidas, ahí un **monorepo** cobra sentido.

---

## ✅ Checklist rápida para Deploy

```md
### Angular + Vercel Deploy Checklist

- [ ] `.gitignore` incluye `dist/`, `node_modules/`, `.env*`
- [ ] Variables sensibles están en `.env.production` y NO en código
- [ ] Subí las variables a Vercel → Settings → Environment Variables
- [ ] `angular.json` tiene `"outputPath": "dist"` (o `dist/browser`)
- [ ] Configuré en Vercel la **Output Directory**
- [ ] `vercel.json` contiene rewrites hacia `index.html`
- [ ] Carpeta `dist/` borrada antes de hacer `git push`
- [ ] Deploy limpio (opcional: clear cache en Vercel)
- [ ] Verifico que rutas internas carguen (SPA funcionando)
```

---

## 🚀 Uso

1. Instala dependencias:  
   ```bash
   npm install
   ```

2. Compila para producción:  
   ```bash
   npm run build
   ```

3. Sube a Vercel (conectar repo GitHub o usar CLI):  
   ```bash
   vercel --prod
   ```

---

Hecho con ❤️ como referencia completa para tus futuros deploys en Vercel.
