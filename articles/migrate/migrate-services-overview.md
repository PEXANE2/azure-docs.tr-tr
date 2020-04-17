---
title: Azure Geçişi Hakkında
description: Azure Geçiş hizmeti hakkında bilgi edinin.
ms.topic: overview
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: aa999b874b6e8b0075ab905a72ff04f03cb39ac7
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537908"
---
# <a name="about-azure-migrate"></a>Azure Geçişi Hakkında

Bu makalede, Azure Geçiş hizmetine hızlı bir genel bakış sağlanmaktadır.

Azure Geçiş, şirket içi sunucuları, altyapıyı, uygulamaları ve verileri Azure'a değerlendirmek ve geçirmek için merkezi bir hub sağlar. Azure Geçiş aşağıdaki özellikleri sağlar:

- **Birleşik geçiş platformu**: Azure'a geçiş yolculuğunuz için başlangıç, çalıştırma ve izleme tek bir portal.
- **Araç aralığı**: Değerlendirme ve geçiş için çeşitli araçlar. Araçlar arasında Azure Geçişi: Sunucu Değerlendirmesi ve Azure Geçişi: Sunucu Geçişi. Azure Geçir, diğer Azure hizmetleriyle ve diğer araçlarla ve bağımsız yazılım satıcısı (ISV) teklifleriyle tümleşir.
- **Değerlendirme ve geçiş**: Azure Geçir hub'ında aşağıdakileri değerlendirebilir ve geçirebilirsiniz:
    - **Sunucular**: Şirket içi sunucuları değerlendirin ve Azure VM'lerine geçirin.
    - **Veritabanları**: Şirket içi veritabanlarını Azure SQL DB'ye veya Azure SQL Yönetilen Örneği'ne değerlendirin ve geçirin.
    - **Web uygulamaları**: Azure Uygulama Hizmeti Yardımcısı'nı kullanarak şirket içi web uygulamalarını değerlendirin ve Azure Uygulama Hizmeti'ne geçirin.
    - **Sanal masaüstü bilgisayarlar**: Şirket içi sanal masaüstü altyapınızı (VDI) Azure'daki Windows Sanal Masaüstü'ne değerlendirin ve geçirin.
    - **Veri**: Azure Veri Kutusu ürünlerini kullanarak büyük miktarda veriyi Hızlı ve uygun maliyetli bir şekilde Azure'a taşıyın. 


## <a name="integrated-tools"></a>Entegre araçlar

Azure Geçir hub'ı bu araçları içerir.

**Araç** | **Değerlendir/Geçir** | **Şey**
--- | --- | ---
**Azure Geçiş:Sunucu Değerlendirmesi** | Sunucuları değerlendirin. | Azure'a geçişe hazırlık olarak şirket içi VMware VM'leri, Hyper-V V'leri ve fiziksel sunucuları keşfedin ve değerlendirin.
**Azure Geçir:Sunucu Geçişi** | Sunucuları geçirin. | VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları, diğer sanallaştırılmış makineleri ve genel bulut VM'lerini Azure'a geçirin. 
**Veritabanı Geçiş Yardımcısı (DMA)** | Azure SQL DB, Azure SQL Yönetilen Örneği veya SQL Server çalıştıran Azure VM'lere geçiş için şirket içi SQL Server veritabanlarını değerlendirin. | DMA, geçiş için olası engelleme sorunlarını belirlemeye yardımcı olur. Desteklenmeyen özellikleri, geçişten sonra yararlanabileceğiniz yeni özellikleri tanımlar ve veritabanı geçişi için doğru yolu belirlemenize yardımcı olur. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).
**Veritabanı Geçiş Hizmeti (DMS)** | Şirket içi veritabanlarını SQL, Azure SQL DB ve Azure SQL Yönetilen Örnekleri çalıştıran Azure VM'lerine geçirin. | DMS hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/dms/dms-overview)
**Movere** | Sunucuları değerlendirin. | Movere hakkında [daha fazla bilgi edinin.](#movere)
**Web Uygulaması Geçiş Asistanı** | Şirket içi web uygulamalarını değerlendirin ve Azure'a geçirin. |  Şirket içi web sitelerini Azure Uygulama Hizmeti'ne geçiş için değerlendirmek için Azure Uygulama Hizmeti Geçiş Yardımcısı'nı kullanın.<br/><br/> .NET ve PHP web uygulamalarını Azure'a geçirmek için Yardımcısı'nı kullanın. Azure Uygulama Hizmeti Geçiş Yardımcısı hakkında [daha fazla bilgi edinin.](https://appmigration.microsoft.com/)
**Azure Data Box** | Çevrimdışı veri geçişi. | Büyük miktarda veriyi çevrimdışı olarak Azure'a taşımak için Azure Veri Kutusu ürünlerini kullanın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/databox/).

> [!NOTE]
> Azure Kamu'daysanız, harici tümleşik araçlar ve ISV teklifleri Azure Geçiş projelerine veri gönderemez. Araçları bağımsız olarak kullanabilirsiniz.

## <a name="isv-integration"></a>ISV entegrasyonu

Azure Geçir, bir dizi ISV teklifiyle tümleşir. 

**ısv**    | **Özellik**
--- | ---
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Sunucuları geçir
[Cloudamize](https://www.cloudamize.com/platform) | Sunucuları değerlendirin
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Sunucuları değerlendirme ve geçirme
[Cihaz 42](https://docs.device42.com/) | Sunucuları değerlendirin
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | VDI'yi Değerlendirin
[Raf Lar](https://go.microsoft.com/fwlink/?linkid=2102735) | Sunucuları geçir
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Sunucuları değerlendirin
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Sunucuları ve veritabanlarını değerlendirme


## <a name="azure-migrate-server-assessment-tool"></a>Azure Geçir Sunucu Değerlendirme aracı

Azure Geçir:Sunucu Değerlendirmesi aracı, Azure'a geçiş için şirket içi VMware VM'leri, Hyper-V V'leri ve fiziksel sunucuları keşfeder ve değerlendirir. Araç şunları yapar:

- **Azure hazırlığı:** Şirket içi makinelerin Azure'a geçişe hazır olup olmadığını değerlendirir.
- **Azure boyutlandırma:** Geçişten sonra Azure VM'lerinin boyutunu tahmin ediyor.
- **Azure maliyet tahmini:** Azure'da şirket içi sunucuları çalıştırmak için tahmini maliyetler.
- **Bağımlılık analizi:** [Bağımlılık çözümlemesi](concepts-dependency-visualization.md)ile Sunucu Değerlendirmesi kullanıyorsanız, sunucular arası bağımlılıkları etkili bir şekilde tanımlayabilir ve bağımlı sunucuları Azure'a taşımak için stratejileri optimize edebilirsiniz.


Sunucu Değerlendirmesi, şirket içinde dağıttığınız hafif bir [Azure Geçiş cihazı](migrate-appliance.md) kullanır.

- Cihaz VM veya fiziksel sunucuda çalışır. İndirilen bir şablonu kullanarak kolayca yükleyebilirsiniz.
- Cihaz şirket içi makineleri keşfeder ve sürekli olarak Azure Geçiş'e makine meta verileri ve performans verileri gönderir.
- Cihaz keşfi aracısızdır. Keşfedilen makinelere hiçbir şey yüklenmesin.
- Cihaz keşfinden sonra, keşfedilen makineleri gruplar halinde toplar ve bir grup için değerlendirmeler çalıştırın.

## <a name="azure-migrate-server-migration-tool"></a>Azure Geçişi Sunucu Geçişi aracı

Azure Geçiş:Sunucu Geçişi aracı, şirket içi VMware VM'leri, Hyper-V V'leri, fiziksel sunucuları, diğer sanallaştırılmış makineleri ve genel bulut VM'lerini Azure'a geçirmenize yardımcı olur. Makineleri değerlendirdikten sonra geçirebilirsiniz veya değerlendirme yapmadan geçirebilirsiniz. 

VMware VM'lerin aracısız geçişi ve Hyper-V VM'lerin geçişi için Sunucu Geçişi, şirket içinde dağıttığınız bir Azure Geçiş cihazı kullanır. Sunucu değerlendirmesini ayarlarsanız cihaz da kullanılır ve önceki bölümde açıklanmıştır.


## <a name="selecting-assessmentmigration-tools"></a>Değerlendirme/geçiş araçlarını seçme

Azure Geçir hub'ında, değerlendirme veya geçiş için kullanmak istediğiniz aracı seçin ve bir Azure Geçiş projesine eklersiniz. Bir ISV aracı veya Movere eklerseniz:

- Başlamak için bir lisans almak veya araç talimatlarına uygun olarak ücretsiz deneme için kaydolun. Araç lisanslama ISV veya araç tarafından belirlenir. 
- Her araçta, Azure Geçiş'e bağlanma seçeneği vardır. Bağlanmak için araç yönergelerini izleyin.
- Geçiş seyahatinizi Azure Geçiş projesi nin içinden tüm araçlarda izlersiniz.


## <a name="movere"></a>Movere

Movere, tüm BT ortamlarını tek bir gün içinde doğru bir şekilde sunarak iş zekalarını artıran bir SaaS platformudur. Kuruluşlar büyüdükçe, değiştikçe ve dijital olarak optimize ettikçe, çözüm işletmelere platform, uygulama veya coğrafyadan bağımsız olarak ortamlarını görünürlük ve kontrol sahibi olmaları için ihtiyaç duydukları güveni sağlar. Movere Microsoft tarafından [satın alındı](https://azure.microsoft.com/blog/microsoft-acquires-movere-to-help-customers-unlock-cloud-innovation-with-seamless-migration-tools/) ve artık bağımsız bir teklif olarak satılamaz.  Movere, Microsoft Çözüm Değerlendirme ve Bulut Ekonomisi Programları aracılığıyla kullanılabilir. Movere hakkında [daha fazla bilgi edinin.](https://www.movere.io) 

Yerleşik geçiş hizmetimiz olan Azure Geçiş'e de bakmanızı öneririz. Azure Geçiş, buluta geçişinizi kolaylaştırmak için merkezi bir hub sağlar. Hub, fiziksel ve sanal sunucular, veritabanları ve uygulamalar da dahil olmak üzere farklı iş yükleri için kapsamlı destek sunar. Uçtan uca görünürlük, keşif, değerlendirme ve geçiş boyunca kaydedilen ilerlemeyi izlemeyi kolaylaştırır. Azure ve iş ortağı ISV araçlarının yerleşik olduğu Azure Geçir, sanal ve fiziksel sunucu bulma, performansa dayalı doğru boyutlandırma, maliyet planlaması, içe aktarma tabanlı değerlendirmeler ve aracısız uygulama bağımlılık çözümlemesi gibi geniş bir özelliklere sahiptir. Başlamak için uzman yardımı arıyorsanız, Microsoft yolculuk boyunca size rehberlik edecek yetenekli [Azure Uzman Yönetilen Hizmet Sağlayıcısı'na](https://azure.microsoft.com/partners) sahiptir. [Azure Geçiş web sitesine](https://azure.microsoft.com/services/azure-migrate/)göz atın. 
 

## <a name="azure-migrate-versions"></a>Azure Geçiş sürümleri

Azure Geçiş hizmetinin iki sürümü vardır:

- **Geçerli sürüm**: Azure Geçir projeleri oluşturmak, şirket içi makineleri keşfetmek ve değerlendirmeleri ve geçişleri düzenlemek için bu sürümü kullanın. Bu sürümdeki yenilikler hakkında [daha fazla bilgi edinin.](whats-new.md)
- **Önceki sürüm**: Azure Geçiş önceki sürümünü kullandıysanız (yalnızca şirket içi VMware VM'lerin değerlendirmesi desteklendi), artık geçerli sürümü kullanmanız gerekir. Önceki sürümü kullanarak azure geçiş projeleri oluşturamaz ve yeni keşifler gerçekleştirmemenizi öneririz. Azure portalında varolan projelere erişmek için **Azure Geçiş'i**arayın ve seçin. Azure **Geçir** panosunda, eski Azure Geçir projelerine erişmek için bir bildirim ve bağlantı vardır.



## <a name="next-steps"></a>Sonraki adımlar

- [VMware VM'leri,](tutorial-prepare-vmware.md) [Hyper-V VM'leri](tutorial-prepare-hyper-v.md)veya [fiziksel sunucuları](tutorial-prepare-physical.md)değerlendirmek için eğitimlerimizi deneyin.
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).
