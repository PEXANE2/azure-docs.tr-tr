---
title: ISMC sorunlarını giderme
description: BT Hizmet Yönetimi Bağlayıcısı 'deki yaygın sorunları çözmeyi öğrenin.
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: 7094bf07453c06831fecfa2056480bf498b26a72
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102041629"
---
# <a name="troubleshoot-problems-in-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı sorunları giderme

Bu makalede BT Hizmet Yönetimi Bağlayıcısı (ITSMC) ' deki yaygın sorunlar ve bunların nasıl giderileceği açıklanmaktadır.

Azure Izleyici, izleme verilerinizde önemli koşullar bulduğunda uyarıları size bildirir. Bu uyarılar, sisteminizin kullanıcıları tarafından bildirilmeksizin sorunları belirlemenize ve adresetmenize yardımcı olur.

Uyarıları nasıl almak istediğinizi seçebilirsiniz. Posta, SMS veya Web kancasını seçebilir veya bir çözümü otomatik hale getirebilirsiniz. 

Diğer bir seçenek de ıSMC aracılığıyla bilgilendirilir. ISMC, size ServiceNow gibi bir dış bilet oluşturma sistemine uyarı gönderme seçeneği sunar.

## <a name="use-the-dashboard-to-analyze-incident-and-change-request-data"></a>Olay ve değişiklik isteği verilerini çözümlemek için panoyu kullanma

Bir bağlantı ayarlarken yapılandırmanıza bağlı olarak, ıSMC, 120 güne kadar olay ve değişiklik isteği verilerini eşitleyebilir. Bu verilerin günlük kaydı şemasını almak için, [ITSM ürün makaleinizden eşitlenen verilere](./itsmc-synced-data.md) bakın.

ISMC panosunu kullanarak olay ve değişiklik isteği verilerini görselleştirebilirsiniz:

![ISMC panosunu gösteren ekran görüntüsü.](media/itsmc-overview/itsmc-overview-sample-log-analytics.png)

Pano, bağlayıcı durumu hakkında bilgi de sağlar. Bağlantılarla ilgili sorunları çözümlemek için bu bilgileri bir başlangıç noktası olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Panoyu kullanarak hata araştırma](./itsmc-dashboard.md).

## <a name="use-service-map-to-visualize-incidents"></a>Olayları görselleştirmek için Hizmet Eşlemesi kullanma

Ayrıca, Hizmet Eşlemesi etkilenen bilgisayarlara karşı eşitlenen olayları da görselleştirebilirsiniz.

Hizmet Eşlemesi Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Sunucularınızı göz önünde bulundurmak üzere görüntülemenize izin verir: önemli hizmetler sunan bağlantılı sistemler olarak. 

Hizmet Eşlemesi, tüm TCP bağlantılı mimarideki sunucular, süreçler ve bağlantı noktaları arasındaki bağlantıları gösterir. Bir aracının yüklenmesinden başka bir yapılandırma gerekmez. Daha fazla bilgi için bkz. [hizmet eşlemesi kullanma](../vm/service-map.md).

Hizmet Eşlemesi kullanıyorsanız, bu örnekte gösterildiği gibi, It Service Management (ıTSSM) çözümlerinde oluşturulan hizmet Masası öğelerini görüntüleyebilirsiniz:

![Log Analytics ekranını gösteren ekran görüntüsü.](media/itsmc-overview/itsmc-overview-integrated-solutions.png)

## <a name="resolve-problems"></a>Sorunları çözme

Aşağıdaki bölümler, yaygın belirtileri, olası nedenleri ve çözümleri belirler. 

### <a name="a-connection-to-the-itsm-system-fails-and-you-get-an-error-in-saving-connection-message"></a>ITSM sistemine bir bağlantı başarısız olur ve "bağlantı kaydedilirken hata oluştu" iletisini alırsınız

**Neden**: neden şu seçeneklerden biri olabilir:

* Kimlik bilgileri yanlış.
* Ayrıcalıklar yetersiz.
* Service Manager bağlantıları için: Web uygulaması yanlış dağıtıldı.

**Çözüm**:

* ServiceNow, Cherwell ve Provance bağlantıları için:
  * Bağlantıların her biri için Kullanıcı adını, parolayı, istemci KIMLIĞINI ve istemci gizli anahtarını doğru girdiğinizden emin olun.  
  * ServiceNow için, karşılık gelen ıTSM ürününde [yeterli ayrıcalıklara](itsmc-connections-servicenow.md#install-the-user-app-and-create-the-user-role) sahip olduğunuzdan emin olun.

* Service Manager bağlantıları için:  
  * Web uygulamasının başarıyla dağıtıldığından ve karma bağlantının oluşturulduğundan emin olun. Bağlantının şirket içi Service Manager bilgisayarla başarıyla yapıldığını doğrulamak için, [karma bağlantı oluşturma belgelerinde](./itsmc-connections-scsm.md#configure-the-hybrid-connection)açıklandığı gibi Web uygulaması URL 'sine gidin.  

### <a name="duplicate-work-items-are-created"></a>Yinelenen iş öğeleri oluşturuldu

**Neden**: neden bu iki seçenekten biri olabilir:

* Uyarı için birden fazla ıTSSM eylemi tanımlandı.
* Uyarı çözüldü.

**Çözüm**: iki çözüm olabilir:

* Uyarı başına bir tek ıSM eylem grubuna sahip olduğunuzdan emin olun.
* Uyarı çözümlendiğinde ıSMC, eşleşen iş öğelerinin durum güncelleştirmelerini desteklemez. Yeni bir çözümlenmiş iş öğesi oluşturun.

### <a name="work-items-are-not-created"></a>İş öğeleri oluşturulmaz

**Neden**: bu belirtinin çeşitli nedenleri olabilir:

* Kod ServiceNow tarafında değiştirildi.
* İzinler yanlış yapılandırılmış.
* ServiceNow hız sınırları çok yüksek veya çok düşük.
* Yenileme belirtecinin zaman aşımına uğradı.
* ISMC silindi.

**Çözüm**: [panoyu](itsmc-dashboard.md) denetleyin ve bağlayıcı durumu bölümündeki hataları gözden geçirin. Ardından [sık karşılaşılan hataları ve bunların çözümlerini](itsmc-dashboard-errors.md)gözden geçirin.

### <a name="you-cant-create-an-itsm-action-for-an-action-group"></a>Bir eylem grubu için ıTSSM eylemi oluşturamazsınız

**Neden**: yeni oluşturulan bir ısmc örneği, ilk eşitlemeyi henüz bitiremedi.

**Çözüm**: [ortak hataları ve bunların çözümlerini](itsmc-dashboard-errors.md)gözden geçirin.

### <a name="sync-connection"></a>Bağlantıyı Eşitle 

**Neden**: bu belirtinin çeşitli nedenleri olabilir:

* Şablonlar, eylem tanımının bir parçası olarak gösterilmez.
* Incedents/olayları ServiceNow içinde oluşturulmaz.

**Çözüm**: [bağlayıcıyı eşitleyin](itsmc-resync-servicenow.md).
