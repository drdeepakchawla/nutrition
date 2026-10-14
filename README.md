# Enteral Nutrient Calculator

A web version of the unit's "Enteral Nutrient calculation" Google Sheet. Residents enter a preterm infant's weight, human milk volume and the day's fortifier and supplements. The calculator shows daily intake of fluid, energy, protein, iron, vitamins D and A, zinc and calcium, and checks each one against ESPGHAN recommendations.

The whole tool is one file, `index.html`. It needs no server-side code, database or build step.

## Features

- Results update as you type, with a colour-coded status for each nutrient: **below** (blue), **within** (green) or **above** (red). Each status also has a text label and an arrow, so it can be read without relying on colour.
- A range bar for each nutrient shows how far the intake is from target, e.g. "14 short of 115".
- Targets can be switched between **ESPGHAN 2022** (default) and **ESPGHAN 2010** (the targets used in the original sheet).
- **Download PDF** produces a one-page A4 report with patient details, prescription, results, contribution by source, and the reference used. The file is named after the CR number and date.
- **Print** gives a compact printable layout.
- Shows total milk per day, and the volume per feed for 2-hourly and 3-hourly feeds.
- Catches common input errors, such as weight entered in grams or a very high milk volume.
- Includes a contribution-by-source table and the composition values used.
- Works on phones and supports dark mode.
- Privacy: all calculations run in the browser. Names and CR numbers are not saved or sent anywhere.

## Hosting

### Option 1: GitHub Pages (free, from this repository)

1. On GitHub, open the repository's **Settings → Pages**.
2. Under **Build and deployment**, choose **Deploy from a branch**. Select the branch that contains `index.html` and the `/ (root)` folder, then save.
3. After a minute the calculator is live at `https://drdeepakchawla.github.io/nutrition/`.

### Option 2: your own website

Upload `index.html` to your web host, renaming it if you like (e.g. `enteral-calculator.html`).

### Option 3: embed in an existing page (WordPress, Google Sites, Wix, etc.)

Once the calculator is hosted (e.g. on GitHub Pages), add it to any page with an embed or "Custom HTML" block:

```html
<iframe src="https://drdeepakchawla.github.io/nutrition/"
        title="Enteral Nutrient Calculator"
        style="width:100%; height:1500px; border:0;"></iframe>
```

The PDF library (jsPDF) and fonts load from public CDNs (cdnjs.cloudflare.com and Google Fonts). If a hospital network blocks these sites, the calculator still works. The fonts fall back to system fonts, and the Download PDF button asks the user to use Print → Save as PDF instead.

## Updating values

All numbers are in the configuration block near the top of the `<script>` in `index.html`:

- `NUTRIENTS`: the composition of each product (per 100 ml human milk, per g HMF, per ml MCT, MVI, vitamin D drops, calcium-phosphorus syrup, and per drop of iron). Update these if the unit changes brand of fortifier or supplement.
- `STANDARDS`: the recommended ranges for ESPGHAN 2022 and 2010.
- `EXAMPLE`: the values used by the "Load example" button (the values from the original sheet).

## Recommendations used

| Nutrient | ESPGHAN 2010 (original sheet) | ESPGHAN 2022 |
|---|---|---|
| Fluid, ml/kg/day | 135–200 | 150–180 (135–200) |
| Energy, kcal/kg/day | 110–135 | 115–140 (up to 160) |
| Protein, g/kg/day | 3.5–4.5 | 3.5–4.0 (up to 4.5) |
| Protein:energy ratio, g/100 kcal | – | 2.8–3.6 |
| Iron, mg/kg/day | 2–3 | 2–3 (up to 6) |
| Vitamin D | 800–1000 **IU/day** | 400–700 **IU/kg/day**, max 1000 IU/day |
| Vitamin A, µg RE/kg/day | 400–1000 | 400–1000 |
| Zinc, mg/kg/day | 1.1–2.0 | **2.0–3.0** |
| Calcium, mg/kg/day | 120–140 | **120–200** |

References: Embleton ND, et al. *Enteral nutrition in preterm infants (2022): a position paper from the ESPGHAN Committee on Nutrition and invited experts.* JPGN 2023;76:248–68 (doi:10.1097/MPG.0000000000003642). Agostoni C, et al. *Enteral nutrient supply for preterm infants.* JPGN 2010;50:85–91.

## Differences from the original sheet

The calculator uses the sheet's composition values and formulas, with these corrections:

1. **Vitamin D from human milk** was calculated per kg (`B31*B4/100`) but added to a per-day total. The calculator multiplies it by weight.
2. **Vitamin D from calcium-phosphorus syrup** was divided by weight (`H31*B10/B3`), although the vitamin D row is per day. The calculator uses the full daily amount. For the sheet's example (1.254 kg, 1 ml vitamin D, 4.5 ml syrup), vitamin D is 584 IU/day instead of 547 IU/day.
3. **Zinc above range** was shaded light green in the sheet. Here it shows as "above", in red, like every other nutrient.

All other results match the sheet. Status is judged on the rounded value shown on screen.
