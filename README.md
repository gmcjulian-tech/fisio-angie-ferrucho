# Software de fisioterapia · Angie Ferrucho

Aplicación web para gestionar pacientes, historia clínica con cuestionario detox, agenda semanal de citas (consultorio y domicilio), sesiones realizadas y catálogo de servicios.

- Sitio estático: se publica gratis con GitHub Pages.
- Datos: Supabase (PostgreSQL + inicio de sesión). Cada fila queda protegida con seguridad por filas (RLS): solo la cuenta de Angie puede leerla o modificarla.
- Sin configurar Supabase, la aplicación abre en **modo demostración** con datos ficticios guardados solo en el navegador.

## Puesta en marcha

### 1. Supabase
1. Crea un proyecto en <https://supabase.com> (región sugerida: la más cercana, por ejemplo São Paulo).
2. **SQL Editor → New query**: pega el contenido de `supabase/schema.sql` y pulsa **Run**.
3. **Authentication → Sign In / Providers**: desactiva **Allow new users to sign up**, para que nadie más pueda crear una cuenta.
4. **Authentication → Users → Add user → Create new user**: crea el usuario de Angie con su correo. La contraseña la elige ella; no la compartas por chat ni la guardes en el repositorio.
5. **Project Settings → API**: copia la **Project URL** y la clave pública (**anon** o **publishable**).

### 2. Configuración
Edita `config.js` y pega los dos valores:

```js
window.FISIO_CONFIG = {
  supabaseUrl: 'https://xxxx.supabase.co',
  supabaseAnonKey: 'eyJ...'
};
```

La clave pública puede ir en el repositorio: está diseñada para usarse en el navegador y la protección la dan las políticas RLS. **Nunca** pongas la clave `service_role` o `secret`.

### 3. GitHub Pages
**Settings → Pages → Build and deployment → Deploy from a branch → `main` / `(root)` → Save.** En uno o dos minutos el sitio queda en `https://<usuario>.github.io/<repositorio>/`.

## Estructura

```
index.html            Aplicación generada (no editar a mano)
config.js             URL y clave pública de Supabase
assets/               Ilustración y estilos
supabase/schema.sql   Tablas y seguridad de la base de datos
src/screens/          Plantilla (.html) y lógica (.js) de cada pantalla
src/db.js             Acceso a datos (Supabase o modo demostración)
src/runtime.js        Motor que dibuja las pantallas
src/build.py          Regenera index.html:  python3 src/build.py
```

## Reglas clínicas implementadas
- Edad calculada desde la fecha de nacimiento; IMC = peso ÷ estatura² con su clasificación.
- Cuestionario detox: 8 sistemas, escala 0–4 por síntoma, total por sistema y puntaje global.
- Nivel de toxicidad: menos de 30 bajo, 30 a 60 moderado, más de 60 alto.
- Ranking de sistemas, síntomas con mayor puntaje y un objetivo terapéutico editable por cada sistema con más de 5 puntos.
- Si se puntúa ideación suicida, la aplicación muestra una alerta de remisión prioritaria a salud mental.

## Pendiente
- Consentimiento informado con firma en pantalla (a la espera del formato).
- Copias de seguridad: los planes gratuitos de Supabase no incluyen respaldos diarios descargables; conviene exportar la base de datos periódicamente.
