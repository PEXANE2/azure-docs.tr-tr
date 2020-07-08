---
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 03/31/2020
ms.author: juliako
ms.openlocfilehash: d6ff823951e6474a35a514036f02c0aabb17bf01
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82997721"
---
> [!NOTE]
> Düzeltilmeyen kaynaklar için, kotalarda artış istemek üzere bir destek bileti açın. Daha yüksek limitleri elde etmeye yönelik bir denemede ek Azure Media Services hesapları oluşturmayın.

### <a name="account-limits"></a>Hesap sınırları

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Tek bir abonelikteki hesapları Media Services | 25 (sabit) |

### <a name="asset-limits"></a>Varlık sınırları

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Media Services hesap başına varlık sayısı | 1.000.000|

### <a name="storage-media-limits"></a>Depolama (medya) sınırları

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Dosya boyutu| Bazı senaryolarda, Media Services ' de işlenmek üzere desteklenen en büyük dosya boyutu sınırı vardır. <sup>(1</sup> |
| Depolama hesapları | 100<sup>(2)</sup> (Sabit) |

<sup>1</sup> tek bir blob için desteklenen en büyük boyut Şu anda Azure Blob depolamada 5 TB 'a kadar. Ek sınırlar, hizmet tarafından kullanılan VM boyutlarına göre Media Services geçerlidir. Boyut sınırı, karşıya yüklediğiniz dosyalar ve ayrıca Media Services işleme sonucu olarak oluşturulan dosyalar için geçerlidir (kodlama veya çözümleme). Kaynak dosyanız 260 GB 'tan büyükse, Işiniz muhtemelen başarısız olur. 

Aşağıdaki tabloda S1, S2 ve S3 medya ayrılmış birimleri üzerindeki sınırlar gösterilmektedir. Kaynak dosyanız tabloda tanımlanan sınırlardan daha büyükse, kodlama işiniz başarısız olur. Uzun süreli 4K çözümleme kaynaklarını kodlarsanız, gereken performansı elde etmek için S3 medya ayrılmış birimlerini kullanmanız gerekir. S3 medya ayrılmış birimlerindeki 260 GB sınırından büyük 4K içeriğiniz varsa bir destek bileti açın.

|Medya ayrılmış birim türü|Maksimum giriş boyutu (GB)|
|---|---|
|S1 |    26|
|S2    | 60|
|S3    |260|

<sup>2</sup> depolama hesapları aynı Azure aboneliğinden olmalıdır.

### <a name="jobs-encoding--analyzing-limits"></a>İşler (kodlama & analiz) sınırları

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Media Services hesap başına iş sayısı | 500.000 <sup>(3)</sup> (Sabit)|
| Iş başına iş girişleri | 50 (düzeltildi)|
| Iş başına iş çıkışları | 20 (Sabit) |
| Media Services hesap başına dönüşümler | 100 (düzeltildi)|
| Dönüşümde çıkışları dönüştürme | 20 (Sabit) |
| İş girişi başına dosya sayısı|10 (Sabit)|

<sup>3</sup> bu sayı, kuyruğa alınmış, tamamlanmış, etkin ve Iptal edilmiş işleri içerir. Silinen Işleri içermez. 

Toplam kayıt sayısı maksimum kotanın altında olsa da, hesabınızdaki 90 günden eski bir Iş kaydı otomatik olarak silinir. 

### <a name="live-streaming-limits"></a>Canlı akış sınırları

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Media Services hesap başına canlı olaylar <sup>(4)</sup> |5|
| Canlı olay başına canlı çıktılar |3 <sup>(5)</sup> |
| En büyük canlı çıkış süresi | [DVR penceresinin boyutu](../articles/media-services/latest/live-event-cloud-dvr.md) |

<sup>4</sup> canlı olay sınırlamaları hakkında ayrıntılı bilgi için bkz. [canlı olay türleri karşılaştırması ve sınırlamaları](../articles/media-services/latest/live-event-types-comparison.md).

<sup>5</sup> canlı çıktılar oluşturma sırasında başlar ve silindiğinde durdurulur.

### <a name="packaging--delivery-limits"></a>Paketleme & teslim limitleri

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Media Services hesap başına akış uç noktaları (durdurulmuş veya çalışıyor)|2 (Sabit)|
| Dinamik Bildirim Filtreleri|100|
| Akış İlkeleri | 100 <sup>(6)</sup> |
| Tek seferde bir varlıkla ilişkili benzersiz akış Konumlandırıcı | 100<sup>(7)</sup> (Sabit) |

<sup>6</sup> özel bir [akış Ilkesi](https://docs.microsoft.com/rest/api/media/streamingpolicies)kullanırken, medya hizmeti hesabınız için sınırlı sayıda ilke kümesi tasarlamalı ve aynı şifreleme seçenekleri ve protokoller gerektiğinde bunları streamingbulucular için yeniden kullanmanız gerekir. Her bir akış bulucu için yeni bir akış Ilkesi oluşturmamalısınız.

<sup>7</sup> akış Konumlandırıcı, Kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın.

### <a name="protection-limits"></a>Koruma sınırları

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Içerik anahtarı Ilkesi başına seçenekler |30 | 
| Hesap başına Media Services anahtar teslim hizmetindeki her bir DRM türü için aylık lisans sayısı|1.000.000|

### <a name="support-ticket"></a>Destek bileti

Düzeltilmeyen kaynaklar için bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açarak kotaların ortaya çıkarılmasını isteyebilirsiniz. İsteğe bağlı istenen kota değişiklikleri, kullanım örneği senaryoları ve gerekli bölgeler üzerinde ayrıntılı bilgi ekleyin. <br/>Daha yüksek sınırlar elde etmek için başka Azure Media Services hesapları **oluşturmayın**.
