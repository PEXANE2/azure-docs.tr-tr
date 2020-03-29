---
title: Azure API Yönetimi hizmetlerini birden çok Azure bölgesine dağıtma
titleSuffix: Azure API Management
description: Azure API Yönetimi hizmeti örneğini birden çok Azure bölgesine nasıl dağıtılayacağım öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/12/2019
ms.author: apimpm
ms.openlocfilehash: 5c71f37741de06b8633e7eafaae2f29823214f74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442659"
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Bir Azure API Management hizmeti örneğini birden fazla Azure bölgesine dağıtma

Azure API Yönetimi, API yayıncılarının desteklenen Azure bölgelerine tek bir Azure API yönetimi hizmeti dağıtmasına olanak tanıyan çok bölgeli dağıtımı destekler. Çok bölgeli özellik, coğrafi olarak dağıtılmış API tüketicileri tarafından algılanan istek gecikmesini azaltmaya yardımcı olur ve bir bölge çevrimdışı olursa hizmet kullanılabilirliğini artırır.

Yeni bir Azure API Yönetimi hizmeti başlangıçta tek bir Azure bölgesinde yalnızca bir [birim][unit] içerir, birincil bölge. Birincil veya İkincil bölgelere ek bölgeler eklenebilir. Seçili her Birincil ve İkincil bölgeye bir API Yönetimi ağ geçidi bileşeni dağıtılır. Gelen API istekleri otomatik olarak en yakın bölgeye yönlendirilir. Bir bölge çevrimdışı olursa, API istekleri başarısız bölge etrafında otomatik olarak bir sonraki en yakın ağ geçidine yönlendirilecektir.

> [!NOTE]
> Yalnızca API Yönetimi'nin ağ geçidi bileşeni tüm bölgelere dağıtılır. Hizmet yönetimi bileşeni ve geliştirici portalı yalnızca Birincil bölgede barındırılır. Bu nedenle, Birincil bölge kesintisi durumunda, geliştirici portalına erişim ve yapılandırmayı değiştirme yeteneği (örn. API ekleme, ilkeler uygulama) Birincil bölge çevrimiçi olana kadar bozulacaktır. Birincil bölge çevrimdışı yken ikincil bölgeler, kendilerine sunulan en son yapılandırmayı kullanarak API trafiğine hizmet vermeye devam edecektir.

[!INCLUDE [premium.md](../../includes/api-management-availability-premium.md)]

## <a name="deploy-an-api-management-service-instance-to-a-new-region"></a><a name="add-region"> </a>Yeni bir bölgeye API Yönetimi hizmeti örneği dağıtma

> [!NOTE]
> Henüz bir API Yönetimi hizmeti örneği oluşturmadıysanız, [bkz.][create an api management service instance]

Azure portalında, API Yönetimi hizmet örneğiniz için **Ölçek ve fiyatlandırma** sayfasına gidin.

![Ölçek sekmesi][api-management-scale-service]

Yeni bir bölgeye dağıtmak için araç çubuğundan **+ Bölge Ekle'yi** tıklatın.

![Bölge ekle][api-management-add-region]

Açılan listeden konumu seçin ve kaydırıcıyla ilgili birim sayısını ayarlayın.

![Birimleri belirtin][api-management-select-location-units]

Seçiminizi Konumlar tablosuna yerleştirmek için **Ekle'yi** tıklatın.

Tüm konumları yapılandırana kadar bu işlemi tekrarlayın ve dağıtım işlemini başlatmak için araç çubuğundan **Kaydet'i** tıklatın.

## <a name="delete-an-api-management-service-instance-from-a-location"></a><a name="remove-region"> </a>Bir konumdan API Yönetimi hizmeti örneğini silme

Azure portalında, API Yönetimi hizmet örneğiniz için **Ölçek ve fiyatlandırma** sayfasına gidin.

![Ölçek sekmesi][api-management-scale-service]

Kaldırmak istediğiniz konum için, tablonun sağ ucundaki **...** düğmesini kullanarak bağlam menüsünü açın. **Sil** seçeneğini belirleyin.

Silme işlemini onaylayın ve değişiklikleri uygulamak için **Kaydet'i** tıklatın.

## <a name="route-api-calls-to-regional-backend-services"></a><a name="route-backend"> </a>Bölgesel arka uç hizmetlerine Route API çağrıları

Azure API Yönetimi yalnızca bir arka uç hizmet URL'si sunar. Çeşitli bölgelerde Azure API Yönetimi örnekleri olsa da, API ağ geçidi istekleri yalnızca bir bölgede dağıtılan aynı arka uç hizmetine iletmeye devam eder. Bu durumda, performans kazancı yalnızca isteğe özgü bir bölgede Azure API Yönetimi içinde önbelleğe alınan yanıtlardan gelir, ancak dünya genelinde arka uçla iletişim kurmak yine de yüksek gecikme ye neden olabilir.

Sisteminizin coğrafi dağılımından tam olarak yararlanmak için, Azure API Yönetimi örnekleriyle aynı bölgelerde arka uç hizmetlerine sahip olmalısınız. Ardından, ilkeleri ve `@(context.Deployment.Region)` özelliği kullanarak, trafiği arka uçünüzün yerel örneklerine yönlendirebilirsiniz.

1. Azure API Yönetimi örneğinize gidin ve sol menüden **API'lere** tıklayın.
2. İstediğiniz API'yi seçin.
3. **Gelen işlemedeki**ok açılır yerinden **Kod düzenleyicisini** tıklatın.

    ![API kod düzenleyicisi](./media/api-management-howto-deploy-multi-region/api-management-api-code-editor.png)

4. Dosya `set-backend` `<inbound> </inbound>` bölümünde uygun `choose` bir yönlendirme ilkesi oluşturmak için koşullu ilkelerle birleştirme kullanın.

    Örneğin, aşağıdaki XML dosyası Batı ABD ve Doğu Asya bölgeleri için çalışır:

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
> Ayrıca, [Azure Trafik Yöneticisi](https://azure.microsoft.com/services/traffic-manager/)ile arka uç hizmetlerinizin önüne çıkabilir, API çağrılarını Trafik Yöneticisi'ne yönlendirebilir ve yönlendirmeyi otomatik olarak çözmesine izin verebilirsiniz.

## <a name="use-custom-routing-to-api-management-regional-gateways"></a><a name="custom-routing"> </a>API Yönetimi bölgesel ağ geçitlerine özel yönlendirme kullanın

API Yönetimi istekleri [en düşük gecikme durumuna](../traffic-manager/traffic-manager-routing-methods.md#performance)göre bölgesel bir ağ _geçidine_ yönlendirir. API Yönetimi'nde bu ayarı geçersiz kılmak mümkün olmasa da, kendi Trafik Yöneticinizi özel yönlendirme kurallarıyla kullanabilirsiniz.

1. Kendi [Azure Trafik Yöneticinizi](https://azure.microsoft.com/services/traffic-manager/)oluşturun.
1. Özel bir etki alanı kullanıyorsanız, api yönetimi hizmeti yerine [Trafik Yöneticisi ile kullanın.](../traffic-manager/traffic-manager-point-internet-domain.md)
1. [Trafik Yöneticisi'ndeki API Yönetimi bölgesel uç noktalarını yapılandırın.](../traffic-manager/traffic-manager-manage-endpoints.md) Bölgesel uç `https://<service-name>-<region>-01.regional.azure-api.net`noktalar, örneğin `https://contoso-westus2-01.regional.azure-api.net`URL deseni izleyin.
1. [Trafik Yöneticisi'ndeki API Yönetimi bölgesel durum bitiş noktalarını yapılandırın.](../traffic-manager/traffic-manager-monitoring.md) Bölgesel durum bitiş `https://<service-name>-<region>-01.regional.azure-api.net/status-0123456789abcdef`noktaları, örneğin `https://contoso-westus2-01.regional.azure-api.net/status-0123456789abcdef`URL deseni izleyin.
1. Trafik [Yöneticisi'nin yönlendirme yöntemini](../traffic-manager/traffic-manager-routing-methods.md) belirtin.

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png
[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png
[create an api management service instance]: get-started-create-service-instance.md
[get started with azure api management]: get-started-create-service-instance.md
[deploy an api management service instance to a new region]: #add-region
[delete an api management service instance from a region]: #remove-region
[unit]: https://azure.microsoft.com/pricing/details/api-management/
[premium]: https://azure.microsoft.com/pricing/details/api-management/
