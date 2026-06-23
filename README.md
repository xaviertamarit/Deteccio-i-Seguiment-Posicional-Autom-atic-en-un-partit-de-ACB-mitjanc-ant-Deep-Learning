# Detecció i Seguiment Posicional Automàtic en un partit de l'ACB mitjançant Deep Learning

Pipeline de visió per computador que combina detecció de persones (YOLOv8) i un model de re-identificació (Re-ID) entrenat des de zero per identificar individualment cada jugador d'un partit de l'ACB a partir d'un vídeo de retransmissió real, i registrar-ne la posició al llarg del partit sense necessitat d'etiquetar manualment cada fotograma.

El cas d'ús concret implementat al repositori és el partit Hiopos Lleida – Joventut de Badalona (jornada 23, retransmès per 3Cat).

## 📋 Què fa el projecte

1. Es descarrega el vídeo del partit i, a partir d'un conjunt de fotogrames seleccionats manualment, es retallen imatges de cada jugador amb YOLOv8 per construir un dataset de Re-ID propi (un jugador = una classe).
2. Amb aquest dataset s'entrena una xarxa ResNet50 modificada especialitzada en re-identificació de persones.
3. Es torna a processar el vídeo sencer: YOLOv8 detecta totes les persones de cada fotograma i, per a cada detecció, el model de Re-ID calcula un *embedding* i el compara amb la galeria de jugadors per saber qui és.
4. Es filtren el públic, els tècnics i els plans en primer pla (per mida de la capsa i nombre mínim de jugadors a pista), i es classifica cada jugador pel seu equip (color lila = Lleida, verd = Joventut).
5. Es desa la posició en píxels dels peus de cada jugador (centre inferior de la bounding box) fotograma a fotograma.
6. El partit es processa per trams, generant per a cadascun un vídeo anotat i un fitxer `.json` amb les coordenades.

## 📁 Contingut del repositori

| Fitxer | Descripció |
|---|---|
| `creador_dataset.ipynb` | Descarrega el vídeo del partit (`yt-dlp` + 3Cat), detecta persones amb YOLOv8x i, mitjançant una eina interactiva (`ipywidgets`), permet retallar i etiquetar manualment imatges de cada jugador per construir el dataset de Re-ID. |
| `dataset_reid.zip` | Dataset de Re-ID resultant (1.105 imatges), organitzat en `train/`, `test/query/` i `test/gallery/`, amb una carpeta per classe (jugador). |
|`creador_heatmap.ipynb`| Agafa els resultats dels JSON anterior i crea 2 mapes de calor per jugador.|
| `etiquetar_i_processar_video.ipynb` | Notebook principal: defineix l'arquitectura, entrena el model de Re-ID, l'avalua i l'aplica sobre el vídeo complet del partit per generar el seguiment posicional. |
| `posicions_jugadors*.json` | Coordenades de posició de cada jugador, exportades per trams del partit. |



### Classes del dataset de Re-ID

21 classes en total: 10 jugadors del Hiopos Lleida, 9 jugadors de la Joventut de Badalona, una classe `Arbitre` i una classe `soroll` (fons / falsos positius), per ajudar el model a no confondre públic o àrbitres amb jugadors.

## 🧠 Arquitectura del model de Re-ID

`ResNet50ReID`: una ResNet-50 preentrenada modificada per a tasques de Re-ID:

- *Stride* de la `layer4` reduït a `(1,1)` per conservar més resolució espacial.
- Capa `BNNeck` (BatchNorm sense bias) entre el backbone i el classificador, per equilibrar la Triplet Loss i la Cross-Entropy Loss durant l'entrenament.
- Sortida: vector d'embedding de 2048 dimensions normalitzat (L2), usat per comparar identitats per distància.

**Hiperparàmetres d'entrenament:** batch size 32, 20 èpoques, learning rate 0.00035.

## 📦 Format de les dades de sortida

Cada `posicions_jugadorsN.json` és un diccionari amb el nom de cada jugador com a clau i la llista de coordenades `[x, y]` (en píxels del fotograma) detectades durant aquell tram:

```json
{
  "01_James_Batemon": [[1096, 461], [1104, 485], [1142, 534], ...],
  "09_R_Rubio": [[823, 502], [831, 498], ...]
}
```

## 🛠️ Requisits

- Python 3.9+ amb GPU (CUDA) recomanat per entrenar i fer inferència en temps raonable.
- Llibreries principals: `torch`, `torchvision`, `ultralytics` (YOLOv8), `opencv-python`, `numpy`, `matplotlib`, `seaborn`, `pillow`, `yt-dlp`, `ipywidgets`.

Els notebooks detecten automàticament si s'executen a Google Colab, Kaggle o en local, i ajusten les rutes d'entrada/sortida en conseqüència.

## ▶️ Com utilitzar-ho

1. Clona el repositori:
```bash
   git clone https://github.com/xaviertamarit/Deteccio-i-Seguiment-Posicional-Autom-atic-en-un-partit-de-ACB-mitjanc-ant-Deep-Learning.git
```
2. (Opcional) Obre `creador_dataset.ipynb` si vols ampliar o regenerar el dataset de Re-ID a partir d'un altre vídeo. Si només vols entrenar/provar el model, ja tens `dataset_reid.zip` inclòs.
3. Obre `etiquetar_i_processar_video.ipynb` i executa les cel·les en ordre: carrega el dataset, entrena el model de Re-ID, l'avalua amb el conjunt `query`/`gallery`, i finalment processa el vídeo del partit per generar els vídeos anotats i els `.json` de posicions.
4. Obre  `creador_heatmap.ipynb` per a generar els heatmap de tots els jugadors.

## ⚠️ Notes

- El vídeo del partit es descarrega de 3Cat únicament amb finalitat acadèmica/de recerca; els drets de la retransmissió pertanyen als seus titulars.
- El projecte té finalitats acadèmiques i de recerca en l'àmbit de l'analítica esportiva i la intel·ligència artificial.
## 🎥 Vídeo de mostra del resultat
[Mostra del seguiment posicional generat](https://youtu.be/Lg08MtWiK70)
