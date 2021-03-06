---
title: 'Zelfstudie: Een model voor de classificatie van afbeeldingen trainen met de Azure Machine Learning-service'
description: In deze zelfstudie leert u hoe u met de service Azure Machine Learning een model voor het classificeren van afbeeldingen traint met scikit-learn in een Python Jupyter-notebook. Deze zelfstudie is deel één van een serie van twee.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: e6e49a03ee76c50cb2fff492bfd50b2820abafe4
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343755"
---
# <a name="tutorial-1-train-an-image-classification-model-with-azure-machine-learning-service"></a>Zelfstudie 1: Een model voor de classificatie van afbeeldingen trainen met de Azure Machine Learning-service

In deze zelfstudie gaat u een machine learning-model zowel lokaal als op externe rekenresources trainen. U gebruikt de werkstroom voor training en implementatie voor de Azure Machine Learning-service (preview) in een Python Jupyter-notebook.  Vervolgens kunt u het notebook gebruiken als een sjabloon voor het trainen van uw eigen machine learning-model met uw eigen gegevens. Deze zelfstudie is **deel één van een serie van twee**.  

In deze zelfstudie traint u een eenvoudig logistieke regressiemodel met de gegevensset [MNIST](http://yann.lecun.com/exdb/mnist/) en [scikit-learn](http://scikit-learn.org) met behulp van de Azure Machine Learning-service.  MNIST is een populaire gegevensset die bestaat uit 70.000 afbeeldingen in grijstinten. Elke afbeelding is een handgeschreven cijfer van 28 x 28 pixels, dat een getal tussen 0-9 vertegenwoordigt. Het doel is om een classificatiemechanisme met meerdere klassen te maken om het cijfer te identificeren dat een bepaalde afbeelding vertegenwoordigt. 

Leer hoe u het volgende doet:

> [!div class="checklist"]
> * De ontwikkelomgeving instellen
> * De gegevens downloaden en controleren
> * Een eenvoudig logistiek regressiemodel trainen met behulp van de populaire bibliotheek voor machine learning scikit-learn. 
> * Meerdere modellen trainen op een extern cluster
> * Trainingsresultaten bekijken en het beste model registreren

In [deel twee van deze zelfstudie](tutorial-deploy-models-with-aml.md) leert u hoe u een model selecteert en dit implementeert. 

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="get-the-notebook"></a>De notebook ophalen

Voor uw gemak is deze zelfstudie beschikbaar gemaakt als een [Jupyter-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/01.train-models.ipynb). Voer het `01.train-models.ipynb`-notebook uit in Azure Notebooks of op uw eigen Jupyter-notebookserver.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

>[!NOTE]
> In deze zelfstudie is getest met Azure Machine Learning SDK versie 0.168 

## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen

De configuratie van uw ontwikkelomgeving kan worden uitgevoerd met een Python-notebook.  De configuratie omvat het volgende:

* Python-pakketten importeren
* Verbinding maken met een werkruimte om communicatie tussen uw lokale computer en externe resources mogelijk te maken
* Experiment maken om alle uitvoeringen (runs) bij te houden
* Een extern rekendoel voor training

### <a name="import-packages"></a>Pakketten importeren

Importeer de Python-pakketten die u nodig hebt in deze sessie. Controleer ook wat de versie is van de SDK voor Azure Machine Learning.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

import azureml
from azureml.core import Workspace, Run

# check core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="connect-to-workspace"></a>Verbinding maken met werkruimte

Maak een werkruimte-object van de bestaande werkruimte. `Workspace.from_config()` leest het bestand **config.json** en laadt de gegevens in een object met de naam `ws`.

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, ws.location, sep = '\t')
```

### <a name="create-experiment"></a>Experiment maken

Maak een experiment voor het volgen van de runs in uw werkruimte. Een werkruimte kan meerdere experimenten bevatten. 

```python
experiment_name = 'sklearn-mnist'

from azureml.core import Experiment
exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-remote-compute-target"></a>Extern rekendoel maken

Azure Batch AI is een beheerde service waarmee gegevenswetenschappers machine learning-modellen kunnen trainen op clusters met virtuele Azure-machines, inclusief VM's met GPU-ondersteuning.  In deze zelfstudie maakt u een Azure Batch AI-cluster als uw trainingsomgeving. Met deze code wordt een cluster voor u gemaakt als dit niet nog niet bestaat in uw werkruimte. 

 **Het maken van het cluster duurt ongeveer 5 minuten.** Als het cluster al aanwezig is in de werkruimte, wordt het cluster gebruikt door deze code en wordt er geen nieuw cluster gemaakt.


```python
from azureml.core.compute import ComputeTarget, BatchAiCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
batchai_cluster_name = "traincluster"

try:
    # look for the existing cluster by name
    compute_target = ComputeTarget(workspace=ws, name=batchai_cluster_name)
    if type(compute_target) is BatchAiCompute:
        print('found compute target {}, just use it.'.format(batchai_cluster_name))
    else:
        print('{} exists but it is not a Batch AI cluster. Please choose a different name.'.format(batchai_cluster_name))
except ComputeTargetException:
    print('creating a new compute target...')
    compute_config = BatchAiCompute.provisioning_configuration(vm_size="STANDARD_D2_V2", # small CPU-based VM
                                                                #vm_priority='lowpriority', # optional
                                                                autoscale_enabled=True,
                                                                cluster_min_nodes=0, 
                                                                cluster_max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, batchai_cluster_name, compute_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it uses the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
    # Use the 'status' property to get a detailed status for the current cluster. 
    print(compute_target.status.serialize())
```

U beschikt nu over de vereiste pakketten en rekenresources voor het trainen van een model in de cloud. 

## <a name="explore-data"></a>Gegevens verkennen

Voordat u een model gaat trainen, is het belangrijk dat u de gegevens begrijpt die u gebruikt voor het trainen.  U moet deze gegevens ook naar de cloud kopiëren, zodat ze toegankelijk zijn voor uw trainingsomgeving in de cloud.  In dit gedeelte leert u het volgende:

* De MNIST-gegevensset downloaden
* Enkele voorbeeldafbeeldingen weergeven
* Gegevens uploaden naar de cloud

### <a name="download-the-mnist-dataset"></a>De MNIST-gegevensset downloaden

Download de MNIST-gegevensset en sla de bestanden op in een lokale map `data`.  Er worden afbeeldingen en labels gedownload voor trainings- en testdoeleinden.  


```python
import os
import urllib.request

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename='./data/train-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename='./data/train-labels.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename='./data/test-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename='./data/test-labels.gz')
```

### <a name="display-some-sample-images"></a>Enkele voorbeeldafbeeldingen weergeven

Laad de gecomprimeerde bestanden in `numpy`-matrices. Gebruik vervolgens `matplotlib` om 30 willekeurige afbeeldingen uit de gegevensset te tekenen, met de bijbehorende labels erboven. Voor deze stap hebt u een functie `load_data` nodig die opgenomen in het bestand `util.py`. Dit bestand staat in de map met voorbeelden. Zorg ervoor dat u het bestand in de map met dit notebook zet. De functie `load_data` parseert de gecomprimeerde bestanden in numpy-matrices.



```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data('./data/train-images.gz', False) / 255.0
y_train = load_data('./data/train-labels.gz', True).reshape(-1)

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

# now let's show some randomly chosen images from the traininng set.
count = 0
sample_size = 30
plt.figure(figsize = (16, 6))
for i in np.random.permutation(X_train.shape[0])[:sample_size]:
    count = count + 1
    plt.subplot(1, sample_size, count)
    plt.axhline('')
    plt.axvline('')
    plt.text(x=10, y=-10, s=y_train[i], fontsize=18)
    plt.imshow(X_train[i].reshape(28, 28), cmap=plt.cm.Greys)
plt.show()
```

Er wordt een steekproef van afbeeldingen weergegeven:

![steekproef van afbeeldingen](./media/tutorial-train-models-with-aml/digits.png)

U hebt nu een beter beeld van hoe deze afbeeldingen eruit zien en wat u voor resultaat kunt verwachten van de voorspelling.

### <a name="upload-data-to-the-cloud"></a>Gegevens uploaden naar de cloud

U kunt de gegevens nu op afstand toegankelijk maken door ze vanaf uw lokale computer te uploaden naar Azure, zodat de gegevens kunnen worden gebruikt voor externe training. Het gegevensarchief is een handige structuur die is gekoppeld aan uw werkruimte voor het uploaden/downloaden van gegevens en voor interactie met de gegevens vanaf uw externe rekendoelen. Het archief is toegankelijk met uw Azure Blob Storage-account.

De MNIST-bestanden worden geüpload naar een map met de naam `mnist` in de hoofdmap van het gegevensarchief.

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)

ds.upload(src_dir='./data', target_path='mnist', overwrite=True, show_progress=True)
```
U hebt nu alles wat u nodig hebt om een model te gaan trainen. 

## <a name="train-a-model-locally"></a>Een model lokaal trainen

U gaat een eenvoudig logistiek regressiemodel lokaal trainen vanuit scikit-learn.

**Lokaal trainen duurt een minuut of twee**, afhankelijk van de configuratie van uw computer.

```python
%%time
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
```

Vervolgens doet u voorspellingen aan de hand van de testset en berekent u de nauwkeurigheid. 

```python
y_hat = clf.predict(X_test)
print(np.average(y_hat == y_test))
```

De nauwkeurigheid van het lokale model wordt weergegeven:

`0.9202`

Met slechts een paar regels code hebt u een nauwkeurigheid van 92%.

## <a name="train-on-a-remote-cluster"></a>Trainen op een extern cluster

U kunt dit eenvoudige model uitbreiden door een model met een andere regularisatiefrequentie te bouwen. Dit keer traint u het model op een externe resource.  

Voor deze taak verstuurt u de taak naar het cluster voor externe training dat u eerder hebt ingesteld.  Om een taak te verzenden, moet u het volgende doen:
* Een map maken
* Een trainingsscript maken
* Een estimator maken
* De taak verzenden 

### <a name="create-a-directory"></a>Een map maken

Maak een map om de benodigde code vanaf uw computer aan te bieden aan de externe resource.

```python
import os
script_folder = './sklearn-mnist'
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>Een trainingsscript maken

Als u de taak naar het cluster wilt verzenden, moet u eerst een trainingsscript maken. Voer de volgende code uit om in de map die u net hebt gemaakt een trainingsscript te maken met de naam `train.py`. Met deze training wordt een regularisatiefrequentie toegevoegd aan het trainingsalgoritme, en produceert dus een iets ander model dan de lokale versie.

```python
%%writefile $script_folder/train.py

import argparse
import os
import numpy as np

from sklearn.linear_model import LogisticRegression
from sklearn.externals import joblib

from azureml.core import Run
from utils import load_data

# let user feed in 2 parameters, the location of the data files (from datastore), and the regularization rate of the logistic regression model
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = os.path.join(args.data_folder, 'mnist')
print('Data folder:', data_folder)

# load train and test set into numpy arrays
# note we scale the pixel intensity values to 0-1 (by dividing it with 255.0) so the model can converge faster.
X_train = load_data(os.path.join(data_folder, 'train-images.gz'), False) / 255.0
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_train = load_data(os.path.join(data_folder, 'train-labels.gz'), True).reshape(-1)
y_test = load_data(os.path.join(data_folder, 'test-labels.gz'), True).reshape(-1)
print(X_train.shape, y_train.shape, X_test.shape, y_test.shape, sep = '\n')

# get hold of the current run
run = Run.get_context()

print('Train a logistic regression model with regularizaion rate of', args.reg)
clf = LogisticRegression(C=1.0/args.reg, random_state=42)
clf.fit(X_train, y_train)

print('Predict the test set')
y_hat = clf.predict(X_test)

# calculate accuracy on the prediction
acc = np.average(y_hat == y_test)
print('Accuracy is', acc)

run.log('regularization rate', np.float(args.reg))
run.log('accuracy', np.float(acc))

os.makedirs('outputs', exist_ok=True)
# note file saved in the outputs folder is automatically uploaded into experiment record
joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')
```

U ziet hoe met het script gegevens worden opgehaald en modellen worden opgeslagen:

+ Het trainingsscript leest een argument om de map met de gegevens te vinden.  Als u de taak later verstuurt, wijst u naar het gegevensarchief voor dit argument: `parser.add_argument('--data-folder', type = str, dest = 'data_folder', help = 'data directory mounting point')`

    
+ Het trainingsscript slaat uw model op in een map met de naam outputs. <br/>
`joblib.dump(value = clf, filename = 'outputs/sklearn_mnist_model.pkl')`<br/>
Alles gegevens die naar deze map worden geschreven, worden automatisch geüpload naar uw werkruimte. Verderop in de zelfstudie gaat u dit model openen vanuit deze map.

Vanuit het trainingsscript wordt verwezen naar het bestand `utils.py` om de gegevensset juist te laden.  Kopieer dit script naar de map script, zodat het samen met het trainingsscript vanaf de externe resource kan worden geopend.


```python
import shutil
shutil.copy('utils.py', script_folder)
```


### <a name="create-an-estimator"></a>Een estimator maken

Er wordt een estimator-object gebruikt om de run te verzenden.  Maak de estimator door de volgende code uit te voeren en deze items te definiëren:

* De naam van het estimator-object, `est`
* De map met uw scripts. Alle bestanden in deze map worden naar de clusterknooppunten geüpload voor uitvoering. 
* Het rekendoel.  In dit geval gebruikt u het Batch AI-cluster dat u hebt gemaakt.
* De naam van het trainingsscript, train.py
* Vereiste parameters uit het trainingsscript 
* Python-pakketten die nodig zijn voor training

In deze zelfstudie is dit doel het Batch AI-cluster. Alle bestanden in de projectmap worden naar de clusterknooppunten geüpload voor uitvoering. De data_folder is ingesteld voor gebruik van het gegevensarchief (`ds.as_mount()`).

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

est = Estimator(source_directory=script_folder,
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py',
                conda_packages=['scikit-learn'])
```


### <a name="submit-the-job-to-the-cluster"></a>De taak verzenden naar het cluster

Voer het experiment uit door het estimator-object in te dienen.

```python
run = exp.submit(config=est)
run
```

Aangezien de aanroep asynchroon is, retourneert deze een status **Preparing** of **Running** zodra de taak is gestart.

## <a name="monitor-a-remote-run"></a>Een externe run controleren

In totaal duurt de eerste run **ongeveer tien minuten**. Voor latere runs wordt dezelfde afbeelding echter opnieuw gebruikt waardoor het opstarten van de container veel sneller gaat. Dit is overigens alleen zo als de scriptafhankelijkheden niet zijn gewijzigd.

Dit is wat er gebeurt terwijl u wacht:

- **Maken van afbeelding**: er wordt een Docker-afbeelding gemaakt die overeenkomt met de Python-omgeving die is opgegeven met de estimator. De afbeelding wordt naar de werkruimte geüpload. Het maken en uploaden van de afbeelding duurt **circa vijf minuten**. 

  Deze fase vindt eenmaal plaats voor elke Python-omgeving, aangezien de container voor volgende runs in de cache wordt opgeslagen.  Tijdens het maken van de afbeelding, worden er logboeken gestreamd naar de uitvoeringsgeschiedenis. U kunt de voortgang van het maken van afbeeldingen volgen aan de hand van deze logboeken.

- **Schalen**: als het externe cluster meer knooppunten vereist voor het uitvoeren van de uitvoering dan er momenteel beschikbaar zijn, worden er automatisch extra knooppunten toegevoegd. Het schalen duurt meestal **ongeveer vijf minuten.**

- **Uitvoering**: in deze fase worden de noodzakelijke scripts en bestanden verzonden naar het rekendoel, waarna er gegevensarchieven worden gekoppeld/gekopieerd en ten slotte entry_script wordt uitgevoerd. Terwijl de taak wordt uitgevoerd, worden stdout en de map ./logs naar de uitvoeringsgeschiedenis gestreamd. U kunt de voortgang van de rum volgen aan de hand van deze logboeken.

- **Nabewerking**: de map ./outputs van de run wordt naar de uitvoeringsgeschiedenis in uw werkruimte gekopieerd, zodat u deze resultaten kunt bekijken.


U kunt de voortgang van een actieve taak op verschillende manieren controleren. In deze zelfstudie wordt een Jupyter-widget, evenals de methode `wait_for_completion`. 

### <a name="jupyter-widget"></a>Jupyter-widget

U kunt de voortgang van de rum volgen met een Jupyter-widget.  Net als het indienen van de run, is de widget asynchroon en biedt deze elke 10-15 seconden live updates totdat de taak is voltooid.


```python
from azureml.train.widgets import RunDetails
RunDetails(run).show()
```

Hier ziet u een momentopname van de widget zoals deze wordt weergegeven aan het einde van de training:

![notebook-widget](./media/tutorial-train-models-with-aml/widget.png)

### <a name="get-log-results-upon-completion"></a>Resultaten van logboeken weergeven bij voltooiing

Het trainen en controleren van het model gebeurt op de achtergrond. Wacht totdat het trainen van het model is voltooid voordat u andere code uitvoert. Gebruik `wait_for_completion` om de resultaten weer te geven wanneer het trainen van het model is voltooid. 


```python
run.wait_for_completion(show_output=False) # specify True for a verbose log
```

### <a name="display-run-results"></a>Resultaten van run weergeven

U hebt nu een model dat is getraind op een extern cluster.  De nauwkeurigheid van het model opvragen:

```python
print(run.get_metrics())
```
De uitvoer laat zien dat het externe model een nauwkeurigheid heeft die iets hoger is dan die van het lokale model, dankzij het toevoegen van de regularisatiefrequentie tijdens het trainen.  

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

In de zelfstudie over implementatie wordt dit model in meer detail besproken.

## <a name="register-model"></a>Model registreren

Met de laatste stap in het trainingsscript wordt het bestand `outputs/sklearn_mnist_model.pkl` weggeschreven naar een map met de naam `outputs` op de VM van het cluster waarop de taak wordt uitgevoerd. `outputs` is een speciale map omdat alle inhoud in deze map automatisch wordt geüpload naar uw werkruimte.  Deze inhoud wordt opgenomen in de uitvoeringsrecord in het experiment, onder uw werkruimte. Daardoor is het modelbestand nu ook beschikbaar in uw werkruimte.

U kunt zien welke bestanden aan die run zijn gekoppeld.

```python
print(run.get_file_names())
```

Registreer het model in de werkruimte, zodat u (of anderen) het model later kunnen bevragen, onderzoeken en implementeren.

```python
# register model 
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep = '\t')
```

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

U kunt ook gewoon alleen het Azure Managed Compute-cluster verwijderen. Aangezien automatische schaalaanpassing is ingeschakeld en het clusterminimum 0 is, worden er voor deze specifieke resource geen extra rekenkosten in rekening gebracht wanneer deze niet in gebruik is.


```python
# optionally, delete the Azure Managed Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie over de Azure Machine Learning-service hebt u Python gebruikt om het volgende te doen:

> [!div class="checklist"]
> * De ontwikkelomgeving instellen
> * De gegevens downloaden en controleren
> * Een eenvoudig logistiek regressiemodel trainen met behulp van de populaire bibliotheek voor machine learning scikit-learn.
> * Meerdere modellen trainen op een extern cluster
> * Trainingsgegevens bekijken en het beste model registreren

U kunt dit geregistreerde model nu implementeren aan de hand van de instructies in het volgende deel van de reeks zelfstudies:

> [!div class="nextstepaction"]
> [Zelfstudie 2: Modellen implementeren](tutorial-deploy-models-with-aml.md)
