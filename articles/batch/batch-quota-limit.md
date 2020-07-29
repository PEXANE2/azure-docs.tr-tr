---
title: Hizmet kotaları ve limitleri
description: Varsayılan Azure Batch kotaları, sınırları ve kısıtlamaları hakkında bilgi edinin ve kota artışlarının nasıl isteneceğini öğrenin
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: 4c13df8b537d701400a22cd2871e7f8362f02455
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84417285"
---
# <a name="batch-service-quotas-and-limits"></a>Batch hizmet kotaları ve limitleri

Diğer Azure hizmetlerinde olduğu gibi, Batch hizmetiyle ilişkili belirli kaynaklarda sınırlamalar vardır. Bu limitlerin birçoğu, Azure tarafından abonelik veya hesap düzeyinde uygulanan varsayılan kotalardır.

Batch iş yüklerinizi tasarlarken ve ölçeklendirebilmeniz için bu kotaları göz önünde bulundurun. Örneğin, havuzunuz belirttiğiniz işlem düğümlerinin hedef sayısına ulaşmazsa, Batch hesabınız için çekirdek kota sınırına ulaşmış olabilirsiniz.

Tek bir Batch hesabında birden fazla Batch iş yükü çalıştırabilir ya da iş yüklerinizi aynı abonelikte ancak farklı Azure bölgelerindeki Batch hesapları arasında dağıtabilirsiniz.

Batch 'de üretim iş yüklerini çalıştırmayı planlıyorsanız, varsayılan olarak bir veya daha fazla kotayı artırmanız gerekebilir. Bir kota yükseltmek istiyorsanız, çevrimiçi bir [Müşteri Destek isteğini](#increase-a-quota) ücretsiz olarak açabilirsiniz.

## <a name="resource-quotas"></a>Kaynak kotaları

Kota, kapasite garantisi değil, kredi limiti olur. Büyük ölçekli kapasite gereksinimleriniz varsa lütfen Azure desteğine başvurun.

Ayrıca kotaların garantili değerler olmadığına de unutmayın. Kotalar, Batch hizmetinden değişikliklere göre değişiklik gösterebilir veya bir Kullanıcı isteği kota değerini değiştirebilir.

[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

### <a name="cores-quotas-in-user-subscription-mode"></a>Kullanıcı abonelik modundaki çekirdek kotaları

Havuz ayırma modu **Kullanıcı aboneliğine**ayarlanmış bir [Batch hesabı](accounts.md) oluşturduysanız, Kotalar farklı şekilde uygulanır. Bu modda, bir havuz oluşturulduğunda Batch VM 'Leri ve diğer kaynaklar doğrudan aboneliğinizde oluşturulur. Azure Batch çekirdekler kotaları, bu modda oluşturulan bir hesaba uygulanmaz. Bunun yerine, bölgesel işlem çekirdekleri ve diğer kaynaklar için aboneliğinizdeki kotalar uygulanır.

Bu kotalar hakkında daha fazla bilgi edinmek için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md).

## <a name="pool-size-limits"></a>Havuz boyutu sınırları

Havuz boyutu sınırları Batch hizmeti tarafından ayarlanır. [Kaynak kotalarından](#resource-quotas)farklı olarak, bu değerler değiştirilemez. Yalnızca düğümler arası iletişim ve özel görüntülere sahip havuzlar standart kotadan farklı kısıtlamalara sahiptir.

| **Kaynak** | **Maksimum sınır** |
| --- | --- |
| **[Düğüm içi iletişim etkin havuzunda](batch-mpi.md) işlem düğümleri**  ||
| Batch hizmeti havuz ayırma modu | 100 |
| Batch abonelik havuzu ayırma modu | 80 |
| ** [Yönetilen görüntü kaynağı 1 ile oluşturulan havuzdaki](batch-custom-images.md)işlem düğümleri**<sup>1</sup> ||
| Ayrılmış düğümler | 2000 |
| Düşük öncelikli düğümler | 1000 |

düğüm içi iletişim olmayan havuzlar için <sup>1</sup> .

## <a name="other-limits"></a>Diğer sınırlar

Batch hizmeti tarafından ayarlanan ek sınırlar. [Kaynak kotalarından](#resource-quotas)farklı olarak, bu değerler değiştirilemez.

| **Kaynak** | **Maksimum sınır** |
| --- | --- |
| İşlem düğümü başına [eşzamanlı görevler](batch-parallel-node-tasks.md) | 4 x düğüm çekirdeği sayısı |
| Batch hesabı başına [uygulamalar](batch-application-packages.md) | 20 |
| Uygulama başına uygulama paketleri | 40 |
| Havuz başına uygulama paketleri | 10 |
| En fazla görev ömrü | 180 gün<sup>1</sup> |
| İşlem düğümü başına [bağlama](virtual-file-mount.md) | 10 |

<sup>1</sup> bir görevin, ne zaman tamamlandığında işe eklendiği zaman, 180 gün olur. Tamamlanan görevler yedi gün boyunca devam ederse; Maksimum yaşam süresi içinde tamamlanmayan görevlere yönelik verilere erişilemiyor.

## <a name="view-batch-quotas"></a>Batch kotalarını görüntüleme

Batch hesabı kotalarınızı [Azure Portal](https://portal.azure.com)görüntülemek için:

1. **Batch hesapları**' nı seçin ve ilgilendiğiniz Batch hesabını seçin.
1. Batch hesabının menüsünde **Kotalar** ' ı seçin.
1. Batch hesabına uygulanmış olan kotaları görüntüleyin.

    ![Batch hesabı kotaları][account_quotas]

## <a name="increase-a-quota"></a>Kotayı artırma

[Azure Portal](https://portal.azure.com)kullanarak, Batch hesabınız veya aboneliğiniz için bir kota artışı isteyebilirsiniz. Kota artışı türü, Batch hesabınızın havuz ayırma moduna bağlıdır. Kota artışı istemek için, kotayı artırmak istediğiniz VM serisini dahil etmeniz gerekir. Kota artışı uygulandığında, tüm VM dizisine uygulanır.

1. Portal panonuzda **Yardım + Destek** kutucuğunu veya portalın sağ üst köşesindeki soru işaretini (**?**) seçin.
1. **Yeni destek isteği**  >  **temelleri**' ni seçin.
1. **Temel bilgiler**:
   
    1. **Sorun türü**  >  **Hizmet ve abonelik limitleri (kotalar)**
   
    1. Aboneliğinizi seçin.
   
    1. **Kota türü**  >  **Batch**
      
       **İleri**’yi seçin.
    
1. **Ayrıntılar** bölümünde:
      
    1. **Ayrıntıları sağla**bölümünde konumu, kota türünü ve Batch hesabını belirtin.
    
       ![Toplu kota artışı][quota_increase]

       Kota türleri şunlardır:

       * **Batch hesabı başına**  
         Adanmış ve düşük öncelikli çekirdekler ve iş ve havuz sayısı dahil olmak üzere tek bir Batch hesabına özgü değerler.
        
       * **Bölge başına**  
         Bir bölgedeki tüm Batch hesaplarına uygulanan ve abonelik başına bölge başına toplu Işlem hesaplarının sayısını içeren değerler.

       Düşük öncelikli kota, tüm VM serileri genelinde tek bir değerdir. Kısıtlanmış SKU 'Lara ihtiyacınız varsa **düşük öncelikli çekirdekler** ' ı seçmeniz ve isteğe sanal makine aileleri eklemeniz gerekir.

    1. [İşletmenizin etkilerine](https://aka.ms/supportseverity)göre **önem derecesi** seçin.

       **İleri**’yi seçin.

1. **İletişim bilgileri**:
   
    1. **Tercih edilen bir iletişim yöntemi**seçin.
   
    1. Gerekli iletişim ayrıntılarını doğrulayın ve girin.
   
       Destek isteğini göndermek için **Oluştur** ' u seçin.

Destek isteğinizi gönderdikten sonra, Azure desteği sizinle iletişim kuracaktır. Kota istekleri, birkaç dakika veya en fazla iki iş günü içinde tamamlanabilir.

## <a name="related-quotas-for-vm-pools"></a>VM havuzları için ilgili kotalar

Bir Azure sanal ağında dağıtılan sanal makine yapılandırmasındaki toplu iş havuzları, otomatik olarak ek Azure ağ kaynakları ayırır. Bir sanal ağdaki her 50 havuz düğümü için aşağıdaki kaynaklar gereklidir:

- Bir [ağ güvenlik grubu](../virtual-network/security-overview.md#network-security-groups)
- Bir [genel IP adresi](../virtual-network/public-ip-addresses.md)
- Bir [yük dengeleyici](../load-balancer/load-balancer-overview.md)

Bu kaynaklar, Batch havuzu oluşturulurken sağlanan sanal ağı içeren abonelikte ayrılır. Bu kaynaklar, aboneliğin [kaynak kotalarıyla](../azure-resource-manager/management/azure-subscription-service-limits.md) sınırlıdır. Büyük havuz dağıtımlarını bir sanal ağda planlarsanız, bu kaynaklar için aboneliğin kotalarını kontrol edin. Gerekirse, **Yardım + Destek**' i seçerek Azure Portal bir artış isteyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Portal kullanarak bir Azure Batch hesabı oluşturun](batch-account-create-portal.md).
* [Batch hizmeti iş akışı ve](batch-service-workflow-features.md) havuzlar, düğümler, işler ve görevler gibi birincil kaynaklar hakkında bilgi edinin.
* [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)hakkında bilgi edinin.

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
[quota_increase]: ./media/batch-quota-limit/quota-increase.png
