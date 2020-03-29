---
title: Azure CDN gerçek zamanlı uyarılar | Microsoft Dokümanlar
description: Microsoft Azure CDN'de gerçek zamanlı uyarılar. Gerçek zamanlı uyarılar, CDN profilinizdeki uç noktaların performansı hakkında bildirimler sağlar.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 1e85b809-e1a9-4473-b835-69d1b4ed3393
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 4b8cbc27757cf6c321ea4b3c27720a129aa27c1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593471"
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Microsoft Azure CDN'de gerçek zamanlı uyarılar
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış
Bu belge, Microsoft Azure CDN'deki gerçek zamanlı uyarıları açıklar. Bu işlevsellik, CDN profilinizdeki uç noktaların performansı hakkında gerçek zamanlı bildirimler sağlar.  E-posta veya HTTP uyarıları şu temellere göre ayarlayabilirsiniz:

* Bant genişliği
* Durum Kodları
* Önbellek Durumları
* Bağlantılar

## <a name="creating-a-real-time-alert"></a>Gerçek zamanlı uyarı oluşturma
1. Azure [portalında](https://portal.azure.com)CDN profilinize göz atın.
   
    ![CDN profili](./media/cdn-real-time-alerts/cdn-profile-blade.png)
1. CDN profil bıçağından **Yönet** düğmesini tıklatın.
   
    ![CDN profil yönetme düğmesi](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
3. **Analytics** sekmesinin üzerine binin, ardından **Gerçek Zamanlı İstatistikler** uçuşunun üzerinde gezin.  Gerçek **Zamanlı Uyarılar'a**tıklayın.
   
    ![CDN yönetim portalı](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    Varolan uyarı yapılandırmalarının listesi (varsa) görüntülenir.
4. Uyarı **Ekle** düğmesini tıklatın.
   
    ![Uyarı ekle düğmesi](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    Yeni bir uyarı oluşturmak için bir form görüntülenir.
   
    ![Yeni Uyarı formu](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. Kaydet'i tıklattığınızda bu **Save**uyarının etkin olmasını istiyorsanız, **Alert Enabled** onay kutusunu işaretleyin.
6. **Ad** alanına uyarınız için açıklayıcı bir ad girin.
7. Media **Type** açılır düşüşünde **HTTP Büyük Nesne'yi**seçin.
   
    ![HTTP Büyük Nesnesi seçili Ortam Türü](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > **Medya Türü**olarak HTTP **Büyük Nesne'yi** seçmeniz gerekir.  Diğer seçenekler **Verizon Azure CDN**tarafından kullanılmaz.  **HTTP Büyük Nesne'nin** seçilememesi, uyarınızın hiçbir zaman tetiklenmemesine neden olur.
   > 
   > 
8. **Metrik**, **Işleç**ve **Tetik değeri**seçerek izlemek için bir **İfade** oluşturun.
   
   * **Metrik**için, izlenmesini istediğiniz koşul türünü seçin.  **Bant genişliği Mbps** saniyede megabit bant genişliği kullanım miktarıdır.  **Toplam Bağlantılar,** kenar sunucularımıza eşzamanlı HTTP bağlantılarının sayısıdır.  Çeşitli önbellek durumları ve durum kodlarının tanımları için [Azure CDN Önbellek Durum Kodları](/previous-versions/azure/mt759237(v=azure.100)) ve Azure [CDN HTTP Durum Kodları'na](/previous-versions/azure/mt759238(v=azure.100)) bakın
   * **İşleç,** metrik ve tetikleyici değer arasındaki ilişkiyi kuran matematiksel işleçtir.
   * **Tetikleyici Değeri,** bildirim gönderilmeden önce karşılanması gereken eşik değerdir.
     
     Aşağıdaki örnekte, oluşturulan ifade, 404 durum kodu sayısı 25'ten büyük olduğunda bir bildirim gönderildiğini gösterir.
     
     ![Gerçek zamanlı uyarı örnek ifadesi](./media/cdn-real-time-alerts/cdn-expression.png)
9. **Interval**için, ifadenin ne sıklıkta değerlendirilmesini istediğinizi girin.
10. Açılan **bildirimde,** ifade nin doğru olduğunda ne zaman bilgilendirilmek istediğinizi seçin.
    
    * **Koşul Başlangıç,** belirtilen koşul ilk algılandığında bir bildirimgönderildiğini gösterir.
    * **Durum Sonu,** belirtilen koşul artık algılanmadıklarında bir bildirim gönderildiğini gösterir. Bu bildirim, yalnızca ağ izleme sistemimizbelirtilen koşulun oluştuğunu algıladıktan sonra tetiklenebilir.
    * **Sürekli,** ağ izleme sistemi belirtilen koşulu algılayan her bildirimin gönderildiğini gösterir. Ağ izleme sisteminin belirtilen koşul için aralık başına yalnızca bir kez denetler unutmayın.
    * **Koşul Başlangıç ve Bitiş,** belirtilen koşulun ilk kez algılandığında ve koşul artık algılanmadığınızda bir kez daha bir bildirim gönderildiğini gösterir.
1. Bildirimleri e-posta ile almak istiyorsanız, **E-posta yla Bildir** onay kutusunu işaretleyin.  
    
    ![E-posta formu ile bildirin](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    **To** alanına, bildirimlerin gönderilmesini istediğiniz e-posta adresini girin. **Konu** ve **Gövde**için varsayılanı bırakabilirsiniz veya ileti gönderildiğinde dinamik olarak uyarı verilerini eklemek için Kullanılabilir **anahtar kelimeler** listesini kullanarak iletiyi özelleştirebilirsiniz.
    
    > [!NOTE]
    > E-posta bildirimini **Test Bildirimi** düğmesini tıklatarak, ancak uyarı yapılandırması kaydedildikten sonra sınayabilirsiniz.
    > 
    > 
12. Bildirimlerin bir web sunucusuna nakledilmesini istiyorsanız, HTTP Post onay kutusunu **bildir'i** kontrol edin.
    
    ![HTTP Post formu ile bildirin](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    **Url** alanına, HTTP iletisinin yayınlanmasını istediğiniz URL'yi girin. **Üstbilgi** textbox'ına, istekte gönderilmek üzere HTTP üstbilgisini girin.  **Body**için, ileti gönderildiğinde dinamik olarak uyarı verilerini eklemek için **Kullanılabilir anahtar kelimeler** listesini kullanarak iletiyi özelleştirebilirsiniz.  **Üstbilgi** ve **Gövde,** aşağıdaki örneğe benzer bir XML yüküne varsayılan dır:
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > HTTP Post bildirimini **Test Bildirimi** düğmesini tıklatarak, ancak uyarı yapılandırması kaydedildikten sonra test edebilirsiniz.
    > 
    > 
13. Uyarı yapılandırmanızı kaydetmek için **Kaydet** düğmesini tıklatın.  Adım 5'te **Alert Enabled'ı** işaretlediyseniz, uyarınız artık etkindir.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure CDN'de gerçek zamanlı istatistikleri](cdn-real-time-stats.md) analiz edin
* [Gelişmiş HTTP raporlarıyla](cdn-advanced-http-reports.md) daha derine inin
* [Kullanım modellerini](cdn-analyze-usage-patterns.md) analiz edin

