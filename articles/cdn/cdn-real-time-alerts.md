---
title: Gerçek zamanlı uyarılar Azure CDN | Microsoft Docs
description: Microsoft Azure CDN 'de gerçek zamanlı uyarılar. Gerçek zamanlı uyarılar, CDN profilinizde uç noktaların performansı hakkında bildirimler sağlar.
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
ms.topic: how-to
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6811a06eb3483fd53b6e566033935c3b2e00ceca
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84887255"
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Microsoft Azure CDN 'de gerçek zamanlı uyarılar
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış
Bu belgede Microsoft Azure CDN 'de gerçek zamanlı uyarılar açıklanmaktadır. Bu işlev, CDN profilinizde uç noktaların performansı hakkında gerçek zamanlı bildirimler sağlar.  E-posta veya HTTP uyarılarını şunları temel alarak ayarlayabilirsiniz:

* Bant genişliği
* Durum Kodları
* Önbellek durumları
* Bağlantılar

## <a name="creating-a-real-time-alert"></a>Gerçek zamanlı uyarı oluşturma
1. [Azure Portal](https://portal.azure.com), CDN profilinize gidin.
   
    ![CDN profili](./media/cdn-real-time-alerts/cdn-profile-blade.png)
1. CDN profili dikey penceresinde **Yönet** düğmesine tıklayın.
   
    ![CDN profili Yönet düğmesi](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
3. **Analiz** sekmesinin üzerine gelin ve ardından **gerçek zamanlı istatistikler** açılır öğesi üzerine gelin.  Gerçek zamanlı **Uyarılar**' a tıklayın.
   
    ![CDN yönetim portalı](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    Mevcut uyarı yapılandırmalarının (varsa) listesi görüntülenir.
4. **Uyarı Ekle** düğmesine tıklayın.
   
    ![Uyarı Ekle düğmesi](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    Yeni bir uyarı oluşturmak için bir form görüntülenir.
   
    ![Yeni uyarı formu](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. **Kaydet**' e tıkladığınızda bu uyarının etkin olmasını Istiyorsanız, **uyarı etkin** onay kutusunu işaretleyin.
6. **Ad** alanına Uyarınız için açıklayıcı bir ad girin.
7. **Medya türü** açılan listesinde **http büyük nesne**' yi seçin.
   
    ![HTTP büyük nesne seçiliyken Medya türü](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > **Medya türü**olarak **http büyük nesne** ' yi seçmeniz gerekir.  Diğer seçimler, **Verizon ' dan Azure CDN**tarafından kullanılmaz.  **Http büyük nesne** seçme hatası, uyarıınızın hiçbir şekilde tetiklenmesine neden olur.
   > 
   > 
8. Bir **ölçüm**, **işleç**ve **Tetikleyici değeri**seçerek izlenecek bir **ifade** oluşturun.
   
   * **Ölçüm**için, izlenmesini istediğiniz koşul türünü seçin.  **Bant genişliği Mbps** , saniye başına megabit cinsinden bant genişliği kullanım miktarıdır.  **Toplam bağlantı** sayısı, uç sunucularımızla aynı anda http bağlantısı sayısıdır.  Çeşitli önbellek durumlarının ve durum kodlarının tanımları için bkz. [Azure CDN önbellek durum kodları](/previous-versions/azure/mt759237(v=azure.100)) ve [Azure CDN http durum kodları](/previous-versions/azure/mt759238(v=azure.100))
   * **İşleç** , ölçüm ve tetikleyici değeri arasındaki ilişkiyi kuran matematik işleçtir.
   * **Tetikleyici değeri** , bir bildirim gönderilmeden önce karşılanması gereken eşik değeridir.
     
     Aşağıdaki örnekte, oluşturulan ifade 404 durum kodunun sayısı 25 ' ten büyük olduğunda bir bildirimin gönderileceğini gösterir.
     
     ![Gerçek zamanlı uyarı örnek ifadesi](./media/cdn-real-time-alerts/cdn-expression.png)
9. **Aralık**için, ifadenin ne sıklıkta değerlendirileceğini girin.
10. **Üzerine bildir** açılır listesinde, ifade doğru olduğunda ne zaman bildirilmesini istediğinizi seçin.
    
    * **Koşul başlangıcı** , belirtilen koşul ilk algılandığında bir bildirimin gönderildiğini gösterir.
    * **Koşul sonu** , belirtilen koşul artık algılanmadığında bir bildirimin gönderileceğini belirtir. Bu bildirim yalnızca ağ izleme sistemimizin belirtilen koşulun oluştuğunu algıladıktan sonra tetiklenebilir.
    * **Sürekli** , ağ izleme sisteminin belirtilen koşulu algıladığı her seferinde bir bildirimin gönderileceğini belirtir. Ağ izleme sisteminin belirtilen koşul için her aralığa yalnızca bir kez denetleyeceğini göz önünde bulundurun.
    * **Koşul başlangıcı ve sonu** , belirtilen koşulun tespit edildiğinde ilk kez bir bildirimin gönderildiğini ve koşulun artık algılanmadığında bir kez daha olduğunu gösterir.
1. E-posta ile bildirim almak istiyorsanız, **e-postaya göre bildir** onay kutusunu işaretleyin.  
    
    ![E-posta formuna bildir](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    **Kime** alanına, bildirimlerin gönderilmesini istediğiniz e-posta adresini girin. **Konu** ve **gövde**için, varsayılan olarak bırakabilirsiniz veya ileti gönderildiğinde uyarı verilerini dinamik olarak eklemek için **kullanılabilir anahtar sözcükler** listesini kullanarak iletiyi özelleştirebilirsiniz.
    
    > [!NOTE]
    > **Test bildirimi** düğmesine tıklayarak, ancak yalnızca uyarı yapılandırması kaydedildikten sonra e-posta bildirimini test edebilirsiniz.
    > 
    > 
12. Bildirimlerin bir Web sunucusuna gönderilmesini istiyorsanız **http post 'a göre bildir** onay kutusunu işaretleyin.
    
    ![HTTP POST formu ile bildir](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    **URL** ALANıNA, HTTP iletisinin gönderilmesini istediğiniz URL 'yi girin. **Üstbilgiler** metin kutusunda, ISTEKTE gönderilecek http üst bilgilerini girin.  **Gövde**için, ileti gönderildiğinde uyarı verilerini dinamik olarak eklemek için **kullanılabilir anahtar sözcükler** listesini kullanarak iletiyi özelleştirebilirsiniz.  **Üst bilgiler** ve **gövde** varsayılan olarak AŞAĞıDAKI örneğe benzer bir XML yüküne sahiptir:
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > **Test bildirimi** DÜĞMESINE tıklayarak http post bildirimini test edebilirsiniz, ancak yalnızca uyarı yapılandırması kaydedildikten sonra.
    > 
    > 
13. Uyarı yapılandırmanızı kaydetmek için **Kaydet** düğmesine tıklayın.  5. adımda **Uyarı etkinleştirildiyse** , uyarınız artık etkin olur.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure CDN 'Daki gerçek zamanlı istatistikleri](cdn-real-time-stats.md) analiz etme
* [GELIŞMIŞ http raporları](cdn-advanced-http-reports.md) ile daha derin bilgi
* [Kullanım düzenlerini](cdn-analyze-usage-patterns.md) çözümleme

