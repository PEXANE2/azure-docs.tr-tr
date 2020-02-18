---
title: Azure geçişi gereci hakkında sık sorulan sorular
description: Azure geçişi gereci hakkında sık sorulan soruların yanıtlarını alın
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 3bb066b08447a951665e629da5ebcb75714b9f1e
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425364"
---
# <a name="azure-migrate-appliance-common-questions"></a>Azure geçişi gereci: genel sorular

Bu makalede, Azure geçişi gereci hakkında sık sorulan sorular yanıtlanmaktadır. Başka sorularınız varsa, şu makaleleri gözden geçirin:

- Azure geçişi hakkında [genel sorular](resources-faq.md) .
- Bulma, değerlendirme ve bağımlılık görselleştirmesiyle ilgili [sorular](common-questions-discovery-assessment.md) .
- Sunucu geçişi hakkında [sorular](common-questions-server-migration.md) .
- [Azure geçişi forumundan](https://aka.ms/AzureMigrateForum)sorulara yanıt alın. 


## <a name="what-is-the-azure-migrate-appliance"></a>Azure geçişi gereci nedir?

Azure geçişi gereci, şirket içi sunucuları bulup değerlendirmek için Azure geçişi: Sunucu değerlendirmesi aracı tarafından kullanılan hafif bir gereç. Gereç, şirket içi VMware VM 'lerinin aracısız geçişi için Azure geçişi: sunucu geçiş aracı tarafından da kullanılır. 

- Gereç, şirket içinde bir VM veya fiziksel makine olarak dağıtılır.
- Gereç, şirket içi makineleri bulur ve sürekli olarak makine meta verilerini ve performans verilerini Azure geçişi 'ne gönderir.
- Gereç keşfi aracısız. Bulunan makinelerde hiçbir şey yüklü değil.

Gereç hakkında [daha fazla bilgi edinin](migrate-appliance.md) .

## <a name="how-does-the-appliance-connect-to-azure"></a>Gereç Azure 'a nasıl bağlanır?

Gereç Internet üzerinden bağlanabilir veya Azure ExpressRoute 'u ortak/Microsoft eşlemesiyle kullanabilir.

## <a name="does-appliance-analysis-impact-performance"></a>Gereç Analizi etkisi performansı mı?

Azure, Gereç profillerini şirket içi makineleri performans verilerini ölçmek için sürekli olarak geçirin. Bu profil oluşturma, neredeyse hiçbir performans etkisi profili oluşturulmuş makineye sahiptir.

### <a name="can-i-harden-the-appliance-vm"></a>Gereç sanal makinesini kullanabilir miyim?

İndirilen şablonu kullanarak gereç sanal makinesini oluşturduğunuzda, Azure geçişi gereci yerinde gereken iletişim ve güvenlik duvarı kurallarını bıraktığınızda, şablona bileşen (örneğin, bir virüsten koruma) ekleyebilirsiniz.


## <a name="what-network-connectivity-is-needed"></a>Hangi ağ bağlantısı gerekir?

Ağ bağlantısıyla ilgili bilgileri gözden geçirin:
- Azure geçişi gereci kullanarak VMware değerlendirmesi: [URL](migrate-appliance.md#url-access) ve [bağlantı noktası](migrate-support-matrix-vmware.md#port-access) erişim gereksinimleri.
- Azure geçişi gereci kullanarak VMware aracısız geçişi: [URL](migrate-appliance.md#url-access) ve [bağlantı noktası](migrate-support-matrix-vmware-migration.md#agentless-ports) erişim gereksinimleri.
- Azure geçişi gereci kullanan Hyper-V değerlendirmesi: [URL](migrate-appliance.md#url-access) ve [bağlantı noktası](migrate-support-matrix-hyper-v.md#port-access) erişim gereksinimleri.


## <a name="what-data-does-the-appliance-collect"></a>Gereç hangi verileri toplar?

Toplanan verileri gözden geçirin:

- VMware VM [performans verileri](migrate-appliance.md#collected-performance-data-vmware) ve [meta](migrate-appliance.md#collected-metadata-vmware)verileri.
- Hyper-V VM [performans verileri](migrate-appliance.md#collected-performance-data-hyper-v) ve [meta verileri](migrate-appliance.md#collected-metadata-hyper-v).


## <a name="how-is-data-stored"></a>Veriler nasıl depolanır?

Azure geçişi gereci tarafından toplanan veriler, Azure geçişi projesini oluşturduğunuz Azure konumunda depolanır. 

- Veriler bir Microsoft aboneliğine güvenli bir şekilde depolanır ve Azure geçişi projesini sildiğinizde silinir.
- [Bağımlılık görselleştirmesi](concepts-dependency-visualization.md)kullanıyorsanız, toplanan veriler Azure aboneliğinde oluşturulan bir Log Analytics çalışma alanında Birleşik Devletler depolanır. Bu veriler, aboneliğinizdeki Log Analytics çalışma alanını sildiğinizde silinir.

## <a name="how-much-data-is-uploaded-in-continuous-profiling"></a>Sürekli profil oluşturmaya ne kadar veri yüklendi?

Azure geçişi 'ne gönderilen veri hacmi, bir dizi parametreye bağlıdır. Size bir fikir vermek için, 10 makineye sahip bir Azure geçişi projesi (her biri bir disk ve bir NIC ile) günde 50 MB 'a gönderilir. Bu değer, NIC ve disklerin veri noktası sayısına göre yaklaşık ve değişikliklerdir. Makine, NIC 'ler veya disk sayısında artış varsa, gönderilen verilerdeki artış doğrusal değildir.

## <a name="is-data-encrypted-at-restin-transit"></a>Veriler Rest/geçiş sırasında mi şifrelendi?

Evet, her ikisi için.

- Meta veriler, HTTPS üzerinden Internet üzerinden Azure geçişi hizmetine güvenli bir şekilde gönderilir.
- Meta veriler bir [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) veritabanında ve [Azure Blob depolamada](../storage/common/storage-service-encryption.md), bir Microsoft aboneliğine depolanır. Meta veriler Rest 'de şifrelenir.
- Bağımlılık analizine yönelik veriler de aktarım sırasında şifrelenir (güvenli HTTPS). Bu, aboneliğinizdeki bir Log Analytics çalışma alanında depolanır. Ayrıca, Rest 'ten de şifrelenir.

## <a name="how-does-the-appliance-connect-to-vcenter-server"></a>Gereç vCenter Server nasıl bağlanır?

1. Gereç, gereci ayarlarken verdiğiniz kimlik bilgilerini kullanarak vCenter Server (bağlantı noktası 443) öğesine bağlanır.
2. Gereç, vCenter Server tarafından yönetilen VM 'Lerle ilgili meta verileri toplamak için vCenter Server sorgulamak üzere VMware PowerCLI kullanır.
3. Gereç, sanal makineler (çekirdek, bellek, diskler, NIC 'Ler) ve geçmiş ay için her bir VM 'nin performans geçmişi hakkında yapılandırma verileri toplar.
4. Toplanan meta veriler Azure geçişi 'ne gönderilir: değerlendirme için sunucu değerlendirmesi (HTTPS üzerinden Internet üzerinden).

## <a name="can-i-connect-the-appliance-to-multiple-vcenter-servers"></a>Gereci birden çok vCenter sunucusuna bağlayabilirim miyim?

Hayır. Bir gereç ve vCenter Server arasında bire bir eşleme vardır. Birden çok vCenter Server örneğinde VM 'Leri saptamak için birden çok gereç dağıtmanız gerekir.

## <a name="how-many-vms-or-servers-can-i-discover-with-an-appliance"></a>Bir gereç ile kaç VM veya sunucu keşfedebilirim?

En fazla 10.000 VMware VM, en fazla 5.000 Hyper-V VM ve tek bir gereç ile en fazla 250 fiziksel sunucu bulabilirsiniz. Şirket içi ortamınızda daha fazla makineniz varsa, [Hyper-V](scale-hyper-v-assessment.md), [VMware](scale-vmware-assessment.md) ve [fiziksel](scale-physical-assessment.md) değerlendirmesi ölçekleme hakkında bilgi edinin.

## <a name="can-i-delete-an-appliance"></a>Bir gereci silebilir miyim?

Şu anda, bir gereci projeden silme desteklenmez.

- Gereci silmenin tek yolu, Gereç ile ilişkili Azure geçişi projesini içeren kaynak grubunu silmektir.
- Ancak, kaynak grubu silindiğinde diğer kayıtlı gereçler, bulunan envanter, değerlendirmeler ve kaynak grubundaki projeyle ilişkili diğer tüm Azure bileşenleri de silinir.


## <a name="can-i-use-the-appliance-with-a-different-subscriptionproject"></a>Gereci farklı bir abonelik/proje ile kullanabilir miyim?

Bulmayı başlatmak için gereci kullandıktan sonra, farklı bir Azure aboneliğiyle yeniden yapılandıramazsınız veya farklı bir Azure geçişi projesinde kullanamazsınız. Ayrıca, farklı bir vCenter Server VM 'Leri de keşfedeceksiniz. Bu görevler için yeni bir gereç ayarlayın.

## <a name="can-i-set-up-the-appliance-on-an-azure-vm"></a>Gereci bir Azure VM üzerinde ayarlayabilir miyim?
Hayır. Bu, şu anda desteklenmemektedir. 

## <a name="can-i-discover-on-an-esxi-host"></a>ESXi konağında bulunabilir miyim?
Hayır. VMware VM 'lerini bulmanız için bir vCenter Server gerekir.

## <a name="how-do-i-update-the-appliance"></a>Nasıl yaparım? gereç güncelleştirilsin mi?

Varsayılan olarak, Gereç ve yüklü aracıları otomatik olarak güncelleştirilir. Gereç her 24 saatte bir güncelleştirmeleri denetler. Güncelleştirme işlemi başarısız olursa, yeniden deneme olur. Otomatik Güncelleştirmeler yalnızca Gereç ve gereç aracılarını güncelleştirir. İşletim sistemi güncelleştirilmedi. İşletim sistemini güncel tutmak için Microsoft güncelleştirmelerini kullanın.

## <a name="can-i-check-agent-health"></a>Aracı sistem durumunu denetleyebilir miyim?

Portalda, sunucu değerlendirmesi veya sunucu geçiş aracında **Aracı sistem durumu** sayfasına gidin. Burada, Gereç ve Azure 'daki bulma ve değerlendirme aracıları arasındaki bağlantı durumunu kontrol edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure geçişi 'ne genel bakış](migrate-services-overview.md)konusunu okuyun.
