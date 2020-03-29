---
title: Uç nokta kotası artırın - LUIS
titleSuffix: Azure Cognitive Services
description: Dil Anlama (LUIS), bitiş noktası istek kotasını tek bir anahtarın kotasının ötesine çıkarma olanağı sunar. Bu, LUIS için daha fazla anahtar oluşturarak ve bunları Kaynaklar **ve Anahtarlar** bölümündeki **Yayımla** sayfasındaki LUIS uygulamasına ekleyerek yapılır.
author: diberry
manager: nitinme
ms.custom: seodec18
services: cognitive-services
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: c4ea9c5663755a4feb1693dd925d99b10c466140
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70256598"
---
# <a name="use-microsoft-azure-traffic-manager-to-manage-endpoint-quota-across-keys"></a>Anahtarlar arasında uç nokta kotasını yönetmek için Microsoft Azure Trafik Yöneticisi'ni kullanın
Dil Anlama (LUIS), bitiş noktası istek kotasını tek bir anahtarın kotasının ötesine çıkarma olanağı sunar. Bu, LUIS için daha fazla anahtar oluşturarak ve bunları Kaynaklar **ve Anahtarlar** bölümündeki **Yayımla** sayfasındaki LUIS uygulamasına ekleyerek yapılır. 

İstemci uygulaması anahtarları arasında trafik yönetmek zorundadır. LUIS bunu yapmaz. 

Bu makalede, Azure [Trafik Yöneticisi][traffic-manager-marketing]ile anahtarlar arasında trafik nasıl yönetilir. Zaten eğitimli ve yayınlanmış bir LUIS uygulaması olmalıdır. Eğer yoksa, Önceden oluşturulmuş etki alanı [quickstart](luis-get-started-create-app.md)izleyin. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="connect-to-powershell-in-the-azure-portal"></a>Azure portalında PowerShell'e bağlanın
[Azure][azure-portal] portalında PowerShell penceresini açın. PowerShell penceresi nin simgesi, üst teki gezinti çubuğundaki **>_.** Portaldan PowerShell'i kullanarak en son PowerShell sürümünü elde emiş olursunuz ve kimlik doğrulanırsınız. Portaldaki PowerShell için bir [Azure Depolama](https://azure.microsoft.com/services/storage/) hesabı gerekiyor. 

![Powershell penceresi açık azure portalının ekran görüntüsü](./media/traffic-manager/azure-portal-powershell.png)

Aşağıdaki bölümlerde [Traffic Manager PowerShell cmdlets](https://docs.microsoft.com/powershell/module/az.trafficmanager/#traffic_manager)kullanın.

## <a name="create-azure-resource-group-with-powershell"></a>PowerShell ile Azure kaynak grubu oluşturma
Azure kaynaklarını oluşturmadan önce, tüm kaynakları içerecek bir kaynak grubu oluşturun. Kaynak grubunu `luis-traffic-manager` adlandırın ve `West US`bölgeyi kullanın. Kaynak grubunun bölgesi grup la ilgili meta verileri depolar. Başka bir bölgede olmaları kaynaklarınızı yavaşlatamaz. 

**[New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)** cmdlet ile kaynak grubu oluşturun:

```powerShell
New-AzResourceGroup -Name luis-traffic-manager -Location "West US"
```

## <a name="create-luis-keys-to-increase-total-endpoint-quota"></a>Toplam bitiş noktası kotasını artırmak için LUIS tuşları oluşturma
1. Azure portalında, biri **Language Understanding** 'de `West US` ve `East US`bir'in içinde olmak üzere iki Dil Anlama anahtarı oluşturun. Önceki bölümde oluşturulan ve '. `luis-traffic-manager` 

    ![Luis-traffic-manager kaynak grubunda iki LUIS anahtarı bulunan Azure portalının ekran görüntüsü](./media/traffic-manager/luis-keys.png)

2. [LUIS][LUIS] web sitesinde, **Azure Kaynakları** sayfasında **Yönet** bölümünde, uygulamanın anahtarlarını atayın ve sağ üst menüdeki **Yayımla** düğmesini seçerek uygulamayı yeniden yayımlayın. 

    **Bitiş noktası** sütunundaki örnek URL, sorgu parametresi olarak bitiş noktası anahtarını içeren bir GET isteği kullanır. İki yeni anahtarın uç noktası URL'lerini kopyalayın. Bunlar, bu makalenin ilerleyen saatlerinde Trafik Yöneticisi yapılandırmasının bir parçası olarak kullanılır.

## <a name="manage-luis-endpoint-requests-across-keys-with-traffic-manager"></a>TuP uç noktası isteklerini trafik yöneticisi ile tuşlar arasında yönetme
Trafik Yöneticisi, uç noktalarınız için yeni bir DNS erişim noktası oluşturur. Bir ağ geçidi veya proxy olarak değil, kesinlikle DNS düzeyinde hareket etmez. Bu örnek, herhangi bir DNS kaydını değiştirmez. Bu özel istek için doğru bitiş noktasını almak için Trafik Yöneticisi ile iletişim kurmak için bir DNS kitaplığı kullanır. LUIS için tasarlanan _her_ istek, öncelikle hangi LUIS bitiş noktasının kullanılacağını belirlemek için bir Trafik Yöneticisi isteği gerektirir. 

### <a name="polling-uses-luis-endpoint"></a>Yoklama LUIS bitiş noktasını kullanır
Trafik Yöneticisi bitiş noktasının hala kullanılabilir olduğundan emin olmak için bitiş noktalarını düzenli aralıklarla yoklar. Ankete katılan Trafik Yöneticisi URL'sinin GET isteğiyle erişilebilir olması ve 200'lük bir iade etmesi gerekir. **Yayımla** sayfasındaki bitiş noktası URL'si bunu yapar. Her uç nokta anahtarının farklı bir rota ve sorgu dize parametreleri olduğundan, her uç nokta anahtarının farklı bir yoklama yolu gerekir. Trafik Yöneticisi anketler her zaman, bir kota isteği maliyeti yok. LUIS bitiş noktasının sorgu dize parametresi **q** LUIS'e gönderilen söyleyişdir. Bu parametre, bir sözcük göndermek yerine, Trafik Yöneticisi yapılandırılırken hata ayıklama tekniği olarak LUIS uç noktası günlüğüne Trafik Yöneticisi yoklama eklemek için kullanılır.

Her LUIS bitiş noktasının kendi yoluna ihtiyacı olduğundan, kendi Trafik Yöneticisi profiline ihtiyaç duyar. Profiller arasında yönetmek için [ _iç içe bir_ Trafik Yöneticisi](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-nested-profiles) mimarisi oluşturun. Bir üst profil, alt profilleri işaret eder ve bu profiller üzerindeki trafiği yönetir.

Trafik Yöneticisi yapılandırıldıktan sonra, günlük ünüzün yoklamayla dolmaması için logging=false query string parametresini kullanma yolunu değiştirmeyi unutmayın.

## <a name="configure-traffic-manager-with-nested-profiles"></a>İç içe profillerle Trafik Yöneticisini yapılandırın
Aşağıdaki bölümlerde biri Doğu LUIS anahtarı, diğeri West LUIS anahtarı olmak üzere iki alt profil oluşturulur. Daha sonra bir üst profil oluşturulur ve iki alt profil üst profile eklenir. 

### <a name="create-the-east-us-traffic-manager-profile-with-powershell"></a>PowerShell ile Doğu ABD Trafik Yöneticisi profilini oluşturun
Doğu ABD Trafik Yöneticisi profilini oluşturmak için birkaç adım vardır: profil oluşturma, bitiş noktası ekleme ve bitiş noktası ayarlama. Trafik Yöneticisi profilinde birçok uç nokta olabilir, ancak her bitiş noktası aynı doğrulama yoluna sahiptir. Doğu ve batı abonelikleri için LUIS uç noktası URL'leri bölge ve bitiş noktası anahtarı nedeniyle farklı olduğundan, her LUIS bitiş noktası profilde tek bir bitiş noktası olmalıdır. 

1. **[New-AzTrafficManagerProfile](https://docs.microsoft.com/powershell/module/az.trafficmanager/new-aztrafficmanagerprofile)** cmdlet ile profil oluşturma

    Profili oluşturmak için aşağıdaki cmdlet'i kullanın. Ve değiştirmek `appIdLuis` için `subscriptionKeyLuis`emin olun . AbonelikKey Doğu ABD LUIS anahtarı içindir. LUIS uygulama kimliği ve bitiş noktası anahtarı da dahil olmak üzere yol doğru değilse, Trafik Yöneticisi yoklama, Trafik Yönetimi'nin LUIS bitiş noktasını başarıyla `degraded` isteyememesi nedeniyle bir durumdur. Bu değeri LUIS `q` `traffic-manager-east` uç nokta günlüklerinde görebilmeniz için değerinin olduğundan emin olun.

    ```powerShell
    $eastprofile = New-AzTrafficManagerProfile -Name luis-profile-eastus -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-eastus -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/luis/v2.0/apps/<appID>?subscription-key=<subscriptionKey>&q=traffic-manager-east"
    ```
    
    Bu tablo cmdlet her değişkenaçıklar:
    
    |Yapılandırma parametresi|Değişken adı veya Değeri|Amaç|
    |--|--|--|
    |-Name|luis-profil-eastus|Azure portalında Trafik Yöneticisi adı|
    |-KaynakGrubuAdı|luis-trafik yöneticisi|Önceki bölümde oluşturulmuştur|
    |-TrafikRoutingYöntemi|Performans|Daha fazla bilgi için [Trafik Yöneticisi yönlendirme yöntemlerine][routing-methods]bakın. Performansı kullanıyorsanız, Trafik Yöneticisi'ne gelen URL isteği kullanıcının bölgesinden gelmelidir. Bir chatbot veya başka bir uygulama üzerinden gidiyorsanız, Trafik Yöneticisi arama da bölgeyi taklit chatbot sorumluluğundadır. |
    |-RelativeDnsName|luis-dns-eastus|Bu hizmet için alt etki alanı: luis-dns-eastus.trafficmanager.net|
    |-Ttl|30|Yoklama aralığı, 30 saniye|
    |-Monitör Protokolü<BR>-Monitör Portu|HTTPS<br>443|LUIS için bağlantı noktası ve protokolü HTTPS/443'dür|
    |-Monitör Yolu|`/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-east`|`<appIdLuis>` Değiştirin `<subscriptionKeyLuis>` ve kendi değerlerinizle.|
    
    Başarılı bir istek yanıt ı yoktur.

2. **[Add-AzTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/az.trafficmanager/add-aztrafficmanagerendpointconfig)** cmdlet ile Doğu ABD uç noktası ekleyin

    ```powerShell
    Add-AzTrafficManagerEndpointConfig -EndpointName luis-east-endpoint -TrafficManagerProfile $eastprofile -Type ExternalEndpoints -Target eastus.api.cognitive.microsoft.com -EndpointLocation "eastus" -EndpointStatus Enabled
    ```
    Bu tablo cmdlet her değişkenaçıklar:

    |Yapılandırma parametresi|Değişken adı veya Değeri|Amaç|
    |--|--|--|
    |-EndpointName|luis-doğu-bitiş noktası|Profilin altında görüntülenen uç nokta adı|
    |-TrafficManagerProfili|$eastprofile|Adım 1'de oluşturulan profil nesnesinin kullanımı|
    |-Türü|Harici Uç Noktaları|Daha fazla bilgi için [Trafik Yöneticisi bitiş noktasına][traffic-manager-endpoints] bakın |
    |-Target|eastus.api.cognitive.microsoft.com|Bu LUIS bitiş noktası için etki alanıdır.|
    |-Bitiş NoktasıKonum|"eastus"|Bitiş noktasının bölgesi|
    |-Bitiş Noktası Durumu|Etkin|Oluşturulduğunda bitiş noktasını etkinleştirme|

    Başarılı yanıt gibi görünüyor:

    ```console
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-eastus
    Name                             : luis-profile-eastus
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-eastus
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /luis/v2.0/apps/<luis-app-id>?subscription-key=f0517d185bcf467cba5147d6260bb868&q=traffic-manager-east
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {luis-east-endpoint}
    ```

3. **[Set-AzTrafficManagerProfil](https://docs.microsoft.com/powershell/module/az.trafficmanager/set-aztrafficmanagerprofile)** cmdlet ile Doğu ABD bitiş noktası ayarlayın

    ```powerShell
    Set-AzTrafficManagerProfile -TrafficManagerProfile $eastprofile
    ```

    Başarılı bir yanıt adım 2 ile aynı yanıt olacaktır.

### <a name="create-the-west-us-traffic-manager-profile-with-powershell"></a>PowerShell ile Batı ABD Trafik Yöneticisi profilini oluşturun
Batı ABD Trafik Yöneticisi profilini oluşturmak için aynı adımları izleyin: profil oluşturun, bitiş noktası ekleyin ve bitiş noktası ayarlayın.

1. **[New-AzTrafficManagerProfile](https://docs.microsoft.com/powershell/module/az.TrafficManager/New-azTrafficManagerProfile)** cmdlet ile profil oluşturma

    Profili oluşturmak için aşağıdaki cmdlet'i kullanın. Ve değiştirmek `appIdLuis` için `subscriptionKeyLuis`emin olun . AbonelikKey Doğu ABD LUIS anahtarı içindir. LUIS uygulama kimliği ve bitiş noktası anahtarı da dahil olmak üzere yol doğru `degraded` değilse, Trafik Yöneticisi yoklama, Trafik Yönetimi'nin LUIS bitiş noktasını başarıyla isteyememesi nedeniyle bir durumdur. Bu değeri LUIS `q` `traffic-manager-west` uç nokta günlüklerinde görebilmeniz için değerinin olduğundan emin olun.

    ```powerShell
    $westprofile = New-AzTrafficManagerProfile -Name luis-profile-westus -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-westus -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-west"
    ```
    
    Bu tablo cmdlet her değişkenaçıklar:
    
    |Yapılandırma parametresi|Değişken adı veya Değeri|Amaç|
    |--|--|--|
    |-Name|luis-profil-westus|Azure portalında Trafik Yöneticisi adı|
    |-KaynakGrubuAdı|luis-trafik yöneticisi|Önceki bölümde oluşturulmuştur|
    |-TrafikRoutingYöntemi|Performans|Daha fazla bilgi için [Trafik Yöneticisi yönlendirme yöntemlerine][routing-methods]bakın. Performansı kullanıyorsanız, Trafik Yöneticisi'ne gelen URL isteği kullanıcının bölgesinden gelmelidir. Bir chatbot veya başka bir uygulama üzerinden gidiyorsanız, Trafik Yöneticisi arama da bölgeyi taklit chatbot sorumluluğundadır. |
    |-RelativeDnsName|luis-dns-westus|Bu hizmet için alt etki alanı: luis-dns-westus.trafficmanager.net|
    |-Ttl|30|Yoklama aralığı, 30 saniye|
    |-Monitör Protokolü<BR>-Monitör Portu|HTTPS<br>443|LUIS için bağlantı noktası ve protokolü HTTPS/443'dür|
    |-Monitör Yolu|`/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-west`|`<appId>` Değiştirin `<subscriptionKey>` ve kendi değerlerinizle. Bu bitiş noktası anahtarının doğu uç noktası anahtarından farklı olduğunu unutmayın|
    
    Başarılı bir istek yanıt ı yoktur.

2. **[Add-AzTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/az.TrafficManager/Add-azTrafficManagerEndpointConfig)** cmdlet ile Batı ABD uç noktası ekleyin

    ```powerShell
    Add-AzTrafficManagerEndpointConfig -EndpointName luis-west-endpoint -TrafficManagerProfile $westprofile -Type ExternalEndpoints -Target westus.api.cognitive.microsoft.com -EndpointLocation "westus" -EndpointStatus Enabled
    ```

    Bu tablo cmdlet her değişkenaçıklar:

    |Yapılandırma parametresi|Değişken adı veya Değeri|Amaç|
    |--|--|--|
    |-EndpointName|luis-batı-bitiş noktası|Profilin altında görüntülenen uç nokta adı|
    |-TrafficManagerProfili|$westprofile|Adım 1'de oluşturulan profil nesnesinin kullanımı|
    |-Türü|Harici Uç Noktaları|Daha fazla bilgi için [Trafik Yöneticisi bitiş noktasına][traffic-manager-endpoints] bakın |
    |-Target|westus.api.cognitive.microsoft.com|Bu LUIS bitiş noktası için etki alanıdır.|
    |-Bitiş NoktasıKonum|"westus"|Bitiş noktasının bölgesi|
    |-Bitiş Noktası Durumu|Etkin|Oluşturulduğunda bitiş noktasını etkinleştirme|

    Başarılı yanıt gibi görünüyor:

    ```console
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-westus
    Name                             : luis-profile-westus
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-westus
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /luis/v2.0/apps/c3fc5d1e-5187-40cc-af0f-fbde328aa16b?subscription-key=e3605f07e3cc4bedb7e02698a54c19cc&q=traffic-manager-west
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {luis-west-endpoint}
    ```

3. **[Set-AzTrafficManagerProfil](https://docs.microsoft.com/powershell/module/az.TrafficManager/Set-azTrafficManagerProfile)** cmdlet ile Batı ABD bitiş noktası ayarlayın

    ```powerShell
    Set-AzTrafficManagerProfile -TrafficManagerProfile $westprofile
    ```

    Başarılı bir yanıt, adım 2 ile aynı yanıttır.

### <a name="create-parent-traffic-manager-profile"></a>Üst Trafik Yöneticisi profili oluşturma
Üst Trafik Yöneticisi profilini oluşturun ve iki alt Trafik Yöneticisi profilini üst öğeye bağla.

1. **[New-AzTrafficManagerProfile](https://docs.microsoft.com/powershell/module/az.TrafficManager/New-azTrafficManagerProfile)** cmdlet ile ana profil oluşturma

    ```powerShell
    $parentprofile = New-AzTrafficManagerProfile -Name luis-profile-parent -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-parent -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/"
    ```

    Bu tablo cmdlet her değişkenaçıklar:

    |Yapılandırma parametresi|Değişken adı veya Değeri|Amaç|
    |--|--|--|
    |-Name|luis-profil-ebeveyn|Azure portalında Trafik Yöneticisi adı|
    |-KaynakGrubuAdı|luis-trafik yöneticisi|Önceki bölümde oluşturulmuştur|
    |-TrafikRoutingYöntemi|Performans|Daha fazla bilgi için [Trafik Yöneticisi yönlendirme yöntemlerine][routing-methods]bakın. Performansı kullanıyorsanız, Trafik Yöneticisi'ne gelen URL isteği kullanıcının bölgesinden gelmelidir. Bir chatbot veya başka bir uygulama üzerinden gidiyorsanız, Trafik Yöneticisi arama da bölgeyi taklit chatbot sorumluluğundadır. |
    |-RelativeDnsName|luis-dns-ebeveyn|Bu hizmet için alt etki alanı: luis-dns-parent.trafficmanager.net|
    |-Ttl|30|Yoklama aralığı, 30 saniye|
    |-Monitör Protokolü<BR>-Monitör Portu|HTTPS<br>443|LUIS için bağlantı noktası ve protokolü HTTPS/443'dür|
    |-Monitör Yolu|`/`|Bunun yerine alt uç nokta yolları kullanıldığından bu yol önemli değildir.|

    Başarılı bir istek yanıt ı yoktur.

2. **[Add-AzTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/az.TrafficManager/Add-azTrafficManagerEndpointConfig)** ve **NestedEndpoints** türüyle ebeveyne Doğu ABD alt profilini ekleyin

    ```powerShell
    Add-AzTrafficManagerEndpointConfig -EndpointName child-endpoint-useast -TrafficManagerProfile $parentprofile -Type NestedEndpoints -TargetResourceId $eastprofile.Id -EndpointStatus Enabled -EndpointLocation "eastus" -MinChildEndpoints 1
    ```

    Bu tablo cmdlet her değişkenaçıklar:

    |Yapılandırma parametresi|Değişken adı veya Değeri|Amaç|
    |--|--|--|
    |-EndpointName|çocuk sonu-useast|Doğu profili|
    |-TrafficManagerProfili|$parentprofile|Bu bitiş noktasını atamak için profil|
    |-Türü|İç Içe Noktalar|Daha fazla bilgi için bkz: [Add-AzTrafficManagerEndpointConfig.](https://docs.microsoft.com/powershell/module/az.trafficmanager/Add-azTrafficManagerEndpointConfig) |
    |-TargetResourceId|$eastprofile. Kimliği|Alt profilin kimliği|
    |-Bitiş Noktası Durumu|Etkin|Üst öğeye ekledikten sonra bitiş noktası durumu|
    |-Bitiş NoktasıKonum|"eastus"|Kaynağın [Azure bölge adı](https://azure.microsoft.com/global-infrastructure/regions/)|
    |-MinChildEndpoints|1|Alt uç noktalarına minimum sayı|

    Başarılı yanıt aşağıdaki gibi görünür ve `child-endpoint-useast` yeni bitiş noktasını içerir:    

    ```console
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-parent
    Name                             : luis-profile-parent
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-parent
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {child-endpoint-useast}
    ```

3. **[Add-AzTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/az.TrafficManager/Add-azTrafficManagerEndpointConfig)** cmdlet ve **NestedEndpoints** türü ile ebeveyne Batı ABD alt profili ekleyin

    ```powerShell
    Add-AzTrafficManagerEndpointConfig -EndpointName child-endpoint-uswest -TrafficManagerProfile $parentprofile -Type NestedEndpoints -TargetResourceId $westprofile.Id -EndpointStatus Enabled -EndpointLocation "westus" -MinChildEndpoints 1
    ```

    Bu tablo cmdlet her değişkenaçıklar:

    |Yapılandırma parametresi|Değişken adı veya Değeri|Amaç|
    |--|--|--|
    |-EndpointName|çocuk sonu-uswest|Batı profili|
    |-TrafficManagerProfili|$parentprofile|Bu bitiş noktasını atamak için profil|
    |-Türü|İç Içe Noktalar|Daha fazla bilgi için bkz: [Add-AzTrafficManagerEndpointConfig.](https://docs.microsoft.com/powershell/module/az.trafficmanager/Add-azTrafficManagerEndpointConfig) |
    |-TargetResourceId|$westprofile. Kimliği|Alt profilin kimliği|
    |-Bitiş Noktası Durumu|Etkin|Üst öğeye ekledikten sonra bitiş noktası durumu|
    |-Bitiş NoktasıKonum|"westus"|Kaynağın [Azure bölge adı](https://azure.microsoft.com/global-infrastructure/regions/)|
    |-MinChildEndpoints|1|Alt uç noktalarına minimum sayı|

    Başarılı yanıt gibi görünür ve `child-endpoint-useast` hem önceki bitiş `child-endpoint-uswest` noktasını hem de yeni bitiş noktasını içerir:

    ```console
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-parent
    Name                             : luis-profile-parent
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-parent
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {child-endpoint-useast, child-endpoint-uswest}
    ```

4. **[Set-AzTrafficManagerProfil](https://docs.microsoft.com/powershell/module/az.TrafficManager/Set-azTrafficManagerProfile)** cmdlet ile uç noktaları ayarlayın 

    ```powerShell
    Set-AzTrafficManagerProfile -TrafficManagerProfile $parentprofile
    ```

    Başarılı bir yanıt, adım 3 ile aynı yanıttır.

### <a name="powershell-variables"></a>PowerShell değişkenleri
Önceki bölümlerde üç PowerShell değişkeni `$eastprofile`oluşturuldu: `$parentprofile`, , `$westprofile`. Bu değişkenler Trafik Yöneticisi yapılandırmasının sonuna doğru kullanılır. Değişkenleri oluşturmamayı seçtiyseniz veya unuttuysanız veya PowerShell pencereniz iniyorsa, profili yeniden almak ve bir değişkene atamak için PowerShell **[cmdlet,Get-AzTrafficManagerProfile'ı](https://docs.microsoft.com/powershell/module/az.TrafficManager/Get-azTrafficManagerProfile)** kullanabilirsiniz. 

Öğeleri açı paranteziçinde, `<>`ihtiyacınız olan üç profilin her biri için doğru değerlerle değiştirin. 

```powerShell
$<variable-name> = Get-AzTrafficManagerProfile -Name <profile-name> -ResourceGroupName luis-traffic-manager
```

## <a name="verify-traffic-manager-works"></a>Trafik Yöneticisi'nin çalıştığını doğrula
Trafik Yöneticisi profillerinin çalıştığını doğrulamak için profillerin `Online` bu durumun durumuna sahip olması gerekir, bitiş noktasının yoklama yoluna dayanır. 

### <a name="view-new-profiles-in-the-azure-portal"></a>Azure portalındaki yeni profilleri görüntüleme
Kaynak grubundaki kaynaklara bakarak üç profilin `luis-traffic-manager` de oluşturulduğunu doğrulayabilirsiniz.

![Azure kaynak grubunun ekran görüntüsü luis-traffic-manager](./media/traffic-manager/traffic-manager-profiles.png)

### <a name="verify-the-profile-status-is-online"></a>Profil durumunun Çevrimiçi olduğunu doğrulama
Trafik Yöneticisi, çevrimiçi olduğundan emin olmak için her bitiş noktasının yolunu yoklar. Çevrimiçiyse, alt profillerin durumu `Online`. Bu, her profilin **Genel Bakış'ında** görüntülenir. 

![Azure Trafik Yöneticisi profilinin ekran görüntüsü Çevrimiçi Monitör Durumunu gösteren Genel Bakış](./media/traffic-manager/profile-status-online.png)

### <a name="validate-traffic-manager-polling-works"></a>Trafik Yöneticisi yoklama çalışmalarını doğrula
Trafik yöneticisi yoklama çalışmalarını doğrulamanın bir diğer yolu da LUIS uç nokta günlükleridir. [LUIS][LUIS] web sitesi uygulamaları listesi sayfasında, uygulama için bitiş noktası günlüğünü dışa aktarın. Trafik Yöneticisi genellikle iki uç nokta için anket ler yaptığından, günlüklerde yalnızca birkaç dakika olsalar bile girişler vardır. Sorgunun `traffic-manager-`başladığı girişleri aramayı unutmayın.

```console
traffic-manager-west    6/7/2018 19:19  {"query":"traffic-manager-west","intents":[{"intent":"None","score":0.944767}],"entities":[]}
traffic-manager-east    6/7/2018 19:20  {"query":"traffic-manager-east","intents":[{"intent":"None","score":0.944767}],"entities":[]}
```

### <a name="validate-dns-response-from-traffic-manager-works"></a>Trafik Yöneticisi çalışır DNS yanıtı doğrulayın
DNS yanıtının bir LUIS bitiş noktası döndürür doğrulamak için, DNS istemci kitaplığı kullanarak Trafik Yönet üst profili DNS'yi isteyin. Üst profilin DNS `luis-dns-parent.trafficmanager.net`adı.

Aşağıdaki Node.js kodu üst profil için bir istekte bulunmaz ve bir LUIS bitiş noktası döndürür:

```javascript
const dns = require('dns');

dns.resolveAny('luis-dns-parent.trafficmanager.net', (err, ret) => {
  console.log('ret', ret);
});
```

LUIS bitiş noktası ile başarılı yanıt:

```json
[
    {
        value: 'westus.api.cognitive.microsoft.com', 
        type: 'CNAME'
    }
]
```

## <a name="use-the-traffic-manager-parent-profile"></a>Trafik Yöneticisi üst profilini kullanma
Uç noktalar arasında trafiği yönetmek için, LUIS bitiş noktasını bulmak için Trafik Yöneticisi DNS'ye bir çağrı eklemeniz gerekir. Bu arama her LUIS uç noktası isteği için yapılır ve LUIS istemci uygulamasının kullanıcıcoğrafi konumunu simüle etmek gerekir. Luis istemci uygulamanız ile istek arasındaki DNS yanıt kodunu bitiş noktası tahmini için LUIS'e ekleyin. 

## <a name="resolving-a-degraded-state"></a>Bozulmuş bir durumu çözme

Uç nokta durumunun neden bozulduğunu görmek için Trafik Yöneticisi için [tanı günlüklerini](../../traffic-manager/traffic-manager-diagnostic-logs.md) etkinleştirin.

## <a name="clean-up"></a>Temizleme
İki LUIS uç noktası anahtarını, üç Trafik Yöneticisi profilini ve bu beş kaynağı içeren kaynak grubunu kaldırın. Bu işlem Azure portalından yapılır. Kaynaklar listesinden beş kaynağı silersiniz. Ardından kaynak grubunu silin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu trafik yönetim kodunun BotFramework botuna nasıl eklenebilir olduğunu anlamak için BotFramework v4'teki [ara yazılım](https://docs.microsoft.com/azure/bot-service/bot-builder-create-middleware?view=azure-bot-service-4.0&tabs=csaddmiddleware%2Ccsetagoverwrite%2Ccsmiddlewareshortcircuit%2Ccsfallback%2Ccsactivityhandler) seçeneklerini gözden geçirin. 

[traffic-manager-marketing]: https://azure.microsoft.com/services/traffic-manager/
[traffic-manager-docs]: https://docs.microsoft.com/azure/traffic-manager/
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[azure-portal]: https://portal.azure.com/
[azure-storage]: https://azure.microsoft.com/services/storage/
[routing-methods]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods
[traffic-manager-endpoints]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-endpoint-types
