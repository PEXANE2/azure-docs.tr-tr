---
title: Hizmet bitiş noktalarıyla Uygulama Ağ Geçidi tümleştirmesi - Azure Uygulama Hizmeti | Microsoft Dokümanlar
description: Uygulama Ağ Geçidi'nin hizmet bitiş noktalarıyla güvenli Azure Uygulama Hizmeti ile nasıl entegre olduğunu açıklar.
services: app-service
documentationcenter: ''
author: madsd
manager: ccompy
editor: ''
ms.assetid: 073eb49c-efa1-4760-9f0c-1fecd5c251cc
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 12/09/2019
ms.author: madsd
ms.custom: seodec18
ms.openlocfilehash: 5e32baa10e98f0f57a861f8cebfb7506ad615631
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74980068"
---
# <a name="application-gateway-integration-with-service-endpoints"></a>Hizmet bitiş noktalarıyla uygulama ağ geçidi tümleştirmesi
Azure Application Gateway ile tümleştirmenin biraz farklı bir yapılandırmasını gerektiren Uygulama Hizmeti'nin üç varyasyonu vardır. Varyasyonlar düzenli App Service içerir - aynı zamanda çok kiracı olarak bilinen, Dahili Yük Dengeleyici (ILB) App Service Environment (ASE) ve Dış ASE. Bu makalede, Uygulama Hizmeti (çok kiracı) ile yapılandırmak ve ILB ve Dış ASE ile ilgili hususlar tartışmak nasıl üzerinden yürüyecek.

## <a name="integration-with-app-service-multi-tenant"></a>Uygulama Hizmeti ile tümleştirme (çok kiracılı)
Uygulama Hizmeti (çok kiracı) bir kamu internet bitiş noktasına bakan vardır. [Hizmet bitiş noktalarını](../../virtual-network/virtual-network-service-endpoints-overview.md) kullanarak yalnızca Azure Sanal Ağı içindeki belirli bir alt ağdan trafiğe izin verebilir ve diğer her şeyi engelleyebilirsiniz. Aşağıdaki senaryoda, bir Uygulama Hizmeti örneğinin yalnızca belirli bir Uygulama Ağ Geçidi örneğinden trafik alabilmesi için bu işlevi kullanırız.

![Uygulama Hizmeti ile Uygulama Ağ Geçidi entegrasyonu](./media/app-gateway-with-service-endpoints/service-endpoints-appgw.png)

Bu yapılandırmanın Uygulama Hizmeti ve Uygulama Ağ Geçidi oluşturmanın yanı sıra iki bölümü vardır. İlk bölüm, Uygulama Ağ Geçidi'nin dağıtıldığı Sanal Ağ'ın alt ağındaki hizmet uç noktalarını etkinleştirmektir. Hizmet bitiş noktaları, alt ağdan Uygulama Hizmeti'ne doğru çıkan tüm ağ trafiğinin belirli subnet kimliğiyle etiketleneceğini garanti edecektir. İkinci bölüm, yalnızca bu belirli alt net kimliğiyle etiketlenmiş trafiğe izin verilen bir web uygulamasına erişim kısıtlaması ayarlamaktır. Tercihe bağlı olarak farklı araçlar kullanarak yapılandırabilirsiniz.

## <a name="using-azure-portal"></a>Azure portalını kullanma
Azure portalı ile, kurulumu sağlamak ve yapılandırmak için dört adımı izlersiniz. Varolan kaynaklarınız varsa, ilk adımları atlayabilirsiniz.
1. [Örneğin,.Net Core Quickstart](../../app-service/app-service-web-get-started-dotnet.md) gibi, Uygulama Hizmeti belgelerindeki Quickstarts'lardan birini kullanarak bir Uygulama Hizmeti oluşturun
2. [Quickstart portalını](../../application-gateway/quick-create-portal.md)kullanarak bir Uygulama Ağ Geçidi oluşturun, ancak arka uç hedeflerini ekle bölümünü atlayın.
3. Uygulama [Ağ Geçidi'nde Uygulama Hizmetini arka uç olarak](../../application-gateway/configure-web-app-portal.md)yapılandırın, ancak erişimi kısıtla bölümünü atlayın.
4. Son olarak [hizmet uç noktalarını kullanarak erişim kısıtlaması](../../app-service/app-service-ip-restrictions.md#service-endpoints)oluşturun.

Artık Uygulama Ağ Geçidi üzerinden Uygulama Hizmetine erişebilirsiniz, ancak Uygulama Hizmetine doğrudan erişmeye çalışırsanız, web sitesinin durdurulduğunu belirten bir 403 HTTP hatası almanız gerekir.

![Uygulama Hizmeti ile Uygulama Ağ Geçidi entegrasyonu](./media/app-gateway-with-service-endpoints/web-site-stopped.png)

## <a name="using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanma
[Kaynak Yöneticisi dağıtım şablonu][template-app-gateway-app-service-complete] tam bir senaryo sağlar. Senaryo, yalnızca Uygulama Ağ Geçidi'nden trafik almak için hizmet bitiş noktaları ve erişim kısıtlamasıyla kilitlenmiş bir Uygulama Hizmeti örneğinden oluşur. Şablon, kaynak adlarına eklenen birçok Akıllı Varsayılan ve benzersiz posta düzeltmelerini içerir. Bunları geçersiz kılmak için, repo'yu klonlamanız veya şablonu indirmeniz ve yeniden oluşturmanız gerekir. 

Şablonu uygulamak için şablonun açıklamasında bulunan Azure'a Dağıt düğmesini kullanabilir veya uygun PowerShell/CLI'yi kullanabilirsiniz.

## <a name="using-azure-command-line-interface"></a>Azure Komut Satırı Arabirimini Kullanma
[Azure CLI örneği,](../../app-service/scripts/cli-integrate-app-service-with-application-gateway.md) yalnızca Uygulama Ağ Geçidi'nden trafik almak için hizmet bitiş noktaları ve erişim kısıtlamasıyla kilitlenmiş bir Uygulama Hizmeti sağlayacaktır. Trafiği varolan bir Uygulama Ağ Geçidi'nden varolan bir Uygulama Hizmetine yalıtmanız gerekiyorsa, aşağıdaki komut yeterlidir.

```azurecli-interactive
az webapp config access-restriction add --resource-group myRG --name myWebApp --rule-name AppGwSubnet --priority 200 --subnet mySubNetName --vnet-name myVnetName
```

Varsayılan yapılandırmada, komut hem alt ağdaki hizmet bitiş noktası yapılandırmasının ayarını hem de Uygulama Hizmetindeki erişim kısıtlamasını sağlar.

## <a name="considerations-for-ilb-ase"></a>ILB ASE için Dikkat Edilecek Hususlar
ILB ASE, örnek le Uygulama Ağ Geçidi arasındaki internet ve trafiğe maruz kalmamış sayılmadığı için sanal ağiçin zaten izole edilmiştir. Aşağıdaki [nasıl yapIlir kılavuzu,](../environment/integrate-with-application-gateway.md) bir ILB ASE'yi yapılandırır ve Azure portalını kullanarak bir Uygulama Ağ Geçidi ile tümleştirir. 

Yalnızca Uygulama Ağ Geçidi alt ağındaki trafiğin ASE'ye ulaştığından emin olmak istiyorsanız, ASE'deki tüm web uygulamalarını etkileyen bir Ağ güvenlik grubu (NSG) yapılandırabilirsiniz. NSG için, alt net IP aralığını ve isteğe bağlı olarak bağlantı noktalarını (80/443) belirtebilirsiniz. ASE'nin düzgün çalışması için [gerekli NSG kurallarını](../environment/network-info.md#network-security-groups) geçersiz kılmadığınızdan emin olun.

Tek bir web uygulamasına trafiği yalıtmak için, hizmet bitiş noktaları ASE için çalışmayacağı için ip tabanlı erişim kısıtlamalarını kullanmanız gerekir. IP adresi, Uygulama Ağ Geçidi örneğinin özel IP'si olmalıdır.

## <a name="considerations-for-external-ase"></a>Dış ASE için Dikkat Edilecek Hususlar
Harici ASE, çok kiracılı Uygulama Hizmeti gibi kamuya açık bir yük dengeleyiciye sahiptir. Hizmet bitiş noktaları ASE için çalışmaz ve bu nedenle Uygulama Ağ Geçidi örneğinin genel IP'sini kullanarak ip tabanlı erişim kısıtlamaları kullanmanız gerekir. Azure portalını kullanarak harici bir ASE oluşturmak için bu [Quickstart'ı](../environment/create-external-ase.md) takip edebilirsiniz

[template-app-gateway-app-service-complete]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-app-gateway-v2/ "Tam senaryo için Azure Kaynak Yöneticisi şablonu"

## <a name="considerations-for-kuduscm-site"></a>Kudu/scm sitesi için dikkat edilmesi gerekenler
Kudu olarak da bilinen scm sitesi, her web uygulaması için var olan bir yönetici sitesidir. Bu proxy scm site ters mümkün değildir ve büyük olasılıkla da tek tek IP adresleri veya belirli bir alt net kilitlemek istiyorum.

Ana siteyle aynı erişim kısıtlamalarını kullanmak istiyorsanız, aşağıdaki komutu kullanarak ayarları devralabilirsiniz.

```azurecli-interactive
az webapp config access-restriction set --resource-group myRG --name myWebApp --use-same-restrictions-for-scm-site
```

SCM sitesi için tek tek erişim kısıtlamaları ayarlamak istiyorsanız, aşağıda gösterildiği gibi --scm-site bayrağını kullanarak erişim kısıtlamaları ekleyebilirsiniz.

```azurecli-interactive
az webapp config access-restriction add --resource-group myRG --name myWebApp --scm-site --rule-name KudoAccess --priority 200 --ip-address 208.130.0.0/16
```

## <a name="next-steps"></a>Sonraki adımlar
Uygulama Hizmet Ortamı hakkında daha fazla bilgi [için, Uygulama Hizmeti Ortamı belgelerine](https://docs.microsoft.com/azure/app-service/environment)bakın.

Web uygulamanızı daha da güvenli hale getirmek için, Uygulama Ağ Geçidi'ndeki Web Uygulaması Güvenlik Duvarı hakkındaki bilgileri [Azure Web Uygulaması Güvenlik Duvarı belgelerinde](../../web-application-firewall/ag/ag-overview.md)bulabilirsiniz.