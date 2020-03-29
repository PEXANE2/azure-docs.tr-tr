---
title: Veri içeri aktarma
titleSuffix: Azure Machine Learning
description: Verilerinizi çeşitli veri kaynaklarından Azure Machine Learning tasarımcısına nasıl aktarılacınız öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: peterclu
ms.author: peterlu
ms.date: 01/16/2020
ms.openlocfilehash: bd1c2ca182ae8be8425246f691dca805bf38637b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064028"
---
# <a name="import-your-data-into-azure-machine-learning-designer-preview"></a>Verilerinizi Azure Machine Learning tasarımcısına aktarın (önizleme)

Bu makalede, özel çözümler oluşturmak için tasarımcıda kendi verilerinizi nasıl içe aktarabileceğinizi öğrenirsiniz. Tasarımcıya veri aktarabilirsiniz iki yolu vardır: 

* **Azure Machine Learning veri kümeleri** - Verilerinizi yönetmenize yardımcı olan gelişmiş özellikleri etkinleştirmek için Azure Machine Learning'de [veri kümelerini](concept-data.md#datasets) kaydedin.
* **Veri Aktarma modülü** - Çevrimiçi veri kaynaklarından doğrudan verilere erişmek için [Veri Alma](algorithm-module-reference/import-data.md) modüllerini kullanın.

## <a name="use-azure-machine-learning-datasets"></a>Azure Machine Learning veri kümelerini kullanma

Verileri tasarımcıya almak için [veri kümelerini](concept-data.md#datasets) kullanmanızı öneririz. Bir veri kümesini kaydettirdiğinizde, [sürüm leme, izleme ve](how-to-version-track-datasets.md) veri [izleme](how-to-monitor-datasets.md)gibi gelişmiş veri özelliklerinden tam olarak yararlanabilirsiniz.

### <a name="register-a-dataset"></a>Bir veri kümesi kaydetme

Varolan veri kümelerini [Programlı olarak SDK'ya](how-to-create-register-datasets.md#use-the-sdk) veya [Azure Machine Learning stüdyosuna](how-to-create-register-datasets.md#use-the-ui)kaydedebilirsiniz.

Ayrıca, herhangi bir tasarımcı modülü için çıktıyı veri kümesi olarak kaydedebilirsiniz.

1. Kaydetmek istediğiniz verileri oluşturan modülü seçin.

1. Özellikler bölmesinde, **Çıktılar** > **Kayıt veri kümesini**seçin.

    ![Register Dataset seçeneğine nasıl gidilir gösteren ekran görüntüsü](media/how-to-designer-import-data/register-dataset-designer.png)

### <a name="use-a-dataset"></a>Veri kümesi kullanma

Kayıtlı veri kümeleriniz, **Datasets** > **My Datasets**altında modül paletinde bulunabilir. Bir veri kümesi ni kullanmak için sürükleyin ve boru hattı tuvaline bırakın. Ardından, veri kümesinin çıkış bağlantı noktasını paletteki diğer modüllere bağlayın.

![Tasarımcı paletinde kaydedilen veri kümelerinin konumunu gösteren ekran görüntüsü](media/how-to-designer-import-data/use-datasets-designer.png)


> [!NOTE]
> Tasarımcı şu anda yalnızca [tabular veri kümelerini](how-to-create-register-datasets.md#dataset-types)işlemeyi destekler. [Dosya veri kümelerini](how-to-create-register-datasets.md#dataset-types)kullanmak istiyorsanız, Python ve R için kullanılabilen Azure Machine Learning SDK'yı kullanın.

## <a name="import-data-using-the-import-data-module"></a>İçe Alma Verisi modüllerini kullanarak veri alma

Veri almak için veri kümelerini kullanmanızı öneririz, ancak [Veri Alma](algorithm-module-reference/import-data.md) modüllerini de kullanabilirsiniz. Veri Alma modülü, Azure Machine Learning'de veri kümenizi kaydetmeyi atlar ve verileri doğrudan bir [veri deposundan](concept-data.md#datastores) veya HTTP URL'sinden içeri aktarıyor.

Alma Verileri modülünün nasıl kullanılacağı hakkında ayrıntılı bilgi [için, İçe İşlem Verileri başvuru sayfasına](algorithm-module-reference/import-data.md)bakın.

> [!NOTE]
> Veri kümenizde çok fazla sütun varsa, aşağıdaki hatayla karşılaşabilirsiniz: "Doğrulama boyut sınırlaması nedeniyle başarısız oldu". Bunu önlemek için [veri kümesini Veri Kümeleri arabirimine kaydedin.](how-to-create-register-datasets.md#use-the-ui)

## <a name="supported-sources"></a>Desteklenen kaynaklar

Bu bölümde tasarımcı tarafından desteklenen veri kaynakları listelenir. Veriler tasarımcıya bir veri deposundan veya [tabular veri kümesinden](how-to-create-register-datasets.md#dataset-types)gelir.

### <a name="datastore-sources"></a>Datastore kaynakları
Desteklenen veri deposu kaynaklarının listesi için Azure [depolama hizmetlerinde Access verilerine](how-to-access-data.md#supported-data-storage-service-types)bakın.

### <a name="tabular-dataset-sources"></a>Tabular veri seti kaynakları

Tasarımcı aşağıdaki kaynaklardan oluşturulan tabular veri kümelerini destekler:
 * Sınırlı dosyalar
 * JSON dosyaları
 * Parquet dosyaları
 * SQL sorguları

## <a name="data-types"></a>Veri türleri

Tasarımcı dahili olarak aşağıdaki veri türlerini tanır:

* Dize
* Tamsayı
* Ondalık
* Boole
* Tarih

Tasarımcı, modüller arasında veri aktarmak için bir iç veri türü kullanır. [Dataset'e Dönüştür](algorithm-module-reference/convert-to-dataset.md) modüllerini kullanarak verilerinizi açıkça veri tablosu biçimine dönüştürebilirsiniz. İç biçim dışındaki biçimleri kabul eden herhangi bir modül, verileri bir sonraki modüle geçirmeden önce sessizce dönüştürür.

## <a name="data-constraints"></a>Veri kısıtlamaları

Tasarımcıdaki modüller bilgi işlem hedefinin boyutuyla sınırlıdır. Daha büyük veri kümeleri için daha büyük bir Azure Machine Learning bilgi işlem kaynağı kullanmanız gerekir. Azure Machine Learning bilgi işlem hakkında daha fazla bilgi için Azure [Machine Learning'de bilgi işlem hedefleri nelerdir?](concept-compute-target.md#azure-machine-learning-compute-managed)

## <a name="next-steps"></a>Sonraki adımlar

Tutorial ile tasarımcının temellerini [öğrenin: Tasarımcı ile otomobil fiyatını tahmin edin.](tutorial-designer-automobile-price-train-score.md)
