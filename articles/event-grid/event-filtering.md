---
title: Azure Olay Ağıtı için olay filtreleme
description: Azure Olay Ağı aboneliği oluştururken olayları nasıl filtreleyen izninizden önce açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: spelluru
ms.openlocfilehash: ce1bb3760ae73a9eaeee3cde957cc94841ebdf29
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81731949"
---
# <a name="understand-event-filtering-for-event-grid-subscriptions"></a>Olay Ağı abonelikleri için olay filtrelemeanlama

Bu makalede, bitiş noktanıza hangi olayların gönderildiğini filtrelemenin farklı yolları açıklanmaktadır. Bir olay aboneliği oluştururken, filtreleme için üç seçeneğiniz var:

* Olay türleri
* Özne ile başlar veya biter
* Gelişmiş alanlar ve operatörler

## <a name="event-type-filtering"></a>Olay türü filtreleme

Varsayılan olarak, olay kaynağı için tüm [olay türleri](event-schema.md) bitiş noktasına gönderilir. Yalnızca belirli olay türlerini bitiş noktanıza göndermeye karar verebilirsiniz. Örneğin, kaynaklarınızdaki güncelleştirmelerden haberdar edilebilir, ancak silme gibi diğer işlemler için bilgilendirilmezsiniz. Bu durumda, olay `Microsoft.Resources.ResourceWriteSuccess` türüne göre filtre uygulayın. Olay türlerini içeren bir dizi `All` sağlayın veya olay kaynağı için tüm olay türlerini almak için belirtin.

Olay türüne göre filtreleme için JSON sözdizimi:

```json
"filter": {
  "includedEventTypes": [
    "Microsoft.Resources.ResourceWriteFailure",
    "Microsoft.Resources.ResourceWriteSuccess"
  ]
}
```

## <a name="subject-filtering"></a>Konu filtreleme

Konuya göre basit filtreleme için, özne için bir başlangıç veya bitiş değeri belirtin. Örneğin, yalnızca bir metin dosyasını depolama hesabına yüklemeyle ilgili olayları almak için konunun sona ermesini `.txt` belirtebilirsiniz. Veya, bu kapsayıcı `/blobServices/default/containers/testcontainer` için tüm olayları almak için başlar ama depolama hesabında diğer kapsayıcılar değil, konu filtreleyebilirsiniz.

Etkinlikleri özel konulara yayınlarken, etkinliklerinizin etkinlikleriiçin, abonelerin etkinlikle ilgilenip ilgilenmediklerini bilmelerini kolaylaştıran konular oluşturun. Aboneler olayları filtrelemek ve yönlendirmek için konu özelliğini kullanır. Abonelerin bu yolun bölümlerine göre filtre uygulayabilmesi için, olayın gerçekleştiği yer için yolu eklemeyi düşünün. Yol, abonelerin olayları dar veya genel olarak filtrelemesini sağlar. Konuyla ilgili gibi `/A/B/C` üç segmentli bir yol sağlarsanız, `/A` aboneler geniş bir olay kümesi elde etmek için ilk segmente göre filtre uygulayabilir. Bu aboneler gibi `/A/B/C` konular `/A/D/E`ile olaylar olsun ya da . Diğer aboneler daha `/A/B` dar bir olay kümesi almak için filtre uygulayabilir.

Konuya göre filtreleme için JSON sözdizimi:

```json
"filter": {
  "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
  "subjectEndsWith": ".jpg"
}

```

## <a name="advanced-filtering"></a>Gelişmiş filtreleme

Veri alanlarındaki değerlere göre filtre uygulayıp karşılaştırma işleci belirtmek için gelişmiş filtreleme seçeneğini kullanın. Gelişmiş filtrelemede şunları belirtirsiniz:

* işleç türü - Karşılaştırma türü.
* anahtar - Filtreleme için kullandığınız olay verilerindeki alan. Bir sayı, boolean veya dize olabilir.
* değerleri - Anahtarla karşılaştırılacak değer veya değerler.

Birden çok değere sahip tek bir filtre belirtirseniz, bir **OR** işlemi gerçekleştirilir, bu nedenle anahtar alanın değeri bu değerlerden biri olmalıdır. Örnek aşağıda verilmiştir:

```json
"advancedFilters": [
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/microsoft.devtestlab/",
            "/providers/Microsoft.Compute/virtualMachines/"
        ]
    }
]
```

Birden çok farklı filtre belirtirseniz, bir **AND** işlemi gerçekleştirilir, bu nedenle her filtre koşulu nun karşılanması gerekir. Örnek aşağıda verilmiştir: 

```json
"advancedFilters": [
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/microsoft.devtestlab/"
        ]
    },
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/Microsoft.Compute/virtualMachines/"
        ]
    }
]
```

### <a name="operator"></a>İşleç

Sayılar için kullanılabilir işleçler şunlardır:

* NumberGreaterthan
* NumberGreaterThanOrEquals
* Sayısız
* NumberLessThanOrEquals
* NumberIn
* NumberNotin

Booleans için kullanılabilir operatör: BoolEquals

Dizeleri için kullanılabilir işleçleri şunlardır:

* StringContains
* StringBeginsWith
* StringEndsWith
* Stringin
* StringNotin

Tüm dize karşılaştırmaları büyük/küçük harf duyarsızdır.

### <a name="key"></a>Anahtar

Olay Izgara şemasındaki olaylar için anahtar için aşağıdaki değerleri kullanın:

* Kimlik
* Konu başlığı
* Özne
* Olay türü
* DataVersion
* Olay verileri (Data.key1 gibi)

Bulut Olayları şemasındaki olaylar için anahtar için aşağıdaki değerleri kullanın:

* Eventıd
* Kaynak
* Olay türü
* EventTypeVersion
* Olay verileri (Data.key1 gibi)

Özel giriş şeması için olay veri alanlarını (Data.key1 gibi) kullanın.

### <a name="values"></a>Değerler

Değerler şu olabilir:

* number
* string
* boole
* array

### <a name="limitations"></a>Sınırlamalar

Gelişmiş filtreleme aşağıdaki sınırlamaları vardır:

* Olay ızgarası aboneliği başına beş gelişmiş filtre
* Dize değeri başına 512 karakter
* Operatörlerde **değil, içinde** beş değer **not in**

Aynı anahtar birden fazla filtrede kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell ve Azure CLI ile olayları filtreleme hakkında bilgi edinmek [için Olay Ağıtı için Filtre olaylarını](how-to-filter-events.md)görün.
* Olay Ağıt'ı kullanmaya hızla başlamak için [Azure Olay Ağıtı ile özel etkinlikler oluştur ve yönlendir'e](custom-event-quickstart.md)bakın.
