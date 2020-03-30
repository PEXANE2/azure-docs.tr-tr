---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 02/24/2020
ms.topic: include
ms.openlocfilehash: c77849b2285283a34e6adf84dc3845a4076407af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597958"
---
## <a name="attack-scenario"></a>Saldırı senaryosu

Kaba kuvvet saldırıları genellikle bir VM'ye erişmek için yönetim bağlantı noktalarını hedef alır. Başarılı olursa, saldırgan VM'nin kontrolünü ele alabilir ve ortamınıza bir dayanak noktası oluşturabilirsiniz.

Kaba kuvvet saldırısına maruz kalma azaltmanın bir yolu, bir bağlantı noktasının açık olduğu süreyi sınırlamaktır. Yönetim bağlantı noktalarının her zaman açık olması gerekmez. Örneğin, yönetim veya bakım görevlerini gerçekleştirmek için VM'ye bağlıyken açık olmaları gerekir. Tam zamanında etkinleştirildiğinde, Güvenlik Merkezi [ağ güvenlik grubu](../articles/virtual-network/security-overview.md#security-rules) (NSG) ve Azure Güvenlik Duvarı kurallarını kullanır ve bu kurallar, saldırganlar tarafından hedef alınamaması için yönetim bağlantı noktalarına erişimi kısıtlar.

![Tam zamanında senaryo](../articles/security-center/media/security-center-just-in-time/just-in-time-scenario.png)

## <a name="how-does-jit-access-work"></a>JIT erişimi nasıl çalışır?

Tam zamanında etkinleştirildiğinde, Güvenlik Merkezi bir NSG kuralı oluşturarak Azure VM'lerinize gelen trafiği kilitler. Gelen trafiğin kilitlendiği VM'deki bağlantı noktalarını seçersiniz. Bu bağlantı noktaları tam zamanında çözüm tarafından kontrol edilir.

Bir kullanıcı bir VM'ye erişim istediğinde, Güvenlik Merkezi kullanıcının bu VM için [Rol Tabanlı Erişim Denetimi (RBAC)](../articles/role-based-access-control/role-assignments-portal.md) izinlerine sahip olduğunu denetler. İstek onaylanırsa, Güvenlik Merkezi, seçilen bağlantı noktalarına ve istenen kaynak IP adreslerine veya aralıklarına gelen trafiğin belirtilmeye kadar girmesine izin verecek şekilde Ağ Güvenlik Grupları (NSG'ler) ve Azure Güvenlik Duvarı'nı otomatik olarak yapılandırır. Süre dolduktan sonra, Güvenlik Merkezi NSG'leri önceki eyaletlerine geri yükler. Ancak, zaten kurulmuş olan bu bağlantılar kesilmiyor.

 > [!NOTE]
 > Bir Azure Güvenlik Duvarı'nın arkasındaki Bir VM için JIT erişim isteği onaylanırsa, Güvenlik Merkezi hem NSG hem de güvenlik duvarı ilkesi kurallarını otomatik olarak değiştirir. Belirtilen süre boyunca, kurallar seçili bağlantı noktalarına ve istenen kaynak IP adreslerine veya aralıklarına gelen trafiğe izin verir. Süre dolduktan sonra, Güvenlik Merkezi güvenlik duvarını ve NSG kurallarını önceki eyaletlerine geri yükler.


## <a name="permissions-needed-to-configure-and-use-jit"></a>JIT'yi yapılandırmak ve kullanmak için gereken izinler

| Bir kullanıcının şunları yapabilmesini sağlamak için: | Ayarlanan izinler|
| --- | --- |
| VM için JIT ilkesini yapılandırma veya bu politikayı yeniden yapılandırma | *Bu eylemleri role atama:*  <ul><li>VM ile ilişkili bir abonelik veya kaynak grubunun kapsamında:<br/> `Microsoft.Security/locations/jitNetworkAccessPolicies/write` </li><li> VM'nin abonelik veya kaynak grubu kapsamında: <br/>`Microsoft.Compute/virtualMachines/write`</li></ul> | 
|VM'ye JIT erişimi isteği | *Bu eylemleri kullanıcıya atama:*  <ul><li>VM ile ilişkili bir abonelik veya kaynak grubunun kapsamında:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action` </li><li>VM ile ilişkili bir abonelik veya kaynak grubunun kapsamında:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/*/read` </li><li>  Bir abonelik veya kaynak grubu veya VM kapsamında:<br/> `Microsoft.Compute/virtualMachines/read` </li><li>  Bir abonelik veya kaynak grubu veya VM kapsamında:<br/> `Microsoft.Network/networkInterfaces/*/read` </li></ul>|