# Arizona Crop Weather

Weather decision tools for Arizona cotton and alfalfa, built on AZMet data with S2S outlooks (WKU prototype).

**Live site:** https://manmeet3591.github.io/arizona-crop-weather/

Each tool runs a published crop model on observed station weather, then carries it forward with a 16-day forecast and a 45-day ECMWF IFS sub-seasonal (S2S) ensemble outlook.

| Tool | Model and thresholds |
|---|---|
| Cotton heat units | 86/55°F HUAP; stage benchmarks and heat-stress levels as used by AZMet |
| Alfalfa water use | AZMet Penman-Monteith ETo × FAO-56 alfalfa Kc for individual cutting cycles |

## How it works

- `index.html` is the whole app: plain HTML, CSS and JavaScript, no build step.
- `data/data.txt` holds the weather it reads. A GitHub Action runs `scripts/update_data.py` (with `REGION=az`) every morning and commits fresh data from the [AZMet API](https://azmet.arizona.edu/) for Arizona stations (Maricopa, Coolidge, Buckeye, Paloma, Safford, Yuma Valley) and [Open-Meteo](https://open-meteo.com/) forecast and seasonal ensemble APIs.
- To run locally: `pip install requests && REGION=az python scripts/update_data.py`, then `python -m http.server` and open `http://localhost:8000`.

## Forecast sources

- Days 1–15: Open-Meteo forecast API.
- Beyond day 15, out to day 45: the **ECMWF IFS extended-range (sub-seasonal) ensemble, EC46**, 51 members (control + 50), requested with `models=ecmwf_ec46` from the Open-Meteo seasonal API. ECMWF runs it daily. Data CC BY 4.0.

To swap in a different S2S source (for example the WKU AI model), replace `seasonal()` in `scripts/update_data.py` with output in the same shape: `S0`…`S50` lines of daily mean °F per member and `SMED` lines of the ensemble median (plus ET0 in inches for Arizona).

## Status

Research prototype from Western Kentucky University; not an official Extension product.
