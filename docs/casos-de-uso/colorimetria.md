# Casos de Uso — Colorimetría

## CU-05: Generar opciones de colores (Pre-Colorimetría)

**Actor principal:** Usuario  
**Actores secundarios:** IA de Colorimetría (Gemini / n8n)

**Objetivo:**  
Generar opciones de paletas y atributos de color basados en la imagen del usuario, sin almacenarlos aún de forma definitiva.

**Precondiciones:**  
- El usuario debe estar autenticado.  
- El usuario sube una imagen válida.

**Flujo principal:**  
1. El usuario sube una imagen al endpoint `/colorimetria/generar`.  
2. La API envía la imagen al servicio de IA.  
3. La IA devuelve múltiples alternativas, incluyendo:  
   - Color de piel  
   - Tonos de pelo  
   - Color de ojos  
   - Color de cejas  
   - Estación sugerida y subestación  
4. La API no guarda datos en la base de datos.  
5. La API retorna todas las opciones generadas para que el usuario seleccione su preferencia.

**Postcondiciones:**  
- Se genera un conjunto de opciones de color sugeridas.  
- Ningún dato se almacena en la tabla `colorimetria`.  
- El sistema queda a la espera de la confirmación del usuario (CU-06).

---

## CU-06: Confirmar colorimetría definitiva (Post-Selección)

**Actor principal:** Usuario  
**Actores secundarios:** Base de Datos

**Objetivo:**  
Permitir que el usuario seleccione y confirme su colorimetría final, la cual será almacenada de forma permanente.

**Precondiciones:**  
- CU-05 debe haber generado opciones válidas.  
- El usuario debe haber seleccionado una combinación.  
- Usuario autenticado.

**Flujo principal:**  
1. El usuario envía su selección final al endpoint `/colorimetria/confirmar`.  
2. La API valida que la información corresponde a un resultado generado en CU-05.  
3. La API inserta en la tabla `colorimetria` los siguientes campos:  
   - `color_piel`  
   - `color_pelo`  
   - `color_ojos`  
   - `color_cejas`  
   - `estacion`  
   - `sub_estacion`  
   - Paleta final confirmada  
4. El sistema retorna la colorimetría final almacenada.  

**Postcondiciones:**  
- La colorimetría definitiva queda registrada en la base de datos.  
- Esta versión pasa a ser la oficial para recomendaciones, productos y outfits.

---