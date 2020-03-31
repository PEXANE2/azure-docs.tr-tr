---
title: include dosyası
description: include dosyası
services: data-factory
author: memildin
ms.service: data-factory
ms.topic: include
ms.date: 03/17/2020
ms.author: memildin
ms.custom: include file
ms.openlocfilehash: 7d81799f7fbdb2b41db421daa1a85ec8cde511eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79538837"
---
### <a name="the-data-model-of-the-schema"></a>Şema veri modeli

|Alan|Açıklama|
|----|----|
|**AlertName**|Uyarı ekran adı|
|**Alerttype**|benzersiz uyarı tanımlayıcısı|
|**Güven Düzeyi**|(İsteğe bağlı) Bu uyarının güven düzeyi (Yüksek/Düşük)|
|**Güven Puanı**|(İsteğe bağlı) Güvenlik uyarısının sayısal güven göstergesi|
|**Açıklama**|Uyarı için açıklama metni|
|**Displayname**|Uyarının görüntü adı|
|**Endtime**|Uyarının etki bitiş saati (uyarıya katkıda bulunan son olayın zamanı)|
|**Varlıklar**|Uyarıyla ilgili varlıkların listesi. Bu liste, farklı türde varlıkların bir karışımını tutabilir|
|**Genişletilmiş Linkler**|(İsteğe bağlı) Uyarı ile ilgili tüm bağlantılar için bir çanta. Bu çanta çeşitli türleri için bağlantıların bir karışımı tutabilir|
|**Genişletilmiş Özellikler**|Uyarıyla ilgili ek alanlardan oluşan bir çanta|
|**IsIncident**|Uyarının bir olay mı yoksa normal bir uyarı mı olduğunu belirler. Olay, birden çok uyarıyı bir güvenlik olayına toplayan bir güvenlik uyarısıdır|
|**İşlemeSon Zaman**|Uyarının oluşturulduğu UTC zaman damgası|
|**ÜrünBileşen Adı**|(İsteğe bağlı) Uyarıyı oluşturan ürünün içindeki bileşenin adı.|
|**ProductName**|sabit ('Azure Güvenlik Merkezi')|
|**Sağlayıcı Adı**|Kullanılma -yan|
|**Düzeltme Adımları**|Güvenlik tehdidini düzeltmek için el ile eylem öğeleri|
|**ResourceId**|Etkilenen kaynağın tam tanımlayıcısı|
|**Önem Derecesi**|Uyarı şiddeti (Yüksek/Orta/Düşük/Bilgilendir)|
|**SourceComputerId**|etkilenen sunucu için benzersiz bir GUID (uyarı sunucuda oluşturulursa)|
|**SourceSystem**|Kullanılma -yan|
|**Starttime**|Uyarının etki başlangıç saati (uyarıya katkıda bulunan ilk olayın saati)|
|**SystemAlertId**|Bu güvenlik uyarısı örneğinin benzersiz tanımlayıcısı|
|**Kiracı Kimliği**|taranan kaynağın bulunduğu aboneliğin üst Azure Etkin dizin kiracısının tanımlayıcısı|
|**TimeGenerated**|Değerlendirmenin gerçekleştiği UTC zaman damgası (Güvenlik Merkezi'nin tçalışma süresi) (DiscoveredTimeUTC ile aynı)|
|**Tür**|sabit ('SecurityAlert')|
|**Satıcıadı**|Uyarıyı sağlayan satıcının adı (örn. 'Microsoft')|
|**SatıcıOriginalId**|Kullanılma -yan|
|**Çalışma AlanıKaynak Grubu**|uyarının bir VM, Sunucu, Sanal Makine Ölçeği Seti veya Uygulama Hizmeti örneğinde oluşturulması durumunda, çalışma alanına rapor veren, bu çalışma alanı kaynak grubu adını içeren|
|**Çalışma AlanıAbonelikId**|uyarının bir VM, Sunucu, Sanal Makine Ölçeği Seti veya Uygulama Hizmeti örneğinde oluşturulması durumunda, bir çalışma alanına rapor veren, bu çalışma alanı aboneliğiNi içeren|
|||
