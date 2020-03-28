---
title: 'Öğretici: Azure Ön Kapı - Azure portalı için WAF ilkesi oluşturun'
description: Bu eğitimde, Azure portalını kullanarak Bir Web Uygulama Güvenlik Duvarı (WAF) ilkesioluşturmayı öğrenirsiniz.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 03/10/2020
ms.author: victorh
ms.openlocfilehash: be66a93ea4a518b26d973d222caf58e73b6986a3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79475850"
---
# <a name="tutorial-create-a-web-application-firewall-policy-on-azure-front-door-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak Azure Ön Kapı'da Bir Web Uygulaması Güvenlik Duvarı ilkesi oluşturun

Bu öğretici, temel bir Azure Web Uygulaması Güvenlik Duvarı (WAF) ilkesini nasıl oluşturacağınızı ve Azure Ön Kapı'daki bir ön uç ana bilgisayara nasıl uygulayacağınızı gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * WAF ilkesi oluşturma
> * Ön uç ana bilgisayarla ilişkilendirin
> * WAF kurallarını yapılandırma

## <a name="prerequisites"></a>Ön koşullar

[Quickstart: Ön Kapı profili](../../frontdoor/quickstart-create-front-door.md)oluşturma'da açıklanan yönergeleri izleyerek bir Ön Kapı profili oluşturun. 

## <a name="create-a-web-application-firewall-policy"></a>Web Uygulaması Güvenlik Duvarı ilkesi oluşturma

İlk olarak, portalı kullanarak yönetilen Varsayılan Kural Kümesi (DRS) ile temel bir WAF ilkesi oluşturun. 

1. Ekranın sol üst tarafında, **WAF** için **kaynak>** arama oluşturma>Web uygulaması güvenlik **duvarı (Önizleme)** > **Oluştur'u**seçin.
2. **WAF ilke** sayfasının **Temeller** sekmesinde, aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılanları kabul edin ve ardından Gözden Geçir + oluştur seçeneğini **belirleyin:**

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik            |Ön Kapı abonelik adınızı seçin.|
    | Kaynak grubu          |Ön Kapı kaynak grup adınızı seçin.|
    | İlke adı             |WAF politikanız için benzersiz bir ad girin.|

   ![WAF ilkesi oluşturma](../media/waf-front-door-create-portal/basic.png)

3. **WAF ilkesi oluştur** sayfasının **Ilişkilendirme** sekmesinde, **ön uç ana bilgisayar Ekle'yi**seçin, aşağıdaki ayarları girin ve sonra **Ekle'yi**seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Ön kapı              | Ön Kapı profil adınızı seçin.|
    | Ön uç ana bilgisayar           | Ön kapı ana host'unuzun adını seçin ve sonra **Ekle'yi**seçin.|
    
    > [!NOTE]
    > Ön uç ana bilgisayar bir WAF ilkesiyle ilişkiliyse, gri renkte olarak gösterilir. Önce ön yüz ana bilgisayarını ilişkili ilkeden kaldırmanız ve ardından ön yüz ana bilgisayarını yeni bir WAF ilkesiyle yeniden ilişkilendirmeniz gerekir.
1. **Gözden Geçir + oluştur'u**seçin, ardından **Oluştur'u**seçin.

## <a name="configure-web-application-firewall-rules-optional"></a>Web Uygulaması Güvenlik Duvarı kurallarını yapılandırma (isteğe bağlı)

### <a name="change-mode"></a>Modu değiştir

Bir WAF ilkesi oluşturduğunuzda, varsayılan WAF ilkesi **Algılama** modundadır. **Algılama** modunda, WAF herhangi bir isteği engellemez, bunun yerine, WAF kurallarıyla eşleşen istekler WAF günlüklerinde günlüğe kaydedilir.
WAF'ı iş başında görmek için mod ayarlarını **Algılama'dan** **Önleme'ye**değiştirebilirsiniz. **Önleme** modunda, Varsayılan Kural Kümesi'nde (DRS) tanımlanan kuralları eşleştirebilen istekler engellenir ve WAF günlüklerinde günlüğe kaydedilir.

 ![WAF ilke modunu değiştirme](../media/waf-front-door-create-portal/policy.png)

### <a name="custom-rules"></a>Özel kurallar

**Özel kurallar** bölümünde **özel kural ekle'yi** seçerek özel bir kural oluşturabilirsiniz. Bu, özel kural yapılandırma sayfasını başlatir. Aşağıda, sorgu dizesi **blokme**içeriyorsa, isteği engellemek için özel bir kural yapılandırma örneği verilmiştir.

![WAF ilke modunu değiştirme](../media/waf-front-door-create-portal/customquerystring2.png)

### <a name="default-rule-set-drs"></a>Varsayılan Kural Kümesi (DRS)

Azure tarafından yönetilen Varsayılan Kural Kümesi varsayılan olarak etkinleştirilir. Kural grubu içindeki tek bir kuralı devre dışı bırakıp, bu kural grubu içindeki kuralları genişletin, kural numarasının önündeki **onay kutusunu** seçin ve yukarıdaki sekmede Devre **Dışı Nı** seçin. Kural kümesiiçindeki tek tek kurallar için eylem türlerini değiştirmek için, kural numarasının önündeki onay kutusunu seçin ve ardından yukarıdaki eylem sekmesini **değiştir'i** seçin.

 ![WAF Kural Kümesini Değiştir](../media/waf-front-door-create-portal/managed2.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Web Uygulaması Güvenlik Duvarı](../overview.md)
> [hakkında bilgi edinin Azure Ön Kapı hakkında daha fazla bilgi edinin](../../frontdoor/front-door-overview.md)