---
title: 'Tutorial: Erste Schritte mit Machine Learning: Python'
titleSuffix: Azure Machine Learning
description: In diesem Tutorial erhalten Sie Informationen zu den ersten Schritten mit dem Azure Machine Learning SDK für Python, das in Ihrer persönlichen Entwicklungsumgebung ausgeführt wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: c0fe3c3808709de732bec8ce0599d380094405e8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91368480"
---
# <a name="tutorial-get-started-with-azure-machine-learning-in-your-development-environment-part-1-of-4"></a>Tutorial: Erste Schritte mit Azure Machine Learning in Ihrer Entwicklungsumgebung (Teil 1 von 4)

In dieser *vierteiligen Tutorialreihe* lernen Sie die Grundlagen von Azure Machine Learning kennen und führen auftragsbasierte Python-Machine Learning-Aufgaben auf der Azure-Cloudplattform durch. Zu diesen Aufgaben zählt Folgendes:

1. Einrichten eines Arbeitsbereichs und Ihrer lokalen Entwicklerumgebung für maschinelles Lernen
2. Ausführen von Code in der Cloud mit dem Azure Machine Learning SDK für Python
3. Verwalten der Python-Umgebung, die Sie zum Trainieren von Modellen verwenden
4. Hochladen von Daten in Azure und verbrauchen dieser Daten im Training.

In Teil 1 dieser Tutorialreihe führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Installieren des Azure Machine Learning SDK
> * Einrichten der Verzeichnisstruktur für Code
> * Erstellen eines Azure Machine Learning-Arbeitsbereichs
> * Konfigurieren Ihrer lokalen Entwicklungsumgebung
> * Einrichten eines Computeclusters

>[!NOTE]
> In dieser Tutorialreihe geht es um die Konzepte in Azure Machine Learning, die für *auftragsbasierte* Python-Machine Learning-Aufgaben geeignet sind, bei denen eine hohe Rechenintensität bzw. Reproduzierbarkeit gefordert ist. Wenn Ihre Machine Learning-Aufgaben nicht in dieses Profil passen, sollten Sie für die Umstellung auf Azure Machine Learning die [Jupyter- oder RStudio-Funktionalität auf einer Compute-Instanz für Azure Machine Learning](tutorial-1st-experiment-sdk-setup.md) verwenden.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie [Azure Machine Learning](https://aka.ms/AMLFree) aus.
- Vertrautheit mit Python- und [Machine Learning-Konzepten](concept-azure-machine-learning-architecture.md). Beispiele hierfür sind Umgebungen, Training und Bewertung.
- Eine lokale Entwicklungsumgebung: ein Laptop, auf dem Python und Ihre bevorzugte IDE (z. B. Visual Studio Code, PyCharm oder Jupyter) installiert sind.

## <a name="install-the-azure-machine-learning-sdk"></a>Installieren des Azure Machine Learning SDK

In diesem Tutorial verwenden wir durchgängig das Azure Machine Learning SDK für Python.

Sie können die Tools verwenden, mit denen Sie am besten vertraut sind (z. B. Conda und pip), um eine Umgebung einzurichten, die Sie im Rahmen dieses Tutorials verwenden. Installieren Sie das Azure Machine Learning SDK für Python mithilfe von pip in der Umgebung:

```bash
pip install azureml-sdk
```

## <a name="create-a-directory-structure-for-code"></a>Erstellen einer Verzeichnisstruktur für Code
Wir empfehlen Ihnen, für dieses Tutorial die folgende einfache Verzeichnisstruktur einzurichten:

```markdown
tutorial
└──.azureml
```

- `tutorial`: Oberstes Verzeichnis des Projekts.
- `.azureml`: Ausgeblendetes Unterverzeichnis zum Speichern von Azure Machine Learning-Konfigurationsdateien.

## <a name="create-an-azure-machine-learning-workspace"></a>Erstellen eines Azure Machine Learning-Arbeitsbereichs

Ein Arbeitsbereich stellt eine Ressource der obersten Ebene für Azure Machine Learning dar und bildet den zentralen Ort für diese Aufgaben:

- Verwalten von Ressourcen, z. B. Computeressourcen
- Speichern von Ressourcen wie Notebooks, Umgebungen, Datasets, Pipelines, Modellen und Endpunkten
- Zusammenarbeiten mit anderen Teammitgliedern

Fügen Sie im Verzeichnis der obersten Ebene (`tutorial`) eine neue Python-Datei mit dem Namen `01-create-workspace.py` hinzu, indem Sie den unten angegebenen Code verwenden. Passen Sie die Parameter (Name, Abonnement-ID, Ressourcengruppe und [Speicherort](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service)) nach Ihren Wünschen an.

Sie können den Code in einer interaktiven Sitzung oder als Python-Datei ausführen.

>[!NOTE]
> Bei Verwendung einer lokalen Entwicklungsumgebung (z. B. Laptop) werden Sie zum Durchführen der Authentifizierung für Ihren Arbeitsbereich mit einem *Gerätecode* aufgefordert, wenn Sie den unten angegebenen Code zum ersten Mal ausführen. Befolgen Sie die Anweisungen auf dem Bildschirm.

```python
# tutorial/01-create-workspace.py
from azureml.core import Workspace

ws = Workspace.create(name='<my_workspace_name>', # provide a name for your workspace
                      subscription_id='<azure-subscription-id>', # provide your subscription ID
                      resource_group='<myresourcegroup>', # provide a resource group name
                      create_resource_group=True,
                      location='<NAME_OF_REGION>') # For example: 'westeurope' or 'eastus2' or 'westus2' or 'southeastasia'.

# write out the workspace details to a configuration file: .azureml/config.json
ws.write_config(path='.azureml')
```

Führen Sie diesen Code aus dem `tutorial`-Verzeichnis aus:

```bash
cd <path/to/tutorial>
python ./01-create-workspace.py
```

Nachdem Sie den obigen Codeausschnitt ausgeführt haben, sieht Ihre Ordnerstruktur wie folgt aus:

```markdown
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
```

Die Datei `.azureml/config.json` enthält die Metadaten, die für die Verbindungsherstellung mit Ihrem Azure Machine Learning-Arbeitsbereich benötigt werden. Sie enthält die Abonnement-ID, die Ressourcengruppe und den Namen des Arbeitsbereichs. 

> [!NOTE]
> Beim Inhalt von `config.json` handelt es sich nicht um Geheimnisse. Diese Details können also weitergegeben werden.
>
> Authentifizierung ist jedoch trotzdem erforderlich, um mit Ihrem Azure Machine Learning-Arbeitsbereich zu interagieren.

## <a name="create-an-azure-machine-learning-compute-cluster"></a>Erstellen eines Computeclusters für Azure Machine Learning

Erstellen Sie im Verzeichnis `tutorial` der obersten Ebene ein Python-Skript mit dem Namen `02-create-compute.py`. Fügen Sie den folgenden Code ein, um einen Azure Machine Learning-Computecluster zu erstellen, mit dem automatisch eine Skalierung auf null bis vier Knoten durchgeführt wird:

```python
# tutorial/02-create-compute.py
from azureml.core import Workspace
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

ws = Workspace.from_config() # This automatically looks for a directory .azureml

# Choose a name for your CPU cluster
cpu_cluster_name = "cpu-cluster"

# Verify that the cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                            max_nodes=4, 
                                                            idle_seconds_before_scaledown=2400)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)
```

Führen Sie die Python-Datei aus:

```bash
python ./02-create-compute.py
```


> [!NOTE]
> Nachdem der Cluster erstellt wurde, weist er keine bereitgestellten Knoten auf. Der Cluster verursacht *keine* Kosten, bis Sie dafür einen Auftrag übermitteln. Der Cluster wird herunterskaliert, nachdem er sich 2.400 Sekunden (40 Minuten) im Leerlauf befunden hat.

Ihre Ordnerstruktur sieht nun wie folgt aus:

```bash
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
└──02-create-compute.py
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Setup-Tutorial haben Sie folgende Aufgaben durchgeführt:

- Erstellen eines Azure Machine Learning-Arbeitsbereichs
- Einrichten Ihrer lokalen Entwicklungsumgebung
- Erstellen eines Computeclusters für Azure Machine Learning

Im nächsten Tutorial erfahren Sie, wie Sie ein Skript an den Azure Machine Learning-Computecluster senden.

> [!div class="nextstepaction"]
> [Tutorial: Ausführen eines Python-Skripts „Hello World!“ (Teil 2 von 4)](tutorial-1st-experiment-hello-world.md)
