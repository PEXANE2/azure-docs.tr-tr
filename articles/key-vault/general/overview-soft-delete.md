---
title: Azure Key Vault geçici silme | Microsoft Docs
description: Azure Key Vault 'da geçici silme, anahtarlar, gizlilikler ve sertifikalar gibi Silinen anahtar kasalarını ve Anahtar Kasası nesnelerini kurtarmanıza olanak sağlar.
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 03/19/2019
ms.openlocfilehash: 347f8a4cf1fb95849bcf1008e91d17878f3d01f8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82598530"
---
# <a name="azure-key-vault-soft-delete-overview"></a>Azure Key Vault geçici silmeye genel bakış

Key Vault geçici silme özelliği, geçici silme olarak bilinen silinmiş kasaların ve kasa nesnelerinin kurtarılmasına olanak tanır. Özellikle, aşağıdaki senaryolara adresliyoruz:

- Bir anahtar kasasının kurtarılabilir silme desteği
- Anahtar Kasası nesnelerinin kurtarılabilir silme desteği (örn. Anahtarlar, gizlilikler, sertifikalar)

## <a name="supporting-interfaces"></a>Destekleyici arabirimler

Geçici silme özelliği, ilk olarak [rest](/rest/api/keyvault/), [CLI](soft-delete-cli.md), [PowerShell](soft-delete-powershell.md)ve [.NET/C#](/dotnet/api/microsoft.azure.keyvault?view=azure-dotnet) arabirimleri ve [ARM şablonları](https://docs.microsoft.com/azure/templates/microsoft.keyvault/2019-09-01/vaults)aracılığıyla kullanılabilir.

## <a name="scenarios"></a>Senaryolar

Azure Anahtar kasaları, Azure Resource Manager tarafından yönetilen, izlenen kaynaklardır. Azure Resource Manager, silme işlemi için iyi tanımlanmış bir davranış belirtir. Bu, başarılı bir SILME işleminin, o kaynağa artık erişilemeyecek şekilde neden olması gerekir. Geçici silme özelliği, silmenin yanlışlıkla mi yoksa bilerek mi olduğunu, Silinen nesnenin kurtarılmasına yöneliktir.

1. Tipik senaryoda, bir kullanıcı yanlışlıkla bir anahtar kasasını veya bir Anahtar Kasası nesnesini silmiş olabilir; Bu Anahtar Kasası veya Anahtar Kasası nesnesi önceden belirlenmiş bir süre için kurtarılabilir ise, Kullanıcı silme işlemini geri alabilir ve verilerini kurtarabilir.

2. Farklı bir senaryoda, standart dışı bir Kullanıcı bir anahtar kasasını veya kasadaki bir anahtar gibi bir Anahtar Kasası nesnesini bir iş kesintisi olacak şekilde silmeye çalışabilir. Temel verilerin gerçek silinmesinden, anahtar kasasının veya Anahtar Kasası nesnesinin silinmesini bir şekilde ayırmak, örneğin, veri silme izinlerini farklı, güvenilen bir rolde kısıtlamak için tarafından bir güvenlik önlemi olarak kullanılabilir. Bu yaklaşım, bir işlem için çekirdek gerektirir, aksi takdirde veri kaybına neden olabilir.

### <a name="soft-delete-behavior"></a>Geçici silme davranışı

Geçici silme etkin olduğunda, silinen kaynaklar olarak işaretlenen kaynaklar belirli bir süre boyunca (varsayılan olarak 90 gün) tutulur. Bu hizmet daha sonra silme işlemini geri almak için silinen nesneyi kurtarmaya yönelik bir mekanizma sağlar.

Yeni bir Anahtar Kasası oluştururken geçici silme işlemi varsayılan olarak açık olur. [Azure CLI](soft-delete-cli.md) veya [Azure PowerShell](soft-delete-powershell.md)aracılığıyla geçici silme olmadan bir Anahtar Kasası oluşturabilirsiniz. Bir anahtar kasasında geçici silme etkinleştirildikten sonra devre dışı bırakılamaz

Varsayılan saklama süresi 90 gündür ancak Anahtar Kasası oluşturma sırasında, bekletme ilkesi aralığını, Azure portal ile 90 gün arasında bir değere ayarlamak mümkündür. Temizleme koruması bekletme ilkesi aynı aralığı kullanır. Bir kez ayarlandıktan sonra bekletme ilkesi aralığı değiştirilemez.

Saklama süresi geçene kadar geçici olarak silinen bir anahtar kasasının adını yeniden kullanamazsınız.

### <a name="purge-protection"></a>Korumayı temizle 

Temizleme koruması isteğe bağlı bir Key Vault davranıştır ve **Varsayılan olarak etkin değildir**. Temizleme koruması, yalnızca geçici silme etkin olduğunda etkinleştirilebilir.  [CLI](soft-delete-cli.md#enabling-purge-protection) veya [PowerShell](soft-delete-powershell.md#enabling-purge-protection)aracılığıyla etkinleştirilebilir.

Temizleme koruması açık olduğunda, saklama süresi geçene kadar bir kasa veya silinen durumdaki bir nesne temizlenemiyor. Geçici olarak silinen kasalar ve nesneler kurtarılabilir ve bu da bekletme ilkesinin izlenmeyeceğinden emin olur. 

Varsayılan saklama süresi 90 gündür, ancak bekletme ilkesi aralığını Azure portal ile 90 7 güne kadar bir değere ayarlamak mümkündür. Bekletme ilkesi aralığı ayarlandıktan ve kaydedildikten sonra bu kasa için değiştirilemez. 

### <a name="permitted-purge"></a>İzin verilen temizleme

Bir anahtar kasasını kalıcı olarak silme, Temizleme, proxy kaynağında bir POST işlemi yoluyla yapılabilir ve özel ayrıcalıklar gerektirir. Genellikle, bir anahtar kasasını yalnızca abonelik sahibi temizlenebilir. POST işlemi, bu kasanın anlık ve kurtarılabilir silme işlemlerini tetikler. 

Özel durumlar şunlardır:
- Azure aboneliği *silinebilir*olarak işaretlendiyse. Bu durumda, yalnızca hizmet gerçek silme işlemini gerçekleştirebilir ve bu işlemi zamanlanmış bir işlem olarak yapar. 
- `--enable-purge-protection flag`Kasa üzerinde etkinleştirildiğinde. Bu durumda, Key Vault, özgün gizli nesne, nesneyi kalıcı olarak silmek için silinmek üzere işaretlendiyse, 90 gün boyunca bekleyecektir.

### <a name="key-vault-recovery"></a>Anahtar Kasası kurtarma

Bir anahtar kasasını sildikten sonra, hizmet aboneliğin altında, kurtarma için yeterli meta veri ekleyerek bir proxy kaynağı oluşturur. Proxy kaynağı, silinen Anahtar Kasası ile aynı konumda bulunan saklı bir nesnedir. 

### <a name="key-vault-object-recovery"></a>Anahtar Kasası nesne kurtarma

Anahtar gibi bir Anahtar Kasası nesnesini sildikten sonra, hizmet nesneyi silinen bir duruma yerleştirir ve bu işlem herhangi bir alma işlemi için erişilemez hale gelir. Bu durumda, Anahtar Kasası nesnesi yalnızca listelenmiş, kurtarılabilir veya zorla/kalıcı olarak silinebilir. 

Aynı zamanda Key Vault, önceden belirlenmiş bir saklama aralığından sonra yürütülmek üzere silinen Anahtar Kasası veya Anahtar Kasası nesnesine karşılık gelen temel verilerin silinmesini zamanlayamaz. Kasaya karşılık gelen DNS kaydı, bekletme aralığı süresince de korunur.

### <a name="soft-delete-retention-period"></a>Geçici silme bekletme süresi

Geçici olarak silinen kaynaklar ayarlanan süre, 90 gün boyunca tutulur. Geçici silme bekletme aralığı sırasında aşağıdakiler geçerlidir:

- Tüm anahtar kasalarını ve Anahtar Kasası nesnelerini, aboneliğiniz için geçici silme durumunda listeleyebilir ve bunlarla ilgili silme ve kurtarma bilgilerine erişim de sağlayabilirsiniz.
    - Yalnızca özel izinlere sahip kullanıcılar silinmiş kasaları listeleyebilir. Kullanıcılarınızın silinen kasaların işlenmesi için bu özel izinlerle özel bir rol oluşturmasını öneririz.
- Aynı konumda aynı ada sahip bir Anahtar Kasası oluşturulamıyor; Bu Anahtar Kasası, aynı ada sahip ve silinmiş durumda olan bir nesne içeriyorsa, belirli bir kasada Anahtar Kasası nesnesi oluşturulamıyor 
- Yalnızca özel ayrıcalıklı bir Kullanıcı, ilgili proxy kaynağında bir kurtarma komutu vererek bir Anahtar Kasası veya Anahtar Kasası nesnesini geri yükleyebilir.
    - Kaynak grubu altında bir Anahtar Kasası oluşturma ayrıcalığına sahip olan Kullanıcı, özel rolün üyesi kasayı geri yükleyebilir.
- Bir anahtar kasasını veya Anahtar Kasası nesnesini, ilgili proxy kaynağında silme komutu vererek zorla silebilir.

Bir Anahtar Kasası veya Anahtar Kasası nesnesi kurtarılamazsa, bekletme aralığının sonunda hizmet, geçici olarak silinen anahtar kasasını veya Anahtar Kasası nesnesini ve içeriğini temizleme işlemini gerçekleştirir. Kaynak silme işlemi yeniden planlanmayabilir.

### <a name="billing-implications"></a>Faturalandırma etkileri

Genel olarak, bir nesne (bir Anahtar Kasası veya bir anahtar veya gizli dizi) silinmiş durumdaysa yalnızca iki işlem olabilir: ' Temizle ' ve ' Recover '. Diğer tüm işlemler başarısız olur. Bu nedenle, nesne mevcut olsa bile hiçbir işlem gerçekleştirilemediği için hiçbir kullanım gerçekleşmeyecektir. Ancak şu özel durumlar mevcuttur:

- ' Temizleme ' ve ' Kurtarma ' eylemleri, normal Anahtar Kasası işlemlerine göre sayılır ve faturalandırılır.
- Nesne bir HSM anahtaralıyorsa, son 30 gün içinde bir anahtar sürümü kullanılıyorsa, aylık ücret başına ' HSM korumalı anahtar ' ücreti uygulanır. Bundan sonra, nesne silinmiş durumda olduğundan, buna karşı hiçbir işlem gerçekleştirilemediği için hiçbir ücret uygulanmaz.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki iki kılavuz, geçici silme kullanmaya yönelik birincil kullanım senaryolarını sunmaktadır.

- [Key Vault geçici silmeyi PowerShell ile kullanma](soft-delete-powershell.md) 
- [Key Vault geçici silmeyi CLI ile kullanma](soft-delete-cli.md)

