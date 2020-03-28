---
title: Veri Fabrikası'nda Python komut dosyalarını çalıştırma - Azure Toplu Python
description: Öğretici - Azure Toplu İşlemini kullanarak Azure Veri Fabrikası'nda bir boru hattının parçası olarak Python komut dosyalarını nasıl çalıştıracağımız öğrenin.
services: batch
author: mammask
manager: jeconnoc
ms.service: batch
ms.devlang: python
ms.topic: tutorial
ms.date: 12/11/2019
ms.author: komammas
ms.custom: mvc
ms.openlocfilehash: 2995c5da4491f14471d9ed03022a144a02beab5a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78201839"
---
# <a name="tutorial-run-python-scripts-through-azure-data-factory-using-azure-batch"></a>Öğretici: Azure Toplu İşlemini kullanarak Python komut dosyalarını Azure Veri Fabrikası'nda çalıştırın

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Batch ve Depolama hesapları ile kimlik doğrulaması
> * Python'da bir komut dosyası geliştirme ve çalıştırma
> * Bir uygulamayı çalıştırmak için işlem düğümleri havuzu oluşturma
> * Python iş yüklerinizi zamanlama
> * Analiz ardınızı izleyin
> * Günlük dosyalarınıza erişin

Aşağıdaki örnekte, blob depolama kapsayıcısından CSV girişi alan, veri işleme işlemi gerçekleştiren ve çıktıyı ayrı bir blob depolama kapsayıcısına yazan bir Python komut dosyası çalışır.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Yerel [sınama](https://www.python.org/downloads/) için yüklü bir Python dağıtımı.
* [Azure](https://pypi.org/project/azure/) `pip` paketi.
* Bir Azure Batch hesabı ve bağlı bir Azure Depolama hesabı. Toplu Iş hesaplarının nasıl oluşturulup depolama hesaplarına bağlanılabilenhakkında daha fazla bilgi için [Toplu İş Hesabı Oluştur'a](quick-create-portal.md#create-a-batch-account) bakın.
* Azure Veri Fabrikası hesabı. Azure portalı üzerinden bir veri fabrikası nın nasıl oluşturulabildiğini hakkında daha fazla bilgi için [bir veri fabrikası oluşturun'](../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory) a bakın.
* [Toplu Gezgin](https://azure.github.io/BatchExplorer/).
* [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

[!INCLUDE [batch-common-credentials](../../includes/batch-common-credentials.md)]

## <a name="create-a-batch-pool-using-batch-explorer"></a>Toplu İş Gezgini'ni kullanarak Toplu Iş havuzu oluşturma

Bu bölümde, Azure Veri fabrika ardışık ardışık ardınızda kullanacağı Toplu İş havuzunu oluşturmak için Toplu Gezgin'i kullanırsınız. 

1. Azure kimlik bilgilerinizi kullanarak Toplu İş Gezgini'nde oturum açın.
1. Toplu İş hesabınızı seçin
1. Sol taraftaki çubuktaki **Havuzlar'ı,** ardından arama formunun üzerindeki **Ekle** düğmesini seçerek bir havuz oluşturun. 
    1. Bir kimlik ve görüntü adı seçin. Bu örneği `custom-activity-pool` kullanacağız.
    1. Ölçek türünü **Sabit boyuta**ayarlayın ve özel düğüm sayısını 2 olarak ayarlayın.
    1. **Veri bilimi**altında, işletim sistemi olarak **Dsvm Windows'u** seçin.
    1. Sanal `Standard_f2s_v2` makine boyutu olarak seçin.
    1. Başlangıç görevini etkinleştirin ve `cmd /c "pip install pandas"`komutu ekleyin. Kullanıcı kimliği varsayılan Havuz **kullanıcısı**olarak kalabilir.
    1. **Tamam'ı**seçin.

## <a name="create-blob-containers"></a>Blob kapları oluşturma

Burada, OCR Toplu iş için giriş ve çıktı dosyalarınızı depolayacak blob kapsayıcıları oluşturacaksınız.

1. Azure kimlik bilgilerinizi kullanarak Depolama Gezgini'nde oturum açın.
1. Toplu Iş hesabınıza bağlı depolama hesabını kullanarak, [blob kapsayıcısı oluştur'daki](../vs-azure-tools-storage-explorer-blobs.md#create-a-blob-container)adımları izleyerek iki blob kapsayıcısı (biri giriş dosyaları için, diğeri çıkış dosyaları için) oluşturun.
    * Bu örnekte, giriş kabımızı `input`ve çıkış kabımızı `output`çağıracağız.
1. `main.py` [Blob kabındaki lekeleri yönetme](../vs-azure-tools-storage-explorer-blobs.md#managing-blobs-in-a-blob-container) adımlarını izleyerek Depolama Gezgini'ni kullanarak giriş kabınıza yükleme ve `iris.csv` giriş kabına `input` yükleme


## <a name="develop-a-script-in-python"></a>Python'da komut dosyası geliştirme

Aşağıdaki Python komut `iris.csv` dosyası, veri `input` kümesini kapsayıcınızdan yükler, veri işleme işlemi `output` gerçekleştirir ve sonuçları kapsayıcıya geri kaydeder.

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

Komut dosyasını `main.py` olarak kaydedin ve **Azure Depolama** kapsayıcısına yükleyin. Blob kapsayıcınıza yüklemeden önce işlevselliğini yerel olarak sınayıp doğruladığınızdan emin olun:

``` bash
python main.py
```

## <a name="set-up-an-azure-data-factory-pipeline"></a>Azure Veri Fabrikası ardışık bir iş tonu ayarlama

Bu bölümde, Python komut dosyanızı kullanarak bir ardışık hat lar oluşturup doğrularsınız.

1. [Bu makalenin](../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory)"Veri fabrikası oluşturma" bölümü altında bir veri fabrikası oluşturmak için adımları izleyin.
1. Fabrika **Kaynakları** kutusunda + (artı) düğmesini seçin ve ardından **Pipeline'ı** seçin
1. **Genel** sekmesinde, ardışık hattın adını "Python'u Çalıştır" olarak ayarlayın

    ![](./media/run-python-batch-azure-data-factory/create-pipeline.png)

1. **Etkinlikler** kutusunda, **Toplu Hizmet'i**genişletin. Özel etkinliği **Etkinlikler** araç kutusundan boru hattı tasarımcısı yüzeyine sürükleyin.
1. **Genel** sekmesinde, Ad için **testPipeline** belirtin

    ![](./media/run-python-batch-azure-data-factory/create-custom-task.png)
1. Azure **Toplu İş** sekmesinde, başarılı olduğundan emin olmak için önceki adımlarda oluşturulan **Toplu İş Hesabı'nı** ve **Test bağlantısını** ekleyin

    ![](./media/run-python-batch-azure-data-factory/integrate-pipeline-with-azure-batch.png)

1. **Ayarlar** sekmesine, komutu `python main.py`girin.
1. Kaynak **Bağlantılı Hizmet**için, önceki adımlarda oluşturulan depolama hesabıekleyin. Başarılı olduğundan emin olmak için bağlantıyı test edin.
1. Klasör **Yolu'nda,** Python komut dosyasını ve ilişkili girişleri içeren **Azure Blob Depolama** kapsayıcısının adını seçin. Bu, Python komut dosyasının yürütülmesinden önce kaptaki dosyayı havuz düğümü örneklerine indirir.

    ![](./media/run-python-batch-azure-data-factory/create-custom-task-py-script-command.png)
1. İşlem hattı ayarlarını doğrulamak için işlem hattı araç çubuğunda **Doğrula**'ya tıklayın. İşlem hattının başarıyla doğrulandığını onaylayın. Doğrulama çıktısını kapatmak için &gt;&gt; (sağ ok) düğmesini seçin.
1. Ardışık bölümü test etmek ve doğru çalıştığından emin olmak için **Hata Ayıklama'yı** tıklatın.
1. Ardışık hattı yayımlamak için **Yayımla'yı** tıklatın.
1. Toplu iş işleminin bir parçası olarak Python komut dosyasını çalıştırmak için **Tetikleyici'yi** tıklatın.

    ![](./media/run-python-batch-azure-data-factory/create-custom-task-py-success-run.png)

### <a name="monitor-the-log-files"></a>Günlük dosyalarını izleme

Uyarı veya hataların komut dosyanızın yürütülmesiyle üretiliyorsa, kullanıma alabilir `stdout.txt` veya `stderr.txt` günlüğe kaydedilmiş çıktı hakkında daha fazla bilgi alabilirsiniz.

1. Toplu İş Gezgini'nin sol tarafından **İşler'i** seçin.
1. Veri fabrikanız tarafından oluşturulan işi seçin. Havuzunuza `custom-activity-pool`isim verdiğinizi varsayarsak, seçin. `adfv2-custom-activity-pool`
1. Hata çıkış kodu olan göreve tıklayın.
1. Sorununuzu görüntülemek `stdout.txt` ve `stderr.txt` araştırmak ve tanılamak için.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Toplu İşi'ni kullanarak Azure Veri Fabrikası'nda bir boru hattının parçası olarak Python komut dosyalarını çalıştırmayı öğreten bir örnek araştırdınız.

Azure Veri Fabrikası hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Azure Veri Fabrikası](../data-factory/introduction.md)
> [Boru Hatları ve etkinlikleri](../data-factory/concepts-pipelines-activities.md)
> [Özel etkinlikler](../data-factory/transform-data-using-dotnet-custom-activity.md)
