---
title: Azure BT operatörleri için başlangıç kılavuzu | Microsoft Dokümanlar
description: Azure BT operatörleri için başlangıç kılavuzunu başlatın
author: RicksterCDN
ms.author: rclaus
tags: azure-resource-manager
ms.service: azure
ms.topic: overview
ms.workload: infrastructure
ms.date: 08/24/2018
ms.openlocfilehash: 4f9da6cbfe8d1e6b92c39148b275de193730c8f1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77623570"
---
# <a name="get-started-for-azure-it-operators"></a>Azure BT operatörleri için başlayın

Bu kılavuz, Microsoft Azure altyapısının dağıtımı ve yönetimiyle ilgili temel kavramları tanıtır. Bulut bilgi işlemde veya Azure'un kendisinde yeniyseniz, bu kılavuz kavramlar, dağıtım ve yönetim ayrıntılarıyla hızla başlamanıza yardımcı olur. Bu kılavuzun birçok bölümü sanal bir makine dağıtma gibi bir işlemi tartışır ve ardından ayrıntılı teknik ayrıntılar için bir bağlantı sağlar.

## <a name="cloud-computing-overview"></a>Bulut bilgi işleme genel bakış

Bulut bilgi işlem, geleneksel şirket içi veri merkezine modern bir alternatif sunar. Genel bulut satıcıları tüm bilgi işlem altyapısını ve temel yönetim yazılımını sağlar ve yönetir. Bu satıcılar çok çeşitli bulut hizmetleri sağlar. Bu durumda bir bulut hizmeti sanal bir makine, bir web sunucusu veya bulut barındırılan veritabanı altyapısı olabilir. Bulut sağlayıcısı müşterisi olarak, bu bulut hizmetlerini gerektiği gibi kiralarsınız. Bunu yaparken, donanım bakımının sermaye giderini işletme giderine dönüştürürsunuz. Bulut hizmeti de şu avantajları sağlar:

- Büyük bilgi işlem ortamlarının hızlı dağıtımı

- Artık gerekli olmayan sistemlerin hızlı deallocation

- Yük dengeleyicileri gibi geleneksel olarak karmaşık sistemlerin kolay dağıtımı

- Gerektiğinde esnek bilgi işlem kapasitesi veya ölçek lendirme becerisi

- Daha uygun maliyetli bilgi işlem ortamları

- Web tabanlı bir portal veya programlı otomasyon ile her yerden erişim

- Çoğu bilgi işlem ve uygulama gereksinimlerini karşılamak için bulut tabanlı hizmetler

Şirket içi altyapı ile, dağıtılan donanım ve yazılım üzerinde tam denetime sahipsiniz. Tarihsel olarak, bu kadar ölçekleme odaklanmak donanım satın alma kararları yol açmıştır. Bir örnek, en yüksek performans gereksinimlerini karşılamak için daha fazla çekirdekli bir sunucu satın almaktır. Ne yazık ki, bu altyapı bir talep penceresi dışında yeterince kullanılabilir. Azure ile yalnızca ihtiyacınız olan altyapıyı dağıtabilir ve istediğiniz zaman yukarı veya aşağı ayarlayabilirsiniz. Bu, bir performans gereksinimini karşılamak için ek bilgi işlem düğümlerinin dağıtımı yoluyla ölçekleme üzerinde odaklanmaya yol açar. Bulut hizmetlerini ölçekleme, pahalı donanımlar aracılığıyla ölçekleme yapmaktan daha uygun maliyetlidir.

Microsoft, daha planlı olarak dünya çapında birçok Azure veri merkezi dağıttı. Ayrıca, Microsoft Çin ve Almanya gibi bölgelerde egemen bulutlar artmaktadır. Yalnızca en büyük küresel işletmeler veri merkezlerini bu şekilde dağıtabilir, bu nedenle Azure'u kullanmak her boyuttaki işletmelerin hizmetlerini müşterilerine yakın bir şekilde dağıtmasını kolaylaştırır.

Azure, küçük işletmeler için, bilgi işlem talebi arttıkça hızla ölçeklendirme özelliğiyle düşük maliyetli bir giriş noktası sağlar. Bu, altyapıya büyük bir ön sermaye yatırımını önler ve gerektiğinde mimar ve yeniden mimar sistemleri için esneklik sağlar. Bulut bilgi işlem kullanımı, başlangıç büyümesinin ölçek hızında ve başarısız hızlı modeline çok iyi uyum sağlar.

Kullanılabilir Azure bölgeleri hakkında daha fazla bilgi için Azure [bölgelerine](https://azure.microsoft.com/regions/)bakın.

### <a name="cloud-computing-model"></a>Bulut bilgi işlem modeli

Azure, müşterilere sağlanan hizmet kategorilerini temel alan bir bulut bilgi işlem modeli kullanır. Hizmet in üç kategorisi hizmet olarak altyapı (IaaS), Hizmet Olarak Platform (PaaS) ve Hizmet Olarak Yazılım (SaaS) içerir. Satıcılar, bu kategorilerin her birinde bilgi işlem yığınındaki bileşenlerin sorumluluğunun bir kısmını veya tümünün paylaşMaktadır. Bulut bilgi işlem kategorilerinin her birine bir göz atalım.
![Bulut Bilgi İşlem Yığını Karşılaştırması](./media/cloud-computing-comparison.png)

#### <a name="iaas-infrastructure-as-a-service"></a>IaaS: Hizmet olarak altyapı

Bir IaaS bulut satıcısı, bilgisayar sanallaştırmasını etkinleştirmek için tüm fiziksel bilgi işlem kaynaklarını ve gerekli yazılımı çalıştırıp yönetir. Bu hizmetin bir müşterisi, barındırılan bu veri merkezlerinde sanal makineler dağıtıyor. Sanal makineler iş yeri dışında bir veri merkezinde bulunsa da, IaaS tüketicisi işletim sisteminin yapılandırması ve yönetimi üzerinde denetime sahiptir ve altta yatan altyapıyı bulut satıcısına bırakır.

Azure, sanal makineler, sanal makine ölçek kümeleri ve ilgili ağ altyapısı dahil olmak üzere çeşitli IaaS çözümleri içerir. Sanal makineler, "kaldırma ve kaydırma" geçiş modeline olanak sağladığından, başlangıçta hizmetleri Azure'a geçirmek için popüler bir seçimdir. Veri merkezinizde hizmetlerinizi çalıştıran altyapı gibi bir VM yapılandırabilir ve ardından yazılımınızı yeni VM'ye geçirebilirsiniz. URL'ler gibi yapılandırma güncelleştirmeleri yapmanız gerekebilir, ancak birçok uygulamayı bu şekilde geçirebilirsiniz.

Sanal makine ölçek kümeleri Azure Sanal Makineleri'nin üzerine inşa edilmiştir ve aynı Sanal Makinelerden kümeleri dağıtmak için kolay bir yol sağlar. Sanal makine ölçek kümeleri, gerektiğinde yeni VM'lerin otomatik olarak dağıtılabilmesi için otomatik ölçeklendirmeyi de destekler. Bu, sanal makine ölçeği kümelerini Azure Hizmet Kumaşı ve Azure Kapsayıcı Hizmeti gibi üst düzey mikrohizmet bilgi işlem kümelerini barındırmak için ideal bir platform haline getirir.

#### <a name="paas-platform-as-a-service"></a>PaaS: Hizmet olarak platform

PaaS ile, uygulamanızı bulut hizmeti satıcısının sağladığı bir ortama dağısınız. Uygulama geliştirme ve veri yönetimine odaklanabilmeniz için satıcı tüm altyapı yönetimini yapar.

Azure, Azure Uygulama Hizmeti'nin Web Uygulamaları özelliği ve Azure Bulut Hizmetleri (web ve çalışan rolleri) dahil olmak üzere çeşitli PaaS hesaplama teklifleri sunar. Her iki durumda da, geliştiricilerin uygulamayı destekleyen somunlar ve cıvatalar hakkında hiçbir şey bilmeden uygulamalarını dağıtmanın birden çok yolu vardır. Geliştiricilerin sanal makineler (VM' ler) oluşturması, her birinde oturum açmak için Uzak Masaüstü Protokolü (RDP) kullanması veya uygulamayı yüklemesi gerekmiyor. Onlar sadece bir düğmeye (veya yakın) ve Microsoft tarafından sağlanan araçlar VM'ler hükmü ve daha sonra dağıtmak ve onlara uygulama yükleyin.

#### <a name="saas-software-as-a-service"></a>SaaS: Bir hizmet olarak yazılım

SaaS merkezi olarak barındırılan ve yönetilen bir yazılımdır. Genellikle çok kiracılı bir mimariye dayanır- uygulamanın tek bir sürümü tüm müşteriler için kullanılır. Tüm konumlarda en iyi performansı sağlamak için birden çok örneğe ölçeklenebilir. SaaS yazılımı genellikle aylık veya yıllık abonelik yoluyla lisanslanır. SaaS yazılım satıcıları yazılım yığınının tüm bileşenlerinden sorumludur, bu nedenle tek yönettiğiniz hizmetler sağlanan hizmetlerdir.

Microsoft Office 365, SaaS teklifinin iyi bir örneğidir. Aboneler aylık veya yıllık abonelik ücreti öder ler ve Microsoft Exchange, Microsoft OneDrive ve Microsoft Office paketinin geri kalanını hizmet olarak alırlar. Aboneler her zaman en son sürümü almak ve Exchange sunucusu sizin için yönetilir. Office'i her yıl yüklemek ve yükseltmekle karşılaştırıldığında, bu daha az pahalıdır ve daha az çaba gerektirir.

## <a name="azure-services"></a>Azure hizmetleri

Azure, bulut bilgi işlem platformunda birçok hizmet sunmaktadır. Bu hizmetler aşağıdakileri içerir.

### <a name="compute-services"></a>İşlem hizmetleri

Uygulama iş yükünü barındırma ve çalıştırma hizmetleri:

- Azure Sanal Makineler—hem Linux hem de Windows

- Uygulama Hizmetleri (Web Uygulamaları, Mobil Uygulamalar, Mantık Uygulamaları, API Uygulamaları ve Fonksiyon Uygulamaları)

- Azure Toplu İş (büyük ölçekli paralel ve toplu işlem işleri için)

- Azure Service Fabric

- Azure Container Service

### <a name="data-services"></a>Veri hizmetleri

Verileri depolama ve yönetme hizmetleri:

- Azure Depolama (Azure Blob, Sıra, Tablo ve Dosya hizmetlerini içerir)

- Azure SQL Veritabanı

- Azure Cosmos DB

- Microsoft Azure StorSimple

- Redis için Azure Önbelleği

### <a name="application-services"></a>Uygulama hizmetleri

Bina ve işletme uygulamaları için hizmetler:

- Azure Active Directory (Azure AD)

- Dağıtılmış sistemleri bağlamak için Azure Servis Veri Servisi

- Büyük verileri işlemek için Azure HDInsight

- Tümleştirme ve düzenleme iş akışları için Azure Logic Apps

- Azure Medya Hizmetleri

### <a name="network-services"></a>Ağ hizmetleri

Hem Azure içinde hem de Azure ve şirket içi veri merkezleri arasında ağ oluşturmak için hizmetler:

- Azure Sanal Ağ

- Azure ExpressRoute

- Azure tarafından sağlanan DNS

- Azure Traffic Manager

- Azure Content Delivery Network

Azure hizmetleriyle ilgili ayrıntılı belgeler için [Azure hizmet belgelerine](https://docs.microsoft.com/azure)bakın.

## <a name="azure-key-concepts"></a>Azure anahtar kavramları

### <a name="datacenters-and-regions"></a>Veri merkezleri ve bölgeler

Azure, genellikle dünyanın birçok bölgesinde kullanılabilen küresel bir bulut platformudur. Azure'da bir hizmet, uygulama veya VM sağladığınızda, bir bölge seçmeniz istenir. Seçili bölge, uygulamanızın çalıştığı belirli bir veri merkezini temsil eder. Daha fazla bilgi için Azure [bölgelerine](https://azure.microsoft.com/regions/)bakın.

Azure'u kullanmanın avantajlarından biri, uygulamalarınızı dünyanın çeşitli veri merkezlerine dağıtabiliyor olmaktır. Seçtiğiniz bölge uygulamanızın performansını etkileyebilir. Ağ isteklerindeki gecikme süresini azaltmak için müşterilerinizin çoğuna daha yakın bir bölge seçmek en uygun uyrulur. Uygulamanızı belirli ülkelerde/bölgelerde dağıtmak için yasal gereklilikleri karşılamak için bir bölge de seçebilirsiniz.

### <a name="azure-portal"></a>Azure portalında

Azure portalı, Azure kaynakları ve hizmetleri oluşturmak, yönetmek ve kaldırmak için kullanılabilecek web tabanlı bir uygulamadır. Azure portalı [portal.azure.com](https://portal.azure.com)adresinde yer alır. Azure kaynaklarını yönetmek için özelleştirilebilir bir pano ve araç içerir. Ayrıca fatura ve abonelik bilgileri sağlar. Daha fazla bilgi için [Microsoft Azure portalına genel bakış](https://azure.microsoft.com/documentation/articles/azure-portal-overview/) ve Azure kaynaklarını portal aracılığıyla [yönet'e](https://docs.microsoft.com/azure/azure-portal/resource-group-portal)bakın.

### <a name="resources"></a>Kaynaklar

Azure kaynakları, Azure aboneliğine dağıtılan tek tek bilgi işlem, ağ, veri veya uygulama barındırma hizmetleridir. Bazı yaygın kaynaklar sanal makineler, depolama hesapları veya SQL veritabanlarıdır. Azure hizmetleri genellikle birkaç ilgili Azure kaynağından oluşur. Örneğin, bir Azure sanal makinesi nde VM, depolama hesabı, ağ bağdaştırıcısı ve genel IP adresi olabilir. Bu kaynaklar tek tek veya grup olarak oluşturulabilir, yönetilebilir ve silinebilir. Azure kaynakları daha sonra bu kılavuzda daha ayrıntılı olarak ele alınmıştır.

### <a name="resource-groups"></a>Kaynak grupları

Azure kaynak grubu, Bir Azure çözümü için ilgili kaynakları barındıran bir kapsayıcıdır. Kaynak grubu, çözüm için tüm kaynakları veya grup olarak yönetmek istediğiniz kaynakları içerebilir. Azure kaynak grupları daha sonra bu kılavuzda daha ayrıntılı olarak ele alınmıştır.

### <a name="resource-manager-templates"></a>Resource Manager şablonları

Azure Kaynak Yöneticisi şablonu, kaynak grubuna dağıtılabilmek için bir veya daha fazla kaynak tanımlayan bir JavaScript Nesne Gösterimi (JSON) dosyasıdır. Ayrıca, dağıtılan kaynaklar arasındaki bağımlılıkları da tanımlar. Kaynak Yöneticisi şablonları daha sonra bu kılavuzda daha ayrıntılı olarak ele alınmıştır.

### <a name="automation"></a>Automation

Azure portalını kullanarak kaynak oluşturmanın, yönetmenin ve silmenin yanı sıra PowerShell veya Azure komut satırı arabirimini (CLI) kullanarak bu etkinlikleri otomatikleştirebilirsiniz.

#### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell, Azure'u yönetmek için cmdlets sağlayan bir modül kümesidir. Azure hizmetlerini oluşturmak, yönetmek ve kaldırmak için cmdlet'leri kullanabilirsiniz. Cmdlets tutarlı, tekrarlanabilir ve hands-off dağıtımları elde edebilirsiniz yardımcı olabilir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-Az-ps).

#### <a name="azure-command-line-interface"></a>Azure komut satırı arabirimi

Azure komut satırı arabirimi, Azure kaynaklarını komut satırından oluşturmak, yönetmek ve kaldırmak için kullanabileceğiniz bir araçtır. Azure CLI, Linux, Mac OS X ve Windows için kullanılabilir. Daha fazla bilgi ve teknik ayrıntılar için Azure [CLI'yi yükleyin.](/cli/azure/install-azure-cli)

#### <a name="rest-apis"></a>REST API'leri

Azure, Azure portalı ui'sini destekleyen bir REST API kümesi üzerine oluşturulmuştür. Bu REST API'lerinin çoğu, Azure kaynaklarınızı ve uygulamalarınızı Internet özellikli herhangi bir cihazdan programlı olarak sağlamanıza ve yönetmenize olanak sağlamak için de desteklenir. Daha fazla bilgi için [Azure REST SDK Başvurusu'na](https://docs.microsoft.com/rest/api/index)bakın.

### <a name="azure-cloud-shell"></a>Azure Cloud Shell

Yöneticiler Azure PowerShell ve Azure CLI'ye Azure Bulut Shell adı verilen tarayıcıda erişilebilen bir deneyimle erişebilir. Bu etkileşimli arabirim, Linux ve Windows yöneticilerinin Bash veya PowerShell gibi tercih ettikleri komut satırı arabirimini kullanmaları için esnek bir araç sağlar. Azure Bulut Shell portalından [(shell.azure.com](https://shell.azure.com)tek başına bir web arabirimi olarak veya diğer erişim noktalarından erişebilir. Daha fazla bilgi için Azure [Bulut BulutU'na Genel Bakış](https://docs.microsoft.com/azure/cloud-shell/overview)bölümüne bakın.

## <a name="azure-subscriptions"></a>Azure abonelikleri

Abonelik, Azure hesabına bağlı Azure hizmetlerinin mantıksal bir gruplandırmasIdır. Tek bir Azure hesabı birden çok abonelik içerebilir. Azure hizmetlerinin faturalandırması abonelik başına yapılır. Azure aboneliklerinde, abonelik üzerinde tam denetime sahip bir Hesap Yöneticisi ve abonelikteki tüm hizmetleri denetleyen bir Hizmet Yöneticisi vardır. Klasik abonelik yöneticileri hakkında bilgi için Azure [abonelik yöneticileri ekle veya değiştir'e](../../cost-management-billing/manage/add-change-subscription-administrator.md)bakın. Yöneticilere ek olarak, tek tek hesaplara rol [tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md)kullanılarak Azure kaynaklarının ayrıntılı denetimi verilebilir.

### <a name="select-and-enable-an-azure-subscription"></a>Azure aboneliğini seçin ve etkinleştirin

Azure hizmetleriyle çalışabilmeniz için önce bir aboneliğe ihtiyacınız var. Çeşitli abonelik türleri mevcuttur.

**Ücretsiz hesaplar**: Ücretsiz bir hesaba kaydolmak için bağlantı [Azure web sitesindedir.](https://azure.microsoft.com/) Bu, Azure'daki herhangi bir kaynak birleşimini denemeniz için 30 gün boyunca kredi verir. Kredi tutarınızı aşarsanız, hesabınız askıya alınır. Deneme nin sonunda, hizmetleriniz kullanımdan kaldırıldı ve artık çalışmayacak. İstediğiniz zaman istediğiniz kadar öde aboneliğine yükseltebilirsiniz.

**MSDN abonelikleri**: MSDN aboneliğiniz varsa, her ay Azure kredisinde belirli bir tutar elde edeyim. Örneğin, MSDN aboneliği olan bir Microsoft Visual Studio \$Enterprise'A sahipseniz, Azure kredisinde ayda 150 alırsınız.

Kredi tutarını aşarsanız, hizmetiniz bir sonraki ay başlayana kadar devre dışı bırakılır. Harcama limitini kapatabilir ve ek maliyetler için kullanılacak bir kredi kartı ekleyebilirsiniz. Bu maliyetlerden bazıları MSDN hesapları için indirimlidir. Örneğin, Windows Server çalıştıran VM'ler için Linux ücretini ödersiniz ve Microsoft SQL Server gibi Microsoft sunucuları için ek ücret alınmaz. Bu, MSDN hesaplarını geliştirme ve test senaryoları için ideal hale getirir.

**BizSpark hesapları**: Microsoft BizSpark programı yeni başlayanlar için birçok avantaj sağlar. Bu avantajlardan biri, en fazla beş MSDN hesabı için geliştirme ve test ortamları için tüm Microsoft yazılımlarına erişimdir. Bu beş MSDN hesabının her biri için 150 TL Azure kredisi alırsınız ve Sanal Makineler gibi Azure hizmetlerinin birkaçı için indirimli ücretler ödenirsiniz.

**Kullandıkça Öde**: Bu abonelikle, kullandığınız parayı hesaba bir kredi kartı veya banka kartı ekleyerek ödenebilirsiniz. Bir kuruluşsanız, faturalama için de onaylanabilirsiniz.

**Kurumsal anlaşmalar**: Kurumsal bir sözleşmeyle, gelecek yıl Azure'da belirli sayıda hizmet kullanmayı taahhüt edersiniz ve bu tutarı önceden ödersiniz. Yaptığınız taahhüt yıl boyunca tüketilir. Taahhüt tutarını aşarsanız, fazla ödemeyi borç olarak ödeyebilirsiniz. Taahhüdün miktarına bağlı olarak, Azure'daki hizmetlerde indirim alırsınız.

### <a name="grant-administrative-access-to-an-azure-subscription"></a>Azure aboneliğine yönetim erişimi verme

RBAC'ın izin atamak için kullanabileceğiniz birkaç yerleşik rolü vardır. Bir kullanıcıyı Azure aboneliğinin yöneticisi yapmak için, bu kullanıcıya abonelik kapsamında [Ki'nin Sahibi](../../role-based-access-control/built-in-roles.md#owner) rolünü atayın. Sahip rolü, kullanıcıya, başkalarına erişim yetkisi devretme hakkı da dahil olmak üzere abonelikteki tüm kaynaklara tam erişim izni verir.

Daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).

### <a name="view-billing-information-in-the-azure-portal"></a>Azure portalında fatura bilgilerini görüntüleme

Azure'u kullanmanın önemli bir bileşeni, fatura bilgilerini görüntüleme yeteneğidir. Azure portalı, Azure faturalandırma bilgileri hakkında ayrıntılı bilgiler sağlar.

Daha fazla bilgi için [Azure fatura faturanızı ve günlük kullanım verilerinizi nasıl karşıdan yükleyebilirsiniz.](../../cost-management-billing/manage/download-azure-invoice-daily-usage-date.md)

### <a name="get-billing-information-from-billing-apis"></a>Fatura apilerinden fatura bilgileri alma

Portaldaki faturalandırmayı görüntülemenin yanı sıra, Azure FaturaLAMA REST API'leri aracılığıyla bir komut dosyası veya program kullanarak fatura bilgilerine de erişebilirsiniz:

- Kullanım verilerinizi almak için Azure Kullanım API'sini kullanabilirsiniz. İlgili Azure kaynaklarını etiketleyerek fatura kullanım bilgilerinde ince ayar yapabilirsiniz. Örneğin, bir kaynak grubundaki kaynakların her birini bir departman adı veya proje adı yla etiketleyebilir ve ardından bu etiketiçin özel olarak maliyetleri izleyebilirsiniz.

- Azure Hız Kartı API'sini, bu kaynakların her biri hakkındaki meta veriler ve fiyatlandırma bilgileriyle birlikte kullanılabilir tüm kaynakları listelemek için kullanabilirsiniz.

Daha fazla bilgi için bkz. [Microsoft Azure kaynak kullanımınızla ilgili içgörüler edinin](../../cost-management-billing/manage/usage-rate-card-overview.md).

### <a name="forecast-cost-with-the-pricing-calculator"></a>Fiyatlandırma hesaplayıcısı ile tahmini maliyet

Azure'daki her hizmetin fiyatlandırması farklıdır. Birçok Azure hizmeti Temel, Standart ve Premium katmanları sağlar. Genellikle, her katmanda birkaç fiyat ve performans düzeyi vardır. [Çevrimiçi fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator)kullanarak, fiyatlandırma tahminleri oluşturabilirsiniz. Hesap makinesi, tek bir kaynak veya bir kaynak grubunun maliyetini tahmin etme esnekliğini içerir.

## <a name="azure-resource-manager"></a>Azure Resource Manager

Azure Kaynak Yöneticisi, Azure kaynakları için bir dağıtım, yönetim ve kuruluş mekanizmasıdır. Kaynak Yöneticisi'ni kullanarak, birçok ayrı kaynağı bir kaynak grubunda bir araya getirebilirsiniz.

Kaynak Yöneticisi, ilgili kaynakların özelleştirilebilir dağıtımına ve yapılandırmasına izin veren dağıtım özelliklerini de içerir. Örneğin, Kaynak Yöneticisi'ni kullanarak, birden çok sanal makine, yük dengeleyicisi ve SQL veritabanından oluşan bir uygulamayı tek bir birim olarak dağıtabilirsiniz. Kaynak Yöneticisi şablonu kullanarak bu dağıtımları geliştirirsiniz.

Resource Manager çeşitli avantajlar sunar:

- Çözümünüzdeki tüm kaynakları ayrı ayrı ele almak yerine bunları grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.

- Çözümünüzü geliştirme yaşam döngüsü boyunca sürekli olarak dağıtabilir ve kaynaklarınızın tutarlı bir durumda dağıtıldıklarından emin olabilirsiniz.

- Altyapınızı betikler yerine bildirim temelli şablonları kullanarak yönetebilirsiniz.

- Doğru sırayla dağıtılmalarını sağlamak için kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

- RBAC yerel olarak yönetim platformuna entegre olduğundan, kaynak grubunuzdaki tüm hizmetlere erişim denetimi uygulayabilirsiniz.

- Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklara etiketler uygulayabilirsiniz.

- Aynı etiketi paylaşan bir kaynak grubunun maliyetlerini görüntüleyerek kuruluşunuzun faturalandırmasını netleştirebilirsiniz.

### <a name="tips-for-creating-resource-groups"></a>Kaynak grupları oluşturmak için ipuçları

Kaynak gruplarınızla ilgili kararlar alırken şu ipuçlarını göz önünde bulundurun:

- Kaynak grubundaki tüm kaynaklar aynı yaşam döngüsüne sahip olmalıdır.

- Bir kaynağı aynı anda yalnızca bir gruba atayabilirsiniz.

- İstediğiniz zaman kaynak grubundan kaynak ekleyebilir veya kaldırabilirsiniz. Her kaynak bir kaynak grubuna ait olmalıdır. Bu nedenle, bir kaynağı bir gruptan kaldırırsanız, başka bir gruba eklemeniz gerekir.

- Çoğu kaynak türünü istediğiniz zaman farklı bir kaynak grubuna taşıyabilirsiniz.

- Kaynak grubundaki kaynaklar farklı bölgelerde olabilir.

- Kaynak grubunu, kaynakların erişimini denetlemek için kullanabilirsiniz.

### <a name="building-resource-manager-templates"></a>Kaynak Yöneticisi şablonları oluşturma

Kaynak Yöneticisi şablonları, tek bir kaynak grubuna dağıtılacak kaynakları ve kaynak yapılandırmalarını bildirimsel olarak tanımlar. Fazla komut dosyası veya el ile yapılandırmaya gerek kalmadan karmaşık dağıtımları düzenlemek için Kaynak Yöneticisi şablonlarını kullanabilirsiniz. Bir şablon geliştirdikten sonra, şablonu her seferinde aynı sonuçla birden çok kez dağıtabilirsiniz.

Kaynak Yöneticisi şablonu dört bölümden oluşur:

- **Parametreler**: Bunlar dağıtıma girişlerdir. Parametre değerleri bir insan veya otomatik bir işlem tarafından sağlanabilir. Örnek bir parametre, Windows VM için yönetici kullanıcı adı ve parola olabilir. Parametre değerleri, dağıtış boyunca belirtildiğinde kullanılır.

- **Değişkenler**: Bunlar dağıtım boyunca kullanılan değerleri tutmak için kullanılır. Parametrelerden farklı olarak, dağıtım zamanında değişken bir değer sağlanmaz. Bunun yerine, sabit kodlanmış veya dinamik olarak oluşturulur.

- **Kaynaklar**: Şablonun bu bölümü, sanal makineler, depolama hesapları ve sanal ağlar gibi dağıtılacak kaynakları tanımlar.

- **Çıktı**: Dağıtım tamamlandıktan sonra, Kaynak Yöneticisi dinamik olarak oluşturulan bağlantı dizeleri gibi verileri döndürebilir.

Dağıtım otomasyonu için aşağıdaki mekanizmalar kullanılabilir:

- **Fonksiyonlar**: Kaynak Yöneticisi şablonlarında çeşitli işlevler kullanabilirsiniz. Bunlar arasında dizeyi küçük harfe dönüştürme, tanımlı bir kaynağın birden çok örneğini dağıtma ve hedef kaynak grubunu dinamik olarak döndürme gibi işlemler yer alır. Kaynak Yöneticisi işlevleri dinamik dağıtımlar oluşturmaya yardımcı olur.

- **Kaynak bağımlılıkları**: Birden çok kaynak dağıtırken, bazı kaynakların diğerlerine bağımlılığı olacaktır. Dağıtımı kolaylaştırmak için, bağımlı kaynakların diğerlerinden önce dağıtılması için bağımlılık bildirimi kullanabilirsiniz.

- **Şablon bağlama**: Bir Kaynak Yöneticisi şablonu içinden başka bir şablona bağlanabilirsiniz. Bu, hedeflenen, amaca özel şablonlar kümesine dağıtım ayrıştırma sağlar.

Kaynak Yöneticisi şablonlarını herhangi bir metin düzenleyicisinde oluşturabilirsiniz. Ancak Visual Studio için Azure SDK size yardımcı olacak araçlar içerir. Visual Studio'yu kullanarak, şablona bir sihirbaz aracılığıyla kaynak ekleyebilir, ardından şablonu doğrudan Visual Studio'nun içinden dağıtabilir ve hata ayıklayabilirsiniz. Daha fazla bilgi için bkz: [Azure Kaynak Yöneticisi şablonları yazar.](../../resource-group-authoring-templates.md)

Son olarak, varolan kaynak gruplarını Azure portalından yeniden kullanılabilir bir şablona dönüştürebilirsiniz. Bu, varolan bir kaynak grubunun dağıtılabilir bir şablonu oluşturmak veya sadece temel deki JSON'u incelemek istiyorsanız yararlı olabilir. Kaynak grubu dışa aktarmak için kaynak grubunun ayarlarından **Otomasyon Komut Dosyası** düğmesini seçin.

## <a name="security-of-azure-resources-rbac"></a>Azure kaynaklarının güvenliği (RBAC)

Abonelik, kaynak grubu veya tek tek kaynak gibi belirli bir kapsamda ki kullanıcı hesaplarına operasyonel erişim izni verebilirsiniz. Bu, sanal makine ve ilgili tüm kaynaklar gibi bir kaynak grubuna bir kaynak kümesi dağıtabileceğiniz ve belirli bir kullanıcıya veya gruba izin verebileceğiniz anlamına gelir. Bu yaklaşım, yalnızca hedef kaynak grubuna ait kaynaklara erişimi sınırlar. Sanal makine veya sanal ağ gibi tek bir kaynağa erişim izni de verebilirsiniz.

Erişim sağlamak için, kullanıcıya veya kullanıcı grubuna bir rol atarsınız. Önceden tanımlanmış birçok rol vardır. Kendi özel rollerinizi de tanımlayabilirsiniz.

[Azure'da yerleşik](../../role-based-access-control/built-in-roles.md)birkaç rol şunlardır:

- **Sahibi**: Bu role sahip bir kullanıcı erişim de dahil olmak üzere her şeyi yönetebilir.

- **Okuyucu**: Bu role sahip bir kullanıcı tüm türdeki kaynakları (sırlar hariç) okuyabilir, ancak değişiklik yapamaz.

- **Sanal Makine Katılımcısı**: Bu role sahip bir kullanıcı sanal makineleri yönetebilir, ancak bağlı oldukları sanal ağı veya VHD dosyasının bulunduğu depolama hesabını yönetemez.

- **SQL DB Katılımcısı**: Bu role sahip bir kullanıcı SQL veritabanlarını yönetebilir, ancak güvenlikle ilgili ilkelerini yönetemez.

- **SQL Security Manager**: Bu role sahip bir kullanıcı, SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetebilir.

- **Depolama Hesabı Katılımcısı**: Bu role sahip bir kullanıcı depolama hesaplarını yönetebilir ancak depolama hesaplarına erişimi yönetemez.

Daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).

## <a name="azure-virtual-machines"></a>Azure Sanal Makineler

Azure Sanal Makineler, Azure'daki merkezi IaaS hizmetlerinden biridir. Azure Sanal Makineler, Windows veya Linux sanal makinelerinin Microsoft Azure veri merkezinde dağıtımını destekler. Azure Sanal Makineler ile VM yapılandırması üzerinde tam denetime sahipsiniz ve tüm yazılım yükleme, yapılandırma ve bakımdan sorumlusunuz.

Bir Azure VM dağıtırken, Azure Marketi'nden bir resim seçebilir veya genelleştirilmiş bir görüntü sağlayabilirsiniz. Bu görüntü işletim sistemi ve ilk yapılandırma uygulamak için kullanılır. Dağıtım sırasında Kaynak Yöneticisi, bilgisayar adını, yönetim kimlik bilgilerini ve ağ yapılandırmasını atama gibi bazı yapılandırma ayarlarını işler. Yazılım yükleme, virüsten koruma yapılandırması ve izleme çözümleri gibi yapılandırmaları daha fazla otomatikleştirmek için Azure sanal makine uzantılarını kullanabilirsiniz.

Birçok farklı boyutlarda sanal makineler oluşturabilirsiniz. Sanal makinenin boyutu işleme, bellek ve depolama kapasitesi gibi kaynak ayırmayı belirler. Bazı durumlarda, RDMA özellikli ağ bağdaştırıcıları ve SSD diskler gibi belirli özellikler yalnızca belirli VM boyutlarında kullanılabilir. VM boyutları nın ve özelliklerinin tam listesi için [Windows](../../virtual-machines/windows/sizes.md) ve [Linux](../../virtual-machines/linux/sizes.md)için "Azure'daki sanal makineler için boyutlar" bölümüne bakın.

### <a name="use-cases"></a>Uygulama alanları

Azure sanal makineleri yapılandırma üzerinde tam denetim sunduğundan, PaaS modeline sığmayan çok çeşitli sunucu iş yükleri için idealdir. Veritabanı sunucuları (SQL Server, Oracle veya MongoDB), Windows Server Active Directory, Microsoft SharePoint ve daha birçok sunucu iş yükleri Microsoft Azure platformunda çalıştırmak mümkün olur. İstenirseniz, bu tür iş yüklerini şirket içi bir veri merkezinden büyük miktarda yeniden yapılandırma olmadan bir veya daha fazla Azure bölgesine taşıyabilirsiniz.

### <a name="deployment-of-virtual-machines"></a>Sanal makinelerin dağıtımı

Azure sanal makinelerini Azure portalını kullanarak, Azure PowerShell modülüyle otomasyon uyarak veya çapraz platform CLI ile otomasyon kullanarak dağıtabilirsiniz.

#### <a name="portal"></a>Portal

Azure portalını kullanarak sanal bir makinedağıtmak yalnızca etkin bir Azure aboneliği ve bir web tarayıcısına erişim gerektirir. Farklı yapılandırmalara sahip birçok farklı işletim sistemi görüntüsü seçebilirsiniz. Tüm depolama ve ağ gereksinimleri dağıtım sırasında yapılandırılır. Daha fazla bilgi için [Windows](../../virtual-machines/windows/quick-create-portal.md) ve [Linux](../../virtual-machines/linux/quick-create-portal.md)için "Azure portalında sanal bir makine oluşturun" bilgisine bakın.

Azure portalından sanal bir makine dağıtmanın yanı sıra, portaldan bir Azure Kaynak Yöneticisi şablonu da dağıtabilirsiniz. Bu, şablonda tanımlandığı şekilde tüm kaynakları dağıtacak ve yapılandıracaktır. Daha fazla bilgi için [Kaynak Yöneticisi şablonları ve Azure portalı ile kaynakları dağıt'a](../../azure-resource-manager/templates/deploy-portal.md)bakın.

#### <a name="powershell"></a>PowerShell

PowerShell'i kullanarak bir Azure sanal makinedağıtmak, depolama ve ağ dahil olmak üzere ilgili tüm sanal makine kaynaklarının tam dağıtım otomasyonuna olanak tanır. Daha fazla bilgi için Kaynak [Yöneticisi ve PowerShell'i kullanarak Windows VM oluştur'a](../../virtual-machines/windows/quick-create-powershell.md)bakın.

Azure bilgi işlem kaynaklarını tek tek dağıtmanın yanı sıra, Azure Kaynak Yöneticisi şablonu dağıtmak için Azure PowerShell modüllerini kullanabilirsiniz. Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure PowerShell ile kaynakları dağıt'a](../../azure-resource-manager/templates/deploy-powershell.md)bakın.

#### <a name="command-line-interface-cli"></a>Komut satırı arabirimi (CLI)

PowerShell modülünde olduğu gibi, Azure komut satırı arabirimi dağıtım otomasyonu sağlar ve Windows, OS X veya Linux sistemlerinde kullanılabilir. Azure CLI **vm hızlı oluşturma** komutunu kullanırken, ilgili tüm sanal makine kaynakları (depolama ve ağ dahil) ve sanal makinenin kendisi dağıtılır. Daha fazla bilgi için [CLI'yi kullanarak Azure'da Linux VM oluştur'a](../../virtual-machines/linux/quick-create-cli.md)bakın.

Aynı şekilde, Azure Kaynak Yöneticisi şablonu dağıtmak için Azure CLI'yi kullanabilirsiniz. Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure CLI ile kaynakları dağıt'a](../../azure-resource-manager/templates/deploy-cli.md)bakın.

### <a name="access-and-security-for-virtual-machines"></a>Sanal makineler için erişim ve güvenlik

Sanal bir makineye Internet'ten erişmek, ilişkili ağ arabiriminin veya varsa yük dengeleyicisinin ortak bir IP adresiyle yapılandırılmasını gerektirir. Ortak IP adresi, sanal makineveya yük dengeleyicisine çözülecek bir DNS adı içerir. Daha fazla bilgi için [Azure'daki IP adreslerine](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)bakın.

Bir ağ güvenlik grubu (NSG) kaynağı kullanarak ortak IP adresi üzerinden sanal makineye erişimi yönetirsiniz. NSG bir güvenlik duvarı gibi davranır ve ağ arabirimi veya alt ağ üzerinden tanımlanan bağlantı noktaları kümesindeki trafiğe izin verir veya reddeder. Örneğin, Azure VM'li uzak masaüstü oturumu oluşturmak için, 3389 bağlantı noktasında gelen trafiğe izin verecek şekilde NSG'yi yapılandırmanız gerekir. Daha fazla bilgi için Bkz. [Azure portalını kullanarak Azure'da bir VM bağlantı noktalarını açma.](../../virtual-machines/windows/nsg-quickstart-portal.md)

Son olarak, herhangi bir bilgisayar sisteminin yönetiminde olduğu gibi, güvenlik kimlik bilgilerini ve yazılım güvenlik duvarlarını kullanarak işletim sistemindeki bir Azure sanal makinesinin güvenliğini sağlamanız gerekir.

## <a name="azure-storage"></a>Azure Storage

Azure Depolama, dayanıklı, ölçeklenebilir ve gereksiz depolama sağlayan Microsoft tarafından yönetilen bir hizmettir. Herhangi bir kaynak dağıtım yöntemini kullanarak herhangi bir kaynak grubuna kaynak olarak bir Azure depolama hesabı ekleyebilirsiniz. Azure dört depolama türü içerir: Blob depolama, Dosya Depolama, Tablo depolama ve Sıra depolama. Bir depolama hesabı dağıtılırken, genel amaçlı ve blob depolama olmak üzere iki hesap türü kullanılabilir. Genel amaçlı bir depolama hesabı, dört depolama türüne de erişmenizi sağlar. Blob depolama hesapları genel amaçlı hesaplara benzer, ancak sıcak ve soğuk erişim katmanları içeren özel lekeler içerir. Blob depolama hakkında daha fazla bilgi için [Azure Blob depolama alanına](../../storage/blobs/storage-blob-storage-tiers.md)bakın.

Azure depolama hesapları farklı artıklık düzeyleriyle yapılandırılabilir:

- **Yerel olarak yedekli depolama,** bir yazma başarılı kabul edilmeden önce tüm verilerin üç kopyasının eşzamanlı olarak yapılmasını sağlayarak yüksek kullanılabilirlik sağlar. Bu kopyalar tek bir bölgedeki tek bir tesiste saklanır. Yinelemeler ayrı hata etki alanlarında ve yükseltme etki alanlarında yer alıyor. Bu, verilerinizi tutan bir depolama düğümü başarısız olsa veya güncelleştirilecek şekilde çevrimdışı olsa bile verilerin kullanılabilir olduğu anlamına gelir.

- **Coğrafi yedekli depolama,** yüksek kullanılabilirlik için birincil bölgedeki verilerin üç eşzamanlı kopyasını yapar ve ardından eş zamanlı olarak olağanüstü durum kurtarma için eşleştirilmiş bir bölgede üç yineleme yapar.

- **Okuma-erişim coğrafi yedekli depolama** jeo-yedekli depolama artı ikincil bölgedeki verileri okuma yeteneğidir. Bu yetenek kısmi felaket kurtarma için uygun hale getirir. Birincil bölgeyle ilgili bir sorun varsa, uygulamanızı eşleştirilmiş bölgeye salt okunur erişim ene sahip olacak şekilde değiştirebilirsiniz.

### <a name="use-cases"></a>Uygulama alanları

Her depolama türü farklı bir kullanım örneği vardır.

#### <a name="blob-storage"></a>Blob depolama

*Blob* *sözcüğü, ikili büyük nesnenin*kısaltmasıdır. Blobs, bilgisayarınızda depoladığınız dosyalar gibi yapılandırılmamış dosyalardır. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır. Azure Blob depolama da Azure Sanal Makineler veri diskleri tutar.

Azure Depolama üç blob türünü destekler:

- **Blok lekelerboyutu** (4 MB × 50.000 blok) boyutu (4 MB × 50.000 blok) 195 GB'a kadar sıradan dosyaları tutmak için kullanılır. Blok bloblarının birincil kullanım örneği, medya dosyaları veya web siteleri için görüntü dosyaları gibi baştan sona okunan dosyaların depolanmasıdır. 64 MB'dan büyük dosyaların küçük bloklar olarak yüklenmesi gerektiğinden, bunlarblok blobs olarak adlandırılır. Bu bloklar daha sonra son blob içine konsolide (veya taahhüt) leştirilir.

- **Sayfa lekeleri,** rasgele erişilen dosyaları 1 TB boyutuna kadar tutmak için kullanılır. Sayfa lekeleri, azure'daki IaaS bilgi işlem hizmeti olan Azure Sanal Makineleri için dayanıklı diskler sağlayan VHD'ler için öncelikle destek depolama alanı olarak kullanılır. 512 baytlık sayfalara rastgele okuma/yazma erişimi sağladıklarından sayfa blobu olarak adlandırılır.

- **Ek blobs** blok blobs gibi bloklar oluşur, ancak ek işlemleri için optimize edilmiştir. Bunlar genellikle bir veya daha fazla kaynaktan aynı blob bilgileri günlüğe kaydetme için kullanılır. Örneğin, birden çok VM'de çalışan bir uygulama için tüm izleme günleme gününüzü aynı ek blob'a yazabilirsiniz. Tek bir ekleme blobu en fazla 195 GB olabilir.

Daha fazla bilgi için [.NET'i kullanarak Azure Blob depolama alanına başlayın'a](../../storage/blobs/storage-dotnet-how-to-use-blobs.md)bakın.

#### <a name="file-storage"></a>Dosya depolama

Azure Dosya depolama, standart Sunucu İleti Bloğu (SMB) protokolünü kullanarak bulutta dosya paylaşımları sunan bir hizmettir. Hizmet hem SMB 2.1 hem de SMB 3.0'ı destekler. Azure Dosya depolama ile, dosya paylaşımlarına dayanan uygulamaları hızlı ve maliyetli yeniden yazmadan Azure'a geçirebilirsiniz. Azure sanal makinelerinde, bulut hizmetlerinde veya şirket içi istemcilerden çalışan uygulamalar buluta dosya paylaşımı ekleyebilir. Bu, bir masaüstü uygulamasının tipik bir SMB payını nasıl birşekilde monte ettiğine benzer. Ardından herhangi sayıda uygulama bileşeni eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

Dosya depolama paylaşımı standart bir Kobİ dosya paylaşımı olduğundan, Azure'da çalışan uygulamalar dosya sistemi G/Ç API'leri aracılığıyla paylaşımdaki verilere erişebilir. Bu nedenle geliştiriciler varolan uygulamaları geçirmek için varolan kod ve becerilerini kullanabilirler. BT uzmanları, Azure uygulamalarının yönetiminin bir parçası olarak Dosya depolama paylaşımlarını oluşturmak, monte etmek ve yönetmek için PowerShell cmdlets'i kullanabilir.

Daha fazla bilgi için [Windows'daki Azure Dosyası depolama alanına başlayın](../../storage/files/storage-how-to-use-files-windows.md) veya Linux ile Azure Dosyası [depolamasını nasıl](../../storage/files/storage-how-to-use-files-linux.md)kullanacağız'a bakın.

#### <a name="table-storage"></a>Table Storage

Azure Table Storage, bulutta yapılandırılmış NoSQL verileri depolayan bir hizmettir. Tablo depolama, şemasız tasarıma sahip bir anahtar/öznitelik deposudur. Tablo depolama şemasız olduğundan, uygulamanızın gereksinimleri geliştikçe verilerinizi uyarlamak kolaydır. Her türlü uygulama için verilere erişim hızlı ve uygun maliyetlidir. Table Storage, benzer hacimdeki veriler için geleneksel SQL’e oranla çok daha düşük maliyetlidir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri ve hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini depolamak üzere Table Storage’ı kullanabilirsiniz. Bir tabloda istediğiniz sayıda varlığı depolayabilirsiniz. Depolama hesabı, depolama hesabının kapasite sınırına kadar herhangi bir sayıda tablo içerebilir.

Daha fazla bilgi için bkz: [Azure Tablo depolama alanına başlayın.](../../cosmos-db/table-storage-how-to-use-dotnet.md)

#### <a name="queue-storage"></a>Kuyruk depolama

Azure Queue depolama birimi, uygulama bileşenleri arasında bulut mesajlaşma özelliği sağlar. Ölçek uygulamaları tasarlanırken, uygulama bileşenleri genellikle bağımsız ölçeklendirebilmeleri için birbirinden ayrıştırılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki iletişim için zaman uyumsuz mesajlaşma sunar. Kuyruk depolama zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını da destekler.

Daha fazla bilgi için bkz: [Azure Kuyruk depolama alanına başlayın.](../../storage/queues/storage-dotnet-how-to-use-queues.md)

### <a name="deploying-a-storage-account"></a>Depolama hesabı dağıtma

Depolama hesabı dağıtmak için çeşitli seçenekler vardır.

#### <a name="portal"></a>Portal

Azure portalını kullanarak bir depolama hesabı dağıtmak yalnızca etkin bir Azure aboneliği ve bir web tarayıcısına erişim gerektirir. Yeni bir depolama hesabını yeni veya varolan bir kaynak grubuna dağıtabilirsiniz. Depolama hesabını oluşturduktan sonra, portalı kullanarak bir blob kapsayıcısı veya dosya paylaşımı oluşturabilirsiniz. Tablo ve Sıra depolama varlıkları katmanlı olarak oluşturabilirsiniz. Daha fazla bilgi için bkz. [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md).

Azure portalından bir depolama hesabı dağıtmanın yanı sıra, portaldan bir Azure Kaynak Yöneticisi şablonu da dağıtabilirsiniz. Bu, herhangi bir depolama hesapları da dahil olmak üzere şablonda tanımlanan tüm kaynakları dağıtacak ve yapılandıracaktır. Daha fazla bilgi için [Kaynak Yöneticisi şablonları ve Azure portalı ile kaynakları dağıt'a](../../azure-resource-manager/templates/deploy-portal.md)bakın.

#### <a name="powershell"></a>PowerShell

PowerShell'i kullanarak bir Azure depolama hesabı dağıtmak, depolama hesabının tam dağıtım otomasyonuna olanak tanır. Daha fazla bilgi için bkz: [Azure Depolama ile Azure PowerShell'i kullanma.](../../storage/common/storage-powershell-guide-full.md)

Azure kaynaklarını tek tek dağıtmanın yanı sıra, Azure Kaynak Yöneticisi şablonu dağıtmak için Azure PowerShell modüllerini kullanabilirsiniz. Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure PowerShell ile kaynakları dağıt'a](../../azure-resource-manager/templates/deploy-powershell.md)bakın.

#### <a name="command-line-interface-cli"></a>Komut satırı arabirimi (CLI)

PowerShell modülünde olduğu gibi, Azure komut satırı Arabirimi dağıtım otomasyonu sağlar ve Windows, OS X veya Linux sistemlerinde kullanılabilir. Bir depolama hesabı oluşturmak için Azure CLI **depolama hesabı oluşturma** komutunu kullanabilirsiniz. Daha fazla bilgi için [bkz.](../../storage/common/storage-azure-cli.md)

Aynı şekilde, Azure Kaynak Yöneticisi şablonu dağıtmak için Azure CLI'yi kullanabilirsiniz. Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure CLI ile kaynakları dağıt'a](../../resource-group-template-deploy-cli.md)bakın.

### <a name="access-and-security-for-azure-storage"></a>Azure Depolama için erişim ve güvenlik

Azure Depolama'ya, Azure portalı, VM oluşturma ve çalışma sırasında ve Depolama istemci kitaplıkları dahil olmak üzere çeşitli şekillerde erişilir.

#### <a name="virtual-machine-disks"></a>Sanal makine diskleri

Sanal bir makine dağıtırken, sanal makine işletim sistemi diskini ve ek veri disklerini tutmak için bir depolama hesabı oluşturmanız gerekir. Varolan bir depolama hesabı seçebilir veya yeni bir depo oluşturabilirsiniz. Bir blob maksimum boyutu 1.024 GB olduğundan, tek bir VM disk 1.023 GB maksimum boyutu vardır. Daha büyük bir veri diskini yapılandırmak için, sanal makineye birden çok veri diski sunabilir ve bunları tek bir mantıksal disk olarak bir araya getirebilirsiniz. Daha fazla bilgi için [Windows](../../virtual-machines/windows/tutorial-manage-data-disk.md) ve [Linux](../../virtual-machines/linux/tutorial-manage-disks.md)için "Azure disklerini Yönet" başlıklı bilgi için.

#### <a name="storage-tools"></a>Depolama araçları

Azure depolama hesaplarına Visual Studio Cloud Explorer gibi birçok farklı depolama gezgini aracılığıyla erişilebilir. Bu araçlar, depolama hesaplarına ve verilere göz atmanıza izin sağlar. Daha fazla bilgi ve kullanılabilir depolama kaşiflerinin listesi için [Azure Depolama istemcisi araçlarına](../../storage/common/storage-explorers.md)bakın.

#### <a name="storage-api"></a>Depolama API'si

Depolama kaynaklarına HTTP/HTTPS isteklerini gerçekleştirebilecek herhangi bir dil erişebilir. Ayrıca, Azure Storage birkaç popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar, senkron ve eşzamanlı çağırma, işlemlerin toplu kullanımı, özel durum yönetimi ve otomatik yeniden denemeler gibi ayrıntıları işleyerek Azure Depolama ile çalışmayı kolaylaştırır. Daha fazla bilgi için Azure [Depolama hizmeti REST API başvurusuna](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference)bakın.

#### <a name="storage-access-keys"></a>Depolama erişim anahtarları

Her depolama hesabının iki kimlik doğrulama anahtarı vardır, bir birincil ve ikincil. Her ikisi de depolama erişim işlemleri için kullanılabilir. Bu depolama anahtarları, bir depolama hesabının güvenliğini sağlamaya yardımcı olmak için kullanılır ve verilere programlı olarak erişmek için gereklidir. Güvenliği artırmak için tuşların ara sıra devrine izin veren iki anahtar vardır. Anahtarların güvenliğini sağlamak çok önemlidir, çünkü sahip oldukları, hesap adı ile birlikte, depolama hesabındaki herhangi bir veriye sınırsız erişim eve erişime olanak sağlar.

#### <a name="shared-access-signatures"></a>Paylaşılan erişim imzaları

Kullanıcıların depolama kaynaklarınıza kontrollü erişim ebilmelerine izin vermek istiyorsanız, paylaşılan bir erişim imzası oluşturabilirsiniz. Paylaşılan erişim imzası, depolama kaynağına devredilen erişimi sağlayan bir URL'ye eklenebilen bir belirteçtir. Belirteci sahip olan herkes, işaret ettiği kaynağa, geçerli olduğu süre boyunca, belirttiği izinlerle erişebilir. Daha fazla bilgi için [bkz.](../../storage/common/storage-dotnet-shared-access-signature-part-1.md)

## <a name="azure-virtual-network"></a>Azure Sanal Ağ

Sanal ağlar sanal makineler arasındaki iletişimi desteklemek için gereklidir. Alt ağlar, özel IP adresi, DNS ayarları, güvenlik filtreleme ve yük dengeleme tanımlayabilirsiniz. Azure, yalnızca bulut ağları veya karma sanal ağlar gibi farklı kullanım durumlarını destekler.

### <a name="cloud-only-virtual-networks"></a>Yalnızca bulutsanal ağlar

Varsayılan olarak Azure sanal ağına yalnızca Azure'da depolanan kaynaklar erişebilir. Aynı sanal ağa bağlı kaynaklar birbirleriyle iletişim kurabilir. Sanal makinenin Internet üzerinden erişilebilir olmasını sağlamak için sanal makine ağ arabirimlerini ve yük dengeleyicilerini genel bir IP adresiyle ilişkilendirebilirsiniz. Bir ağ güvenlik grubu kullanarak, genel olarak açığa çıkan kaynaklara erişimin güvenli hale kullanılmasına yardımcı olabilirsiniz.

![2 katmanlı bir Web Uygulaması için Azure Sanal Ağı](https://docs.microsoft.com/azure/load-balancer/media/load-balancer-internal-overview/ic744147.png)

### <a name="hybrid-virtual-networks"></a>Karma sanal ağlar

ExpressRoute'u veya siteden siteye VPN bağlantısını kullanarak şirket içi ağı Azure sanal ağına bağlayabilirsiniz. Bu yapılandırmada, Azure sanal ağı aslında şirket içi ağınızın bulut tabanlı bir uzantısıdır.

Azure sanal ağı şirket içi ağınıza bağlı olduğundan, tesisler arası sanal ağlar kuruluşunuzun kullandığı adres alanının benzersiz bir bölümünü kullanmalıdır. Farklı kurumsal konumlara belirli bir IP alt ağı atanması gibi, Azure ağınızı genişlettikçe başka bir konum haline gelir.
Sanal ağ dağıtmak için çeşitli seçenekler vardır.

- [Portal](../..//virtual-network/quick-create-portal.md)

- [Powershell](../../virtual-network/quick-create-powershell.md)

- [Komut Satırı Arabirimi (CLI)](../../virtual-network/quick-create-cli.md)

- Azure Kaynak Yöneticisi Şablonları

> **Ne zaman kullanılır**: Azure'da SANAL M'lerle çalışırken sanal ağlarla çalışırsınız. Bu, Sanal M'lerinizi kamuya açık ve özel alt ağlara benzer şirket içi veri merkezlerine segmente etme olanağı sağlar.
>
> **Başlayın**: Azure portalını kullanarak bir Azure sanal ağı dağıtmak yalnızca etkin bir Azure aboneliği ve bir web tarayıcısına erişim gerektirir. Yeni bir sanal ağı yeni veya varolan bir kaynak grubuna dağıtabilirsiniz. Portaldan yeni bir sanal makine oluştururken, varolan bir sanal ağı seçebilir veya yeni bir ağ oluşturabilirsiniz. Azure portalını kullanarak başlatın ve [sanal ağ oluşturun.](../../virtual-network/quick-create-portal.md)

### <a name="access-and-security-for-virtual-networks"></a>Sanal ağlar için erişim ve güvenlik

Bir ağ güvenlik grubu kullanarak Azure sanal ağlarının güvenliğini sağlamaya yardımcı olabilirsiniz. NSG'ler, sanal ağdaki VM örneklerinize ağ trafiğine izin veren veya reddeden bir erişim denetim listesi (ACL) kuralları içerir. NSG'leri bu alt ağ daki alt ağlarla veya tek tek VM örnekleriyle ilişkilendirebilirsiniz. Bir NSG'yi bir alt ağla ilişkilendirdiğinizde, ACL kuralları o alt ağdaki tüm VM örnekleri için geçerlidir. Buna ek olarak, bir NSG'yi doğrudan bu VM ile ilişkilendirerek trafiği tek bir VM ile daha da kısıtlayabilirsiniz. Daha fazla bilgi için bkz. [Ağ güvenlik grupları ile ağ trafiğini filtreleme](../../virtual-network/security-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Windows VM oluşturma](../../virtual-machines/windows/quick-create-portal.md)
- [Linux VM oluşturma](../../virtual-machines/linux/quick-create-portal.md)
