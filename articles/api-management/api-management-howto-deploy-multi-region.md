---
title: Azure API Management hizmetlerini birden çok Azure bölgesine dağıtma
titleSuffix: Azure API Management
description: Azure API Management hizmet örneğini birden çok Azure bölgesine dağıtmayı öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/20/2020
ms.author: apimpm
ms.openlocfilehash: 17c92558ebef2eee0a4daead45d16a295cedd1bb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82790488"
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Bir Azure API Management hizmeti örneğini birden fazla Azure bölgesine dağıtma

Azure API Management, API yayımcılarının herhangi bir sayıda desteklenen Azure bölgesinde tek bir Azure API Management hizmetini dağıtmasını sağlayan çok bölgeli dağıtımı destekler. Çok bölgeli özellik, coğrafi olarak dağıtılan API tüketicileri tarafından algılanan istek gecikmesini azaltmaya yardımcı olur ve bir bölgenin çevrimdışı olması durumunda hizmet kullanılabilirliğini geliştirir.

Yeni bir Azure API Management hizmeti başlangıçta tek bir Azure bölgesinde, birincil bölgede yalnızca bir [birim][unit] içerir. Birincil veya Ikincil bölgelere ek birimler eklenebilir. API Management ağ geçidi bileşeni, seçilen her birincil ve Ikincil bölgeye dağıtılır. Gelen API istekleri, en yakın bölgeye otomatik olarak yönlendirilir. Bir bölge çevrimdışı kalırsa, API istekleri otomatik olarak, başarısız bölge sonraki en yakın ağ geçidine yönlendirilir.

> [!NOTE]
> Yalnızca API Management ağ geçidi bileşeni tüm bölgelere dağıtılır. Hizmet Yönetimi bileşeni ve geliştirici portalı yalnızca birincil bölgede barındırılır. Bu nedenle, birincil bölge kesintisi durumunda, geliştirici portalına erişim ve yapılandırmayı değiştirebilme (ör. API ekleme, ilkeleri uygulama), birincil bölge yeniden çevrimiçi olana kadar zayıflatılabilir. Birincil bölge çevrimdışı iken kullanılabilir olan Ikincil bölgeler, API trafiği için kullanılabilen en son yapılandırmayı kullanmaya devam eder.

[!INCLUDE [premium.md](../../includes/api-management-availability-premium.md)]

## <a name="deploy-api-management-service-to-a-new-region"></a><a name="add-region"> </a>API Management hizmetini yeni bir bölgeye dağıtma

> [!NOTE]
> Henüz bir API Management hizmet örneği oluşturmadıysanız, bkz. [API Management hizmet örneği oluşturma][create an api management service instance].

1. Azure portal, API Management hizmetinize gidin ve menüdeki **konumlar** girdisine tıklayın.
2. Üstteki çubukta **+ Ekle** ' ye tıklayın.
3. Açılan listeden konumu seçin ve kaydırıcıyla birim sayısını ayarlayın.
4. Onaylamak için **Ekle** düğmesine tıklayın.
5. Tüm konumları yapılandırmadan bu işlemi tekrarlayın.
6. Dağıtım işlemini başlatmak için üstteki çubukta **Kaydet** ' e tıklayın.

## <a name="delete-an-api-management-service-location"></a><a name="remove-region"> </a>API Management hizmet konumunu silme

1. Azure portal, API Management hizmetinize gidin ve menüdeki **konumlar** girdisine tıklayın.
2. Kaldırmak istediğiniz konum için, tablonun sağ ucundaki **...** düğmesini kullanarak bağlam menüsünü açın. **Sil** seçeneğini belirleyin.
3. Silme işlemini onaylayın ve değişiklikleri uygulamak için **Kaydet** ' e tıklayın.

## <a name="route-api-calls-to-regional-backend-services"></a><a name="route-backend"> </a>Bölgesel arka uç hIzmetlerIne yönelik API çağrılarını yönlendir

Azure API Management yalnızca bir arka uç hizmeti URL 'SI sunar. Çeşitli bölgelerde Azure API Management örnekleri olsa da API ağ geçidi, istekleri yalnızca tek bir bölgede dağıtılan aynı arka uç hizmetine iletir. Bu durumda, performans kazancı yalnızca isteğe özgü bir bölgede Azure API Management önbelleğe alınan yanıtlardan gelir, ancak dünya genelindeki arka uca iletişim kurarak yüksek gecikme süresine neden olabilir.

Sisteminizin coğrafi dağıtımını tam olarak yararlanmak için, arka uç hizmetlerinizin Azure API Management örnekleri ile aynı bölgelerde dağıtılması gerekir. Ardından, ilkeleri ve `@(context.Deployment.Region)` özelliği kullanarak, trafiği arka ucunuzun yerel örneklerine yönlendirebilirsiniz.

1. Azure API Management örneğinize gidin ve sol menüden **API 'ler** ' e tıklayın.
2. İstediğiniz API 'YI seçin.
3. **Gelen işlem**içindeki ok açılan listesinden **Kod Düzenleyicisi** ' ne tıklayın.

    ![API kodu Düzenleyicisi](./media/api-management-howto-deploy-multi-region/api-management-api-code-editor.png)

4. `set-backend` `choose` Dosyanın bölümünde uygun bir yönlendirme ilkesi oluşturmak için, koşullu ilkelerle birlikte kullanın `<inbound> </inbound>` .

    Örneğin, aşağıdaki XML dosyası Batı ABD ve Doğu Asya bölgelerinde çalışır:

    ```xml
    <policies>
        <inbound>
            <base />
            <choose>
                <when condition="@("West US".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-us.com/" />
                </when>
                <when condition="@("East Asia".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-asia.com/" />
                </when>
                <otherwise>
                    <set-backend-service base-url="http://contoso-other.com/" />
                </otherwise>
            </choose>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>
    ```

> [!TIP]
> Arka uç hizmetlerinizi [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/)ile de ÖNYÜKLEYEBILIRSINIZ, apı çağrılarını Traffic Manager yönlendirebilir ve yönlendirmeyi otomatik olarak çözümlemesine izin verin.

## <a name="use-custom-routing-to-api-management-regional-gateways"></a><a name="custom-routing"> </a>Bölgesel ağ geçitlerini API Management için özel yönlendirme kullanma

API Management, istekleri [En düşük gecikme süresine](../traffic-manager/traffic-manager-routing-methods.md#performance)göre bölgesel bir _ağ geçidine_ yönlendirir. API Management bu ayarı geçersiz kılmak mümkün olmasa da, özel yönlendirme kuralları ile kendi Traffic Manager kullanabilirsiniz.

1. Kendi [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/)oluşturun.
1. Özel bir etki alanı kullanıyorsanız, API Management hizmeti yerine [Traffic Manager kullanın](../traffic-manager/traffic-manager-point-internet-domain.md) .
1. [Traffic Manager API Management bölgesel uç noktalarını yapılandırın](../traffic-manager/traffic-manager-manage-endpoints.md). Bölgesel uç noktalar, örneğin URL deseninin yerine gelmelidir `https://<service-name>-<region>-01.regional.azure-api.net` `https://contoso-westus2-01.regional.azure-api.net` .
1. [Traffic Manager API Management bölgesel durum uç noktalarını yapılandırın](../traffic-manager/traffic-manager-monitoring.md). Bölgesel durum uç noktaları, örneğin URL örüntüsünün izler `https://<service-name>-<region>-01.regional.azure-api.net/status-0123456789abcdef` `https://contoso-westus2-01.regional.azure-api.net/status-0123456789abcdef` .
1. Traffic Manager [yönlendirme yöntemini](../traffic-manager/traffic-manager-routing-methods.md) belirtin.

[create an api management service instance]: get-started-create-service-instance.md
[get started with azure api management]: get-started-create-service-instance.md
[deploy an api management service instance to a new region]: #add-region
[delete an api management service instance from a region]: #remove-region
[unit]: https://azure.microsoft.com/pricing/details/api-management/
[premium]: https://azure.microsoft.com/pricing/details/api-management/
