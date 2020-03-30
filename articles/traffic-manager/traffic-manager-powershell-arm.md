---
title: Azure'da Trafik Yöneticisi'ni yönetmek için PowerShell'i kullanma
description: Bu öğrenme yolu ile Trafik Yöneticisi için Azure PowerShell'i kullanmaya başlayın.
services: traffic-manager
documentationcenter: na
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: rohink
ms.openlocfilehash: 7886764a69eefa68be071a801bea65ae995fbdc3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938505"
---
# <a name="using-powershell-to-manage-traffic-manager"></a>Trafik Yöneticisi'ni yönetmek için PowerShell'i kullanma

Azure Kaynak Yöneticisi, Azure'daki hizmetler için tercih edilen yönetim arabirimidir. Azure Trafik Yöneticisi profilleri, Azure Kaynak Yöneticisi tabanlı API'ler ve araçlar kullanılarak yönetilebilir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="resource-model"></a>Kaynak modeli

Azure Trafik Yöneticisi, Trafik Yöneticisi profili adı verilen bir ayar koleksiyonu kullanılarak yapılandırılır. Bu profil, DNS ayarlarını, trafik yönlendirme ayarlarını, bitiş noktası izleme ayarlarını ve trafiğin yönlendirildiği hizmet bitiş noktalarının listesini içerir.

Her Trafik Yöneticisi profili ' TrafficManagerProfiles' türünden bir kaynak tarafından temsil edilir. REST API düzeyinde, her profil için URI aşağıdaki gibidir:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a>Azure PowerShell'i ayarlama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu yönergeler Microsoft Azure PowerShell'i kullanır. Aşağıdaki makalede Azure PowerShell nasıl yüklenir ve yapılandırılır.

* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)

Bu makaledeki örnekler, varolan bir kaynak grubunuz olduğunu varsayar. Aşağıdaki komutu kullanarak bir kaynak grubu oluşturabilirsiniz:

```powershell
New-AzResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> Azure Kaynak Yöneticisi, tüm kaynak gruplarının bir konuma sahip olduğunu gerektirir. Bu konum, bu kaynak grubunda oluşturulan kaynaklar için varsayılan olarak kullanılır. Ancak, Trafik Yöneticisi profil kaynakları bölgesel değil, genel olduğundan, kaynak grubu konumunun seçiminin Azure Trafik Yöneticisi üzerinde hiçbir etkisi yoktur.

## <a name="create-a-traffic-manager-profile"></a>Trafik Yöneticisi Profili Oluşturma

Trafik Yöneticisi profili oluşturmak için `New-AzTrafficManagerProfile` cmdlet'i kullanın:

```powershell
$TmProfile = New-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

Aşağıdaki tabloparametreleri açıklar:

| Parametre | Açıklama |
| --- | --- |
| Adı |Trafik Yöneticisi profil kaynağının kaynak adı. Aynı kaynak grubundaki profillerin benzersiz adları olmalıdır. Bu ad, DNS sorguları için kullanılan DNS adından ayrıdır. |
| ResourceGroupName |Profil kaynağını içeren kaynak grubunun adı. |
| Trafik RoutingYöntemi |DNS sorgusunda yanıt olarak hangi uç noktanın döndürülür belirlemek için kullanılan trafik yönlendirme yöntemini belirtir. Olası değerler 'Performans', 'Ağırlıklı' veya 'Öncelik' dir. |
| RelativeDnsName |Bu Trafik Yöneticisi profili tarafından sağlanan DNS adının ana bilgisayar adı bölümünü belirtir. Bu değer, azure trafik yöneticisi tarafından profilin tam nitelikli etki alanı adını (FQDN) oluşturmak için kullanılan DNS etki alanı adı ile birleştirilir. Örneğin, 'contoso' değerini ayarlamak 'contoso.trafficmanager.net' olur. |
| TTL |DNS'nin Canlı Süresi'ni (TTL) saniyeler içinde belirtir. Bu TTL, Yerel DNS çözümleyicilerine ve DNS istemcilerine bu Trafik Yöneticisi profili için DNS yanıtlarını ne kadar süreyle önbelleğe alacağını bildirir. |
| İzleme Protokolü |Uç nokta durumunu izlemek için kullanılacak protokolü belirtir. Olası değerler 'HTTP' ve 'HTTPS' dir. |
| MonitorPort |Uç nokta durumunu izlemek için kullanılan TCP bağlantı noktasını belirtir. |
| Monitör Yolu |Bitiş noktası durumu için sonda için kullanılan bitiş noktası etki alanı adı ile göreli yolu belirtir. |

Cmdlet, Azure'da bir Trafik Yöneticisi profili oluşturur ve ilgili profil nesnesini PowerShell'e döndürür. Bu noktada, profil herhangi bir uç nokta içermez. Trafik Yöneticisi profiline uç noktaları ekleme hakkında daha fazla bilgi için bkz.

## <a name="get-a-traffic-manager-profile"></a>Trafik Yöneticisi Profili Al

Varolan bir Trafik Yöneticisi profil `Get-AzTrafficManagerProfle` nesnesini almak için cmdlet'i kullanın:

```powershell
$TmProfile = Get-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Bu cmdlet bir Trafik Yöneticisi profil nesnesi döndürür.

## <a name="update-a-traffic-manager-profile"></a>Trafik Yöneticisi Profilini Güncelleştir

Trafik Yöneticisi profillerini değiştirmek 3 adımlı bir işlemi izler:

1. Profili kullanarak `Get-AzTrafficManagerProfile` profili alın veya `New-AzTrafficManagerProfile`döndürülen profili kullanın.
2. Profili değiştirin. Uç noktaları ekleyebilir ve kaldırabilir veya bitiş noktası veya profil parametrelerini değiştirebilirsiniz. Bu değişiklikler çevrimdışı işlemlerdir. Yalnızca profili temsil eden bellekteki yerel nesneyi değiştirin.
3. `Set-AzTrafficManagerProfile` Cmdlet kullanarak değişikliklerinizi gerçekleştirin.

Profilin RelativeDnsName'si dışında tüm profil özellikleri değiştirilebilir. RelativeDnsName'yi değiştirmek için profili ve yeni bir ada sahip yeni bir profili silmeniz gerekir.

Aşağıdaki örnek, profilin TTL'sinin nasıl değiştirilebildiğini gösterir:

```powershell
$TmProfile = Get-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$TmProfile.Ttl = 300
Set-AzTrafficManagerProfile -TrafficManagerProfile $TmProfile
```

Üç tür Trafik Yöneticisi uç noktası vardır:

1. **Azure uç noktaları** Azure'da barındırılan hizmetlerdir
2. **Dış uç noktalar** Azure dışında barındırılan hizmetlerdir
3. **İç içe uç noktalar,** Trafik Yöneticisi profillerinin iç içe olan hiyerarşilerini oluşturmak için kullanılır. İç içe uç noktalar, karmaşık uygulamalar için gelişmiş trafik yönlendirme yapılandırmaları sağlar.

Her üç durumda da uç noktalar iki şekilde eklenebilir:

1. Daha önce açıklanan 3 adımlı bir işlem kullanma. Bu yöntemin avantajı, tek bir güncelleştirmede birkaç uç nokta değişikliği yapılabilir.
2. New-AzTrafficManagerEndpoint cmdlet kullanarak. Bu cmdlet, tek bir işlemde varolan Bir Trafik Yöneticisi profiline bir bitiş noktası ekler.

## <a name="adding-azure-endpoints"></a>Azure Uç Noktaları Ekleme

Azure'da barındırılan Azure uç noktaları başvuru hizmetleri. İki tür Azure uç noktası desteklenir:

1. Azure App Service
2. Azure PublicIpAddress kaynakları (bir yük bakiyesi veya sanal makine NIC'ye eklenebilir). PublicIpAddress Trafik Yöneticisi kullanılmak üzere atanmış bir DNS adı olmalıdır.

Her durumda:

* Hizmet 'targetResourceId' parametresi `Add-AzTrafficManagerEndpointConfig` veya `New-AzTrafficManagerEndpoint`.
* 'Hedef' ve 'EndpointLocation' TargetResourceId tarafından ima edilir.
* 'Ağırlık' belirtilmesi isteğe bağlıdır. Ağırlıklar yalnızca profil 'Ağırlıklı' trafik yönlendirme yöntemini kullanacak şekilde yapılandırılırsa kullanılır. Aksi takdirde, bunlar yoksayılır. Belirtilirse, değer 1 ile 1000 arasında bir sayı olmalıdır. Varsayılan değer '1'dir.
* 'Öncelik' belirtilmesi isteğe bağlıdır. Öncelikler yalnızca profil 'Öncelik' trafik yönlendirme yöntemini kullanacak şekilde yapılandırıldığında kullanılır. Aksi takdirde, bunlar yoksayılır. Geçerli değerler 1 ile 1000 arasındadır ve daha düşük değerler daha yüksek bir önceliği gösterir. Bir bitiş noktası için belirtilirse, bunların tüm uç noktalar için belirtilmesi gerekir. Atlanırsa, uç noktaların listelenme sırasına göre '1'den başlayan varsayılan değerler uygulanır.

### <a name="example-1-adding-app-service-endpoints-using-add-aztrafficmanagerendpointconfig"></a>Örnek 1: Uygulama Hizmeti uç noktaları ekleme`Add-AzTrafficManagerEndpointConfig`

Bu örnekte, bir Trafik Yöneticisi profili oluşturuyoruz ve `Add-AzTrafficManagerEndpointConfig` cmdlet'i kullanarak iki Uygulama Hizmeti bitiş noktası ekliyoruz.

```powershell
$TmProfile = New-AzTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzWebApp -Name webapp1
Add-AzTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $TmProfile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzWebApp -Name webapp2
Add-AzTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $TmProfile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzTrafficManagerProfile -TrafficManagerProfile $TmProfile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-aztrafficmanagerendpoint"></a>Örnek 2: PublicIpAddress uç noktası kullanarak ekleme`New-AzTrafficManagerEndpoint`

Bu örnekte, Trafik Yöneticisi profiline ortak bir IP adresi kaynağı eklenir. Ortak IP adresi nin bir DNS adı yapılandırılmış olması gerekir ve bir VM'nin NIC'sine veya yük dengeleyicisine bağlanabilir.

```powershell
$ip = Get-AzPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a>Dış Uç Noktaları Ekleme

Trafik Yöneticisi, trafiği Azure dışında barındırılan hizmetlere yönlendirmek için harici uç noktaları kullanır. Azure uç noktalarında olduğu gibi, harici uç `Add-AzTrafficManagerEndpointConfig` noktalar `Set-AzTrafficManagerProfile`da `New-AzTrafficManagerEndpoint`aşağıdaki ler kullanılarak eklenebilir.

Dış uç noktaları belirtirken:

* Bitiş noktası etki alanı adı 'Hedef' parametresi kullanılarak belirtilmelidir
* 'Performans' trafik yönlendirme yöntemi kullanılırsa, 'EndpointLocation' gereklidir. Aksi takdirde isteğe bağlıdır. Değer geçerli bir [Azure bölge adı](https://azure.microsoft.com/regions/)olmalıdır.
* 'Ağırlık' ve 'Öncelik' isteğe bağlıdır.

### <a name="example-1-adding-external-endpoints-using-add-aztrafficmanagerendpointconfig-and-set-aztrafficmanagerprofile"></a>Örnek 1: Harici uç `Add-AzTrafficManagerEndpointConfig` noktaları kullanarak ekleme ve`Set-AzTrafficManagerProfile`

Bu örnekte, bir Trafik Yöneticisi profili oluşturur, iki dış uç nokta ekler ve değişiklikleri işler.

```powershell
$TmProfile = New-AzTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $TmProfile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $TmProfile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzTrafficManagerProfile -TrafficManagerProfile $TmProfile
```

### <a name="example-2-adding-external-endpoints-using-new-aztrafficmanagerendpoint"></a>Örnek 2: Harici uç noktaları kullanarak ekleme`New-AzTrafficManagerEndpoint`

Bu örnekte, varolan bir profile harici bir bitiş noktası ekleriz. Profil, profil ve kaynak grubu adları kullanılarak belirtilir.

```powershell
New-AzTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a>'İç içe' uç noktaları ekleme

Her Trafik Yöneticisi profili nde tek bir trafik yönlendirme yöntemi belirtilir. Ancak, tek bir Trafik Yöneticisi profili tarafından sağlanan yönlendirmedaha karmaşık trafik yönlendirme gerektiren senaryolar vardır. Birden fazla trafik yönlendirme yönteminin avantajlarını birleştirmek için Trafik Yöneticisi profillerini iç içe çekebilirsiniz. İç içe profiller, daha büyük ve daha karmaşık uygulama dağıtımlarını desteklemek için varsayılan Trafik Yöneticisi davranışını geçersiz kılmanızı sağlar. Daha ayrıntılı örnekler için İç [Içe Trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md)bakın.

İç içe uç noktalar, belirli bir uç nokta türü olan 'NestedEndpoints' kullanılarak üst profilde yapılandırılır. İç içe uç noktaları belirtirken:

* Bitiş noktası 'targetResourceId' parametresi kullanılarak belirtilmelidir
* 'Performans' trafik yönlendirme yöntemi kullanılırsa, 'EndpointLocation' gereklidir. Aksi takdirde isteğe bağlıdır. Değer geçerli bir [Azure bölge adı](https://azure.microsoft.com/regions/)olmalıdır.
* Azure uç noktalarında olduğu gibi 'Ağırlık' ve 'Öncelik' isteğe bağlıdır.
* 'MinChildEndpoints' parametresi isteğe bağlıdır. Varsayılan değer '1'dir. Kullanılabilir uç nokta sayısı bu eşiğin altına düşerse, üst profil alt profili 'bozulmuş' olarak kabul eder ve trafiği üst profildeki diğer uç noktalara yönlendirir.

### <a name="example-1-adding-nested-endpoints-using-add-aztrafficmanagerendpointconfig-and-set-aztrafficmanagerprofile"></a>Örnek 1: İç içe `Add-AzTrafficManagerEndpointConfig` uç noktaları kullanarak ekleme ve`Set-AzTrafficManagerProfile`

Bu örnekte, yeni Trafik Yöneticisi alt ve üst profilleri oluşturur, alt öğeyi iç içe geçen bir bitiş noktası olarak ekleyebilir ve değişiklikleri işler.

```powershell
$child = New-AzTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzTrafficManagerProfile -TrafficManagerProfile $parent
```

Bu örnekte kısa lık için, alt veya üst profillere başka uç nokta eklemedik.

### <a name="example-2-adding-nested-endpoints-using-new-aztrafficmanagerendpoint"></a>Örnek 2: İç içe uç noktaları kullanarak ekleme`New-AzTrafficManagerEndpoint`

Bu örnekte, varolan bir üst profile iç içe geçmiş bir uç nokta olarak varolan bir alt profil ekliyoruz. Profil, profil ve kaynak grubu adları kullanılarak belirtilir.

```powershell
$child = Get-AzTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="adding-endpoints-from-another-subscription"></a>Başka bir abonelikten uç noktaları ekleme

Trafik Yöneticisi, farklı aboneliklerden uç noktalarla çalışabilir. Gerekli girişi Trafik Yöneticisi'ne almak için eklemek istediğiniz bitiş noktasıyla aboneliğe geçmeniz gerekir. Ardından Trafik Yöneticisi profiliyle aboneliklere geçmeniz ve bitiş noktasını eklemeniz gerekir. Aşağıdaki örnekte, bunu genel bir IP adresiyle nasıl yapacağız gösterilmektedir.

```powershell
Set-AzContext -SubscriptionId $EndpointSubscription
$ip = Get-AzPublicIpAddress -Name $IpAddressName -ResourceGroupName $EndpointRG

Set-AzContext -SubscriptionId $trafficmanagerSubscription
New-AzTrafficManagerEndpoint -Name $EndpointName -ProfileName $ProfileName -ResourceGroupName $TrafficManagerRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="update-a-traffic-manager-endpoint"></a>Trafik Yöneticisi Bitiş Noktasını Güncelleştirme

Varolan bir Trafik Yöneticisi bitiş noktasını güncelleştirmenin iki yolu vardır:

1. Trafik Yöneticisi profilini `Get-AzTrafficManagerProfile`kullanarak alın, profildeki bitiş noktası özelliklerini güncelleştirin ve değişiklikleri kullanarak `Set-AzTrafficManagerProfile`gerçekleştirin. Bu yöntem, tek bir işlemde birden fazla uç noktayı güncelleştirme avantajına sahiptir.
2. Trafik Yöneticisi bitiş noktasını `Get-AzTrafficManagerEndpoint`kullanarak alın, bitiş noktası özelliklerini `Set-AzTrafficManagerEndpoint`güncelleştirin ve değişiklikleri kullanarak gerçekleştirin. Profildeki Uç Noktalar dizisine dizin oluşturmayı gerektirmediğinden, bu yöntem daha basittir.

### <a name="example-1-updating-endpoints-using-get-aztrafficmanagerprofile-and-set-aztrafficmanagerprofile"></a>Örnek 1: Uç noktaları `Get-AzTrafficManagerProfile` kullanarak güncelleme ve`Set-AzTrafficManagerProfile`

Bu örnekte, varolan bir profildeki iki uç noktadaki önceliği değiştiririz.

```powershell
$TmProfile = Get-AzTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$TmProfile.Endpoints[0].Priority = 2
$TmProfile.Endpoints[1].Priority = 1
Set-AzTrafficManagerProfile -TrafficManagerProfile $TmProfile
```

### <a name="example-2-updating-an-endpoint-using-get-aztrafficmanagerendpoint-and-set-aztrafficmanagerendpoint"></a>Örnek 2: Bir uç `Get-AzTrafficManagerEndpoint` noktanın güncellenmesi ve`Set-AzTrafficManagerEndpoint`

Bu örnekte, varolan bir profildeki tek bir bitiş noktasının ağırlığını değiştiririz.

```powershell
$endpoint = Get-AzTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Uç Noktaları ve Profilleri Etkinleştirme ve Devre Dışı Bırakma

Trafik Yöneticisi, tek tek uç noktaların etkinleştirilmesine ve devre dışı bırakılmasına izin vermenin yanı sıra tüm profillerin etkinleştirilmesine ve devre dışı bırakılmasına izin verir.
Bu değişiklikler, bitiş noktasıveya profil kaynaklarını alarak/güncelleyerek/ayarlayarak yapılabilir. Bu ortak işlemleri kolaylaştırmak için, onlar da özel cmdlets ile desteklenir.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Örnek 1: Trafik Yöneticisi profilini etkinleştirme ve devre dışı bırakma

Trafik Yöneticisi profilini etkinleştirmek `Enable-AzTrafficManagerProfile`için . Profil bir profil nesnesi kullanılarak belirtilebilir. Profil nesnesi boru hattı üzerinden veya '-TrafficManagerProfile' parametresi kullanılarak geçirilebilir. Bu örnekte, profili profil ve kaynak grubu adına göre belirtiriz.

```powershell
Enable-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Trafik Yöneticisi profilini devre dışı kalmak için:

```powershell
Disable-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Disable-AzTrafficManagerProfile cmdlet onay ister. Bu istemi '-Kuvvet' parametresi kullanılarak bastırılabilir.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Örnek 2: Trafik Yöneticisi bitiş noktasını etkinleştirme ve devre dışı bırakma

Trafik Yöneticisi bitiş noktasını etkinleştirmek `Enable-AzTrafficManagerEndpoint`için. Bitiş noktasını belirtmenin iki yolu vardır

1. Boru hattı üzerinden geçirilen trafficManagerEndpoint nesnesini kullanma veya '-TrafficManagerEndpoint' parametresini kullanma
2. Uç nokta adını, bitiş noktası türünü, profil adını ve kaynak grup adını kullanma:

```powershell
Enable-AzTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Benzer şekilde, Trafik Yöneticisi bitiş noktasını devre dışı kılabilir:

```powershell
Disable-AzTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Olduğu `Disable-AzTrafficManagerProfile`gibi `Disable-AzTrafficManagerEndpoint` , cmdlet onay ister. Bu istemi '-Kuvvet' parametresi kullanılarak bastırılabilir.

## <a name="delete-a-traffic-manager-endpoint"></a>Trafik Yöneticisi Bitiş Noktasını Silme

Tek tek uç noktaları `Remove-AzTrafficManagerEndpoint` kaldırmak için cmdlet'i kullanın:

```powershell
Remove-AzTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Bu cmdlet onay ister. Bu istemi '-Kuvvet' parametresi kullanılarak bastırılabilir.

## <a name="delete-a-traffic-manager-profile"></a>Trafik Yöneticisi Profilini Silme

Trafik Yöneticisi profilini silmek `Remove-AzTrafficManagerProfile` için, profil ve kaynak grubu adlarını belirterek cmdlet'i kullanın:

```powershell
Remove-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Bu cmdlet onay ister. Bu istemi '-Kuvvet' parametresi kullanılarak bastırılabilir.

Silinecek profil, profil nesnesi kullanılarak da belirtilebilir:

```powershell
$TmProfile = Get-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzTrafficManagerProfile -TrafficManagerProfile $TmProfile [-Force]
```

Bu sıra da borulu olabilir:

```powershell
Get-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Sonraki adımlar

[Trafik Yöneticisi izleme](traffic-manager-monitoring.md)

[Traffic Manager için performans konuları](traffic-manager-performance-considerations.md)
