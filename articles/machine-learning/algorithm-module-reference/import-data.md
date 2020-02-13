---
title: 'İçeri aktarma verileri: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Mevcut bulut veri hizmetlerinden bir makine öğrenimi ardışık düzenine veri yüklemek için Azure Machine Learning veri alma modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: e7aa19c1d189eb19237ea85aae1ad2441d7e98b9
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77163203"
---
# <a name="import-data-module"></a>Veri modülünü içeri aktarma

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Mevcut bulut veri hizmetlerinden bir makine öğrenimi ardışık düzenine veri yüklemek için bu modülü kullanın. 

> [!Note]
> Bu modül tarafından sunulan tüm işlevler, çalışma alanı giriş sayfasında **veri deposu** ve **veri kümeleri** tarafından yapılabilir. Veri izleme gibi ek özellikler içeren veri **deposu** ve veri **kümesi** kullanmanızı öneririz. Daha fazla bilgi edinmek için bkz. [verilere erişme ve veri](../how-to-access-data.md) [kümelerini kaydetme](../how-to-create-register-datasets.md) makalesi.
> Bir veri kümesini kaydettikten sonra, tasarımcı arabirimindeki veri **kümeleri kategorim** -> **veri** kümelerinde bulabilirsiniz. Bu modül, tanıdık bir deneyim için Studio (klasik) kullanıcıları için ayrılmıştır. 
>

**Veri Içeri aktarma** modülü aşağıdaki kaynaklardan gelen verileri oku destekler:

- HTTP aracılığıyla URL
- Azure bulut, [**veri depoları**](../how-to-access-data.md)üzerinden depolar)
    - Azure Blob kapsayıcısı
    - Azure Dosya Paylaşımı
    - Azure Data Lake
    - Azure Data Lake Gen2
    - Azure SQL Veritabanı
    - Azure PostgreSQL    

Bulut depolama 'yı kullanmadan önce, Azure Machine Learning çalışma alanınıza önce bir veri deposu kaydetmeniz gerekir. Daha fazla bilgi için bkz. [verilere erişme](../how-to-access-data.md). 

İstediğiniz verileri tanımladıktan ve kaynağa bağlandıktan sonra, **[verileri Içeri aktarın](./import-data.md)** her bir sütunun veri türünü içerdiği değerlere göre alır ve verileri Tasarımcı işlem hattınızla yükler. **Içeri aktarma verilerinin** çıktısı, herhangi bir tasarımcı işlem hattı ile kullanılabilen bir veri kümesidir.

Kaynak verileriniz değişirse, [verileri Içeri aktar](./import-data.md)' ı yeniden çalıştırarak veri kümesini yenileyebilir ve yeni veri ekleyebilirsiniz.

## <a name="how-to-configure-import-data"></a>Içeri aktarma verilerini yapılandırma

1. **Veri alım** modülünü işlem hattınızı ekleyin. Bu modülü, tasarımcıda **veri girişi ve çıkış** kategorisinde bulabilirsiniz.

1. Sihirbaz kullanarak veri kaynağını yapılandırmak için **veri alma Sihirbazı 'Nı Başlat** ' a tıklayın.

    Sihirbaz hesap adını ve kimlik bilgilerini alır ve diğer seçenekleri yapılandırmanıza yardımcı olur. Var olan bir yapılandırmayı düzenliyorsanız, önce geçerli değerleri yükler.

1. **Veri kaynağı**' nı seçin ve veri kaynağı türünü seçin. HTTP veya veri deposu olabilir.

    Veri deposu ' nu seçerseniz, Azure Machine Learning çalışma alanınıza zaten kayıtlı olan mevcut veri depolarını seçebilirsiniz veya yeni bir veri deposu oluşturabilirsiniz. Ardından veri deposuna aktarılacak verilerin yolunu tanımlayın. **Yolu ![içeri** aktarma-veri yolu](media/module/import-data-path.png) ' ne tıklayarak yola kolayca gözatamazsınız

1. Dahil etmek istediğiniz sütunları filtrelemek için Önizleme şemasını seçin. Ayrıca, ayrıştırma seçeneklerinde sınırlayıcı gibi gelişmiş ayarları tanımlayabilirsiniz.

    ![İçeri aktarma-veri önizleme](media/module/import-data.png)



1. İşlem hattını çalıştırma.

    Veri Içeri aktarma verileri tasarımcıya yüklerken, her bir sütunun veri türünü, sayısal veya kategorik, içerdiği değerlere göre alır.

    Bir üst bilgi varsa, çıkış veri kümesinin sütunlarını adlandırmak için üst bilgi kullanılır.

    Verilerde mevcut sütun başlıkları yoksa, yeni sütun adları Sütun1, col2 biçimi kullanılarak üretilir,... , Coln *.

## <a name="results"></a>Sonuçlar

İçeri aktarma tamamlandığında, çıktı veri kümesine tıklayın ve verilerin başarıyla içeri aktarılmadığını görmek için **Görselleştir** ' i seçin.

İşlem hattı her çalıştırıldığında verilerin yeni bir kümesini içeri aktarmak yerine yeniden kullanım için kaydetmek istiyorsanız, modülün sağ panelindeki **çıktılar** sekmesinin altında bulunan veri **kümesini kaydet** simgesini seçin. Veri kümesi için bir ad seçin. Kaydedilen veri kümesi, kaydetme sırasında verileri korur, işlem hattının veri kümesi değişse bile, işlem hattı yeniden çalıştırıldığında veri kümesi güncellenmez. Bu, verilerin anlık görüntülerini almak için yararlı olabilir.

Verileri içeri aktardıktan sonra, modelleme ve analiz için bazı ek hazırlıklar gerekebilir:

- Sütun adlarını değiştirmek, bir sütunu farklı bir veri türü olarak işlemek veya bazı sütunların Etiketler ya da Özellikler olduğunu göstermek için [meta verileri Düzenle](./edit-metadata.md) ' yi kullanın.

- Modelde dönüştürülecek veya kullanılacak sütunların bir alt kümesini seçmek için [veri kümesindeki sütunları seç '](./select-columns-in-dataset.md) i kullanın. Dönüştürülen veya kaldırılan sütunlar, [sütun Ekle](./add-columns.md) modülü kullanılarak, özgün veri kümesine kolayca yeniden katılabilir.  

- Veri kümesini bölmek, örnekleme gerçekleştirmek veya ilk n satırı almak için [bölüm ve örnek](./partition-and-sample.md) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
