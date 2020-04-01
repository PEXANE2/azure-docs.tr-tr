---
title: Azure Medya Hizmetleri v3'te kotalar ve sınırlamalar | Microsoft Dokümanlar
description: Bu konu, Microsoft Azure Media Services v3'teki kotaları ve sınırlamaları açıklar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/17/2019
ms.author: juliako
ms.openlocfilehash: 514c1466bc1a686adfc3e07e1f19bd566e979dc1
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420884"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Azure Medya Hizmetleri v3'teki kotalar ve sınırlamalar

Bu makalede, Azure Medya Hizmetleri v3'teki kotalar ve sınırlamalar açıklanmaktadır.

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Azure Media Services hesabı başına Varlık sayısı | 1.000.000|
| Dinamik Bildirim Filtreleri|100|
| İş Başına İş Girişleri | 50 (sabit)|
| İş Başına İş çıktıları | 20 (sabit) |
| Dönüşüm'de çıktıları dönüştürün | 20 (sabit) |
| İş girişi başına dosyalar|10 (sabit)|
| Dosya boyutu| Bazı senaryolarda, Medya Hizmetleri'nde işlenmek üzere desteklenen en büyük dosya boyutunda bir sınır vardır. <sup>(1)</sup> |
| Medya Hizmetleri hesabı başına işler | 500.000 <sup>(2)</sup> (sabit)|
| Media Services hesabı başına Canlı Etkinlik sayısı |5|
| Tek bir abonelikteki Medya Hizmetleri hesapları | 25 (sabit) |
| Canlı Etkinlik Başına Canlı Çıktılar |3 <sup>(3)</sup> |
| Max Canlı Çıkış süresi | 25 saat |
| Depolama hesapları | 100<sup>(4)</sup> (sabit) |
| Medya Hizmetleri hesabı başına Akış Uç Noktaları (durduruldu veya çalışıyor)|2 (sabit)|
| Akış İlkeleri | 100 <sup>(5)</sup> |
| Medya Hizmetleri hesabı başına dönüşümler | 100 (sabit)|
| Bir Varlıkla aynı anda ilişkili Benzersiz Akış Konumbelirleyicileri | 100<sup>(6)</sup> (sabit) |
| İçerik Anahtar İlkesi Başına Seçenekler |30 | 
| Medya Hizmetleri anahtar teslim hizmetindeki DRM türlerinin her biri için hesap başına aylık lisanslar|1.000.000|

<sup>1</sup> Tek bir blob için desteklenen maksimum boyut şu anda Azure Blob Depolama'da 5 TB'a kadardır. Hizmet tarafından kullanılan VM boyutlarına dayalı Medya Hizmetlerinde ek sınırlar geçerlidir. Boyut sınırı, yüklediğiniz dosyalar ve Medya Hizmetleri işleme (kodlama veya çözümleme) sonucunda oluşturulan dosyalar için de geçerlidir. Kaynak dosyanız 260 GB'dan büyükse, İşiniz büyük olasılıkla başarısız olur. 

Aşağıdaki tabloda medya ayrılmış birimleri S1, S2 ve S3 sınırları gösterir. Kaynak dosyanız tabloda tanımlanan sınırlardan daha büyükse, kodlama işiniz başarısız olur. Uzun süreli 4K çözünürlük kaynaklarını kodlarsanız, gereken performansı elde etmek için S3 ortam ayrılmış birimleri kullanmanız gerekir. S3 medya rezerve birimlerindeki 260 GB sınırından daha büyük 4K içeriğiniz varsa, bir destek bileti açın.

|Ortam ayrılmış birim türü   |Maksimum giriş boyutu (GB)|
|---|---|
|S1 |   26|
|S2 | 60|
|S3 |260|

<sup>2</sup> Bu numara sıralanmış, tamamlanmış, etkin leştirilmiş ve iptal edilen İşleri içerir. Silinen İşler içermez. 

Toplam kayıt sayısı maksimum kotanın altında olsa bile, hesabınızdaki 90 günden büyük tüm İş kayıtları otomatik olarak silinir. 

<sup>3</sup> Canlı Çıktılar oluşturmayla başlar ve silindiğinde durur.

<sup>4</sup> Depolama hesapları aynı Azure aboneliğinden olmalıdır.

<sup>5</sup> Özel bir [Akış İlkesi](https://docs.microsoft.com/rest/api/media/streamingpolicies)kullanırken, Medya Hizmeti hesabınız için sınırlı sayıda bu tür ilke ler tasarlamalı ve aynı şifreleme seçenekleri ve protokolleri gerektiğinde Bunları StreamingLocators'Larınız için yeniden kullanmalısınız. Her Akış Bulucu için yeni bir Akış İlkesi oluşturmamalısınız.

<sup>6</sup> Akış Lı Konum belirleyiciler, kullanıcı başına erişim denetimini yönetmek için tasarlanmaz. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın.

## <a name="support-ticket"></a>Destek bileti

Sabit olmayan kaynaklar için, bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açarak kotaların yükseltilmesini isteyebilirsiniz. İstenilen kota değişiklikleri, kullanım durumu senaryoları ve gerekli bölgeler le ilgili isteğe ayrıntılı bilgi ekleyin. <br/>Daha yüksek sınırlar elde etmek için başka Azure Media Services hesapları **oluşturmayın**.

## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](media-services-overview.md)
