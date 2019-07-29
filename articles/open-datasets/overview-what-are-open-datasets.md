---
title: Açık veri kümeleri nelerdir? Seçkin ortak veri kümeleri
titleSuffix: Azure Open Datasets (preview)
description: Makine öğrenimi ve analiz çözümlerinde kullanıma hazırlamış olan genel etki alanındaki Azure açık veri kümeleri (Önizleme), seçkin veri kümeleri hakkında bilgi edinin. Veri kümeleri, öngörülü çözümleri zenginleştirmenize yardımcı olmak için hava durumu, Census, tatiller ve konum gibi genel verileri içerir.
ms.service: open-datasets
ms.topic: overview
author: cjgronlund
ms.author: cgronlun
ms.date: 05/02/2019
ms.openlocfilehash: 7fa8562c466c81ebc05360bf06cdde1ee29bed9e
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598975"
---
# <a name="what-are-azure-open-datasets-preview-and-how-can-you-use-them"></a>Azure açık veri kümeleri (Önizleme) nedir ve nasıl kullanılabilir?

[Azure açık veri](https://azure.microsoft.com/services/open-datasets/) kümeleri, daha doğru modeller için makine öğrenimi çözümlerine senaryoya özgü özellikler eklemek için kullanabileceğiniz, seçkin ortak veri kümeleridir. Açık veri kümeleri bulutta Microsoft Azure ve Azure Databricks, Machine Learning hizmeti ve Machine Learning Studio için hazır. Ayrıca, veri kümelerine API 'Ler aracılığıyla erişebilir ve bunları Power BI ve Azure Data Factory gibi diğer ürünlerde kullanabilirsiniz.

Veri kümeleri, makine öğrenimi modellerini ve zenginleştirme çözümlerini eğitmenize yardımcı olan hava durumu, Census, tatiller, genel güvenlik ve konum için genel etki alanı verilerini içerir. Ayrıca, Azure açık veri kümelerinde ortak veri kümelerinizi de paylaşabilirsiniz. 

![Azure açık veri kümesi bileşenleri](./media/overview-what-are-open-datasets/open-datasets-components.png)

## <a name="curated-prepared-datasets"></a>Seçkin, hazırlanan veri kümeleri
Azure açık veri kümelerinde seçkin açık genel veri kümeleri, makine öğrenimi iş akışlarında tüketim için iyileştirilmiştir. 

Veri bilimcileri, genellikle gelişmiş analizler için verileri temizleme ve hazırlama zamanının çoğunu harcamaktadır. Açık veri kümeleri Azure bulutuna kopyalanır ve zamandan tasarruf etmek için önceden işlenir. Düzenli aralıklarla veriler, Ulusal Okyanus ve atmosfer yönetimine (NOAA) yönelik FTP bağlantısı, yapılandırılmış bir biçimde ayrıştırılıp ZIP kodu veya konumu gibi özelliklerle uygun şekilde zenginleştirerek kaynaklar tarafından çekilir. En yakın hava durumu istasyonu.

Veri kümeleri, Azure 'da erişim ve düzenleme daha kolay hale getirerek bulut işlem ile birlikte barındırılır.  

Aşağıda, kullanılabilir veri kümeleri örnekleri verilmiştir. 

### <a name="weather-data"></a>Hava durumu verileri
 
|Veri kümesi         | Notebooks     | Açıklama                                    |
|----------------|---------------|------------------------------------------------|
|[NOAA tümleşik yüzey verileri (ıSD)](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/?tab=data-access#AzureDatabricks) | Kuzey Amerika, Avrupa, Avustralya ve Asya parçaları için en iyi uzamsal kapsama sahip NOAA 'den dünya çapındaki saatlik hava durumu verileri. Günlük olarak güncelleştirilir. |
|[NOAA küresel tahmin sistemi (GFS)](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/?tab=data-access#AzureDatabricks) | NOAA 'den 15 günlük ABD/saat hava durumu tahmin verileri. Günlük olarak güncelleştirilir. |

### <a name="calendar-data"></a>Takvim verileri

|Veri kümesi         | Notebooks     | Açıklama                                    |
|----------------|---------------|------------------------------------------------|
|[Ortak tatiller](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/?tab=data-access#AzureDatabricks) | 41 ülke veya bölge ile 2099 1970 arasındaki dünya çapında ortak tatil verileri. Ülke ve insanların büyük bir süre kapalı olup olmadığını içerir. |

## <a name="access-to-datasets"></a>Veri kümelerine erişim  
Azure hesabıyla, kod kullanarak veya Azure hizmet arabirimi aracılığıyla açık veri kümelerine erişebilirsiniz. Veriler, Machine Learning çözümünüzde kullanılmak üzere Azure bulut işlem kaynaklarıyla birlikte bulunur.  

Açık veri kümeleri, verileri Azure Machine Learning hizmetine ve Azure Databricks bağlamak için kullanabileceğiniz Azure Notebooks ve Azure Databricks Not defterleri sağlar. Veri kümelerine bir Python SDK üzerinden de erişilebilir. 

Ancak açık veri kümelerine erişmeniz için bir Azure hesabınızın olması gerekmez; kendileriyle, Spark olmadan veya eklemeden herhangi bir Python ortamından erişebilirsiniz.

## <a name="request-or-contribute-datasets"></a>Veri kümeleri isteme veya katkıda bulunma

İstediğiniz verileri bulamazsanız, bir veri kümesi [istemek](mailto:aod@microsoft.com?Subject=Request%20dataset%3A%20%3Creplace%20with%20dataset%20name%3E&Body=%0AYour%20name%20and%20institution%3A%20%0A%0ADataset%20name%3A%0A%20%0ADataset%20description%3A%20%0A%3Cfill%20in%20a%20brief%20description%20and%20share%20any%20web%20links%20of%20the%20dataset%3E%20%0A%0ADataset%20size%3A%20%0A%3Chow%20much%20space%20does%20the%20dataset%20need%20today%20and%20how%20much%20is%20it%20expected%20to%20grow%20each%20year%3E%20%0A%0ADataset%20file%20formats%3A%20%0A%3Ccurrent%20dataset%20file%20formats%2C%20and%20optionally%2C%20any%20formats%20that%20the%20dataset%20must%20be%20transformed%20to%20for%20easy%20access%3E%0A%0ALicense%3A%20%0A%3Cwhat%20is%20the%20license%20or%20terms%20and%20conditions%20governing%20the%20distribution%20of%20this%20dataset%3E%0A%0AUse%20cases%3A%20%0A%3CExplain%20some%20common%20use%20of%20the%20dataset.%20E.g.%20weather%20dataset%20can%20be%20useful%20in%20demand%20forecasting%20and%20predictive%20maintenance%20scenarios%3E%20%0A%0AAny%20additional%20information%20you%20want%20us%20to%20know%3A%0A) veya [bir veri kümesi ile katkıda](mailto:aod@microsoft.com?Subject=Contribute%20dataset%3A%20%3Creplace%20with%20dataset%20name%3E&Body=%0AYour%20name%20and%20institution%3A%20%0A%0ADataset%20name%3A%0A%20%0ADataset%20description%3A%20%0A%3Cfill%20in%20a%20brief%20description%20and%20share%20any%20web%20links%20of%20the%20dataset%3E%20%0A%0ADataset%20size%3A%20%0A%3Chow%20much%20space%20does%20the%20dataset%20need%20today%20and%20how%20much%20is%20it%20expected%20to%20grow%20each%20year%3E%20%0A%0ADataset%20file%20formats%3A%20%0A%3Ccurrent%20dataset%20file%20formats%2C%20and%20optionally%2C%20any%20formats%20that%20the%20dataset%20must%20be%20transformed%20to%20for%20easy%20access%3E%0A%0ALicense%3A%20%0A%3Cwhat%20is%20the%20license%20or%20terms%20and%20conditions%20governing%20the%20distribution%20of%20this%20dataset%3E%0A%0AUse%20cases%3A%20%0A%3CExplain%20some%20common%20use%20of%20the%20dataset.%20E.g.%20weather%20dataset%20can%20be%20useful%20in%20demand%20forecasting%20and%20predictive%20maintenance%20scenarios%3E%20%0A%0AAny%20additional%20information%20you%20want%20us%20to%20know%3A%0A)bulunmak için bize e-posta gönderin. 

## <a name="next-steps"></a>Sonraki adımlar
* [Örnek Not defteri](samples.md)
* [Öğretici: NY TAXI verileri ile gerileme modelleme](tutorial-opendatasets-automl.md)
* [Açık veri kümeleri için Python SDK](/python/api/azureml-opendatasets/?view=azure-ml-py)
