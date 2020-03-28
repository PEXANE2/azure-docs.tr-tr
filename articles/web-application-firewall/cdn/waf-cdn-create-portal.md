---
title: 'Öğretici: Azure CDN için WAF ilkesi oluşturma - Azure portalı'
description: Bu eğitimde, Azure portalını kullanarak Azure CDN'de Bir Web Uygulama Güvenlik Duvarı (WAF) ilkesi oluşturmayı öğrenirsiniz.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 03/18/2020
ms.author: victorh
ms.openlocfilehash: 738be1361bfbd944575abceb08781b241336f6e8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79485597"
---
# <a name="tutorial-create-a-waf-policy-on-azure-cdn-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak Azure CDN'de BIR WAF ilkesi oluşturun

Bu öğretici, temel bir Azure Web Uygulaması Güvenlik Duvarı (WAF) ilkesini nasıl oluşturacağınızı ve azure İçerik Teslim Ağı'nda (CDN) bir bitiş noktasına nasıl uygulayacağınızı gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * WAF ilkesi oluşturma
> * CDN bitiş noktasıyla ilişkilendirme
> * WAF kurallarını yapılandırma

## <a name="prerequisites"></a>Ön koşullar

[Quickstart: Azure CDN profili ve bitiş noktası](../../cdn/cdn-create-new-endpoint.md)oluşturma yönergelerini izleyerek bir Azure CDN profili ve bitiş noktası oluşturun. 

## <a name="create-a-web-application-firewall-policy"></a>Web Uygulaması Güvenlik Duvarı ilkesi oluşturma

İlk olarak, portalı kullanarak yönetilen varsayılan kural kümesi (DRS) ile temel bir WAF ilkesi oluşturun.

1. Ekranın sol üst tarafında, **WAF** için kaynak>arama **oluşturma**>**Web uygulaması güvenlik duvarını** seçin > **Oluştur'u**seçin.
2. **WAF ilke** sayfasının **Temeller** sekmesinde, aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılanları kabul edin ve ardından Gözden Geçir + oluştur seçeneğini **belirleyin:**

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Için İlke            |Azure CDN (Önizleme) seçeneğini belirleyin.|
    | Abonelik            |Ön Kapı abonelik adınızı seçin.|
    | Kaynak grubu          |Ön Kapı kaynak grup adınızı seçin.|
    | İlke adı             |WAF politikanız için benzersiz bir ad girin.|

   ![WAF ilkesi oluşturma](../media/waf-cdn-create-portal/basic.png)

3. **WAF ilkesi oluştur** sayfasının **Ilişkilendirme** sekmesinde **CDN Bitiş Noktası Ekle'yi**seçin, aşağıdaki ayarları girin ve sonra **Ekle'yi**seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | CDN Profili              | CDN profil adınızı seçin.|
    | Uç Nokta           | Bitiş noktanızın adını seçin, ardından **Ekle'yi**seçin.|
    
    > [!NOTE]
    > Bitiş noktası bir WAF ilkesiyle ilişkiliyse, gri renkte gösterilir. Önce Bitiş Noktasını ilişkili ilkeden kaldırmanız ve ardından bitiş noktasını yeni bir WAF ilkesiyle yeniden ilişkilendirmeniz gerekir.
1. **Gözden Geçir + oluştur'u**seçin, ardından **Oluştur'u**seçin.

## <a name="configure-web-application-firewall-policy-optional"></a>Web Uygulaması Güvenlik Duvarı ilkesini yapılandırma (isteğe bağlı)

### <a name="change-mode"></a>Modu değiştir

Varsayılan olarak WAF ilkesi, bir WAF ilkesi oluşturduğunuzda *Algılama* modundadır. *Algılama* modunda, WAF herhangi bir isteği engellemez. Bunun yerine, WAF kurallarıyla eşleşen istekler WAF günlüklerinde günlüğe kaydedilir.

WAF'ı iş başında görmek için mod ayarlarını *Algılama'dan* *Önleme'ye*değiştirebilirsiniz. *Önleme* modunda, Varsayılan Kural Kümesi'nde (DRS) tanımlanan kuralları eşleştirebilen istekler engellenir ve WAF günlüklerinde günlüğe kaydedilir.

 ![WAF ilke modunu değiştirme](../media/waf-cdn-create-portal/policy.png)

### <a name="custom-rules"></a>Özel kurallar

Özel bir kural oluşturmak **için, Özel kurallar** bölümüne **özel kural ekle'yi** seçin. Bu, özel kural yapılandırma sayfasını açar. İki tür özel kural vardır: **eşleç kuralı** ve **oran sınırı** kuralı.

Aşağıdaki ekran görüntüsü, sorgu dizesi değer **blokme**içeriyorsa isteği engellemek için özel bir eşleme kuralı nı gösterir.

![WAF ilke modunu değiştirme](../media/waf-cdn-create-portal/custommatch.png)

Oran sınırı kuralları iki ek alan gerektirir: Aşağıdaki örnekte gösterildiği gibi **oran sınırı süresi** ve Oran sınır **eşiği (istekleri):**

![WAF ilke modunu değiştirme](../media/waf-cdn-create-portal/customrate.png)

### <a name="default-rule-set-drs"></a>Varsayılan Kural Kümesi (DRS)

Azure yönetilen Varsayılan Kural Kümesi varsayılan olarak etkinleştirilir. Kural grubu içindeki tek bir kuralı devre dışı bırakıp, bu kural grubu içindeki kuralları genişletin, kural numarasının önündeki onay kutusunu seçin ve yukarıdaki sekmede **Devre Dışı Nı** seçin. Kural kümesiiçindeki tek tek kurallar için eylem türlerini değiştirmek için, kural numarasının önündeki onay kutusunu seçin ve ardından yukarıdaki eylem sekmesini **değiştir'i** seçin.

 ![WAF Kural Kümesini Değiştir](../media/waf-cdn-create-portal/managed2.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Web Uygulaması Güvenlik Duvarı hakkında bilgi edinin](../overview.md)
