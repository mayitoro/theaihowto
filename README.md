# AIverse — Portal de Inteligencia Artificial

Portal completo de IA en español: herramientas, tutoriales, artículos, prompts y comunidad.

## 🚀 Stack tecnológico

- **Frontend**: HTML/CSS/JS puro (listo para migrar a Next.js)
- **Hosting**: Vercel (deployment automático desde GitHub)
- **Base de datos**: Supabase (usuarios, foro, newsletter)
- **CMS**: Opcional → Sanity / Contentful para artículos

---

## 📦 Estructura del proyecto

```
aiverse/
├── index.html              # Página principal (este archivo)
├── /pages
│   ├── herramientas/       # Páginas individuales por herramienta
│   │   ├── chatgpt.html
│   │   ├── claude.html
│   │   └── gemini.html
│   ├── tutoriales/         # Tutoriales individuales
│   ├── articulos/          # Blog/artículos
│   └── foro/               # Sistema de foro
├── /assets
│   ├── css/
│   └── js/
├── vercel.json             # Configuración Vercel
├── sitemap.xml             # SEO sitemap
└── robots.txt              # SEO robots
```

---

## 🛠 Deploy en Vercel (desde tu cuenta existente)

1. **Push a GitHub**:
```bash
git init
git add .
git commit -m "feat: launch AIverse AI portal"
git remote add origin https://github.com/TU_USUARIO/aiverse
git push -u origin main
```

2. **Conectar en Vercel**:
   - Ve a vercel.com → "Add New Project"
   - Importa el repositorio `aiverse`
   - Framework: **Other** (HTML estático)
   - Deploy 🚀

3. **Dominio personalizado** (recomendado):
   - `aiverse.app` o `iaverso.com` o `guia-ia.es`
   - Configurar en Vercel → Settings → Domains

---

## 🗄 Supabase — Base de datos

Crea estas tablas en tu proyecto Supabase existente (o uno nuevo):

```sql
-- Newsletter subscribers
CREATE TABLE subscribers (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  confirmed BOOLEAN DEFAULT false
);

-- Forum threads
CREATE TABLE forum_threads (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  title TEXT NOT NULL,
  content TEXT,
  author_id UUID REFERENCES auth.users(id),
  category TEXT,
  views INT DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Forum replies
CREATE TABLE forum_replies (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  thread_id UUID REFERENCES forum_threads(id) ON DELETE CASCADE,
  content TEXT NOT NULL,
  author_id UUID REFERENCES auth.users(id),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Tool ratings
CREATE TABLE tool_ratings (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  tool_slug TEXT NOT NULL,
  user_id UUID REFERENCES auth.users(id),
  rating INT CHECK (rating BETWEEN 1 AND 5),
  review TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(tool_slug, user_id)
);

-- Prompt saves
CREATE TABLE saved_prompts (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id),
  prompt_text TEXT NOT NULL,
  category TEXT,
  copies INT DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 💰 Estrategia de monetización

### 1. Google AdSense (tráfico orgánico)
Los slots de anuncios ya están marcados en el HTML:
- Banner 728×90 → debajo del nav
- 300×250 → sidebar de artículos
- Ingresos estimados: $3-8 CPM en nicho tech/IA

Reemplaza los divs `ad-slot` con el código de AdSense:
```html
<ins class="adsbygoogle" style="display:block" 
  data-ad-client="ca-pub-XXXX" data-ad-slot="XXXX" 
  data-ad-format="auto"></ins>
```

### 2. Afiliados (mayor CPL)
- **Jasper AI** → $29-49/referido (programa de afiliados)
- **ChatGPT Plus** → OpenAI no tiene programa directo pero sí con revendedores
- **Cursor** → programa de afiliados activo
- **Midjourney** → no tienen afiliados directos pero puedes hacer reviews patrocinadas
- **Hostinger/Vercel** → para tutoriales de deploy

Añade `?ref=aiverse` o links de afiliado en los botones "Probar →" de cada herramienta.

### 3. Newsletter patrocinada
Con 5,000+ suscriptores puedes cobrar $200-500 por mención patrocinada.
Plataformas: Beehiiv, Substack, ConvertKit.

### 4. Cursos premium (mayor margen)
- Curso "Domina ChatGPT de cero a experto" → $47-97
- Curso "Crea apps con IA sin código" → $97-197
- Plataforma: Gumroad, Podia o tu propio checkout con Stripe

### 5. Herramientas freemium propias
- **Generador de prompts** (con Claude API) → premium $9/mes
- **Comparador de respuestas** (múltiples modelos) → premium $15/mes

---

## 📈 SEO — Acciones prioritarias

### On-page (ya implementado):
- ✅ Schema.org markup (WebSite + ItemList)
- ✅ Meta tags optimizados
- ✅ Canonical URLs
- ✅ H1-H6 correctamente estructurados
- ✅ alt text en imágenes
- ✅ Mobile-first responsive

### Off-page (a implementar):
1. **Crear sitemap.xml** y enviar a Google Search Console
2. **robots.txt** con Sitemap URL
3. **Core Web Vitals**: el HTML estático ya da puntuaciones >90
4. **Keywords a atacar**:
   - "chatgpt en español" (18k búsquedas/mes)
   - "mejores herramientas IA" (8k/mes)
   - "tutorial claude" (4k/mes)
   - "prompts para chatgpt" (22k/mes)
5. **Link building**: escribir en Hackernoon, DEV.to, Medium con links al portal

### Contenido programático:
Genera páginas automáticamente con Supabase:
- `/herramientas/[slug]` → página por cada herramienta
- `/tutoriales/[categoria]/[slug]` → artículos de blog
- `/comparativas/[herramienta1]-vs-[herramienta2]` → ¡muy buscado!

---

## 📊 Analytics

Añade antes de `</head>`:
```html
<!-- Google Analytics 4 -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXX');
</script>
```

KPIs a medir:
- Páginas/sesión (objetivo >3)
- Tiempo en página (objetivo >2 min)
- CTR a herramientas externas (afiliados)
- Conversión newsletter

---

## 🔜 Roadmap — Próximos pasos

### Fase 1 (Semana 1-2): Lanzamiento
- [ ] Deploy en Vercel con dominio personalizado
- [ ] Configurar Supabase (newsletter + auth básico)
- [ ] Google Search Console + Analytics
- [ ] Sitemap.xml enviado
- [ ] 5 artículos de blog iniciales

### Fase 2 (Semana 3-4): Contenido
- [ ] 20 páginas individuales de herramientas
- [ ] Sistema de foro con Supabase
- [ ] Newsletter con Beehiiv/ConvertKit
- [ ] 50 prompts en la biblioteca

### Fase 3 (Mes 2): Monetización
- [ ] Google AdSense aprobado
- [ ] Links de afiliado implementados
- [ ] Primer contenido patrocinado

### Fase 4 (Mes 3+): Escalar
- [ ] Migrar a Next.js para SSR/ISR
- [ ] Generación de páginas programáticas
- [ ] Primer producto digital (guía/curso)
- [ ] Community en Discord
