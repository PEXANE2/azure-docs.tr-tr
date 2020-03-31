---
title: Azure sanal ağ hizmet uç noktası ilkeleri | Microsoft Docs
description: Hizmet Uç Noktası İlkelerini kullanarak Azure hizmet kaynaklarına yönelik Sanal Ağ trafiğini filtrelemeyi öğrenin
services: virtual-network
documentationcenter: na
author: RDhillon
ms.service: virtual-network
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2020
ms.author: rdhillon
ms.openlocfilehash: 926da07ffaf0c61ca2a7fd02351ef3635ec4d73b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651312"
---
# <a name="virtual-network-service-endpoint-policies-for-azure-storage"></a>Azure Depolama için sanal ağ hizmeti bitiş noktası ilkeleri

Sanal Ağ (VNet) hizmet bitiş noktası ilkeleri, hizmet bitiş noktası üzerinden Azure Depolama hesaplarına sanal ağ trafiğini filtrelemenize ve yalnızca belirli Azure Depolama hesaplarına veri sızmasına izin vermenize olanak tanır. Uç nokta ilkeleri, hizmet bitiş noktası üzerinden bağlanırken Azure Depolama'ya sanal ağ trafiği için parçalı erişim denetimi sağlar.

![Azure Depolama hesaplarına Sanal ağ giden trafiğini güvence altına alma](./media/virtual-network-service-endpoint-policies-overview/vnet-service-endpoint-policies-overview.png)

Bu özellik genellikle tüm genel __Azure bölgelerinde__ __Azure Depolama__ için kullanılabilir.

## <a name="key-benefits"></a>Önemli avantajlar

Sanal ağ hizmet uç noktası ilkeleri aşağıdaki avantajları sağlar:

- __Sanal Ağ trafiğinizin Azure Depolama'ya geliştirilmiş güvenliği__

  [Ağ güvenlik grupları için Azure hizmet etiketleri,](https://aka.ms/servicetags) sanal ağ giden trafiğini belirli Azure Depolama bölgeleriyle sınırlamanıza olanak tanır. Ancak bu, seçili Azure Depolama bölgesindeki herhangi bir hesaba trafik yapılmasına izin verir.
  
  Uç nokta ilkeleri, sanal ağ giden erişime izin verilen Azure Depolama hesaplarını belirtmenize olanak tanır ve diğer tüm depolama hesaplarına erişimi kısıtlar. Bu, sanal ağınızdaki verileri korumak için çok daha ayrıntılı güvenlik denetimi sağlar.

- __Azure hizmet trafiğini filtrelemek için ölçeklenebilir, yüksek kullanılabilirliğe sahip ilkeler__

   Uç nokta ilkeleri, hizmet uç noktaları üzerinden sanal ağlardan gelen Azure hizmeti trafiğini filtrelemek için yatay olarak ölçeklenebilir, yüksek kullanılabilirliğe sahip bir çözüm sağlar. Sanal ağlarınızda bu trafiğe yönelik merkezi ağ aletleri bulundurmak için ek yük gerekmez.

## <a name="json-object-for-service-endpoint-policies"></a>Hizmet Bitiş Noktası ilkeleri için JSON Nesnesi
Service Endpoint İlkesi nesnesine hızlıca bir göz atalım.

```json
"serviceEndpointPolicyDefinitions": [
    {
            "description": null,
            "name": "MySEP-Definition",
            "resourceGroup": "MySEPDeployment",
            "service": "Microsoft.Storage",
            "serviceResources": [ 
                    "/subscriptions/subscriptionID/resourceGroups/MySEPDeployment/providers/Microsoft.Storage/storageAccounts/mystgacc"
            ],
            "type": "Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions"
    }
]
```

## <a name="configuration"></a>Yapılandırma

-   Sanal ağ trafiğini belirli Azure Depolama hesaplarıyla sınırlamak için bitiş noktası ilkelerini yapılandırabilirsiniz.
-   Uç nokta ilkesi bir sanal ağ içindeki alt ağ üzerinde yapılandırılır. İlkeyi uygulamak için alt ağda Azure Depolama için hizmet bitiş noktaları etkinleştirilmelidir.
-   Uç nokta ilkesi, resourceID biçimini kullanarak listeye izin vermek için belirli Azure Depolama hesapları eklemenize olanak tanır. Erişimi kısıtlayabilirsiniz
    - abonelikteki tüm depolama hesapları<br>
      `E.g. /subscriptions/subscriptionId`

    - kaynak grubundaki tüm depolama hesapları<br>
      `E.g. subscriptions/subscriptionId/resourceGroups/resourceGroupName`
     
    - ilgili Azure Kaynak Yöneticisi kaynağını listeleyerek tek bir depolama hesabı. Bu bloblara, tablolara, kuyruklara, dosyalara ve Azure Data Lake Storage 2. Nesil'e yönelik trafiği kapsar. <br>
    `E.g. /subscriptions/subscriptionId/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/storageAccountName`
-   Varsayılan olarak, uç noktaları olan bir alt ağa hiçbir ilke eklenmezse, hizmetteki tüm depolama hesaplarına erişebilirsiniz. Alt ağda bir ilke yapılandırıldıktan sonra, o alt ağdaki işlem örneklerinden yalnızca ilkede belirtilen kaynaklara erişilebilir. Diğer tüm depolama hesaplarına erişim reddedilir.
-   Bir alt ağda Hizmet Bitiş Noktası ilkeleri uygulanırken, Azure Depolama *Hizmeti Bitiş Noktası kapsamı* bölgeselden **genele**yükseltilir. Bu, Azure Depolama'ya olan tüm trafiğin hizmet bitiş noktasından güvenli olduğu anlamına gelir. Hizmet bitiş noktası ilkeleri de genel olarak uygulanabilir, bu nedenle açıkça izin verilmeyen tüm depolama hesaplarıerişimengellenir. 
-   Bir alt ağa birden çok ilke uygulayabilirsiniz. Alt ağla ilişkili birden çok ilke olduğunda, bu ilkelerden herhangi birinde belirtilen kaynaklara sanal ağ trafiğine izin verilir. İlkelerden hiçbirinde belirtilmeyen diğer tüm hizmet kaynaklarına yönelik erişim reddedilir.

    > [!NOTE]  
    > Hizmet bitiş noktası **ilkeleri,** belirtilen kaynaklar dışında, diğer tüm kaynaklar sınırlıdır ilkeleri izin verir. Lütfen uygulamalarınız için tüm hizmet kaynağı bağımlılıklarının tanımlandığından ve ilkede listelendirdiğinden emin olun.

- Uç nokta ilkesinde yalnızca Azure Dağıtım Modeli'ni kullanan depolama hesapları belirtilebilir. Klasik Azure Depolama hesaplarınız Azure Hizmet Bitiş Noktası İlkeleri'ni desteklemez.
- Birincil hesap listelenirse RA-GRS ikincil erişimine otomatik olarak izin verilir.
- Depolama hesapları sanal ağ ile aynı veya farklı abonelikte ya da Azure Active Directory kiracısında yer alabilir.

## <a name="scenarios"></a>Senaryolar

- **Eşleştirilmiş, bağlı veya birden çok sanal ağ**: Eşleştirilmiş sanal ağlarda trafiği filtrelemek için, uç nokta ilkelerinin bu sanal ağlara tek tek uygulanması gerekir.
- **Internet trafiğini Ağ Cihazları veya Azure Güvenlik Duvarı ile filtreleme**: Azure hizmet trafiğini ilkelerle, hizmet bitiş noktaları üzerinden filtreleyin ve internet veya Azure trafiğinin geri kalanını cihazlar veya Azure Güvenlik Duvarı üzerinden filtreleyin.
- **Sanal Ağlara dağıtılan Azure hizmetlerindeki trafiği filtreleme**: Şu anda, Azure Hizmet Bitiş Noktası İlkeleri sanal ağınızda dağıtılan yönetilen Azure hizmetleri için desteklenmez. 
- **Şirket içinden Azure hizmetlerine yönelik trafiği filtreleme**: Hizmet uç noktası ilkeleri yalnızca ilkelerle ilişkilendirilmiş uç noktalardan gelen trafiğe uygulanır. Şirket içinden belirli Azure hizmet kaynaklarına yönelik trafiğe izin vermek için, trafiğin ağ sanal cihazları veya güvenlik duvarları kullanılarak filtrelenmesi gerekir.

## <a name="logging-and-troubleshooting"></a>Günlüğe kaydetme ve sorun giderme
Hizmet uç noktası ilkelerinde hiçbir merkezi günlük sağlanmaz. Hizmet tanılama günlükleri için bkz. [Hizmet uç noktaları günlüğü](virtual-network-service-endpoints-overview.md#logging-and-troubleshooting).

### <a name="troubleshooting-scenarios"></a>Sorun giderme senaryoları
- Önizlemede çalışan depolama hesaplarına erişim reddedildi (coğrafi eşleştirilmiş bölgede değil)
  - Azure Depolama'nın Genel Hizmet Etiketleri'ni kullanacak şekilde yükseltmesiyle, Hizmet Bitiş Noktası'nın kapsamı ve böylece Hizmet Bitiş Noktası ilkeleri artık Geneldir. Bu nedenle Azure Depolama'ya olan herhangi bir trafik Hizmet Bitiş Noktaları üzerinden şifrelenir ve yalnızca ilkede açıkça listelenen Depolama hesaplarına erişim izni verilir.
  - Erişimi geri yüklemek için gerekli tüm Depolama hesaplarını açıkça listelemesine izin verin.  
  - Azure desteğine başvurun.
- Uç nokta ilkelerinde listelenen hesaplar için erişim reddedildi
  - Ağ güvenlik grupları veya güvenlik duvarı filtrelemesi erişimi engelliyor olabilir
  - İlkenin kaldırılması/yeniden uygulanması bağlantı kaybına yol açıyorsa:
    - Azure hizmetinin uç noktalar üzerinden sanal ağdan erişime izin verecek şekilde yapılandırılıp yapılandırılmadığını veya kaynağın varsayılan ilkesinin *Tümüne İzin Ver*olarak ayarlanıp ayarlanmadığını doğrulayın.
    - Hizmet tanılamalarında uç noktalar üzerinden trafiğin gösterildiğini doğrulayın.
    - Ağ güvenlik grubu akış günlüklerinde erişimin gösterilip gösterilmediğini ve depolama günlüklerinde beklendiği gibi hizmet uç noktaları üzerinden erişimin gösterilip gösterilmediğini denetleyin.
    - Azure desteğine başvurun.
- Hizmet uç noktası ilkelerinde listelenmeyen hesaplar için erişim reddedildi
  - Azure Depolama'nın son noktalar üzerinden sanal ağdan erişime izin verecek şekilde yapılandırılıp yapılandırılmadığını veya kaynağın varsayılan ilkesinin *Tümüne İzin Ver*olarak ayarlanıp ayarlanmadığını doğrulayın.
  - Hesapların alt ağda hizmet bitiş noktası ilkeleri içeren **klasik depolama hesapları** olmadığından emin olun.
- Yönetilen bir Azure Hizmeti, alt ağ üzerinden Bir Hizmet Bitiş Noktası İlkesi uyguladıktan sonra çalışmayı durdurdu
  - Yönetilen hizmetler şu anda hizmet bitiş noktası ilkeleriyle desteklenmez. *Güncelleştirmeler için bu alanı izleyin.*

## <a name="provisioning"></a>Sağlama

Hizmet uç noktası ilkeleri, sanal ağda yazma erişimine sahip bir kullanıcı tarafından alt ağlarda yapılandırılabilir. Azure [yerleşik rolleri](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [özel rollere](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) belirli izinlerin atanması hakkında daha fazla bilgi edinin.

Sanal ağlar ve Azure Depolama hesapları aynı veya farklı aboneliklerde veya Azure Etkin Dizin kiracılarında olabilir.

## <a name="limitations"></a>Sınırlamalar

- Hizmet uç noktası ilkelerini yalnızca Azure Resource Manager dağıtım modeli üzerinden dağıtılmış olan sanal ağlarda dağıtabilirsiniz.
- Sanal ağların hizmet uç noktası ilkesiyle aynı bölgede bulunması gerekir.
- Hizmet uç noktası ilkesini bir alt ağa uygulayabilmeniz için, ilkede listelenen Azure hizmetleri için hizmet uç noktalarının yapılandırılmış olması gerekir.
- Şirket içi ağınızdan Azure hizmetlerine yönelik trafik için hizmet uç noktası ilkelerini kullanamazsınız.
- Azure yönetilen hizmetler şu anda Endpoint ilkelerini desteklemez. Buna paylaşılan alt ağlara (örneğin *Azure HDInsight, Azure Toplu İş, Azure EKLER, Azure APplication Ağ Geçidi, Azure VPN ağ geçidi, Azure Güvenlik Duvarı)* veya özel alt ağlara (örneğin *Azure Uygulama Hizmet Ortamı, Azure Redis Önbelleği, Azure API Yönetimi, Azure SQL MI, klasik yönetilen hizmetler)* dağıtılan yönetilen hizmetler dahildir.

 > [!WARNING]
 > Sanal ağınıza dağıtılan Azure HDInsight gibi Azure hizmetleri, altyapı gereksinimlerinden dolayı Azure Depolama gibi diğer Azure hizmetlerine erişir. Uç nokta ilkesini belirli kaynaklarla kısıtlamak, sanal ağınızda dağıtılan Azure hizmetleri için bu altyapı kaynaklarına erişimi kesebilir.

- Klasik depolama hesapları uç nokta ilkelerinde desteklenmez. İlkeler varsayılan olarak tüm klasik depolama hesaplarına erişimi reddedecektir. Uygulamanızın Azure Resource Manager'a ve klasik depolama hesaplarına erişmesi gerekiyorsa, bu trafik için uç nokta ilkeleri kullanılmamalıdır.

## <a name="pricing-and-limits"></a>Fiyatlandırma ve limitler

Hizmet uç noktası ilkelerinin kullanımından ek ücret alınmaz. Hizmet uç noktaları üzerinden Azure hizmetleri (Azure Depolama gibi) için güncel fiyatlandırma modeli uygulanır.

Hizmet uç noktası ilkelerinde aşağıdaki limitler zorunlu tutulur: 

 |Kaynak | Varsayılan limit |
 |---------|---------------|
 |ServiceEndpointPoliciesPerSubscription |500 |
 |ServiceEndpintPoliciesPerSubnet|100 |
 |ServiceResourcesPerServiceEndpointPolicyDefinition|200 |

## <a name="next-steps"></a>Sonraki Adımlar

- [Sanal ağ hizmet uç noktası ilkelerini nasıl yapılandıracağınızı](virtual-network-service-endpoint-policies-portal.md) öğrenin
- [Sanal Ağ Hizmet Uç Noktaları](virtual-network-service-endpoints-overview.md) hakkında daha fazla bilgi edinin
