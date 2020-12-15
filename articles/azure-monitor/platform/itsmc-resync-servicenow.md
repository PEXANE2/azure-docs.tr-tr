---
title: ServiceNow eşitleme sorunlarını el ile çözme
description: ServiceNow bağlantısını sıfırlayın Microsoft Azure içindeki uyarılar ServiceNow 'ı çağırabilir
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: 3e836873219bde3836f2863e328b0b6f5b89addc
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97507294"
---
# <a name="troubleshooting-problems-in-itsm-connector"></a>ITSM Bağlayıcısındaki sorunları giderme

Bu makalede ITSM Bağlayıcısı ve bunların nasıl giderileceği hakkında yaygın sorunlar ele alınmaktadır.

İzleme verilerinizde önemli koşullar bulunduğunda Azure Izleyici uyarıları size önceden bildirimde bulunur. Bunlar, sisteminizin kullanıcıları tarafından bildirilmeksizin sorunları tanımlamanızı ve adreslerinizi belirlemenizi sağlar. Uyarı hakkında daha fazla bilgi için bkz. Microsoft Azure uyarılara genel bakış.
Müşteri, bir çözümü otomatik hale getirmek için e-posta, SMS, Web kancası veya hatta Bildirim almak için başka bir seçenek ıTSM kullanmaktır.
ITSM, uyarıları ServiceNow gibi dış bilet oluşturma sistemine gönderme seçeneği sunar.

## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Olay ve değişiklik isteği verilerini görselleştirin ve çözümleyin

Bir bağlantı ayarlarken yapılandırmanıza bağlı olarak, ıSMC, 120 güne kadar olay ve değişiklik isteği verilerini eşitleyebilir. Bu veriler için günlük kaydı şeması, bu makalenin [ek bilgiler bölümünde](https://docs.microsoft.com/azure/azure-monitor/platform/itsmc-overview#additional-information) verilmiştir.

ISMC panosunu kullanarak olay ve değişiklik isteği verilerini görselleştirebilirsiniz:

![ISMC panosunu gösteren ekran görüntüsü.](media/itsmc-overview/itsmc-overview-sample-log-analytics.png)

Pano, bağlantı sorunlarını analiz etmek için bir başlangıç noktası olarak kullanabileceğiniz bağlayıcı durumu hakkında bilgi de sağlar.

Ayrıca, Hizmet Eşlemesi etkilenen bilgisayarlara karşı eşitlenen olayları da görselleştirebilirsiniz.

Hizmet Eşlemesi Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Sunucularınızı göz önünde bulundurmak üzere görüntülemenize izin verir: önemli hizmetler sunan bağlantılı sistemler olarak. Hizmet Eşlemesi, tüm TCP bağlantılı mimarideki sunucular, süreçler ve bağlantı noktaları arasındaki bağlantıları gösterir. Bir aracının yüklenmesinden başka bir yapılandırma gerekmez. Daha fazla bilgi için bkz. [hizmet eşlemesi kullanma](../insights/service-map.md).

Hizmet Eşlemesi kullanıyorsanız, ıTSM çözümlerinde oluşturulan hizmet Masası öğelerini burada gösterildiği gibi görüntüleyebilirsiniz:

![Log Analytics ekranını gösteren ekran görüntüsü.](media/itsmc-overview/itsmc-overview-integrated-solutions.png)

## <a name="how-to-manually-fix-servicenow-sync-problems"></a>ServiceNow eşitleme sorunlarını el ile çözme

Azure Izleyici, üçüncü taraf BT hizmet yönetimi (ıSM) sağlayıcılarına bağlanabilir. ServiceNow, bu sağlayıcılardan biridir.

Güvenlik nedenleriyle, ServiceNow ile bağlantınız için kullanılan kimlik doğrulama belirtecini yenilemeniz gerekebilir.
Bağlantıyı yeniden etkinleştirmek ve belirteci yenilemek için aşağıdaki eşitleme sürecini kullanın:


1. Üst arama başlığında çözümü arayın ve ilgili çözümleri seçin

    ![En üstteki arama başlığını ve ilgili çözümlerin nerede seçdiğinin gösterildiği ekran görüntüsü.](media/itsmc-resync-servicenow/solution-search-8bit.png)

1. Çözüm ekranında, abonelik filtresindeki "Tümünü Seç" i seçin ve ardından "ServiceDesk" olarak filtreleyin

    ![Tümünü Seç ' in nerede ve ServiceDesk tarafından filtreleneceğini gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/solutions-list-8bit.png)

1. ITSM bağlantınızın çözümünü seçin.
1. Sol başlıkta ıTSM bağlantısı ' nı seçin.

    ![ITSM bağlantılarının nerede seçdiğinin gösterildiği ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-connector-8bit.png)

1. Listedeki her bağlayıcıyı seçin. 
    1. Yapılandırmak için bağlayıcı adına tıklayın
    1. Artık kullanımda olmayan bağlayıcıları Sil

    1. İş ortağı türüne göre [bu tanımlara](./itsmc-connections.md) göre alanları güncelleştirin

    1. Eşitle 'ye tıklayın

       ![Eşitle düğmesini vurgulayan ekran görüntüsü.](media/itsmc-resync-servicenow/resync-8bit2.png)

    1. Kaydet 'e tıklayın

        ![Yeni bağlantı](media/itsmc-resync-servicenow/save-8bit.png)

f.    İşlemin başarılı ile bitmediğini görmek için bildirimleri gözden geçirin

## <a name="troubleshoot-itsm-connections"></a>ITSM bağlantılarında sorun giderme

- Bağlantı bağlanılan kaynağın kullanıcı arabiriminden başarısız olursa ve **bağlantı iletisini kaydederken bir hata** alırsanız, aşağıdaki adımları uygulayın:
   - ServiceNow, Cherwell ve Provance bağlantıları için:  
     - Bağlantıların her biri için Kullanıcı adını, parolayı, istemci KIMLIĞINI ve istemci gizli anahtarını doğru girdiğinizden emin olun.  
     - Bağlantıyı yapmak için karşılık gelen ıTSM ürününde yeterli ayrıcalıklara sahip olduğunuzdan emin olun.  
   - Service Manager bağlantıları için:  
     - Web uygulamasının başarıyla dağıtıldığından ve karma bağlantının oluşturulduğundan emin olun. Bağlantının şirket içi Service Manager bilgisayarla başarıyla yapıldığını doğrulamak için [karma bağlantı](./itsmc-connections.md#configure-the-hybrid-connection)oluşturma belgelerinde açıklandığı gibi Web uygulaması URL 'sine gidin.  

- ServiceNow 'daki veriler Log Analytics ile eşitlenmemişse ServiceNow örneğinin uyuma olmadığından emin olun. ServiceNow dev örnekleri bazen uzun bir süre boşta kaldığında uyku moduna geçebilir. Bu durum oluşmuyorsa, sorunu bildirin.
- Log Analytics uyarılar harekete geçmemişse ancak ıTSM ürününde iş öğeleri oluşturulmadıysa, yapılandırma öğeleri iş öğelerine oluşturulmaz/bağlanmadıysa veya diğer bilgiler için şu kaynaklara bakın:
   -  ISMC: çözüm, bağlantıların, iş öğelerinin, bilgisayarların ve daha fazlasını bir Özet gösterir. **Bağlayıcı durumu** etiketine sahip kutucuğu seçin. Bunu yapmak ilgili sorguyla **aramayı günlüğe kaydetmek** için yönlendirir. `LogType_S`Daha fazla bilgi için ' a sahip günlük kayıtlarına bakın `ERROR` .
   - **Günlük arama** sayfası: doğrudan sorgu kullanarak hataları ve Ilgili bilgileri görüntüleyin `*ServiceDeskLog_CL*` .

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Web uygulaması dağıtımı Service Manager sorunlarını giderme

-   Web uygulaması dağıtımıyla ilgili sorun yaşıyorsanız, abonelikte kaynak oluşturma/dağıtma izinlerinizin olduğundan emin olun.
-   Bir **nesne başvurusunu alırsanız bir nesne hatası örneğine ayarlı değilse** , [komut dosyasını](itsmc-service-manager-script.md)çalıştırdığınızda, **Kullanıcı Yapılandırması** bölümüne geçerli değerler girdiğinizden emin olun.
-   Service Bus geçişi ad alanını oluşturmadıysanız, gerekli kaynak sağlayıcının abonelikte kayıtlı olduğundan emin olun. Kayıtlı değilse, Service Bus geçişi ad alanını Azure portal el ile oluşturun. [Karma bağlantıyı](./itsmc-connections.md#configure-the-hybrid-connection) Azure Portal oluşturduğunuzda da oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

[BT hizmet yönetimi bağlantıları](itsmc-connections.md) hakkında daha fazla bilgi edinin
