---
title: Azure Geçişi Hakkında
description: Azure Geçiş hizmeti hakkında bilgi edinin.
ms.topic: overview
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: fe6386346282cf182397f6420c541d629ba0aab3
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81768391"
---
# <a name="about-azure-migrate"></a>Azure Geçişi Hakkında

Bu makalede, Azure Geçiş hizmetine hızlı bir genel bakış sağlanmaktadır.

Azure Geçiş, Azure şirket içi sunucuları, altyapı, uygulamalar ve verileri değerlendirmek ve Azure'a geçirmek için merkezi bir hub sağlar.

Azure Geçiş aşağıdaki özellikleri sağlar:

- **Birleşik geçiş platformu**: Azure'a geçişinizi başlatmak, çalıştırmak ve izlemek için tek bir portal.
- **Araç aralığı**: Değerlendirme ve geçiş için çeşitli araçlar. Araçlar arasında Azure Geçişi: Sunucu Değerlendirmesi ve Azure Geçişi: Sunucu Geçişi. Azure Geçir, diğer Azure hizmetleriyle ve diğer araçlarla ve bağımsız yazılım satıcısı (ISV) teklifleriyle tümleşir.
- **Değerlendirme ve geçiş**: Azure Geçir hub'ında şunları değerlendirebilir ve geçirebilirsiniz:
    - **Sunucular**: Şirket içi sunucuları değerlendirin ve azure sanal makinelerine geçirin.
    - **Veritabanları**: Şirket içi veritabanlarını değerlendirin ve bunları Azure SQL Veritabanı'na veya Azure SQL Veritabanı yönetilen bir örneğe geçirin.
    - **Web uygulamaları**: Azure Uygulama Hizmeti Geçiş Yardımcısı'nı kullanarak şirket içi web uygulamalarını değerlendirin ve Azure Uygulama Hizmeti'ne geçirin.
    - **Sanal masaüstü bilgisayarlar**: Şirket içi sanal masaüstü altyapınızı (VDI) değerlendirin ve Azure'da Windows Sanal Masaüstüne geçirin.
    - **Veri**: Azure Veri Kutusu ürünlerini kullanarak büyük miktarda veriyi Hızlı ve uygun maliyetli bir şekilde Azure'a taşıyın.

## <a name="integrated-tools"></a>Entegre araçlar

Azure Geçiş merkezi şu araçları içerir:

**Araç** | **Değerlendirme ve göç** | **Şey**
--- | --- | ---
**Azure Geçiş: Sunucu Değerlendirmesi** | Sunucuları değerlendirin. | Azure'a geçiş ekihazırlık için şirket içi VMware VM'leri, Hyper-V V'leri ve fiziksel sunucuları keşfedin ve değerlendirin.
**Azure Geçiş: Sunucu Geçişi** | Sunucuları geçirin. | VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları, diğer sanallaştırılmış makineleri ve genel bulut VM'lerini Azure'a geçirin.
**Data Migration Yardımcısı** | Azure SQL Veritabanı'na geçiş için şirket içi SQL Server veritabanlarını, Azure SQL Veritabanı yönetilen örneğini veya SQL Server çalıştıran Azure VM'leri değerlendirin. | Veri Geçişi Yardımcısı, geçişi engelleyen olası sorunları belirlemeye yardımcı olur. Desteklenmeyen özellikleri, geçişten sonra size fayda sağabilecek yeni özellikleri ve veritabanı geçişi için doğru yolu tanımlar. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).
**Azure Veritabanı Geçiş Hizmeti** | Şirket içi veritabanlarını SQL Server, Azure SQL Veritabanı veya Azure SQL Veritabanı yönetilen örnekleri çalıştıran Azure VM'lerine geçirin. | Veritabanı Geçiş Hizmeti hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/dms/dms-overview)
**Movere** | Sunucuları değerlendirin. | Movere hakkında [daha fazla bilgi edinin.](#movere)
**Web uygulaması geçiş asistanı** | Şirket içi web uygulamalarını değerlendirin ve Azure'a geçirin. |  Şirket içi web sitelerini Azure Uygulama Hizmeti'ne geçiş için değerlendirmek için Azure Uygulama Hizmeti Geçiş Yardımcısı'nı kullanın.<br/><br/> .NET ve PHP web uygulamalarını Azure'a geçirmek için Geçiş Yardımcısı'nı kullanın. Azure Uygulama Hizmeti Geçiş Yardımcısı hakkında [daha fazla bilgi edinin.](https://appmigration.microsoft.com/)
**Azure Data Box** | Çevrimdışı verileri geçirin. | Büyük miktarda çevrimdışı veriyi Azure'a taşımak için Azure Veri Kutusu ürünlerini kullanın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/databox/).

> [!NOTE]
> Azure Kamu'daysanız, harici tümleşik araçlar ve ISV teklifleri Azure Geçiş projelerine veri gönderemez. Araçları bağımsız olarak kullanabilirsiniz.

## <a name="isv-integration"></a>ISV entegrasyonu

Azure Geçir, çeşitli ISV teklifleri ile tümleşir. 

**ısv**    | **Özellik**
--- | ---
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Sunucuları geçirin.
[Cloudamize](https://www.cloudamize.com/platform) | Sunucuları değerlendirin.
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Sunucuları değerlendirin ve geçirin.
[Device42](https://docs.device42.com/) | Sunucuları değerlendirin.
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | VDI'yi değerlendirin.
[Raf Lar](https://go.microsoft.com/fwlink/?linkid=2102735) | Sunucuları geçirin.
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Sunucuları değerlendirin.
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Sunucuları ve veritabanlarını değerlendirin.

## <a name="azure-migrate-server-assessment-tool"></a>Azure Geçiş: Sunucu Değerlendirme aracı

Azure Geçişi: Sunucu Değerlendirme aracı, Azure'a geçiş için şirket içi VMware VM'leri, Hyper-V V'leri ve fiziksel sunucuları keşfeder ve değerlendirir.

Araç şunları yapar:

- **Azure'a hazırlık**: Şirket içi makinelerin Azure'a geçişe hazır olup olmadığını değerlendirir.
- **Azure boyutlandırma**: Geçişten sonra Azure VM'lerinin boyutunu tahmin eder.
- **Azure maliyet tahmini**: Azure'da şirket içi sunucuçalıştırma maliyetlerini tahmin eder.
- **Bağımlılık çözümlemesi**: Bağımlı sunucuları Azure'a taşımak için sunucular arası bağımlılıkları ve optimizasyon stratejilerini tanımlar. [Bağımlılık analizi](concepts-dependency-visualization.md)ile Sunucu Değerlendirmesi hakkında daha fazla bilgi edinin.

Sunucu Değerlendirmesi, şirket içinde dağıttığınız hafif bir [Azure Geçiş cihazı](migrate-appliance.md) kullanır.

- Cihaz VM veya fiziksel sunucuda çalışır. İndirilen bir şablonu kullanarak kolayca yükleyebilirsiniz.
- Cihaz, şirket içi makineleri keşfeder. Ayrıca, makine meta verilerini ve performans verilerini Sürekli olarak Azure Geçiş'e gönderir.
- Cihaz keşfi aracısızdır. Keşfedilen makinelere hiçbir şey yüklenmesin.
- Cihaz keşfinden sonra, keşfedilen makineleri gruplar halinde toplayabilir ve her grup için değerlendirmeler çalıştırabilirsiniz.

## <a name="azure-migrate-server-migration-tool"></a>Azure Geçiş: Sunucu Geçişi aracı

Azure Geçir: Sunucu Geçişi aracı Azure'a geçiş yapmaya yardımcı olur:

- Şirket içi VMware sanal makineleri
- Hyper-V Sanal Makineleri
- Fiziksel sunucular
- Diğer sanallaştırılmış makineler
- Genel bulut VM'leri

Makineleri değerlendirdikten sonra geçirebilir veya değerlendirme yapmadan geçirebilirsiniz.

VMware VM'lerinin aracısız geçişi ve Hyper-V VM'lerin geçişi için Sunucu Geçişi, şirket içinde dağıttığınız bir Azure Geçiş cihazı kullanır. Sunucu değerlendirmesini ayarlarsanız cihaz da kullanılır. Bir önceki bölümde açıklanmıştır.

## <a name="selecting-assessment-and-migration-tools"></a>Değerlendirme ve geçiş araçlarının seçilmesi

Azure Geçir hub'ında, değerlendirme veya geçiş için kullanmak istediğiniz aracı seçin ve bir Azure Geçiş projesine eklersiniz. Bir ISV aracı veya Movere eklerseniz:

- Başlamak için, bir lisans almak veya araç yönergelerini izleyerek ücretsiz deneme için kaydolun. Her ISV veya araç araç lisanslama belirtir.
- Her aracın Azure Geçiş'e bağlanma seçeneği vardır. Bağlanmak için araç yönergelerini izleyin.
- Azure Geçiş projesi nin içinden tüm araçlararasında geçişinizi izleyin.

## <a name="movere"></a>Movere

Movere bir hizmet (SaaS) platformu olarak bir yazılımdır. Tüm BT ortamlarını tek bir gün içinde doğru bir şekilde sunarak iş zekalarını artırır. Kuruluşlar ve işletmeler büyür, değişir ve dijital olarak optimize eder. Bunu yaparken, Movere platform, uygulama veya coğrafya ne olursa olsun, çevrelerini görmek ve kontrol etmek için gerekli güveni sağlar.

Microsoft [Movere'ı satın aldı](https://azure.microsoft.com/blog/microsoft-acquires-movere-to-help-customers-unlock-cloud-innovation-with-seamless-migration-tools/) ve artık tek başına bir teklif olarak satılmadı. Movere, Microsoft Çözüm Değerlendirmesi ve Microsoft Bulut Ekonomisi Programı aracılığıyla kullanılabilir. Movere hakkında [daha fazla bilgi edinin.](https://www.movere.io)

Yerleşik geçiş hizmetimiz olan Azure Geçiş'e de bakmanızı öneririz. Azure Geçiş, bulut geçişinizi basitleştirmek için merkezi bir hub sağlar. Hub, fiziksel ve sanal sunucular, veritabanları ve uygulamalar gibi iş yüklerini kapsamlı bir şekilde destekler. Uçtan uca görünürlük, keşif, değerlendirme ve geçiş boyunca kaydedilen ilerlemeyi kolayca izlemenize olanak tanır.

Azure ve iş ortağı ISV araçları nın yerleşik olduğu Azure Geçiş, aşağıdakiler dahil olmak üzere çok çeşitli özelliklere sahiptir:

- Sanal ve fiziksel sunucuların keşfi.
- Performansa dayalı hak etme.
- Maliyet planlaması.
- İthalat tabanlı değerlendirmeler.
- Aracısız uygulamaların bağımlılık analizi.

Başlamak için uzman yardımı arıyorsanız, Microsoft size rehberlik edecek yetenekli [Azure Uzman Yönetilen Hizmet Sağlayıcılarına](https://azure.microsoft.com/partners) sahiptir. [Azure Geçiş web sitesine](https://azure.microsoft.com/services/azure-migrate/)göz atın. 

## <a name="azure-migrate-versions"></a>Azure Geçiş sürümleri

Azure Geçiş hizmetinin iki sürümü vardır.

- **Geçerli sürüm**: Azure Geçir projeleri oluşturmak, şirket içi makineleri keşfetmek ve değerlendirmeleri ve geçişleri düzenlemek için bu sürümü kullanın. Bu sürümdeki yenilikler hakkında [daha fazla bilgi edinin.](whats-new.md)
- **Önceki sürüm**: Azure Geçir'in önceki sürümü yalnızca şirket içi VMware VM'lerin değerlendirilmesini destekler. Önceki sürümü kullandıysanız, şimdi geçerli sürümü kullanmanız gerekir. Önceki sürümü kullanarak Azure Geçiş projeleri oluşturamaz. Ve onunla yeni keşifler yapmamanızı öneririz.

    Azure portalındaki varolan projelere erişmek için **Azure Geçiş'i**arayın ve seçin. **Azure Geçir** panosunda eski Azure Geçir projelerine erişmek için bir bildirim ve bağlantı vardır.

## <a name="next-steps"></a>Sonraki adımlar

- [VMware VM'leri,](tutorial-prepare-vmware.md) [Hyper-V VM'leri](tutorial-prepare-hyper-v.md)veya [fiziksel sunucuları](tutorial-prepare-physical.md)değerlendirmek için eğitimlerimizi deneyin.
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).
