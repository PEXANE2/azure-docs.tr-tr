---
title: Çok değerli trafik yönlendirmeyi yapılandırma - Azure Trafik Yöneticisi
description: Bu makalede, Trafik Yöneticisi'nin trafiği A/AAAA uç noktalarına yönlendirmek için nasıl yapılandırılabildiğini açıklanmaktadır.
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: rohink
ms.openlocfilehash: daf7d09916d276130e337f7acea738228ee23707
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938772"
---
# <a name="configure-multivalue-routing-method-in-traffic-manager"></a>Trafik Yöneticisi'nde Çok Değerli yönlendirme yöntemini yapılandırma

Bu makalede, MultiValue trafik yönlendirme yönteminin nasıl yapılandırılabildiğini açıklanmaktadır. **Çok değerli** trafik yönlendirme yöntemi, birden çok sağlıklı uç noktayı döndürmenize olanak tanır ve istemcilerin başka bir DNS araması yapmak zorunda kalmadan yeniden deneme seçenekleri olduğundan uygulamanızın güvenilirliğini artırmaya yardımcı olur. MultiValue yönlendirme, yalnızca IPv4 veya IPv6 adresleri kullanılarak tüm uç noktaları belirtilen profiller için etkinleştirilir. Bu profil için bir sorgu alındığı zaman, tüm sağlıklı uç noktalar belirtilen yapılandırılabilir maksimum iade sayısına göre döndürülür. 

>[!NOTE]
> Şu anda IPv4 veya IPv6 adreslerini kullanarak uç noktaları eklemek yalnızca **Dış** türün uç noktaları için desteklenir ve dolayısıyla MultiValue yönlendirmesi yalnızca bu uç noktalar için desteklenir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Trafik Yöneticisi profili için bir kaynak grubu oluşturun.
1. Azure portalının sol bölmesine **Kaynak gruplarını**seçin.
2. **Kaynak gruplarında,** sayfanın üst kısmında **Ekle'yi**seçin.
3. **Kaynak grup adına,** *myResourceGroupTM1*adında bir ad yazın. **Kaynak grubu konumu**için Doğu **ABD'yi**seçin ve ardından **Tamam'ı**seçin.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma
En düşük gecikme ile bitiş noktasına göndererek kullanıcı trafiğini yönlendiren bir Trafik Yöneticisi profili oluşturun.

1. **Networking** > Ekranın sol üst tarafında, **kaynak** > Oluştur**Trafik Yöneticisi profili** > **Oluştur'u**seçin.
2. **Trafik Yöneticisi Oluştur profilinde,** girin veya seçin, aşağıdaki bilgileri, kalan ayarlar için varsayılanları kabul edin ve sonra **Oluştur'u**seçin:
    
    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Adı                   | Bu adın trafficmanager.net bölgesinde benzersiz olması ve Traffic Manager profilinize erişmek için kullanılan trafficmanager.net DNS adı ile sonuçlanması gerekir.                                   |
    | Yönlendirme yöntemi          | Çok **değerli** yönlendirme yöntemini seçin.                                       |
    | Abonelik            | Aboneliğinizi seçin.                          |
    | Kaynak grubu          | *MyResourceGroupTM1'i*seçin. |
    | Konum                | Bu ayar, kaynak grubunun konumunu ifade eder ve genel olarak dağıtılacak Traffic Manager profilini etkilemez.                              |
   |        |           | 
  
   ![Traffic Manager profili oluşturma](./media/traffic-manager-multivalue-routing-method/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

Önceki adımda oluşturduğunuz MultiValue Traffic Manager profiline harici uç nokta olarak iki IP adresi ekleyin.

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın ve görüntülenen sonuçların arasından bu profili seçin.
2. **Traffic Manager profili** sayfasının **Ayarlar** bölümünde **Uç noktalar**'a ve ardından **Ekle**'ye tıklayın.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Tür                    | Dış uç nokta                                   |
    | Adı           | myEndpoint1                                        |
    | Tam nitelikli alan adı (FQDN) veya IP           | Bu Trafik Yöneticisi profiline eklemek istediğiniz bitiş noktasının Genel IP adresini yazın                         |
    |        |           |

4. **Tam nitelikli etki alanı adı (FQDN) veya IP**için *myEndpoint2*adlı başka bir bitiş noktası eklemek için 2 ve 3 adımlarını yineleyin, ikinci bitiş noktasının genel IP adresini girin.
5. Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte **Traffic Manager profili** bölümünde gösterilir.

   ![Traffic Manager uç noktası ekleme](./media/traffic-manager-multivalue-routing-method/add-endpoint.png)
 
## <a name="next-steps"></a>Sonraki adımlar

- [Ağırlıklı trafik yönlendirme yöntemi](traffic-manager-configure-weighted-routing-method.md) hakkında bilgi edinin.
- [Öncelik yönlendirme yöntemi](traffic-manager-configure-priority-routing-method.md) hakkında bilgi edinin.
- [Performans yönlendirme yöntemi](traffic-manager-configure-performance-routing-method.md) hakkında daha fazla bilgi edinin
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.


