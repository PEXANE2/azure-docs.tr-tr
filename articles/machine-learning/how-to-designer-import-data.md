---
title: Veri içeri aktarma
titleSuffix: Azure Machine Learning
description: Verilerinizi çeşitli veri kaynaklarından Azure Machine Learning tasarımcı 'ya aktarmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: peterclu
ms.author: peterlu
ms.date: 01/16/2020
ms.openlocfilehash: 8cd49f9714746578ec701e22f9e6b0ccce772c6b
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78942299"
---
# <a name="import-your-data-into-azure-machine-learning-designer-preview"></a>Verilerinizi Azure Machine Learning tasarımcı 'ya aktarma (Önizleme)

Bu makalede, özel çözümler oluşturmak için kendi verilerinizi tasarımcıda nasıl içeri aktaracağınızı öğreneceksiniz. Verileri tasarımcıya içeri aktarabilmeniz için kullanabileceğiniz iki yol vardır: 

* **Azure Machine Learning veri kümeleri** -verilerinizi yönetmenize yardımcı olan gelişmiş özellikleri etkinleştirmek için Azure Machine Learning veri [kümelerini](concept-data.md#datasets) kaydettirin.
* **Veri modülünü Içeri aktarma** -çevrimiçi veri kaynaklarında verilere doğrudan erişmek Için [verileri içeri aktarma](algorithm-module-reference/import-data.md) modülünü kullanın.

## <a name="use-azure-machine-learning-datasets"></a>Azure Machine Learning veri kümelerini kullanma

Verileri tasarımcıya aktarmak için veri [kümelerini](concept-data.md#datasets) kullanmanızı öneririz. Bir veri kümesini kaydettiğinizde, [sürüm oluşturma ve izleme](how-to-version-track-datasets.md) ve [veri izleme](how-to-monitor-datasets.md)gibi gelişmiş veri özelliklerinden tam olarak yararlanabilirsiniz.

### <a name="register-a-dataset"></a>Veri kümesini kaydetme

Var olan veri kümelerini [SDK ile program aracılığıyla](how-to-create-register-datasets.md#use-the-sdk) veya [Azure Machine Learning Studio 'da görsel olarak](how-to-create-register-datasets.md#use-the-ui)kaydedebilirsiniz.

Ayrıca, herhangi bir tasarımcı modülünün çıkışını bir veri kümesi olarak kaydedebilirsiniz.

1. Kaydetmek istediğiniz verilerin çıkışı olan modülü seçin.

1. Özellikler bölmesinde, **çıktılar** > **veri kümesini kaydet**' i seçin.

    ![Register DataSet seçeneğine nasıl gidebileceğiniz gösteren ekran görüntüsü](media/how-to-designer-import-data/register-dataset-designer.png)

### <a name="use-a-dataset"></a>Veri kümesi kullanma

Kayıtlı veri kümeleriniz, veri **kümelerim** > **veri** kümeleri altında bulunan modül paletinde bulunabilir. Bir veri kümesini kullanmak için sürükleyin ve ardışık düzen tuvaline bırakın. Ardından, veri kümesinin çıkış bağlantı noktasını paletteki diğer modüllere bağlayın.

![Tasarımcı paletindeki kaydedilen veri kümelerinin konumunu gösteren ekran görüntüsü](media/how-to-designer-import-data/use-datasets-designer.png)



> [!NOTE]
> Tasarımcı şu anda yalnızca [tablolu veri kümelerini](how-to-create-register-datasets.md#dataset-types)işlemeyi destekliyor. [Dosya veri kümelerini](how-to-create-register-datasets.md#dataset-types)kullanmak istiyorsanız Python ve R için kullanılabilen Azure Machine Learning SDK 'sını kullanın.

## <a name="import-data-using-the-import-data-module"></a>Verileri Içeri aktarma modülünü kullanarak içeri aktarma

Verileri içeri aktarmak için veri kümelerini kullanmanızı öneririz, ayrıca [verileri Içeri aktarma](algorithm-module-reference/import-data.md) modülünü de kullanabilirsiniz. Veri Içeri aktarma modülü, veri kümenizin Azure Machine Learning kaydedilmesini atlar ve verileri doğrudan bir [veri deposundan](concept-data.md#datastores) veya http url 'sinden içeri aktarır.

Veri alma modülünü kullanma hakkında ayrıntılı bilgi için bkz. [veri başvurusunu Içeri aktarma sayfası](algorithm-module-reference/import-data.md).

[!NOTE]
> Veride çok sayıda sütun varsa, verileri Içeri aktarma modülünde "boyut sınırlaması nedeniyle doğrulama başarısız oldu" ile karşılaşabilirsiniz. Bunun nedeni, sütununun kodlama sonrasında maxmıum modülü parametre uzunluğunu dışladığı için. Bu durumda, [veri kümesini veri kümeleri Kullanıcı arabiriminde kaydetmenizi](how-to-create-register-datasets.md#use-the-ui)öneririz, bu da hatayı ortadan kaldırabilirsiniz.  

## <a name="supported-sources"></a>Desteklenen kaynaklar

Bu bölüm, tasarımcı tarafından desteklenen veri kaynaklarını listeler. Veriler bir veri deposundan veya [tablo veri kümesinden](how-to-create-register-datasets.md#dataset-types)tasarımcıya girer.

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

## <a name="next-steps"></a>Sonraki adımlar

Öğreticiyle tasarımcı 'nın temellerini öğrenin [: tasarımcı ile otomobil fiyatını tahmin](tutorial-designer-automobile-price-train-score.md)edin.
