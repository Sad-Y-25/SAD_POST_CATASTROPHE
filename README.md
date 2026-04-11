# 🌍 SpaceNet Infrastructure & Road Network Extraction

> Pipeline complet de vision par ordinateur pour la détection de bâtiments et l'extraction de réseaux routiers à partir d'imageries satellites. De pixels bruts à un graphe navigable pour faciliter l'acheminement des secours en zone de catastrophe.

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-FF6F00?logo=tensorflow&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-4.x-5C3EE8?logo=opencv&logoColor=white)
![NetworkX](https://img.shields.io/badge/NetworkX-Graph-brightgreen)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## 📋 Table des Matières

- [Fonctionnalités Clés](#-fonctionnalités-clés)
- [Stack Technique](#️-stack-technique)
- [Pipeline du Projet](#-pipeline-du-projet)
- [Résultats](#-résultats)
- [Installation](#-installation)
- [Perspectives](#️-perspectives)

---

## 🚀 Fonctionnalités Clés

| Fonctionnalité | Description |
|---|---|
| 🧠 **Segmentation Sémantique** | Architecture U-Net personnalisée pour la détection de routes et bâtiments |
| ⚖️ **Gestion du Déséquilibre** | Dice Loss pour pallier la rareté des pixels cibles (~5.9% des données) |
| 🗺️ **Vectorisation** | Conversion des masques de segmentation en squelettes filaires |
| 🔗 **Analyse de Graphe** | Transformation en graphes topologiques (Nœuds & Arêtes) via NetworkX |
| 🧭 **Navigation** | Calcul d'itinéraires optimaux via l'algorithme de Dijkstra |

---

## 🛠️ Stack Technique

```
Python 3.13
├── Deep Learning    → TensorFlow / Keras
├── Traitement Image → OpenCV, Scikit-Image (skimage)
├── Analyse Réseau   → NetworkX, Skan (Skeleton Analysis)
└── Visualisation    → Matplotlib, Seaborn
```

---

## 📈 Pipeline du Projet

```
Images Satellites
      │
      ▼
┌─────────────────┐
│  1. Prétraitement│  Patches 256×256px · Normalisation RGB
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  2. U-Net Model │  Encodeur → Décodeur · Skip Connections · Dice Loss
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  3. Squelette   │  Masque Binaire → Squelettisation → Nœuds & Arêtes
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  4. Itinéraire  │  Composante Connexe Principale · Dijkstra
└─────────────────┘
```

### 1. Prétraitement des Données

Découpage des images satellites haute résolution en patches de **256×256 pixels** et normalisation des canaux RGB.

### 2. Modélisation — U-Net

L'architecture repose sur un **encodeur** (contraction) et un **décodeur** (expansion) reliés par des **Skip Connections** pour préserver les détails spatiaux des infrastructures fines.

> **💡 Note Tactique :** L'utilisation de la `binary_crossentropy` produisait initialement des masques vides. Le passage à la **Dice Loss** a forcé le modèle à se concentrer sur la superposition des formes plutôt que sur la précision globale.

### 3. Extraction de Squelette & Graphe

Le masque binaire produit par l'IA est affiné par **squelettisation**, puis analysé pour extraire :

- **Nœuds** — Intersections et impasses
- **Arêtes** — Segments de routes avec calcul de distance en pixels

### 4. Calcul d'Itinéraire

Nettoyage du graphe pour ne conserver que la **composante principale connectée**, puis démonstration du calcul du chemin le plus court entre deux points du réseau extrait.

---

## 📊 Résultats

| Métrique | Valeur |
|---|---|
| 🎯 Perte Dice finale | ~0.05 sur l'échantillon d'entraînement |
| 🔵 Nœuds extraits | 14 nœuds (intersections & impasses) |
| 🛣️ Voirie détectée | 860+ pixels de réseau sur un échantillon type |

---

## 💻 Installation

```bash
# Cloner le dépôt
git clone https://github.com/votre-username/spacenet-extraction.git
cd spacenet-extraction

# Installer les dépendances
pip install tensorflow numpy matplotlib scikit-image networkx skan opencv-python
```

---

## 🗺️ Perspectives

- **📐 Scaling** — Traitement de tuiles satellites complètes via fenêtre glissante (*sliding window*)
- **🏷️ Multi-classe** — Distinction explicite entre routes goudronnées, pistes et types de bâtiments
- **🤖 RAG Integration** — Couplage avec un agent intelligent pour interroger le graphe en langage naturel *(ex: "Quelle est la route la plus courte vers l'hôpital ?")*

---

<div align="center">
  <sub>Projet réalisé dans le cadre du Système d'Aide à la Décision Post-Catastrophe · Phase 1</sub>
</div>
