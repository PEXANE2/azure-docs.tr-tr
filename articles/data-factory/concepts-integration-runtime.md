---
title: Tümleştirme çalışma zamanı
description: Azure Data Factory'deki tümleştirme çalışma zamanı hakkında bilgi edinin.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/26/2020
ms.openlocfilehash: ffa348c796a4d9d4e3bdb8e7ce18ba0eb82e17ad
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418397"
---
# <a name="integration-runtime-in-azure-data-factory"></a>Azure Data Factory'deki tümleştirme çalışma zamanı 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Integration Runtime (IR), Azure Data Factory tarafından farklı ağ ortamlarında aşağıdaki veri tümleştirme özelliklerini sunmak için kullanılan işlem altyapısıdır:

- **Veri Akışı**: Yönetilen Azure bilgi işlem ortamında veri [akışını](concepts-data-flow-overview.md) yürütün.  
- **Veri hareketi**: Verileri genel ağdaki veri depoları arasında ve özel ağdaki veri depolarında (şirket içi veya sanal özel ağ) kopyalayın. Yerleşik bağlayıcılar, biçim dönüştürme, sütun eşleme, performanslı ve ölçeklenebilir veri aktarımı desteği sunar.
- **Etkinlik gönderimi**: Azure Databricks, Azure HDInsight, Azure Machine Learning, Azure SQL Database, SQL Server ve daha fazlası gibi çeşitli bilgi işlem hizmetlerinde çalışan dönüşüm etkinliklerini gönderme ve izleme.
- **SSIS paketi yürütme**: SQL Server Integration Services (SSIS) paketlerini yönetilen bir Azure işlem ortamında yerel olarak yürütün.

Data Factory'de etkinlik, gerçekleştirilecek eylemi tanımlar. Bağlı hizmet, bir hedef veri deposunu veya işlem hizmetini tanımlar. Tümleştirme çalışma zamanı, etkinlik ile bağlı Hizmetler arasında köprü görevi görür.  Bağlı hizmet veya etkinlik tarafından başvurulur ve etkinliğin çalıştığı veya gönderildiği işlem ortamını sağlar. Bu şekilde etkinlik hedef veri deposuna veya işlem hizmetine en yakın bölgeden en yüksek performansla gerçekleştirilirken güvenlik ve uyum gereksinimleri korunmuş olur.

## <a name="integration-runtime-types"></a>Tümleştirme çalışma zamanı türleri

Veri Fabrikası üç tür Tümleştirme Çalışma Süresi (IR) sunar ve aradığınız veri tümleştirme özelliklerine ve ağ ortamı gereksinimlerine en iyi şekilde hizmet veren türü seçmelisiniz.  Bu üç tür şunlardır:

- Azure
- Kendinden konak
- Azure-SSIS

Aşağıdaki tabloda tümleştirme çalışma zamanı türlerinin her birinin sunduğu özellikler ve ağ desteği açıklanmaktadır:

IR türü | Ortak ağ | Özel ağ
------- | -------------- | ---------------
Azure | Veri Akışı<br/>Veri taşıma<br/>Etkinlik dağıtma | &nbsp;
Kendinden konak | Veri taşıma<br/>Etkinlik dağıtma | Veri taşıma<br/>Etkinlik dağıtma
Azure-SSIS | SSIS paketi yürütme | SSIS paketi yürütme

Aşağıdaki şemada gelişmiş veri tümleştirme özellikleri ve ağ desteği sunmak için birlikte kullanılabilecek farklı tümleştirme çalışma zamanları gösterilmiştir:

![Farklı tümleştirme çalışma zamanı türleri](media/concepts-integration-runtime/different-integration-runtimes.png)

## <a name="azure-integration-runtime"></a>Azure tümleştirme çalışma zamanı

Azure tümleştirme çalışma zamanı şunları yapabilir:

- Azure'da Veri Akışlarını Çalıştırma 
- Bulut veri depoları arasında kopyalama etkinliği gerçekleştirme
- Genel ağda aşağıdaki dönüşüm etkinliklerinin gönderilmesi: Databricks Notebook/ Jar/ Python etkinliği, HDInsight Hive etkinliği, HDInsight Pig etkinliği, HDInsight MapAzaltma etkinliği, HDInsight Spark etkinliği, HDInsight Streaming etkinliği, Machine Learning Toplu Yürütme etkinliği, Machine Learning Update Kaynak etkinlikleri, Depolanan Yordam etkinliği, Veri Gölü Analizi U-SQL etkinliği, .NET özel etkinlik, Web etkinliği, Arama etkinliği ve Meta veri etkinliği alın.

### <a name="azure-ir-network-environment"></a>Azure IR ağ ortamı

Azure Tümleştirme Çalışma Süresi, veri depolarına bağlanmayı destekler ve hizmetleri herkese açık uç noktalarla hesaplar. Azure Sanal Ağ ortamı için kendiliğinden konak tümleştirme çalışma zamanı kullanın.

### <a name="azure-ir-compute-resource-and-scaling"></a>Azure IR işlem kaynağı ve ölçeklendirme
Azure tümleştirme çalışma zamanı Azure'da tamamen yönetilebilen ve sunucusuz bir işlem sunar.  Altyapı sağlama, yazılım yükleme, yama veya kapasite ölçekleme konusunda endişelenmenize gerek yoktur.  Ayrıca yalnızca gerçekten kullandığınız süre boyunca ödeme yaparsınız.

Azure tümleştirme çalışma zamanı verileri bulut veri depoları arasında güvenli, güvenilir ve yüksek performanslı bir şekilde taşınması için gerekli yerel işlemi sunar.  Kopyalama etkinliğinde kullanılacak veri tümleştirme birimi sayısını belirleyebilirsiniz. Bunu yaptığınızda Azure IR işlem boyutu esnek şekilde ölçeklendirilerek Azure Integration Runtime boyutunu el ile ayarlama ihtiyacını ortadan kaldırır. 

Etkinlik gönderme, etkinliği hedef işlem hizmetine yönlendirmek için hafif bir işlemdir, bu nedenle bu senaryo için işlem boyutunu büyütmenize gerek yoktur.

Bir Azure IR oluşturma ve yapılandırma hakkında bilgi için, yol larla nasıl oluşturulup yapılandırılabilen Azure IR'yi nasıl oluşturup yapılandırılabildiğini öğrenin. 

> [!NOTE] 
> Azure Tümleştirme çalışma süresi, veri akışlarını çalıştırmak için kullanılacak temel bilgi işlem altyapısını tanımlayan Veri Akışı çalışma zamanıyla ilgili özelliklere sahiptir. 

## <a name="self-hosted-integration-runtime"></a>Kendinden konak tümleştirme çalışma zamanı

Kendinden konak IR şu özelliklere sahiptir:

- Bulut veri depoları ve özel ağdaki veri deposu arasında kopyalama etkinliği çalıştırma.
- Şirket içi veya Azure Sanal Ağı'nda bilgi işlem kaynaklarına karşı aşağıdaki dönüşüm etkinliklerinin gönderilmesi: HDInsight Hive etkinliği (BYOC-Bring Your Own Cluster), HDInsight Pig etkinliği (BYOC), HDInsight MapReduce etkinliği (BYOC), HDInsight Spark etkinliği (BYOC), HDInsight Streaming etkinliği (BYOC), Machine Learning Toplu Yürütme etkinliği, Machine Learning Kaynak Güncelleme etkinlikleri, Stored Procedure etkinliği, Veri Gölü Analizi U-SQL etkinliği, Özel Etkinlik (Toplu Toplu Çalıştırmada çalışır) , Arama etkinliği ve Meta veri etkinliği alın.

> [!NOTE] 
> SAP Hana, MySQL, vb. gibi kendi sürücünüzü getirmenizi gerektiren veri depolarını desteklemek için kendi barındırılan tümleştirme çalışma zamanını kullanın.  Daha fazla bilgi için [desteklenen veri depoları'na](copy-activity-overview.md#supported-data-stores-and-formats)bakın.

> [!NOTE] 
> Java Runtime Environment (JRE), Self Hosted IR'nin bir bağımlılığıdır. Lütfen jre aynı ana bilgisayarda yüklü olduğundan emin olun.

### <a name="self-hosted-ir-network-environment"></a>Kendinden konak IR ağ ortamı

Genel bulut ortamından doğrudan görüş alanı olmayan özel bir ağ ortamında veri tümleştirmesini güvenli bir şekilde gerçekleştirmek istiyorsanız, şirket güvenlik duvarınızın arkasındaki şirket ortamına veya sanal özel bir ağ içinde kendi kendine barındırılan bir IR yükleyebilirsiniz.  Kendinden konak tümleştirme çalışma zamanı yalnızca açık internete giden HTTP tabanlı bağlantılar oluşturur.

### <a name="self-hosted-ir-compute-resource-and-scaling"></a>Kendinden konak IR işlem kaynağı ve ölçeklendirme

Kendi kendine barındırılan IR'yi şirket içi bir makineye veya özel bir ağ içinde sanal bir makineye yükleyin. Şu anda kendinden konak IR yalnızca Windows işletim sistemlerinde çalışmaktadır.  

Yüksek kullanılabilirlik ve ölçeklenebilirlik için kendinden konak IR ölçeğini mantıksal örneği birden fazla şirket içi makineyle etkin-etkin modda ilişkilendirerek genişletebilirsiniz.  Daha fazla bilgi için, ayrıntılar için nasıl kılavuzlar oluşturulup, kendi kendine barındırılan IR makalesinin nasıl [oluşturulup yapılandırılabildiğini](create-self-hosted-integration-runtime.md) öğrenin.

## <a name="azure-ssis-integration-runtime"></a>Azure-SSIS Integration Runtime

Var olan SSIS iş yükünü artırmak ve değiştirmek için Azure-SSIS IR oluşturarak SSIS paketlerini yerel ortamda yürütebilirsiniz.

### <a name="azure-ssis-ir-network-environment"></a>Azure-SSIS IR ağ ortamı

Azure-SSIS IR ortak ağ veya özel ağ üzerinde sağlanabilir.  Şirket içi verilere erişim için Azure-SSIS IR’nin şirket içi ağınıza bağlı bir Sanal Ağa katılması gerekir.  

### <a name="azure-ssis-ir-compute-resource-and-scaling"></a>Azure-SSIS IR işlem kaynağı ve ölçeklendirme

Azure-SSIS IR, SSIS paketlerinizi çalıştırmaya ayrılmış Azure sanal makinelerinin tam yönetilen bir kümesidir. Bağlı olacak SSIS projeleri/paketleri (SSISDB) kataloğunu barındırmak için kendi Azure SQL Veritabanınızı veya Yönetilen Örnek sunucunuzu getirebilirsiniz. Düğüm boyutunu belirttikten sonra kümedeki düğüm sayısını belirtik ölçeğini genişleterek işlem gücünü artırabilirsiniz. Azure-SSIS Integration Runtime hizmetini gerekli olduğunda durdurup başlatarak çalıştırma maliyetlerini kontrol altına alabilirsiniz.

Daha fazla bilgi için nasıl yapılır kılavuzlarında Azure SSIS IR oluşturma ve yapılandırma makalesine bakın.  Oluşturduktan sonra var olan SSIS paketlerinizi çok az veya sıfır değişiklikle SQL Server Veri Araçları (SSDT) ve SQL Server Management Studio (SSMS) gibi bilinen araçları kullanarak şirket içi SSIS kullanır gibi dağıtabilir ve yönetebilirsiniz.

Azure-SSIS çalışma zamanı hakkında daha fazla bilgi için aşağıdaki makalelere bakın: 

- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makalede, bir Azure-SSIS IR oluşturmak için adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL Veritabanı kullanır. 
- [Nasıl yapılır: Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale, öğreticiyi genişletir ve Azure SQL Veritabanı Yönetilen Örneği'ni kullanma ve IR'yi sanal bir ağa katılma hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleyin](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure portalını kullanarak Azure-SSIS IR’nin sanal ağa katılmasını sağlayacak şekilde sanal ağı yapılandırma adımlarını da sunar. 

## <a name="integration-runtime-location"></a>Tümleştirme çalışma zamanının konumu

Data Factory konumu, veri fabrikası meta verilerinin depolandığı ve işlem hattı tetiklemesinin başlatıldığı konumdur. Bu arada, verileri veri depoları arasında taşımak ve işlem hizmetlerini kullanarak verileri işlemek amacıyla veri fabrikası, başka Azure bölgelerindeki veri depolarına ve işlem hizmetlerine erişebilir. Bu davranış veri uyumluluğu, verimlilik ve düşük ağ kullanım maliyetleri için [global ölçekte kullanılabilen IR](https://azure.microsoft.com/global-infrastructure/services/) aracılığıyla gerçekleştirilir.

IR Konumu arka uç işleminin konumunu tanımlar ve bu veri taşıma, etkinlik dağıtımı ve SSIS paket yürütme işlemlerinin gerçekleştirileceği konumdur. IR konumu veri fabrikasının ait olduğu konumdan farklı olabilir. 

### <a name="azure-ir-location"></a>Azure IR konumu

- Kopyalama etkinliği için ADF, lavabo veri deponuzun konumunu otomatik olarak algılamak için en iyi çabayı gösterecektir ve ardından varsa aynı bölgede veya aynı coğrafyadaki en yakın bölgede IR'yi kullanacaktır; lavabo veri deposunun bölgesi algılanabilir değilse, alternatif olarak veri fabrikası bölgesinde IR kullanılır.

  Örneğin, fabrikanızı Doğu ABD'de oluşturdunuz, 
  
  - Verileri Batı ABD'deki Azure Blob'a kopyaladığında, ADF Blob'un Batı ABD'de olduğunu başarıyla tespit ettiyse, kopyalama etkinliği Batı ABD'de IR'de yürütülür; bölge algılama başarısız olursa, kopyalama etkinliği Doğu ABD'de IR yürütülür.
  - Verileri, bölgenin algılanamayan Salesforce'a kopyaladığı zaman, kopyalama etkinliği Doğu ABD'de IR'de yürütülür.

- Kopyalama etkinliği için ADF, aynı bölgede (varsa) veya aynı coğrafyadaki en yakın konumda veya veri fabrikası bölgesini alternatif olarak kullanmak için algılanamazsa en iyi konumu seçmek için lavabo ve kaynak veri deponuzu otomatik olarak algılamak için en iyi çabayı gösterir.

- Arama/GetMetadata/Delete etkinlik yürütme (Pipeline etkinlikleri olarak da bilinir), dönüşüm etkinliği gönderme (Dış etkinlikler olarak da bilinir) ve yazma işlemleri (test bağlantısı, klasör listesine ve tablo listesine göz atma, önizleme verileri) için ADF, veri fabrikası bölgesinde IR kullanır.

- Veri Akışı için ADF, veri fabrikası bölgesinde IR kullanır. 

  > [!TIP] 
  > İyi bir uygulama, Veri akışının ilgili veri depolarınızla aynı bölgede çalıştığından emin olmak olacaktır (mümkünse). Bunu, Azure IR'yi otomatik olarak çözerek (veri depolama konumu Veri Fabrikası konumuyla aynıysa) veya veri depolarınızla aynı bölgede yeni bir Azure IR örneği oluşturarak ve üzerindeki veri akışını yürüterek elde edebilirsiniz. 

Kullanıcı arabirimindeki işlem hattı etkinliğini izleme görünümünde veya etkinlik izleme yükündeki etkinlik yürütme işlemi sırasında kullanıma alınan IR konumunu izleyebilirsiniz.

### <a name="self-hosted-ir-location"></a>Kendinden konak IR konumu

Kendinden konak IR, mantıksal olarak Data Factory'ye kayıtlıdır ve işlevini desteklemek için kullanılan işlem sizin tarafınızdan sağlanır. Bu nedenle kendinden konak IR için açık bir konum özelliği yoktur. 

Kendinden konak IR veri taşıma işlemini gerçekleştirmek için kullanıldığında kaynaktan veri ayıklar ve hedefe yazar.

### <a name="azure-ssis-ir-location"></a>Azure SSIS IR konumu

Ayıklama, dönüştürme, yükleme (ETL) iş akışlarınızda yüksek performansa ulaşmak için doğru Azure-SSIS IR konumunu seçmek önemlidir.

- Azure-SSIS IR'nizin konumunun veri fabrikanızın konumuyla aynı olması gerekmez, ancak SSISDB'nin barındırılan olduğu kendi Azure Veritabanınızın veya Yönetilen Örnek sunucunuzun konumuyla aynı olmalıdır. Bu şekilde Azure-SSIS Integration Runtime biriminiz farklı konumlar arasında aşırı trafik oluşturmadan kolayca SSISDB öğesine erişebilir.
- SSISDB'yi barındıracak varolan bir Azure SQL Veritabanınız veya Yönetilen Örnek sunucunuz yoksa, ancak şirket içi veri kaynaklarınız/hedefleriniz varsa, şirket içi ağınıza bağlı sanal bir ağın aynı konumunda yeni bir Azure SQL Veritabanı veya Yönetilen Örnek sunucusu oluşturmanız gerekir.  Bu şekilde, azure-ssis IR'nizi yeni Azure SQL Veritabanı veya Yönetilen Örnek sunucusunu kullanarak oluşturabilir ve aynı konumda bulunan sanal ağa katılarak farklı konumlardaki veri hareketlerini etkili bir şekilde en aza indirebilirsiniz.
- SSISDB'nin barındırıldığı mevcut Azure SQL Veritabanı veya Yönetilen Örnek sunucunuzun konumu şirket içi ağınıza bağlı bir sanal ağın konumuyla aynı değilse, önce mevcut bir Azure SQL Veritabanı veya Yönetilen Örnek sunucusunu kullanarak Azure-SSIS IR'nizi oluşturun ve aynı konumdaki başka bir sanal ağa katılın ve ardından sanal ağı farklı konumlar arasında sanal ağ bağlantısına yapılandırın.

Aşağıdaki şemada Data Factory konum ayarları ve tümleştirme çalışma zamanları gösterilmektedir:

![Tümleştirme çalışma zamanının konumu](media/concepts-integration-runtime/integration-runtime-location.png)

## <a name="determining-which-ir-to-use"></a>Kullanılacak IR'yi belirleme

### <a name="copy-activity"></a>Kopyalama etkinliği

Kopyalama etkinliği için veri akışı yönünü tanımlamak üzere kaynak ve havuz bağlantılı hizmetleri gerektirir. Kopyalama işlemini gerçekleştirmek için kullanılacak olan tümleştirme çalışma zamanı örneğini belirlemek için aşağıdaki mantık kullanılır: 

- **İki bulut veri kaynağı arasında kopyalama**: Hem kaynak hem de lavabobağlantılı hizmetler Azure IR kullanıyorsa, ADF belirttiğiniz zaman bölgesel Azure IR'sini kullanır veya [Tümleştirme çalışma zamanı konum](#integration-runtime-location) bölümünde açıklandığı gibi otomatik çözümleme IR'sini (varsayılan) seçerseniz Azure IR konumunu otomatik olarak belirler.
- **Bir bulut veri kaynağından özel ağdaki veri kaynağına kopyalama**: Kaynak veya havuz bağlantılı hizmet noktaları kendinden konak IR birimine işaret ediyorsa kopyalama etkinliği kendinden konak Integration Runtime üzerinde yürütülür.
- **Özel ağ üzerindeki iki veri kaynağı arasında kopyalama**: Hem kaynak hem de havuz Bağlantılı Hizmetin aynı tümleştirme çalışma zamanı örneğine işaret etmesi gerekir ve kopyalama Etkinliğini yürütmek için bu tümleştirme çalışma zamanı kullanılır.

### <a name="lookup-and-getmetadata-activity"></a>Lookup ve GetMetadata etkinliği

Lookup ve GetMetadata etkinliği, veri deposu bağlı hizmetiyle ilişkili tümleştirme çalışma zamanı üzerinde yürütülür.

### <a name="external-transformation-activity"></a>Dış dönüşüm etkinliği

Harici bir işlem altyapısı kullanan her dış dönüşüm etkinliğinin, tümleştirme çalışma süresine işaret eden bir hedef işlem Bağlantılı Hizmeti vardır. Bu tümleştirme çalışma zamanı örneği, bu dış el kodlu dönüştürme etkinliğinin gönderildiği konumu belirler.

### <a name="data-flow-activity"></a>Veri Akışı etkinliği

Veri Akışı etkinlikleri, onunla ilişkili Azure tümleştirme çalışma zamanında yürütülür. Veri Akışları tarafından kullanılan Spark bilgi işlem, Azure Tümleştirme Çalışma Zamanı'nızdaki veri akışı özellikleri tarafından belirlenir ve tamamen ADF tarafından yönetilir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere bakın:

- [Azure tümleştirme çalışma zamanı oluşturma](create-azure-integration-runtime.md)
- [Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma](create-self-hosted-integration-runtime.md)
- [Azure-SSIS tümleştirme çalışma zamanı oluşturun.](create-azure-ssis-integration-runtime.md) Bu makale, öğreticiyi genişletir ve Azure SQL Veritabanı Yönetilen Örneği'ni kullanma ve IR'yi sanal bir ağa katılma hakkında yönergeler sağlar. 
