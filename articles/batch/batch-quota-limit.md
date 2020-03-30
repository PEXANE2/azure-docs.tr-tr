---
title: Hizmet kotaları ve limitleri - Azure Toplu İş | Microsoft Dokümanlar
description: Varsayılan Azure Toplu Iş Kotaları, sınırları ve kısıtlamaları ve kota artışlarını nasıl isteyebilirsiniz hakkında bilgi edinin
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/13/2019
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: c8a78acfa1f3e7332cee337ae1a82ee365fe356a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248235"
---
# <a name="batch-service-quotas-and-limits"></a>Batch hizmet kotaları ve limitleri

Diğer Azure hizmetlerinde olduğu gibi, Toplu İşlem hizmetiyle ilişkili belirli kaynaklarda da sınırlamalar vardır. Bu sınırların çoğu, Azure tarafından abonelik veya hesap düzeyinde uygulanan varsayılan kotalardır. Bu makalede, bu varsayılanlar ve kota artışlarını nasıl isteyebileceğiniz açıklanmıştır.

Toplu iş yüklerinizi tasarlayıp büyüterken bu kotaları aklınızda bulundurun. Örneğin, havuzunuz belirttiğiniz işlem düğümlerinin hedef sayısına ulaşamazsa, Toplu Iş hesabınız için temel kota sınırına ulaşmış olabilirsiniz.

Tek bir Batch hesabında birden fazla Batch iş yükü çalıştırabilir ya da iş yüklerinizi aynı abonelik ve farklı Azure bölgelerindeki Batch hesapları arasında dağıtabilirsiniz.

Toplu Iş Yerinde üretim iş yüklerini çalıştırmayı planlıyorsanız, kotalardan birini veya birkaçını varsayılanın üzerinde artırmanız gerekebilir. Kota yükseltmek istiyorsanız, ücretsiz olarak çevrimiçi [müşteri destek isteği](#increase-a-quota) açabilirsiniz.

## <a name="resource-quotas"></a>Kaynak kotaları

Kota kredi limitidir, kapasite garantisi değil. Büyük ölçekli kapasite gereksinimleriniz varsa, lütfen Azure desteğine başvurun.

Ayrıca kotaların garantili değerler olmadığını da unutmayın. Kotalar, Toplu İşlem hizmetindeki değişikliklere veya kota değerini değiştirmek için kullanıcı isteğine bağlı olarak değişebilir.

[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

### <a name="cores-quotas-in-user-subscription-mode"></a>Kullanıcı abonelik modunda çekirdek kotaları

**Kullanıcı aboneliğine**ayarlanmış havuz ayırma moduna sahip bir Toplu Iş hesabı oluşturduysanız, kotalar farklı şekilde uygulanır. Bu modda, toplu toplu iş vm'ler ve diğer kaynaklar, bir havuz oluşturulduğunda doğrudan aboneliğinizde oluşturulur. Azure Toplu Iş çekirdekkotaları bu modda oluşturulan bir hesaba uygulanmaz. Bunun yerine, bölgesel işlem çekirdekleri ve diğer kaynaklar için aboneliğinizdeki kotalar uygulanır. [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)da dahil olmak üzere bu kotalar hakkında daha fazla bilgi edinin.

## <a name="pool-size-limits"></a>Havuz boyutu sınırları

Havuz boyutu sınırları Toplu İşlem hizmeti tarafından belirlenir. [Kaynak kotalarının](#resource-quotas)aksine, bu değerler değiştirilemez. Yalnızca düğümler arası iletişim ve özel görüntülere sahip havuzlarda standart kotadan farklı kısıtlamalar vardır.

| **Kaynak** | **Üst Sınır** |
| --- | --- |
| **[Düğümler arası iletişim etkin havuzunda](batch-mpi.md) düğümleri hesaplama**  ||
| Toplu servis havuzu ayırma modu | 100 |
| Toplu abonelik havuzu ayırma modu | 80 |
| ** [Yönetilen bir görüntü kaynağı yla oluşturulan havuzdaki](batch-custom-images.md)hesaplama düğümleri**<sup>1</sup> ||
| Ayrılmış düğümler | 2000 |
| Düşük öncelikli düğümler | 1000 |

<sup>1</sup> Düğümler arası iletişim etkin olmayan havuzlar için.

## <a name="other-limits"></a>Diğer sınırlar

Toplu İşlem hizmeti tarafından belirlenen ek sınırlar. [Kaynak kotalarının](#resource-quotas)aksine, bu değerler değiştirilemez.

| **Kaynak** | **Üst Sınır** |
| --- | --- |
| İşlem düğümü başına [eşzamanlı görevler](batch-parallel-node-tasks.md) | Düğüm çekirdeğinin 4 x sayısı |
| Toplu Iş hesabı başına [uygulamalar](batch-application-packages.md) | 20 |
| Uygulama başına uygulama paketleri | 40 |
| Havuz başına uygulama paketleri | 10 |
| Maksimum görev ömrü | 180 gün<sup>1</sup> |
| İşlem düğümü başına [bağlar](virtual-file-mount.md) | 10 |

<sup>1</sup> Bir görevin en fazla süresi, işe eklendiğinden tamamlandığından tamamlandığından 180 gündür. Tamamlanan görevler yedi gün süreyle devam ediyor; maksimum kullanım ömrü içinde tamamlanmamış görevlere ait verilere erişilemez.

## <a name="view-batch-quotas"></a>Toplu iş kotalarını görüntüleme

Toplu Iş hesabı kotalarınızı [Azure portalında][portal]görüntüleyin.

1. Portaldaki **Toplu İş hesaplarını** seçin ve ardından ilgilendiğiniz Toplu İş hesabını seçin.
1. Toplu İşlem hesabının menüsünde **Kotalar'ı** seçin.
1. Toplu İşlem hesabına uygulanan kotaları görüntüleme

    ![Toplu hesap kotaları][account_quotas]

## <a name="increase-a-quota"></a>Kotayı artırma

[Azure portalını][portal]kullanarak Toplu Iş hesabınız veya aboneliğiniz için kota artışı talep etmek için aşağıdaki adımları izleyin. Kota artışının türü, Toplu Iş hesabınızın havuz ayırma moduna bağlıdır. Kota artışı talep etmek için kotayı artırmak istediğiniz VM serisini eklemeniz gerekir. Kota artışı uygulandığında, tüm VM serisine uygulanır.

### <a name="increase-cores-quota-in-batch"></a>Toplu İşlemde çekirdek kotası artırın 

1. Portal panonuzdaki **Yardım + destek** döşemesini veya portalın sağ üst köşesindeki soru işaretini (**?**) seçin.
1. **Yeni destek isteği** > **Temelleri'ni**seçin.
1. **Temel olarak:**
   
    a. **Sorun Türü** > **Hizmeti ve abonelik limitleri (kotalar)**
   
    b. Aboneliğinizi seçin.
   
    c. **Kota türü** > **Toplu**
      
    **Sonraki'ni**seçin.
    
1. **Ayrıntılar** bölümünde:
      
    a. **Ayrıntıları sağla'da**konumu, kota türünü ve Toplu İşlem hesabını belirtin.
    
    ![Toplu kota artışı][quota_increase]

    Kota türleri şunlardır:

    * **Toplu Hesap Başına**  
        Özel ve düşük öncelikli çekirdekler ve iş ve havuz sayısı dahil olmak üzere tek bir Toplu Iş hesabına özgü değerler.
        
    * **Bölge başına**  
        Bir bölgedeki tüm Toplu İşlem hesapları için geçerli olan ve abonelik başına bölge başına Toplu Iş hesabı sayısını içeren değerler.

    Düşük öncelikli kota, tüm VM serileri arasında tek bir değerdir. Kısıtlı SNU'lara ihtiyacınız varsa, **Düşük öncelikli çekirdekleri** seçmeniz ve talep tepecek VM ailelerini eklemeniz gerekir.

    b. [İş etkinize][support_sev]göre bir **Önem Derecesi** seçin.

    **Sonraki'ni**seçin.

1. **İletişim bilgileri**:
   
    a. Tercih **edilen kişi yöntemini**seçin.
   
    b. Gerekli iletişim bilgilerini doğrulayın ve girin.
   
    Destek isteğini göndermek için **Oluştur'u** seçin.

Destek isteğinizi gönderdikten sonra Azure desteği sizinle iletişime geçer. Kota talepleri birkaç dakika veya iki iş günü içinde tamamlanabilir.

## <a name="related-quotas-for-vm-pools"></a>VM havuzları için ilgili kotalar

Azure sanal ağında dağıtılan Sanal Makine Yapılandırması'ndaki toplu iş havuzları, ek Azure ağ kaynaklarını otomatik olarak ayırır. Sanal ağdaki her 50 havuz düğümü için aşağıdaki kaynaklar gereklidir:

* Bir [ağ güvenlik grubu](../virtual-network/security-overview.md#network-security-groups)
* Bir [genel IP adresi](../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* Bir [yük dengeleyici](../load-balancer/load-balancer-overview.md)

Bu kaynaklar, Toplu Iş havuzu oluşturulurken sağlanan sanal ağı içeren abonelikte ayrılır. Bu kaynaklar, aboneliğin [kaynak kotalarıyla](../azure-resource-manager/management/azure-subscription-service-limits.md) sınırlıdır. Sanal ağda büyük havuz dağıtımları planlıyorsanız, bu kaynaklar için aboneliğin kotalarını denetleyin. Gerekirse, **Yardım + destek'i**seçerek Azure portalında bir artış isteyin.


## <a name="related-topics"></a>İlgili konular
* [Azure portalı kullanarak Azure Batch hesabı oluşturma](batch-account-create-portal.md)
* [Azure Toplu İşlem özelliğine genel bakış](batch-api-basics.md)
* [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: https://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
[quota_increase]: ./media/batch-quota-limit/quota-increase.png
