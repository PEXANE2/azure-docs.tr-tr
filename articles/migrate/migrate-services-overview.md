---
title: Azure Geçişi Hakkında
description: Azure geçişi hizmeti hakkında bilgi edinin.
ms.topic: overview
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: 72f2dfb3e5e05466892d838af4e1c84c855b94a9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86122090"
---
# <a name="about-azure-migrate"></a>Azure Geçişi Hakkında

Bu makalede, Azure geçişi hizmetine hızlı bir genel bakış sunulmaktadır.

Azure geçişi, Azure şirket içi sunucuları, altyapısı, uygulamaları ve verileri değerlendirmek ve bu uygulamalara geçiş yapmak için merkezi bir merkez sağlar. Şunları sağlar:

- **Birleşik geçiş platformu**: Azure 'a geçişinizi başlatmak, çalıştırmak ve izlemek için tek bir portal.
- **Araç aralığı**: değerlendirme ve geçiş için bir dizi araç. Azure geçiş araçları arasında sunucu değerlendirmesi ve Azure geçişi: sunucu geçişi bulunur. Azure geçişi, diğer Azure hizmetleri ve araçlarıyla ve bağımsız yazılım satıcısı (ISV) teklifleriyle de tümleştirilir.
- **Değerlendirme ve geçiş**: Azure geçiş hub 'ında şunları değerlendirebileceğiniz ve geçirebilirsiniz:
    - **Sunucular**: şirket içi sunucuları değerlendirin ve Azure sanal makinelerine veya Azure VMware çözümüne (AVS) (Önizleme) geçirin.
    - **Veritabanları**: şirket içi veritabanlarını değerlendirin ve BUNLARı Azure SQL veritabanı 'NA veya SQL yönetilen örneği 'ne geçirin.
    - **Web uygulamaları**: şirket içi Web uygulamalarını değerlendirin ve Azure App Service Geçiş Yardımcısı kullanarak onları Azure App Service geçirin.
    - **Sanal masaüstleri**: şirket içi sanal masaüstü ALTYAPıNıZı (VDI) değerlendirin ve Azure 'Da Windows sanal masaüstüne geçirin.
    - **Veriler**: büyük miktarlarda verileri Azure 'a hızla ve Azure Data Box ürünlerini kullanarak uygun maliyetli bir şekilde geçirin.

## <a name="integrated-tools"></a>Tümleşik araçlar

Azure geçişi hub 'ı şu araçları içerir:

**Araç** | **Değerlendirin ve geçirin** | **Ayrıntılar**
--- | --- | ---
**Azure geçişi: Sunucu değerlendirmesi** | Sunucuları değerlendirin. | Şirket içi VMware VM 'Leri, Hyper-V VM 'Leri ve fiziksel sunucuları Azure 'a geçişe hazırlık olarak bulun ve değerlendirin.
**Azure geçişi: sunucu geçişi** | Sunucuları geçirin. | VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, diğer sanallaştırılmış makineler ve genel bulut VM 'lerini Azure 'a geçirin.
**Data Migration Yardımcısı** | Azure SQL veritabanı, Azure SQL yönetilen örneği veya SQL Server çalıştıran Azure VM 'lerine geçiş için SQL Server veritabanlarını değerlendirin. | Data Migration Yardımcısı, geçişi engelleyen olası sorunları belirlemenize yardımcı olur. Desteklenmeyen özellikleri, geçişten sonra size faydalanabilir yeni özellikleri ve veritabanı geçişinin doğru yolunu tanımlar. [Daha fazla bilgi edinin](/sql/dma/dma-overview?view=sql-server-2017).
**Azure Veritabanı Geçiş Hizmeti** | Şirket içi veritabanlarını SQL Server, Azure SQL veritabanı veya SQL yönetilen örnekleri çalıştıran Azure VM 'lerine geçirin. | Veritabanı geçiş hizmeti hakkında [daha fazla bilgi edinin](../dms/dms-overview.md) .
**Movere** | Sunucuları değerlendirin. | Movere hakkında [daha fazla bilgi edinin](#movere) .
**Web uygulaması Geçiş Yardımcısı** | Şirket içi Web uygulamalarını değerlendirin ve bunları Azure 'a geçirin. |  Azure App Service geçiş için şirket içi web sitelerini değerlendirmek üzere Azure App Service Geçiş Yardımcısı kullanın.<br/><br/> .NET ve PHP Web uygulamalarını Azure 'a geçirmek için Geçiş Yardımcısı kullanın. Azure App Service Geçiş Yardımcısı hakkında [daha fazla bilgi edinin](https://appmigration.microsoft.com/) .
**Azure Data Box** | Çevrimdışı verileri geçirin. | Büyük miktarlarda çevrimdışı verileri Azure 'a taşımak için Azure Data Box ürünlerini kullanın. [Daha fazla bilgi edinin](../databox/index.yml).

> [!NOTE]
> Azure Kamu kullanıyorsanız, dış tümleşik araçlar ve ISV teklifleri verileri Azure geçişi projelerine gönderemez. Araçları bağımsız olarak kullanabilirsiniz.

## <a name="isv-integration"></a>ISV tümleştirmesi

Azure geçişi, çeşitli ISV teklifleriyle tümleştirilir. 

**'Ye**    | **Özellik**
--- | ---
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Sunucuları geçirin.
[Cloudamize](https://www.cloudamize.com/platform) | Sunucuları değerlendirin.
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Sunucuları değerlendirin ve geçirin.
[Device42](https://docs.device42.com/) | Sunucuları değerlendirin.
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | VDı 'yi değerlendirin.
[RackWare](https://go.microsoft.com/fwlink/?linkid=2102735) | Sunucuları geçirin.
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Sunucuları değerlendirin.
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Sunucuları ve veritabanlarını değerlendirin.

## <a name="azure-migrate-server-assessment-tool"></a>Azure geçişi: Sunucu değerlendirmesi aracı

Azure geçişi: Sunucu değerlendirmesi Aracı, Azure 'a geçiş için şirket içi VMware VM 'Leri, Hyper-V VM 'Leri ve fiziksel sunucuları bulur ve değerlendirir. 

Araç şunları yapar:

- **Azure hazırlığı**: değerlendirir şirket Içi makinelerin Azure 'a geçiş için hazır olup olmadığını belirtir.
- **Azure boyutlandırma**: geçiş sonrasında Azure VM 'lerinin boyutunu veya Azure VMware düğümlerinin sayısını tahmin eder.
- **Azure maliyet tahmini**: Azure 'da şirket içi sunucuları çalıştırmaya yönelik maliyetleri tahmin eder.
- **Bağımlılık Analizi**: bağımlı sunucuları Azure 'a taşımak için çapraz sunucu bağımlılıklarını ve iyileştirme stratejilerini belirler. [Bağımlılık analizine](concepts-dependency-visualization.md)sahip sunucu değerlendirmesi hakkında daha fazla bilgi edinin.

Sunucu değerlendirmesi, şirket içinde dağıttığınız basit bir [Azure geçiş](migrate-appliance.md) gereci kullanır.

- Gereç bir VM veya fiziksel sunucuda çalışır. İndirilen bir şablonu kullanarak kolayca yükleyebilirsiniz.
- Gereç, şirket içi makineleri bulur. Ayrıca, Azure geçişi 'ne sürekli olarak makine meta verileri ve performans verileri gönderir.
- Gereç keşfi aracısız. Bulunan makinelerde hiçbir şey yüklü değil.
- Gereç keşfi sonrasında, bulunan makineleri gruplar halinde toplayabilir ve her grup için değerlendirmeler çalıştırabilirsiniz.

## <a name="azure-migrate-server-migration-tool"></a>Azure geçişi: sunucu geçiş aracı

Azure geçişi: sunucu geçiş aracı, Azure 'a geçiş yapmanıza yardımcı olur:

**Geçiş** | **Ayrıntılar**
--- | ---
Şirket içi VMware sanal makineleri | Aracısız veya aracı tabanlı geçiş kullanarak VM 'Leri Azure 'a geçirin.<br/><br/> Aracısız geçiş için sunucu geçişi, şirket içinde dağıttığınız bir Azure geçiş gereci kullanır. Sunucu değerlendirmesi için kullandığınız gereç türü vardır.<br/><br/> Sunucu değerlendirmesi, aracı tabanlı geçiş için bir çoğaltma gereci kullanır.
Şirket içi Hyper-V VM'leri | VM 'Leri Azure 'a geçirin.<br/><br/> Sunucu değerlendirmesi, geçiş için Hyper-V konağına yüklenen sağlayıcı aracılarını kullanır.
Şirket içi fiziksel sunucular | Fiziksel makineleri Azure 'a geçirebilirsiniz. Ayrıca, diğer sanallaştırılmış makineleri ve diğer genel bulutlardan VM 'Leri geçiş amacıyla sanal makineler olarak düşünerek geçirebilirsiniz. | Sunucu değerlendirmesi geçiş için bir çoğaltma gereci kullanır.


## <a name="selecting-assessment-and-migration-tools"></a>Değerlendirme ve geçiş araçlarını seçme

Azure geçişi hub 'ında değerlendirme veya geçiş için kullanmak istediğiniz aracı seçin ve bunu bir Azure geçişi projesine ekleyin. Bir ISV aracı veya Movere eklerseniz:

- Başlamak için, araç yönergelerini izleyerek bir lisans alın veya ücretsiz deneme için kaydolun. Her ISV veya araç, araç lisansını belirler.
- Her aracın Azure geçişi 'ne bağlanma seçeneği vardır. Bağlanmak için araç yönergelerini izleyin.
- Azure geçişi projesi içinden tüm araçlarda geçişlerinizi izleyin.

## <a name="movere"></a>Movere

Movere, hizmet olarak yazılım (SaaS) platformudur. Tek bir gün içinde tüm BT ortamlarını doğru şekilde sunarak iş zekası artar. Kuruluşlar ve kuruluşlar büyür, değişir ve dijital olarak iyileştirmektedir. Bu şekilde, Movere, platformlarını, platformunu, uygulamayı veya coğrafi konumu her ne olursa olsun, ortamları görmek ve denetlemek için gereken güvenle sağlar.

Microsoft movere 'yi [edinmiş](https://azure.microsoft.com/blog/microsoft-acquires-movere-to-help-customers-unlock-cloud-innovation-with-seamless-migration-tools/) ve artık tek başına bir teklif olarak satılmamaktadır. Movere, Microsoft çözüm değerlendirmesi ve Microsoft Bulut Ekonomikler programı aracılığıyla kullanılabilir. Movere hakkında [daha fazla bilgi edinin](https://www.movere.io) .

Ayrıca, yerleşik geçiş hizmetimiz olan Azure geçişi ' ne da bakmanız önerilir. Azure geçişi, bulut geçişinizi basitleştirecek bir merkezi Merkez sağlar. Merkez, fiziksel ve sanal sunucular, veritabanları ve uygulamalar gibi iş yüklerini büyük ölçüde destekler. Uçtan uca görünürlük, bulma, değerlendirme ve geçiş genelinde ilerlemeyi kolayca izlemenize olanak sağlar.

Hem Azure hem de iş ortağı ISV araçlarıyla Azure geçişi, aşağıdakiler dahil olmak üzere çok çeşitli özelliklere sahiptir:

- Sanal ve fiziksel sunucuları bulma.
- Performans tabanlı doğru geliştirme.
- Maliyet planlaması.
- İçeri aktarma temelli değerlendirmeler.
- Aracısız uygulamaların bağımlılık analizi.

Kullanmaya başlamak için uzman yardımı arıyorsanız, Microsoft size rehberlik etmek için [Azure uzman tarafından yönetilen hizmet sağlayıcılarına](https://azure.microsoft.com/partners) sahiptir. [Azure geçişi Web sitesine](https://azure.microsoft.com/services/azure-migrate/)göz atın. 

## <a name="azure-migrate-versions"></a>Azure geçişi sürümleri

Azure geçişi hizmetinin iki sürümü vardır.

- **Geçerli sürüm**: bu sürümü kullanarak Azure geçişi projelerini oluşturun, şirket içi makineleri bulun ve değerlendirmeleri ve geçişleri düzenleyin. Bu sürümdeki yenilikler hakkında [daha fazla bilgi edinin](whats-new.md) .
- **Önceki sürüm**: Azure geçişi 'nin önceki sürümü yalnızca şirket Içi VMware VM 'lerinin değerlendirmesini destekler. Önceki sürümü kullandıysanız, artık geçerli sürümü kullanmanız gerekir. Artık önceki sürümü kullanarak Azure geçişi projelerini oluşturamazsınız. Ayrıca, bununla yeni bulmalar yapmanızı öneririz.

    Azure portal mevcut projelere erişmek için **Azure geçişi**' ni arayıp seçin. **Azure geçiş** panosu, eski Azure geçiş projelerine erişim için bir bildirime ve bağlantıya sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

- [VMware VM](tutorial-prepare-vmware.md)'leri, [Hyper-V VM](tutorial-prepare-hyper-v.md)'lerini veya [fiziksel sunucuları](tutorial-prepare-physical.md)değerlendirmek için öğreticilerimizi deneyin.
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).
