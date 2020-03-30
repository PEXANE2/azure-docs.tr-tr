---
title: Azure Uygulama Ağ Geçidi özel hata sayfaları oluşturma
description: Bu makalede, Uygulama Ağ Geçidi özel hata sayfalarını nasıl oluşturabileceğiniz gösterilmektedir. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/16/2019
ms.author: victorh
ms.openlocfilehash: ff11f686287498fe12b31d15a630178bb12035ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74129872"
---
# <a name="create-application-gateway-custom-error-pages"></a>Uygulama Ağ Geçidi özel hata sayfaları oluşturma

Application Gateway, varsayılan hata sayfalarını göstermek yerine özel hata sayfaları oluşturmanızı sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz.

Örneğin, web uygulamanıza erişilemiyorsa kendi bakım sayfanızı tanımlayabilirsiniz. Veya, kötü amaçlı bir istek bir web uygulamasına gönderilirse yetkisiz bir erişim sayfası oluşturabilirsiniz.

Özel hata sayfaları aşağıdaki iki senaryo için desteklenir:

- **Bakım sayfası** - Bu özel hata sayfası 502 kötü ağ geçidi sayfası yerine gönderilir. Uygulama Ağ Geçidi'nin trafiği yönlendirmek için arka ucu olmadığında gösterilir. Örneğin, zamanlanmış bakım olduğunda veya öngörülemeyen bir sorun arka uç havuzu erişimini etkilediğinde.
- **Yetkisiz erişim sayfası** - Bu özel hata sayfası 403 yetkisiz erişim sayfası yerine gönderilir. Uygulama Ağ Geçidi WAF kötü amaçlı trafiği algıladığında ve engellediğinde gösterilir.

Bir hata arka uç sunucularından kaynaklanıyorsa, değiştirilmemiş olarak arayana aktarılır. Özel bir hata sayfası görüntülenmez. Uygulama ağ geçidi, bir istek arka uca ulaşadığında özel bir hata sayfası görüntüleyebilir.

Özel hata sayfaları genel düzeyde ve dinleyici düzeyinde tanımlanabilir:

- **Genel düzey** - hata sayfası, bu uygulama ağ geçidinde dağıtılan tüm web uygulamaları için trafik için geçerlidir.
- **Dinleyici düzeyi** - hata sayfası bu dinleyiciden alınan trafiğe uygulanır.
- **Her ikisi de** - dinleyici düzeyinde tanımlanan özel hata sayfası, genel düzeyde ayarlananı geçersiz kılar.

Özel bir hata sayfası oluşturmak için şunları yapmış olmalısınız:

- bir HTTP yanıt durum kodu.
- hata sayfası için karşılık gelen konum. 
- konum için herkese açık bir Azure depolama blob'u.
- *.htm veya *.html uzantı türü. 

Hata sayfasının boyutu 1 MB'dan az olmalıdır. Hata sayfasında bağlı görüntüler varsa, bunlar alenen erişilebilir mutlak URL'ler veya özel hata sayfasında base64 kodlanmış görüntü satırında olmalıdır. Aynı blob konumundaki görüntülere sahip göreli bağlantılar şu anda desteklenmez. 

Bir hata sayfası belirttikten sonra, uygulama ağ geçidi sayfayı depolama blob konumundan karşıdan yükler ve yerel uygulama ağ geçidi önbelleğine kaydeder. Daha sonra hata sayfası doğrudan uygulama ağ geçidinden sunulur. Varolan bir özel hata sayfasını değiştirmek için, uygulama ağ geçidi yapılandırmasında farklı bir blob konumuna işaret etmelisiniz. Uygulama ağ geçidi, yeni sürümler getirmek için blob konumunu düzenli olarak denetlemez.

## <a name="portal-configuration"></a>Portal yapılandırması

1. Portaldaki Uygulama Ağ Geçidi'ne gidin ve bir uygulama ağ geçidi seçin.

    ![ag-genel bakış](media/custom-error/ag-overview.png)
2. **Dinleyicileri** tıklatın ve bir hata sayfası belirtmek istediğiniz belirli bir dinleyiciye gidin.

    ![Uygulama Ağ Geçidi dinleyicileri](media/custom-error/ag-listener.png)
3. Dinleyici düzeyinde 403 WAF hatası veya 502 bakım sayfası için özel bir hata sayfası yapılandırın.

    > [!NOTE]
    > Azure portalından genel düzey özel hata sayfaları oluşturmak şu anda desteklenmez.

4. Belirli bir hata durum kodu için herkese açık bir blob URL'si belirtin ve **Kaydet'i**tıklatın. Uygulama Ağ Geçidi artık özel hata sayfasıyla yapılandırıldı.

   ![Uygulama Ağ Geçidi hata kodları](media/custom-error/ag-error-codes.png)

## <a name="azure-powershell-configuration"></a>Azure PowerShell yapılandırması

Özel bir hata sayfasını yapılandırmak için Azure PowerShell'i kullanabilirsiniz. Örneğin, genel bir özel hata sayfası:

`$updatedgateway = Add-AzApplicationGatewayCustomError -ApplicationGateway $appgw -StatusCode HttpStatus502 -CustomErrorPageUrl $customError502Url`

Veya dinleyici düzeyi hata sayfası:

`$updatedlistener = Add-AzApplicationGatewayHttpListenerCustomError -HttpListener $listener01 -StatusCode HttpStatus502 -CustomErrorPageUrl $customError502Url`

Daha fazla bilgi için bkz: [Add-AzApplicationGatewayCustomError](https://docs.microsoft.com/powershell/module/az.network/add-azapplicationgatewaycustomerror?view=azps-1.2.0) ve [Add-AzApplicationGatewayHttpListenerCustomError](https://docs.microsoft.com/powershell/module/az.network/add-azapplicationgatewayhttplistenercustomerror?view=azps-1.3.0).

## <a name="next-steps"></a>Sonraki adımlar

Uygulama Ağ Geçidi tanılama hakkında bilgi için, [Bkz. Arka uç durumu, tanılama günlükleri ve Uygulama Ağ Geçidi ölçümleri.](application-gateway-diagnostics.md)