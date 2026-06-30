# Aural Diversity Soundscape Stimuli — FA2026

This repository contains the soundscape audio stimuli used in the perceptual listening study reported in:

> Magrath, P., Davies, W., and Gregory, S. (2026). Aural diversity in soundscape perception: Comparing ADHD, Autistic, and Neurotypical listeners on the ISO 12913 circumplex. *Proceedings of Forum Acusticum 2026*. LAURA Leverhulme Trust Aural Diversity Doctoral Research Hub, Acoustics Research Centre, University of Salford.

---

## Contents

Eleven 38-second stereo soundscape excerpts, each normalised for headphone-based web playback and evaluated using the ISO 12913-2 circumplex framework. All files are delivered as 2-channel stereo WAV (24-bit PCM, 44,100 Hz, 2067 kbps).

| # | Soundscape | Location | Recorded | LUFS-I | LRA | Character |
|---|---|---|---|---|---|---|
| 1 | Amble Sea | Links Road Dunes, Amble, Northumberland | Jan 2025 | −17.4 | 12.8 LU | Coastal surf, high dynamic variability |
| 2 | Crescent Meadow | University of Salford, UK | Feb 2025 | −16.9 | 2.7 LU | Urban greenspace, low dynamic complexity |
| 3 | Leck Stream | Leck Beck, near Cowan Bridge, Lancashire | Dec 2024 | −18.0 | 2.1 LU | Rural stream, stable and tranquil |
| 4 | Peel Park Aircraft | Peel Park, Salford | Mar 2025 | −18.0 | 7.0 LU | Urban park with overhead aircraft transient |
| 5 | Peel Park Bridge – Bin & Bike | Peel Park, Salford | Mar 2025 | −17.8 | 12.4 LU | Urban park, high dynamic variability, motorbike transient |
| 6 | Peel Park Jogger Ducks | Irwell Sculpture Trail, Peel Park, Salford | Mar 2025 | −17.4 | 2.6 LU | Urban greenspace, jogger and avian activity |
| 7 | Robin Wood's Rock | Northumberland Coast National Landscape | Jan 2025 | −18.3 | 12.9 LU | Remote coastal, wind and seabirds |
| 8 | Samyeling River | Samye Ling Temple Grounds, Eskdalemuir, Scotland | Jan 2025 | −17.8 | 2.2 LU | River, minimal anthropogenic content |
| 9 | Samyeling Stupa Pond Ducks | Samye Ling Temple Grounds, Eskdalemuir, Scotland | Jan 2025 | −17.4 | 7.1 LU | Pond, duck vocalisations and water texture |
| 10 | Seahouses Harbour | Seahouses Harbour, Northumberland | Jan 2025 | −16.9 | 3.9 LU | Maritime harbour, human and marine activity |
| 11 | wlk_stp2_1 | Near Crescent Meadow, University of Salford | 2025 | −18.0 | 5.3 LU | Urban greenspace, low-level ambient texture |

> **Note:** this repository contains eleven audio files. Ten soundscapes (all listed above except `wlk_stp2_1`) were rated by participants and form the analysed dataset reported in the FA2026 paper (`wlk1_stp2` in the analysis data corresponds to this recording site). `wlk_stp2_1` is an additional recording from the same location, included here for completeness but not part of the rated study.

---

## Recording & Processing

### Ambisonic Decoding

B-format recordings were decoded to binaural stereo using the **ambix_binaural_o1** plugin (Kronlachner, 2014) in **REAPER 7.35**, employing the `icosahedron-3h3v` HRTF preset. This first-order UHJ rendering method follows Gerzon's (1992) original formulation and has been validated for headphone-based soundscape evaluation by Stevens et al. (2017).

### Loudness Normalisation

All stimuli were normalised to a target of **−18 LUFS ±0.5** integrated loudness with a true peak ceiling of **−1.0 dBTP** (−1.2 dBTP for Peel Park Bridge–Bin & Bike and Seahouses Harbour), following **ITU-R BS.1770-4** and **AES TD1004.1.15-10** (Cox & Katz, 2015). Gain adjustment was applied via JS: Volume Adjustment and limiting via JS: Master Limiter. Metering was performed with Youlean Loudness Meter 2.

### Output Format

| Parameter | Value |
|---|---|
| Format | WAV, 24-bit PCM |
| Sample rate | 44,100 Hz |
| Channels | 2 (stereo, binaural) |
| Bit rate | 2067 kbps |
| Duration | 0:38.000 per stimulus |
| Loudness target | −18 LUFS ±0.5 (ITU-R BS.1770-4) |
| True peak ceiling | −1.0 dBTP |

---

## Soundscape Documentation

Detailed processing notes, loudness metrics, REAPER render analysis, and ISO 12913-2 circumplex response summaries for each soundscape are provided in the individual PDF documentation files in this repository.

---

## Analysis Code

The perceptual data collected in this study are analysed using the Jupyter notebook `soundscape_analysis_v8.ipynb`, included in this repository. The notebook reproduces all figures and statistical results reported in the FA2026 paper.

### Requirements

```
python >= 3.10
pandas
numpy
scipy
matplotlib
seaborn
plotly
soundscapy
```

Install dependencies with:

```bash
pip install pandas numpy scipy matplotlib seaborn plotly soundscapy
```

The notebook also uses a local development copy of the [circumplex](https://github.com/MitchellAcoustics/circumplex) package (Mitchell et al., 2024). Clone it to `~/Desktop/circumplex-dev` or update the path in Cell 1 to match your local installation.

### Data files required

Place the following CSV files in the same directory as the notebook before running. The filenames below reflect the balanced 30-per-group design used in the FA2026 study; if you are running the notebook with a different sample, rename your files to match or update the filenames in Cell 5 (Load Data) to point to your own CSVs.

| File | Contents |
|---|---|
| `ADHD_Group_XY_n30.csv` | Circumplex click coordinates, ADHD group |
| `Autistic_Group_XY_n30.csv` | Circumplex click coordinates, Autistic group |
| `Neurotypical_Group_XY_n30.csv` | Circumplex click coordinates, Neurotypical group |

Each CSV must contain at minimum the columns `Participant Public ID`, `Soundscape`, `Circumplex X`, and `Circumplex Y`. Groups do not need to be equal in size; the notebook's non-parametric tests (Kruskal–Wallis, Mann–Whitney) are valid for unequal n. If you add or remove soundscapes, no changes to the notebook are needed — the per-soundscape analysis loops over whatever soundscape labels are present in the data.

### Y-axis correction

Gorilla Experiment Builder records the circumplex Y coordinate with the axis inverted (Y = 0 at the top, Eventful; Y = 1 at the bottom, Uneventful). The ISO 12913-2 circumplex places Eventful at positive Y. The notebook applies the correction `ISOEventful = 1 − Circumplex Y` throughout. A widget calibration transform is also applied to account for the offset between click position and circumplex centre in the Gorilla interface:

```
ISOPleasant = (Circumplex X − 0.516) / 0.300
ISOEventful = (0.507 − Circumplex Y) / 0.307
```

All inferential statistics are invariant to this transform.

### Running the notebook

Open the notebook in Jupyter and run cells in order from top to bottom, or use **Run All**. The final export cell saves all outputs to the working directory. An `exports/` subdirectory is created automatically for the 600 dpi figure exports.

### Outputs

Running the full notebook generates:

| Output | Description |
|---|---|
| `circumplex_scatter_all_groups.png` | Main scatter plot, all groups overlaid |
| `circumplex_density_overlaid.png` | Kernel density estimates, all groups overlaid |
| `radial_distribution_by_group.png` | Response extremity (radial distance from origin) by group |
| `Heatmap_Combined.png` | Group-specific Gaussian KDE density heatmaps |
| `MSN_fits_by_group.png` | Multivariate skew-normal distribution fits, all groups |
| `per_soundscape_eventful_medians.png` | Median ISOEventful per soundscape by group |
| `per_soundscape_eventful_gap.png` | ADHD–NT eventfulness gap, forest plot |
| `circumplex_interactive.html` | Interactive Plotly circumplex (click to isolate groups) |
| `pooled_stats.csv` | Pooled Kruskal–Wallis and Mann–Whitney results across all soundscapes |
| `per_soundscape_stats.csv` | Per-soundscape statistical results with Benjamini–Hochberg FDR correction |
| `SPI_ADHD_vs_NT.csv` | Soundscape Perception Index, ADHD versus Neurotypical, per soundscape |
| `SPI_Autistic_vs_NT.csv` | Soundscape Perception Index, Autistic versus Neurotypical, per soundscape |
| `SPI_ADHD_vs_Autistic.csv` | Soundscape Perception Index, ADHD versus Autistic, per soundscape |
| `MSN_Parameters.csv` | Fitted multivariate skew-normal parameters for each group |
| `Soundscape_Means_by_Group.csv` | Per-soundscape mean ISOPleasant and ISOEventful by group |
| `Quadrant_Distribution.csv` | Percentage of responses in each circumplex quadrant by group |
| `all_data_corrected.csv` | Full corrected and calibrated dataset (all groups, all soundscapes) |

---

## ISO 12913-2 Framework

Soundscape quality was assessed using the **ISO 12913-2:2018** circumplex model, characterising each stimulus along two orthogonal perceptual dimensions:

- **Pleasantness** — ranging from annoying to pleasant
- **Eventfulness** — ranging from uneventful to eventful

The eight ISO 12913-2 attributes (vibrant, chaotic, monotonous, calm, pleasant, annoying, eventful, uneventful) were collected via a Prolific-hosted online listening experiment using the **Method A (survey)** protocol. Stimuli were presented over headphones and participants completed a headphone screening check prior to data collection.

---

## Study Context

This stimulus set was developed as part of a doctoral research project examining **aural diversity in soundscape perception** across three listener groups:

- **ADHD** (n = 30)
- **Autistic** (n = 30)
- **Neurotypical** (n = 30)

The project is situated within the **LAURA Leverhulme Trust Aural Diversity Doctoral Research Hub** at the **Acoustics Research Centre, University of Salford**, supervised by Prof. Bill Davies and Dr. Samantha Gregory.

The primary finding reported in the FA2026 paper is that ADHD listeners rated soundscapes as significantly less eventful than Neurotypical listeners (Mann–Whitney U, p = 0.043), with Autistic listeners closely tracking the Neurotypical group (Soundscape Perception Index similarity = 92.4%).

---

## Acknowledgements

This work was supported by the **Leverhulme Trust** through the LAURA Aural Diversity Doctoral Research Hub (grant reference RPG-2021-086). Participants were recruited via **Prolific**.

---

## References

Cox, T. J. and Katz, B. F. G. (2015). Recommendation for loudness of Internet audio streaming and on-demand distribution. *AES Technical Document TD1004.1.15-10*. Audio Engineering Society.

Gerzon, M. A. (1992). General metatheory of auditory localisation. In *Proceedings of the 92nd Audio Engineering Society Convention*. Vienna, Austria: Audio Engineering Society.

ISO 12913-2:2018. *Acoustics — Soundscape — Part 2: Data collection and reporting requirements*. Geneva: International Organisation for Standardisation.

Kronlachner, M. (2014). *AmbiX ambisonic plugin suite (v0.2.8)* [VST/AU plugins]. Available at: http://www.matthiaskronlachner.com/?p=2015

Mitchell, A., Aletta, F., Oberman, T., and Kang, J. (2024). Soundscapy: A Python package for soundscape assessment. *Journal of Open Source Software*, 9(94), 6100. https://doi.org/10.21105/joss.06100

Stevens, F., Murphy, D. T., and Smith, S. L. (2017). Ecological validity of stereo UHJ soundscape reproduction. In *Proceedings of the 142nd Audio Engineering Society Convention*, Berlin, Germany. AES Convention Paper 9765.

---

## Licence

Audio stimuli are made available for academic research purposes. If you use these materials, please cite the FA2026 paper above.

---

*Acoustics Research Centre, University of Salford | LAURA Leverhulme Trust Aural Diversity Doctoral Research Hub | 2026*
