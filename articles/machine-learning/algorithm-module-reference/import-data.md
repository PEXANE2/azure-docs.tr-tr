---
title: 'İthalat Verileri: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Mevcut bulut veri hizmetlerinden bir makine öğrenimi boru hattına veri yüklemek için Azure Machine Learning'deki Veri Alma modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: d124fdc15bd34743b237985a66cc35625f5d9a4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456208"
---
# <a name="import-data-module"></a>İthalat Veri modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Mevcut bulut veri hizmetlerinden bir makine öğrenme ardışık hattına veri yüklemek için bu modülü kullanın. 

> [!Note]
> Bu modül tarafından sağlanan tüm işlevler, iş sapı açılış **sayfasındaki datastore** ve **veri kümeleri** tarafından yapılabilir. Veri izleme gibi ek özellikler içeren **veri depolama** ve **veri kümesini** kullanmanızı öneririz. Daha fazla bilgi için [veriye nasıl erişilir](../how-to-access-data.md) ve Datasets makalesini [nasıl kaydedebilirsiniz makalesine](../how-to-create-register-datasets.md) bakın.
> Bir veri kümesini kaydettikten sonra, tasarımcı arabirimindeki **Veri Kümeleri** -> **My Datasets** kategorisinde bulabilirsiniz. Bu modül tanıdık bir deneyim için Studio (klasik) kullanıcıları için ayrılmıştır. 
>

**Veri Alma** modülü desteği aşağıdaki kaynaklardan alınan verileri okuyun:

- HTTP üzerinden URL
- [**Datastores**](../how-to-access-data.md)aracılığıyla Azure bulut depolamaları)
    - Azure Blob Konteyner
    - Azure Dosya Paylaşımı
    - Azure Data Lake
    - Azure Veri Gölü Gen2
    - Azure SQL Veritabanı
    - Azure PostgreSQL    

Bulut depolamayı kullanmadan önce Azure Machine Learning çalışma alanınızda bir veri deposu kaydetmeniz gerekir. Daha fazla bilgi için [verilere nasıl erişilir'](../how-to-access-data.md)e bakın. 

İstediğiniz verileri tanımladıktan ve kaynağa bağlandıktan sonra, **[İçe Aktar Verileri](./import-data.md)** içerdiği değerleri temel alan her sütunun veri türünü çıkartır ve verileri tasarımcı ardınıza yükler. **Alma Verileri** çıktısı, herhangi bir tasarımcı ardışık alanıyla kullanılabilen bir veri kümesidir.

Kaynak verileriniz değişirse, Veri Kümesini yenileyebilir ve [Veri Aktar'ı](./import-data.md)yeniden çalıştırarak yeni veriler ekleyebilirsiniz.

## <a name="how-to-configure-import-data"></a>Alma Verileri nasıl yapılandırılır?

1. Veri **Aktar'ı** boru hattınıza ekleyin. Bu modülü tasarımcıda **Veri Girişi ve Çıktısı** kategorisinde bulabilirsiniz.

1. Sağ bölmeyi açmak için modülü seçin.

1. **Veri kaynağını**seçin ve veri kaynağı türünü seçin. Bu HTTP veya datastore olabilir.

    Datastore'u seçerseniz, Azure Machine Learning çalışma alanınıza zaten kaydedilmiş varolan veri depolarını seçebilir veya yeni bir veri deposu oluşturabilirsiniz. Ardından veri deposunda aktak etmek için veri yolunu tanımlayın. **Gözat Yolu'nu** ![tıklatarak yola kolayca göz atabilirsiniz.](media/module/import-data-path.png)

1. Eklemek istediğiniz sütunları filtrelemek için önizleme şemasını seçin. Ayrışma seçeneklerinde Delimiter gibi gelişmiş ayarları da tanımlayabilirsiniz.

    ![alma-veri önizleme](media/module/import-data.png)

1. Onay kutusu, **Çıktıyı Yeniden Oluşturma**, çalışma zamanında çıktıyı yeniden oluşturmak için modülü n için çalıştırıp çalıştırmayacağına karar verir. 

    Varsayılan olarak seçili değil, yani modül daha önce aynı parametrelerle yürütülmüşse, sistem çalışma süresini azaltmak için son çalıştırmadaki çıktıyı yeniden kullanır. 

    Seçilirse, sistem çıkışı yeniden oluşturmak için modülü yeniden çalıştıracaktır. Bu nedenle, depolama alanında temel veriler güncelleştirildiğinde bu seçeneği belirleyin, en son verileri almaya yardımcı olabilir.


1. Boru hattını gönderin.

    Veri Alma verileri tasarımcıya yüklendiğinde, her sütunun veri türünü içerdiği değerlere göre çıkartır, sayısal veya kategorik.

    Üstbilgi varsa, çıktı veri kümesinin sütunlarını adlandırmak için üstbilgi kullanılır.

    Verilerde varolan sütun üstbilgileri yoksa, col1, col2 biçimi kullanılarak yeni sütun adları oluşturulur,... , coln*.

## <a name="results"></a>Sonuçlar

Alma işlemi tamamlandığında, çıktı veri kümesini tıklatın ve verilerin başarılı bir şekilde içeri mi alındığını görmek için **Visualize'ı** seçin.

Verileri yeniden kullanmak için kaydetmek istiyorsanız, ardışık hatlar her çalıştırılınca yeni bir veri kümesi almak yerine, modülün sağ panelindeki **Çıktılar** sekmesinin altındaki **Kayıt veri kümesi** simgesini seçin. Veri kümesi için bir ad seçin. Kaydedilen veri kümesi, tasarruf sırasında verileri korur, veri hattı ndaki veri kümesi değişse bile, ardışık iş aktarım yeniden çalıştırıldığında veri kümesi güncelleştirilemiyor. Bu, verilerin anlık görüntülerini almak için yararlı olabilir.

Verileri aldıktan sonra, modelleme ve analiz için bazı ek hazırlıklar gerekebilir:

- Sütun adlarını değiştirmek, bir sütunu farklı bir veri türü olarak işlemek veya bazı sütunların etiket veya özellik olduğunu belirtmek için [Meta Verileri Edit'i](./edit-metadata.md) kullanın.

- Dönüştürme veya modellemede kullanmak üzere sütun alt kümesini seçmek için [Veri Kümesi'nde Sütunları Seç'i](./select-columns-in-dataset.md) kullanın. Dönüştürülen veya kaldırılan sütunlar, [Sütun Ekle](./add-columns.md) modülü kullanılarak kolayca orijinal veri kümesine yeniden katılabilir.  

- Veri kümesini bölmek, örnekleme gerçekleştirmek veya üst n satırlarını almak için [Bölüm ve Örnek'i](./partition-and-sample.md) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
