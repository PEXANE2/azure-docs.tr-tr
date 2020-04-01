---
title: Azure Geçiş cihazı SSS
description: Azure Geçiş cihazı yla ilgili sık sorulan soruların yanıtlarını alın.
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: a08423637f255b973f52616f53172ef6e48d0804
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437605"
---
# <a name="azure-migrate-appliance-common-questions"></a>Azure Geçiş cihazı: Sık sorulan sorular

Bu makalede, Azure Geçiş cihazı yla ilgili sık sorulan sorular yanıtlanmaktadır. Başka sorularınız varsa, şu kaynakları denetleyin:

- Azure Geçiş hakkında [genel sorular](resources-faq.md)
- [Keşif, değerlendirme ve bağımlılık görselleştirme](common-questions-discovery-assessment.md) ile ilgili sorular
- Sunucu [geçişi](common-questions-server-migration.md) ile ilgili sorular
- [Azure Geçiş forumunda](https://aka.ms/AzureMigrateForum) soruları yanıtlayın

## <a name="what-is-the-azure-migrate-appliance"></a>Azure Geçiş cihazı nedir?

Azure Geçir cihazı, Azure Geçiş: Sunucu Değerlendirme aracının şirket içi sunucuları keşfetmek ve değerlendirmek için kullandığı hafif bir cihazdır. Azure Geçiş: Sunucu Geçişi aracı, cihaza şirket içi VMware VM'lerin aracısız geçişi için de kullanılır.

Azure Geçiş cihazı hakkında daha fazla bilgi aşağıda vereb

- Cihaz, vm veya fiziksel makine olarak şirket içinde dağıtılır.
- Cihaz şirket içi makineleri keşfeder ve sürekli olarak Azure Geçiş'e makine meta verileri ve performans verileri gönderir.
- Cihaz keşfi aracısızdır. Keşfedilen makinelere hiçbir şey yüklenmesin.

Cihaz hakkında [daha fazla bilgi edinin.](migrate-appliance.md)

## <a name="how-does-the-appliance-connect-to-azure"></a>Cihaz Azure'a nasıl bağlanır?

Cihaz internet üzerinden veya Azure ExpressRoute kullanarak genel/Microsoft bakışlarıyla bağlanabilir.

## <a name="does-appliance-analysis-affect-performance"></a>Cihaz analizi performansı etkiler mi?

Azure Geçir cihazı profilleri, performans verilerini ölçmek için sürekli olarak şirket içi makinelerde dir. Bu profil oluşturmanın profilli makineler üzerinde neredeyse hiç performans etkisi yoktur.

## <a name="can-i-harden-the-appliance-vm"></a>VM cihazını sertleştirir miyim?

İndirilen şablonu vm cihazını oluşturmak için kullandığınızda, Azure Geçiş cihazı için gerekli olan iletişim ve güvenlik duvarı kurallarını yerinde bırakırsanız şablona bileşenler (örneğin antivirüs) ekleyebilirsiniz.

## <a name="what-network-connectivity-is-required"></a>Hangi ağ bağlantısı gereklidir?

Azure Geçiş cihazı için ağ bağlantısı gereksinimleri hakkında bilgi için aşağıdaki makalelere bakın:

- **VMware değerlendirmesi**: [URL erişimi](migrate-appliance.md#url-access) ve bağlantı [noktası erişimi](migrate-support-matrix-vmware.md#port-access)
- **VMware aracısız geçiş**: [URL erişimi](migrate-appliance.md#url-access) ve bağlantı [noktası erişimi](migrate-support-matrix-vmware-migration.md#agentless-ports)
- **Hyper-V değerlendirmesi**: [URL erişimi](migrate-appliance.md#url-access) ve bağlantı [noktası erişimi](migrate-support-matrix-hyper-v.md#port-access)

## <a name="what-data-does-the-appliance-collect"></a>Cihaz hangi verileri topluyor?

Azure Geçiş cihazının Sanal Cihazlarda topladığı veriler hakkında bilgi için aşağıdaki makalelere bakın:

- **VMware VM**: Toplanan verileri [gözden geçirin.](migrate-appliance.md#collected-data---vmware) [
- **Hyper-V VM**: Toplanan verileri [gözden geçirin.](migrate-appliance.md#collected-data---hyper-v)

## <a name="how-is-data-stored"></a>Veriler nasıl depolanır?

Azure Geçir cihazı tarafından toplanan veriler, Azure Geçir projesini oluşturduğunuz Azure konumunda depolanır.

Verilerin nasıl depolandırıştırılabildikleri hakkında daha fazla bilgi aşağıda vereb

- Toplanan veriler, CosmosDB'de bir Microsoft aboneliğinde güvenli bir şekilde depolanır. Azure Geçir projesini sildiğinizde veriler silinir. Depolama, Azure Geçiş tarafından işlenir. Toplanan veriler için özel olarak bir depolama hesabı seçemezsiniz.
- [Bağımlılık görselleştirmesini](concepts-dependency-visualization.md)kullanıyorsanız, toplanan veriler Abd'de Azure aboneliğinizde oluşturulan bir Azure Log Analytics çalışma alanında depolanır. Aboneliğinizdeki Günlük Analizi çalışma alanını sildiğinizde veriler silinir.

## <a name="how-much-data-is-uploaded-during-continuous-profiling"></a>Sürekli profil oluşturma sırasında ne kadar veri yüklenir?

Azure Geçiş'e gönderilen veri hacmi birden çok parametreye bağlıdır. Örnek olarak, 10 makinesi (her biri bir disk ve bir NIC'si olan) bir Azure Geçiş projesi günde yaklaşık 50 MB veri gönderir. Bu değer yaklaşıktır; gerçek değer, diskler ve NIC'ler için veri noktalarının sayısına bağlı olarak değişir. Makine, disk veya NIC sayısı artarsa, gönderilen verilerdeki artış doğrusal değildir.

## <a name="is-data-encrypted-at-rest-and-in-transit"></a>Veriler istirahatte ve aktarımsırasında şifreleniyor mu?

Evet, her ikisi için de:

- Meta veriler, HTTPS üzerinden Internet üzerinden Azure Geçir hizmetine güvenli bir şekilde gönderilir.
- Meta veriler bir [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) veritabanında ve Microsoft aboneliğinde [Azure Blob depolama](../storage/common/storage-service-encryption.md) alanında depolanır. Meta veriler depolama için istirahatte şifrelenir.
- Bağımlılık analizi için veriler de aktarım sırasında şifrelenir (güvenli HTTPS ile). Aboneliğinizde bir Log Analytics çalışma alanında depolanır. Veriler bağımlılık analizi için istirahatte şifrelenir.

## <a name="how-does-the-appliance-connect-to-vcenter-server"></a>Cihaz vCenter Server'a nasıl bağlanır?

Bu adımlar, cihazın VMware vCenter Server'a nasıl bağlandığı açıklanabilir:

1. Cihaz, cihazı kurarken sağladığınız kimlik bilgilerini kullanarak vCenter Server'a (bağlantı noktası 443) bağlanır.
2. Cihaz vCenter Server tarafından yönetilen VM'ler hakkında meta veri toplamak için vCenter Server'ı sorgulamak için VMware PowerCLI kullanır.
3. Cihaz, VM'ler (çekirdekler, bellek, diskler, NIC'ler) ve her Bir VM'nin son bir aydaki performans geçmişi hakkında yapılandırma verileri toplar.
4. Toplanan meta veriler, değerlendirme için Azure Geçiş: Sunucu Değerlendirme aracına (HTTPS üzerinden internet üzerinden) gönderilir.

## <a name="can-the-azure-migrate-appliance-connect-to-multiple-vcenter-servers"></a>Azure Geçiş cihazı birden çok vCenter Sunucusuna bağlanabilir mi?

Hayır. [Azure Geçir cihazı](migrate-appliance.md) ile vCenter Server arasında bire bir eşleme vardır. Birden çok vCenter Server örneğinde VM'leri bulmak için birden çok cihaz dağıtmanız gerekir. 

## <a name="can-an-azure-migrate-project-have-multiple-appliances"></a>Azure Geçiş projesinde birden çok cihaz olabilir mi?
Bir projede birden çok cihaz eklenebilir. Ancak, bir cihaz yalnızca bir projeyle ilişkilendirilebilir. 

## <a name="can-the-azure-migrate-appliancereplication-appliance-connect-to-the-same-vcenter"></a>Azure Geçiş cihazı/Çoğaltma cihazı aynı vCenter'a bağlanabilir mi?
Evet. Hem Azure Geçiş cihazını (değerlendirme ve aracısız VMware geçişi için kullanılır) hem de çoğaltma cihazını (VMware VM'lerin aracı tabanlı geçişi için kullanılır) aynı vCenter sunucusuna ekleyebilirsiniz.


## <a name="how-many-vms-or-servers-can-i-discover-with-an-appliance"></a>Bir cihazla kaç VM veya sunucu keşfedebilirim?

10.000 VM'ye kadar VM', 5.000'e kadar Hyper-V VM ve tek bir cihazla 250'ye kadar fiziksel sunucu keşfedebilirsiniz. Şirket içi ortamınızda daha fazla makineniz varsa, [Hyper-V değerlendirmesini ölçeklendirme,](scale-hyper-v-assessment.md) [VMware değerlendirmesini ölçeklendirme](scale-vmware-assessment.md)ve [fiziksel sunucu değerlendirmesini ölçeklendirme](scale-physical-assessment.md)hakkında bilgi edinin.

## <a name="can-i-delete-an-appliance"></a>Bir cihazı silebilir miyim?

Şu anda, bir cihazın projeden silmesi desteklenmez.

Cihazı silmenin tek yolu, cihazla ilişkili Azure Geçiş projesini içeren kaynak grubunu silmektir.

Ancak, kaynak grubunun silinmesinde, kaynak grubundaki diğer kayıtlı cihazları, keşfedilen envanteri, değerlendirmeleri ve projeyle ilişkili kaynak grubundaki diğer tüm Azure bileşenlerini de silinir.

## <a name="can-i-use-the-appliance-with-a-different-subscription-or-project"></a>Cihazı farklı bir abonelik veya proje ile kullanabilir miyim?

Cihazı keşif başlatmak için kullandıktan sonra, cihazı farklı bir Azure aboneliğiyle kullanacak şekilde yeniden yapılandıramaz ve farklı bir Azure Geçiş projesinde kullanamazsınız. VM'leri farklı bir vCenter Server örneğinde de keşfedemezsiniz. Bu görevler için yeni bir cihaz ayarlayın.

## <a name="can-i-set-up-the-appliance-on-an-azure-vm"></a>Cihazı Azure VM'de ayarlayabilir miyim?

Hayır. Şu anda, bu seçenek desteklenmez. 

## <a name="can-i-discover-on-an-esxi-host"></a>Bir ESXi sunucusunda keşfedebilir miyim?

Hayır. VMware VM'leri keşfetmek için vCenter Server'a sahip olmalısınız.

## <a name="how-do-i-update-the-appliance"></a>Cihazı nasıl güncelleyebilirim?

Varsayılan olarak, cihaz ve yüklü aracıları otomatik olarak güncelleştirilir. Cihaz her 24 saatte bir güncellemeleri kontrol eder. Başarısız güncelleştirmeler yeniden denendir. 

Bu otomatik güncellemeler yalnızca cihaz ve cihaz temsilcileri tarafından güncellenir. İşletim sistemi Azure Geçiş otomatik güncelleştirmeleri tarafından güncelleştirilmeyecek. İşletim sistemini güncel tutmak için Windows Güncelleştirmeleri'ni kullanın.

## <a name="can-i-check-agent-health"></a>Ajan sağlığını kontrol edebilir miyim?

Evet. Portalda, Azure Geçişi: Sunucu Değerlendirmesi veya Azure Geçişi: Sunucu Geçişi aracı için **Aracı durumu** sayfasına gidin. Burada, Azure ile cihazdaki keşif ve değerlendirme aracıları arasındaki bağlantı durumunu kontrol edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure [Geçiş genel görünümünü](migrate-services-overview.md)okuyun.
