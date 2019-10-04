---
title: Azure Data Factory giriş | Microsoft Docs
description: Veri taşıma ve dönüştürme işlemlerini düzenleyen ve otomatikleştiren bir bulut veri tümleştirme hizmeti olan Azure Data Factory hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: overview
ms.date: 09/30/2019
ms.openlocfilehash: 7bc03e80fc49756d19677edbef6bd8d372849732
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71937248"
---
# <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir?

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-introduction.md)
> * [Güncel sürüm](introduction.md)

Büyük veri dünyasında, ham, düzensiz veriler genellikle ilişkisel, ilişkisel olmayan ve diğer depolama sistemlerinde depolanır. Ancak, kendi kendine ham verileri analist, veri bilimcileri veya iş karar mekanizmalarının anlamlı içgörüleri sağlamak için uygun bağlam veya anlamı yoktur. 

Büyük veriler, ham verilerin bu zenginleştirme mağazalarını uygulanabilir iş içgörülerine dönüştürmek üzere işlemleri düzenleyebilen ve işletirebilecek bir hizmet gerektirir. Azure Data Factory, bu karmaşık karma Ayıkla-Dönüştür-yükle (ETL), ayıkla-yükle-Dönüştür (ELT) ve veri tümleştirme projeleri için oluşturulmuş, yönetilen bir bulut hizmetidir.

Örneğin, bulutta Oyunlar tarafından üretilen oyun günlüklerinin petabaytlarca toplayan bir oyun şirketi düşünün. Şirket, müşteri tercihleri, demografik ve kullanım davranışına yönelik Öngörüler elde etmek için bu günlükleri çözümlemek istiyor. Ayrıca, ticari satış ve çapraz satış fırsatlarını belirlemek, etkileyici yeni özellikler geliştirmek, işletmenizin büyümesini sağlamak ve müşterilerine daha iyi bir deneyim sunmak istemektedir.

Bu günlükleri analiz etmek için, şirketin şirket içi veri deposunda bulunan müşteri bilgileri, Oyun bilgileri ve pazarlama kampanyası bilgileri gibi başvuru verilerini kullanması gerekir. Şirket, şirket içi veri deposundaki bu verileri kullanarak bulut veri deposunda sahip olduğu ek günlük verileriyle birleştirerek bu verileri kullanmak istiyor. 

Öngörüleri ayıklamak için, bulutta (Azure HDInsight) bir Spark kümesi kullanarak birleştirilmiş verileri işlemeyi ve bu verilerin üzerine kolayca bir rapor oluşturmak için dönüştürülmüş verileri Azure SQL veri ambarı gibi bir bulut veri ambarında yayımlamayı planlamaktadır. Bu iş akışını otomatik hale getirmek ve günlük bir zamanlamaya göre izlemek ve yönetmek istiyorlar. Bunlar ayrıca dosyalar bir Blob deposu kapsayıcısına geldiğinde yürütmek ister.

Azure Data Factory, bu tür veri senaryolarını çözen platformudur. *Veri taşıma ve veri dönüştürmeyi düzenlemek ve otomatikleştirmek için bulutta veri odaklı iş akışları oluşturmanıza olanak tanıyan, bulut tabanlı bir veri tümleştirme hizmetidir*. Azure Data Factory kullanarak, farklı veri depolarından veri alabilen veri odaklı iş akışları (işlem hatları olarak adlandırılır) oluşturabilir ve zamanlayabilirsiniz. Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics ve Azure Machine Learning gibi işlem hizmetlerini kullanarak verileri işleyebilir ve dönüştürebilir. 

Ayrıca, iş zekası (BI) uygulamalarının kullanması için Azure SQL veri ambarı gibi veri depolarında çıkış verileri yayımlayabilirsiniz. Sonuçta Azure Data Factory, ham veriler, daha iyi iş kararları vermek için anlamlı veri depoları ve veri Lakes halinde düzenlenebilir.

![Data Factory üst düzey görünümü](media/introduction/big-picture.png)

## <a name="how-does-it-work"></a>Nasıl çalışır?
Azure Data Factory işlem hatları (veri odaklı iş akışları) genellikle aşağıdaki dört adımı gerçekleştirir:

![Veri odaklı bir iş akışının dört adımı](media/introduction/four-steps-of-a-workflow.png)

### <a name="connect-and-collect"></a>Bağlanma ve toplama

Kuruluşlar şirket içinde, bulutta, yapılandırılmış, yapılandırılmamış ve yarı yapılandırılmış, hepsi farklı aralıklarda ve hızlarda gelen farklı kaynaklarda bulunan çeşitli türlerdeki verilere sahiptir. 

Bilgi üretim sistemi oluşturmanın ilk adımı, hizmet olarak yazılım (SaaS) Hizmetleri, veritabanları, dosya paylaşımları ve FTP Web Hizmetleri gibi tüm gerekli veri ve işleme kaynaklarına bağlanmaktadır. Sonraki adım, verileri gerektiğinde izleyen işleme için merkezi bir konuma taşımadır.

Data Factory olmadan, kuruluşların bu veri kaynaklarını ve işlemeyi bütünleştirmek için özel veri taşıma bileşenleri oluşturması veya özel hizmetler yazması gerekir. Bu tür sistemleri tümleştirme ve bakımını yapmak pahalı ve zordur. Bunlara ek olarak, genellikle tam olarak yönetilen bir hizmetin sunduğu kurumsal sınıf izleme, uyarı ve denetimlerin bulunmaması da olur.

Data Factory, veri işlem hattındaki [kopyalama etkinliğini](copy-activity-overview.md) kullanarak daha fazla analiz için hem şirket içi hem de bulut kaynak veri mağazalarından verileri bulutta bir merkezileşmeyi veri deposuna taşıyabilirsiniz. Örneğin, Azure Data Lake Storage verileri toplayabilir ve Azure Data Lake Analytics bir işlem hizmeti kullanarak verileri daha sonra dönüştürebilirsiniz. Ayrıca, verileri Azure Blob depolama alanında toplayıp daha sonra bir Azure HDInsight Hadoop kümesi kullanarak dönüştürebilirsiniz.

### <a name="transform-and-enrich"></a>Dönüştürme ve zenginleştirme
Veriler buluttaki merkezi bir veri deposunda mevcut olduktan sonra, toplanan verileri HDInsight Hadoop, Spark, Data Lake Analytics ve Machine Learning gibi işlem hizmetlerini kullanarak işleyin veya dönüştürün. Güvenilen verilerle üretim ortamlarının akışını yapmak için, sürdürülebilir ve denetlenen bir zamanlamaya göre dönüştürülmüş verileri güvenilir bir şekilde üretmek istiyorsunuz.

### <a name="publish"></a>yayınlamanız
Ham veriler iş için kullanılabilir bir tüketilebilir biçimde iyileştirildikten sonra, verileri Azure veri ambarı, Azure SQL veritabanı, Azure CosmosDB 'ye veya iş kullanıcılarınızın iş zekası araçlarından işaret edebildiği herhangi bir analiz motoruna yükleyin.

### <a name="monitor"></a>İzle
Veri tümleştirme işlem hattınızı başarıyla oluşturup dağıttıktan sonra, iyileştirilmiş verilerden iş değeri sunarak, başarı ve başarısızlık ücretleri için zamanlanmış etkinlikleri ve işlem hatlarını izleyin. Azure Data Factory, Azure Izleyici, API, PowerShell, Azure Izleyici günlükleri ve Azure portal sistem durumu bölmeleri aracılığıyla işlem hattı izleme için yerleşik desteğe sahiptir.

## <a name="top-level-concepts"></a>Üst düzey kavramlar
Bir Azure aboneliğinde bir veya daha fazla Azure Data Factory örneği (veya Veri Fabrikası) olabilir. Azure Data Factory dört ana bileşenden oluşur. Bu bileşenler, veri taşıma ve dönüştürme adımları ile veri odaklı iş akışları oluşturabileceğiniz platformu sağlamak üzere birlikte çalışır.

### <a name="pipeline"></a>Konfigüre
Veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattı, bir iş birimi oluşturan etkinliklerin mantıksal gruplandırmasıdır. Birlikte, bir işlem hattındaki etkinlikler bir görevi gerçekleştirir. Örneğin, bir işlem hattı Azure blobundan verileri alan ve ardından HDInsight kümesinde verileri bölümlemek için bir Hive sorgusu çalıştıran bir etkinlik grubunu içerebilir. 

Bunun avantajı, işlem hattının her birini ayrı ayrı yönetmek yerine etkinlikleri bir küme olarak yönetmenize olanak tanımasıdır. İşlem hattındaki etkinlikler, sırayla çalışmak üzere birbirine zincirlenebilir veya paralel olarak bağımsız bir şekilde çalışabilir.

### <a name="activity"></a>Etkinlik
Etkinlikler bir işlem hattındaki işleme adımını temsil eder. Örneğin, bir veri deposundan başka bir veri deposuna veri kopyalamak için kopyalama etkinliği kullanabilirsiniz. Benzer şekilde, verilerinizi dönüştürmek veya analiz etmek için bir Azure HDInsight kümesinde Hive sorgusu çalıştıran bir Hive etkinliği kullanabilirsiniz. Data Factory üç tür etkinliği destekler: veri taşıma etkinlikleri, veri dönüştürme etkinlikleri ve denetim etkinlikleri.

### <a name="datasets"></a>Veri kümeleri
Veri kümeleri, Veri depolarındaki veri yapılarını temsil eder. Bu, etkinliklerinizde, giriş veya çıkış olarak kullanmak istediğiniz verilere işaret eden veya başvuruda bulunmak yeterlidir. 

### <a name="linked-services"></a>Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Bu şekilde düşünün: bağlı bir hizmet, veri kaynağıyla bağlantıyı tanımlar ve veri kümesi verilerin yapısını temsil eder. Örneğin, Azure depolama bağlı hizmeti Azure depolama hesabına bağlanmak için bir bağlantı dizesi belirtir. Ayrıca, bir Azure blob veri kümesi blob kapsayıcısını ve verileri içeren klasörü belirtir.

Bağlı hizmetler Data Factory iki amaçla kullanılır:

- Bir şirket içi SQL Server veritabanı, Oracle veritabanı, dosya paylaşma veya Azure Blob depolama hesabı içeren, ancak bunlarla sınırlı olmayan bir **veri deposunu** temsil etmek için. Desteklenen veri depolarının listesi için bkz. [kopyalama etkinliği](copy-activity-overview.md) makalesi.

- Bir etkinliğin yürütülmesini barındırameyen bir **işlem kaynağını** temsil etmek için. Örneğin, Hdınsighthive etkinliği bir HDInsight Hadoop kümesinde çalışır. Dönüştürme etkinlikleri ve desteklenen işlem ortamlarının listesi için [veri dönüştürme](transform-data.md) makalesine bakın.

### <a name="triggers"></a>Tetikleyiciler
Tetikleyiciler, bir işlem hattı yürütmesinin ne zaman bir şekilde ele alınması gerektiğini belirleyen işlem birimini temsil eder. Farklı olay türleri için farklı türde tetikleyiciler vardır.

### <a name="pipeline-runs"></a>İşlem hattı çalıştırmaları
İşlem hattı çalıştırması, işlem hattı yürütmesinin bir örneğidir. İşlem hattı çalıştırmaları genellikle bağımsız değişkenlerin işlem hatlarında tanımlanan parametrelere iletilmesiyle oluşturulur. Bağımsız değişkenler el ile veya tetikleyici tanımı içinde geçirilebilir.

### <a name="parameters"></a>Parametreler
Parametreler, salt okunurdur yapılandırma anahtar-değer çiftleridir.  Parametreler ardışık düzen içinde tanımlanmıştır. Tanımlı parametrelerin bağımsız değişkenleri, bir tetikleyici tarafından oluşturulan çalıştırma bağlamından veya el ile yürütülen bir işlem hattından yürütme sırasında geçirilir. İşlem hattının içindeki etkinlikler parametre değerlerini kullanır.

Veri kümesi türü kesin belirlenmiş bir parametre ve yeniden kullanılabilir/Referenceable bir varlıktır. Bir etkinlik, veri kümelerine başvurabilir ve veri kümesi tanımında tanımlanan özellikleri kullanabilir.

Bağlı hizmet Ayrıca, bir veri deposuna veya bir işlem ortamına yönelik bağlantı bilgilerini içeren, kesin olarak belirlenmiş bir parametredir. Ayrıca yeniden kullanılabilir/Referenceable bir varlıktır.

### <a name="control-flow"></a>Denetim akışı
Denetim akışı, bir sırayla zincirleme etkinlikleri ve işlem hattı düzeyinde parametreleri tanımlamak ve işlem hattını talep üzerine veya bir tetikleyiciden çağırırken bağımsız değişkenleri geçirerek bir işlem hattı etkinlikleri düzenleme işlemi. Ayrıca, özel durum geçirme ve döngü kapsayıcıları, diğer bir deyişle,-her yineleyiciler da içerir.


Data Factory kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Veri kümesi ve bağlı hizmetler](concepts-datasets-linked-services.md)
- [İşlem hatları ve Etkinlikler](concepts-pipelines-activities.md)
- [Tümleştirme çalışma zamanı](concepts-integration-runtime.md)

## <a name="supported-regions"></a>Desteklenen bölgeler

Data Factory Şu anda kullanılabildiği Azure bölgelerinin bir listesi için, aşağıdaki sayfada ilgilendiğiniz bölgeleri seçin ve sonra **Data Factory**: [bölgeye göre kullanılabilir ürünleri](https://azure.microsoft.com/global-infrastructure/services/)bulmak için **analiz** ' ı genişletin. Ancak, bir veri fabrikası, verileri veri depoları arasında taşımak veya işlem hizmetleri 'ni kullanarak veri işlemek için diğer Azure bölgelerindeki veri depolarına ve işlem hizmetlerine erişebilir.

Azure Data Factory kendisi herhangi bir veri depolamaz. Desteklenen veri depoları arasında veri hareketini ve diğer bölgelerde veya şirket içi bir ortamda işlem hizmetlerini kullanarak veri işlemeyi düzenlemek için veri odaklı iş akışları oluşturmanıza olanak tanır. Ayrıca, programlama ve UI mekanizmalarını kullanarak iş akışlarını izlemenizi ve yönetmenizi de sağlar.

Data Factory yalnızca belirli bölgelerde kullanılabilir olsa da, Data Factory veri hareketini destekleyen hizmet birçok bölgede küresel olarak kullanılabilir. Bir veri deposu bir güvenlik duvarının arkasındaysa, şirket içi ortamınızda yüklü olan şirket içinde barındırılan bir Integration Runtime bunun yerine verileri taşımalıdır.

Örneğin, Azure HDInsight kümesi ve Azure Machine Learning gibi işlem ortamlarınızın Batı Avrupa bölgenin dışında çalıştığını varsayalım. Doğu ABD veya Doğu ABD 2 Azure Data Factory bir örnek oluşturup kullanabilir ve bunu, Batı Avrupa içindeki işlem ortamlarınızda iş zamanlamak için kullanabilirsiniz. Data Factory işlem ortamınızda işi tetiklemesi birkaç milisaniye sürer, ancak bilgi işlem ortamınızda işin çalıştırılması için geçen süre değişmez.

## <a name="accessibility"></a>Erişilebilirlik

Azure portal Data Factory Kullanıcı deneyimine erişilebilir.

## <a name="compare-with-version-1"></a>Sürüm 1 ile karşılaştır
Sürüm 1 ile Data Factory hizmetinin geçerli sürümü arasındaki farkların listesi için bkz. [Sürüm 1 Ile karşılaştırma](compare-versions.md). 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki araçlardan/SDK 'Lardan birini kullanarak Data Factory işlem hattı oluşturmaya başlayın: 

- [Azure portal Kullanıcı arabirimini Data Factory](quickstart-create-data-factory-portal.md)
- [Veri Kopyalama araç Azure portal](quickstart-create-data-factory-copy-data-tool.md)
- [PowerShell](quickstart-create-data-factory-powershell.md)
- [.NET](quickstart-create-data-factory-dot-net.md)
- [Python](quickstart-create-data-factory-python.md)
- [REST](quickstart-create-data-factory-rest-api.md)
- [Azure Resource Manager şablonu](quickstart-create-data-factory-resource-manager-template.md)
 
