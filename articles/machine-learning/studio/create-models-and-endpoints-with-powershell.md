---
title: Model için birden çok uç nokta oluşturma
titleSuffix: ML Studio (classic) - Azure
description: Aynı algoritmaya sahip, ancak farklı eğitim veri kümelerine sahip birden çok Machine Learning modeli ve web hizmeti bitiş noktası oluşturmak için PowerShell'i kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 04/04/2017
ms.openlocfilehash: 4f8bb1f20dcc3a7900e3347616018a6e156962d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218182"
---
# <a name="use-powershell-to-create-studio-classic-models-and-web-service-endpoints-from-one-experiment"></a>Studio (klasik) modelleri ve web hizmeti bitiş noktaları oluşturmak için PowerShell'i kullanın

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Burada yaygın bir makine öğrenme sorunu: Aynı eğitim iş akışına sahip ve aynı algoritmayı kullanan birçok model oluşturmak istiyorsunuz. Ancak, giriş olarak farklı eğitim veri kümelerine sahip olmalarını istersiniz. Bu makalede, azure machine learning studio 'da (klasik) yalnızca tek bir deneme kullanarak bunu ölçekte nasıl yapacağınızı gösterilmektedir.

Örneğin, küresel bir bisiklet kiralama franchise iş sahibi varsayalım. Geçmiş verilere dayalı olarak kiralama talebini tahmin etmek için bir regresyon modeli oluşturmak istiyorsunuz. Dünya çapında 1.000 kiralama yeriniz vardır ve her konum için bir veri kümesi topladınız. Bunlar, her konuma özgü tarih, saat, hava durumu ve trafik gibi önemli özellikleri içerir.

Tüm konumlardaki tüm veri kümelerinin birleştirilmiş sürümünü kullanarak modelinizi bir kez eğitebilirsiniz. Ancak, konumlarınızın her birinin benzersiz bir ortamı vardır. Yani daha iyi bir yaklaşım her konum için veri kümesini kullanarak ayrı ayrı regresyon modeli eğitmek olacaktır. Bu şekilde, her eğitimli model dikkate farklı mağaza boyutları, hacim, coğrafya, nüfus, bisiklet dostu trafik ortamı ve daha alabilir.

Bu en iyi yaklaşım olabilir, ancak Azure Machine Learning Studio'da (klasik) her biri benzersiz bir konumu temsil eden 1.000 eğitim deneyi oluşturmak istemezsiniz. Ezici bir görev olmasının yanı sıra, her deneme eğitim veri kümesi dışında tüm aynı bileşenlere sahip olacağı için de verimsiz görünüyor.

Neyse ki, Bunu Azure [Machine Learning Studio (klasik) YENIDEN Eğitim API'sini](/azure/machine-learning/studio/retrain-machine-learning-model) kullanarak ve [Azure Machine Learning Studio (klasik) PowerShell](powershell-module.md)ile görevi otomatikleştirerek başarabilirsiniz.

> [!NOTE]
> Numunenizin daha hızlı çalışmasını sağlamak için konum sayısını 1.000'den 10'a düşürün. Ancak aynı ilke ve prosedürler 1000 yer için de geçerli. Ancak, 1.000 veri kümelerinden eğitim almak istiyorsanız, aşağıdaki PowerShell komut dosyalarını paralel olarak çalıştırmak isteyebilirsiniz. Nasıl bu makalenin kapsamı dışında, ancak Internet'te PowerShell çoklu iş parçacığı örnekleri bulabilirsiniz.  
> 
> 

## <a name="set-up-the-training-experiment"></a>Eğitim denemesini ayarlama
[Cortana İstihbarat Galerisi'ndeki](https://gallery.azure.ai)örnek [eğitim denemesini](https://gallery.azure.ai/Experiment/Bike-Rental-Training-Experiment-1) kullanın. Bu denemeyi [Azure Machine Learning Studio (klasik)](https://studio.azureml.net) çalışma alanınızda açın.

> [!NOTE]
> Bu örneği izlemek için, boş bir çalışma alanı yerine standart bir çalışma alanı kullanmak isteyebilirsiniz. Her müşteri için toplam 10 uç nokta için bir bitiş noktası oluşturursunuz ve ücretsiz çalışma alanı 3 uç noktayla sınırlı olduğundan standart bir çalışma alanı gerektirir.
> 
> 

Deneme, bir Azure depolama hesabından eğitim veri kümesi *müşteri001.csv* almak için bir **Alma Veri** modülü kullanır. Tüm bisiklet kiralama yerlerinden eğitim veri setleri topladığınızı ve *bunları rentalloc001.csv'den* *rentalloc10.csv'ye*kadar dosya adlarıyla aynı blob depolama yerinde depoladığınızı varsayalım.

![Okuyucu modülü azure blob'undan veri aktarır](./media/create-models-and-endpoints-with-powershell/reader-module.png)

**Tren Modeli** modülüne bir Web **Hizmeti Çıktı** modülü eklendiğini unutmayın.
Bu deneme bir web hizmeti olarak dağıtıldığında, bu çıktıyla ilişkili bitiş noktası, eğitilen modeli .ilearner dosyası biçiminde döndürür.

Ayrıca, **İçe Aktar Veri** modülünün kullandığı URL'yi tanımlayan bir web hizmeti parametresi ayarladığınızı da unutmayın. Bu, her konum için modeli eğitmek için tek tek eğitim veri kümelerini belirtmek için parametreyi kullanmanıza olanak tanır.
Bunu yapmanın başka yolları da var. BIR SQL Azure veritabanından veri almak için web hizmeti parametresi olan bir SQL sorgusu kullanabilirsiniz. Veya bir veri kümesinden web hizmetine geçmek için bir **Web Hizmeti Giriş** modülü kullanabilirsiniz.

![Bir Web hizmeti çıktı modülüne Eğitilmiş Model modülü çıktıları](./media/create-models-and-endpoints-with-powershell/web-service-output.png)

Şimdi, bu eğitim denemesini varsayılan değer *kiralama001.csv'yi* eğitim veri seti olarak kullanarak çalıştıralım. **Değerlendirme** modülünün çıktısını görürseniz (çıktıyı tıklatın ve **Visualize'ı**seçin), *AUC* = 0,91'in iyi bir performansını elde ettiğinizi görebilirsiniz. Bu noktada, bu eğitim denemesinin dışında bir web hizmeti dağıtmaya hazırsınız.

## <a name="deploy-the-training-and-scoring-web-services"></a>Eğitim ve puanlama web hizmetlerini dağıtma
Eğitim web hizmetini dağıtmak için, deneme tuvalinin altındaki **Web Hizmeti Ayarla** düğmesini tıklatın ve **Web Hizmeti Dağıt'ı**seçin. Bu web hizmeti "Bisiklet Kiralama Eğitimi" arayın.

Şimdi puanlama web hizmetini dağıtmanız gerekir.
Bunu yapmak için, tuvalin altındaki **Web Hizmetini Ayarla'yı** tıklatın ve **Tahmine Dayalı Web Hizmeti'ni**seçin. Bu bir puanlama deneyi oluşturur.
Bir web hizmeti olarak çalışması için birkaç küçük ayarlamalar yapmanız gerekir. Giriş verilerinden etiket sütunu "cnt" kaldırın ve çıktıyı yalnızca örnek id ve ilgili tahmin edilen değerle sınırlayın.

Kendinizi bu işi kurtarmak için, önceden hazırlanmış olan Galeri'de [tahmine dayalı deneyi](https://gallery.azure.ai/Experiment/Bike-Rental-Predicative-Experiment-1) açabilirsiniz.

Web hizmetini dağıtmak için tahmin etüdlerini çalıştırın ve tuvalin altındaki **Web Hizmeti Dağıt** düğmesini tıklatın. Puanlama web hizmeti "Bisiklet Kiralama Puanlama" adı.

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>PowerShell ile 10 aynı web hizmeti bitiş noktası oluşturun
Bu web hizmeti varsayılan bir bitiş noktası ile birlikte gelir. Ancak güncelleştirilemediğinden varsayılan bitiş noktasıyla ilgilenmiyorsunuz. Yapmanız gereken, her konum için bir tane olmak üzere 10 ek uç nokta oluşturmaktır. Bunu PowerShell ile yapabilirsiniz.

İlk olarak, PowerShell ortamını ayarlarsınız:

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

Şimdi 10 uç nokta oluşturdu ve hepsi *customer001.csv*eğitimli aynı eğitimli modeli içerir. Bunları Azure portalında görüntüleyebilirsiniz.

![Portaldaki eğitimli modellerin listesini görüntüleyin](./media/create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>PowerShell'i kullanarak ayrı eğitim veri kümelerini kullanmak için uç noktaları güncelleştirin
Bir sonraki adım, uç noktaları her müşterinin bireysel verileri üzerinde benzersiz olarak eğitilmiş modellerle güncelleştirmektir. Ama önce **Bisiklet Kiralama Eğitim** web hizmeti bu modelleri üretmek gerekir. **Bisiklet Kiralama Eğitimi** web hizmetine geri dönelim. 10 farklı model üretmek için 10 farklı eğitim veri seti ile BES uç noktasını 10 kez aramanız gerekir. Bunu yapmak için **InovkeAmlWebServiceBESEndpoint** PowerShell cmdlet kullanın.

Ayrıca blob depolama hesabınız için kimlik bilgilerini `$configContent`sağlamanız gerekir. Yani, tarlalarda `AccountName` `AccountKey`, `RelativeLocation`ve . Azure `AccountName` **portalında** *(Depolama* sekmesi) görüldüğü gibi hesap adlarınızdan biri olabilir. Bir depolama hesabına tıkladığınızda, en alttaki **Erişim Anahtarlarını Yönet** düğmesine basarak ve *Birincil Erişim Anahtarı'nı*kopyalayarak bu `AccountKey` hesabı bulabilirsiniz. Bu, `RelativeLocation` depolama alanınıza göre yeni bir modelin depolanacağı yoldur. Örneğin, aşağıdaki `hai/retrain/bike_rental/` komut dosyasındaki yol, adlı `hai`bir `/retrain/bike_rental/` kapsayıcıya işaret eder ve alt klasörlerdir. Şu anda, portal Kullanıcı Arabirimi üzerinden alt klasörler oluşturamazsınız, ancak bunu yapmanıza izin veren [birkaç Azure Depolama Gezgini](../../storage/common/storage-explorers.md) vardır. Yeni eğitilmiş modelleri (.iLearner dosyaları) aşağıdaki gibi depolamak için depolama alanınızda yeni bir kapsayıcı **Add** oluşturmanız önerilir: depolama `retrain`sayfanızdan, alttaki Ekle düğmesini tıklatın ve adını adlandırın. Özetle, aşağıdaki komut dosyasında gerekli `AccountName`değişiklikler `AccountKey`, `RelativeLocation` ,`"retrain/model' + $seq + '.ilearner"`ve (: ) ile ilgilidir.

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
> BES bitiş noktası, bu işlem için desteklenen tek moddur. RRS eğitimli modeller üretmek için kullanılamaz.
> 
> 

Yukarıda gördüğünüz gibi, 10 farklı BES iş yapılandırma json dosyaları oluşturmak yerine, dinamik yerine config dize oluşturmak. Daha sonra **InvokeAmlWebServceBESEndpoint** cmdlet'in *jobConfigString* parametresine yedirin. Bir kopyasını diskte saklamaya gerek yok.

Her şey yolunda giderse, bir süre sonra 10 .iLearner dosyaları, *model001.ilearner* *model010.ilearner*, Azure depolama hesabınızda görmelisiniz. Şimdi **Patch-AmlWebServiceEndpoint** PowerShell cmdlet kullanarak bu modeller ile 10 puanlama web hizmeti uç noktaları güncellemeye hazırsınız. Yalnızca daha önce programlı olarak oluşturduğunuz varsayılan olmayan uç noktaları yamalayabileceğinizi yeniden unutmayın.

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

Bu oldukça hızlı bir şekilde çalışması gerekir. Yürütme sona erdiğinde, 10 tahmine dayalı web hizmeti bitiş noktası nı başarıyla oluşturmuş olacaksınız. Her biri, tek bir eğitim deneyinden, kiralık bir konuma özgü veri seti üzerinde benzersiz olarak eğitilmiş eğitimli bir model içerecektir. Bunu doğrulamak için, **invokeAmlWebServiceRRSEndpoint** cmdlet kullanarak bu uç noktaları aramayı deneyebilir ve onlara aynı giriş verilerini sağlayabilirsiniz. Modeller farklı eğitim setleri ile eğitildiği için farklı tahmin sonuçları görmeyi beklemelisiniz.

## <a name="full-powershell-script"></a>Tam PowerShell komut dosyası
İşte tam kaynak kodunun listesi:

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
