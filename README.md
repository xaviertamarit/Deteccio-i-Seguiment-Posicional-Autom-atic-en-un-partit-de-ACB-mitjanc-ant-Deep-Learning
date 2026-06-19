Detecció i Seguiment Posicional de Jugadors de Bàsquet ACB mitjançant Deep Learning
Aquest repositori conté la implementació pràctica de l'arquitectura en cascada desacoblada dissenyada per a la detecció, re-identificació i generació de mapes de densitat de pas d'atletes de la Lliga ACB. El sistema utilitza exclusivament com a entrada el senyal de vídeo d'una retransmissió comercial de televisió, democratitzant l'accés a l'analítica avançada de rendiment sense dependre d'infraestructures de sensorització física.
🏀 Arquitectura del Sistema
El pipeline s'executa mitjançant un processat seqüencial modular frame a frame sota el següent esquema:
1. Detecció Espacial (YOLOv8x): Localització de siluetes humanes en pista amb un llindar de confiança de ￼ i filtre NMS de ￼.
2. Re-Identificació Mètrica (ResNet50 Re-ID): Extracció de l'embedding d'ADN de 2048 dimensions del BatchNorm Neck (BNNeck) amb pas de stride corregit a ￼ a la darrera capa convolucional.
3. Filtre de Soroll Actiu: Segregació de l'estament arbitral i de dades d'entorn mitjançant classes de control d'exclusió actives i llindar de distància euclidiana mètrica de ￼.
4. Representació d'Ocupació Espacial (KDE): Transformació de les coordenades de contacte del calçat aïllades en mapes de densitat continus en coordenades de pantalla.
📂 Estructura del Repositori
￼ ⁠creacio-dataset.ipynb⁠: Quadern destinat a la descàrrega automàtica del vídeo amb ⁠yt-dlp⁠, l'extracció semiautomàtica de siluetes d'atletes sota OpenCV i el visor interactiu d'etiquetatge biomètric multiclasse amb ⁠ipywidgets⁠.
￼ ⁠processat-video.ipynb⁠: Pipeline mestre que executa la inferència dinàmica per blocs de 7.000 frames, l'extracció en viu de l'ADN visual, el filtre de talls comercials, el protocol de prevenció d'errors CUDA OOM i l'exportació de fitxers JSON de posicions.
￼ ⁠creacio-heatmaps.ipynb⁠: Mòdul de de dades que llegeix el fitxer estructurat de posicions en format JSON i aplica l'algorisme de densitat de Kernel (KDE) bivariant per a superposar els contorns de pas en la imatge d'OpenCV.
￼ ⁠dataset_reid/⁠: Estructura unificada de directoris que conté els retalls biomètrics dels 19 jugadors catalogats, la classe d'àrbitres i la de soroll d'entorn, dividits en carpetes de ⁠train⁠, ⁠test/gallery⁠ i ⁠test/query⁠.
￼ ⁠posicions_json/⁠: Emmagatzematge continu sense pèrdua de dades de les coordenades ￼ extretes del punt de contacte inferior del calçat de cada bounding box.
￼ ⁠requirements.txt⁠: Catàleg unificat de biblioteques de Python necessàries per a l'execució del projecte.
🖥️ Demo i Visualització de Resultats
Per a avaluar de forma dinàmica el rendiment del programari desenvolupat d'enginyeria, es pot visualitzar el clip sencer del partit processat i netejat (amb el traçat de caixes de delimitació i l'exclusió activa d'àrbitres en directe) mitjançant el següent enllaç a YouTube:
🎥 Vídeo del Partit Processat a YouTube (Ocult) (substitueix aquest enllaç pel link un cop l'hagis penjat)
⚙️ Requisits d'Instal·lació i Ús
1. Clonar el repositori
2. Instal·lar dependències
Es recomana utilitzar un entorn virtual sota Python 3.10.12:
3. Executar els experiments
￼ Executa el quadern ⁠creacio-dataset.ipynb⁠ per realitzar un cribratge o afegir noves identitats d'atletes.
￼ Executa ⁠processat-video.ipynb⁠ per analitzar un fragment o clip de partit ACB en Full HD i generar els fitxers de coordenades unificats JSON.
￼ Executa ⁠creacio-heatmaps.ipynb⁠ per projectar els contorns continus de pas directament sobre els fotogrames del pavelló de Lleida.
🎓 Crèdits i Acadèmica
￼ Autor: Xavier Tamarit
￼ Projecte: Treball Final de Grau (TFG) en Matemàtica Computacional i Analítica de Dades.
￼ Institució: Facultat d'Enginyeria - Universitat Autònoma de Barcelona (UAB), Juny de 2026.
