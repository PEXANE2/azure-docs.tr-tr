---
title: Azure Geçişi Hakkında
description: Azure geçişi hizmeti hakkında bilgi edinin.
ms.topic: overview
ms.date: 12/29/2019
ms.custom: mvc
ms.openlocfilehash: c336b2f95a17896edcc7eaad2b2586c6859804f9
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78362141"
---
# <a name="about-azure-migrate"></a>Azure Geçişi Hakkında

Bu makalede, Azure geçişi hizmetine hızlı bir genel bakış sunulmaktadır.

Azure 'a geçiş için Azure geçişi ' ni kullanın. Azure geçişi, aşağıdaki özelliklerle şirket içi altyapıyı, uygulamaları ve verileri değerlendirmek ve Azure 'a geçirmek için merkezi bir merkez sağlar:

- **Birleşik geçiş platformu**: Azure 'a geçiş yolculuğunu başlatmak, çalıştırmak ve izlemek için tek bir portal.
- **Araç aralığı**: değerlendirme ve geçiş için bir dizi araç. Hub, Azure geçişi: Sunucu değerlendirmesi ve Azure geçişi: sunucu geçişi içerir. Diğer Azure hizmetleriyle ve diğer araçlarla ve bağımsız yazılım satıcısı (ISV) teklifleriyle tümleştirilir.
- **Değerlendirme ve geçiş**: Azure geçiş hub 'ında şunları değerlendirebileceğiniz ve geçirebilirsiniz:
    - **Sunucular**: şirket içi sunucuları değerlendirin ve Azure VM 'lerine geçirin.
    - **Veritabanları**: Şirket Içi VERITABANLARıNı Azure SQL DB 'ye veya Azure SQL yönetilen örneğine değerlendirin ve geçirin.
    - **Web uygulamaları**: Azure App Service Yardımcısı 'nı kullanarak şirket içi Web uygulamalarını değerlendirin ve Azure App Service geçirin.
    - **Sanal masaüstleri**: şirket içi sanal masaüstü ALTYAPıNıZı (VDI) Azure 'Da Windows sanal masaüstüne değerlendirin ve geçirin.
    - **Veriler**: Azure Data Box ürünlerini kullanarak, büyük miktarlarda verileri Azure 'a hızla ve uygun maliyetli bir şekilde geçirin. 


## <a name="integrated-tools"></a>Tümleşik araçlar

Azure geçişi hub 'ı aşağıdaki araçları sağlar.

**Araç** | **Değerlendir/geçir** | **Ayrıntılar**
--- | --- | ---
**Azure geçişi: Sunucu değerlendirmesi** | Sunucuları değerlendir | Şirket içi VMware VM 'Leri, Hyper-V VM 'Leri ve fiziksel sunucuları Azure 'a geçiş için hazır olup olmadığını anlamak için bulup değerlendirin.
**Azure geçişi: sunucu geçişi** | Sunucuları geçirme | VMware VM 'lerini, Hyper-V VM 'lerini, fiziksel sunucuları ve diğer sanallaştırılmış makineleri ve genel bulut VM 'lerini Azure 'a geçirin. 
**Veritabanı Geçiş Yardımcısı (DMA)** | Azure SQL VERITABANı, Azure SQL yönetilen örneği veya SQL Server çalıştıran Azure VM 'lerine geçiş için şirket içi SQL Server veritabanlarını değerlendirin. | DMA, geçiş için olası engelleyici sorunlar hakkında bilgi sağlar. Bu, desteklenmeyen özellikleri ve geçişten sonra faydalanabilecek yeni özellikleri tanımlar ve veritabanı geçişinin doğru yolunu tanımlamanızı sağlar. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).
**Veritabanı geçiş hizmeti (DMS)** | Şirket içi veritabanlarını SQL, Azure SQL VERITABANı ve Azure SQL yönetilen örnekleri çalıştıran Azure VM 'lerine geçirin. | DMS hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/dms/dms-overview) .
**Movere** | Sunucuları değerlendir | Movere hakkında [daha fazla bilgi edinin](#movere) .
**Web uygulaması Geçiş Yardımcısı** | Şirket içi Web uygulamalarını değerlendirin ve Azure 'a geçirin. |  Azure App Service 'e geçiş için şirket içi web sitelerini değerlendirmek üzere Azure App Service Geçiş Yardımcısı kullanın.<br/><br/> Azure App Service Geçiş Yardımcısı kullanarak .NET ve PHP Web uygulamalarını Azure 'a geçirin. Yardımcı hakkında [daha fazla bilgi edinin](https://appmigration.microsoft.com/) .
**Azure Data Box** | Çevrimdışı veri geçişi. | Büyük miktarlardaki verileri Azure 'a çevrimdışı taşımak için Azure Data Box ürünlerini kullanın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/databox/).

## <a name="isv-integration"></a>ISV tümleştirmesi

Azure geçişi, çeşitli ISV teklifleri ile tümleşir. 

**'Ye** | **Özellik**
--- | ---
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Sunucuları geçirme
[Cloudamize](https://www.cloudamize.com/platform) | Sunucuları değerlendir
[Corent teknolojisi](https://www.corenttech.com/AzureMigrate/) | Sunucuları değerlendirin ve geçirin
[Cihaz 42](https://docs.device42.com/) | Sunucuları değerlendir
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | VDı 'yi değerlendir
[RackWare](https://go.microsoft.com/fwlink/?linkid=2102735) | Sunucuları geçirme
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Sunucuları değerlendir
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Sunucuları ve veritabanlarını değerlendir


## <a name="azure-migrate-server-assessment-tool"></a>Azure geçişi: Sunucu değerlendirmesi aracı

Azure geçişi: Sunucu değerlendirmesi Aracı, Azure 'a geçiş için şirket içi VMware VM 'leri, Hyper-V VM 'Leri ve fiziksel sunucuları bulur ve değerlendirir. Aşağıdakileri belirlemenize yardımcı olur:

- **Azure hazırlığı:** Şirket içi makinelerin Azure 'a geçiş için hazırlanın olup olmadığını değerlendirin.
- **Azure boyutlandırma:** Geçişten sonra Azure VM 'lerinin tahmini boyutu.
- **Azure maliyet tahmini:** Azure 'da şirket içi sunucuları çalıştırmaya yönelik tahmini maliyetler.
- **Bağımlılık görselleştirmesi:** Bağımlılık görselleştirmesiyle sunucu değerlendirmesi kullanıyorsanız, sunucular arası bağımlılıkları ve bağımlı sunucuları Azure 'a taşımanın en iyi yollarını etkin bir şekilde belirleyebilirsiniz.

Sunucu değerlendirmesi, şirket içinde dağıttığınız bir basit gereç kullanır ve sunucu değerlendirmesiyle kayıt yaptırın.

- Gereç bir fiziksel sunucu veya VM üzerinde çalışır ve indirilen bir şablon kullanılarak kolayca yüklenir.
- Gereç, şirket içi makineleri bulur ve sürekli olarak makine meta verilerini ve performans verilerini Azure geçişi 'ne gönderir.
- Gereç keşfi aracısız. Bulunan makinelerde hiçbir şeyin yüklü olması gerekmez.
- Bulmadan sonra, bulunan makineleri gruplar halinde toplar ve geçiş için grupları değerlendirin.


## <a name="azure-migrate-server-migration-tool"></a>Azure geçişi: sunucu geçiş aracı

Azure geçişi: sunucu geçiş aracı, şirket içi VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, diğer sanallaştırılmış makineler ve genel bulut VM 'lerini Azure 'a geçirmenize yardımcı olur. Makineleri değerlendirdikten sonra geçirebilir veya bir değerlendirme olmadan geçirebilirsiniz.


## <a name="select-a-tool"></a>Bir araç seçin

Azure geçişi hub 'ında değerlendirme için kullanmak istediğiniz aracı seçin ve bunu bir Azure geçişi projesine ekleyin. Bir ISV aracı veya Movere eklerseniz:

- Araç yönergelerine uygun olarak bir lisans edinerek veya ücretsiz deneme için kaydolarak başlayın. Araçların lisanslaması ISV/Tool tarafından belirlenir. 
- Her araçta Azure geçişi 'ne bağlanma seçeneği vardır. Bağlanmak için yönergeleri izleyin.
- Tüm araçların tamamında Azure geçişi projesi içinden geçiş yolculuğunu izleyin.


## <a name="movere"></a>Movere

Movere, tüm BT ortamlarını tek bir gün içinde doğru şekilde sunarak iş zekası artıran bir SaaS platformudur. Kuruluşlar arttıkça, değiştirirken ve dijital olarak iyileştirirken, uygulamalar platforma, uygulamaya veya coğrafi hale bakılmaksızın ortamları üzerinde görünürlük ve denetim sahibi olmaları gereken güveni sağlar. Movere Microsoft tarafından [alındı](https://azure.microsoft.com/blog/microsoft-acquires-movere-to-help-customers-unlock-cloud-innovation-with-seamless-migration-tools/) ve artık tek başına bir teklif olarak satılmamaktadır.  Movere, Microsoft çözüm değerlendirmesi ve bulut Ekonomikler programları aracılığıyla kullanılabilir. Movere hakkında [daha fazla bilgi edinin](https://www.movere.io) . Sorularınız varsa, bunları ' ye gönderebilirsiniz: movereq@microsoft.com veya Microsoft temsilcinize başvurun.

Ayrıca, yerleşik geçiş hizmetimiz olan Azure geçişi ' ne da bakmanız önerilir. Azure geçişi, buluta geçişinizi basitleştirecek bir merkezi Merkez sağlar. Hub, fiziksel ve sanal sunucular, veritabanları ve uygulamalar dahil farklı iş yükleri için kapsamlı destek sunar. Uçtan uca görünürlük, bulma, değerlendirme ve geçiş genelinde ilerlemeyi izlemeyi kolaylaştırır. Hem Azure hem de partner ISV araçları ile birlikte Azure geçişi, sanal ve fiziksel sunucu keşfi, performans tabanlı doğru boyutlandırma, maliyet planlama, içeri aktarma tabanlı değerlendirmeler ve aracısız uygulama gibi çok çeşitli özelliklere sahiptir. bağımlılık analizi. Kullanmaya başlamak için uzman yardımı arıyorsanız, Microsoft, yolculukta size rehberlik etmek için [Azure uzman tarafından yönetilen hizmet sağlayıcısına](https://azure.microsoft.com/partners) sahiptir. [Azure geçişi Web sitesine](https://azure.microsoft.com/services/azure-migrate/)göz atın. 
 

## <a name="azure-migrate-versions"></a>Azure geçişi sürümleri

Azure geçişi hizmetinin iki sürümü vardır:

- **Geçerli sürüm**: bu sürümü kullanarak Azure geçişi projelerini oluşturun, şirket içi makineleri bulun ve değerlendirmeleri ve geçişleri düzenleyin. Bu sürümdeki yenilikler hakkında [daha fazla bilgi edinin](whats-new.md) .
- **Önceki sürüm**: Azure geçişi 'nin önceki sürümünü kullandıysanız (yalnızca şirket Içi VMware VM 'lerinin değerlendirmesi desteklenir), artık geçerli sürümü kullanmalısınız. Artık önceki sürümü kullanarak Azure geçişi projelerini oluşturamazsınız ve yeni bulmalar gerçekleştirmemenizi öneririz. Mevcut projelere erişmek için, Azure portal **Azure geçişi**' ni arayıp seçin. **Azure geçişi** panosunda, eski Azure geçiş projelerine erişim için bir bildirim ve bir bağlantı vardır.



## <a name="next-steps"></a>Sonraki adımlar

- [VMware VM](tutorial-prepare-vmware.md)'leri, [Hyper-V VM 'leri](tutorial-prepare-hyper-v.md)ve [fiziksel sunucuları](tutorial-prepare-physical.md)değerlendirmek için öğreticilerimizi deneyin.
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).
