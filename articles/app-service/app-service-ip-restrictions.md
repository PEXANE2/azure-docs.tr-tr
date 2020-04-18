---
title: Azure App Service erişim kısıtlamaları
description: Erişim kısıtlamaları belirterek Azure Uygulama Hizmeti'nde uygulamanızı nasıl güvene aldığınızı öğrenin.
author: ccompy
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 298555da2056bc4c16d4d7b16615604f9798b91b
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81639279"
---
# <a name="azure-app-service-access-restrictions"></a>Azure App Service erişim kısıtlamaları

Erişim kısıtlamaları, uygulamanıza ağ erişimini kontrol eden öncelikli bir sıralı izin/reddet listesi tanımlamanıza olanak tanır. Listede IP adresleri veya Azure Sanal Ağ alt ağları bulunabilir. Bir veya daha fazla giriş olduğunda, listenin sonunda bulunan örtük bir "tüm reddet" vardır.

Erişim kısıtlamaları özelliği, uygulama hizmetibarındırılan tüm iş yükleri de dahil olmak üzere çalışır; web uygulamaları, API uygulamaları, Linux uygulamaları, Linux konteyner uygulamaları ve Fonksiyonlar.

Uygulamanıza bir istekyapıldığında, FROM adresi erişim kısıtlamaları listenizdeki IP adresi kurallarına göre değerlendirilir. FROM adresi, microsoft.web'e hizmet bitiş noktalarıyla yapılandırılan bir alt ağdaysa, kaynak alt ağ erişim kısıtlamaları listenizdeki sanal ağ kurallarıyla karşılaştırılır. Adrese listedeki kurallara göre erişim izni verilmezse, hizmet [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) durum koduyla yanıt verir.

Erişim kısıtlamaları özelliği, kodunuzu çalıştırdığı alt ana bilgisayarların yukarısına gelen Uygulama Hizmeti ön uç rollerinde uygulanır. Bu nedenle, erişim kısıtlamaları etkili ağ ALA'ları vardır.

Bir Azure Sanal Ağı'ndan (VNet) web uygulamanıza erişimi kısıtlama özelliğine [hizmet bitiş noktaları][serviceendpoints]denir. Hizmet bitiş noktaları, çok kiracılı bir hizmete erişimi seçili alt ağlardan kısıtlamanızı sağlar. Hem ağ tarafında hem de etkinleştirildiği hizmette etkinleştirilmelidir. Bir Uygulama Hizmeti Ortamında barındırılan uygulamalarla trafiği kısıtlamak için çalışmaz. Bir Uygulama Hizmet Ortamındaysanız, IP adresi kurallarıyla uygulamanıza erişimi kontrol edebilirsiniz.

![erişim kısıtlamaları akışı](media/app-service-ip-restrictions/access-restrictions-flow.png)

## <a name="adding-and-editing-access-restriction-rules-in-the-portal"></a>Portalda erişim kısıtlama kuralları ekleme ve düzenleme ##

Uygulamanıza erişim kısıtlaması kuralı eklemek için, **Ağ**>**Erişim Kısıtlamaları'nı** açmak için menüyü kullanın ve **Access Kısıtlamalarını Yapılandırma'yı** tıklatın

![Uygulama Hizmeti ağ seçenekleri](media/app-service-ip-restrictions/access-restrictions.png)  

Erişim Kısıtlamaları UI'sinden, uygulamanız için tanımlanan erişim kısıtlama kuralları listesini inceleyebilirsiniz.

![liste erişim kısıtlamaları](media/app-service-ip-restrictions/access-restrictions-browse.png)

Liste, uygulamanızdaki tüm geçerli kısıtlamaları gösterir. Uygulamanızda VNet kısıtlaması varsa, Microsoft.Web için hizmet bitiş noktaları etkin olup olmadığını tablo gösterir. Uygulamanızda tanımlı bir kısıtlama olmadığında, uygulamanıza her yerden erişilebilir.  

## <a name="adding-ip-address-rules"></a>IP adresi kuralları ekleme

Yeni bir erişim kısıtlaması kuralı eklemek için **[+] Ekle kuralına** tıklayabilirsiniz. Bir kural eklediğinizde, hemen etkili olur. Kurallar, en düşük sayıdan başlayıp yukarı yada öncelik sırasına göre uygulanır. Tek bir kural bile eklediğinizde yürürlükte olan her şeyi üstü kapalı bir şekilde inkar edebilirsiniz.

Bir kural oluştururken, izin ver/reddet ve ayrıca kural türünü seçmeniz gerekir. Ayrıca öncelik değerini ve erişimi kısıtlayan değeri sağlamanız gerekir.  İsteğe bağlı olarak bir ad ve kurala açıklama ekleyebilirsiniz.  

![IP erişim kısıtlama kuralı ekleme](media/app-service-ip-restrictions/access-restrictions-ip-add.png)

IP adresi tabanlı bir kural belirlemek için bir IPv4 veya IPv6 türünü seçin. IP Adresi gösterimi, hem IPv4 hem de IPv6 adresleri için CIDR gösteriminde belirtilmelidir. Tam bir adres belirtmek için, ilk dört sekizlinin IP adresinizi temsil ettiği ve /32'nin maske olduğu 1.2.3.4/32 gibi bir şey kullanabilirsiniz. Tüm adresler için IPv4 CIDR gösterimi 0.0.0.0/0'dır. CIDR gösterimi hakkında daha fazla bilgi edinmek için [Sınıfsız Etki Alanları Yönlendirme'yi](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)okuyabilirsiniz. 

## <a name="service-endpoints"></a>Hizmet uç noktaları

Hizmet bitiş noktaları, seçili Azure sanal ağ alt ağlarına erişimi kısıtlamanızı sağlar. Belirli bir alt ağa erişimi kısıtlamak için, sanal ağ türüyle bir kısıtlama kuralı oluşturun. Erişime izin vermek veya reddetmek istediğiniz aboneliği, VNet'i ve alt ağı seçebilirsiniz. Seçtiğiniz alt ağ için Microsoft.Web ile hizmet bitiş noktaları zaten etkinleştirilmezse, bunu yapmamasını isteyen kutuyu onaylamadığınız sürece otomatik olarak sizin için etkinleştirilir. Uygulamada etkinleştirmek istediğiniz ancak alt ağda olmayan durum, alt ağda hizmet bitiş noktalarını etkinleştirme izinleriniz olup olmadığıyla büyük ölçüde ilişkilidir. Alt ağda hizmet uç noktalarını etkinleştirecek başka birini bulmanız gerekiyorsa, kutuyu işaretleyebilir ve uygulamanızın daha sonra alt ağda etkinleştirilmesini beklentisiyle hizmet bitiş noktaları için yapılandırabilirsiniz. 

![VNet erişim kısıtlama kuralı ekleme](media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

Hizmet bitiş noktaları, Bir Uygulama Hizmeti Ortamında çalışan uygulamalara erişimi kısıtlamak için kullanılamaz. Uygulamanız Bir Uygulama Hizmet Ortamındayken, IP erişim kurallarıyla uygulamanıza erişimi kontrol edebilirsiniz. 

Hizmet bitiş noktalarıyla uygulamanızı Uygulama Ağ Geçitleri veya diğer WAF aygıtlarıyla yapılandırabilirsiniz. Çok katmanlı uygulamaları güvenli arka uçlarla da yapılandırabilirsiniz. Bazı olasılıklar hakkında daha fazla bilgi için [Ağ özellikleri ve Uygulama Hizmeti ve](networking-features.md) Uygulama Ağ Geçidi [tümleştirmesini hizmet bitiş noktalarıyla](networking/app-gateway-with-service-endpoints.md)okuyun.

## <a name="managing-access-restriction-rules"></a>Erişim kısıtlama kurallarını yönetme

Varolan bir erişim kısıtlaması kuralını yönetmek için herhangi bir satırı tıklatabilirsiniz. Değişiklikler, öncelik sıralamadaki değişiklikler de dahil olmak üzere hemen etkilidir.

![erişim kısıtlaması kuralını yönetme](media/app-service-ip-restrictions/access-restrictions-ip-edit.png)

Bir kuralı değiştirdiğinizde, IP adresi kuralı ile Sanal Ağ kuralı arasındaki türü değiştiremezsiniz. 

![erişim kısıtlaması kuralını yönetme](media/app-service-ip-restrictions/access-restrictions-vnet-edit.png)

Bir kuralı silmek için kuralınızdaki **...** 'u tıklatın ve sonra **Kaldır'ı**tıklatın.

![erişim kısıtlaması kuralını silme](media/app-service-ip-restrictions/access-restrictions-delete.png)

## <a name="blocking-a-single-ip-address"></a>Tek bir IP adresini engelleme ##

İlk IP Kısıtlama kuralınızı eklerken, hizmet 2147483647 önceliği olan açık bir **Reddet kuralı** ekler. Uygulamada, açık **Reddet tüm** kuralı son kural yürütülür ve açıkça izin **kuralı** kullanılarak izin verilmeyen herhangi bir IP adresine erişimi engeller.

Kullanıcıların tek bir IP adresini veya IP adresi bloğunu açıkça engellemek, ancak diğer her şeye erişmesine izin vermek istedikleri senaryo için, açık bir **Herkese İzin Ver** kuralı eklemek gerekir.

![blok tek ip adresi](media/app-service-ip-restrictions/block-single-address.png)

## <a name="scm-site"></a>SCM sitesi 

Uygulamanıza erişimi kontrol edebilmenin yanı sıra, uygulamanız tarafından kullanılan scm sitesine erişimi de kısıtlayabilirsiniz. Scm sitesi web dağıtmak bitiş noktası ve aynı zamanda Kudu konsoludur. Uygulamadan scm sitesine erişim kısıtlamaları atayabilir veya hem uygulama hem de scm sitesi için aynı seti kullanabilirsiniz. Uygulamanızla aynı kısıtlamalara sahip olacak şekilde kutuyu işaretlediğinizde, her şey boşa bırakılır. Kutunun üzerindeki işaretini kaldırırsanız, scm sitesinde daha önce sahip olduğunuz ayarlar uygulanır. 

![liste erişim kısıtlamaları](media/app-service-ip-restrictions/access-restrictions-scm-browse.png)

## <a name="programmatic-manipulation-of-access-restriction-rules"></a>Erişim kısıtlama kurallarının programlı manipülasyonu ##

[Azure CLI](https://docs.microsoft.com/cli/azure/webapp/config/access-restriction?view=azure-cli-latest) ve [Azure PowerShell](https://docs.microsoft.com/powershell/module/Az.Websites/Add-AzWebAppAccessRestrictionRule?view=azps-3.1.0) erişim kısıtlamalarını düzenlemek için destek sağlar. Azure CLI kullanarak erişim kısıtlaması ekleme örneği:

```azurecli-interactive
az webapp config access-restriction add --resource-group ResourceGroup --name AppName \
    --rule-name 'IP example rule' --action Allow --ip-address 122.133.144.0/24 --priority 100
```
Azure PowerShell kullanarak erişim kısıtlaması ekleme örneği:

```azurepowershell-interactive
Add-AzWebAppAccessRestrictionRule -ResourceGroupName "ResourceGroup" -WebAppName "AppName"
    -Name "Ip example rule" -Priority 100 -Action Allow -IpAddress 122.133.144.0/24
```

Değerler, Kaynak Yöneticisi'ndeki uygulama yapılandırmasında ki Bir [Azure REST API](https://docs.microsoft.com/rest/api/azure/) PUT işlemiyle veya Azure Kaynak Yöneticisi şablonu kullanılarak el ile ayarlanabilir. Örnek olarak, gerekli JSON'u eklemek için resources.azure.com kullanabilir ve ipSecurityRestrictions bloğunu edinebilirsiniz.

Kaynak Yöneticisi'ndeki bu bilgilerin konumu:

management.azure.com/subscriptions/**abonelik kimliği**/kaynakGrupları/ kaynak**grupları**/sağlayıcılar/Microsoft.Web/sites/ web**uygulaması adı**/config/web?api-version=2018-02-01

Önceki örnek için JSON sözdizimi:
```json
{
  "properties": {
    "ipSecurityRestrictions": [
      {
        "ipAddress": "122.133.144.0/24",
        "action": "Allow",
        "priority": 100,
        "name": "IP example rule"
      }
    ]
  }
}
```

## <a name="azure-functions-access-restrictions"></a>Azure İşlevler erişim kısıtlamaları

Uygulama Hizmeti planlarıyla aynı işlevsellik teki işlev uygulamaları için erişim kısıtlamaları da mevcuttur. Erişim kısıtlamalarını etkinleştirmek, izin verilmeyen IP'ler için portal kodu düzenleyicisini devre dışı bırakmaz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure İşlevleri için erişim kısıtlamaları](../azure-functions/functions-networking-options.md#inbound-ip-restrictions)

[Hizmet bitiş noktalarıyla uygulama ağ geçidi tümleştirmesi](networking/app-gateway-with-service-endpoints.md)

<!--Links-->
[serviceendpoints]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview
