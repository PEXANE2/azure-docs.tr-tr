---
title: Python betiklerini Data Factory Azure Batch Python aracılığıyla çalıştırma
description: Öğretici-Azure Batch kullanarak Azure Data Factory bir işlem hattının parçası olarak Python betikleri çalıştırmayı öğrenin.
author: mammask
ms.devlang: python
ms.topic: tutorial
ms.date: 12/11/2019
ms.author: komammas
ms.custom: mvc
ms.openlocfilehash: 6761896a6555c11d7957f923a5951641c1541012
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82117072"
---
# <a name="tutorial-run-python-scripts-through-azure-data-factory-using-azure-batch"></a>Öğretici: Azure Batch kullanarak Azure Data Factory Python betikleri çalıştırma

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Batch ve Depolama hesapları ile kimlik doğrulaması
> * Python 'da betik geliştirme ve çalıştırma
> * Bir uygulamayı çalıştırmak için işlem düğümleri havuzu oluşturma
> * Python iş yüklerinizi zamanlama
> * Analiz işlem hattınızı izleme
> * Günlük verilerinize erişin

Aşağıdaki örnek, bir BLOB depolama kapsayıcısından CSV girişi alan bir Python betiği çalıştırır, bir veri işleme işlemi gerçekleştirir ve çıktıyı ayrı bir BLOB depolama kapsayıcısına yazar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Yerel test için yüklü bir [Python](https://www.python.org/downloads/) dağıtımı.
* [Azure](https://pypi.org/project/azure/) `pip` paketi.
* Bir Azure Batch hesabı ve bağlı bir Azure Depolama hesabı. Batch hesaplarını oluşturma ve depolama hesaplarına bağlama hakkında daha fazla bilgi için bkz. [Batch hesabı oluşturma](quick-create-portal.md#create-a-batch-account) .
* Azure Data Factory hesabı. Azure portal aracılığıyla veri fabrikası oluşturma hakkında daha fazla bilgi için bkz. [Veri Fabrikası oluşturma](../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory) .
* [Batch Explorer](https://azure.github.io/BatchExplorer/).
* [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

[!INCLUDE [batch-common-credentials](../../includes/batch-common-credentials.md)]

## <a name="create-a-batch-pool-using-batch-explorer"></a>Batch Explorer kullanarak bir Batch havuzu oluşturma

Bu bölümde, Azure Data Factory Işlem hattının kullanacağı toplu Iş havuzunu oluşturmak için Batch Explorer kullanacaksınız. 

1. Batch Explorer için Azure kimlik bilgilerinizi kullanarak oturum açın.
1. Batch hesabınızı seçin
1. Sol taraftaki çubukta **havuzlar** ' ı seçerek bir havuz oluşturun, sonra arama formunun üzerindeki **Ekle** düğmesine basın. 
    1. Bir KIMLIK ve görünen ad seçin. Bu örnek `custom-activity-pool` için kullanacağız.
    1. Ölçek türünü **sabit boyut**olarak ayarlayın ve adanmış düğüm sayısını 2 olarak ayarlayın.
    1. **Veri bilimi**altında, işletim sistemi olarak **dsvm Windows** ' u seçin.
    1. Sanal `Standard_f2s_v2` makine boyutu olarak seçin.
    1. Başlangıç görevini etkinleştirin ve komutunu `cmd /c "pip install pandas"`ekleyin. Kullanıcı kimliği varsayılan **Havuz kullanıcısı**olarak kalabilir.
    1. **Tamam ' ı**seçin.

## <a name="create-blob-containers"></a>Blob kapsayıcıları oluşturma

Burada, OCR toplu işi için giriş ve çıkış dosyalarınızı depolayacak blob kapsayıcıları oluşturacaksınız.

1. Depolama Gezgini için Azure kimlik bilgilerinizi kullanarak oturum açın.
1. Batch hesabınıza bağlı depolama hesabını kullanarak, [BLOB kapsayıcısı oluşturma](../vs-azure-tools-storage-explorer-blobs.md#create-a-blob-container)konumundaki adımları izleyerek iki blob kapsayıcı (bir diğeri çıkış dosyaları için bir tane) oluşturun.
    * Bu örnekte, giriş kapsayımuzu `input`ve çıkış kapsayımuzu `output`arayacağız.
1. `iris.csv` [BLOB kapsayıcısında blob 'ları yönetme](../vs-azure-tools-storage-explorer-blobs.md#managing-blobs-in-a-blob-container) adımlarını `input` izleyerek Depolama Gezgini kullanarak giriş kapsayıcınızı karşıya yükleyin `main.py`


## <a name="develop-a-script-in-python"></a>Python 'da betik geliştirme

Aşağıdaki Python betiği, veri `iris.csv` kümesini kapsayıcılarınızdan `input` yükler, bir veri işleme işlemi gerçekleştirir ve sonuçları `output` kapsayıcıya geri kaydeder.

``` python
# Load libraries
from azure.storage.blob import BlockBlobService
import pandas as pd

# Define parameters
storageAccountName = "<storage-account-name>"
storageKey         = "<storage-account-key>"
containerName      = "output"

# Establish connection with the blob storage account
blobService = BlockBlobService(account_name=storageAccountName,
                               account_key=storageKey
                               )

# Load iris dataset from the task node
df = pd.read_csv("iris.csv")

# Subset records
df = df[df['Species'] == "setosa"]

# Save the subset of the iris dataframe locally in task node
df.to_csv("iris_setosa.csv", index = False)

# Upload iris dataset
blobService.create_blob_from_text(containerName, "iris_setosa.csv", "iris_setosa.csv")
```

Betiği kaydedin `main.py` ve **Azure depolama** kapsayıcısına yükleyin. Blob kapsayıcınıza yüklemeden önce işlevselliğini yerel olarak test edin ve doğrulayın:

``` bash
python main.py
```

## <a name="set-up-an-azure-data-factory-pipeline"></a>Azure Data Factory işlem hattı ayarlama

Bu bölümde, Python komut dosyanızı kullanarak bir işlem hattı oluşturup doğrulayacaksınız.

1. [Bu makalenin](../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory)"Veri Fabrikası oluşturma" bölümünde bir veri fabrikası oluşturmak için adımları izleyin.
1. **Fabrika kaynakları** kutusunda + (artı) düğmesini seçin ve ardından işlem **hattı** ' nı seçin.
1. **Genel** sekmesinde, işlem hattının adını "Python Çalıştır" olarak ayarlayın

    ![](./media/run-python-batch-azure-data-factory/create-pipeline.png)

1. **Etkinlikler** kutusunda **Batch hizmeti**' ni genişletin. **Etkinlikler** araç kutusundan özel etkinliği işlem hattı tasarımcı yüzeyine sürükleyin.
1. **Genel** sekmesinde, ad Için **testpipeline** belirtin

    ![](./media/run-python-batch-azure-data-factory/create-custom-task.png)
1. **Azure Batch** sekmesinde, önceki adımlarda oluşturulan **Batch hesabını** ekleyin ve başarılı olduğundan emin olmak için **bağlantıyı test** edin

    ![](./media/run-python-batch-azure-data-factory/integrate-pipeline-with-azure-batch.png)

1. **Ayarlar** sekmesinde, komutunu `python main.py`girin.
1. **Kaynak bağlı hizmeti**için, önceki adımlarda oluşturulan depolama hesabını ekleyin. Başarılı olduğundan emin olmak için bağlantıyı test edin.
1. **Klasör yolu**' nda, Python betiğini ve ilişkili girişleri Içeren **Azure Blob depolama** kapsayıcısının adını seçin. Bu işlem, Python betiği yürütülmeden önce seçili dosyaları kapsayıcıdan havuzdan düğüm örneklerine indirir.

    ![](./media/run-python-batch-azure-data-factory/create-custom-task-py-script-command.png)
1. İşlem hattı ayarlarını doğrulamak için işlem hattı araç çubuğunda **Doğrula**'ya tıklayın. İşlem hattının başarıyla doğrulandığını onaylayın. Doğrulama çıktısını kapatmak için &gt;&gt; (sağ ok) düğmesini seçin.
1. İşlem hattını test etmek ve düzgün çalıştığından emin olmak için **Hata Ayıkla** ' ya tıklayın.
1. İşlem hattını yayımlamak için **Yayımla** ' ya tıklayın.
1. Python betiğini toplu işlemin bir parçası olarak çalıştırmak için **Tetikle** ' e tıklayın.

    ![](./media/run-python-batch-azure-data-factory/create-custom-task-py-success-run.png)

### <a name="monitor-the-log-files"></a>Günlük dosyalarını izleme

Uyarıları veya hataları komut dosyanızın yürütülmesi tarafından üretildiğinde, günlüğe kaydedilen çıktı hakkında daha fazla bilgi alabilirsiniz `stdout.txt` `stderr.txt` .

1. Batch Explorer sol taraftaki **işleri** seçin.
1. Veri fabrikanızın oluşturduğu işi seçin. Havuzunuzu `custom-activity-pool`adlandırdığınız varsayılarak, öğesini `adfv2-custom-activity-pool`seçin.
1. Hata çıkış kodu olan göreve tıklayın.
1. Sorununuzu `stdout.txt` inceleyin `stderr.txt` ve tanılamanıza ve tanılayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Batch kullanarak Azure Data Factory bir işlem hattının parçası olarak Python betiklerini nasıl çalıştıracağınızı öğrettin bir örnek incelediniz.

Azure Data Factory hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Azure Data Factory](../data-factory/introduction.md)
> İşlem[hatları ve Etkinlikler](../data-factory/concepts-pipelines-activities.md)
> [özel etkinlikleri](../data-factory/transform-data-using-dotnet-custom-activity.md) Azure Data Factory
