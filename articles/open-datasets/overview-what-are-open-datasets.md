---
title: Açık veri kümeleri nedir? Genel veri kümeleri
titleSuffix: Azure Open Datasets
description: Tahmine dayalı çözümleri zenginleştirmek için Azure Açık Veri kümeleri, hava durumu, nüfus sayımı, tatiller ve konum gibi genel etki alanından seçilmiş veri kümeleri hakkında bilgi edinin.
ms.service: open-datasets
ms.topic: overview
author: cjgronlund
ms.author: cgronlun
ms.date: 11/04/2019
ms.openlocfilehash: fd5697f9c325dc4ad866c333ce1b20e008ebfa24
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73606168"
---
# <a name="what-are-azure-open-datasets-and-how-can-you-use-them"></a>Azure Açık Veri Setleri nedir ve bunları nasıl kullanabilirsiniz?

[Azure Açık Veri Kümeleri,](https://azure.microsoft.com/services/open-datasets/) daha doğru modeller için makine öğrenimi çözümlerine senaryoya özgü özellikler eklemek için kullanabileceğiniz genel veri kümeleridir. Açık Veri Setleri Microsoft Azure'da buluttadır ve Azure Machine Learning'e entegre edilir ve Azure Databricks ve Machine Learning Studio (klasik) tarafından kolayca kullanılabilir. Ayrıca, veri kümelerine API'ler aracılığıyla erişebilir ve bunları Power BI ve Azure Veri Fabrikası gibi diğer ürünlerde de kullanabilirsiniz.

Veri kümeleri, hava durumu, nüfus sayımı, tatiller, genel güvenlik ve makine öğrenimi modellerini eğitmenize ve tahmine dayalı çözümleri zenginleştirmenize yardımcı olan konum için genel alan verilerini içerir. Azure Açık Veri Kümelerinde herkese açık veri kümelerinizi de paylaşabilirsiniz. 

![Azure Açık Veri Setleri bileşenleri](./media/overview-what-are-open-datasets/open-datasets-components.png)

## <a name="curated-prepared-datasets"></a>Küratörlü, hazırlanmış veri kümeleri
Azure Açık Veri kümelerinde genel kullanıma açık veri kümeleri, makine öğrenimi iş akışlarında tüketim için optimize edilmiştir. 

Veri bilimciler zamanlarının çoğunu gelişmiş analizler için veri temizlemek ve hazırlamakla geçirirler. Açık Veri Kümeleri Azure bulutuna kopyalanır ve zamandan tasarruf etmenizi sağlamak için önceden işlenir. Düzenli aralıklarla veriler kaynaklardan çekilir, örneğin Ulusal Okyanus ve Atmosfer İdaresi'ne (NOAA) ftp bağlantısı ile. Daha sonra, veriler yapılandırılmış bir biçimde ayrıştırılır ve ardından posta kodu veya en yakın hava istasyonunun konumu gibi özelliklerle uygun şekilde zenginleştirilmiştir.

Veri kümeleri, Azure'da bulut bilgi işlemle birlikte barındırılır ve erişim ve işleme kolaylaşır.  

Aşağıdaki veri kümeleri örnekleri kullanılabilir. 

### <a name="weather-data"></a>Hava durumu verileri
 
|Veri kümesi         | Notebooks     | Açıklama                                    |
|----------------|---------------|------------------------------------------------|
|[NOAA Entegre Yüzey Verileri (ISD)](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/?tab=data-access#AzureDatabricks) | Kuzey Amerika, Avrupa, Avustralya ve Asya'nın bazı bölgelerinde ki en iyi mekansal kapsama alanına sahip NOAA'dan dünya çapında saatlik hava durumu verileri. Günlük olarak güncellenir. |
|[NOAA Küresel Tahmin Sistemi (GFS)](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system/?tab=data-access#AzureDatabricks) | NOAA'dan 15 günlük ABD saatlik hava tahmini verileri. Günlük olarak güncellenir. |

### <a name="calendar-data"></a>Takvim verileri

|Veri kümesi         | Notebooks     | Açıklama                                    |
|----------------|---------------|------------------------------------------------|
|[Resmi Tatiller](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) | [Azure Notebooks](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/?tab=data-access#AzureNotebooks) <br> [Azure Databricks](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/?tab=data-access#AzureDatabricks) | 1970'ten 2099'a kadar 41 ülke veya bölgeyi kapsayan dünya çapındaki resmi tatil verileri. Ülke ve çoğu insan izin ödemiş olup olmadığını içerir. |

## <a name="access-to-datasets"></a>Veri kümelerine erişim  
Azure hesabıyla, açık veri kümelerini kod kullanarak veya Azure hizmet arabirimi üzerinden erişebilirsiniz. Veriler, makine öğrenimi çözümünüzde kullanılmak üzere Azure bulut bilgi işlem kaynaklarıyla birlikte konumlanır.  

Açık Veri kümeleri Azure Machine Learning UI ve SDK aracılığıyla kullanılabilir. Açık Veri Setleri ayrıca verileri Azure Machine Learning ve Azure Databricks'e bağlamak için kullanabileceğiniz Azure Dizüstü Bilgisayarlar ve Azure Databricks dizüstü bilgisayarları da sağlar. Veri kümelerine Python SDK üzerinden de erişilebilir. 

Ancak, Açık Veri Kümelerine erişmek için bir Azure hesabına ihtiyacınız yoktur; spark olsun veya olmasın herhangi bir Python ortamından erişebilirsiniz.

## <a name="request-or-contribute-datasets"></a>Veri kümelerini isteme veya katkıda bulunma

İstediğiniz verileri bulamıyorsanız, veri seti [istemek](mailto:aod@microsoft.com?Subject=Request%20dataset%3A%20%3Creplace%20with%20dataset%20name%3E&Body=%0AYour%20name%20and%20institution%3A%20%0A%0ADataset%20name%3A%0A%20%0ADataset%20description%3A%20%0A%3Cfill%20in%20a%20brief%20description%20and%20share%20any%20web%20links%20of%20the%20dataset%3E%20%0A%0ADataset%20size%3A%20%0A%3Chow%20much%20space%20does%20the%20dataset%20need%20today%20and%20how%20much%20is%20it%20expected%20to%20grow%20each%20year%3E%20%0A%0ADataset%20file%20formats%3A%20%0A%3Ccurrent%20dataset%20file%20formats%2C%20and%20optionally%2C%20any%20formats%20that%20the%20dataset%20must%20be%20transformed%20to%20for%20easy%20access%3E%0A%0ALicense%3A%20%0A%3Cwhat%20is%20the%20license%20or%20terms%20and%20conditions%20governing%20the%20distribution%20of%20this%20dataset%3E%0A%0AUse%20cases%3A%20%0A%3CExplain%20some%20common%20use%20of%20the%20dataset.%20E.g.%20weather%20dataset%20can%20be%20useful%20in%20demand%20forecasting%20and%20predictive%20maintenance%20scenarios%3E%20%0A%0AAny%20additional%20information%20you%20want%20us%20to%20know%3A%0A) veya [bir veri kümesine katkıda bulunmak](mailto:aod@microsoft.com?Subject=Contribute%20dataset%3A%20%3Creplace%20with%20dataset%20name%3E&Body=%0AYour%20name%20and%20institution%3A%20%0A%0ADataset%20name%3A%0A%20%0ADataset%20description%3A%20%0A%3Cfill%20in%20a%20brief%20description%20and%20share%20any%20web%20links%20of%20the%20dataset%3E%20%0A%0ADataset%20size%3A%20%0A%3Chow%20much%20space%20does%20the%20dataset%20need%20today%20and%20how%20much%20is%20it%20expected%20to%20grow%20each%20year%3E%20%0A%0ADataset%20file%20formats%3A%20%0A%3Ccurrent%20dataset%20file%20formats%2C%20and%20optionally%2C%20any%20formats%20that%20the%20dataset%20must%20be%20transformed%20to%20for%20easy%20access%3E%0A%0ALicense%3A%20%0A%3Cwhat%20is%20the%20license%20or%20terms%20and%20conditions%20governing%20the%20distribution%20of%20this%20dataset%3E%0A%0AUse%20cases%3A%20%0A%3CExplain%20some%20common%20use%20of%20the%20dataset.%20E.g.%20weather%20dataset%20can%20be%20useful%20in%20demand%20forecasting%20and%20predictive%20maintenance%20scenarios%3E%20%0A%0AAny%20additional%20information%20you%20want%20us%20to%20know%3A%0A)için bize e-posta gönder. 

## <a name="next-steps"></a>Sonraki adımlar
* [Örnek not defteri](samples.md)
* [Öğretici: NY taksi verileri ile regresyon modelleme](/azure/machine-learning/service/tutorial-auto-train-models?context=azure/open-datasets/context/open-datasets-context)
* [Açık Veri Kümeleri için Python SDK](/python/api/azureml-opendatasets/?view=azure-ml-py)
