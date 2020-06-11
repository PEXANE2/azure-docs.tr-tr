---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 02/24/2020
ms.topic: include
ms.openlocfilehash: 5efa8b845952c8ab5b66b781fb534c3754239b4e
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84677448"
---
## <a name="attack-scenario"></a>Saldırı senaryosu

Deneme yanılma saldırıları, bir VM 'ye erişim kazanmak için genellikle hedef yönetim bağlantı noktalarına bir yol olarak. Başarılı olursa, bir saldırgan VM üzerinde denetim alabilir ve ortamınıza bir altbilgi kurabilir.

Bir deneme yanılma saldırısı olasılığını azaltmanın bir yolu, bir bağlantı noktasının açık olduğu süreyi sınırlandırmaktır. Yönetim bağlantı noktalarının her zaman açık olması gerekmez. Yalnızca VM 'ye bağlıyken açık olmaları gerekir, örneğin, yönetim veya bakım görevlerini gerçekleştirmek için. Tam zamanında etkinleştirildiğinde Güvenlik Merkezi, [ağ güvenlik grubu](../articles/virtual-network/security-overview.md#security-rules) (NSG) ve Azure Güvenlik duvarı kurallarını kullanarak, saldırganlar tarafından hedeflenememesi için yönetim bağlantı noktalarına erişimi kısıtlar.

## <a name="how-does-jit-access-work"></a>JıT erişimi nasıl çalışır?

Tam zamanında etkinleştirildiğinde, güvenlik merkezi bir NSG kuralı oluşturarak Azure VM 'lerinize gelen trafiği kilitler. Gelen trafiğin kilitlenebileceği sanal makine üzerindeki bağlantı noktalarını seçersiniz. Bu bağlantı noktaları, tam zamanında çözüm tarafından denetlenir.

Bir Kullanıcı bir VM 'ye erişim istediğinde, güvenlik merkezi kullanıcının bu VM için [rol tabanlı Access Control (RBAC)](../articles/role-based-access-control/role-assignments-portal.md) izinlerine sahip olduğunu denetler. İstek onaylanırsa, Güvenlik Merkezi, belirtilen süre boyunca seçilen bağlantı noktalarına ve istenen kaynak IP adreslerine veya aralıklarına gelen trafiğe izin vermek için ağ güvenlik gruplarını (NSG 'ler) ve Azure Güvenlik duvarını otomatik olarak yapılandırır. Sürenin süresi dolduktan sonra, güvenlik merkezi NSG 'leri önceki durumlarına geri yükler. Ancak önceden oluşturulan bağlantılar kesilmez.

 > [!NOTE]
 > Azure Güvenlik duvarının arkasındaki bir sanal makine için bir JıT erişim isteği onaylanmışsa, güvenlik merkezi hem NSG hem de güvenlik duvarı ilkesi kurallarını otomatik olarak değiştirir. Belirtilen süre boyunca, kurallar seçili bağlantı noktalarına gelen trafiğe ve istenen kaynak IP adreslerine veya aralıklarına izin verir. Süre bittikten sonra, Güvenlik Merkezi güvenlik duvarı ve NSG kurallarını önceki durumlarına geri yükler.


## <a name="roles-that-can-read-jit-policies"></a>JıT ilkelerini okuyabilen roller

**Reader** ve **securityreader** rolleri, her iki ilkeyi de okuyabilir.

## <a name="permissions-needed-to-configure-and-use-jit"></a>JıT 'i yapılandırmak ve kullanmak için gerekli izinler

JıT ile çalışabilebilmeniz gereken özel Roller oluşturmak istiyorsanız aşağıdaki ayrıntılara ihtiyacınız vardır:

| Bir kullanıcıyı şu şekilde etkinleştirmek için: | Ayarlanacak izinler|
| --- | --- |
| VM için bir JıT ilkesi yapılandırma veya düzenleme | *Bu eylemleri role atayın:*  <ul><li>VM ile ilişkili bir abonelik veya kaynak grubu kapsamında:<br/> `Microsoft.Security/locations/jitNetworkAccessPolicies/write` </li><li> Bir abonelik veya VM kaynak grubu kapsamında: <br/>`Microsoft.Compute/virtualMachines/write`</li></ul> | 
|Bir VM 'ye JıT erişimi isteme | *Bu eylemleri kullanıcıya ata:*  <ul><li>VM ile ilişkili bir abonelik veya kaynak grubu kapsamında:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action` </li><li>VM ile ilişkili bir abonelik veya kaynak grubu kapsamında:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/*/read` </li><li>  Bir aboneliğin veya kaynak grubunun veya VM 'nin kapsamında:<br/> `Microsoft.Compute/virtualMachines/read` </li><li>  Bir aboneliğin veya kaynak grubunun veya VM 'nin kapsamında:<br/> `Microsoft.Network/networkInterfaces/*/read` </li></ul>|
|JıT ilkelerini okuma| *Bu eylemleri kullanıcıya ata:*  <ul><li>`Microsoft.Security/locations/jitNetworkAccessPolicies/read`</li><li>`Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action`</li><li>`Microsoft.Security/policies/read`</li><li>`Microsoft.Compute/virtualMachines/read`</li><li>`Microsoft.Network/*/read`</li>|