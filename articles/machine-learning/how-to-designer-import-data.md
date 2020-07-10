---
title: Verileri tasarımcıya aktarma (Önizleme)
titleSuffix: Azure Machine Learning
description: Çeşitli veri kaynaklarından verileri Azure Machine Learning tasarımcı 'ya (Önizleme) aktarmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: peterclu
ms.author: peterlu
ms.date: 01/16/2020
ms.custom: designer
ms.openlocfilehash: 7a204310454cb636e26e6366d4ff7375f4faea8d
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86207521"
---
# <a name="import-data-into-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısına veri aktarma (Önizleme)

Bu makalede, özel çözümler oluşturmak için kendi verilerinizi tasarımcıda nasıl içeri aktaracağınızı öğreneceksiniz. Verileri tasarımcıya içeri aktarabilmeniz için kullanabileceğiniz iki yol vardır: 

* **Azure Machine Learning veri kümeleri** -verilerinizi yönetmenize yardımcı olan gelişmiş özellikleri etkinleştirmek için Azure Machine Learning veri [kümelerini](concept-data.md#datasets) kaydettirin.
* **Veri modülünü Içeri aktarma** -çevrimiçi veri kaynaklarında verilere doğrudan erişmek Için [verileri içeri aktarma](algorithm-module-reference/import-data.md) modülünü kullanın.

## <a name="use-azure-machine-learning-datasets"></a>Azure Machine Learning veri kümelerini kullanma

Verileri tasarımcıya aktarmak için veri [kümelerini](concept-data.md#datasets) kullanmanızı öneririz. Bir veri kümesini kaydettiğinizde, [sürüm oluşturma ve izleme](how-to-version-track-datasets.md) ve [veri izleme](how-to-monitor-datasets.md)gibi gelişmiş veri özelliklerinden tam olarak yararlanabilirsiniz.

### <a name="register-a-dataset"></a>Veri kümesini kaydetme

Var olan veri kümelerini [SDK ile program aracılığıyla](how-to-create-register-datasets.md#use-the-sdk) veya [Azure Machine Learning Studio 'da görsel olarak](how-to-create-register-datasets.md#use-the-ui)kaydedebilirsiniz.

Ayrıca, herhangi bir tasarımcı modülünün çıkışını bir veri kümesi olarak kaydedebilirsiniz.

1. Kaydetmek istediğiniz verilerin çıkışı olan modülü seçin.

1. Özellikler bölmesinde, **çıktılar**  >  **kayıt veri kümesi**' ni seçin.

    ![Register DataSet seçeneğine nasıl gidebileceğiniz gösteren ekran görüntüsü](media/how-to-designer-import-data/register-dataset-designer.png)

### <a name="use-a-dataset"></a>Veri kümesi kullanma

Kayıtlı veri kümeleriniz **, veri kümeleri veri kümeleri altında bulunan**modül paletinde bulunabilir  >  **My Datasets**. Bir veri kümesini kullanmak için sürükleyin ve ardışık düzen tuvaline bırakın. Ardından, veri kümesinin çıkış bağlantı noktasını paletteki diğer modüllere bağlayın.

![Tasarımcı paletindeki kaydedilen veri kümelerinin konumunu gösteren ekran görüntüsü](media/how-to-designer-import-data/use-datasets-designer.png)


> [!NOTE]
> Tasarımcı şu anda yalnızca [tablolu veri kümelerini](how-to-create-register-datasets.md#dataset-types)işlemeyi destekliyor. [Dosya veri kümelerini](how-to-create-register-datasets.md#dataset-types)kullanmak istiyorsanız Python ve R için kullanılabilen Azure Machine Learning SDK 'sını kullanın.

## <a name="import-data-using-the-import-data-module"></a>Verileri Içeri aktarma modülünü kullanarak içeri aktarma

Verileri içeri aktarmak için veri kümelerini kullanmanızı öneririz, ayrıca [verileri Içeri aktarma](algorithm-module-reference/import-data.md) modülünü de kullanabilirsiniz. Veri Içeri aktarma modülü, veri kümenizin Azure Machine Learning kaydedilmesini atlar ve verileri doğrudan bir [veri deposundan](concept-data.md#datastores) veya http url 'sinden içeri aktarır.

Veri alma modülünü kullanma hakkında ayrıntılı bilgi için bkz. [veri başvurusunu Içeri aktarma sayfası](algorithm-module-reference/import-data.md).

> [!NOTE]
> Veri kümenizin çok fazla sütunu varsa, aşağıdaki hatayla karşılaşabilirsiniz: "boyut sınırlaması nedeniyle doğrulama başarısız oldu". Bunu önlemek için [veri kümesini veri kümeleri arabirimine kaydedin](how-to-create-register-datasets.md#use-the-ui).

## <a name="supported-sources"></a>Desteklenen kaynaklar

Bu bölüm, tasarımcı tarafından desteklenen veri kaynaklarını listeler. Veriler bir veri deposundan veya [tablo veri kümesinden](how-to-create-register-datasets.md#dataset-types)tasarımcıya gelir.

### <a name="datastore-sources"></a>Veri deposu kaynakları
Desteklenen veri deposu kaynaklarının listesi için bkz. [Azure Storage hizmetlerindeki verilere erişme](how-to-access-data.md#supported-data-storage-service-types).

### <a name="tabular-dataset-sources"></a>Tablo veri kümesi kaynakları

Tasarımcı, aşağıdaki kaynaklardan oluşturulan tablo veri kümelerini destekler:
 * Sınırlandırılmış dosyalar
 * JSON dosyaları
 * Parquet dosyaları
 * SQL sorguları

## <a name="data-types"></a>Veri türleri

Tasarımcı aşağıdaki veri türlerini dahili olarak tanır:

* Dize
* Tamsayı
* Ondalık
* Boole
* Tarih

Tasarımcı, modüller arasında veri geçirmek için bir iç veri türü kullanır. Veri [kümesine Dönüştür](algorithm-module-reference/convert-to-dataset.md) modülünü kullanarak verilerinizi veri tablosu biçimine açıkça dönüştürebilirsiniz. İç biçim dışındaki biçimleri kabul eden herhangi bir modül, verileri bir sonraki modüle geçirmeden önce sessizce dönüştürür.

## <a name="data-constraints"></a>Veri kısıtlamaları

Tasarlayıcıdaki modüller, işlem hedefinin boyutuyla sınırlıdır. Daha büyük veri kümelerinde daha büyük bir Azure Machine Learning işlem kaynağı kullanmanız gerekir. Azure Machine Learning işlem hakkında daha fazla bilgi için bkz. [Azure Machine Learning işlem hedefleri nelerdir?](concept-compute-target.md#azure-machine-learning-compute-managed)

## <a name="access-data-in-a-virtual-network"></a>Bir sanal ağdaki verilere erişme

Çalışma alanınız bir sanal ağda ise, tasarımcıda verileri görselleştirmek için ek yapılandırma adımları gerçekleştirmeniz gerekir. Bir sanal ağda veri depoları ve veri kümelerinin nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [özel sanal ağlarla eğitim sırasında ağ yalıtımı &](how-to-enable-virtual-network.md#machine-learning-studio).

## <a name="next-steps"></a>Sonraki adımlar

Öğreticiyle tasarımcı 'nın temellerini öğrenin [: tasarımcı ile otomobil fiyatını tahmin](tutorial-designer-automobile-price-train-score.md)edin.
