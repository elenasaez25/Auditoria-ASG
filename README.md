
# Auditoría ASG y Refactorización Sostenible
**Módulo:** Sostenibilidad Aplicada al Sistema Productivo  
**Unidad:** 6   
**Alumno:** Elena Sáez

---

## 1. Contexto y Objetivos

Acabas de ser contratado/a como *Junior GreenOps Developer* en una consultora tecnológica. Tu primer encargo es realizar una **Auditoría de Sostenibilidad (ASG)** sobre la página web corporativa de una empresa local.

Basándote en los principios de *Green Software Engineering*, el objetivo es analizar la web actual, identificar su "deuda técnica" en términos de sostenibilidad, y elaborar una propuesta de refactorización que la convierta en una herramienta eficiente (A), equitativa (S) y ética (G).

**Web analizada:** https://www.expansion.com/directorio-empresas/aceituneros-de-salteras-sl_1195321_A01_41.html

**Objetivos de aprendizaje:**
- Aplicar métricas de eficiencia energética en un entorno web real.
- Evaluar el cumplimiento de estándares sociales (Accesibilidad WCAG 2.2).
- Proponer soluciones de *Green Coding* y Ecodiseño para reducir la huella de carbono digital.

---

## 2. Fases de la Auditoría

### Fase 1: Dimensión Ambiental (A)

**1. Medición inicial**
Se utilizó *Lighthouse* para obtener la huella de carbono estimada por visita.

<img width="815" height="610" alt="image" src="https://github.com/user-attachments/assets/89a7d5dd-172f-43c9-be9a-0ed359cf11d4" />


Los resultados muestran métricas poco favorables en rendimiento y sostenibilidad.

**2. Identificación de *Bloatware***
Los 3 recursos más pesados identificados en la pestaña *Network* son:
1. Librería JavaScript
2. Imágenes `.png` sin comprimir
3. Exceso de anuncios y cookies que bloquean el contenido antes de ser aceptadas

**3. Análisis — ¿Sufre la web de "inflación de software"?**
Sí. La web presenta dependencias excesivas de JavaScript, una cantidad desproporcionada de anuncios para ser un directorio de empresas, y recursos sin optimizar. Las soluciones recomendadas son: eliminar librerías innecesarias, convertir imágenes a `.webp` y aplicar *lazy loading* para cargar recursos solo cuando el usuario los necesite.

---

### Fase 2: Dimensión Social (S)

**1. Test de accesibilidad**
Se utilizó la extensión *Lighthouse* en Chrome:

<img width="825" height="716" alt="image" src="https://github.com/user-attachments/assets/6cd4da83-4e4c-4159-bd5b-b2b2c5a90fa3" />


Los resultados muestran indicadores mayoritariamente en rojo/naranja, lo que refleja una accesibilidad deficiente.

**2. Barreras identificadas**
1. Solicitudes que bloquean el renderizado de la página
2. Política de caché ineficiente
3. Problemas con la visualización de fuentes
4. Uso de JavaScript desactualizado
5. Árbol de dependencias de red excesivo

---

### Fase 3: Dimensión de Gobernanza (G)

**1. Transparencia en cookies**
La web permite rechazar cookies no esenciales sin forzar al usuario a aceptarlas ni exigir suscripción de pago. No se detectan patrones oscuros graves en este aspecto.

**2. Datos personales solicitados**
Solo se solicita correo electrónico y contraseña. También permite acceso mediante cuenta de Google o Microsoft. No se recogen datos excesivos como dirección, código postal o teléfono.

---

### Fase 4: Propuesta de Refactorización

**Optimización de activos**
- **Formato de imágenes:** Se recomienda WebP por su alto nivel de compresión sin pérdida de calidad visible, frente a PNG o JPG.
- **Lazy Loading:** Sí se implementaría, para cargar imágenes únicamente cuando el usuario llega a esa sección de la página.

**Reducción de peticiones**
- Eliminar o aplazar las librerías y scripts de JavaScript externos, sustituyéndolos por código moderno y limpio, con el menor número posible de dependencias externas.

**Reflexión sobre la Paradoja de Jevons**
Si la optimización atrae más usuarios, el ahorro energético podría verse anulado. Para evitarlo: optimizar imágenes, desacoplar el consumo energético del volumen de visitas y alojar la web en proveedores de *hosting* verde que utilicen energías renovables.

---

## 3. Mejoras Técnicas Estructuradas

### 3.1 Mejoras Ambientales (A)

**Scripts bloqueantes**

*Antes*: cargados en el `<head>` sin atributos, deteniendo el renderizado:
```html
<script src="jquery.min.js"></script>
<script src="ue-utils.js"></script>
<script src="autosuggest.js"></script>
```

*Después*: Utilizo el atributo `defer` para indicar al navegador que descargue el archivo JavaScript en segundo plano mientras sigue leyendo el resto de la página. 

```html
<script src="jquery.min.js" defer></script>
<script src="ue-utils.js" defer></script>
<script src="autosuggest.js" defer></script>
```

---

**Viewport**

*Antes*: fijo a 1024px e impidiendo el zoom:
```html
<meta name="viewport" content="width=1024, maximum-scale=1.0"/>
```

*Después*: adaptable a cualquier dispositivo:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

---

**Imágenes**

*Antes*: PNG sin optimizar y sin carga diferida:

```html
<img src="logo_expansion_noticia.png" alt="expansion.com"/>
<img src="powered_by_axesor.png">
```

*Después*: WebP con lazy loading:
```html
<picture>
  <source srcset="logo_expansion.webp" type="image/webp">
  <img src="logo_expansion.png" alt="Expansión" loading="lazy" width="200" height="50">
</picture>

<picture>
  <source srcset="powered_by_axesor.webp" type="image/webp">
  <img src="powered_by_axesor.png" alt="Información proporcionada por Axesor" loading="lazy" width="120" height="30">
</picture>
```

---

### 3.2 Mejoras Sociales (S)

**Doble `<h1>`**

*Antes*: el logo y el título de la empresa eran ambos `<h1>`:
```html
<h1>
  <a href="https://www.expansion.com/">
    <img alt="expansion.com" src="logo_expansion_noticia.png"/>
  </a>
</h1>
<h1>ACEITUNEROS DE SALTERAS SL</h1>
```

*Después*: el logo pasa a `<div>` y solo queda un `<h1>`:
```html
<div class="logo">
  <a href="https://www.expansion.com/">
    <img alt="Expansión" src="logo_expansion_noticia.png"/>
  </a>
</div>
<h1>Aceituneros de Salteras SL</h1>
```

---

**Formulario con etiquetas desvinculadas**

*Antes*: etiquetas no asociadas a sus campos y placeholder falso:
```html
<label>Nombre</label>
<input type="text" id="dr_nombre">

<label>C.I.F.</label>
<input type="text" id="dr_cif">

<input type="text" value="Introduzca nombre..." onclick="this.value=''"/>
```

*Después*: cada etiqueta vinculada a su campo y placeholder real:
```html
<label for="dr_nombre">Nombre</label>
<input type="text" id="dr_nombre">

<label for="dr_cif">C.I.F.</label>
<input type="text" id="dr_cif">

<input type="text" placeholder="Introduzca nombre..." aria-label="Buscar empresa"/>
```

---

### 3.3 Mejoras de Gobernanza (G)

**Código PHP visible en el HTML**

*Antes*: el código PHP aparecía sin ejecutar en el pie de página:
```html
<p>© <?php echo date('Y'); ?> Unidad Editorial</p>
<?php @include_once($static_path.'assets.php.inc'); ?>
```

*Después*: el servidor lo procesa correctamente antes de enviarlo al navegador:
```html
<p>© 2026 Unidad Editorial</p>
```
## Bibliografía (Estilo IEEE)

[1] Axarnet, “Refactorización de código: qué es y por qué es importante,” *Axarnet Blog*. Disponible en: https://axarnet.es/blog/refactorizacion-codigo. [Accedido: 20-may-2026].

[2] Mozilla Developer Network (MDN), “::placeholder,” *MDN Web Docs*. Disponible en: https://developer.mozilla.org/es/docs/Web/CSS/Reference/Selectors/::placeholder. [Accedido: 20-may-2026].

[3] Mozilla Developer Network (MDN), “viewport,” *MDN Web Docs*. Disponible en: https://developer.mozilla.org/es/docs/Web/HTML/Reference/Elements/meta/name/viewport. [Accedido: 20-may-2026].

[4] W3Schools, “HTML script defer Attribute,” *W3Schools*. Disponible en: https://www.w3schools.com/tags/att_script_defer.asp. [Accedido: 20-may-2026].
