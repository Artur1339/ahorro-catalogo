# Catálogo remoto

Con este catálogo puedes poner al día las rentabilidades de cuentas, depósitos y fondos sin
reinstalar la app: editas un fichero en GitHub y la app lo descarga sola una vez al día.

El fichero `catalog.json` de esta carpeta es una copia exacta del catálogo que lleva la app
dentro. No contiene nada tuyo: solo productos públicos, sus rentabilidades y la fecha en que
se comprobaron.

## Ponerlo en marcha (una sola vez)

1. Entra en https://github.com y crea un repositorio nuevo. Tiene que ser **público**: la app
   lo descarga sin contraseña. Un nombre posible: `ahorro-catalogo`.
2. En el repositorio, pulsa **Add file > Upload files** y sube el `catalog.json` de esta carpeta.
3. Abre el fichero en GitHub y pulsa **Raw**. Copia la dirección de la barra del navegador.
   Tendrá esta forma:

   ```
   https://raw.githubusercontent.com/TU_USUARIO/ahorro-catalogo/main/catalog.json
   ```

4. La app ya trae puesta esta dirección:

   ```
   https://raw.githubusercontent.com/Artur1339/ahorro-catalogo/main/catalog.json
   ```

   Si el repositorio se llama así, no hay que hacer nada más. Si usas otro nombre o
   quieres desactivarlo, se cambia en **Ajustes > Mejora mínima para cambiar** (la pantalla
   «Reglas de aviso»), en **Dirección del catálogo remoto**.

A partir de ahí, la app descarga el catálogo una vez al día y al pulsar «Actualizar ahora».
Si un día no hay conexión o el fichero tiene un error, sigue con lo que tenía. El estado se
ve en **Ajustes > Datos automáticos > Catálogo de productos**.

## Actualizar un dato

1. En GitHub, abre `catalog.json` y pulsa el lápiz (**Edit**).
2. Cambia el dato del producto. Por ejemplo, si una cuenta pasa del 2,27 % al 2,10 %:

   ```json
   "annualYieldPercent": "2.10",
   "verifiedOn": "2026-12-01",
   ```

   - Los porcentajes van **entre comillas y con punto**: `"2.10"`, no `2,10`.
   - `verifiedOn` es el día en que lo has comprobado tú en la web del banco (año-mes-día).
     Cámbialo siempre que revises un dato, aunque el número no haya cambiado: pasados 30
     días sin confirmar, la app deja de usar ese producto para recomendar cambios.
3. Cambia también `updatedOn`, al principio del fichero, por la fecha de hoy.
4. Pulsa **Commit changes**.

## Reglas que conviene conocer

- **Gana el dato comprobado más recientemente.** Si editas un producto a mano en la app y
  después el catálogo remoto trae una fecha posterior, se usa el del catálogo.
- **Las Letras del Tesoro no hace falta tocarlas:** la app las pone al día sola con el
  resultado de la última subasta.
- **Para añadir un producto**, copia el bloque de uno parecido, cámbiale el `id` (único, sin
  espacios) y los datos. **Para quitarlo**, borra su bloque: la app lo retira, salvo que
  tengas dinero apuntado en él.
- **Si te equivocas con una coma o una llave**, la app rechaza el fichero entero y no cambia
  nada. Puedes comprobarlo antes pegando el contenido en https://jsonlint.com.
- Un producto con un dato imposible (por ejemplo, una rentabilidad del 250 %) se descarta
  sin afectar a los demás.

## Campos de cada producto

| Campo | Qué es |
| --- | --- |
| `id` | Identificador único. No lo cambies una vez creado. |
| `type` | `SAVINGS_ACCOUNT`, `DEPOSIT`, `TREASURY_BILL`, `MONEY_MARKET_FUND`, `SHORT_TERM_BOND_FUND`, `INDEX_FUND` o `INDEX_ETF`. |
| `name`, `entity` | Nombre del producto y del banco o gestora. |
| `annualYieldPercent` | TAE o rentabilidad anual. En los indexados, `"0"`. |
| `annualFeePercent` | Gastos anuales del fondo. Opcional. |
| `liquidity` | `IMMEDIATE` (al momento), `WITHIN_DAYS` (unos días) o `AT_MATURITY` (al vencer). |
| `termMonths` | Plazo en meses, para depósitos y letras. |
| `riskLevel` | Del 1 (mínimo) al 7. |
| `transferable` | `true` si es un fondo que se puede traspasar sin tributar. |
| `minimumAmountEuros`, `maxRemuneratedBalanceEuros` | Mínimo para contratar y saldo máximo con intereses. Opcionales. |
| `promo` | Rentabilidad de bienvenida: `{"ratePercent": "3.00", "endsOn": "2027-01-31"}`. Opcional. |
| `hasRequirements` | `true` si exige nómina, recibos o ser cliente nuevo. |
| `isin`, `priceSymbol` | Código del fondo y su símbolo en Yahoo Finance (en euros). |
| `verifiedOn` | Día en que se comprobó el dato. Sin él, el producto queda «sin confirmar». |
| `notes`, `sourceUrl` | Letra pequeña y página donde comprobarlo. |
