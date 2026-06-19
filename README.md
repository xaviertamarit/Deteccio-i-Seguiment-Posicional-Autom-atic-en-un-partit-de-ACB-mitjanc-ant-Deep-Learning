# Detecció i Seguiment Posicional Automàtic en un Partit de l'ACB mitjançant Deep Learning
Python
PyTorch
CUDA
Ultralytics YOLO
UAB
Aquest repositori conté la implementació pràctica de l'arquitectura en cascada desacoblada dissenyada per a la detecció, re-identificació (Re-ID) de jugadors i generació de mapes de densitat de pas. El projecte s'ha validat utilitzant com a entrada exclusiva el senyal de vídeo d'una retransmissió comercial de la Lliga ACB (Hiopos Lleida contra Asisa Joventut), oferint una alternativa analítica no invasiva de baix cost.
📺 Demostració del Producte Final
Pots visualitzar el resultat del processament del vídeo sencer (amb les caixes de delimitació dinàmiques, etiquetatge nominal d'atletes i l'exclusió activa d'àrbitres en directe) clicant al següent enllaç o imatge de sota:
👉 Veure el vídeo del partit complet processat a YouTube
Vídeo del Partit Processat
📂 Estructura del Repositori
Aquest repositori s'estructura sota una organització modular de dades que conté tant els fitxers de codi com els actius d'avaluació:
🧠 Funcionament dels Notebooks
1. ⁠creacio-dataset.ipynb⁠
Estableix el flux d'enginyeria semiautomàtic per a la curació de la galeria d'ADN mètric del projecte:
￼ Descarrega el vídeo des de la CCMA (3cat) mitjançant la utilitat ⁠yt-dlp⁠.
￼ Llegeix frames en segons d'interès amb OpenCV i extreu caixes candidates de persones amb YOLOv8x.
￼ Desplega una interfície interactiva amb ⁠ipywidgets⁠ on l'usuari pot assignar cada crop a un jugador o rebutjar-lo amb el botó ⁠❌ IGNORAR ELEMENT⁠.
￼ El dataset final es compon de 36 captures per classe (20 de train, 15 de gallery i 1 de query) sumant un total de 772 imatges.
2. ⁠processat-video.ipynb⁠
És el nucli d'inferència del sistema, encarregat d'analitzar el partit per segments de 7.000 frames:
￼ Aplica el detector YOLOv8x amb un llindar d'activació del 0.35 i filtre NMS de 0.45.
￼ Retalla els candidats i calcula el vector normalitzat de 2048 dimensions de la ResNet50 Re-ID.
￼ Aplica un filtre de seguretat mètric (rebutja talls, repeticions i primers plans si ￼ o l'àrea d'un jugador supera el ￼).
￼ Integra un protocol defensiu contra errors de memòria de la GPU (CUDA Out Of Memory) mitjançant el context ⁠torch.no_grad()⁠ i desvinculament de tensors amb ⁠.detach().cpu().numpy()⁠.
3. ⁠creacio-heatmaps.ipynb⁠
Mòdul analític que importa la base de dades posicional continguda en els fitxers unificats JSON i utilitza Seaborn per projectar els contorns continus d'influència tèrmica mitjançant Estimació de Densitat de Kernel (KDE).
⚙️ Instruccions per a la Reproducció en Local
1. Clonar el repositori
2. Instal·lar les dependències
Es recomana l'ús d'un entorn de treball aïllat amb Python 3.10:
3. Executar els experiments
￼ Executa el fitxer ⁠creacio-dataset.ipynb⁠ per a modificar, ampliar o actualitzar la galeria de control d'ADN.
￼ Executa ⁠processat-video.ipynb⁠ per a extreure i indexar el moviment d'un clip de partit Full HD en fitxers JSON de píxels.
￼ Executa ⁠creacio-heatmaps.ipynb⁠ per a generar i superposar les traces de de densitat de pas.
🎓 Crèdits i Informació Acadèmica
￼ Autor: Xavier Tamarit
￼ Titulació: Treball Final de Grau (TFG) en Matemàtica Computacional i Analítica de Dades.
￼ Institució: Universitat Autònoma de Barcelona (UAB) - Facultat d'Enginyeria.
￼ Data de defensa: Juny de 2026.
