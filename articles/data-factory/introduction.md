---
title: Azure Data Factory'ye giriş
description: Verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bir bulut veri tümleştirme hizmeti olan Azure Data Factory hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
ms.service: data-factory
ms.workload: data-services
ms.topic: overview
ms.date: 09/30/2019
ms.openlocfilehash: 327aadbc2996e91ef0f7639da86a647ddd42247c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81410936"
---
# <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir?

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Büyük veri dünyasında ham ve düzensiz veriler genellikle ilişkisel, ilişkisel olmayan ve diğer depolama sistemlerinde depolanır. Ancak, ham veriler kendi başlarına analiz uzmanlarına, veri bilimcilerine veya iş karar mekanizmalarına anlamlı bilgiler sağlamak için uygun bağlama veya anlama sahip değildir. 

Büyük veri için gerekli olan, muazzam büyüklükteki bu ham veri depolarını eyleme dönüştürülebilen iş öngörüleri haline getirmek için düzenleyici ve faaliyete geçiren süreçler sağlayan bir hizmettir. Azure Data Factory, bu karmaşık karma ayıkla-dönüştür-yükle (ETL), ayıkla-yükle-dönüştür (ELT) ve veri tümleştirme projeleri için oluşturulmuş, yönetilen bir bulut hizmetidir.

Örneğin, buluttaki oyunlar tarafından üretilmiş petabaytlarca oyun günlüğünü toplayan bir oyun şirketi düşünün. Şirket, bu günlükleri analiz ederek müşteri tercihleri, demografik verileri ve kullanım davranışı hakkında bilgi sahibi olmak istemektedir. Ayrıca yukarı satış ve çapraz satış fırsatlarını belirlemek, yeni cazip özellikler geliştirmek, işleri büyütmek ve müşterilerine daha iyi bir deneyim sunmayı amaçlamaktadır.

Bu günlükleri analiz etmek için, şirketin şirket içi veri deposunda bulunan müşteri bilgileri, oyun bilgileri ve pazarlama kampanyası bilgileri gibi başvuru verilerini kullanması gerekir. Şirket bu verileri şirket içi veri deposundan bir bulut veri deposunda sahip olduğu ek günlük verileriyle bir arada kullanmak istemektedir. 

Öngörüler elde etmek için, bulutta (Azure HDInsight) bir Spark kümesi kullanarak birleştirilmiş verileri işlemeyi ve bunlara ilişkin bir raporu kolayca oluşturmak üzere dönüştürülmüş verileri Azure SQL Veri Ambarı gibi bir bulut veri ambarında yayımlamayı planlamaktadır. Şirket bu iş akışını otomatik hale getirmeyi ve günlük düzende izleyip yönetmeyi istemektedir. Ayrıca bu iş akışının blob depolama kapsayıcısına dosya geldiğinde yürütülmesini istemektedir.

Azure Data Factory, bu tür veri senaryolarını çözen platformdur. *Veri hareketini düzenlemek ve verileri ölçekte dönüştürmek için veri tabanlı iş akışları oluşturmanıza olanak tanıyan bulut tabanlı ETL ve veri tümleştirme hizmetidir.* Azure Data Factory platformunu kullanarak farklı veri depolarından veri alabilen veri odaklı iş akışları (işlem hattı olarak adlandırılır) oluşturabilir ve zamanlayabilirsiniz. Verileri veri akışlarıyla görsel olarak dönüştüren veya Azure HDInsight Hadoop, Azure Databricks ve Azure SQL Veritabanı gibi bilgi işlem hizmetlerini kullanarak karmaşık ETL işlemleri oluşturabilirsiniz. 

Ayrıca, dönüştürülmüş verilerinizi, iş zekası (BI) uygulamalarının tüketilmesi için Azure SQL Veri Ambarı gibi veri depolarına da yayımlayabilirsiniz. Sonuç olarak, Azure Data Factory sayesinde ham veriler daha iyi iş kararları için anlamlı veri depoları ve veri gölleri halinde düzenlenebilir.

![Data Factory'nin üstten görünümü](media/data-flow/overview.png)

## <a name="how-does-it-work"></a>Nasıl çalışır?

Veri Fabrikası, veri mühendisleri için uçtan uca tam bir platform sağlayan bir dizi birbirine bağlı sistem içerir.

### <a name="connect-and-collect"></a>Bağlanma ve toplama

Kuruluşlar şirket içinde, bulutta bulunan yapılandırılmış, yapılandırılmamış veya yarı yapılandırılmış ve tümü farklı aralık ve hızlarda gelen farklı kaynaklarda bulunan çeşitli veri türlerine sahiptir. 

Bilgi üretim sistemi oluşturmanın ilk adımı hizmet olarak yazılım (SaaS) hizmetleri, veritabanları, dosya paylaşımları, FTP, web hizmetleri gibi tüm gerekli veri kaynaklarına ve işleme çalışmalarına bağlanmaktır. Sonraki adım ise takip eden işleme çalışmaları için gerektiğinde verileri merkezi bir konuma taşımaktır.

Data Factory olmadığında, kuruluşların bu veri kaynaklarını ve işleme çalışmalarını tümleştirmek için özel veri taşıma bileşenleri oluşturması veya özel hizmetler yazması gerekir. Bu tür sistemleri tümleştirmenin ve bakımını yapmanın maliyeti yüksektir. Buna ek olarak bu sistemlerde tamamen yönetilebilir bir hizmetin sunduğu kurumsal sınıf izleme, uyarı oluşturma ve denetim özellikleri mevcut değildir.

Veri Fabrikası ile, verileri [Copy Activity](copy-activity-overview.md) hem şirket içi hem de bulut kaynağı veri depolarından buluttaki merkezileştirme veri deposuna taşımak için veri ardışık ardışık işlem deksüleyi kullanabilirsiniz. Örneğin, Azure Veri Gölü Depolama'da veri toplayabilir ve daha sonra bir Azure Veri Gölü Analizi bilgi işlem hizmetini kullanarak verileri dönüştürebilirsiniz. Verileri Azure Blob depolama alanından toplayıp daha sonra Azure HDInsight Hadoop kümesi kullanarak da dönüştürebilirsiniz.

### <a name="transform-and-enrich"></a>Dönüştürme ve zenginleştirme
Veriler bulutta merkezi bir veri deposunda bulunduktan sonra, ADF eşleme veri akışlarını kullanarak toplanan verileri işleyin veya dönüştürün. Veri akışları, veri mühendislerinin Spark kümelerini veya Spark programlamayı anlamasına gerek kalmadan Spark'ta çalıştırılabilen veri dönüştürme grafikleri oluşturmasına ve sürdürmesine olanak tanır.

Dönüşümleri elle kodlamayı tercih ederseniz, ADF HDInsight Hadoop, Spark, Data Lake Analytics ve Machine Learning gibi bilgi işlem hizmetlerinde dönüşümlerinizi gerçekleştirmek için dış etkinlikleri destekler.

### <a name="cicd-and-publish"></a>CI/CD ve yayım
Veri Fabrikası, Azure DevOps ve GitHub'ı kullanarak veri ardışık hatlarınızın CI/CD'si için tam destek sunar. Bu, bitmiş ürünü yayınlamadan önce ETL süreçlerinizi aşamalı olarak geliştirmenize ve teslim etmenizi sağlar. Ham veriler iş için kullanılabilir biçime getirildikten sonra, verileri Azure Veri Ambarı, Azure SQL Veritabanı, Azure CosmosDB'ye veya şirket kullanıcılarınızın iş zekası araçlarından işaret edebildiği herhangi bir analiz altyapısına yükleyebilirsiniz.

### <a name="monitor"></a>İzleme
Veri tümleştirme işlem hattınızı başarıyla oluşturup dağıtarak iyileştirilmiş verilerden iş değeri elde ettikten sonra, başarı ve hata oranları için zamanlanmış etkinlikleri ve işlem hatlarını izleyin. Azure Veri Fabrikası, Azure portalındaki Azure Monitor, API, PowerShell, Azure Monitor günlükleri ve sistem durumu panelleri aracılığıyla boru hattı izleme için yerleşik desteğe sahiptir.

## <a name="top-level-concepts"></a>Üst düzey kavramlar
Azure aboneliğinin bir veya birden çok Azure Data Factory örneği (veya veri fabrikası) olabilir. Azure Data Factory dört temel bileşenden oluşur. Bu bileşenler, üzerinde veri taşıma ve dönüştürme adımları ile veri odaklı iş akışları oluşturabileceğiniz platformu sağlamak üzere birlikte çalışır.

### <a name="pipeline"></a>İşlem hattı
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattı, bir iş birimini gerçekleştirmeye yönelik mantıksal bir etkinlik grubudur. İşlem hattındaki etkinlikler birlikte bir görev gerçekleştirir. Örneğin, bir işlem hattı Azure blobundan verileri alan ve ardından HDInsight kümesinde Hive sorgusu çalıştırarak verileri bölümlere ayıran bir grup etkinlik içerebilir. 

İşlem hattının avantajı, etkinliklerin her birini tek tek yönetmek yerine bir küme olarak yönetmenize olanak tanımasıdır. Bir işlem hattındaki etkinlikler, sırayla çalışmak üzere birbirine zincirlenebilir veya paralel olarak birbirinden bağımsız çalışabilir.

### <a name="mapping-data-flows"></a>Veri akışlarını eşleme
Herhangi bir boyuttaki veriyi dönüştürmek için kullanabileceğiniz veri dönüştürme mantığının grafiklerini oluşturun ve yönetin. Yeniden kullanılabilir bir veri dönüştürme yordamları kitaplığı oluşturabilir ve bu işlemleri ADF ardışık yapı işlemlerinden ölçeklenmiş bir şekilde yürütebilirsiniz. Veri Fabrikası, ihtiyacınız olduğunda aşağı dönen ve dönen bir Kıvılcım kümesinde mantığınızı çalıştıracaktır. Kümeleri yönetmek veya korumak zorunda kalmazsınız.

### <a name="activity"></a>Etkinlik
Etkinlikler bir işlem hattındaki işleme adımını temsil eder. Örneğin, bir veri deposundan başka bir veri deposuna veri kopyalamak için kopyalama etkinliğini kullanabilirsiniz. Benzer şekilde, verilerinizi dönüştürmek veya analiz etmek amacıyla Azure HDInsight kümesinde bir Hive sorgusu çalıştıran bir Hive etkinliği kullanabilirsiniz. Data Factory üç tür etkinliği destekler: veri taşıma etkinlikleri, veri dönüştürme etkinlikleri ve denetim etkinlikleri.

### <a name="datasets"></a>Veri kümeleri
Veri kümeleri, veri depoları içinde etkinliklerinizde giriş veya çıkış olarak kullanmak istediğiniz verilere işaret eden veya başvuruda bulunan veri yapılarını temsil eder. 

### <a name="linked-services"></a>Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory'ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Şöyle düşünün: bağlı bir hizmet, veri kaynağıyla bağlantıyı tanımlar ve veri kümesi verilerin yapısını temsil eder. Örneğin, Azure Depolama bağlı hizmeti Azure Depolama hesabına bağlanacak bağlantı dizesini belirtir. Ayrıca, bir Azure blob veri kümesi blob kapsayıcıyı ve verileri içeren klasörü belirtir.

Bağlı hizmetler Data Factory’de iki amaçla kullanılır:

- Bir **veri deposunu**, buradakilerle, ancak bunlarla sınırlı olmamak şartıyla göstermek için: şirket içi SQL Server veritabanı, Oracle veritabanı, dosya paylaşımı veya bir Azure blob depolama hesabı. Desteklenen veri depolarının listesi için [kopyalama etkinliği](copy-activity-overview.md) makalesine bakın.

- Bir etkinliğin yürütülmesini barındırabilecek bir **bilgi işlem kaynağını** temsil etmek. Örneğin, HDInsightHive etkinliği bir HDInsight Hadoop kümesinde yürütülür. Dönüştürme etkinlikleri ve desteklenen işlem ortamlarının listesi için [veri dönüştürme](transform-data.md) makalesine bakın.

### <a name="triggers"></a>Tetikleyiciler
Tetikleyiciler, bir işlem hattı çalıştırmasının başlatılması gereken zamanı belirleyen işlem birimini temsil eder. Farklı etkinlik türleri için farklı tetikleyici türleri vardır.

### <a name="pipeline-runs"></a>İşlem hattı çalıştırmaları
İşlem hattı çalıştırması, işlem hattı yürütme örneğidir. İşlem hattı çalıştırmaları örneği genelde bağımsız değişkenlerin işlem hatlarında tanımlanan parametrelere iletilmesiyle oluşturulur. Bağımsız değişkenler el ile veya tetikleyici tanımı içinde geçirilebilir.

### <a name="parameters"></a>Parametreler
Parametreler salt okunur yapılandırmanın anahtar-değer çiftleridir.Parametreler işlem hattında tanımlanır. Tanımlı parametrelerin bağımsız değişkenleri, bir tetikleyici tarafından oluşturulan çalıştırma bağlamı veya el ile yürütülen işlem hattından yürütme sırasında geçirilir. İşlem hattındaki etkinlikler parametre değerlerini kullanır.

Veri kümesi, türü kesin olarak belirtilmiş bir parametre ve yeniden kullanılabilir/başvurulabilir bir varlıktır. Bir etkinlik, veri kümelerine başvurabilir ve veri kümesi tanımında belirtilen özellikleri kullanabilir.

Bağlı hizmet de türü kesin olarak belirtilmiş ve veri deposu ya da işlem ortamı ile bağlantı bilgilerini içeren bir parametredir. Bu da yeniden kullanılabilir/başvurulabilir bir varlıktır.

### <a name="control-flow"></a>Denetim akışı
Denetim akışı, işlem hattı düzeyinde ve işlem hattı talep üzerine ya da bir tetikleyiciden çağrılırken geçirilen bağımsız değişkenlerde tanımlanabilen dizi, dallanma ve parametrelerdeki zincirleme etkinliklerini içeren işlem hattı etkinliklerinin düzenlenmesidir. Ayrıca özel durum geçirme ve döngü kapsayıcılarını, diğer bir deyişle For-each yineleyicilerini içerir.

### <a name="variables"></a>Değişkenler
Değişkenler, geçici değerleri depolamak için ardışık hatlar içinde kullanılabilir ve ardışık hatlar, veri akışları ve diğer etkinlikler arasında geçen değerleri etkinleştirmek için parametrelerle birlikte de kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
Burada keşfetmek için önemli bir sonraki adım belgeleri şunlardır:

- [Veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md)
- [İşlem hatları ve etkinlikler](concepts-pipelines-activities.md)
- [Tümleştirme çalışma zamanı](concepts-integration-runtime.md)
- [Veri Akışlarını Haritalama](concepts-data-flow-overview.md)
- [Azure portalındaki Data Factory kullanıcı arabirimi](quickstart-create-data-factory-portal.md)
- [Azure portalındaki Veri Kopyalama aracı](quickstart-create-data-factory-copy-data-tool.md)
- [PowerShell](quickstart-create-data-factory-powershell.md)
- [.NET](quickstart-create-data-factory-dot-net.md)
- [Python](quickstart-create-data-factory-python.md)
- [REST](quickstart-create-data-factory-rest-api.md)
- [Azure Resource Manager şablonu](quickstart-create-data-factory-resource-manager-template.md)
 
