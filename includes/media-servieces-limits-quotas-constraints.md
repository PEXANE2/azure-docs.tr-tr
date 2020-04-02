---
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 03/31/2020
ms.author: juliako
ms.openlocfilehash: 654af2c9e57bca9368eb0a5cdf32e692852ba522
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80573228"
---
> [!NOTE]
> Sabit olmayan kaynaklar için, kotalarda artış istemek için bir destek bileti açın. Daha yüksek sınırlar elde etmek için ek Azure Medya Hizmetleri hesapları oluşturmayın.

### <a name="account-limits"></a>Hesap limitleri

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Tek bir abonelikteki Medya Hizmetleri hesapları | 25 (sabit) |

### <a name="asset-limits"></a>Varlık limitleri

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Medya Hizmetleri hesabı başına varlıklar | 1.000.000|

### <a name="storage-limits"></a>Depolama limitleri

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Dosya boyutu| Bazı senaryolarda, Medya Hizmetleri'nde işlenmek üzere desteklenen en büyük dosya boyutunda bir sınır vardır. <sup>(1)</sup> |
| Depolama hesapları | 100<sup>(2)</sup> (sabit) |

<sup>1</sup> Tek bir blob için desteklenen maksimum boyut şu anda Azure Blob Depolama'da 5 TB'a kadardır. Hizmet tarafından kullanılan VM boyutlarına dayalı Medya Hizmetlerinde ek sınırlar geçerlidir. Boyut sınırı, yüklediğiniz dosyalar ve Medya Hizmetleri işleme (kodlama veya çözümleme) sonucunda oluşturulan dosyalar için de geçerlidir. Kaynak dosyanız 260 GB'dan büyükse, İşiniz büyük olasılıkla başarısız olur. 

Aşağıdaki tabloda medya ayrılmış birimleri S1, S2 ve S3 sınırları gösterir. Kaynak dosyanız tabloda tanımlanan sınırlardan daha büyükse, kodlama işiniz başarısız olur. Uzun süreli 4K çözünürlük kaynaklarını kodlarsanız, gereken performansı elde etmek için S3 ortam ayrılmış birimleri kullanmanız gerekir. S3 medya rezerve birimlerindeki 260 GB sınırından daha büyük 4K içeriğiniz varsa, bir destek bileti açın.

|Ortam ayrılmış birim türü|Maksimum giriş boyutu (GB)|
|---|---|
|S1 |    26|
|S2    | 60|
|S3    |260|

<sup>2</sup> Depolama hesapları aynı Azure aboneliğinden olmalıdır.

### <a name="jobs-encoding--analyzing-limits"></a>İşler (kodlama & çözümleme) sınırları

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Medya Hizmetleri hesabı başına işler | 500.000 <sup>(3)</sup> (sabit)|
| İş Başına İş Girişleri | 50 (sabit)|
| İş Başına İş çıktıları | 20 (sabit) |
| Medya Hizmetleri hesabı başına dönüşümler | 100 (sabit)|
| Dönüşüm'de çıktıları dönüştürün | 20 (sabit) |
| İş girişi başına dosyalar|10 (sabit)|

<sup>3</sup> Bu numara sıralanmış, tamamlanmış, etkin leştirilmiş ve iptal edilen İşleri içerir. Silinen İşler içermez. 

Toplam kayıt sayısı maksimum kotanın altında olsa bile, hesabınızdaki 90 günden büyük tüm İş kayıtları otomatik olarak silinir. 

### <a name="live-streaming-limits"></a>Canlı akış limitleri

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Medya Hizmetleri hesabı başına Canlı Etkinlikler <sup>(4)</sup> |5|
| Canlı Etkinlik Başına Canlı Çıktılar |3 <sup>(5)</sup> |
| Max Canlı Çıkış süresi | 25 saat |

<sup>4</sup> Live Event sınırlamaları hakkında ayrıntılı bilgi için [Bkz. Canlı Etkinlik türleri karşılaştırması ve sınırlamaları.](../articles/media-services/latest/live-event-types-comparison.md)

<sup>5</sup> Canlı Çıktılar oluşturmayla başlar ve silindiğinde durur.

### <a name="packaging--delivery-limits"></a>Ambalaj & teslimat limitleri

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Medya Hizmetleri hesabı başına Akış Uç Noktaları (durduruldu veya çalışıyor)|2 (sabit)|
| Dinamik Bildirim Filtreleri|100|
| Akış İlkeleri | 100 <sup>(6)</sup> |
| Bir Varlıkla aynı anda ilişkili Benzersiz Akış Konumbelirleyicileri | 100<sup>(7)</sup> (sabit) |

<sup>6</sup> Özel bir [Akış İlkesi](https://docs.microsoft.com/rest/api/media/streamingpolicies)kullanırken, Medya Hizmeti hesabınız için sınırlı sayıda bu tür ilke ler tasarlamalı ve aynı şifreleme seçenekleri ve protokolleri gerektiğinde Bunları StreamingLocators'Larınız için yeniden kullanmalısınız. Her Akış Bulucu için yeni bir Akış İlkesi oluşturmamalısınız.

<sup>7</sup> Akış Lı Konum belirleyiciler, kullanıcı başına erişim denetimini yönetmek için tasarlanmaz. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın.

### <a name="protection-limits"></a>Koruma sınırları

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| İçerik Anahtar İlkesi Başına Seçenekler |30 | 
| Medya Hizmetleri anahtar teslim hizmetindeki DRM türlerinin her biri için hesap başına aylık lisanslar|1.000.000|

### <a name="support-ticket"></a>Destek bileti

Sabit olmayan kaynaklar için, bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açarak kotaların yükseltilmesini isteyebilirsiniz. İstenilen kota değişiklikleri, kullanım durumu senaryoları ve gerekli bölgeler le ilgili isteğe ayrıntılı bilgi ekleyin. <br/>Daha yüksek sınırlar elde etmek için başka Azure Media Services hesapları **oluşturmayın**.
