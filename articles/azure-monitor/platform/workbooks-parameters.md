---
title: Azure Çalışma Kitaplarını parametreler oluşturma
description: Önceden oluşturulmuş ve özel parametreli çalışma kitaplarıyla karmaşık raporlamayı basitleştirin
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 3e7dda85f1f890d5ae0eb4722c3e028b373fdcab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658226"
---
# <a name="workbook-parameters"></a>Çalışma kitabı parametreleri

Parametreler, çalışma kitabı yazarlarının tüketicilerden girdi toplamasına ve çalışma kitabının diğer bölümlerinde referans almasına olanak sağlar – genellikle sonuç kümesini kapsamak veya doğru görseli ayarlamak için. Bu, yazarların etkileşimli raporlar ve deneyimler oluşturmasına olanak tanıyan önemli bir özelliktir. 

Çalışma kitapları, parametre denetimlerinizin tüketicilere nasıl sunulduğunu kontrol etmenizi sağlar – metin kutusu ve açılır, tek ve çok seçmeli, metinden gelen değerler, JSON, KQL veya Azure Kaynak Grafiği, vb.  

Desteklenen parametre türleri şunlardır:
* [Zaman](workbooks-time.md) - kullanıcının önceden doldurulmuş zaman aralıklarından seçim yapmasına veya özel bir aralık seçmesine olanak tanır
* [Açılır -](workbooks-dropdowns.md) kullanıcının bir değer veya değer kümesinden seçim yapmasına olanak tanır
* [Metin](workbooks-text.md) - kullanıcının rasgele metin girmesini sağlar
* [Kaynak](workbooks-resources.md) - bir kullanıcının bir veya daha fazla Azure kaynağı seçmesine olanak tanır
* [Abonelik](workbooks-resources.md) - bir kullanıcının bir veya daha fazla Azure abonelik kaynağı seçmesine olanak tanır
* Kaynak Türü - bir kullanıcının bir veya daha fazla Azure kaynak türü değeri seçmesine olanak tanır
* Konum - bir kullanıcının bir veya daha fazla Azure konum değeri seçmesine olanak tanır

Bu parametre değerleri, çalışma kitaplarının diğer bölümlerinde bağlamalar veya değer genişletmeleri yoluyla başvurulabilir.

## <a name="creating-a-parameter"></a>Parametre oluşturma
1. Edit modunda boş bir çalışma kitabıyla başlayın.
2. Çalışma kitabındaki bağlantılardan _parametre ekle'yi_ seçin.
3. Mavi _Parametre Ekle_ düğmesine tıklayın.
4. Açılan yeni parametre bölmesine girin:
    1. Parametre adı: `TimeRange` *(parametre __adlarının__ boşluk veya özel karakterler **içeremeyeceğini** unutmayın)*
    2. Görüntü adı: `Time Range` *(ancak, ekran __adları__ boşluklar, özel karakterler, emoji, vb içerebilir)*  
    2. Parametre türü:`Time range picker`
    3. Gerekli:`checked`
    4. Kullanılabilir zaman aralıkları: Son saat, Son 12 saat, Son 24 saat, Son 48 saat, Son 3 gün, Son 7 gün ve Özel zaman aralığı seçimine izin verin
5. Parametreyi oluşturmak için araç çubuğundan 'Kaydet'i seçin.

   ![Zaman aralığı parametresi oluşturmayı gösteren görüntü](./media/workbooks-parameters/time-settings.png)

Çalışma kitabı okuma modunda ,"Pills" stilinde böyle görünecektir.

   ![Okuma modunda bir zaman aralığı parametresi gösteren görüntü](./media/workbooks-parameters/parameters-time.png)

## <a name="referencing-a-parameter"></a>Parametreye başvurma
### <a name="via-bindings"></a>Bağlama yoluyla
1. Çalışma kitabına bir sorgu denetimi ekleyin ve bir Uygulama Öngörüleri kaynağını seçin.
2. Zaman _Aralığı_ açılır ve alttaki Parametreler bölümünden `Time Range` seçeneği seçin.
3. Bu, zaman aralığı parametresini grafiğin zaman aralığına bağlar. Örnek sorgunun zaman kapsamı artık Son 24 saattir.
4. Sonuçları görmek için sorguyu çalıştır

    ![Bağlamalar yoluyla başvurulan zaman aralığı parametresi gösteren resim](./media/workbooks-parameters/time-binding.png)

### <a name="in-kql"></a>KQL içinde
1. Çalışma kitabına bir sorgu denetimi ekleyin ve bir Uygulama Öngörüleri kaynağını seçin.
2. KQL'de, parametreyi kullanarak bir zaman kapsamı filtresi girin:`| where timestamp {TimeRange}`
3. Bu, sorgu değerlendirme süresini `| where timestamp > ago(1d)`parametrenin zaman aralığı değeri olana genişletir.
4. Sonuçları görmek için sorguyu çalıştır

    ![KQL'de başvurulan bir zaman aralığını gösteren görüntü](./media/workbooks-parameters/time-in-code.png)

### <a name="in-text"></a>Metin olarak 
1. Çalışma kitabına metin denetimi ekleyin.
2. İşaretle, girin`The chosen time range is {TimeRange:label}`
3. _Bitmiş Düzenleme'yi_ Seçin
4. Metin denetimi metni gösterir: _Seçilen zaman aralığı Son 24 saattir_

## <a name="parameter-options"></a>Parametre seçenekleri
_Metin'de_ `label` değeri yerine parametrenin kullanılır. Parametreler türüne bağlı olarak çeşitli bu tür seçenekleri ortaya çıkarır - örneğin zaman aralığı toplayıcıları değer, etiket, sorgu, başlangıç, bitiş ve gren sağlar.

Parametrenizin `Previews` genişletme seçeneklerini görmek için _Parametreyi Edit_ bölmesinin bölümünü kullanın:

![Zaman aralığı parametre seçeneklerini gösteren resim](./media/workbooks-parameters/time-previews.png)

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitapları hakkında daha fazla bilgi [edinmeye başlayın](workbooks-visualizations.md) birçok zengin görselleştirme seçeneği.
* Çalışma kitabı kaynaklarınıza erişimi [kontrol](workbooks-access-control.md) edin ve paylaşın.
