---
title: HTTP ham günlüklerini Azure CDN
description: Bu makalede HTTP ham günlükleri Azure CDN açıklanmaktadır.
services: cdn
author: sohamnchatterjee
manager: danielgi
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/23/2020
ms.author: sohamnc
ms.openlocfilehash: a2522eba17574246ab99a0d47a42f128d5f61ace
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84888647"
---
# <a name="azure-cdn-http-raw-logs"></a>HTTP ham günlüklerini Azure CDN
Ham Günlükler, denetim ve sorun giderme için önemli olan işlemler ve hatalar hakkında zengin bilgiler sağlar. Ham Günlükler etkinlik günlüklerinden farklıdır. Etkinlik günlükleri, Azure kaynakları üzerinde gerçekleştirilen işlemlere görünürlük sağlar. Ham Günlükler, kaynağınızın bir işlem kaydını sağlar.

> [!IMPORTANT]
> HTTP ham günlükleri özelliği, Microsoft 'tan Azure CDN için kullanılabilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="configuration"></a>Yapılandırma

Microsoft profilinden Azure CDN ham günlüklerini yapılandırmak için: 

1. Azure portal menüsünde **tüm kaynaklar**' ı seçin  >>  **\<your-CDN-profile>** .

2. **İzleme**altında **Tanılama ayarları**' nı seçin.

3. **+ Tanılama ayarı Ekle**' yi seçin.

    ![CDN tanılama ayarı](./media/cdn-raw-logs/raw-logs-01.png)

    > [!IMPORTANT]
    > Ham Günlükler yalnızca profil düzeyinde kullanılabilir, ancak toplu http durum kodu günlükleri uç nokta düzeyinde kullanılabilir.

4. **Tanılama ayarları**' nın altında, tanılama **ayarları adı**altında tanılama ayarı için bir ad girin.

5. **Günlüğü** seçin ve bekletme süresini gün olarak ayarlayın.

6. **Hedef ayrıntılarını**seçin. Hedef seçenekleri şunlardır:
    * **Log Analytics’e gönderme**
        * **Aboneliği** ve **Log Analytics çalışma alanını**seçin.
    * **Bir depolama hesabına Arşivle**
        * **Aboneliği** ve **Depolama hesabını**seçin.
    * **Bir olay hub 'ına akış**
        * **Abonelik**, **Olay Hub 'ı ad alanı**, **Olay Hub 'ı adı (isteğe bağlı)** ve **Olay Hub 'ı ilke adı**' nı seçin.

    ![CDN tanılama ayarı](./media/cdn-raw-logs/raw-logs-02.png)

7. **Kaydet**'i seçin.

## <a name="raw-logs-properties"></a>Ham Günlükler özellikleri

Microsoft Service Azure CDN Şu anda ham Günlükler sağlıyor. Ham Günlükler her bir girdiyle aşağıdaki şemaya sahip API istekleri sağlar: 

| Özellik              | Açıklama                                                                                                                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TrackingReference     | Ön kapıya yönelik olarak sunulan ve istemciye X-Azure-ref üst bilgisi olarak gönderilen bir isteği tanımlayan benzersiz başvuru dizesi. Belirli bir istek için erişim günlüklerindeki ayrıntıları aramak için gereklidir. |
| HttpMethod            | İstek tarafından kullanılan HTTP yöntemi.                                                                                                                                                                     |
| HttpVersion           | İstek veya bağlantı türü.                                                                                                                                                                   |
| RequestUri            | Alınan isteğin URI 'SI.                                                                                                                                                                         |
| RequestBytes          | İstek üst bilgileri ve istek gövdesi dahil olmak üzere HTTP isteği iletisinin bayt cinsinden boyutu.                                                                                                   |
| ResponseBytes         | Yanıt olarak arka uç sunucusu tarafından gönderilen bayt.                                                                                                                                                    |
| Kullanıcı             | İstemcinin kullandığı tarayıcı türü.                                                                                                                                                               |
| ClientIp              | İsteği yapan istemcinin IP adresi.                                                                                                                                                  |
| TimeTaken             | İşlemin gerçekleştiği sürenin milisaniye cinsinden uzunluğu.                                                                                                                                            |
| SecurityProtocol      | İstek tarafından kullanılan TLS/SSL protokol sürümü veya şifreleme yoksa null.                                                                                                                           |
| Uç Nokta              | CDN uç noktası Konağı, üst CDN profili altında yapılandırıldı.                                                                                                                                   |
| Arka uç konak adı     | İsteklerin gönderildiği arka uç konağın veya başlangıcının adı.                                                                                                                                |
| Kaynak Shield 'a gönderildi | True ise, isteğin kenar pop 'u yerine kaynak kalkan önbelleğinden yanıtlandığını gösterir. Kaynak kalkan, önbellek isabet oranını artırmak için kullanılan bir üst önbelleğidir.                                       |
| HttpStatusCode        | Proxy 'den döndürülen HTTP durum kodu.                                                                                                                                                        |
| HttpStatusDetails     | İstek üzerine sonuçtaki durum. Bu dize değerinin anlamı, bir durum başvuru tablosunda bulunabilir.                                                                                              |
| Cağımız                   | Kullanıcı isteğine yanıt veren kenar açılır. Pop 'Ların kısaltmaları ilgili Metros Havaalanı kodlarıdır.                                                                                   |
| Önbellek durumu          | Nesnenin önbellekten döndürülüp döndürülmeyeceğini veya kaynaktan geldiğini belirtir.                                                                                                             |
> [!IMPORTANT]
> HTTP ham günlükleri özelliği, **25 şubat 2020**' den sonra oluşturulan veya güncellenen tüm profiller için otomatik olarak kullanılabilir. Daha önce oluşturulan CDN profilleri için, bir tane, günlüğü ayarladıktan sonra CDN uç noktasını güncelleştirmelidir. Örneğin, bir tane, CDN uç noktaları altında coğrafi filtrelemeye gidebilir ve iş yüküyle ilgili olmayan herhangi bir ülkeyi/bölgeyi engelleyebilir ve Kaydet ' i ziyaret edebilir. 

> [!NOTE]
> Günlükler, bir sorgu çalıştırılarak Log Analytics profiliniz altında görüntülenebilir. Örnek bir sorgu AzureDiagnostics gibi görünür WHERE kategorisi = = "AzureCdnAccessLog"

## <a name="next-steps"></a>Sonraki Adımlar
Bu makalede, Microsoft CDN hizmeti için HTTP RAW günlüklerini etkinleştirdiniz.

Azure CDN ve bu makalede bahsedilen diğer Azure hizmetleri hakkında daha fazla bilgi için bkz.:

* [Çözümle](cdn-log-analysis.md) Kullanım desenlerini Azure CDN.

* [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview)hakkında daha fazla bilgi edinin.

* [Azure izleyici 'de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)yapılandırın.
