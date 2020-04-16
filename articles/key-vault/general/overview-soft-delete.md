---
title: Azure Key Vault yumuşak silme | Microsoft Dokümanlar
description: Azure Key Vault'ta yumuşak silme, silinmiş anahtar kasalarını ve anahtarlar, sırlar ve sertifikalar gibi anahtar kasa nesnelerini kurtarmanızı sağlar.
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 03/19/2019
ms.openlocfilehash: 6185f0d84f27b6be89e797fc7cfb22940d8c6401
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81432105"
---
# <a name="azure-key-vault-soft-delete-overview"></a>Azure Key Vault geçici silmeye genel bakış

Key Vault'un yumuşak silme özelliği, silinen kasaların ve kasa nesnelerinin kurtarılmasına olanak tanır, buna yumuşak silme olarak bilinir. Özellikle, aşağıdaki senaryoları ele alıyoruz:

- Anahtar kasasının geri kazanılabilir silinmesi için destek
- Anahtar kasa nesnelerinin geri kazanılabilir silinmesi için destek (ör. anahtarlar, sırlar, sertifikalar)

## <a name="supporting-interfaces"></a>Destekleyici arabirimler

Yumuşak silme özelliği başlangıçta [REST](/rest/api/keyvault/), [CLI](soft-delete-cli.md), [PowerShell](soft-delete-powershell.md) ve [.NET/C#](/dotnet/api/microsoft.azure.keyvault?view=azure-dotnet) arayüzleri aracılığıyla kullanılabilir.

## <a name="scenarios"></a>Senaryolar

Azure Anahtar Kasaları, Azure Kaynak Yöneticisi tarafından yönetilen izlenen kaynaklardır. Azure Kaynak Yöneticisi ayrıca, başarılı bir DELETE işleminin bu kaynağa artık erişilememesine neden olması gerektiğini gerektiren iyi tanımlanmış bir silme davranışı da belirtir. Yumuşak silme özelliği, silinen nesnenin silinmesi yanlışlıkla veya kasıtlı olsun, kurtarma adresi.

1. Tipik senaryoda, bir kullanıcı yanlışlıkla bir anahtar kasasını veya anahtar kasa nesnesini silmiş olabilir; bu anahtar kasası veya anahtar kasa nesnesi önceden belirlenmiş bir süre için kurtarılabilir ise, kullanıcı silme geri alabilir ve verilerini kurtarmak.

2. Farklı bir senaryoda, haydut bir kullanıcı bir iş kesintisine neden olmak için bir anahtar kasasını veya kasa içindeki anahtar gibi önemli bir kasa nesnesini silmeyi deneyebilir. Anahtar kasasının veya anahtar kasa nesnesinin silinmesini, temel verilerin gerçek silinmesinden ayırmak, örneğin, veri silme izinlerini farklı, güvenilir bir rol ile sınırlandırarak bir güvenlik önlemi olarak kullanılabilir. Bu yaklaşım, aksi takdirde hemen bir veri kaybına neden olabilecek bir işlem için etkin bir çoğunluk gerektirir.

### <a name="soft-delete-behavior"></a>Yumuşak silme davranışı

Yumuşak silme etkinleştirildiğinde, silinmiş kaynaklar olarak işaretlenmiş kaynaklar belirli bir süre (varsayılan olarak 90 gün) tutulur. Hizmet ayrıca silinen nesneyi kurtarmak için bir mekanizma sağlar ve aslında silme işlemini geri alar.

Yeni bir anahtar kasası oluştururken, varsayılan olarak yumuşak silme açıktır. [Azure CLI](soft-delete-cli.md) veya [Azure Powershell](soft-delete-powershell.md)üzerinden yumuşak silme olmadan bir anahtar kasası oluşturabilirsiniz. Anahtar kasasında yumuşak silme etkinleştirildiğinde devre dışı tutulamaz

Varsayılan bekletme süresi 90 gündür, ancak anahtar kasa oluşturma sırasında, bekletme ilkesi aralığını Azure portalı üzerinden 7 ila 90 gün arasında bir değere ayarlamak mümkündür. Temizleme koruma tutma ilkesi aynı aralığı kullanır. Ayarlandıktan sonra, bekletme ilkesi aralığı değiştirilemez.

Bekletme süresi geçene kadar yumuşak silinmiş anahtar kasasının adını yeniden kullanamazsınız.

### <a name="purge-protection"></a>Temizleme koruması 

Temizleme koruması isteğe bağlı bir Anahtar Kasa sıdavranışıdır ve **varsayılan olarak etkinleştirilir.** [CLI](soft-delete-cli.md#enabling-purge-protection) veya [Powershell](soft-delete-powershell.md#enabling-purge-protection)ile açılabilir.

Temizleme koruması açıkken, silinen durumdaki bir kasa veya nesne bekletme süresi geçene kadar temizlenemez. Yumuşak silinmiş kasalar ve nesneler, bekletme ilkesinin izlenmesini sağlayarak kurtarılabilir. 

Varsayılan bekletme süresi 90 gündür, ancak bekletme ilkesi aralığını Azure portalı üzerinden 7 ile 90 gün arasında bir değere ayarlamak mümkündür. Bekletme ilkesi aralığı ayarlanıp kaydedildikten sonra bu kasa için değiştirilemez. 

### <a name="permitted-purge"></a>İzin verilen tasfiye

Kalıcı olarak silme, temizleme, anahtar kasa proxy kaynak üzerinde bir POST işlemi ile mümkündür ve özel ayrıcalıklar gerektirir. Genellikle, yalnızca abonelik sahibi anahtar kasasını temizlemek mümkün olacaktır. POST operasyonu kasanın hemen ve geri alınamaz şekilde silinmesini tetikler. 

İstisnalar şunlardır:
- Azure aboneliği *kullanılamaz*olarak işaretlendiğinde. Bu durumda, yalnızca hizmet daha sonra fiili silme gerçekleştirebilir ve bunu zamanlanmış bir işlem olarak yapar. 
- Tonozda --etkinleştir-temizleme koruma bayrağı etkinleştirildiğinde. Bu durumda, Key Vault, özgün gizli nesnenin nesneyi kalıcı olarak silmek için silinmesi için işaretlendiğinden itibaren 90 gün bekler.

### <a name="key-vault-recovery"></a>Anahtar kasa kurtarma

Anahtar kasası silen hizmet, abonelik altında bir proxy kaynağı oluşturarak kurtarma için yeterli meta veri ekler. Proxy kaynağı, silinen anahtar kasası ile aynı konumda bulunan depolanmış bir nesnedir. 

### <a name="key-vault-object-recovery"></a>Anahtar kasa nesne kurtarma

Anahtar gibi önemli bir kasa nesnesi silindikten sonra, hizmet nesneyi silinmiş bir duruma yerleştirerek herhangi bir alma işlemi için erişilemez hale getirir. Bu durumdayken, anahtar tonoz nesnesi yalnızca listelenebilir, kurtarılabilir veya zorla/kalıcı olarak silinebilir. 

Aynı zamanda, Key Vault, önceden belirlenmiş bir bekletme aralığından sonra yürütülmesi için silinmiş anahtar kasasına veya anahtar kasanesnesine karşılık gelen temel verilerin silinmesini zamanlar. Kasaya karşılık gelen DNS kaydı da bekletme aralığı süresince saklanır.

### <a name="soft-delete-retention-period"></a>Yumuşak silme bekletme süresi

Yumuşak silinen kaynaklar 90 gün, belirli bir süre için tutulur. Yumuşak silme bekletme aralığı sırasında aşağıdaki ler uygulanır:

- Aboneliğiniz için yumuşak silme durumundaki tüm anahtar kasaları ve anahtar kasa nesnelerini listeleyebilir ve bunlarla ilgili silme ve kurtarma bilgilerine erişebilirsiniz.
    - Yalnızca özel izinlere sahip kullanıcılar silinen kasaları listeleyebilir. Kullanıcılarımızın silinen kasaları işlemek için bu özel izinlerle özel bir rol oluşturmalarını öneririz.
- Aynı ada sahip anahtar kasası aynı konumda oluşturulamaz; buna karşılık, bu anahtar tonozaynı ada sahip bir nesne içeriyorsa ve silinmiş durumdaysa, belirli bir kasada anahtar tonoz nesnesi oluşturulamaz 
- Yalnızca özel olarak ayrıcalıklı bir kullanıcı, ilgili proxy kaynağında kurtarma komutu vererek anahtar kasasını veya anahtar kasa nesnesini geri yükleyebilir.
    - Kaynak grubunun altında anahtar kasa oluşturma ayrıcalığına sahip olan kullanıcı, özel rolün üyesi olan kasayı geri yükleyebilir.
- Yalnızca özel olarak ayrıcalıklı bir kullanıcı, ilgili proxy kaynağında silme komutu vererek bir anahtar kasasını veya anahtar kasa nesnesini zorla silebilir.

Anahtar kasa veya anahtar tonoz nesnesi kurtarılmadığı sürece, bekletme aralığının sonunda hizmet yumuşak silinmiş anahtar tonoz veya anahtar kasa nesnesi ve içeriğinin tasfiyesini gerçekleştirir. Kaynak silme yeniden zamanlanmamış olabilir.

### <a name="billing-implications"></a>Faturalandırma sonuçları

Genel olarak, bir nesne (anahtar kasası veya anahtar veya gizli) silinmiş durumda olduğunda, yalnızca iki işlem mümkündür: 'temizleme' ve 'kurtarma'. Diğer tüm işlemler başarısız olur. Bu nedenle, nesne olsa bile, hiçbir işlem gerçekleştirilebilir ve bu nedenle hiçbir kullanım oluşacak, bu nedenle hiçbir fatura. Ancak aşağıdaki istisnalar vardır:

- 'tasfiye' ve 'kurtarma' eylemleri normal anahtar kasa işlemleri ne kadar sayılır ve faturalandırılır.
- Nesne bir HSM tuşuysa, son 30 gün içinde anahtar sürümü kullanılmışsa, aylık anahtar sürümü başına 'HSM Protected tuşu' ücreti uygulanır. Bundan sonra, nesne silinmiş durumda olduğundan, nesne aleyhine hiçbir işlem yapılamaz, bu nedenle hiçbir ücret uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki iki kılavuz, yumuşak silme kullanmak için birincil kullanım senaryolarını sunar.

- [Key Vault geçici silmeyi PowerShell ile kullanma](soft-delete-powershell.md) 
- [Key Vault geçici silmeyi CLI ile kullanma](soft-delete-cli.md)

