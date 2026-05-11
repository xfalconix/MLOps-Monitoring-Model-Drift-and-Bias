# MLOps: Model Drift & Bias Monitoring

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat-square&logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)
![alibi-detect](https://img.shields.io/badge/alibi--detect-1A73E8?style=flat-square)
![sklego](https://img.shields.io/badge/sklego-E8A000?style=flat-square)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=jupyter&logoColor=white)
![LinkedIn](https://img.shields.io/badge/LinkedIn%20Learning-0A66C2?style=flat-square&logo=linkedin&logoColor=white)

Repositorio con el rework personal de los notebooks del curso de LinkedIn Learning **[MLOps Essentials: Monitoring Model Drift and Bias](https://www.linkedin.com/learning/mlops-essentials-monitoring-model-drift-and-bias)** (2023) de **Kumaran Ponnambalam**.

He reconstruido los notebooks desde cero, ajustando el estilo visual y añadiendo comentarios extensos en **espanol** para reforzar la comprension propia.

**Certificado del curso:** [Ver en LinkedIn](https://www.linkedin.com/learning/certificates/a258462ee55bbd8fc518baf4c6d94597609537448bb3472bf29f4c17d01d72cf?trk=share_certificate)

---

## Sobre el curso

> A medida que mas modelos de ML se desarrollan y despliegan, surge la necesidad de garantizar que sean efectivos, seguros y que funcionen segun lo esperado. El monitoreo de modelos, una funcion nucleo de MLOps, ayuda a los cientificos de datos e ingenieros MLOps a cumplir este objetivo. Kumaran Ponnambalam cubre los tipos de monitoreo necesarios, profundiza en drift y bias, explica diferentes tecnicas de deteccion, y las demuestra en Python con librerias de codigo abierto.

**Instructor:** Kumaran Ponnambalam — profesional de datos con mas de 20 anos de experiencia.

---

## Notebooks del repositorio

| Archivo | Tema |
|---------|------|
| `code_03_XX Drift Detection Example.ipynb` | Deteccion de feature drift y concept drift con tests estadisticos usando **alibi-detect** |
| `code_06_03 Equal Opportunity Score with sklego.ipynb` | Medicion de sesgo: Equal Opportunity Score con la libreria **sklego** |

Ambos notebooks usan un dataset de aprobacion de creditos (`credit-approval-training-data.csv`, `credit-approval-prod-data.csv`, `credit-approval-fair-data.csv`) para ilustrar escenarios reales de drift y sesgo.

---

## Stack tecnologico

| Libreria | Uso en el repo |
|-----------|---------------|
| `pandas` | Manipulacion y analisis de datos |
| `numpy` | Operaciones numericas |
| `matplotlib` | Visualizacion de resultados |
| `scikit-learn` | Modelo base (GaussianNB), train_test_split, metricas |
| `alibi-detect` | ChiSquareDrift para deteccion de feature drift |
| `sklego` | equal_opportunity_score para medicion de sesgo |

---

## Reflexion practica: drift y bias en reconocimiento de biofouling en cascos de buques

Los conceptos del curso no son abstractos. Este caso los ilustra todos desde mi experiencia profesional en servicios maritimos.

### Caso: CNN para reconocimiento automatico de biofouling en cascos de buques

El biofouling importa desde dos perspectivas: operacional (resistencia hidroinamica, consumo de combustible, emisiones bajo CII/EEXI) y regulatoria/bioseguridad (especies no indigena, gobernada por estandares como el clean hull standard de Nueva Zelanda).

Un estudio clave es Mannix et al. (2021, Nature Scientific Reports), que entreno una CNN con 10.000+ imagenes anotadas por expertos, alcanzando un acuerdo comparable al de revisores humanos expertos.

Donde se pone interesante:

- **Feature drift:** las imagenes del estudio provienen de tres organizaciones (DAWE-Australia, MPI-Nueva Zelanda, CSLC-California), mayormente buques internacionales. Desplegar ese modelo en el Estrecho de Gibraltar — donde passe anos coordinando inspecciones y limpiezas de cascos para navieras como MAERSK, CMA CGM y SVITZER — y las entradas cambian: biota mediterranea diferente, areas nicho (sea chests, helices, sea boxes), turbidez y luz distintas. Las distribuciones de pixeles se mueven.

- **Concept drift:** la verdad fundamental no es una ley fisica, son etiquetas humanas — y los expertos solo coinciden el 89% del tiempo. Si los reguladores endurecen el estandar manana, la misma imagen pasa de aceptable a no conforme sin que nada en ella cambie. Drift conceptual puro impulsado por gobernanza.

- **Sesgo de entrenamiento:** altamente desbalanceado — la mayoria de las imagenes son cascos limpios (SLoF 0), solo ~10% fouling severo (SLoF 2). El modelo puede aprender a "jugar seguro" y fallar en los casos mas criticos para bioseguridad.

- **Sesgo de etiquetas:** los expertos solo coinciden el 89% del tiempo. El juicio de un annotador se convierte en la "verdad" del modelo.

- **Sesgo de representacion operacional:** buques transoceanicos en puertos de bioseguridad estricta estan sobre-representados; flotas pesqueras y trafco costero, subrepresentados. El rendimiento se degrada precisamente donde los datos son mas escasos.

Aplicar ML a servicios maritimos no es solo sobre precision del modelo. Es sobre entender de donde vino el modelo, donde lo despliegas, y que cambios en el mundo eventualmente lo romperan.

> Mannix, E.J. et al. (2021). Automating the assessment of biofouling in images using expert agreement as a gold standard. *Scientific Reports* 11, 2739. https://doi.org/10.1038/s41598-021-82024-x
