---
title: Dahili sanal ağlarla Azure API Yönetimini kullanma
titleSuffix: Azure API Management
description: Dahili bir sanal ağda Azure API Yönetimi'ni nasıl ayarlayıp yapılandırılamayı öğrenin
services: api-management
documentationcenter: ''
author: vladvino
manager: kjoshi
editor: ''
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/31/2019
ms.author: apimpm
ms.openlocfilehash: 6054c595bca26dc2a0432c53369a60a61e3efde0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841872"
---
# <a name="using-azure-api-management-service-with-an-internal-virtual-network"></a>Azure API Management hizmetini iç sanal ağ ile kullanma
Azure Sanal Ağlar ile Azure API Yönetimi, Internet üzerinden erişilemeyen API'leri yönetebilir. Bağlantıyı kurmak için bir dizi VPN teknolojisi mevcuttur. API Yönetimi, sanal ağ içinde iki ana modda dağıtılabilir:
* Dış
* İç

API Yönetimi dahili sanal ağ modunda dağıtıldığında, tüm hizmet bitiş noktaları (proxy ağ geçidi, Geliştirici portalı, doğrudan yönetim ve Git) yalnızca erişimi denetlediğiniz sanal bir ağ içinde görünür. Hizmet bitiş noktalarının hiçbiri ortak DNS sunucusunda kayıtlı değildir.

> [!NOTE]
> Hizmet bitiş noktaları için DNS girişi olmadığından, DNS sanal ağ için [yapılandırılınana](#apim-dns-configuration) kadar bu uç noktalara erişilemez.

API Yönetimi'ni iç modda kullanarak aşağıdaki senaryoları elde edebilirsiniz:

* Siteden siteye veya Azure ExpressRoute VPN bağlantılarını kullanarak, ÖZEL veri merkezinizde API'lerin site dışından üçüncü taraflarca güvenli bir şekilde erişilebilir olmasını sağlayın.
* Bulut tabanlı API'lerinizi ve şirket içi API'lerinizi ortak bir ağ geçidi aracılığıyla teşhir ederek karma bulut senaryolarını etkinleştirin.
* Tek bir ağ geçidi bitiş noktası kullanarak birden çok coğrafi konumda barındırılan API'lerinizi yönetin.

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunları yapmalısınız:

+ **Etkin bir Azure aboneliği.**

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ **Azure API Yönetimi örneği.** Daha fazla bilgi için [bkz.](get-started-create-service-instance.md)
+ Bir API Yönetimi hizmeti sanal ağda dağıtıldığında, [bağlantı noktalarının listesi](./api-management-using-with-vnet.md#required-ports) kullanılır ve açılması gerekir. 

## <a name="creating-an-api-management-in-an-internal-virtual-network"></a><a name="enable-vpn"> </a>Dahili sanal ağda API Yönetimi oluşturma
Dahili sanal ağdaki API Yönetimi hizmeti, [bir dahili yük dengeleyicisinin (klasik)](https://docs.microsoft.com/azure/load-balancer/load-balancer-get-started-ilb-classic-cloud)arkasında barındırılır. Bu tek seçenektir ve değiştirilemez.

### <a name="enable-a-virtual-network-connection-using-the-azure-portal"></a>Azure portalını kullanarak sanal ağ bağlantısını etkinleştirme

1. [Azure portalında](https://portal.azure.com/)Azure API Yönetimi örneğinize göz atın.
2. **Sanal ağı**seçin.
3. SANAL AĞ Içinde dağıtılacak API Yönetimi örneğini yapılandırın.

    ![Dahili bir sanal ağda Azure API Yönetimi oluşturma menüsü][api-management-using-internal-vnet-menu]

4. **Kaydet'i**seçin.

Dağıtım başarılı olduktan sonra, genel bakış bıçağında API Yönetimi hizmetinizin **özel** sanal IP adresini ve **genel** sanal IP adresini görmeniz gerekir. **Özel** sanal IP adresi, API Yönetimi'nin devrettiği alt ağ `gateway`içinden gelen ve , ve `portal` `management` `scm` uç noktalarına erişilebilen bir yük dengeli IP adresidir. **Genel** sanal IP adresi **yalnızca** 3443 bağlantı noktası üzerinde uç noktasına giden düzlem trafiğini kontrol etmek `management` için kullanılır ve [ApiManagement][ServiceTags] servis etiketine kilitlenebilir.

![Yapılandırılmış bir iç sanal ağ ile API Yönetimi panosu][api-management-internal-vnet-dashboard]

> [!NOTE]
> Azure Portalı'nda bulunan Test konsolu, Genel DNS'de Kayıtlı olmadığı için **Dahili** VNET dağıtılmış hizmet için çalışmaz. Bunun yerine **Geliştirici portalında**sağlanan Test Konsolu kullanmalısınız.

### <a name="enable-a-virtual-network-connection-by-using-powershell-cmdlets"></a>PowerShell cmdlets kullanarak sanal ağ bağlantısı etkinleştirin

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell cmdlets kullanarak sanal ağ bağlantısını da etkinleştirebilirsiniz.

* Sanal ağ içinde bir API Yönetimi hizmeti oluşturun: Sanal ağ içinde bir Azure API Yönetimi hizmeti oluşturmak ve dahili sanal ağ türünü kullanacak şekilde yapılandırmak için cmdlet [New-AzApiManagement'ı](/powershell/module/az.apimanagement/new-azapimanagement) kullanın.

* Sanal ağ içinde bir API Yönetimi hizmetinin varolan dağıtımını güncelleştirin: Varolan bir API Yönetimi hizmetini sanal ağ içinde taşımak ve dahili sanal ağ türünü kullanacak şekilde yapılandırmak için cmdlet [Update-AzApiManagementRegion'ı](/powershell/module/az.apimanagement/update-azapimanagementregion) kullanın.

## <a name="dns-configuration"></a><a name="apim-dns-configuration"></a>DNS yapılandırması
API Yönetimi harici sanal ağ modundayken, DNS Azure tarafından yönetilir. Dahili sanal ağ modu için, kendi yönlendirme yönetmek zorunda.

> [!NOTE]
> API Yönetimi hizmeti, IP adreslerinden gelen istekleri dinlemez. Yalnızca hizmet bitiş noktalarında yapılandırılan ana bilgisayar adına gelen isteklere yanıt verir. Bu uç noktalar ağ geçidi, Azure portalı ve Geliştirici portalı, doğrudan yönetim bitiş noktası ve Git'i içerir.

### <a name="access-on-default-host-names"></a>Varsayılan ana bilgisayar adlarında erişim
Örneğin "contosointernalvnet" adlı bir API Yönetimi hizmeti oluşturduğunuzda, aşağıdaki hizmet uç noktaları varsayılan olarak yapılandırılır:

   * Ağ geçidi veya proxy: contosointernalvnet.azure-api.net

   * Geliştirici portalı: contosointernalvnet.portal.azure-api.net

   * Yeni Geliştirici portalı: contosointernalvnet.developer.azure-api.net

   * Doğrudan yönetim bitiş noktası: contosointernalvnet.management.azure-api.net

   * Git: contosointernalvnet.scm.azure-api.net

Bu API Yönetimi hizmet uç noktalarına erişmek için, API Yönetimi'nin dağıtıldığı sanal ağa bağlı bir alt ağda sanal bir makine oluşturabilirsiniz. Hizmetinizin dahili sanal IP adresinin 10.1.0.5 olduğunu varsayarsak, ana bilgisayar ları dosyasını (%SystemDrive%\drivers\etc\hosts) aşağıdaki gibi eşleyebilirsiniz:

   * 10.1.0.5 contosointernalvnet.azure-api.net

   * 10.1.0.5 contosointernalvnet.portal.azure-api.net

   * 10.1.0.5 contosointernalvnet.developer.azure-api.net

   * 10.1.0.5 contosointernalvnet.management.azure-api.net

   * 10.1.0.5 contosointernalvnet.scm.azure-api.net

Daha sonra oluşturduğunuz sanal makineden tüm hizmet uç noktalarına erişebilirsiniz.
Sanal ağda özel bir DNS sunucusu kullanıyorsanız, A DNS kayıtları oluşturabilir ve bu uç noktalara sanal ağınızın herhangi bir yerinden erişebilirsiniz.

### <a name="access-on-custom-domain-names"></a>Özel alan adlarında erişim

1. Varsayılan ana bilgisayar adlarıyla API Yönetimi hizmetine erişmek istemiyorsanız, aşağıdaki resimde gösterildiği gibi tüm hizmet uç noktanız için özel alan adları ayarlayabilirsiniz:

   ![API Yönetimi için özel bir etki alanı ayarlama][api-management-custom-domain-name]

2. Ardından, yalnızca sanal ağınızdan erişilebilen uç noktalara erişmek için DNS sunucunuzda kayıtlar oluşturabilirsiniz.

## <a name="routing"></a><a name="routing"> </a> Yönlendirme

* Alt net aralığından bir yük dengeli *özel* sanal IP adresi rezerve edilecek ve sanal ağ içinden API Yönetimi hizmet uç noktalarına erişmek için kullanılır. Bu *özel* IP adresi, Azure portalındaki hizmete genel bakış bıçağında bulunabilir. Bu adres, sanal ağ tarafından kullanılan DNS sunucularına kaydedilmelidir.
* 3443 portu üzerinden yönetim hizmeti bitiş noktasına erişim sağlamak için yük dengeli *genel* IP adresi (VIP) de rezerve edilecektir. Bu *genel* IP adresi, Azure portalındaki hizmete genel bakış bıçağında bulunabilir. *Genel* IP adresi yalnızca bağlantı noktası 3443 üzerinde `management` uç noktaya giden düzlem trafiğini kontrol etmek için kullanılır ve [ApiManagement][ServiceTags] servis etiketine kilitlenebilir.
* Alt ağ IP aralığındaki (DIP) IP adresleri hizmetteki her VM'ye atanır ve sanal ağdaki kaynaklara erişmek için kullanılır. Sanal ağ dışındaki kaynaklara erişmek için genel bir IP adresi (VIP) kullanılacaktır. SANAL ağdaki kaynakları güvence altına almak için IP kısıtlama listeleri kullanılıyorsa, API Yönetimi hizmetinin dağıtıldığı alt ağ için tüm aralık, hizmetten erişimi vermek veya kısıtlamak için belirtilmelidir.
* Yük dengeli genel ve özel IP adresleri Azure portalındaki Genel Bakış bıçağında bulunabilir.
* Hizmet kaldırılır ve daha sonra sanal ağa geri eklenirse, genel ve özel erişim için atanan IP adresleri değişebilir. Bu durumda, sanal ağ içinde DNS kayıtlarını, yönlendirme kurallarını ve IP kısıtlama listelerini güncelleştirmek gerekebilir.

## <a name="related-content"></a><a name="related-content"> </a>İlgili içerik
Daha fazla bilgi için aşağıdaki makalelere bakın:
* [Sanal ağda Azure API Yönetimi'ni kurarken sık karşılaşılan ağ yapılandırma sorunları][Common network configuration problems]
* [Sanal ağ SSS'leri](../virtual-network/virtual-networks-faq.md)
* [DNS'de kayıt oluşturma](/previous-versions/windows/it-pro/windows-2000-server/bb727018(v=technet.10))

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-using-with-internal-vnet.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: get-started-create-service-instance.md
[Common network configuration problems]: api-management-using-with-vnet.md#network-configuration-issues

[ServiceTags]: ../virtual-network/security-overview.md#service-tags

