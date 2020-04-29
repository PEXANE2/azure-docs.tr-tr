---
title: 'Öğretici: Azure CDN için WAF ilkesi oluşturma-Azure portal'
description: Bu öğreticide, Azure portal kullanarak Azure CDN bir Web uygulaması güvenlik duvarı (WAF) ilkesi oluşturmayı öğreneceksiniz.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 03/18/2020
ms.author: victorh
ms.openlocfilehash: 738be1361bfbd944575abceb08781b241336f6e8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79485597"
---
# <a name="tutorial-create-a-waf-policy-on-azure-cdn-using-the-azure-portal"></a>Öğretici: Azure CDN Azure portal kullanarak bir WAF ilkesi oluşturma

Bu öğreticide, temel bir Azure Web uygulaması güvenlik duvarı (WAF) ilkesi oluşturma ve bunu Azure Content Delivery Network (CDN) üzerindeki bir uç noktaya uygulama işlemi gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * WAF ilkesi oluşturma
> * Bunu bir CDN uç noktasıyla ilişkilendir
> * WAF kurallarını yapılandırma

## <a name="prerequisites"></a>Ön koşullar

[Hızlı başlangıç: bir Azure CDN profili ve uç noktası oluşturma](../../cdn/cdn-create-new-endpoint.md)bölümündeki yönergeleri izleyerek bir Azure CDN profili ve uç noktası oluşturun. 

## <a name="create-a-web-application-firewall-policy"></a>Web uygulaması güvenlik duvarı ilkesi oluşturma

İlk olarak, portalı kullanarak yönetilen bir varsayılan kural kümesi (DRS) ile temel bir WAF ilkesi oluşturun.

1. Ekranın sol üst kısmında, **kaynak oluştur** ' u seçin>**WAF** araması yapın>**Web uygulaması güvenlik duvarını** seçin > **Oluştur**' u seçin.
2. **BIR WAF Ilkesi oluştur** sayfasının **temel bilgiler** sekmesinde aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | İlke            |Azure CDN (Önizleme) öğesini seçin.|
    | Abonelik            |Ön kapı abonelik adınızı seçin.|
    | Kaynak grubu          |Ön kapı kaynak grubu adınızı seçin.|
    | İlke adı             |WAF ilkeniz için benzersiz bir ad girin.|

   ![WAF ilkesi oluşturma](../media/waf-cdn-create-portal/basic.png)

3. **BIR WAF Ilkesi oluştur** sayfasının **ILIŞKILENDIRME** sekmesinde, **CDN uç noktası Ekle**' yi seçin, aşağıdaki ayarları girin ve ardından **Ekle**' yi seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | CDN profili              | CDN profili adınızı seçin.|
    | Uç Nokta           | Uç noktanızın adını seçin ve ardından **Ekle**' yi seçin.|
    
    > [!NOTE]
    > Uç nokta bir WAF ilkesiyle ilişkiliyse gri görüntülenir. Önce ilgili ilkeden bitiş noktasını kaldırmalı ve ardından uç noktayı yeni bir WAF ilkesiyle yeniden ilişkilendirmeniz gerekir.
1. **Gözden geçir + oluştur**' u seçin ve **Oluştur**' u seçin.

## <a name="configure-web-application-firewall-policy-optional"></a>Web uygulaması güvenlik duvarı ilkesini yapılandırma (isteğe bağlı)

### <a name="change-mode"></a>Modu Değiştir

Varsayılan olarak WAF ilkesi, bir WAF ilkesi oluştururken *algılama* modundadır. *Algılama* modunda WAF hiçbir isteği engellemez. Bunun yerine, WAF kurallarıyla eşleşen istekler WAF günlüklerinde günlüğe kaydedilir.

WAF 'yi eylemde görmek için, mod ayarlarını *algılamayı* *engelleme*olarak değiştirebilirsiniz. *Önleme* modunda, varsayılan kural KÜMESI (DRS) içinde tanımlanan kurallarla eşleşen istekler engellenir ve WAF günlüklerinde günlüğe kaydedilir.

 ![WAF ilke modunu değiştir](../media/waf-cdn-create-portal/policy.png)

### <a name="custom-rules"></a>Özel kurallar

Özel kural oluşturmak için özel **kurallar** bölümünün altında **özel kural ekle** ' yi seçin. Bu, özel kural yapılandırma sayfasını açar. İki tür özel kural vardır: **eşleştirme kuralı** ve **hız sınırı** kuralı.

Aşağıdaki ekran görüntüsünde, sorgu dizesinde **blok değeri varsa**bir isteği engellemek için özel bir eşleştirme kuralı gösterilmektedir.

![WAF ilke modunu değiştir](../media/waf-cdn-create-portal/custommatch.png)

Hız sınırı kuralları için iki ek alan gerekir: aşağıdaki örnekte gösterildiği gibi **hız sınırı süresi** ve **hız sınırı eşiği (istekler)** :

![WAF ilke modunu değiştir](../media/waf-cdn-create-portal/customrate.png)

### <a name="default-rule-set-drs"></a>Varsayılan kural kümesi (DRS)

Azure yönetilen varsayılan kural kümesi varsayılan olarak etkindir. Bir kural grubundaki tek bir kuralı devre dışı bırakmak için bu kural grubundaki kuralları genişletin, kural numarasının önündeki onay kutusunu seçin ve yukarıdaki sekmede **devre dışı bırak** ' ı seçin. Kural kümesindeki tek tek kuralların eylem türlerini değiştirmek için, kural numarasının önündeki onay kutusunu seçin ve ardından yukarıdaki **eylemi Değiştir** sekmesini seçin.

 ![WAF kural kümesini Değiştir](../media/waf-cdn-create-portal/managed2.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Web uygulaması güvenlik duvarı hakkında bilgi edinin](../overview.md)
