---
title: Azure Akış Analitiğini Azure Makine Öğrenimi ile tümleştirin
description: Bu makalede, bir Azure Akış Analizi işinin Azure Machine Learning modelleri ile nasıl tümleştirilen açıklanmaktadır.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: 354b0eae1620c49006b350371aae38f8867c18b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124665"
---
# <a name="integrate-azure-stream-analytics-with-azure-machine-learning-preview"></a>Azure Akış Analizini Azure Makine Öğrenimi ile Tümleştir (Önizleme)

Azure Akış Analizi işlerinizde, akış giriş verileriniz üzerinde gerçek zamanlı puanlama ve tahminler yapmak için makine öğrenimi modellerini kullanıcı tanımlı bir işlev (UDF) olarak uygulayabilirsiniz. [Azure Machine Learning,](../machine-learning/overview-what-is-azure-ml.md) modelleri hazırlamak, eğitmek ve dağıtmak için Tensorflow, scikit-learn veya PyTorch gibi popüler açık kaynak kullanımını sağlar.

> [!NOTE]
> Bu işlevsellik genel önizlemede dir. Bu özelliğe Azure portalından yalnızca [Akış Analizi portalı önizleme bağlantısını](https://aka.ms/asaportalpreview)kullanarak erişebilirsiniz. Bu işlevsellik, Visual Studio [için Stream Analytics araçlarının](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-install)en son sürümünde de mevcuttur.

## <a name="prerequisites"></a>Ön koşullar

Akış Analizi işinize bir işlev olarak bir makine öğrenimi modeli eklemeden önce aşağıdaki adımları tamamlayın:

1. Modelinizi bir web hizmeti olarak dağıtmak için Azure Machine [Learning'i](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where)kullanın.

2. Puanlama komut dosyanızda, Azure Machine Learning tarafından şema belirtimi oluşturmak için kullanılan [örnek giriş ve çıktıları](../machine-learning/how-to-deploy-and-where.md#example-entry-script) olmalıdır. Stream Analytics, web hizmetinizin işlev imzasını anlamak için şemayı kullanır.

3. Web hizmetinizin JSON seri leştirilmiş verileri kabul edip döndürdiğinden emin olun.

4. Yüksek ölçekli üretim dağıtımları için modelinizi [Azure Kubernetes Hizmeti'nde](../machine-learning/how-to-deploy-and-where.md#choose-a-compute-target) dağıtın. Web hizmeti işinizden gelen istek sayısını kaldıramazsa, Stream Analytics işinizin performansı düşürülür ve bu da gecikmeyi etkiler.

## <a name="add-a-machine-learning-model-to-your-job"></a>İşinize bir makine öğrenme modeli ekleyin

Azure Machine Learning işlevlerini Doğrudan Azure portalından Akış Analizi işinize ekleyebilirsiniz.

1. Azure portalındaki Akış Analizi işinize gidin ve İş topolojisi altında İş **topolojisi**altında **İş Işlevleri'ni** seçin. Ardından, + Açılır Menü **ekle** menüsünden Azure **ML Hizmeti'ni** seçin.

   ![Azure ML UDF Ekle](./media/machine-learning-udf/add-azureml-udf.png)

2. **Azure Machine Learning Service işlev** formunu aşağıdaki özellik değerleriyle doldurun:

   ![Azure ML UDF'yi yapılandırma](./media/machine-learning-udf/configure-azureml-udf.png)

Aşağıdaki tabloda, Stream Analytics'teki Azure ML Hizmeti işlevlerinin her özelliği açıklanmaktadır.

|Özellik|Açıklama|
|--------|-----------|
|İşlev takma adı|Sorgunuzdaki işlevi çağırmak için bir ad girin.|
|Abonelik|Azure aboneliğiniz...|
|Azure ML çalışma alanı|Modelinizi web hizmeti olarak dağıtmak için kullandığınız Azure Machine Learning çalışma alanı.|
|Dağıtımlar|Modelinizi barındıran web hizmeti.|
|İşlev imzası|Web hizmetinizin imzası API'nin şema belirtiminden çıkarıldı. İmzanız yüklenmezse, şemayı otomatik olarak oluşturmak için puanlama komut dosyanıza örnek giriş ve çıktı sağladığınızı kontrol edin.|
|Bölüm başına paralel istek sayısı|Bu, yüksek ölçekli iş akışını optimize etmek için gelişmiş bir yapılandırmadır. Bu sayı, işinizin her bir bölümünden web hizmetine gönderilen eşzamanlı istekleri temsil eder. Altı akış birimi (SU) ve daha düşük olan işlerde bir bölüm vardır. 12 SUs ile İşler iki bölüm, 18 SUs üç bölüm ve benzeri var.<br><br> Örneğin, işinizde iki bölüm varsa ve bu parametreyi dörde ayarlarsanız, işinizden web hizmetinize sekiz eşzamanlı istek olacaktır. Genel önizlemenin bu zamanında, bu değer varsayılan olarak 20'ye kadar dır ve güncelleştirilemez.|
|Maksimum toplu iş sayısı|Bu, yüksek ölçekli iş ortası en iyi duruma geçirmek için gelişmiş bir yapılandırmadır. Bu sayı, web hizmetinize gönderilen tek bir istekte bir araya getirilecek en fazla olay sayısını temsil eder.|

## <a name="supported-input-parameters"></a>Desteklenen giriş parametreleri

Akış Analizi sorgunuz bir Azure Machine Learning UDF'yi çağırdığında, iş web hizmeti için JSON serileştirilmiş bir istek oluşturur. İstek, modele özgü bir şemayı temel alın. Otomatik [olarak bir şema oluşturmak](../machine-learning/how-to-deploy-and-where.md)için puanlama komut dosyasında örnek giriş ve çıktı sağlamanız gerekir. Şema, Stream Analytics'in numpy, pandalar ve PySpark gibi desteklenen veri türlerinden herhangi biri için JSON serileştirilmiş isteği oluşturmasına olanak tanır. Birden çok giriş olayı tek bir istekte bir araya getirilebilir.

Aşağıdaki Akış Analizi sorgusu, Azure Machine Learning UDF'yi nasıl çağırılanabildiğini niçin başlatılabildiğini gösterir:

```SQL
SELECT udf.score(<model-specific-data-structure>)
INTO output
FROM input
```

Akış Analizi, Azure Machine Learning işlevleri için yalnızca bir parametre geçmeyi destekler. Verilerinizi makine öğrenimi UDF'sine bir girdi olarak aktarmadan önce hazırlamanız gerekebilir.

## <a name="pass-multiple-input-parameters-to-the-udf"></a>Birden çok giriş parametresini UDF'ye aktarma

Makine öğrenimi modellerine girişlerin en yaygın örnekleri sayısal diziler ve DataFrame'lerdir. JavaScript UDF kullanarak bir dizi oluşturabilir ve yan tümceyi `WITH` kullanarak JSON serileştirilmiş bir DataFrame oluşturabilirsiniz.

### <a name="create-an-input-array"></a>Giriş dizisi oluşturma

*N* giriş sayısını kabul eden ve Azure Machine Learning UDF'nize girdi olarak kullanılabilecek bir dizi oluşturan bir JavaScript UDF oluşturabilirsiniz.

```javascript
function createArray(vendorid, weekday, pickuphour, passenger, distance) {
    'use strict';
    var array = [vendorid, weekday, pickuphour, passenger, distance]
    return array;
}
```

JavaScript UDF'yi işinize ekledikten sonra, aşağıdaki sorguyu kullanarak Azure Machine Learning UDF'nizi çağırabilirsiniz:

```SQL
SELECT udf.score(
udf.createArray(vendorid, weekday, pickuphour, passenger, distance)
)
INTO output
FROM input
```

Aşağıdaki JSON bir örnek istektir:

```JSON
{
    "data": [
        ["1","Mon","12","1","5.8"],
        ["2","Wed","10","2","10"]
    ]
}
```

### <a name="create-a-pandas-or-pyspark-dataframe"></a>Pandalar veya PySpark DataFrame oluşturma

`WITH` Alttaki şekilde Azure Machine Learning UDF'nize girdi olarak geçirilebilen JSON seri leştirilmiş bir DataFrame oluşturmak için yan tümceyi kullanabilirsiniz.

Aşağıdaki sorgu, gerekli alanları seçerek bir DataFrame oluşturur ve Azure Machine Learning UDF'ye giriş olarak DataFrame'i kullanır.

```SQL
WITH 
Dataframe AS (
SELECT vendorid, weekday, pickuphour, passenger, distance
FROM input
)

SELECT udf.score(Dataframe)
INTO output
FROM input
```

Aşağıdaki JSON önceki sorgudan bir örnek istektir:

```JSON
{
    "data": [{
            "vendorid": "1",
            "weekday": "Mon",
            "pickuphour": "12",
            "passenger": "1",
            "distance": "5.8"
        }, {
            "vendorid": "2",
            "weekday": "Tue",
            "pickuphour": "10",
            "passenger": "2",
            "distance": "10"
        }
    ]
}
```

## <a name="optimize-the-performance-for-azure-machine-learning-udfs"></a>Azure Machine Learning UDF'leri için performansı optimize edin

Modelinizi Azure Kubernetes Hizmetine dağıttığınızda, [kaynak kullanımını belirlemek için modelinizin profilini](../machine-learning/how-to-deploy-and-where.md#profilemodel)oluşturabilirsiniz. Ayrıca, istek oranlarını, yanıt sürelerini ve hata oranlarını anlamak [için dağıtımlarınız için App](../machine-learning/how-to-enable-app-insights.md) Insights'ı etkinleştirebilirsiniz.

Yüksek olay verime sahip bir senaryonuz varsa, düşük uçtan uca gecikmelerle en iyi performansı elde etmek için Akış Analizi'nde aşağıdaki parametreleri değiştirmeniz gerekebilir:

1. Maksimum parti sayısı.
2. Bölüm başına paralel istek sayısı.

### <a name="determine-the-right-batch-size"></a>Doğru toplu iş boyutunu belirleme

Web hizmetinizi dağıttıktan sonra, 50'den başlayarak ve yüzlerce sırayla artırarak farklı toplu iş boyutlarında örnek istek gönderirsiniz. Örneğin, 200, 500, 1000, 2000 ve benzeri. Belirli bir toplu iş boyutundan sonra yanıtın gecikmesinin arttığını fark edeceksiniz. Yanıt gecikmesinin arttığı nokta, işiniz için maksimum toplu işlem sayısı olmalıdır.

### <a name="determine-the-number-of-parallel-requests-per-partition"></a>Bölüm başına paralel istek sayısını belirleme

En iyi ölçeklemede, Stream Analytics işin web hizmetinize birden fazla paralel istek gönderebilmeli ve birkaç milisaniye içinde yanıt alabilmeli. Web hizmetinin yanıtının gecikmesi, Stream Analytics işinizin gecikmesini ve performansını doğrudan etkileyebilir. İşinizden web hizmetine yapılan arama uzun zaman alıyorsa, filigran gecikmesinde büyük olasılıkla bir artış göreceksiniz ve biriktirme kaydedilmiş giriş olaylarının sayısında da bir artış görebilirsiniz.

Bu tür gecikmeyi önlemek için, Azure Kubernetes Service (AKS) [kümenizin doğru sayıda düğüm ve yinelemeyle](../machine-learning/how-to-deploy-azure-kubernetes-service.md#using-the-cli)sağlandığından emin olun. Web hizmetinizin son derece kullanılabilir olması ve başarılı yanıtlar sağlaması çok önemlidir. İşiniz web hizmetinizden kullanılamayan bir hizmet yanıtı (503) alırsa, üstel geri çekimle sürekli olarak yeniden dener. Başarı (200) ve kullanılamayan hizmet (503) dışındaki herhangi bir yanıt, işinizin başarısız bir duruma gitmesine neden olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Stream Analytics JavaScript kullanıcı tanımlı işlevleri](stream-analytics-javascript-user-defined-functions.md)
* [Azure Machine Learning Studio (klasik) işleviyle Stream Analytics işinizi ölçeklendirin](stream-analytics-scale-with-machine-learning-functions.md)

