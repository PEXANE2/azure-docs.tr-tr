---
title: Bir web hizmetini yeniden eğitin
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'da (klasik) yeni eğitilmiş bir makine öğrenme modelini kullanmak için bir web hizmetini nasıl güncelleştirtiğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 02/14/2019
ms.openlocfilehash: 218c1c98a2ed775ae86c1657156991879708cc7a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217928"
---
# <a name="retrain-and-deploy-a-machine-learning-model"></a>Bir makine öğrenme modelini yeniden eğitin ve dağıtın

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Yeniden eğitim, makine öğrenimi modellerinin doğru kalmasını ve mevcut en alakalı verilere dayanmasını sağlamanın bir yoludur. Bu makalede, Studio (klasik) yeni bir web hizmeti olarak bir makine öğrenme modeli yeniden eğitmek ve dağıtmak nasıl gösterir. Klasik bir web hizmetini yeniden eğitmek istiyorsanız, [bu nasıl yapılSA makalesini görüntüleyin.](retrain-classic-web-service.md)

Bu makalede, zaten bir tahminweb hizmeti dağıtılan varsayar. Önceden tahmine dayalı bir web hizmetiniz yoksa, [studio (klasik) web hizmetini burada nasıl dağıtabileceğinizi öğrenin.](deploy-a-machine-learning-web-service.md)

Yeni web hizmetini öğrenen bir makineyi yeniden eğitmek ve dağıtmak için aşağıdaki adımları izlersiniz:

1. Yeniden **eğitim web hizmetini** dağıtma
1. Yeniden eğitim web **hizmetinizi** kullanarak yeni bir model eğitin
1. Yeni modeli kullanmak için mevcut **tahmine dayalı denemenizi** güncelleştirin

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="deploy-the-retraining-web-service"></a>Yeniden eğitim web hizmetini dağıtma

Yeniden eğitim web hizmeti, modelinizi yeni veriler gibi yeni bir parametre kümesiyle yeniden eğitmenize ve daha sonraya kaydetmenize olanak tanır. Bir Web **Hizmeti Çıktısını** Bir **Tren Modeline**bağladığınızda, eğitim denemesi kullanmanız için yeni bir model çıkar.

Yeniden eğitim web hizmetini dağıtmak için aşağıdaki adımları kullanın:

1. Veri girişinize bir **Web Hizmeti Giriş** modülü bağlayın. Genellikle, giriş verilerinizin orijinal eğitim verilerinizle aynı şekilde işlendiğinden emin olmak istersiniz.
1. **Tren Modelinizin**çıktısına bir **Web Hizmeti Çıkışı** modülü bağlayın.
1. Bir Model **Değerlendir** modülüne sahipseniz, değerlendirme sonuçlarını çıktılamak için bir **Web Hizmeti Çıktı** modülü bağlayabilirsiniz
1. Deneyini çalıştırın.

    Denemenizi çalıştırdıktan sonra, ortaya çıkan iş akışı aşağıdaki görüntüye benzer olmalıdır:

    ![Ortaya çıkan iş akışı](media/retrain-machine-learning/machine-learning-retrain-models-programmatically-IMAGE04.png)

    Şimdi, eğitim denemesini, eğitilmiş bir model ve model değerlendirme sonuçları ndan çıkan yeniden eğitim web hizmeti olarak dağıtın.

1. Deneme tuvalinin **altında, Web Hizmeti Ayarla'yı** tıklatın
1. **Web Hizmetini Dağıt'ı [Yeni]** seçin. Azure Machine Learning Web Services **portalı, Web Hizmeti Dağıt** sayfasına açılır.
1. Web hizmetiniz için bir ad yazın ve bir ödeme planı seçin.
1. **Dağıt**'ı seçin.

## <a name="retrain-the-model"></a>Modeli yeniden eğitin

Bu örnekte, yeniden eğitim uygulamasını oluşturmak için C# kullanıyoruz. Bu görevi gerçekleştirmek için Python veya R örnek kodunu da kullanabilirsiniz.

Yeniden eğitim API'lerini çağırmak için aşağıdaki adımları kullanın:

1. Visual Studio'da C# konsol uygulaması oluşturun: **Yeni** > **Proje** > **Görsel C#** > **Windows Classic Masaüstü** > **Konsol Uygulaması (.NET Framework)**.
1. Machine Learning Web Services portalında oturum açın.
1. Birlikte çalıştığınız web hizmetini tıklatın.
1. **Tüket'i**tıklatın.
1. **Tüket** sayfasının alt kısmında, **Örnek Kod** bölümünde **Toplu İşlem'i**tıklatın.
1. Toplu yürütme için örnek C# kodunu kopyalayın ve Program.cs dosyasına yapıştırın. Ad alanının bozulmadığından emin olun.

NuGet paketini Microsoft.AspNet.WebApi.Client,açıklamalarda belirtildiği gibi ekleyin. Başvuruyu Microsoft.WindowsAzure.Storage.dll adresine eklemek [için Azure Depolama hizmetleri için istemci kitaplığını](https://www.nuget.org/packages/WindowsAzure.Storage)yüklemeniz gerekebilir.

Aşağıdaki ekran görüntüsü, Azure Machine Learning Web Services portalında **Tüket** sayfasını gösterir.

![Sayfayı tüket](media/retrain-machine-learning/machine-learning-retrain-models-consume-page.png)

### <a name="update-the-apikey-declaration"></a>Apikey bildirimini güncelleştir

**Apikey** bildirimini bulun:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

**Tüket** sayfasının **Temel tüketim bilgileri** bölümünde, birincil anahtarı bulun ve **apikey** bildirimine kopyalayın.

### <a name="update-the-azure-storage-information"></a>Azure Depolama bilgilerini güncelleştirme

BES örnek kodu, yerel bir sürücüden (örneğin, "C:\temp\CensusInput.csv") bir dosyayı Azure Depolama'ya yükler, işler ve sonuçları Azure Depolama'ya geri yazar.

1. Azure portalda oturum açma
1. Soldaki gezinti sütununda, **Daha fazla hizmet,** **Depolama hesaplarını**arama ve seçin' i tıklatın.
1. Depolama hesapları listesinden, yeniden eğitilmiş modeli depolamak için birini seçin.
1. Soldaki gezinti sütununda **Erişim tuşlarına**tıklayın.
1. Birincil Erişim **Anahtarını**kopyalayın ve kaydedin.
1. Sol daki gezinti sütununda **Blobs'u**tıklatın.
1. Varolan bir kapsayıcıseçin veya yeni bir kapsayıcı oluşturun ve adı kaydedin.

*StorageAccountName,* *StorageAccountKey*ve *StorageContainerName* bildirimlerini bulun ve portaldan kaydettiğiniz değerleri güncelleştirin.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Ayrıca, giriş dosyasının kodda belirttiğiniz konumda kullanılabilir olduğundan da emin olmalısınız.

### <a name="specify-the-output-location"></a>Çıktı konumunu belirtin

İstek Yükü'nde çıktı konumunu belirttiğiniz zaman, *RelativeLocation'da* belirtilen dosyanın uzantısı `ilearner`. olarak belirtilmelidir.

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

İşte çıktıyı yeniden eğitime bir örnek:

![Çıktıyı yeniden eğitim](media/retrain-machine-learning/machine-learning-retrain-models-programmatically-IMAGE06.png)

### <a name="evaluate-the-retraining-results"></a>Yeniden eğitim sonuçlarını değerlendirme

Uygulamayı çalıştırdığınızda, çıktı, değerlendirme sonuçlarına erişmek için gerekli olan URL'yi ve paylaşılan erişim imza larını içerir.

Yeniden eğitilen modelin performans sonuçlarını *BaseLocation*, *RelativeLocation*ve *SasBlobToken'i* *çıktı2* çıktı sonuçlarından birleştirerek ve tam URL'yi tarayıcı adresi çubuğuna yapıştırarak görebilirsiniz.

Yeni eğitilen modelin mevcut modelden daha iyi performans gösterip performans gösterip gösterip çalışmamasını belirlemek için sonuçları inceleyin.

*BaseLocation,* *RelativeLocation*ve *SasBlobToken'i* çıktı sonuçlarından kaydedin.

## <a name="update-the-predictive-experiment"></a>Tahmine dayalı denemeyi güncelleştirme

### <a name="sign-in-to-azure-resource-manager"></a>Azure Kaynak Yöneticisi'nde oturum açma

İlk olarak, [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) cmdlet'ini kullanarak PowerShell ortamından Azure hesabınızda oturum açın.

### <a name="get-the-web-service-definition-object"></a>Web Hizmeti Tanımı nesnesini alma

Ardından, [Get-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/get-azmlwebservice) cmdlet'i arayarak Web Hizmeti Tanımı nesnesini alın.

    $wsd = Get-AzMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Varolan bir web hizmetinin kaynak grubu adını belirlemek için, aboneliğinizde web hizmetlerini görüntülemek için herhangi bir parametre olmadan Get-AzMlWebService cmdlet çalıştırın. Web hizmetini bulun ve web hizmeti kimliğine bakın. Kaynak grubunun adı, *kaynak Grupları* öğesinden hemen sonra, ID'deki dördüncü öğedir. Aşağıdaki örnekte, kaynak grubu adı Varsayılan-MachineLearning-SouthCentralUS'tur.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternatif olarak, varolan bir web hizmetinin kaynak grubu adını belirlemek için Azure Machine Learning Web Services portalında oturum açın. Web hizmetini seçin. Kaynak grubu adı, *kaynak Grupları* öğesinden hemen sonra, web hizmetinin URL'sinin beşinci öğesidir. Aşağıdaki örnekte, kaynak grubu adı Varsayılan-MachineLearning-SouthCentralUS'tur.

    https://services.azureml.net/subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237

### <a name="export-the-web-service-definition-object-as-json"></a>Web Hizmeti Tanımı nesnesini JSON olarak dışa aktarma

Yeni eğitilen modeli kullanmak için eğitilmiş modelin tanımını değiştirmek için, önce Bir JSON biçimi dosyasına dışa aktarmak için [Dışa Aktarma-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/export-azmlwebservice) cmdlet kullanmanız gerekir.

    Export-AzMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

### <a name="update-the-reference-to-the-ilearner-blob"></a>Ilearner blob başvuru güncelleme

Varlıklarda, [eğitilmiş modeli] bulun, ilearner blob URI ile *locationInfo* düğümünde *uri* değerini güncelleyin. URI, BES yeniden eğitim çağrısının çıktısından BaseLocation ve *RelativeLocation'ı* birleştirerek oluşturulur. *RelativeLocation*

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

### <a name="import-the-json-into-a-web-service-definition-object"></a>JSON'u Web Hizmeti Tanımı nesnesine alma

Değiştirilen JSON dosyasını, predicative denemesini güncelleştirmek için kullanabileceğiniz bir Web Hizmeti Tanımı nesnesine dönüştürmek için [Alma-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/import-azmlwebservice) cmdlet'ini kullanın.

    $wsd = Import-AzMlWebService -InputFile "C:\temp\mlservice_export.json"

### <a name="update-the-web-service"></a>Web hizmetini güncelleştirme

Son olarak, tahmine dayalı denemeyi güncelleştirmek için [Update-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/update-azmlwebservice) cmdlet'i kullanın.

    Update-AzMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

## <a name="next-steps"></a>Sonraki adımlar

Web hizmetlerini yönetme veya birden çok deneme yi izleme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Web Hizmetleri portalını keşfedin](manage-new-webservice.md)
* [Deneme yinelemelerini yönetme](manage-experiment-iterations.md)
