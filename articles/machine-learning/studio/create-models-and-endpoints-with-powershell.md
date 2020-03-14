---
title: Bir model için birden fazla uç nokta oluşturma
titleSuffix: ML Studio (classic) - Azure
description: Birden çok makine öğrenimi modelleri ve web hizmeti uç noktaları aynı algoritmayı ancak farklı bir eğitim veri kümeleri oluşturmak için PowerShell kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 04/04/2017
ms.openlocfilehash: 4f8bb1f20dcc3a7900e3347616018a6e156962d0
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79218182"
---
# <a name="use-powershell-to-create-studio-classic-models-and-web-service-endpoints-from-one-experiment"></a>Bir deneyden Studio (klasik) modeller ve Web hizmeti uç noktaları oluşturmak için PowerShell 'i kullanma

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bir ortak makine öğrenimi sorunu şu şekildedir: aynı eğitim iş akışı, sahip ve aynı algoritmayı kullanan çok sayıda model oluşturmak istiyorsunuz. Ancak giriş olarak farklı bir eğitim veri kümesi olmasını istersiniz. Bu makalede, yalnızca tek bir deneme kullanılarak bu Azure Machine Learning Studio (klasik) ölçekte nasıl yapılacağı gösterilmektedir.

Örneğin, bir genel bisiklet kiralama franchise işletme sahibi varsayalım. Geçmiş verileri temel alan kiralama talep tahmin etmek için regresyon modeli oluşturmak istiyorsunuz. Dünya genelinde 1.000 kiralama konumları vardır ve her konum için bir veri kümesi derledik. Bunlar, tarih, saat, hava durumu ve trafiği gibi her bir konuma özgü önemli özellikleri içerir.

Bir kez tüm konumlar arasında tüm veri kümelerini birleştirilmiş bir sürümünü kullanarak modelinizi eğitmek. Ancak, konumların her biri benzersiz bir ortamı sahip. Bu nedenle ayrı ayrı her konum için veri kümesini kullanarak, regresyon modeli eğitmek için daha iyi bir yaklaşım olacaktır. Bu şekilde, her eğitilen model dikkate farklı depolama boyutları, birim, Coğrafya, nüfus, bisiklet dostu trafiği ortam ve daha fazla sürebilir.

Bu en iyi yaklaşım olabilir, ancak Azure Machine Learning Studio (klasik) içinde her biri benzersiz bir konum temsil eden 1.000 eğitim denemeleri oluşturmak istemezsiniz. Her deneme aynı bileşenleri eğitim veri kümesi dışında olacağından zor bir görev olmasının yanı sıra, ayrıca verimsiz hatırlıyorum.

Neyse ki, [Azure Machine Learning Studio (klasik) yeniden eğitim API 'sini](/azure/machine-learning/studio/retrain-machine-learning-model) kullanarak ve görevi [Azure Machine Learning Studio (klasik) PowerShell](powershell-module.md)ile otomatikleştirerek bunu yapabilirsiniz.

> [!NOTE]
> Örneğinizi daha hızlı çalışır hale getirmek için 1000 konumlardan 10 sayısını azaltın. Ancak aynı ilke ve yordamlar 1.000 konumları için geçerlidir. Ancak, 1.000 veri kümelerinden eğitmek istiyorsanız, paralel olarak aşağıdaki PowerShell komut dosyalarını çalıştırmak isteyebilirsiniz. Bunu nasıl yapacağınız bu makalenin kapsamı dışındadır, ancak PowerShell örneklerini çoklu iş parçacığı Internet'te bulabilirsiniz.  
> 
> 

## <a name="set-up-the-training-experiment"></a>Eğitim deneme ayarlama
[Cortana Intelligence Gallery](https://gallery.azure.ai)olan örnek [eğitim denemenizi](https://gallery.azure.ai/Experiment/Bike-Rental-Training-Experiment-1) kullanın. Bu denemeyi [Azure Machine Learning Studio (klasik)](https://studio.azureml.net) çalışma alanınızda açın.

> [!NOTE]
> Bu örnek ile birlikte izlemek için ücretsiz bir çalışma alanı yerine standart çalışma kullanmak isteyebilirsiniz. Toplam 10 uç noktalar - için - her müşteri için bir uç nokta oluşturun ve ücretsiz bir çalışma alanı 3 uç noktalar ile sınırlı olduğundan, standart bir çalışma alanı gerektirir.
> 
> 

Deneme, bir Azure Storage hesabından *customer001. csv* veri kümesini içeri aktarmak Için **veri alma** modülünü kullanır. Eğitim veri kümelerini tüm Bisiklet Kiralama konumlarından topladığınızı ve *rentalloc001. csv* ' den *rentalloc10. csv*' ye kadar dosya adlarıyla aynı BLOB depolama konumunda depoladığınız varsayıyoruz.

![Okuyucu modülü verileri bir Azure blobundan içeri aktarır](./media/create-models-and-endpoints-with-powershell/reader-module.png)

**Eğitim modeli** modülüne bir **Web hizmeti çıkış** modülü eklendiğini unutmayın.
Bu deneyde, bir web hizmeti olarak dağıtıldığında, uç nokta çıkış eğitilen model .ilearner dosya biçiminde döndürür ilişkili.

Ayrıca, **veri alma** modülünün kullandığı URL 'yi tanımlayan bir Web hizmeti parametresi ayarlamayacağınızı unutmayın. Bu, her konum için modeli eğitmek için tek bir eğitim veri kümesi belirtmek için parametreyi kullanmanıza olanak sağlar.
Bu tamamlayabilirdik farklı yöntemleri vardır. Bir SQL Azure veritabanı'ndan veri almak için bir web hizmeti parametresi ile bir SQL sorgusu kullanabilirsiniz. Ya da Web hizmetine bir veri kümesi geçirmek için **Web hizmeti giriş** modülünü kullanabilirsiniz.

![Eğitilen model modülü bir Web hizmeti çıkış modülüne çıkış verir](./media/create-models-and-endpoints-with-powershell/web-service-output.png)

Şimdi, eğitim veri kümesi olarak *rental001. csv* varsayılan değerini kullanarak bu eğitim denemesini çalıştıralım. **Değerlendirme** modülünün çıkışını (çıktıyı tıklatın ve **Görselleştir**' i seçin) görürseniz, *AUC* = 0,91 performans performansınızı görebilirsiniz. Bu noktada, bu eğitim denemesini dışında bir web hizmeti dağıtmaya hazır olursunuz.

## <a name="deploy-the-training-and-scoring-web-services"></a>Eğitim ve puanlama web hizmetlerini dağıtma
Eğitim Web hizmetini dağıtmak için deneme tuvalinin altındaki **Web hizmeti ayarla** düğmesine tıklayın ve **Web Hizmeti Dağıt**' ı seçin. Bu web hizmeti "Bisiklet kiralama Eğitim" çağırın.

Şimdi Puanlama web hizmeti dağıtmak gerekir.
Bunu yapmak için, tuvalin altında **Web hizmeti ayarla** ' ya tıklayın ve tahmine **dayalı Web hizmeti**' ni seçin. Bu, bir Puanlama deneme oluşturur.
Bir web hizmeti olarak çalışması için birkaç küçük ayarlamalar yapmanız gerekir. "Cnt" etiket sütunu giriş verileri kaldırın ve çıkış yalnızca örnek kimliği ve karşılık gelen tahmin edilen değer sınırlayabilirsiniz.

Bu işi kaydetmek için, önceden hazırlanmış olan galeride tahmine [dayalı](https://gallery.azure.ai/Experiment/Bike-Rental-Predicative-Experiment-1) denemeyi açabilirsiniz.

Web hizmetini dağıtmak için, tahmine dayalı denemeyi çalıştırın, sonra tuvalin altındaki **Web Hizmeti Dağıt** düğmesine tıklayın. Puanlama web hizmeti "Bisiklet kiralama Puanlama" olarak adlandırın.

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>PowerShell ile 10 aynı web hizmeti uç noktası oluşturma
Bu web hizmeti bir varsayılan uç nokta ile birlikte gelir. Ancak bu yana güncelleştirilemiyor varsayılan uç nokta olarak değil. Yapmanız gerekenler 10 ek uç noktalar, her konum için bir tane oluşturmaktır. PowerShell ile bunu yapabilirsiniz.

İlk olarak, PowerShell ortamı ayarlayın:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Ardından, aşağıdaki PowerShell komutunu çalıştırın:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Artık 10 uç noktası oluşturdunuz ve hepsi, *customer001. csv*üzerinde eğitilen eğitilen modeli içeriyor. Bunları Azure portalında görüntüleyebilirsiniz.

![Portalda eğitilen modellerin listesini görüntüleyin](./media/create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>PowerShell kullanarak bir eğitim veri kümeleri kullanılacak uç noktalarını güncelleştir
Sonraki adım her müşterinin tek tek veri benzersiz olarak eğitilen modelleri ile uç noktaları güncelleştirmektir. Ancak öncelikle bu modelleri **Bisiklet Kiralama eğitimi** Web hizmetinden üretmeniz gerekir. **Bisiklet Kiralama eğitimi** Web hizmetine geri dönelim. BES bitim 10 kez 10 farklı bir eğitim veri kümeleri ile 10 farklı modelleri oluşturmak için çağırmanız gerekir. Bunu yapmak için **ınovkeamlwebservicebesendpoint** PowerShell cmdlet 'ini kullanın.

Ayrıca, `$configContent`' ye blob Storage hesabınızın kimlik bilgilerini sağlamanız gerekir. Yani, alanlar `AccountName`, `AccountKey`ve `RelativeLocation`. `AccountName`, **Azure Portal** (*depolama* sekmesi) görüldüğü gibi hesap adlarınızın biri olabilir. Bir depolama hesabına tıkladığınızda, en alttaki **erişim tuşlarını Yönet** düğmesine basılarak ve *birincil erişim anahtarı*kopyalanırken `AccountKey` bulunabilir. `RelativeLocation`, depolama alanı ile ilgili yeni bir modelin depolanacağı yoldur. Örneğin, aşağıdaki betikteki yol `hai/retrain/bike_rental/` `hai`adlı bir kapsayıcıya işaret eder ve `/retrain/bike_rental/` alt klasörlerdir. Şu anda, Portal Kullanıcı arabirimi aracılığıyla alt klasör oluşturamazsınız, ancak bunu yapmanıza izin veren [birkaç Azure depolama araştırması](../../storage/common/storage-explorers.md) vardır. Yeni eğitilen modelleri (. iLearner dosyaları) şu şekilde depolamak için depolama ortamınızda yeni bir kapsayıcı oluşturmanız önerilir: depolama sayfanızda, alttaki **Ekle** düğmesine tıklayın ve `retrain`adlandırın. Özet olarak, aşağıdaki betikte gerekli değişiklikler `AccountName`, `AccountKey`ve `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`) ile ilgilidir.

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> BES uç nokta bu işlem için desteklenen tek moddur. RRS eğitilen modelleri üretmek için kullanılamaz.
> 
> 

10 farklı BES iş yapılandırma json dosyalarını oluşturmak yerine, yukarıdaki görebileceğiniz gibi dinamik olarak yapılandırma dizesi yerine oluşturun. Ardından, **ınvokeamlwebservcebesendpoint** cmdlet 'Inin *jobconfigstring* parametresine akışı yapın. Gerçekten diskte bir kopyasını tutmaya gerek yoktur.

Her şey iyi olursa bir süre sonra, Azure depolama hesabınızda *model001. ilearner* 'den *model010. ilearner*'e kadar 10. ilearner dosyası görmeniz gerekir. Artık **Patch-AmlWebServiceEndpoint** PowerShell cmdlet 'ini kullanarak bu modellerle 10 Puanlama Web hizmeti uç noktasını güncelleştirmeye hazırsınız. Program aracılığıyla daha önce oluşturduğunuz varsayılan olmayan uç noktaları yalnızca düzeltme yeniden unutmayın.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Bu oldukça hızlı bir şekilde çalışması gerekir. Yürütme sona erdiğinde, başarıyla oluşturuldu 10 Tahmine dayalı web hizmeti uç noktası. Her biri benzersiz bir tek eğitim denemesini tümünden kiralama konuma belirli veri kümesi eğitilmiş eğitilen bir modelin içerir. Bunu doğrulamak için, **ınvokeamlwebservicerrsendpoint** cmdlet 'ini kullanarak bu uç noktaları çağırmayı deneyebilirsiniz ve bunları aynı giriş verileriyle birlikte sağlayabilirsiniz. Modelleri farklı eğitim kümeleriyle eğitilir olduğundan farklı tahmin sonuçlarını görmek beklemeniz gerekir.

## <a name="full-powershell-script"></a>Tam PowerShell Betiği
Tam kaynak kodu listesi aşağıda verilmiştir:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
