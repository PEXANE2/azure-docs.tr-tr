---
title: Azure CDN HTTP ham günlükleri
description: Bu makalede, Azure CDN HTTP ham günlükleri açıklanmaktadır.
services: cdn
author: sohamnchatterjee
manager: danielgi
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2020
ms.author: sohamnc
ms.openlocfilehash: c6e8570746ae3dd0051dbec084c89d90580d28b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371627"
---
# <a name="azure-cdn-http-raw-logs"></a>Azure CDN HTTP ham günlükleri
Ham günlükler, denetim ve sorun giderme için önemli olan işlemler ve hatalar hakkında zengin bilgiler sağlar. Ham günlükleri etkinlik günlüklerinden farklıdır. Etkinlik günlükleri, Azure kaynaklarında yapılan işlemlerde görünürlük sağlar. Ham günlükler, kaynağınızın işlemlerinin kaydını sağlar.

> [!IMPORTANT]
> HTTP ham günlükleri özelliği Microsoft'tan Azure CDN için kullanılabilir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="configuration"></a>Yapılandırma

Azure CDN'niz için Raw günlüklerini Microsoft profilinden yapılandırmak için: 

1. Azure portalı menüsünden TÜM **Kaynaklar** >> **\<CDN profiliniz>. **

2. **İzleme**altında, **Tanılama ayarlarını**seçin.

3. Select **+ Tanılama ayarı ekle.**

    ![CDN tanıalı ayar](./media/cdn-raw-logs/raw-logs-01.png)

    > [!IMPORTANT]
    > Ham günlükler yalnızca profil düzeyinde kullanılabilirken, toplanmış http durum kodu günlükleri bitiş noktası düzeyinde kullanılabilir.

4. **Tanılama ayarları**altında, **Tanılama ayarları adı**altında tanılama ayarı için bir ad girin.

5. **Günlüğü** seçin ve bekletme gün içinde ayarlayın.

6. Hedef **ayrıntılarını**seçin. Hedef seçenekleri şunlardır:
    * **Log Analytics’e gönderme**
        * **Abonelik** ve **Günlük Analizi çalışma alanını**seçin.
    * **Depolama hesabına arşivleme**
        * **Abonelik** ve **Depolama Hesabı'nı**seçin.
    * **Olay merkezine akış**
        * **Abonelik,** **Olay hub ad alanı,** Olay hub adı **(isteğe bağlı)** ve **Olay hub ilke adını**seçin.

    ![CDN tanıalı ayar](./media/cdn-raw-logs/raw-logs-02.png)

7. **Kaydet'i**seçin.

## <a name="raw-logs-properties"></a>Ham günlüközellikleri

Microsoft Service'in Azure CDN'si şu anda Raw günlükleri sağlar. Ham günlükler, her girişte aşağıdaki şemaiçeren ayrı API istekleri sağlar: 

| Özellik              | Açıklama                                                                                                                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İzlemeReferans     | Ön Kapı tarafından sunulan bir isteği tanımlayan ve istemciye X-Azure-Ref üstbilgisi olarak gönderilen benzersiz başvuru dizesi. Belirli bir istek için erişim günlüklerinde arama ayrıntıları için gereklidir. |
| Http Yöntemi            | İstek tarafından kullanılan HTTP yöntemi.                                                                                                                                                                     |
| httpVersion           | İstek veya bağlantı türü.                                                                                                                                                                   |
| Requesturi            | Uri alınan istek.                                                                                                                                                                         |
| İstekBaytlar          | İstek üstbilgisi ve istek gövdesi de dahil olmak üzere baytlar daki HTTP istek iletisinin boyutu.                                                                                                   |
| YanıtBayt         | Yanıt olarak arka uç sunucusu tarafından gönderilen baytlar.                                                                                                                                                    |
| Useragent             | İstemcinin kullandığı tarayıcı türü.                                                                                                                                                               |
| ClientIp              | İstekte bulundu müşterinin IP adresi.                                                                                                                                                  |
| Zaman Dilimi             | Eylemin milisaniye cinsinden aldığı süre.                                                                                                                                            |
| Securityprotocol      | İstek tarafından kullanılan TLS/SSL protokol sürümü veya şifreleme yoksa null.                                                                                                                           |
| Uç Nokta              | CDN uç nokta ana bilgisayar üst CDN profili altında yapılandırıldı.                                                                                                                                   |
| Arka uç Ana Bilgisayar adı     | Geri uç ana bilgisayarının adı veya isteklerin gönderildiği kaynak.                                                                                                                                |
| Menşe kalkanına gönderildi | Doğruysa, bu istek kenar pop yerine başlangıç kalkanı önbelleğinden yanıtlandı anlamına gelir. Başlangıç kalkanı, önbellek isabet oranını iyileştirmek için kullanılan bir üst önbellektir.                                       |
| httpstatuscode        | HTTP durum kodu proxy'den döndürülür.                                                                                                                                                        |
| httpStatusDetails     | İstek te ortaya çıkan durum. Bu dize değerinin anlamı durum başvuru tablosunda bulunabilir.                                                                                              |
| Pop                   | Kullanıcı isteğine yanıt veren kenar pop. POP'ların kısaltmaları kendi metrolarının havaalanı kodlarıdır.                                                                                   |
| Önbellek Durumu          | Nesnenin önbellekten döndürülmüş mü yoksa kaynaktan mı geldiğini belirtir.                                                                                                             |
> [!IMPORTANT]
> HTTP Raw günlükleri **özelliği, 25 Şubat 2020**tarihinden sonra oluşturulan veya güncellenen tüm profiller için otomatik olarak kullanılabilir. Daha önce oluşturulan CDN profilleri için, günlük günlüğe kaydetmeyi ayarladıktan sonra CDN bitiş noktasını güncelleştirmek gerekir. Örneğin, CDN uç noktaları altında coğrafi filtreleme için gezinmek ve iş yükü ile ilgili olmayan herhangi bir ülkeyi engellemek ve kaydet tuşuna basmak. 

> [!NOTE]
> Günlükler, bir sorgu çalıştırarak Log Analytics profiliniz altında görüntülenebilir. Örnek bir sorgu AzureDiagnostics | nerede Kategori == "AzureCdnAccessLog"

## <a name="next-steps"></a>Sonraki Adımlar
Bu makalede, Microsoft CDN hizmeti için HTTP ham günlüklerini etkinleştirdin.

Azure CDN ve bu makalede bahsedilen diğer Azure hizmetleri hakkında daha fazla bilgi için bkz:

* [Analiz et](cdn-log-analysis.md) Azure CDN kullanım desenleri.

* [Azure Monitör](https://docs.microsoft.com/azure/azure-monitor/overview)hakkında daha fazla bilgi edinin.

* Azure [Monitör'de Günlük Analizini](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)Yapılandırın.
