```markdown
# Detecció i Seguiment Posicional Automàtic en un partit de l'ACB mitjançant Deep Learning

Aquest repositori conté el codi font i la documentació del projecte enfocat a la **detecció, seguiment i extracció de coordenades posicionals en temps real** dels jugadors i de la pilota durant un partit de bàsquet de la lliga ACB, utilitzant tècniques avançades de *Deep Learning* i visió per computador.

L'objectiu principal és automatitzar la recollida de dades estadístiques avançades i posicionals (com el *tracking* de jugadors a la pista) a partir de retransmissions de vídeo estàndard o preses de càmera fixa.

## 🚀 Característiques del Projecte

- **Detecció d'Objectes:** Identificació precisa de jugadors, àrbitres i la pilota utilitzant models d'última generació (com YOLOv8 / YOLOv9 / YOLOv10).
- **Seguiment Multiobjecte (Object Tracking):** Assignació d'IDs únics a cada jugador al llarg del temps per evitar pèrdues de traçabilitat durant els encreuaments (utilitzant algorismes com ByteTrack, BoT-SORT o DeepSORT).
- **Homografia i Transformació de Perspectiva:** Projecció de les coordenades en píxels de la pantalla de televisió a un pla 2D real de la pista de bàsquet (ACB), obtenint coordenades $(X, Y)$ reals en metres.
- **Classificació d'Equips:** Identificació automàtica de l'equip de cada jugador basant-se en l'anàlisi de color de la samarreta (Clustering K-Means / HSV).

## 🛠️ Requisits del Sistema i Instal·lació

Assegura't de tenir instal·lat **Python 3.8+** (es recomana entorn amb GPU compatible amb CUDA per a un rendiment òptim).

1. Clona aquest repositori al teu ordinador local:
```bash
   git clone [https://github.com/xaviertamarit/Deteccio-i-Seguiment-Posicional-Autom-atic-en-un-partit-de-ACB-mitjanc-ant-Deep-Learning.git](https://github.com/xaviertamarit/Deteccio-i-Seguiment-Posicional-Autom-atic-en-un-partit-de-ACB-mitjanc-ant-Deep-Learning.git)
   cd Deteccio-i-Seguiment-Posicional-Autom-atic-en-un-partit-de-ACB-mitjanc-ant-Deep-Learning

```

2. Instal·la les dependències necessàries:

```bash
   pip install -r requirements.txt

```

*(Nota: Si utilitzes llibreries principals directes, les dependències típiques inclouen `ultralytics`, `opencv-python`, `numpy`, `scikit-learn` i `matplotlib`)*.

## 📂 Estructura del Repositori

```text
├── data/               # Vídeos de mostra de l'ACB i imatges de la pista 2D
├── models/             # Pesos dels models entrenats o configuracions de YOLO
├── src/                # Codi font del projecte
│   ├── detection.py    # Mòdul de detecció i tracking
│   ├── homography.py   # Càlcul de la matriu d'homografia de la pista
│   ├── utils.py        # Funcions auxiliars (processament de color, dibuix)
│   └── main.py         # Script principal d'execució
├── requirements.txt    # Fitxer de dependències del sistema
└── README.md           # Documentació del projecte

```

## 💻 Funcionament i Ús

Per executar el pipeline complet de detecció, seguiment i projecció sobre el vídeo de mostra, executa l'script principal:

```bash
python src/main.py --input data/partit_mostra.mp4 --output output/partit_processat.mp4

```

### Passos del Pipeline:

1. **Calibratge de la pista:** Selecció de punts clau de control per establir la matriu d'homografia entre el vídeo i la pista ACB en 2D.
2. **Inferència del Model:** Processament del vídeo frame a frame per localitzar jugadors i la pilota.
3. **Generació del mapa tèrmic / Tracking 2D:** Visualització en paral·lel de la retransmissió i de la representació bidimensional interactiva amb les posicions reals.

## 📊 Resultats i Visualització

El sistema genera un fitxer de vídeo de sortida on es pot veure:

* Bounding boxes (caixes de delimitació) amb l'ID de tracking de cada jugador.
* Un gràfic a vista de ocell (*minimap* o tàctic) on es mouen punts que representen els jugadors sobre les línies oficials de la pista ACB.
* Exportació de dades en format `.csv` o `.json` amb les coordenades de cada element per frame per a anàlisi posterior.

## ✒️ Autor

* **Xavier Tamarit** - *Desenvolupament complet del projecte* - [xaviertamarit](https://www.google.com/search?q=https://github.com/xaviertamarit)

---

*Projecte desenvolupat amb finalitats acadèmiques i de recerca en l'àmbit de l'analítica esportiva i la intel·ligència artificial.*

```

```
