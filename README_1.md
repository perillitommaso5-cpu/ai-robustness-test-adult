# AI Robustness Test – Adult Income

**[English](#english) · [Italiano](#italiano)**

This is the second project of a series. The first one, a fairness audit of the same model, is here: **[ai-fairness-audit-adult](https://github.com/perillitommaso5-cpu/ai-fairness-audit-adult)**.

---

## English

### What this project is

A robustness test of a machine learning model, carried out from the point of view of a **QA Engineer** working under the **EU AI Act** (art. 15: accuracy, robustness and resilience to errors in the data).

**Scenario:** the same loan model as in the first project. A bank uses it to decide who receives a loan offer, which is a **high-risk** use case under the AI Act.

**Question:** does the model stay reliable when the data are imperfect, when an irrelevant detail changes, and when it receives extreme cases?

### Key findings

| | Result |
| --- | --- |
| Natural variability of recall (5 data splits) | ±1.6 points |
| Noise ±5% on age and hours worked | +0.1 points: robust |
| Occupation missing for 20% of people | −4.7 points: not robust (threshold 3) |
| Indifference: +1 hour per week | 0.38% of decisions changed: passed |
| Monotonicity: +$5,000 capital gain | 0 violations: passed |
| Counterfactual: Husband ↔ Wife swapped | 11.3% and 12.6% of decisions changed: failed |
| Edge cases | 4 unreasonable answers out of 8 |

- **The model reads an empty field as information.** Removing the occupation lowers the probability of high income from 37% to 19% for the same person.
- **The hypothesis from the first project was only partly confirmed.** The `relationship` column strongly affects decisions, but the Husband/Wife swap favours Wife, so it does not explain the disadvantage of women found in the fairness audit.
- **Inconsistent inputs are accepted.** A 17-year-old with a doctorate receives a 68% probability without any warning.
- **Verdict: robustness not approved.**

### Method

- **Same model and data as the first project:** UCI Adult Income (1994 US Census), logistic regression, 70/30 split, sex and race excluded. Step 1 checks that accuracy and recall match the first project (85.5% and 59.8%).
- **Natural variability** measured first, as the yardstick for every other result.
- **Degradation curve:** increasing noise on age and hours worked, increasing share of missing occupation.
- **Metamorphic tests:** indifference, monotonicity and a counterfactual on the `relationship` column. These tests do not need to know the right answer: they check how the answer must change.
- **Edge cases** designed by hand, including an inconsistent one.
- **Acceptance criteria defined before** running the tests, each with the origin of its threshold.

### Repository contents

| File | Description |
| --- | --- |
| `progetto2_test_robustezza.ipynb` | The executed notebook, with all results and charts |
| `report_test_robustezza_en.pdf` | The full report (English) |
| `report_test_robustezza.pdf` | The full report (Italian) |
| `README.md` | This file |

### How to run it

No installation needed: open the notebook in [Google Colab](https://colab.research.google.com) (*File → Open notebook → GitHub*, then paste this repository's URL) and run the cells from top to bottom. The dataset is downloaded automatically.

### Limitations

Only some types of disturbance tested; the cause of the empty-field effect is not yet verified; the counterfactual covers only Husband and Wife; a single linear model; hand-picked edge cases; 1994 US data. Full details in section 6 of the report.

### Author

**Tommaso Perilli** – QA Engineer, working toward AI quality and AI Act compliance.

---

## Italiano

### Di cosa si tratta

Un test di robustezza su un modello di Machine Learning, svolto dal punto di vista di un **QA Engineer** che lavora nel quadro dell'**AI Act europeo** (art. 15: accuratezza, robustezza e resistenza agli errori nei dati).

**Scenario:** lo stesso modello del prestito del primo progetto. Una banca lo usa per decidere a chi proporre un prestito: un uso **ad alto rischio** secondo l'AI Act.

**Domanda:** il modello resta affidabile quando i dati sono imperfetti, quando cambia un dettaglio irrilevante e quando riceve casi estremi?

### Risultati principali

| | Risultato |
| --- | --- |
| Variabilità naturale del recall (5 divisioni dei dati) | ±1,6 punti |
| Rumore ±5% su età e ore lavorate | +0,1 punti: robusto |
| Professione mancante per il 20% delle persone | −4,7 punti: non robusto (soglia 3) |
| Indifferenza: +1 ora a settimana | 0,38% di decisioni cambiate: superato |
| Monotonia: +5.000 dollari di reddito da capitale | 0 violazioni: superato |
| Controfattuale: Husband ↔ Wife scambiati | 11,3% e 12,6% di decisioni cambiate: non superato |
| Casi limite | 4 risposte poco sensate su 8 |

- **Il modello legge un campo vuoto come un'informazione.** Togliendo la professione, la probabilità di reddito alto della stessa persona scende dal 37% al 19%.
- **L'ipotesi del primo progetto è confermata solo in parte.** La colonna `relationship` pesa molto sulle decisioni, ma lo scambio Husband/Wife favorisce Wife, quindi non spiega lo svantaggio delle donne emerso nell'audit di fairness.
- **Gli input incoerenti vengono accettati.** Un diciassettenne con dottorato riceve una probabilità del 68% senza alcun segnale.
- **Verdetto: robustezza non approvata.**

### Metodo

- **Stesso modello e stessi dati del primo progetto:** UCI Adult Income (censimento USA del 1994), regressione logistica, divisione 70/30, sesso ed etnia esclusi. Il Passo 1 verifica che accuratezza e recall coincidano con il primo progetto (85,5% e 59,8%).
- **Variabilità naturale** misurata per prima, come metro per tutti gli altri risultati.
- **Curva di degrado:** rumore crescente su età e ore lavorate, quota crescente di professioni mancanti.
- **Test metamorfici:** indifferenza, monotonia e un controfattuale sulla colonna `relationship`. Non serve conoscere la risposta giusta: si verifica come deve cambiare.
- **Casi limite** costruiti a mano, incluso un caso incoerente.
- **Criteri di accettazione definiti prima** di eseguire i test, ognuno con l'origine della sua soglia.

### Contenuto del repository

| File | Descrizione |
| --- | --- |
| `progetto2_test_robustezza.ipynb` | Il notebook eseguito, con tutti i risultati e i grafici |
| `report_test_robustezza.pdf` | Il report completo (italiano) |
| `report_test_robustezza_en.pdf` | Il report completo (inglese) |
| `README.md` | Questo file |

### Come eseguirlo

Non serve installare nulla: apri il notebook in [Google Colab](https://colab.research.google.com) (*File → Apri notebook → GitHub*, poi incolla l'indirizzo di questo repository) ed esegui le celle dall'alto verso il basso. Il dataset si scarica da solo.

### Limiti

Solo alcuni tipi di disturbo provati; la causa dell'effetto dei campi vuoti non è ancora verificata; il controfattuale copre solo Husband e Wife; un solo modello lineare; casi limite scelti a mano; dati USA del 1994. I dettagli sono nella sezione 6 del report.

### Autore

**Tommaso Perilli** – QA Engineer, in percorso verso la qualità dei sistemi AI e la conformità all'AI Act.
