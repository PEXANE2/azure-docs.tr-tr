---
title: ITSM Bağlayıcısındaki sorunları giderme
description: BT Hizmet Yönetimi Bağlayıcısı sorunlarını giderme
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: e43c5fb36c5395e12fd0b9c2c67b787a1137f5d0
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98761990"
---
# <a name="troubleshooting-problems-in-itsm-connector"></a>ITSM Bağlayıcısındaki sorunları giderme

Bu makalede ITSM Bağlayıcısı ve bunların nasıl giderileceği hakkında yaygın sorunlar ele alınmaktadır.

İzleme verilerinizde önemli koşullar bulunduğunda Azure Izleyici uyarıları size önceden bildirimde bulunur. Bunlar, sisteminizin kullanıcıları tarafından bildirilmeksizin sorunları tanımlamanızı ve adreslerinizi belirlemenizi sağlar. Uyarı hakkında daha fazla bilgi için bkz. Microsoft Azure uyarılara genel bakış.
Müşteri, bir çözümü otomatik hale getirmek için e-posta, SMS, Web kancası veya hatta Bildirim almak için başka bir seçenek ıTSM kullanmaktır.
ITSM, uyarıları ServiceNow gibi dış bilet oluşturma sistemine gönderme seçeneği sunar.

## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Olay ve değişiklik isteği verilerini görselleştirin ve çözümleyin

Bir bağlantı ayarlarken yapılandırmanıza bağlı olarak, ıSMC, 120 güne kadar olay ve değişiklik isteği verilerini eşitleyebilir. Bu veriler için günlük kaydı şeması, bu makalenin [ek bilgiler bölümünde](./itsmc-synced-data.md) verilmiştir.

ISMC panosunu kullanarak olay ve değişiklik isteği verilerini görselleştirebilirsiniz:

![ISMC panosunu gösteren ekran görüntüsü.](media/itsmc-overview/itsmc-overview-sample-log-analytics.png)

Pano, bağlantı sorunlarını analiz etmek için bir başlangıç noktası olarak kullanabileceğiniz bağlayıcı durumu hakkında bilgi de sağlar.

Pano araştırması hakkında daha fazla bilgi edinmek için, bkz. [Panoyu kullanarak hata araştırma](./itsmc-dashboard.md).

### <a name="service-map"></a>Hizmet eşlemesi

Ayrıca, Hizmet Eşlemesi etkilenen bilgisayarlara karşı eşitlenen olayları da görselleştirebilirsiniz.

Hizmet Eşlemesi Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Sunucularınızı göz önünde bulundurmak üzere görüntülemenize izin verir: önemli hizmetler sunan bağlantılı sistemler olarak. Hizmet Eşlemesi, tüm TCP bağlantılı mimarideki sunucular, süreçler ve bağlantı noktaları arasındaki bağlantıları gösterir. Bir aracının yüklenmesinden başka bir yapılandırma gerekmez. Daha fazla bilgi için bkz. [hizmet eşlemesi kullanma](../insights/service-map.md).

Hizmet Eşlemesi kullanıyorsanız, ıTSM çözümlerinde oluşturulan hizmet Masası öğelerini burada gösterildiği gibi görüntüleyebilirsiniz:

![Log Analytics ekranını gösteren ekran görüntüsü.](media/itsmc-overview/itsmc-overview-integrated-solutions.png)

## <a name="common-symptoms---how-should-it-be-resolved"></a>Ortak belirtiler-nasıl çözülmesi gerekir?

Aşağıdaki liste, yaygın belirtileri ve nasıl çözümlenmelidir:

* **Belirti**: bağlantı ITSM sistemine bağlanamazsa ve **bağlantı Iletisini kaydederken bir hata** alırsanız.

    **Neden**: neden seçeneklerden biri olabilir:
    * Yanlış kimlik bilgileri
     * Yetersiz ayrıcalıklar
     * Web uygulamasının doğru bir şekilde dağıtılması gerekir

    **Çözüm**:
    * ServiceNow, Cherwell ve Provance bağlantıları için:
        * Bağlantıların her biri için Kullanıcı adını, parolayı, istemci KIMLIĞINI ve istemci gizli anahtarını doğru girdiğinizden emin olun.  
        * ServiceNow için: bağlantıyı [belirtildiği](itsmc-connections-servicenow.md#install-the-user-app-and-create-the-user-role)gibi yapmak için KARŞıLıK gelen ITSM ürününde yeterli ayrıcalıklara sahip olduğunuzdan emin olun.
  * Service Manager bağlantıları için:  
      * Web uygulamasının başarıyla dağıtıldığından ve karma bağlantının oluşturulduğundan emin olun. Bağlantının şirket içi Service Manager bilgisayarla başarıyla yapıldığını doğrulamak için [karma bağlantı](./itsmc-connections-scsm.md#configure-the-hybrid-connection)oluşturma belgelerinde açıklandığı gibi Web uygulaması URL 'sine gidin.  
* **Belirti**: yinelenen iş öğeleri oluşturuldu

    **Neden**: neden iki seçenekten biri olabilir:
    * Uyarı için birden fazla ıTSSM eylemi tanımlandı.
    * Uyarı çözüldü.

    **Çözüm**: iki çözüm olabilir:
    * Uyarı başına bir tek ıSM eylem grubuna sahip olduğunuzdan emin olun.
    * ITSM Bağlayıcısı, bir uyarı çözümlendiğinde eşleşen iş öğeleri durum güncelleştirmesini desteklemez. Yeni bir çözümlenmiş iş öğesi oluşturulur.
* **Belirti**: iş öğeleri oluşturulmaz

    **Neden**: bu belirtinin birkaç nedeni olabilir:
    * ServiceNow tarafında kod değişikliği
    * İzinlerin yanlış yapılandırılması
    * ServiceNow hız sınırları çok yüksek/düşük
    * Yenileme belirtecinin geçerliliği zaman aşımına uğradı
    * ITSM Bağlayıcısı silindi

    **Çözüm**: [panoyu](itsmc-dashboard.md) denetleyebilir ve bağlayıcı durumu bölümündeki hataları gözden geçirebilirsiniz. [Yaygın hataları](itsmc-dashboard-errors.md) gözden geçirin ve hatanın nasıl çözümleneceğini öğrenin.

* **Belirti**: eylem grubu için ıtssm eylemi oluşturulamıyor

    **Neden**: yeni oluşturulan ITSM Bağlayıcısı ilk eşitlemeyi henüz bitiremedi.

    **Çözüm**: [ortak kullanıcı arabirimi hatalarını](itsmc-dashboard-errors.md#ui-common-errors) gözden geçirebilir ve hatanın nasıl çözümleneceğini öğrenebilirsiniz.