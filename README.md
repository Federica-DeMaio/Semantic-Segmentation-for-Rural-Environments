
# Semantic Segmentation for Rural Environments - DeepLabV3+ Stable

Questo repository contiene il progetto finale per il corso di Machine Learning, focalizzato sulla **segmentazione semantica** di immagini acquisite da veicoli in ambienti rurali e off-road.

## 🎯 Obiettivo del Progetto

L'obiettivo è la classificazione pixel-wise di immagini acquisite da una camera mobile in condizioni di luce variabile. Il modello deve distinguere tra diversi tipi di terreno, ostacoli e vegetazione, rispettando stringenti vincoli hardware per l'esecuzione su Google Colab.

### Vincoli di Progetto

* **Memoria GPU (Training):** < 5 GB
* **Memoria GPU (Inference):** < 4 GB
* **Dataset:** 931 immagini (formato 1280x720, ridimensionate per l'addestramento).

## 🧠 Architettura: DeepLabV3+ Stable

Il modello implementato è una versione ottimizzata di **DeepLabV3+** che integra:

* **Backbone:** ResNet-101 (Pre-trained) con LR ridotto per il fine-tuning.
* **ASPP (Atrous Spatial Pyramid Pooling):** Per catturare il contesto multi-scala.
* **Decoder:** Modulo di raffinamento per recuperare i dettagli spaziali dai layer a bassa risoluzione.
* **Class Weighting:** Bilanciamento strategico dei pesi (es. Background ridotto al 10%) per dare priorità alle classi critiche come "Puddle" (pozzanghere) e "Obstacle".

## 📊 Classi e Mapping

Il modello gestisce le seguenti 9 classi:
| ID | Nome Classe | Descrizione |
|----|-------------|-------------|
| 0 | Background | Classe ignorata/sfondo |
| 1 | Smooth Trail | Sentiero battuto/piano |
| 2 | Grass | Erba |
| 3 | Rough Trail | Sentiero sconnesso |
| 4 | Puddle | Pozzanghere |
| 5 | Obstacle | Ostacoli |
| 6 | Non-traversable Vegetation | Vegetazione invalicabile |
| 7 | High Vegetation | Vegetazione alta |
| 8 | Sky | Cielo |

## 📈 Risultati Finali

Dopo un processo di ottimizzazione iterativo (incluso l'uso di scheduler adattivi e loss combinata), il modello ha raggiunto:

* **mIoU (Macro Avg):** ~0.646 (64.6%)
* **Mean F1-Score:** ~0.773 (77.3%)
* **Pixel Accuracy:** Estremamente elevata, specialmente su classi dominanti come Sky (IoU 0.901) e High Vegetation (IoU 0.855).

## 🛠️ Pipeline di Training

1. **Preprocessing:** Normalizzazione (ImageNet stats) e ridimensionamento.
2. **Loss:** `CombinedLoss` composta da Weighted Cross-Entropy + Dice Loss (0.5/0.5).
3. **Optimizer:** Adam con Learning Rate differenziati ($10^{-5}$ per il backbone, $10^{-4}$ per i moduli ASPP/Decoder).
4. **Regularization:** Early Stopping con pazienza di 15 epoche e scheduler `ReduceLROnPlateau`.

## 🚀 Utilizzo

1. **Dati:** Inserire le immagini in `./dataset/train` e `./dataset/val`.
2. **Training:** Eseguire le celle del notebook per avviare il loop di addestramento.
3. **Valutazione:** Utilizzare il modulo finale per generare la matrice di confusione e visualizzare i confronti tra Ground Truth e Predizione.

