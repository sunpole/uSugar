# OCR Reality Report

Date: 2026-06-22
Release: uSugar 1.5.5

This audit measures the real OCR behavior on the local safe sample folders. The smoke script does not write to SQLite, does not create glucose_log rows, and does not save OCR attempts.

## Summary

| Source group | Reality | Short conclusion |
|---|---|---|
| `img/simple` | partial | 4/11 accepted, 7/11 manual, 0/11 no_result. Old Libre2 CV remains the best current source, but some frames still need manual confirmation because helper branches disagree. |
| `img/simple_new` | improved partial | After 1.5.4: 1/18 accepted, 9/18 manual, 8/18 no_result. The 1.5.3 landscape ROI still finds more right-side Libre2 values, and 1.5.4 did not weaken that path. |
| `img/simple_gluk` | improved partial, manual only | Before 1.5.4: 0/27 accepted, 8/27 manual, 19/27 no_result. After 1.5.4: 0/27 accepted, 18/27 manual, 9/27 no_result. Glucometer candidates still require manual checking. |

## Safety Findings

- OCR still does not save sugar automatically. Every candidate requires explicit user confirmation before saving.
- Disagreement between sources is treated as manual confirmation, not as averaging.
- Glucometer photo candidates are low-confidence/manual by default until the digit recognition is improved.
- The current smoke is diagnostic only; it should not be used as proof that the photo OCR is production-ready.
- Version 1.5.5 adds an OCR mode selector. It changes source priority, not medical trust: selected new Libre2 and glucometer candidates still require manual confirmation.

## 1.5.5 OCR Mode Selector

The OCR mode selector stores `ocr_mode` in the existing user protocol JSON. Modes:

- `auto` - current source-aware automatic order;
- `libre2_old` - prefer the older Libre2 CV source first;
- `libre2_new` - prefer the updated landscape/narrow Libre2 source first;
- `glucometer` - prefer the manual-glucometer photo source first.

The selector is available through `/ocr`, typed phrases (`старый либре`, `новый либре`, `глюкометр`, `авто`), and Settings WebApp. Other OCR sources still run as diagnostics/backup, and the bot still asks the user to confirm before saving any OCR value.

## 1.5.4 Glucometer Photo Tuning

The real files in `img/simple_gluk` are camera photos of a handheld glucometer. Typical problems:

- small display area inside a much larger phone photo;
- textile/background noise around the device;
- focus and motion blur in several frames;
- glare/reflections on the LCD screen;
- tilted or rotated device angles;
- low-contrast seven-segment digits rather than app-style text.

Version 1.5.4 keeps the existing `glucometer_photo` source and adds a fallback display search with several ROIs (`display_roi_upper`, `display_roi_tight`, `display_roi_center`), contrast/equalize/sharpen preprocessing, upscaling, and small rotation tolerance. These candidates are intentionally low-trust: the bot can show a possible value, but saving still requires explicit user confirmation.

| Folder | Before 1.5.4 | After 1.5.4 | Change |
|---|---:|---:|---|
| `img/simple_gluk` accepted | 0/27 | 0/27 | unchanged by design |
| `img/simple_gluk` manual | 8/27 | 18/27 | +10 manual candidates |
| `img/simple_gluk` no_result | 19/27 | 9/27 | -10 no-result frames |
| `img/simple` | 4 accepted, 7 manual, 0 no_result | 4 accepted, 7 manual, 0 no_result | no regression |
| `img/simple_new` | 1 accepted, 8 manual, 9 no_result | 1 accepted, 9 manual, 8 no_result | no regression |

Important: the improvement is coverage, not medical trust. Glucometer photo OCR can still be wrong because photos are blurred, angled, and low-contrast. Treat all `glucometer_photo` output as a manual candidate.

## 1.5.3 New Libre2 Geometry

The real files in `img/simple_new` are landscape screenshots with size `1280x576`, not the earlier synthetic vertical narrow shape. The visible current glucose value is usually in the right side of the screenshot:

- typical ROI: `x=55-98%`, `y=5-85%`;
- value examples found in the right ROI: `10.1`, `14.6`, `14.1`, `11.0`, `15.6`, `5.1`, `10.2`;
- full-history screenshots without a large current value can still correctly remain `no_result`.

The 1.5.3 tuning adds `libre2_narrow_updated:narrow_landscape_right_value`. It converts component strings such as `1.4.1` into `14.1` only inside this Libre2 landscape ROI. These results are intentionally treated as manual confirmation candidates, not autosave-ready values.

| Folder | Before 1.5.3 | After 1.5.3 | Change |
|---|---:|---:|---|
| `img/simple_new` accepted | 1/18 | 1/18 | unchanged |
| `img/simple_new` manual | 3/18 | 8/18 | +5 useful manual candidates |
| `img/simple_new` no_result | 14/18 | 9/18 | -5 no-result frames |
| `img/simple` | 4 accepted, 7 manual, 0 no_result | 4 accepted, 7 manual, 0 no_result | no regression |
| `img/simple_gluk` | 0 accepted, 8 manual, 19 no_result | 0 accepted, 8 manual, 19 no_result | no regression |

### 1.5.3 `img/simple_new` After Tuning

| File | Source | Candidate | Confidence | Status |
|---|---|---:|---|---|
| `0f4e1786-6c63-4e05-9e92-1e202576c0e9.jfif` | - | - | none | no_result |
| `34f749eb-98c2-4560-9052-7cbe1059069f.jfif` | - | - | none | no_result |
| `3f524d68-83af-464e-b2ae-a24191d35a50.jfif` | - | - | none | no_result |
| `59847551-71dc-48da-b6da-c587f4fc564f.jfif` | libre2_narrow_updated | 10.1 | low | needs_confirmation |
| `5a94cba2-f21a-49c1-bdd6-95403c58d97e.jfif` | libre2_narrow_updated | 14.6 | low | needs_confirmation |
| `659e37c7-bee2-4677-b6a4-50f8a12926c6.jfif` | libre2_narrow_updated | 14.1 | low | needs_confirmation |
| `74670552-5a4c-4390-a912-167e3528fe6e.jfif` | libre2_narrow_updated | 11.0 | low | needs_confirmation |
| `8873145a-715f-40e8-a7ad-121fc197cbac.jfif` | - | - | none | no_result |
| `8ba8defb-fb0c-4b7e-ab5b-4522e5b403a3.jfif` | libre2_narrow_updated | 15.6 | low | needs_confirmation |
| `a052ef26-61d4-475f-9832-81a99bad8de6.jfif` | - | - | none | no_result |
| `a9b1e19a-da93-4fcc-b504-ca90b1ecec87.jfif` | libre2_narrow_updated | 14.3 | medium | needs_confirmation |
| `b7706034-355e-4a5f-82c7-cc7a340515f0.jfif` | libre2_cv_old | 16.3 | high | accepted |
| `b98977de-fa32-4379-bd3d-b2cda84f2e53.jfif` | - | - | none | no_result |
| `c2f2cb0d-3f85-4f4b-af3c-4be130c0bb6c.jfif` | libre2_narrow_updated | 5.1 | low | needs_confirmation |
| `c3c9bd61-690c-4ca7-8941-010947f98bcc.jfif` | - | - | none | no_result |
| `d905ff35-59be-49da-9d24-2989e7a9e8eb.jfif` | - | - | none | no_result |
| `f43139a3-bdb2-4c74-a829-88db7b78f54d.jfif` | - | - | none | no_result |
| `photo_5289843234458771065_y.jpg` | libre2_narrow_updated | 10.2 | medium | needs_confirmation |

## 1.5.2 Baseline File Results

This table is kept as the original 1.5.2 baseline. The updated 1.5.3 results for `img/simple_new` are listed above.

| Folder | File | Detected source | Candidate | Confidence | Status | Reason |
|---|---|---|---:|---|---|---|
| `img/simple` | `photo_2026-05-29_11-33-55.jpg` | libre2_cv_old | 14.0 | medium | needs_confirmation | manual confirmation required; libre2_cv_old:candidate:14.0; libre2_narrow_updated:candidate:7.0; glucometer_photo:low_confidence:none; tesseract:error:none |
| `img/simple` | `photo_2026-05-29_11-34-20.jpg` | libre2_cv_old | 18.6 | medium | needs_confirmation | manual confirmation required; libre2_cv_old:candidate:18.6; libre2_narrow_updated:candidate:1.86; glucometer_photo:candidate:1.86; tesseract:error:none |
| `img/simple` | `photo_2026-05-29_11-34-24.jpg` | libre2_cv_old | 23.0 | medium | needs_confirmation | manual confirmation required; libre2_cv_old:candidate:23.0; libre2_narrow_updated:no_result:none; glucometer_photo:candidate:2.31; tesseract:error:none |
| `img/simple` | `photo_2026-05-29_11-34-28.jpg` | libre2_cv_old | 6.9 | medium | accepted | candidate accepted for user confirmation; libre2_cv_old:candidate:6.9; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple` | `photo_2026-05-29_11-34-31.jpg` | libre2_cv_old | 16.4 | medium | needs_confirmation | manual confirmation required; libre2_cv_old:candidate:16.4; libre2_narrow_updated:candidate:1.6,4.0; glucometer_photo:low_confidence:none; tesseract:error:none |
| `img/simple` | `photo_2026-05-29_11-34-35.jpg` | libre2_cv_old | 17.5 | medium | needs_confirmation | manual confirmation required; libre2_cv_old:candidate:17.5; libre2_narrow_updated:candidate:1.7,5.0; glucometer_photo:low_confidence:none; tesseract:error:none |
| `img/simple` | `photo_2026-05-29_11-34-38.jpg` | libre2_cv_old | 16.3 | medium | needs_confirmation | manual confirmation required; libre2_cv_old:candidate:16.3; libre2_narrow_updated:candidate:1.6,3.0; glucometer_photo:low_confidence:none; tesseract:error:none |
| `img/simple` | `photo_2026-05-29_11-34-41.jpg` | libre2_cv_old | 9.4 | high | accepted | candidate accepted for user confirmation; libre2_cv_old:candidate:9.4; libre2_narrow_updated:candidate:4.0; glucometer_photo:candidate:9.4; tesseract:error:none |
| `img/simple` | `photo_2026-05-29_11-34-44.jpg` | libre2_cv_old | 6.9 | medium | accepted | candidate accepted for user confirmation; libre2_cv_old:candidate:6.9; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple` | `photo_2026-05-29_11-34-47.jpg` | libre2_cv_old | 7.6 | medium | accepted | candidate accepted for user confirmation; libre2_cv_old:candidate:7.6; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple` | `photo_2026-05-29_11-34-51.jpg` | libre2_cv_old | 12.6 | medium | needs_confirmation | manual confirmation required; libre2_cv_old:candidate:12.6; libre2_narrow_updated:no_result:none; glucometer_photo:candidate:4.0; tesseract:error:none |
| `img/simple_new` | `0f4e1786-6c63-4e05-9e92-1e202576c0e9.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `34f749eb-98c2-4560-9052-7cbe1059069f.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `3f524d68-83af-464e-b2ae-a24191d35a50.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `59847551-71dc-48da-b6da-c587f4fc564f.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `5a94cba2-f21a-49c1-bdd6-95403c58d97e.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `659e37c7-bee2-4677-b6a4-50f8a12926c6.jfif` | glucometer_photo | 4.0 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:candidate:4.0; tesseract:error:none |
| `img/simple_new` | `74670552-5a4c-4390-a912-167e3528fe6e.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `8873145a-715f-40e8-a7ad-121fc197cbac.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `8ba8defb-fb0c-4b7e-ab5b-4522e5b403a3.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `a052ef26-61d4-475f-9832-81a99bad8de6.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `a9b1e19a-da93-4fcc-b504-ca90b1ecec87.jfif` | glucometer_photo | 14.0 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:candidate:14.0; tesseract:error:none |
| `img/simple_new` | `b7706034-355e-4a5f-82c7-cc7a340515f0.jfif` | libre2_cv_old | 16.4 | high | accepted | candidate accepted for user confirmation; libre2_cv_old:candidate:16.7; libre2_narrow_updated:no_result:none; glucometer_photo:candidate:16.0; tesseract:error:none |
| `img/simple_new` | `b98977de-fa32-4379-bd3d-b2cda84f2e53.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `c2f2cb0d-3f85-4f4b-af3c-4be130c0bb6c.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `c3c9bd61-690c-4ca7-8941-010947f98bcc.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `d905ff35-59be-49da-9d24-2989e7a9e8eb.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `f43139a3-bdb2-4c74-a829-88db7b78f54d.jfif` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_new` | `photo_5289843234458771065_y.jpg` | glucometer_photo | 10.0 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:candidate:10.0; tesseract:error:none |
| `img/simple_gluk` | `photo_5280645531929615315_y.jpg` | glucometer_photo | 11.0 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:low_confidence:11.0; tesseract:error:none |
| `img/simple_gluk` | `photo_5280645531929615363_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5280645531929615478_y.jpg` | glucometer_photo | 8.0 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:low_confidence:8.0; tesseract:error:none |
| `img/simple_gluk` | `photo_5280645531929615567_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5282897331743300875_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:low_confidence:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5282897331743300955_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5282897331743301162_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:low_confidence:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5282897331743301216_y.jpg` | glucometer_photo | 4.6 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:low_confidence:4.6; tesseract:error:none |
| `img/simple_gluk` | `photo_5282897331743301538_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5282897331743301576_y.jpg` | glucometer_photo | 2.0 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:candidate:2.0; tesseract:error:none |
| `img/simple_gluk` | `photo_5282897331743301659_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473051330_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:low_confidence:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473051404_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473051538_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473051647_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473051737_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473051840_y.jpg` | glucometer_photo | 3.9 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:low_confidence:3.9; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473051948_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473052132_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473052175_y.jpg` | glucometer_photo | 3.0 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:candidate:3.0; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473052201_y.jpg` | glucometer_photo | 8.8 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:candidate:8.8; tesseract:error:none |
| `img/simple_gluk` | `photo_5285104996473052219_y.jpg` | glucometer_photo | 9.9 | low | needs_confirmation | manual confirmation required; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:candidate:9.9; tesseract:error:none |
| `img/simple_gluk` | `photo_5287356796286736329_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:low_confidence:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5287356796286736417_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:low_confidence:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5287573150969306326_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5287573150969306626_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |
| `img/simple_gluk` | `photo_5287573150969306674_y.jpg` | - | - | none | no_result | no candidate; libre2_cv_old:no_result:none; libre2_narrow_updated:no_result:none; glucometer_photo:no_result:none; tesseract:error:none |

## Commands Used

```powershell
venv\Scripts\python.exe scripts\ocr_smoke.py --folder img/simple
venv\Scripts\python.exe scripts\ocr_smoke.py --folder img/simple_new
venv\Scripts\python.exe scripts\ocr_smoke.py --folder img/simple_gluk
```

## Next OCR Work

- Improve the narrow Libre2 source detector using real black-and-white and color updated screenshots.
- Keep the old Libre2 path, but reduce false disagreements from secondary sources.
- Build a dedicated seven-segment/display recognizer for glucometer photos before trusting them beyond manual confirmation.
- Keep OCR confirmation mandatory until the recognition quality is consistently verified.
