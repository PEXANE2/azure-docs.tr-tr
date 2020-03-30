---
title: Bağlantı ve ağ sorunları
titleSuffix: Azure Cloud Services
description: Bu makalede, Microsoft Azure Bulut Hizmetleri için bağlantı ve ağ hakkında sık sorulan sorular listelanmaktadır.
services: cloud-services
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/23/2018
ms.author: genli
ms.openlocfilehash: 7caeba0e88f63106eae80f7142b5d65463f8d7a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77019409"
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure Bulut Hizmetleri için bağlantı ve ağ sorunları: Sık sorulan sorular (SSS)

Bu makalede, [Azure Bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services)için bağlantı ve ağ sorunları hakkında sık sorulan sorular yer almaktadır. Boyut bilgileri için [Bulut Hizmetleri VM boyutu sayfasına](cloud-services-sizes-specs.md)bakın.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Çoklu VIP bulut hizmetinde IP rezerve edemiyorum.
İlk olarak, IP'yi ayırmaya çalıştığınız sanal makine örneğinin açık olduğundan emin olun. İkinci olarak, hem hazırlama hem de üretim dağıtımları için ayrılmış IP'ler kullandığınızdan emin olun. *Do not* Dağıtım yükseltme sırasında ayarları değiştirmeyin.

## <a name="how-do-i-use-remote-desktop-when-i-have-an-nsg"></a>NSG'm varken Uzak Masaüstü'nü nasıl kullanırım?
NSG'ye **3389** ve **20000**bağlantı noktalarında trafiğe izin veren kurallar ekleyin. Uzak Masaüstü **3389**portu kullanır. Bulut hizmeti örnekleri yük dengeliolduğundan, hangi örne bağlanışla bağlanabileceğinizi doğrudan denetemezsiniz. *RemoteForwarder* ve *RemoteAccess* aracıları Uzak Masaüstü Protokolü (RDP) trafiğini yönetir ve istemcinin bir RDP çerezi göndermesine ve bağlanmak için ayrı bir örnek belirtmesine izin verir. *RemoteForwarder* ve *RemoteAccess* aracıları, **20000** bağlantı noktasının açılmasını gerektirir ve bu da NSG'niz varsa engellenebilir.

## <a name="can-i-ping-a-cloud-service"></a>Bir bulut hizmeti ping miyim?

Hayır, normal "ping"/ICMP protokolünü kullanarak değil. Azure yük bakiyesi aracılığıyla ICMP protokolüne izin verilmez.

Bağlantıyı test etmek için, bağlantı noktası ping'i yapmanızı öneririz. Ping.exe ICMP kullanırken, belirli bir TCP bağlantı noktasına bağlantı test etmek için PSPing, Nmap ve telnet gibi diğer araçları kullanabilirsiniz.

Daha fazla bilgi için Azure [VM bağlantısını test etmek için ICMP yerine bağlantı noktası ping'lerini kullan'a](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/)bakın.

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-might-indicate-a-malicious-attack-to-the-cloud-service"></a>Bulut hizmetine kötü amaçlı bir saldırı yı işaret edebilecek bilinmeyen IP adreslerinden binlerce isabet almasını nasıl önlerim?
Azure, platform hizmetlerini dağıtılmış hizmet reddi (DDoS) saldırılarına karşı korumak için çok katmanlı bir ağ güvenliği uygular. Azure DDoS savunma sistemi, Azure'un penetrasyon testi ile sürekli olarak geliştirilen sürekli izleme sürecinin bir parçasıdır. Bu DDoS savunma sistemi, yalnızca dışarıdan gelen saldırılara değil, diğer Azure kiracılarından gelen saldırılara da dayanacak şekilde tasarlanmıştır. Daha fazla bilgi için [Azure ağ güvenliği'ne](https://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf)bakın.

Ayrıca, belirli bazı IP adreslerini seçikez engellemek için bir başlangıç görevi de oluşturabilirsiniz. Daha fazla bilgi için [bkz.](cloud-services-startup-tasks-common.md#block-a-specific-ip-address)

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>Bulut hizmeti örneğime RDP getirmeye çalıştığımda, "Kullanıcı hesabının süresi doldu" iletisini alıyorum.
RDP ayarlarınızda yapılandırılan son kullanma tarihini atladiğinizde "Bu kullanıcı hesabının süresi doldu" hata iletisini alabilirsiniz. Portaldan son kullanma tarihini aşağıdaki adımları izleyerek değiştirebilirsiniz:

1. [Azure portalında](https://portal.azure.com)oturum açın, bulut hizmetinize gidin ve **Uzak Masaüstü** sekmesini seçin.

2. **Üretim** veya **Hazırlama** dağıtım yuvasını seçin.

3. Süresi **DolanA'yı** tarihte değiştirin ve yapılandırmayı kaydedin.

Artık makinenize RDP yapabilmeniz gerekir.

## <a name="why-is-azure-load-balancer-not-balancing-traffic-equally"></a>Azure Load Balancer, trafiği neden eşit olarak dengelemiyor?
Dahili yük dengeleyicinin nasıl çalıştığı hakkında bilgi için [Azure Yük Dengeleyicisi yeni dağıtım moduna](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/)bakın.

Kullanılan dağıtım algoritması kullanılabilir sunuculara trafik haritalamak için 5-tuple (kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası ve protokol türü) karma. Yalnızca bir taşıma oturumu içinde yapışkanlık sağlar. Aynı TCP veya UDP oturumundaki paketler, yük dengeli bitiş noktasının arkasındaki aynı veri merkezi IP (DIP) örneğine yönlendirilir. İstemci bağlantıyı kapatıp yeniden açtığında veya aynı kaynak IP'den yeni bir oturum başlattığında, kaynak bağlantı noktası değişir ve trafiğin farklı bir DIP bitiş noktasına gitmesine neden olur.

## <a name="how-can-i-redirect-incoming-traffic-to-the-default-url-of-my-cloud-service-to-a-custom-url"></a>Gelen trafiği bulut hizmetimin varsayılan URL'sine nasıl özel bir URL'ye yönlendirebilirim?

IIS'nin URL Yeniden Yazma modülü, bulut hizmeti için varsayılan URL'ye gelen trafiği \*(örneğin, .cloudapp.net) bazı özel ada/URL'ye yönlendirmek için kullanılabilir. URL Yeniden Yazma modülü varsayılan olarak web rollerinde etkinleştirildiğinden ve kuralları uygulamanın web.config'inde yapılandırıldığından, yeniden başlatma/yeniden başlatmadan bağımsız olarak VM'de her zaman kullanılabilir. Daha fazla bilgi için bkz:

- [URL Yeniden Yazma modülü için yeniden yazma kuralları oluşturma](https://docs.microsoft.com/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module)
- [Varsayılan bağlantıyı kaldırma](https://stackoverflow.com/questions/32286487/azure-website-how-to-remove-default-link?answertab=votes#tab-top)

## <a name="how-can-i-blockdisable-incoming-traffic-to-the-default-url-of-my-cloud-service"></a>Bulut hizmetimin varsayılan URL'sine gelen trafiği nasıl engelleyebilir/devre dışı düşürebilirim?

Bulut hizmetinizin varsayılan URL'sine/adına gelen trafiği engelleyebilirsiniz \*(örneğin, .cloudapp.net). Bulut hizmeti tanımındaki (*.csdef) dosyadaki\.site bağlama yapılandırması altında ana bilgisayar üstbilgisini (örneğin, www MyCloudService.com) belirtildiği gibi özel bir DNS adına ayarlayın:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicesDemo" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
    <WebRole name="MyWebRole" vmsize="Small">
        <Sites>
            <Site name="Web">
            <Bindings>
                <Binding name="Endpoint1" endpointName="Endpoint1" hostHeader="www.MyCloudService.com" />
            </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
</ServiceDefinition>
```

Bu ana bilgisayar üstbilgi bağlama csdef dosyası üzerinden zorlandığı için, hizmete yalnızca özel adı "www.MyCloudService.com" üzerinden erişilebilir. "*.cloudapp.net" etki alanına gelen tüm istekler her zaman başarısız olursa. Hizmette özel bir SLB sondası veya dahili yük dengeleyicikullanıyorsanız, hizmetin varsayılan URL'sini/adını engellemek sondalama davranışına engel olabilir.

## <a name="how-can-i-make-sure-the-public-facing-ip-address-of-a-cloud-service-never-changes"></a>Bir bulut hizmetinin genel kullanıma açık IP adresinin asla değişmediğinden nasıl emin olabilirim?

Bulut hizmetinizin genel kullanıma açık IP adresinin (VIP olarak da bilinir) birkaç özel müşteri tarafından alışılmış olarak beyaz listeye alınabilmesi için hiçbir zaman değişmediğinden emin olmak için, bu hizmetle ilişkili ayrılmış bir IP'ye sahip olmanızı öneririz. Aksi takdirde, dağıtımı silerseniz Azure tarafından sağlanan sanal IP aboneliğinizden ayrılır. Başarılı BIR VIP takas işlemi için, hem üretim hem de evreleme yuvaları için bireysel ayrılmış IP'lere ihtiyacınız vardır. Onlar olmadan, takas işlemi başarısız olur. Bir IP adresi rezerve etmek ve bulut hizmetinizle ilişkilendirmek için şu makalelere bakın:

- [Varolan bir bulut hizmetinin IP adresini rezerve edin](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip#reserve-the-ip-address-of-an-existing-cloud-service)
- [Bir hizmet yapılandırma dosyası kullanarak ayrılmış bir IP'yi bulut hizmetiyle ilişkilendirme](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

Rolleriniz için birden fazla örneğiniz varsa, RIP'i bulut hizmetinizle ilişkilendirme herhangi bir kapalı kalma süresine neden olmamalıdır. Alternatif olarak, Azure veri merkezinizin IP aralığını izin listesine ekleyebilirsiniz. Tüm Azure IP aralıklarını [Microsoft Download Center'da](https://www.microsoft.com/en-us/download/details.aspx?id=41653)bulabilirsiniz.

Bu dosya, Azure veri merkezlerinde kullanılan IP adres aralıklarını (bilgi işlem, SQL ve depolama aralıkları dahil) içerir. Güncelleştirilmiş bir dosya, şu anda dağıtılan aralıkları ve IP aralıklarında yapılacak değişiklikleri yansıtan haftalık olarak yayınlanır. Dosyada görünen yeni aralıklar veri merkezlerinde en az bir hafta boyunca kullanılmaz. Yeni .xml dosyasını her hafta indirin ve Azure'da çalışan hizmetleri doğru bir şekilde tanımlamak için sitenizde gerekli değişiklikleri gerçekleştirin. Azure ExpressRoute kullanıcıları, bu dosyanın her ayın ilk haftasında Azure alanının BGP reklamını güncelleştirmek için kullanıldığını fark edebilir.

## <a name="how-can-i-use-azure-resource-manager-virtual-networks-with-cloud-services"></a>Bulut hizmetleriyle Azure Kaynak Yöneticisi sanal ağlarını nasıl kullanabilirim?

Bulut hizmetleri Azure Kaynak Yöneticisi sanal ağlarına yerleştirilemez. Kaynak Yöneticisi sanal ağlar ve klasik dağıtım sanal ağlar bakma yoluyla bağlanabilir. Daha fazla bilgi için [Sanal ağ eşleme'ye](../virtual-network/virtual-network-peering-overview.md)bakın.


## <a name="how-can-i-get-the-list-of-public-ips-used-by-my-cloud-services"></a>Bulut Hizmetlerim tarafından kullanılan genel IP listesini nasıl alabilirim?

Aboneliğiniz altında Bulut Hizmetleri için genel IP'lerin listesini almak için PS komut dosyasını aşağıdaki şekilde kullanabilirsiniz

```powershell
$services = Get-AzureService  | Group-Object -Property ServiceName

foreach ($service in $services)
{
    "Cloud Service '$($service.Name)'"

    $deployment = Get-AzureDeployment -ServiceName $service.Name
    "VIP - " +  $deployment.VirtualIPs[0].Address
    "================================="
}
```
