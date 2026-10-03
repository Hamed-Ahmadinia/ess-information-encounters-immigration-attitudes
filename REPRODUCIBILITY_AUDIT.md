# Reproducibility audit

This workflow reconstructs the supplied original analysis from the raw ESS CSV. It does not reproduce every printed claim. Published discrepancies are preserved as validation expectations; they never generate analytical results.

## Scope and source review

The forensic review covered all 65 original notebook cells and stored outputs, all 25 article pages, all nine supplementary pages, and the raw data. `PUBLICATION_OUTPUT_MANIFEST.json` records all 16 numbered outputs. Main figures, the appendix boxplots, and model/table pages were rendered for visual inspection. Original files were not edited.

## Computed sample and execution

Raw: 131,726 rows × 68 columns. Analytical: 117,752; managers: 9,263; other workers: 108,489; countries: 18; rounds: 4; country-year cells: 72.

SHA-256: `3ffcd57ddc096167f608e5ef53beaa9812ae7f18105001f36050484261715a01`. Raw bytes: 26,254,819. No duplicate respondent keys. Outlier union: 6,564 (5.57%).

All four main models and both plotting fits report final convergence. Full diagnostics and warnings are in `outputs/tables/model_diagnostics.csv`. Execution of this audit cell means prior analytical cells completed in this kernel; independent clean-run confirmation is recorded in `EXECUTION_CHECK.json` after the external execution harness finishes.

## Validation totals

1894 checks; 1686 PASS; 208 not passed. Of the latter, 2 are final-digit floating-point differences and 6 concern unavailable numerical figure references. These counts include separate checks of the incorrectly labelled Appendix 5 manager population; the historical full-sample table is checked separately. Internal Wald consistency checks are identified as internal, not publication evidence.

| output | PASS | LABEL/REFERENCE-CATEGORY DIFFERENCE | ROUNDING ONLY | PUBLICATION TYPOGRAPHICAL DISCREPANCY | UNRESOLVED |
| --- | --- | --- | --- | --- | --- |
| Raw data | 4 | 0 | 0 | 0 | 0 |
| Sample | 14 | 0 | 0 | 0 | 0 |
| Appendix 1 | 266 | 0 | 0 | 0 | 0 |
| Appendix 2 | 128 | 0 | 0 | 0 | 0 |
| Appendix 3 | 10 | 0 | 0 | 0 | 0 |
| Appendix 5 historical full sample | 180 | 0 | 0 | 0 | 0 |
| Appendix 5 labelled manager population | 5 | 175 | 0 | 0 | 0 |
| Model 1 / Appendix 6 | 77 | 0 | 1 | 0 | 0 |
| Internal model consistency | 65 | 0 | 0 | 0 | 0 |
| Model 2 / Appendix 7 | 84 | 0 | 0 | 0 | 0 |
| Model 3 / Appendix 8 | 98 | 0 | 1 | 7 | 0 |
| Model 4 / Appendix 9 | 179 | 0 | 0 | 1 | 0 |
| Table 2 | 396 | 1 | 0 | 13 | 0 |
| Figure 2 | 72 | 0 | 0 | 0 | 1 |
| Figure 3 | 8 | 0 | 0 | 0 | 1 |
| Figure 4 | 31 | 0 | 0 | 1 | 1 |
| Figure 5 | 37 | 0 | 0 | 0 | 1 |
| Article prose | 32 | 0 | 0 | 2 | 0 |
| Figure 1 | 0 | 0 | 0 | 0 | 1 |
| Appendix 4 | 0 | 0 | 0 | 0 | 1 |

## Methodological discrepancies

| status | issue | published | computed | interpretation |
| --- | --- | --- | --- | --- |
| COMPUTATIONAL DISCREPANCY | Manager definition | Article p.5: ISCO 1000–1439 | Original code uses all ISCO <=1439, including 355 below 1000; strict 1000–1439 count is 8,908. | Historical classification retained for reproduction; occupations below 1000 are not silently relabelled as standard ISCO managers. |
| COMPUTATIONAL DISCREPANCY | Value reversal | Article p.6: reversed according to ESS recommendations | Original cell 12 maps 1→5,2→4,3→3,4→2,5→1,6→6. | Literal historical rule retained. The highest original response becomes the highest coded value, so the orientation is not monotonic. A corrected scale would be a new analysis. |
| COMPUTATIONAL DISCREPANCY | Scale construction | Article p.6: centered/averaged indexes then standardized | Each item is population-standardized over all raw rows before the sample restriction; composites are means of these z scores and are not standardized again. | No person-mean centering appears in source. Seven-item self-transcendence includes impfree,impdiff,ipcrtiv; preserve explicit item membership. |
| COMPUTATIONAL DISCREPANCY | Robust standard errors | Article p.8 claims robust SEs | Original MixedLM.fit() uses model-based SEs; no robust covariance estimator is requested. | Model-based SEs reproduced. Do not describe them as robust. |
| LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 population | Appendix 5 and Article p.10 say managers | All published mean/SD cells come from the full analytical sample. | Both full-sample historical table and properly filtered manager table are computed and displayed; all differences are in the validation CSV. |
| LABEL/REFERENCE-CATEGORY DIFFERENCE | Model 2 contrast | Table 2: European Manager -0.079; Appendix 7: +0.079 | manager_flipped=2 is manager; reference 1 is other worker. | Computed table reports the positive manager-versus-worker coefficient. Negative prose contrast for other workers on p.13 is consistent after reversing the comparison. |
| COMPUTATIONAL DISCREPANCY | Figure 2 population and trimming | Article p.10 says managers; p.7 says no outliers removed | Original Figure 2 uses both groups and sequential 5–95% trimming for internet, news, then outcome. | Main models retain all observations. Figure 2 alone uses the sequentially trimmed visualization sample. |
| COMPUTATIONAL DISCREPANCY | Figure 2 shaded band | Original caption calls the shading a 95% confidence interval | Original cell 53 uses yhat ± 1.96 × slope SE at every x. | This constant-width band is not a standard regression-mean confidence band. Reproduced and explicitly identified; not silently replaced. |
| LABEL/REFERENCE-CATEGORY DIFFERENCE | Figures 3–5 model identity | Original captions refer to Models 1/3 | Figure 3 fits a separate full-sample ML country-intercept model with round fixed effects; Figures 4–5 share a separate full-sample REML country random-slope model. | The four article models and the two plotting fits are separately reported. |
| COMPUTATIONAL DISCREPANCY | Figures 3–5 uncertainty and interaction language | Published captions describe confidence intervals / interaction effects | Fixed-effect predictions are averaged within observed groups; intervals are mean ±1.96 SD(predictions)/sqrt(N). Models have no managerial-status or product interaction term. | Bands quantify within-group dispersion of fitted predictions, not fitted-parameter uncertainty or a formal interaction test. |
| LABEL/REFERENCE-CATEGORY DIFFERENCE | Figure 5 significance | Article p.16 discusses significant manager-worker differences | Bar darkness tests each prediction-average interval against zero; no between-group contrast is tested. | Do not interpret the shading as a manager-worker significance test. The displayed highest manager means are Germany, Iceland, Slovenia, not the prose country examples. |
| LABEL/REFERENCE-CATEGORY DIFFERENCE | Zero and neutral | Figure 3 uses a Neutral label at zero | Zero is the pooled item-standardization reference, not the ESS response midpoint 5. | Published interpretive axis labels retained with this qualification. |
| UNRESOLVED | Survey weighting and current employment | Article describes native-born workers | Source drops survey weights and does not filter current employment or working age. Birth country and occupation are imputed. | Reproduced unweighted estimand; supplied variables do not establish that every retained respondent is currently employed. |
| UNRESOLVED | Optimizer boundary warnings | Source suppresses ConvergenceWarning | Final convergence is true for all six fits. Model 3 retries after initial optimizer failure; variance-boundary warnings remain for manager models and the full-sample random-slope fit. | Warnings are captured, displayed and exported; convergence does not remove variance-boundary cautions. |
| UNRESOLVED | Unprinted figure values | Publication supplies raster figures without source plotting tables | Rounded point/slope/count labels are checked; exact unprinted values cannot be independently recovered. | All computed plotting summaries, curves and box statistics are exported; visual agreement does not prove equality of unpublished values. |

## Numerical and notation discrepancies

Every nonpassing numerical comparison follows, including the manager-only Appendix 5 cells. Values are unrounded computed results; the decimals column specifies the publication precision. `ROUNDING ONLY` is not promoted to exact agreement.

| output | item | computed | published | decimals | status | source |
| --- | --- | --- | --- | --- | --- | --- |
| Appendix 5 labelled manager population | Belgium/2016/mean | -0.08938064105845683 | -0.16 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Belgium/2016/sd | 0.7459139282248413 | 0.76 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Belgium/2018/mean | -0.06769359656800969 | -0.08 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Belgium/2018/sd | 0.7161120336762434 | 0.71 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Belgium/2020/mean | 0.1669095236126586 | 0.07 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Belgium/2020/sd | 0.7074145645025485 | 0.7 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Belgium/2023/mean | 0.25536729537743313 | 0.0 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Belgium/2023/sd | 0.6748109984224702 | 0.74 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Belgium/Overall/mean | 0.021834492079755154 | -0.05 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Belgium/Overall/sd | 0.7306139449791124 | 0.74 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Finland/2016/mean | 0.39716894876177444 | 0.15 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Finland/2016/sd | 0.6843916107020059 | 0.77 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Finland/2018/mean | 0.5230861357002816 | 0.2 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Finland/2018/sd | 0.6414134967027327 | 0.76 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Finland/2020/mean | 0.7611782096005184 | 0.34 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Finland/2020/sd | 0.655300700712811 | 0.72 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Finland/2023/mean | 0.6278381177876535 | 0.4 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Finland/2023/sd | 0.629239269056217 | 0.74 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Finland/Overall/mean | 0.5584933267372963 | 0.26 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Finland/Overall/sd | 0.6623140752164192 | 0.76 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | France/2016/mean | -0.2114995662361297 | -0.3 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | France/2016/sd | 0.8529315604239528 | 0.91 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | France/2018/sd | 0.9233986322493654 | 0.88 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | France/2020/mean | 0.0802433043056639 | -0.1 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | France/2020/sd | 0.779624489699609 | 0.88 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | France/2023/mean | 0.03807282078094663 | -0.15 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | France/2023/sd | 0.8471098945990168 | 0.89 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | France/Overall/mean | -0.05875328759600526 | -0.19 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | France/Overall/sd | 0.8417016055264871 | 0.89 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Germany/2016/mean | 0.058747015225719845 | 0.02 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Germany/2016/sd | 0.8316490204544941 | 0.85 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Germany/2018/mean | 0.1332564198690243 | 0.08 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Germany/2018/sd | 0.8115535238884974 | 0.83 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Germany/2020/mean | 0.32288558986440313 | 0.02 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Germany/2020/sd | 0.827077863955354 | 0.91 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Germany/2023/mean | 0.14119134092919 | 0.02 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Germany/2023/sd | 0.74567366913874 | 0.84 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Germany/Overall/mean | 0.20665812848300433 | 0.03 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Germany/Overall/sd | 0.8208310213859424 | 0.88 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Hungary/2016/mean | -0.7151385452903704 | -0.83 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Hungary/2016/sd | 0.7971590740524873 | 0.83 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Hungary/2018/mean | -0.5410002709208701 | -0.71 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Hungary/2018/sd | 0.8007315560966709 | 0.79 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Hungary/2020/mean | -0.43927436672964465 | -0.64 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Hungary/2020/sd | 0.7785454108484949 | 0.79 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Hungary/2023/mean | -0.5545823110246108 | -0.73 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Hungary/2023/sd | 1.0186290218728289 | 0.9 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Hungary/Overall/mean | -0.5401240549945219 | -0.73 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Hungary/Overall/sd | 0.869924377682696 | 0.83 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Iceland/2016/mean | 0.6409791497903014 | 0.58 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Iceland/2016/sd | 0.7099594024304023 | 0.7 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Iceland/2018/mean | 0.7729123992096543 | 0.65 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Iceland/2018/sd | 0.7430466243896594 | 0.71 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Iceland/2020/mean | 0.8212542224851891 | 0.69 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Iceland/2020/sd | 0.6659873146296327 | 0.7 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Iceland/2023/mean | 0.6125274166554192 | 0.53 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Iceland/2023/sd | 0.8029532060068141 | 0.76 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Iceland/Overall/mean | 0.7232439111965951 | 0.62 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Iceland/Overall/sd | 0.730017738134484 | 0.72 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Ireland/2016/mean | 0.30366794798645147 | 0.02 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Ireland/2016/sd | 0.8289782971394711 | 0.91 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Ireland/2018/mean | 0.41305311131205347 | 0.16 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Ireland/2018/sd | 0.8743147028269229 | 0.88 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Ireland/2020/mean | 0.7835442609622967 | 0.29 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Ireland/2020/sd | 0.8337833896599591 | 0.86 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Ireland/2023/mean | 0.45402425022351045 | 0.09 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Ireland/2023/sd | 0.8171263682214855 | 0.97 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Ireland/Overall/mean | 0.4235261500086101 | 0.13 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Ireland/Overall/sd | 0.8456362003931701 | 0.91 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Italy/2016/mean | -0.6159686886289837 | -0.68 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Italy/2016/sd | 0.9801404765229271 | 0.96 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Italy/2018/mean | -0.4166517357250518 | -0.52 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Italy/2018/sd | 1.0253193121914612 | 0.94 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Italy/2020/mean | -0.16672089704579884 | -0.29 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Italy/2023/mean | -0.38763905873890003 | -0.38 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Italy/2023/sd | 0.9533340018460109 | 0.86 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Italy/Overall/mean | -0.37863801965270805 | -0.47 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Italy/Overall/sd | 0.9688992999222447 | 0.92 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Lithuania/2016/mean | -0.13163068751007334 | -0.3 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Lithuania/2016/sd | 0.824743580255407 | 0.77 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Lithuania/2018/mean | -0.07850749441962178 | -0.2 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Lithuania/2018/sd | 0.7880067769881702 | 0.85 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Lithuania/2020/mean | -0.06795237135109496 | -0.23 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Lithuania/2020/sd | 0.8327279047808388 | 0.89 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Lithuania/2023/mean | -0.06065503497864931 | -0.15 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Lithuania/2023/sd | 0.7551083200414261 | 0.82 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Lithuania/Overall/mean | -0.08022858333009292 | -0.23 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Lithuania/Overall/sd | 0.800980473524295 | 0.83 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Netherlands/2016/mean | 0.10609633701405453 | -0.03 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Netherlands/2016/sd | 0.654772041173238 | 0.66 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Netherlands/2018/mean | 0.1678556420509744 | 0.07 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Netherlands/2018/sd | 0.5886388958875047 | 0.61 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Netherlands/2020/mean | 0.21740727951203348 | 0.19 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Netherlands/2020/sd | 0.5661944819987696 | 0.61 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Netherlands/2023/mean | 0.17503968416974222 | 0.07 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Netherlands/2023/sd | 0.6617516105996253 | 0.65 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Netherlands/Overall/mean | 0.16593839942577812 | 0.07 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Netherlands/Overall/sd | 0.6220110140965626 | 0.64 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Norway/2016/mean | 0.20324526522179132 | 0.03 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Norway/2016/sd | 0.7400966789347808 | 0.75 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Norway/2018/mean | 0.23769165061514957 | 0.15 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Norway/2018/sd | 0.7701940550802523 | 0.75 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Norway/2020/mean | 0.40440690232937015 | 0.32 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Norway/2020/sd | 0.7533676951147487 | 0.71 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Norway/2023/mean | 0.2620322327599424 | 0.25 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Norway/2023/sd | 0.7464629801867095 | 0.72 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Norway/Overall/mean | 0.2681367524479276 | 0.18 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Norway/Overall/sd | 0.7545300766281793 | 0.74 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Poland/2016/mean | 0.13888012293452962 | -0.1 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Poland/2016/sd | 0.7717328980855672 | 0.75 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Poland/2018/mean | 0.18351529576960549 | 0.02 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Poland/2018/sd | 0.9050275427956099 | 0.79 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Poland/2020/mean | 0.5263516336100051 | 0.23 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Poland/2020/sd | 0.8469800514837674 | 0.93 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Poland/2023/mean | 0.12988078229066638 | -0.04 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Poland/2023/sd | 0.889279842483948 | 0.78 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Poland/Overall/mean | 0.27112923269625844 | 0.04 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Poland/Overall/sd | 0.8689357858655865 | 0.83 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Portugal/2016/mean | 0.09023979781535559 | 0.0 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Portugal/2016/sd | 0.5737077488109605 | 0.79 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Portugal/2018/mean | 0.2891823542299692 | 0.16 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Portugal/2018/sd | 0.7002272512390916 | 0.73 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Portugal/2020/mean | 0.3056672162482743 | 0.09 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Portugal/2023/mean | 0.11242563263956962 | -0.11 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Portugal/2023/sd | 0.6872961868855152 | 0.73 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Portugal/Overall/mean | 0.20903018215525046 | 0.04 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Portugal/Overall/sd | 0.6813282433333794 | 0.74 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Slovenia/2016/mean | -0.11942145176327316 | -0.54 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Slovenia/2016/sd | 0.8861306686501543 | 0.88 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Slovenia/2018/mean | -0.18324708384798194 | -0.48 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Slovenia/2018/sd | 0.9183366327471792 | 0.9 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Slovenia/2020/mean | -0.06818550973206543 | -0.32 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Slovenia/2020/sd | 0.7619640456248488 | 0.85 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Slovenia/2023/mean | -0.012793049026182222 | -0.21 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Slovenia/2023/sd | 0.8227333354758487 | 0.87 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Slovenia/Overall/mean | -0.0906545815020729 | -0.39 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Slovenia/Overall/sd | 0.8461116908290227 | 0.89 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Spain/2016/mean | 0.06151538179792056 | -0.02 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Spain/2016/sd | 0.8208100114321606 | 0.84 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Spain/2018/mean | 0.2267715898869285 | 0.07 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Spain/2018/sd | 0.8578708753258523 | 0.85 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Spain/2020/mean | 0.3464229070923228 | 0.3 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Spain/2020/sd | 0.9060664172992842 | 0.95 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Spain/2023/mean | 0.30836003548480323 | 0.17 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Spain/2023/sd | 0.8339472732739692 | 0.85 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Spain/Overall/mean | 0.25868814691140557 | 0.14 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Spain/Overall/sd | 0.8624230626188335 | 0.89 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Sweden/2016/mean | 0.5274537393524661 | 0.27 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Sweden/2016/sd | 0.6598248134189775 | 0.8 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Sweden/2018/mean | 0.40269132835916316 | 0.3 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Sweden/2020/mean | 0.05061040841211299 | -0.04 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Sweden/2020/sd | 0.9233355322453779 | 0.99 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Sweden/2023/mean | 0.41283513792883947 | 0.34 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Sweden/2023/sd | 0.7846849288598048 | 0.77 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Sweden/Overall/mean | 0.2477743601399699 | 0.18 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Sweden/Overall/sd | 0.866759113910449 | 0.88 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Switzerland/2016/mean | 0.23232199771924175 | 0.05 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Switzerland/2016/sd | 0.8123296835000001 | 0.73 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Switzerland/2018/mean | 0.2774637045714581 | 0.12 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Switzerland/2018/sd | 0.7376912656409483 | 0.68 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Switzerland/2020/mean | 0.37212955638152695 | 0.2 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Switzerland/2020/sd | 0.6037963748774191 | 0.68 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Switzerland/2023/mean | 0.3159624973899519 | 0.17 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Switzerland/2023/sd | 0.6082483641693147 | 0.71 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | Switzerland/Overall/mean | 0.2989606739264184 | 0.13 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | United Kingdom/2016/mean | 0.08902231278061218 | -0.07 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | United Kingdom/2016/sd | 0.9211671219151449 | 0.9 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | United Kingdom/2018/mean | 0.12978483292813825 | 0.0 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | United Kingdom/2018/sd | 0.8625753594319462 | 0.95 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | United Kingdom/2020/mean | 0.37347068409902534 | 0.23 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | United Kingdom/2020/sd | 0.8712675913753775 | 0.92 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | United Kingdom/2023/mean | 0.4326204522761848 | 0.18 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | United Kingdom/2023/sd | 0.8726061916412868 | 0.95 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | United Kingdom/Overall/mean | 0.2506159340826546 | 0.06 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Appendix 5 labelled manager population | United Kingdom/Overall/sd | 0.8917789330332254 | 0.94 | 2.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Appendix 5 p.5 |
| Model 1 / Appendix 6 | deviance full printed precision | 273612.9196406661 | 273612.9196406634 | 10.0 | ROUNDING ONLY | Appendix 6 p.6 |
| Model 3 / Appendix 8 | C(essround)[T.9]/p | 0.032803918780506244 | 0.003 | 3.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Appendix 8 p.8 |
| Model 3 / Appendix 8 | self_transcendence/ci_high | 0.24490413757152943 | 0.2245 | 4.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Appendix 8 p.8 |
| Model 3 / Appendix 8 | nwspol_log/estimate | 0.05122065378556132 | 0.0511 | 4.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Appendix 8 p.8 |
| Model 3 / Appendix 8 | age_category/z | 1.0616773020859003 | -1.062 | 3.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Appendix 8 p.8 |
| Model 3 / Appendix 8 | age_category/ci_high | 0.019160525488216708 | -0.019 | 3.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Appendix 8 p.8 |
| Model 3 / Appendix 8 | rlgdgr/ci_low | -0.003298122614642535 | 0.003 | 3.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Appendix 8 p.8 |
| Model 3 / Appendix 8 | scale | 0.5509288010457872 | 0.55509 | 5.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Appendix 8 p.8 |
| Model 3 / Appendix 8 | deviance full printed precision | 20938.754379727354 | 20938.75437972734 | 11.0 | ROUNDING ONLY | Appendix 8 p.8 |
| Model 4 / Appendix 9 | education_category/ci_low | 0.08880568821053605 | -0.089 | 3.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Appendix 9 p.9 |
| Table 2 | M2/Intercept/se | 0.03349917181500847 | 0.034 | 3.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M3/Intercept/stars | ** | *** | nan | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M4/Intercept/stars | ** | *** | nan | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M3/Religiosity/stars |  | *** | nan | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M2/Self-Transcendence/estimate | 0.181206752751753 | 0.183 | 3.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M2/Conservation/estimate | -0.1939762675058167 | -0.195 | 3.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M2/European Manager/estimate | 0.07891111366336168 | -0.079 | 3.0 | LABEL/REFERENCE-CATEGORY DIFFERENCE | Article pp.12–13 |
| Table 2 | M3/Y.2018/stars | * | *** | nan | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M3/Y.2023/stars | ** | *** | nan | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M4/France/stars |  | *** | nan | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M4/Lithuania/stars |  | * | nan | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M4/Netherlands/stars |  | ** | nan | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M4/Sweden/stars | ** | *** | nan | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Table 2 | M4/Level 3: Year/stars |  | ** | nan | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article pp.12–13 |
| Figure 4 | internet/Medium Low/Other European Worker/mean | -0.14666157520086298 | 0.15 | 2.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article p.14 visual point label |
| Article prose | M4/age_category/estimate | 0.006847060393009188 | -0.02 | 3.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article p.15 |
| Article prose | M4/netustm_log/p | 0.0004416046573578183 | 0.009 | 3.0 | PUBLICATION TYPOGRAPHICAL DISCREPANCY | Article p.15 |
| Figure 1 | Unlabelled exact quartiles and whiskers | Exported from raw-data computation | No numeric reference in PDF | nan | UNRESOLVED | Published figure |
| Figure 2 | Unprinted scatter coordinates and line-band endpoints | Exported from raw-data computation | No numeric reference in PDF | nan | UNRESOLVED | Published figure |
| Figure 3 | Unprinted interval endpoints | Exported from raw-data computation | No numeric reference in PDF | nan | UNRESOLVED | Published figure |
| Figure 4 | Unprinted interval endpoints | Exported from raw-data computation | No numeric reference in PDF | nan | UNRESOLVED | Published figure |
| Figure 5 | Unprinted interval endpoints and threshold decisions | Exported from raw-data computation | No numeric reference in PDF | nan | UNRESOLVED | Published figure |
| Appendix 4 | Unlabelled exact quartiles, whiskers and fliers | Exported from raw-data computation | No numeric reference in PDF | nan | UNRESOLVED | Published figure |

## Reproduction decision

The notebook provides an executable reproduction of the original computation, with publication errors and unresolved interpretations documented. It cannot truthfully be described as 100% reproduction of all final published numbers, labels, and interpretations. No published numeric value is used to generate a fitted model, data-derived table, point, slope, count, or band.

The sample and Appendix 1–3 anchors, historical Appendix 5 full-sample values, fitted model estimates, and labelled figure values are assessed independently in the validation file. Models retain the historical scale coding, unweighted analysis, pooled imputation, and ordinal numeric demographic covariates. The corrected manager-only Appendix 5 table is an explicitly separate view, not a silent alteration of the historical result.

## Data access and archiving

The article p.21 identifies the European Social Survey portal: https://ess.sikt.no/en/. Place the named raw CSV beside the notebook. No cleaned CSV, PDF, original notebook, network connection, or helper script is required to run the final notebook. The archive excludes respondent-level CSV data. Applicable ESS redistribution terms have not been adjudicated here; do not infer data permission from the article or code licence.

## Environment

Python 3.11.9; Windows-10-10.0.26300-SP0. Package versions used in this run are pinned in `requirements.txt`. Historical code was originally run with statsmodels 0.14.2; the successful reproduction preserves its default fitting sequence.

| Package | Version |
| --- | --- |
| pandas | 2.2.3 |
| numpy | 1.26.4 |
| scipy | 1.13.1 |
| statsmodels | 0.14.2 |
| scikit-learn | 1.4.2 |
| matplotlib | 3.8.4 |
| seaborn | 0.13.2 |
| nbformat | 5.9.2 |
| nbclient | 0.8.0 |
| ipykernel | 6.28.0 |
| patsy | 0.5.6 |
| threadpoolctl | 2.2.0 |
