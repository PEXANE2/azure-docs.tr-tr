---
title: 'Öğretici: Azure ön kapısı için WAF ilkesi oluşturma-Azure portal'
description: Bu öğreticide, Azure portal kullanarak bir Web uygulaması güvenlik duvarı (WAF) ilkesi oluşturmayı öğreneceksiniz.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 02/18/2021
ms.author: victorh
ms.openlocfilehash: 8b1d1007e817bafe3d75f0f0d7c3fc6eb5470854
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101729479"
---
# <a name="tutorial-create-a-web-application-firewall-policy-on-azure-front-door-using-the-azure-portal"></a>Öğretici: Azure ön kapıda Azure portal kullanarak Web uygulaması güvenlik duvarı ilkesi oluşturma

Bu öğreticide, temel bir Azure Web uygulaması güvenlik duvarı (WAF) ilkesi oluşturma ve Azure ön kapısının bir ön uç konağına uygulama işlemi gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * WAF ilkesi oluşturma
> * Ön uç konakla ilişkilendirin
> * WAF kurallarını yapılandırma

## <a name="prerequisites"></a>Önkoşullar

[Hızlı başlangıç: ön kapı profili oluşturma](../../frontdoor/quickstart-create-front-door.md)bölümünde açıklanan yönergeleri Izleyerek bir ön kapı profili oluşturun. 

## <a name="create-a-web-application-firewall-policy"></a>Web uygulaması güvenlik duvarı ilkesi oluşturma

İlk olarak, portalı kullanarak yönetilen varsayılan kural kümesi (DRS) ile temel bir WAF ilkesi oluşturun. 

1. Ekranın sol üst kısmında, **kaynak oluştur** ' u seçin>**WAF** araması yapın>**Web uygulaması güvenlik duvarı (Önizleme)** seçin > **Oluştur**' u seçin.
2. **BIR WAF Ilkesi oluştur** sayfasının **temel bilgiler** sekmesinde aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik            |Ön kapı abonelik adınızı seçin.|
    | Kaynak grubu          |Ön kapı kaynak grubu adınızı seçin.|
    | İlke adı             |WAF ilkeniz için benzersiz bir ad girin.|

   :::image type="content" source="../media/waf-front-door-create-portal/basic.png" alt-text="Abonelik, kaynak grubu ve ilke adı için Inceleme + oluştur düğmesi ve liste kutuları içeren bir W A F ilkesi oluşturma sayfasının ekran görüntüsü." border="false":::

3. **BIR WAF Ilkesi oluştur** sayfasının **ilişkilendirme** sekmesinde, **ön uç Konağı Ekle**' yi seçin, aşağıdaki ayarları girin ve ardından **Ekle**' yi seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Ön kapı              | Ön kapı profili adınızı seçin.|
    | Ön uç Konağı           | Ön kapı ana bilgisayarın adını ve ardından **Ekle**' yi seçin.|
    
    > [!NOTE]
    > Ön uç Konağı bir WAF ilkesiyle ilişkiliyse, gri renkte gösterilir. Önce ön uç konağını ilişkili ilkeden kaldırmalı ve ardından ön uç konağını yeni bir WAF ilkesiyle yeniden ilişkilendirmeniz gerekir.
1. **Gözden Geçir + oluştur**’u ve sonra da **Oluştur**’u seçin.

## <a name="configure-web-application-firewall-rules-optional"></a>Web uygulaması güvenlik duvarı kurallarını yapılandırma (isteğe bağlı)

### <a name="change-mode"></a>Modu Değiştir

Bir WAF ilkesi oluşturduğunuzda, varsayılan WAF ilkesi **algılama** modundadır. **Algılama** modunda WAF herhangi bir isteği engellemez, bunun yerine WAF kurallarıyla eşleşen istekler WAF günlüklerinde günlüğe kaydedilir.
WAF 'yi eylemde görmek için, mod ayarlarını **algılamayı** **engelleme** olarak değiştirebilirsiniz. **Önleme** modunda, varsayılan kural KÜMESI (DRS) içinde tanımlanan kurallarla eşleşen istekler engellenir ve WAF günlüklerinde günlüğe kaydedilir.

 :::image type="content" source="../media/waf-front-door-create-portal/policy.png" alt-text="Ilke ayarları bölümünün ekran görüntüsü. Mod değiştirme, önleme olarak ayarlanır." border="false":::

### <a name="custom-rules"></a>Özel kurallar

Özel **kurallar** bölümünün altında **özel kural ekle** seçeneğini belirleyerek özel bir kural oluşturabilirsiniz. Bu, özel kural yapılandırma sayfasını başlatır. Sorgu dizesi **blok içeriyorsa,** bir isteği engellemek için özel bir kural yapılandırmanın bir örneği aşağıda verilmiştir.

:::image type="content" source="../media/waf-front-door-create-portal/customquerystring2.png" alt-text="QueryString değişkeninin değeri blok değerini içerip içermediğini denetleyen özel kural yapılandırma sayfasının ekran görüntüsü." border="false":::

### <a name="default-rule-set-drs"></a>Varsayılan kural kümesi (DRS)

Azure tarafından yönetilen varsayılan kural kümesi varsayılan olarak etkindir. Geçerli varsayılan sürüm DefaultRuleSet_1.0 ' dır. WAF tarafından **yönetilen kurallardan**, **ata**, son kullanılabilir RuleSet Microsoft_DefaultRuleSet_1 1. aşağı açılan listede kullanılabilir.

Bir kural grubundaki tek bir kuralı devre dışı bırakmak için bu kural grubundaki kuralları genişletin, kural numarasının önündeki **onay kutusunu** seçin ve yukarıdaki sekmede **devre dışı bırak** ' ı seçin. Kural kümesindeki tek tek kuralların eylem türlerini değiştirmek için, kural numarasının önündeki onay kutusunu seçin ve ardından yukarıdaki **eylemi Değiştir** sekmesini seçin.

 :::image type="content" source="../media/waf-front-door-create-portal/managed2.png" alt-text="Bir kural kümesi, kural grupları, kurallar ve Etkinleştir, devre dışı bırak ve eylemi Değiştir düğmelerini gösteren yönetilen kurallar sayfasının ekran görüntüsü. Bir kural denetlenir." border="false":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, kaynak grubunu ve tüm ilgili kaynakları kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure ön kapısı hakkında daha fazla bilgi edinin](../../frontdoor/front-door-overview.md)
