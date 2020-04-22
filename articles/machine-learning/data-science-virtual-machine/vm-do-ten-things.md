---
title: Windows'da verileri ve modeli keşfedin
titleSuffix: Azure Data Science Virtual Machine
description: Windows Veri Bilimi Sanal Makine'de veri arama ve modelleme görevleri gerçekleştirin.
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0f42f075f5d3be4486157334403bfa7d3f1aa80c
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682869"
---
# <a name="ten-things-you-can-do-on-the-windows-data-science-virtual-machine"></a>Windows Veri Bilimi Sanal Makine'de yapabileceğiniz on şey

Windows Veri Bilimi Sanal Makine (DSVM), veri arama ve modelleme görevlerini gerçekleştirebileceğiniz güçlü bir veri bilimi geliştirme ortamıdır. Ortam, şirket içi, bulut veya karma dağıtımlar için analizlerinizi başlatmayı kolaylaştıran çeşitli popüler veri analizi araçlarıyla birlikte zaten oluşturulmuş ve birlikte gelir. 

DSVM Azure hizmetleriyle yakın çalışır. Azure'da zaten depolanmış olan verileri Azure SQL Veri Ambarı, Azure Veri Gölü, Azure Depolama veya Azure Cosmos DB'de okuyabilir ve işleyebilir. Ayrıca Azure Machine Learning ve Azure Veri Fabrikası gibi diğer analiz araçlarından da yararlanabilir.

Bu makalede, dsvm'nizi veri bilimi görevlerini gerçekleştirmek ve diğer Azure hizmetleriyle etkileşimde kalmak için nasıl kullanacağınızı öğreneceksiniz. DSVM'de yapabileceklerinden bazıları şunlardır:

- Microsoft Machine Learning Server ve Python'u kullanarak verileri keşfedin ve DSVM'de yerel olarak modeller geliştirin.
- Python 2, Python 3 ve Microsoft R'yi kullanarak tarayıcıdaki verilerinizi denemek için bir Jupyter dizüstü bilgisayar kullanın (Microsoft R, performans için tasarlanmış kurumsal kullanıma hazır bir R sürümüdür.)
- Müşteri uygulamalarının basit bir web hizmeti arabirimi kullanarak modellerinize erişebilmeleri için Azure Machine Learning'de R ve Python aracılığıyla oluşturulmuş modelleri dağıtın.
- Azure kaynaklarınızı Azure portalını veya PowerShell'i kullanarak yönetin.
- DSVM'nizde monte edilebilir bir sürücü olarak bir Azure Files paylaşımı oluşturarak depolama alanınızı genişletin ve büyük ölçekli veri kümelerini/kodunuzu tüm ekibiniz genelinde paylaşın.
- GitHub'ı kullanarak ekibinizle kod paylaşın. Önceden yüklenmiş Git istemcilerini kullanarak deponuza erişin: Git Bash ve Git GUI.
- Azure Blob depolama, Azure Veri Gölü, Azure HDInsight (Hadoop), Azure Cosmos DB, Azure SQL Veri Ambarı ve Azure SQL Veritabanı gibi Azure veri ve analiz hizmetlerine erişin.
- DSVM'de önceden yüklenmiş Power BI Desktop örneğini kullanarak raporlar ve pano oluşturun ve bunları bulutta dağıtın.
- DSVM'nizi projenizin gereksinimlerini karşılamak için dinamik olarak ölçeklendirin.
- Sanal makinenize ek araçlar yükleyin.   

> [!NOTE]
> Bu makalede listelenen veri depolama ve analiz hizmetlerinin çoğu için ek kullanım ücretleri uygulanır. Ayrıntılar için [Azure fiyatlandırma](https://azure.microsoft.com/pricing/) sayfasına bakın.
> 
> 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğine ihtiyacınız var. Ücretsiz [deneme sürümü için kaydolabilirsiniz.](https://azure.microsoft.com/free/)
* Azure portalında Veri Bilimi Sanal Makinesi sağlama yönergeleri [sanal makine oluşturmada](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016)kullanılabilir.


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="explore-data-and-develop-models-with-microsoft-machine-learning-server"></a>Microsoft Machine Learning Server ile verileri keşfedin ve modeller geliştirin
Veri analitiğinizi DSVM'de yapmak için R ve Python gibi dilleri kullanabilirsiniz.

R için, başlat menüsünde veya masaüstünde bulunan RStudio gibi bir IDE kullanabilirsiniz. Ya da Visual Studio için R Tools kullanabilirsiniz. Microsoft, ölçeklenebilir analitik ve paralel yığınlı çözümlemede izin verilen bellek boyutundan daha büyük verileri çözümleme olanağı sağlamak için açık kaynak CRAN R'nin üstüne ek kitaplıklar sağlamıştır. 

Python için, Visual Studio için Python Araçları (PTVS) uzantısı önceden yüklenmiş olan Visual Studio Community Edition gibi bir IDE kullanabilirsiniz. Varsayılan olarak, yalnızca Python 3.6, kök Conda ortamı, PTVS üzerinde yapılandırılır. Anaconda Python 2.7'yi etkinleştirmek için aşağıdaki adımları izleyin:

1. **Araçlar** > **Python Araçları** > **Python Ortamları'na**giderek ve Visual Studio Community Edition'da **+ Özel'i** seçerek her sürüm için özel ortamlar oluşturun.
1. Anaconda Python 2.7 için açıklama verin ve ortam öneki yolunu **c:\anaconda\envs\python2** olarak ayarlayın.
1. Ortamı kaydetmek için Otomatik > **Algıla'yı** seçin. **Auto Detect**

Python ortamlarının nasıl oluşturulabildiğini hakkında daha fazla bilgi için [PTVS belgelerine](https://aka.ms/ptvsdocs) bakın.

Şimdi yeni bir Python projesi oluşturmak için ayarlandınız. **Dosya** > **Yeni** > **Proje** > **Python'a** gidin ve oluşturmakta olduğunuz Python uygulaması türünü seçin. Python **ortamlarını** sağ tıklayıp Python **Ortamlarını Kaldır/Kaldır'ı**seçerek geçerli proje için Python ortamını istenilen sürüme (Python 2.7 veya 3.6) ayarlayabilirsiniz. [Ürün belgelerinde](https://aka.ms/ptvsdocs)PTVS ile çalışma hakkında daha fazla bilgi bulabilirsiniz.

## <a name="use-jupyter-notebooks"></a>Jupyter not defterlerini kullanma
Jupyter Notebook, veri arama ve modelleme için tarayıcı tabanlı bir IDE sağlar. Python 2, Python 3 veya R 'yi (hem açık kaynak hem de Microsoft R Server) bir Jupyter not defterinde kullanabilirsiniz.

Jupyter Notebook'u başlatmak için **Başlat** menüsünde veya masaüstünde **Jupyter Notebook** simgesini seçin. DSVM komut isteminde, komutu ```jupyter notebook``` varolan not defterlerinin bulunduğu veya yeni not defterleri oluşturmak istediğiniz dizinden de çalıştırabilirsiniz.  

Jupyter'ı başlattıktan sonra, DSVM'ye önceden paketlenmiş birkaç örnek not defteri içeren bir dizin görmeniz gerekir. Artık şunları yapabilirsiniz:

* Kodu görmek için not defterini seçin.
* Shift+Enter'u seçerek her hücreyi çalıştırın.
* **Hücre** > Çalıştır'ı seçerek tüm not defterini**çalıştırın.**
* Jupyter simgesini (sol üst köşede) seçerek, sağdaki **Yeni** düğmesini seçerek ve ardından not defteri dilini (çekirdek olarak da bilinir) seçerek yeni bir not defteri oluşturun.   

> [!NOTE]
> Şu anda Python 2.7, Python 3.6, R, Julia ve PySpark çekirdekleri Jupyter'da desteklenmiştir. R çekirdeği hem açık kaynak R hem de Microsoft R programlamayı destekler.   
> 
> 

Not defterindeyken, seçtiğiniz kitaplıkları kullanarak verilerinizi keşfedebilir, modeli oluşturabilir ve modeli test edebilirsiniz.

## <a name="train-and-deploy-models-by-using-azure-machine-learning"></a>Azure Machine Learning'i kullanarak modelleri eğitin ve dağıtın
Modelinizi oluşturup doğruladıktan sonra, bir sonraki adım genellikle modeli üretime dağıtmaktır. Bu adım, istemci uygulamalarınızın model tahminlerini gerçek zamanlı veya toplu iş modu temelinde çağırmasına olanak tanır. Azure Machine Learning, R veya Python'da yerleşik bir modeli işlevsel hale getirmek için bir mekanizma sağlar.

Azure Machine Learning'de modelinizi işletime dahil ettiğinizde, bir web hizmeti açığa çıkarır. Müşterilerin giriş parametrelerini geçen REST çağrıları yapmalarına ve modelden çıktı olarak öngörüler almalarına olanak tanır.

### <a name="build-and-operationalize-python-models"></a>Python modellerini oluşturma ve operasyonel hale
Python Jupyter not defterinde geliştirilen ve Scikit-learn kitaplığını kullanarak basit bir model oluşturan bir kod parçası aşağıda vermiştir:

```python
# IRIS classification
from sklearn import datasets
from sklearn import svm
clf = svm.SVC()
iris = datasets.load_iris()
X, y = iris.data, iris.target
clf.fit(X, y)
```

Python modellerinizi Azure Machine Learning'e dağıtmak için kullanılan yöntem, modelin tahminini bir işleve sarar ve önceden yüklenmiş Azure Machine Learning Python kitaplığı tarafından sağlanan özelliklerle süsler. Öznitelikler, Azure Machine Learning çalışma alanı kimliğinizi, API anahtarınızı ve giriş ve iade parametrelerinizi gösterir.  

```python
from azureml import services
@services.publish(workspaceid, auth_token)
@services.types(sep_l=float, sep_w=float, pet_l=float, pet_w=float)
@services.returns(int)  # 0, or 1, or 2
def predictIris(sep_l, sep_w, pet_l, pet_w):
    inputArray = [sep_l, sep_w, pet_l, pet_w]


return clf.predict(inputArray)
```

Bir istemci artık web hizmetine çağrı yapabilir. Kolaylık sarmalayıcıları REST API isteklerini oluşturmak. Web hizmetini tüketmek için örnek kod aşağıda vesiadır:

```python
# Consume through web service URL and keys
from azureml import services
@services.service(url, api_key)
@services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
@services.returns(float)
def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
pass

IrisPredictor(3,2,3,4)
```

> [!NOTE]
> Şu anda Azure Machine Learning kitaplığı yalnızca Python 2.7'de desteklenir.   
> 
> 

### <a name="build-and-operationalize-r-models"></a>R modelleri oluşturma ve operasyonel hale
Veri Bilimi Sanal Makinesi'nde veya başka bir yerde yerleşik R modellerini Python için yapılana benzer bir şekilde Azure Machine Learning'e dağıtabilirsiniz. Adımlar aşağıdaki gibidir:

1. Çalışma alanı kimliğinizi ve kimlik doğrulama belirtecinizi sağlamak için bir settings.json dosyası oluşturun. 
2. Modelin tahmin işlevi için bir sarmalayıcı yazın.
3. İşlev sarıcıda geçmek için Azure Machine Learning kitaplığını arayın. ```publishWebService```  

Azure Machine Learning'de bir web hizmeti olarak bir modeli ayarlamak, oluşturmak, yayımlamak ve kullanmak için aşağıdaki yordam ve kod parçacıklarını kullanın.

#### <a name="set-up"></a>Ayarla

Ev dizininizin altında çağrılan ```.azureml``` bir dizinin altında settings.json dosyası oluşturun. Azure Machine Learning çalışma alanınızdan parametreleri girin.

Burada settings.json dosya yapısı:

```json
{"workspace":{
"id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
"authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
}}
```

#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a>R modeli oluşturun ve Azure Machine Learning'de yayımlayın

```r
library(AzureML)
ws <- workspace(config="~/.azureml/settings.json")

if(!require("lme4")) install.packages("lme4")
library(lme4)
set.seed(1)
train <- sleepstudy[sample(nrow(sleepstudy), 120),]
m <- lm(Reaction ~ Days + Subject, data = train)

# Define a prediction function to publish based on the model:
sleepyPredict <- function(newdata){
        predict(m, newdata=newdata)
}

ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)
```

#### <a name="consume-the-model-deployed-in-azure-machine-learning"></a>Azure Machine Learning'de dağıtılan modeli tüketin
Bir istemci uygulamasından modeli tüketmek için, yayınlanan web hizmetini adıyla aramak için Azure Machine Learning kitaplığını kullanın. Bitiş `services` noktasını belirlemek için API çağrısını kullanın. Sonra sadece `consume` işlevi arayın ve tahmin edilecek veri çerçevesi içinde geçmek.

Azure Machine Learning web hizmeti olarak yayınlanan modeli kullanmak için aşağıdaki kodu kullanın:

```r
library(AzureML)
library(lme4)
ws <- workspace(config="~/.azureml/settings.json")

s <-  services(ws, name = "sleepy lm")
s <- tail(s, 1) # use the last published function, in case of duplicate function names

ep <- endpoints(ws, s)

# Try this out, and compare with raw data
ans = consume(ep, sleepstudy)$ans
```

Machine Learning [Studio'daki R paketleri](/azure/machine-learning/studio-module-reference/r-packages-supported-by-azure-machine-learning)hakkında daha fazla bilgi için.

## <a name="manage-azure-resources"></a>Azure kaynaklarını yönetme
DSVM yalnızca analiz çözümünüzü sanal makinede yerel olarak oluşturmanıza izin vermez. Ayrıca Azure bulut platformundaki hizmetlere erişmenizi de sağlar. Azure, DSVM'inizden yönetebileceğiniz ve erişebileceğiniz birkaç bilgi işlem, depolama, veri analitiği ve diğer hizmetler sağlar.

Azure aboneliğinizi ve bulut kaynaklarınızı yönetmek için iki seçeneğiniz var:
+ Tarayıcınızı kullanın ve [Azure portalına](https://portal.azure.com)gidin.

+ PowerShell komut dosyalarını kullanın. Azure PowerShell'i masaüstündeki bir kısayoltan veya **Başlat** menüsünden çalıştırın. Tüm ayrıntılar için [Microsoft Azure PowerShell belgelerine](../../powershell-azure-resource-manager.md) bakın. 

## <a name="extend-storage-by-using-shared-file-systems"></a>Paylaşılan dosya sistemlerini kullanarak depolamaalanını genişletme
Veri bilimciler takım içinde büyük veri kümelerini, kodu veya diğer kaynakları paylaşabilir. DSVM'de yaklaşık 45 GB alan mevcuttur. Depolama alanınızı genişletmek için Azure Dosyaları'nı kullanabilir ve bir veya daha fazla DSVM örneğine monte edebilir veya REST API üzerinden erişebilirsiniz. [Ayrıca Azure portalını](../../virtual-machines/windows/attach-managed-disk-portal.md) kullanabilir veya azure [powershell'i](../../virtual-machines/windows/attach-disk-ps.md) kullanarak fazladan özel veri diskleri ekleyebilirsiniz. 

> [!NOTE]
> Azure Dosyaları paylaşımındaki maksimum alan 5 TB'dir. Her dosya için boyut sınırı 1 TB'dir. 

Azure Dosyaları paylaşımı oluşturmak için Azure PowerShell'de bu komut dosyasını kullanabilirsiniz:

```powershell
# Authenticate to Azure.
Connect-AzAccount
# Select your subscription
Get-AzSubscription –SubscriptionName "<your subscription name>" | Select-AzSubscription
# Create a new resource group.
New-AzResourceGroup -Name <dsvmdatarg>
# Create a new storage account. You can reuse existing storage account if you want.
New-AzStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
# Set your current working storage account
Set-AzCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

# Create an Azure Files share
$s = New-AzStorageShare <<teamsharename>>
# Create a directory under the file share. You can give it any name
New-AzStorageDirectory -Share $s -Path <directory name>
# List the share to confirm that everything worked
Get-AzStorageFile -Share $s
```

Artık bir Azure Files paylaşımı oluşturduğunuza göre, azure'daki herhangi bir sanal makineye monte edebilirsiniz. Gecikme ve veri aktarım ücretlerini önlemek için VM'yi depolama hesabıyla aynı Azure veri merkezine koymanızı öneririz. Sürücüyü DSVM'ye monte etmek için Azure PowerShell komutları aşağıda vereb

```powershell
# Get the storage key of the storage account that has the Azure Files share from the Azure portal. Store it securely on the VM to avoid being prompted in the next command.
cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

# Mount the Azure Files share as drive Z on the VM. You can choose another drive letter if you want.
net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>
```

Şimdi VM herhangi bir normal sürücü gibi bu sürücü erişebilirsiniz.

## <a name="share-code-in-github"></a>Kodu GitHub'da paylaşın
GitHub, geliştirici topluluğu tarafından paylaşılan teknolojileri kullanarak çeşitli araçlar için kod örnekleri ve kaynakları bulabileceğiniz bir kod deposudur. Kod dosyalarının sürümlerini izlemek ve depolamak için Git'i teknoloji olarak kullanır. GitHub aynı zamanda ekibinizin paylaşılan kodunu ve belgelerini depolamak, sürüm denetimini uygulamak ve koda kimlerin erişebileceğini kontrol etmek için kendi deponuzu oluşturabileceğiniz bir platformdur. 

Git'i kullanma hakkında daha fazla bilgi için [GitHub yardım sayfalarını](https://help.github.com/) ziyaret edin. GitHub'ı ekibinizle işbirliği yapmanın, topluluk tarafından geliştirilen kodu kullanmanın ve topluluğa kod katkıda bulunmanın yollarından biri olarak kullanabilirsiniz.

DSVM, GitHub deposuna erişmek için komut satırında ve GUI'de istemci araçlarıyla birlikte gelir. Git ve GitHub ile çalışan komut satırı aracı git bash olarak adlandırılır. Visual Studio DSVM yüklü ve Git uzantıları vardır. Bu araçlar için simgeleri **Başlat** menüsünde ve masaüstünde bulabilirsiniz.

GitHub deposundan kod indirmek için komutu ```git clone``` kullanırsınız. Örneğin, Microsoft tarafından yayınlanan veri bilimi deposunu geçerli dizine indirmek için Git Bash'te aşağıdaki komutu çalıştırabilirsiniz:

    git clone https://github.com/Azure/DataScienceVM.git

Visual Studio'da, aynı klon işlemini yapabilirsiniz. Aşağıdaki ekran görüntüsü Visual Studio'da Git ve GitHub araçlarına nasıl erişilir:

![GitHub bağlantısı görüntülenen Visual Studio ekran görüntüsü](./media/vm-do-ten-things/VSGit.PNG)

github.com'da bulunan kaynaklardan GitHub deponuzla çalışmak için Git'i kullanma hakkında daha fazla bilgi bulabilirsiniz. [Hile sayfası](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf) yararlı bir referanstır.

## <a name="access-azure-data-and-analytics-services"></a>Azure veri ve analiz hizmetlerine erişin
### <a name="azure-blob-storage"></a>Azure Blob depolama
Azure Blob depolama, büyük ve küçük veriler için güvenilir ve ekonomik bir bulut depolama hizmetidir. Bu bölümde, verileri Blob depolama alanına nasıl taşıyabileceğiniz ve Azure blob'da depolanan verilere nasıl erişebileceğiniz açıklanmaktadır.

#### <a name="prerequisites"></a>Ön koşullar

* [Azure portalından](https://portal.azure.com)Azure Blob depolama hesabınızı oluşturun.

   ![Azure portalındaki depolama hesabı oluşturma işleminin ekran görüntüsü](./media/vm-do-ten-things/Create_Azure_Blob.PNG)

* Komut satırı AzCopy aracının önceden yüklenmiş ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```olduğunu doğrulayın: . Azcopy.exe içeren dizin zaten PATH ortamı değişkeninde, böylece bu aracı çalıştırırken tam komut yolu yazmaktan kaçınabilirsiniz. AzCopy aracı hakkında daha fazla bilgi [için, AzCopy belgelerine](../../storage/common/storage-use-azcopy.md)bakın.
* Azure Depolama Gezgini aracını başlatın. [Depolama Gezgini web sayfasından](https://storageexplorer.com/)indirebilirsiniz. 

   ![Depolama hesabına erişen Azure Depolama Gezgini ekran görüntüsü](./media/vm-do-ten-things/AzureStorageExplorer_v4.png)

#### <a name="move-data-from-a-vm-to-an-azure-blob-azcopy"></a>Verileri VM'den Azure blob'una taşıma: AzCopy

Yerel dosyalarınız ile Blob depolama arasında veri taşımak için komut satırında veya PowerShell'de AzCopy'yi kullanabilirsiniz:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

**C:\myfolder'ı** dosyanızın depolandığı yolla, **mystorageaccount'u** Blob depolama hesap adınız, konteyner adı ile **mycontainer** ve Blob depolama erişim anahtarınızla **depolama hesabı anahtarıyla** değiştirin. [Azure portalında](https://portal.azure.com)depolama hesabı kimlik bilgilerinizi bulabilirsiniz.

AzCopy komutunu PowerShell'de veya komut isteminden çalıştırın. Aşağıdans komutunun bazı örnek kullanımı aşağıda verilmiştir:

```powershell
# Copy *.sql from a local machine to an Azure blob
"C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

# Copy back all files from an Azure blob container to a local machine

"C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S
```

Azure blob'una kopyalamak için AzCopy komutunu çalıştırdıktan sonra dosyanız Azure Depolama Gezgini'nde görünür.

![Yüklenen CSV dosyasını görüntüleme, depolama hesabının ekran görüntüsü](./media/vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

#### <a name="move-data-from-a-vm-to-an-azure-blob-azure-storage-explorer"></a>Verileri VM'den Azure blob'una taşıma: Azure Depolama Gezgini

Azure Depolama Gezgini'ni kullanarak VM'nizdeki yerel dosyadan veri de yükleyebilirsiniz:

* Bir kapsayıcıya veri yüklemek için hedef kapsayıcıyı seçin ve **Yükle** düğmesini seçin. ![Azure Depolama Gezgini'nde yükleme düğmesinin ekran görüntüsü](./media/vm-do-ten-things/storage-accounts.png)
* **Dosyalar** kutusunun sağındaki elipsleri (**...**) seçin, dosya sisteminden yüklemek için bir veya birden çok dosya seçin ve dosyaları yüklemeye başlamak için **Yükle'yi** seçin. ![Yükleme dosyaları iletişim kutusunun ekran görüntüsü](./media/vm-do-ten-things/upload-files-to-blob.png)

#### <a name="read-data-from-an-azure-blob-machine-learning-reader-module"></a>Azure blob'undaki verileri okuma: Machine Learning okuyucu modülü

Azure Machine Learning Studio'da, damlanızdan alınan verileri okumak için Veri Alma modüllerini kullanabilirsiniz.

![Machine Learning Studio'da İthalat Verisi modülünün ekran görüntüsü](./media/vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

#### <a name="read-data-from-an-azure-blob-python-odbc"></a>Azure blob'undaki verileri okuma: Python ODBC

BlobService kitaplığını, jupyter not defterindeki veya Python programındaki bir blob'dan doğrudan veri okumak için kullanabilirsiniz.

İlk olarak, gerekli paketleri almak:

```python
import pandas as pd
from pandas import Series, DataFrame
import numpy as np
import matplotlib.pyplot as plt
from time import time
import pyodbc
import os
from azure.storage.blob import BlobService
import tables
import time
import zipfile
import random
```

Ardından, Blob depolama hesabı kimlik bilgilerinizi takın ve blob'daki verileri okuyun:

```python
CONTAINERNAME = 'xxx'
STORAGEACCOUNTNAME = 'xxxx'
STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
localfilename = 'trip_data_1.csv'
LOCALDIRECTORY = os.getcwd()
LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

#download from blob
t1 = time.time()
blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
t2 = time.time()
print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))

#unzip downloaded files if needed
#with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
#    z.extractall(LOCALDIRECTORY)

df1 = pd.read_csv(LOCALFILE, header=0)
df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
print 'the size of the data is: %d rows and  %d columns' % df1.shape
```

Veriler bir veri çerçevesi olarak okunur:

![İlk 10 veri satırının ekran görüntüsü](./media/vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a>Azure Data Lake
Azure Veri Gölü Depolama, büyük veri analitiği iş yükleri için hiper ölçekli bir depodur ve Hadoop Distributed File System (HDFS) ile uyumludur. Hadoop, Spark ve Azure Veri Gölü Analytics ile çalışır. Bu bölümde, Azure Veri Gölü Analitiği'ni kullanarak verileri Azure Veri Gölü Depolama'ya nasıl taşıyabileceğinizi ve analitiği nasıl çalıştırabileceğinizi öğreneceksiniz.

#### <a name="prerequisites"></a>Ön koşullar

* [Azure portalında](https://portal.azure.com)Azure Veri Gölü Analizi örneğini oluşturun.

   ![Azure portalından Veri Gölü Analizi örneği oluşturma nın ekran görüntüsü](./media/vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* [Visual Studio eklentisi için Azure Veri Gölü ve Akış Analizi Araçları,](https://www.microsoft.com/download/details.aspx?id=49504) Visual Studio Community Edition'da sanal makineye zaten yüklenmiştir. Visual Studio'yu başlattıktan ve Azure aboneliğinizde oturum açındıktan sonra, Azure Veri Analizi hesabınızı ve depolama alanınızı Visual Studio'nun sol panelinde görmeniz gerekir.

   ![Visual Studio'da Data Lake araçları için eklentinin ekran görüntüsü](./media/vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

#### <a name="move-data-from-a-vm-to-data-lake-azure-data-lake-explorer"></a>Verileri VM'den Veri Gölü'ne taşıma: Azure Veri Gölü Gezgini

[Sanal makinenizdeki yerel dosyalardan verileri Veri Gölü Depolama'ya yüklemek için](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal)Azure Veri Gölü Gezgini'ni kullanabilirsiniz.

Azure [Veri Fabrikası'nı](https://azure.microsoft.com/services/data-factory/)kullanarak Veri hareketinizi Azure Veri Gölü'ne veya Azure Veri Gölü'nden işlevsel hale getirmek için bir veri ardışık alanı da oluşturabilirsiniz. [Bu makalede,](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) veri ardışık lıkları oluşturmak için adımlar boyunca size rehberlik eder.

#### <a name="read-data-from-an-azure-blob-to-data-lake-u-sql"></a>Azure blob'undaki verileri Veri Gölü'ne okuma: U-SQL

Verileriniz Azure Blob depolama alanında yaşıyorsa, U-SQL sorgusundaki Azure blob'undaki verileri doğrudan okuyabilirsiniz. U-SQL sorgunuzu oluşturmadan önce Blob depolama hesabınızın Azure Veri Gölü örneğinize bağlı olduğundan emin olun. Azure portalına gidin, Azure Veri Gölü Analizi panonuzu bulun, **Veri Kaynağı Ekle'yi**seçin, Bir depolama türü **Azure Depolaması**seçin ve Azure depolama hesap adınızı ve anahtarınızı takın. Ardından, depolama hesabında depolanan verilere başvuruda bulunabilirsiniz.

![Veri Kaynağı Ekle iletişim kutusunun ekran görüntüsü](./media/vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

Visual Studio'da Blob depolamadaki verileri okuyabilir, verileri işleyebilir, özellikleri tasarlayabilir ve ortaya çıkan verileri Azure Veri Gölü veya Azure Blob depolamasına gönderebilirsiniz. Blob depolamasındaki verilere başvururken, **wasb://** kullanın. Azure Veri Gölü'ndeki verilere başvururken, **swbhdfs://** kullanın.

Visual Studio'da aşağıdaki U-SQL sorgularını kullanabilirsiniz:

```usql
@a =
    EXTRACT medallion string,
            hack_license string,
            vendor_id string,
            rate_code string,
            store_and_fwd_flag string,
            pickup_datetime string,
            dropoff_datetime string,
            passenger_count int,
            trip_time_in_secs double,
            trip_distance double,
            pickup_longitude string,
            pickup_latitude string,
            dropoff_longitude string,
            dropoff_latitude string

    FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
    USING Extractors.Csv();

@b =
    SELECT vendor_id,
    COUNT(medallion) AS cnt_medallion,
    SUM(passenger_count) AS cnt_passenger,
    AVG(trip_distance) AS avg_trip_dist,
    MIN(trip_distance) AS min_trip_dist,
    MAX(trip_distance) AS max_trip_dist,
    AVG(trip_time_in_secs) AS avg_trip_time
    FROM @a
    GROUP BY vendor_id;

OUTPUT @b   
TO "swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
USING Outputters.Csv();

OUTPUT @b   
TO "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
USING Outputters.Csv();
```

Sorgunuz sunucuya gönderildikten sonra, bir diyagram işinizin durumunu gösterir.

![İş durumu diyagramının ekran görüntüsü](./media/vm-do-ten-things/USQL_Job_Status.PNG)

#### <a name="query-data-in-data-lake-u-sql"></a>Veri Gölü'nde sorgu verisi: U-SQL

Veri kümesi Azure Veri Gölü'nde yutuldükten sonra, verileri sorgulamak ve keşfetmek için [U-SQL dilini](../../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) kullanabilirsiniz. U-SQL dili T-SQL'e benzer, ancak c#'dan bazı özellikleri birleştirir, böylece kullanıcılar özelleştirilmiş modüller ve kullanıcı tanımlı işlevler yazabilir. Komut dosyalarını önceki adımda kullanabilirsiniz.

Sorgu sunucuya gönderildikten sonra tripdata_summary. CSV, Azure Veri Gölü Gezgini'nde görünür. Dosyayı sağ tıklayarak verileri önizleyebilirsiniz.

![Data Lake Explorer'daki CSV dosyasının ekran görüntüsü](./media/vm-do-ten-things/USQL_create_summary.png)

Dosya bilgileri görüntülenir:

![Dosya özeti bilgilerinin ekran görüntüsü](./media/vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a>HDInsight Hadoop kümeleri
Azure HDInsight, bulutta yönetilen bir Apache Hadoop, Spark, HBase ve Storm hizmetidir. Veri Bilimi Sanal Makinesi'nden Azure HDInsight kümeleri ile kolayca çalışabilirsiniz.

#### <a name="prerequisites"></a>Ön koşullar

* [Azure portalından](https://portal.azure.com)Azure Blob depolama hesabınızı oluşturun. Bu depolama hesabı HDInsight kümeleri için veri depolamak için kullanılır.

   ![Azure portalından depolama hesabı oluşturma ekran görüntüsü](./media/vm-do-ten-things/Create_Azure_Blob.PNG)

* [Azure portalından](../team-data-science-process/customize-hadoop-cluster.md)Azure HDInsight Hadoop kümelerini özelleştirin.
  
   Oluşturulduğu zaman oluşturulan depolama hesabını HDInsight kümenizle bağla. Bu depolama hesabı küme içinde işlenebilen verilere erişmek için kullanılır.

   ![HDInsight kümesiyle oluşturulan depolama hesabını bağlama ya da seçim](./media/vm-do-ten-things/Create_HDI_v4.PNG)

* Oluşturulduktan sonra kümenin baş düğümüne Uzak Masaüstü erişimini etkinleştirin. Sonraki yordamda bunlara ihtiyacınız olacak, çünkü burada belirttiğiniz uzaktan erişim kimlik bilgilerini unutmayın.

   ![HDInsight kümesine uzaktan erişimi etkinleştirmek için Uzak Masaüstü düğmesi](./media/vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* Azure Machine Learning çalışma alanı oluşturun. Machine Learning denemeleriniz bu Machine Learning çalışma alanında depolanır. Aşağıdaki ekran görüntüsünde gösterildiği gibi portalda vurgulanan seçenekleri seçin:

   ![Azure Machine Learning çalışma alanı oluşturma](./media/vm-do-ten-things/Create_ML_Space.PNG)

* Çalışma alanınız için parametreleri girin.

   ![Makine Öğrenimi çalışma alanı parametrelerini girin](./media/vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* IPython Notebook'u kullanarak veri yükleyin. Gerekli paketleri içe aktarın, kimlik bilgilerini takın, depolama hesabınızda bir veritabanı oluşturun ve ardından verileri HDI kümelerine yükleyin.

```python
# Import required packages
import pyodbc
import time as time
import json
import os
import urllib
import urllib2
import warnings
import re
import pandas as pd
import matplotlib.pyplot as plt
from azure.storage.blob import BlobService
warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


# Create the connection to Hive by using ODBC
SERVER_NAME = 'xxx.azurehdinsight.net'
DATABASE_NAME = 'nyctaxidb'
USERID = 'xxx'
PASSWORD = 'xxxx'
DB_DRIVER = 'Microsoft Hive ODBC Driver'
driver = 'DRIVER={' + DB_DRIVER + '}'
server = 'Host=' + SERVER_NAME + ';Port=443'
database = 'Schema=' + DATABASE_NAME
hiveserv = 'HiveServerType=2'
auth = 'AuthMech=6'
uid = 'UID=' + USERID
pwd = 'PWD=' + PASSWORD
CONNECTION_STRING = ';'.join(
    [driver, server, database, hiveserv, auth, uid, pwd])
connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
cursor = connection.cursor()


# Create the Hive database and tables
queryString = "create database if not exists nyctaxidb;"
cursor.execute(queryString)

queryString = """
                create external table if not exists nyctaxidb.trip
                (
                    medallion string,
                    hack_license string,
                    vendor_id string,
                    rate_code string,
                    store_and_fwd_flag string,
                    pickup_datetime string,
                    dropoff_datetime string,
                    passenger_count int,
                    trip_time_in_secs double,
                    trip_distance double,
                    pickup_longitude double,
                    pickup_latitude double,
                    dropoff_longitude double,
                    dropoff_latitude double)  
                PARTITIONED BY (month int)
                ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
            """
cursor.execute(queryString)

queryString = """
                create external table if not exists nyctaxidb.fare
                (
                    medallion string,
                    hack_license string,
                    vendor_id string,
                    pickup_datetime string,
                    payment_type string,
                    fare_amount double,
                    surcharge double,
                    mta_tax double,
                    tip_amount double,
                    tolls_amount double,
                    total_amount double)
                PARTITIONED BY (month int)
                ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
            """
cursor.execute(queryString)


# Upload data from Blob storage to an HDI cluster
for i in range(1, 13):
    queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);" % (
        i, i)
    cursor.execute(queryString)
    queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);" % (
        i, i)
    cursor.execute(queryString)
```

Alternatif olarak, NYC Taksi verilerini HDI kümesine yüklemek için [bu izbiyi](../team-data-science-process/hive-walkthrough.md) takip edebilirsiniz. Önemli adımlar şunlardır:
  
* Sıkıştırılmış CV'leri ortak blob'dan yerel klasörünüze indirmek için AzCopy'yi kullanın.
* Yerel klasörden HDI kümesine sıkıştırılmamış CV'leri yüklemek için AzCopy'yi kullanın.
* Hadoop kümesinin baş düğümüne giriş yapın ve araştırmacı veri analizi için hazırlanın.

Veriler HDI kümesine yüklendikten sonra verilerinizi Azure Depolama Gezgini'nde denetleyebilirsiniz. Ve nyctaxidb veritabanı HDI kümesinde oluşturuldu.

#### <a name="data-exploration-hive-queries-in-python"></a>Veri arama: Python'da Hive Sorguları

Veriler Hadoop kümesinde olduğundan, fioop kümelerine bağlanmak ve keşif ve özellik mühendisliği yapmak için Hive'ı kullanarak veritabanlarını sorgulamak için pyodbc paketini kullanabilirsiniz. Oluşturduğunuz varolan tabloları ön koşul adımında görüntüleyebilirsiniz.

```python
queryString = """
    show tables in nyctaxidb2;
    """
pd.read_sql(queryString, connection)
```

![Varolan tabloları görüntüleme](./media/vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Her ayki kayıt sayısına ve bahşişli ya da değil'in frekanslarına gezi tablosunda bakalım:

```python
queryString = """
    select month, count(*) from nyctaxidb.trip group by month;
    """
results = pd.read_sql(queryString,connection)

%matplotlib inline

results.columns = ['month', 'trip_count']
df = results.copy()
df.index = df['month']
df['trip_count'].plot(kind='bar')
```

![Her ayki kayıt sayısının çizimi](./media/vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

```python
queryString = """
    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;
    """
results = pd.read_sql(queryString, connection)

results.columns = ['tipped', 'trip_count']
df = results.copy()
df.index = df['tipped']
df['trip_count'].plot(kind='bar')
```

![Uç frekanslarının çizimi](./media/vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

Ayrıca, teslim alma konumu ile bırakma konumu arasındaki mesafeyi hesaplayabilir ve ardından seyahat mesafesi ile karşılaştırabilirsiniz.

```python
queryString = """
                select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                    3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                    *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                    *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                    /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                    +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                    pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                    from nyctaxidb.trip
                    where month=1
                        and pickup_longitude between -90 and -30
                        and pickup_latitude between 30 and 90
                        and dropoff_longitude between -90 and -30
                        and dropoff_latitude between 30 and 90;
            """
results = pd.read_sql(queryString, connection)
results.head(5)
```

![Pikap ve bırakma masasının üst satırları](./media/vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

```python
results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                   'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
df = results.loc[results['trip_distance'] <= 100]  # remove outliers
df = df.loc[df['direct_distance'] <= 100]  # remove outliers
plt.scatter(df['direct_distance'], df['trip_distance'])
```

![Yolculuk mesafesine pikap/bırakma mesafesinin çizimi](./media/vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

Şimdi modelleme için örneklenmiş (yüzde 1) bir veri kümesi hazırlayalım. Bu verileri Machine Learning okuyucu modülünde kullanabilirsiniz.

```python
queryString = """
create  table if not exists nyctaxi_downsampled_dataset_testNEW (
medallion string,
hack_license string,
vendor_id string,
rate_code string,
store_and_fwd_flag string,
pickup_datetime string,
dropoff_datetime string,
pickup_hour string,
pickup_week string,
weekday string,
passenger_count int,
trip_time_in_secs double,
trip_distance double,
pickup_longitude double,
pickup_latitude double,
dropoff_longitude double,
dropoff_latitude double,
direct_distance double,
payment_type string,
fare_amount double,
surcharge double,
mta_tax double,
tip_amount double,
tolls_amount double,
total_amount double,
tipped string,
tip_class string
)
row format delimited fields terminated by ','
lines terminated by '\\n'
stored as textfile;
"""
cursor.execute(queryString)
```

Şimdi birleştirme içeriğini önceki iç tabloya ekleyin.

```python
queryString = """
insert overwrite table nyctaxi_downsampled_dataset_testNEW
select
t.medallion,
t.hack_license,
t.vendor_id,
t.rate_code,
t.store_and_fwd_flag,
t.pickup_datetime,
t.dropoff_datetime,
hour(t.pickup_datetime) as pickup_hour,
weekofyear(t.pickup_datetime) as pickup_week,
from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
t.passenger_count,
t.trip_time_in_secs,
t.trip_distance,
t.pickup_longitude,
t.pickup_latitude,
t.dropoff_longitude,
t.dropoff_latitude,
t.direct_distance,
f.payment_type,
f.fare_amount,
f.surcharge,
f.mta_tax,
f.tip_amount,
f.tolls_amount,
f.total_amount,
if(tip_amount>0,1,0) as tipped,
if(tip_amount=0,0,
if(tip_amount>0 and tip_amount<=5,1,
if(tip_amount>5 and tip_amount<=10,2,
if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
from
(
select
medallion,
hack_license,
vendor_id,
rate_code,
store_and_fwd_flag,
pickup_datetime,
dropoff_datetime,
passenger_count,
trip_time_in_secs,
trip_distance,
pickup_longitude,
pickup_latitude,
dropoff_longitude,
dropoff_latitude,
3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
/sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
+cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
rand() as sample_key

from trip
where pickup_latitude between 30 and 90
    and pickup_longitude between -90 and -30
    and dropoff_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
)t
join
(
select
medallion,
hack_license,
vendor_id,
pickup_datetime,
payment_type,
fare_amount,
surcharge,
mta_tax,
tip_amount,
tolls_amount,
total_amount
from fare
)f
on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
where t.sample_key<=0.01
"""
cursor.execute(queryString)
```

Bir süre sonra, verilerin Hadoop kümelerinde yüklendiğini görebilirsiniz:

```python
queryString = """
    select * from nyctaxi_downsampled_dataset limit 10;
    """
cursor.execute(queryString)
pd.read_sql(queryString, connection)
```

![Tablodan üst veri satırları](./media/vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

### <a name="azure-sql-data-warehouse-and-databases"></a>Azure SQL Veri Ambarı ve veritabanları
Azure SQL Veri Ambarı, kurumsal sınıf BIR SQL Server deneyimine sahip bir hizmet olarak esnek bir veri ambarıdır.

[Bu makaledeki](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)yönergeleri izleyerek Azure SQL veri ambarınızı sağlayabilirsiniz. SQL veri ambarınızı sağlamadıktan sonra, [bu gözden geçirme yi](../team-data-science-process/sqldw-walkthrough.md) kullanarak SQL veri ambarındaki verileri kullanarak veri yükleme, arama ve modelleme yapabilirsiniz.

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB
Azure Cosmos DB, buluttaki bir NoSQL veritabanıdır. JSON gibi belgelerle çalışmak ve belgeleri depolamak ve sorgulamak için kullanabilirsiniz.

Azure Cosmos DB'ye DSVM'den erişmek için aşağıdaki ön koşul adımlarını kullanın:

1. Azure Cosmos DB Python SDK zaten DSVM yüklü. Güncelleştirmek için ```pip install pydocumentdb --upgrade``` komut isteminden çalıştırın.
2. [Azure portalından](https://portal.azure.com)bir Azure Cosmos DB hesabı ve veritabanı oluşturun.
3. Azure Cosmos DB Veri Geçiş Aracı'nı [Microsoft Download Center'dan](https://www.microsoft.com/download/details.aspx?id=53595) indirin ve seçtiğiniz bir dizine ayıklayın.
4. Ortak bir [blob'da](https://dotnet.microsoft.com/) depolanan JSON verilerini (yanardağ verileri) geçiş aracına aşağıdaki komut parametreleriyle birlikte Azure Cosmos DB'ye aktarın. (Azure Cosmos DB Veri Geçiş Aracı'nı yüklediğiniz dizinden dtui.exe'yi kullanın.) Kaynak ve hedef konumu şu parametrelerle girin:
   
    `/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/;AccountKey=[[KEY];Database=volcano /t.Collection:volcano1`

Verileri aldıktan sonra Jupyter'a gidip *DocumentDBSample*adlı not defterini açabilirsiniz. Azure Cosmos DB'ye erişmek ve bazı temel sorgulamalar yapmak için Python kodu içerir. Hizmetin [dokümantasyon sayfasını](https://docs.microsoft.com/azure/cosmos-db/)ziyaret ederek Azure Cosmos DB hakkında daha fazla bilgi edinebilirsiniz.

## <a name="use-power-bi-reports-and-dashboards"></a>Power BI raporlarını ve panolarını kullanma 
Veriler hakkında görsel bilgiler edinmek için Power BI Desktop'daki önceki Azure Cosmos DB örneğinden Volcano JSON dosyasını görselleştirebilirsiniz. Ayrıntılı adımlar Power [BI makalesinde](../../cosmos-db/powerbi-visualize.md)mevcuttur. Üst düzey adımlar şunlardır:

1. Power BI Desktop'ı açın ve **Veri Al**'ı seçin. URL'yi şu `https://cahandson.blob.core.windows.net/samples/volcano.json`şekilde belirtin: .
2. Bir liste olarak alınan JSON kayıtlarını görmelisiniz. Power BI'nin bu listeyle çalışabilmesi için listeyi tabloya dönüştürün.
4. Genişlet (ok) simgesini seçerek sütunları genişletin.
5. Konumun bir **Kayıt** alanı olduğuna dikkat edin. Kaydı genişletin ve yalnızca koordinatları seçin. **Koordinat** bir liste sütunudur.
6. Liste koordinat sütununu virgülle ayrılmış Bir **LatLong** sütununa dönüştürmek için yeni bir sütun ekleyin. Formülü ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```kullanarak koordinat listesi alanındaki iki öğeyi birleştirir.
7. **Yükseklik** sütununa ondalık sütunu dönüştürün ve **Kapat** ve **Uygula** düğmelerini seçin.

Önceki adımlar yerine, aşağıdaki kodu yapıştırabilirsiniz. Veri dönüşümlerini sorgu dilinde yazmak için Power BI'deki Advanced Editor'da kullanılan adımları komut dosyası olarak açıklar.

```pqfl
let
    Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
    #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
    #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
    #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
    #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
in
    #"Changed Type"
```

Artık Power BI veri modelinizde veri var. Power BI Masaüstü örneğiniz aşağıdaki gibi görünmelidir:

![Power BI Desktop](./media/vm-do-ten-things/PowerBIVolcanoData.png)

Veri modelini kullanarak raporlar ve görselleştirmeler oluşturmaya başlayabilirsiniz. Bir rapor oluşturmak için [bu Power BI makalesindeki](../../cosmos-db/powerbi-visualize.md#build-the-reports) adımları izleyebilirsiniz.

## <a name="scale-the-dsvm-dynamically"></a>DSVM'yi dinamik olarak ölçeklendirin 
Projenizin gereksinimlerini karşılamak için DSVM'yi büyütebilir ve küçültebilirsiniz. VM'yi akşam veya hafta sonları kullanmanız gerekmiyorsa, [Azure portalından](https://portal.azure.com)VM'yi kapatabilirsiniz.

> [!NOTE]
> VM'deki işletim sistemi için sadece kapatma düğmesini kullanırsanız işlem ücretlerine maruz kaldığınız bir işlem ücreti ne olur.  
> 
> 

Bazı büyük ölçekli çözümlemesi işlemek ve daha fazla CPU, bellek veya disk kapasitesi gerekebilir. Bu durumda, bilgi işlem ve bütçe gereksinimlerinizi karşılayan CPU çekirdekleri, derin öğrenme için GPU tabanlı örnekler, bellek kapasitesi ve disk türleri (katı hal sürücüleri dahil) açısından VM boyutu seçenekleri bulabilirsiniz. Sanal Makinelerin tam listesi ve saatlik bilgi işlem fiyatlandırması Azure [Sanal Makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/) sayfasında bulunabilir.

Benzer şekilde, VM işleme kapasitesine olan gereksiniminiz de azalabilir. (Örneğin: büyük bir iş yükünü hadoop veya Spark kümesine taşıdınız.) Daha sonra azure [portalından](https://portal.azure.com) kümeyi küçültebilir ve VM örneğinizin ayarlarına gidebilirsiniz. 

## <a name="add-more-tools"></a>Daha fazla araç ekleyin
DSVM'de önceden oluşturulmuş araçlar birçok yaygın veri analizi gereksinimlerini karşılayabilir. Ortamlarınızı tek tek yüklemeniz ve yapılandırmanız gerekmedığından, bu size zaman kazandırır. Ayrıca, yalnızca kullandığınız kaynaklar için ödeme yaptığınız için paradan tasarruf etmenizi sağlar.

Analitik ortamınızı geliştirmek için bu makalede profili olan diğer Azure veri ve analiz hizmetlerini kullanabilirsiniz. Bazı durumlarda, bazı özel iş ortağı araçları da dahil olmak üzere ek araçlara ihtiyacınız olabilir. İhtiyacınız olan yeni araçları yüklemek için sanal makinede tam yönetim erişimine sahipsiniz. Python ve R'ye önceden yüklenmemiş ek paketler de yükleyebilirsiniz. Python için, ya ```conda``` da ```pip```kullanabilirsiniz . R için R ```install.packages()``` konsolunda kullanabilir veya IDE'yi kullanabilirsiniz ve **Paketleri** > **Yükleyi'yi**seçebilirsiniz.

## <a name="deep-learning"></a>Derin öğrenme

Çerçeve tabanlı örneklere ek olarak, DSVM'de doğrulanmış bir dizi kapsamlı izyol elde edebilirsiniz. Bu izbolar, görüntü ve metin/dil anlama gibi alanlarda derin öğrenme uygulamaları geliştirmenizi hızlandırarak başlatmanıza yardımcı olur.   


- [Farklı çerçevelerde sinir ağlarının çalıştırılması](https://github.com/ilkarman/DeepLearningFrameworks): Bu iz, kodun bir çerçeveden diğerine nasıl geçirileni gösterir. Ayrıca, modeller ve çalışma zamanı performansının çerçeveler arasında nasıl karşılaştırılabildiğini de gösterir. 

- [Görüntülerdeki ürünleri algılamak için uçtan uca bir çözüm oluşturmak için nasıl yapılır kılavuzu](https://github.com/Azure/cortana-intelligence-product-detection-from-images): Görüntü algılama, görüntülerdeki nesneleri bulup sınıflandırabilen bir tekniktir. Bu teknoloji birçok gerçek iş alanlarında büyük ödüller getirmek için potansiyele sahiptir. Örneğin, perakendeciler bu tekniği, müşterinin raftan hangi ürünü aldığını belirlemek için kullanabilir. Bu bilgiler sırayla, mağazaların ürün envanterini yönetmesine yardımcı olur. 

- [Ses için derin öğrenme](https://blogs.technet.microsoft.com/machinelearning/2018/01/30/hearing-ai-getting-started-with-deep-learning-for-audio-on-azure/): Bu öğretici [nasıl kentsel sesler dataset](https://serv.cusp.nyu.edu/projects/urbansounddataset/urbansound8k.html)ses olay algılama için derin öğrenme modeli tren gösterir. Ayrıca ses verileriyle nasıl çalışılabilenlere genel bir bakış sağlar.

- [Metin belgelerinin sınıflandırılması](https://github.com/anargyri/lstm_han): Bu izbis, hiyerarşik dikkat ağı ve Uzun Kısa Süreli Bellek (LSTM) ağı olmak üzere iki sinir ağı mimarisinin nasıl inşa edilip eğitilenbir şekilde yapılacağını gösterir. Bu sinir ağları metin belgelerini sınıflandırmak için derin öğrenme için Keras API'yi kullanır. Keras en popüler derin öğrenme çerçeveleri üç bir ön uç: Microsoft Cognitive Toolkit, TensorFlow ve Theano.

## <a name="summary"></a>Özet
Bu makalede, Microsoft Veri Bilimi Sanal Makine üzerinde yapabileceğiniz bazı şeyler açıklanmıştır. DSVM'yi etkili bir analiz ortamı haline getirmek için yapabileceğiniz daha birçok şey vardır.

