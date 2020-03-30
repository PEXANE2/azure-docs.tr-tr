---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 2fe091a4ff0295ecadfd69ba3f2d4ca59e9612e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334710"
---
>[!NOTE]
>Sabit olmayan kaynaklar için, kotalarda artış istemek için bir destek bileti açın. Daha yüksek sınırlar elde etmek için ek Azure Medya Hizmetleri hesapları oluşturmayın.

| Kaynak | Sınır | 
| --- | --- | 
| Azure Medya Hizmetleri hesapları tek bir abonelikte | 25 (sabit) |
| Medya Hizmetleri hesabı başına ayrılmış medya birimleri |25 (S1)<br/>10 (S2, S3)<sup>1</sup> | 
| Medya Hizmetleri hesabı başına işler | 50.000<sup>2</sup> |
| İş başına zincirleme görev sayısı | 30 (sabit) |
| Medya Hizmetleri hesabı başına varlıklar | 1.000.000|
| Görev başına varlık sayısı | 50 |
| İş başına varlık sayısı | 100 |
| Tek seferde bir varlıkla ilişkilendirilen benzersiz bulucu sayısı | 5<sup>4.000</sup> |
| Medya Hizmetleri hesabı başına canlı kanallar |5|
| Kanal başına durdurulmuş durumdaki program sayısı |50|
| Kanal başına çalışır durumdaki program sayısı |3|
| Medya Hizmetleri hesabı başına durdurulan veya çalışan uç noktaları akışı|2|
| Akış uç noktası başına akış birimleri |10 |
| Depolama hesapları | 1.000<sup>5</sup> (sabit) |
| İlkeler | 1.000.000<sup>6</sup> |
| Dosya boyutu| Bazı senaryolarda, Medya Hizmetleri'nde işlenmek üzere desteklenen maksimum dosya boyutunda bir sınır vardır. <sup>7.000</sup> |

<sup>1.1.2</sup> Örneğin türü S2'den S1'e değiştirirseniz, ayrılmış maksimum birim sınırları sıfırlanır.

<sup>2.000</sup> Bu numara sıralanmış, tamamlanmış, etkin leştirilmiş ve iptal edilen işleri içerir. Silinen işleri içermez. **IJob.Delete** veya **DELETE** HTTP isteğini kullanarak eski işleri silebilirsiniz.

1 Nisan 2017 itibarıyla, hesabınızdaki 90 günden büyük tüm iş kayıtları, ilişkili görev kayıtlarıyla birlikte otomatik olarak silinir. Toplam kayıt sayısı maksimum kotanın altında olsa bile otomatik silme işlemi gerçekleşir. İş ve görev bilgilerini arşivlemek [için, Medya Hizmetleri .NET SDK ile varlıkları yönet'te](../articles/media-services/previous/media-services-dotnet-manage-entities.md)açıklanan kodu kullanın.

<sup>3.2.2</sup> İş varlıklarını listelemek için bir istekte bulunduğunuzda, istek başına en fazla 1.000 iş döndürülür. Gönderilen tüm işleri izlemek için, [OData sistem sorgu seçeneklerinde](/previous-versions/dynamicscrm-2015/developers-guide/gg309461(v=crm.7))açıklandığı gibi üstteki veya sorguları atlayın' ı kullanın.

<sup>4.2.2</sup> Yer bulucular kullanıcı başına erişim denetimini yönetmek için tasarlanmıyor. Bireysel kullanıcılara farklı erişim hakları vermek için dijital haklar yönetimi (DRM) çözümlerini kullanın. Daha fazla bilgi için [bkz.](../articles/media-services/previous/media-services-content-protection-overview.md)

<sup>5.000</sup> Depolama hesapları aynı Azure aboneliğine ait olmalıdır.

<sup>6.000</sup> Farklı Medya Hizmetleri ilkeleri için 1.000.000 politika sınırı vardır. Bir örnek, Konum belirleme ilkesi veya ContentKeyAuthorizationPolicy içindir. 

>[!NOTE]
> Her zaman aynı günleri kullanıyor ve izinlere erişirseniz, aynı ilke kimliğini kullanın. Bilgi ve örnek için [bkz.](../articles/media-services/previous/media-services-dotnet-manage-entities.md#limit-access-policies)

<sup>7.000</sup> Tek bir blob için desteklenen maksimum boyut şu anda Azure Blob Depolama'da en fazla 5 TB'dir. Hizmet tarafından kullanılan VM boyutlarına dayalı Medya Hizmetlerinde ek sınırlar geçerlidir. Boyut sınırı, yüklediğiniz dosyalar ve Medya Hizmetleri işleme (kodlama veya çözümleme) sonucunda oluşturulan dosyalar için de geçerlidir. Kaynak dosyanız 260 GB'dan büyükse, İşiniz büyük olasılıkla başarısız olur. 

Aşağıdaki tabloda medya ayrılmış birimleri S1, S2 ve S3 sınırları gösterir. Kaynak dosyanız tabloda tanımlanan sınırlardan daha büyükse, kodlama işiniz başarısız olur. Uzun süreli 4K çözünürlük kaynaklarını kodlarsanız, gereken performansı elde etmek için S3 ortam ayrılmış birimleri kullanmanız gerekir. S3 medya rezerve birimlerindeki 260 GB sınırından daha büyük 4K içeriğiniz varsa, bir destek bileti açın.

|Ortam ayrılmış birim türü    |Maksimum giriş boyutu (GB)|
|---|---|
|S1 |    26|
|S2    | 60|
|S3    |260|
