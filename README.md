# Arizona Crop Weather

Weather decision tools for Arizona cotton and alfalfa, built on AZMet data with S2S outlooks (WKU prototype).

**Live site:** https://manmeet3591.github.io/arizona-crop-weather/

Each tool runs a published crop model on observed station weather, then carries it forward with a 16-day forecast and a 60-day ensemble outlook. The outlook is the slot for the WKU AI sub-seasonal-to-seasonal (S2S) forecast.

| Tool | Model and thresholds |
|---|---|
| Cotton heat units | 86/55°F HUAP; stage benchmarks and heat-stress levels as used by AZMet |
| Alfalfa water use | AZMet Penman-Monteith ETo × FAO-56 alfalfa Kc for individual cutting cycles |

## How it works

- `index.html` is the whole app: plain HTML, CSS and JavaScript, no build step.
- `data/data.txt` holds the weather it reads. A GitHub Action runs `scripts/update_data.py` (with `REGION=az`) every morning and commits fresh data from the [AZMet API](https://azmet.arizona.edu/) for Arizona stations (Maricopa, Coolidge, Buckeye, Paloma, Safford, Yuma Valley) and [Open-Meteo](https://open-meteo.com/) forecast and seasonal ensemble APIs.
- To run locally: `pip install requests && REGION=az python scripts/update_data.py`, then `python -m http.server` and open `http://localhost:8000`.

## Plugging in the WKU S2S model

`scripts/update_data.py` writes the outlook as `SMED` lines (median daily max/min °F and ET0 in inches, 60 days). Replace the `seasonal()` call with the AI forecast output in the same shape and every tool picks it up.

## Status

Research prototype from Western Kentucky University; not an official Extension product.
