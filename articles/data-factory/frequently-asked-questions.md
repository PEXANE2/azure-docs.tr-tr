---
title: 'Azure Veri Fabrikası: Sık sorulan sorular '
description: Azure Veri Fabrikası hakkında sık sorulan soruların yanıtlarını alın.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: 34972e70039fef17161bdef66f64278cbabf908f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80130794"
---
# <a name="azure-data-factory-faq"></a>Azure Data Factory Hakkında SSS
Bu makalede, Azure Veri Fabrikası hakkında sık sorulan soruların yanıtları verilmektedir.  

## <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir? 
Veri Fabrikası, verilerin hareketini ve dönüşümunu otomatikleştiren, tamamen yönetilen, bulut tabanlı, veri entegrasyonu ETL hizmetidir. Azure Veri Fabrikası, hammaddeleri bitmiş mallara dönüştürmek için ekipman çalıştıran bir fabrika gibi, ham veri toplayan ve kullanıma hazır bilgilere dönüştüren mevcut hizmetleri yönetir. 

Azure Veri Fabrikası'nı kullanarak, verileri şirket içi ve bulut veri depoları arasında taşımak için veri tabanlı iş akışları oluşturabilirsiniz. Ayrıca Veri Akışları ile verileri işleyebilir ve dönüştürebilirsiniz. ADF ayrıca Azure HDInsight, Azure Databricks ve SQL Server Integration Services (SSIS) tümleştirme çalışma zamanı gibi bilgi işlem hizmetlerini kullanarak el kodlu dönüşümler için harici bilgi işlem altyapılarını destekler. 

Veri Fabrikası ile veri işlemenizi Azure tabanlı bir bulut hizmetinde veya SSIS, SQL Server veya Oracle gibi kendi barındırılan bilgi işlem ortamınızda gerçekleştirebilirsiniz. İhtiyacınız olan eylemi gerçekleştiren bir ardışık düzenek oluşturduktan sonra, bu eylemi düzenli aralıklı olarak (örneğin saatlik, günlük veya haftalık), zaman penceresi zamanlaması veya bir olay oluşumundan gelen ardışık etki hattını tetikleecek şekilde zamanlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Data Factory'ye giriş](introduction.md).

### <a name="control-flows-and-scale"></a>Kontrol akışları ve ölçek 
Veri Fabrikası, modern veri ambarındaki çeşitli tümleştirme akışlarını ve desenlerini desteklemek için esnek veri boru hattı modellemesine olanak tanır. Bu, koşullu yürütme, veri ardışıklıklarında dallanma ve parametreleri bu akışlar içinde ve arasında açıkça geçirme olanağını içeren tam kontrol akışı programlama paradigmalarını içerir. Kontrol akışı, kopyalama etkinliği aracılığıyla, etkinlik gönderimi yoluyla verilerin dış yürütme motorlarına ve ölçekteki veri hareketi de dahil olmak üzere veri akışı yeteneklerine dönüştürülmesini de kapsar.

Veri Fabrikası, veri tümleştirmesi için gerekli olan ve isteğe bağlı olarak veya tekrar tekrar bir zamanlamayla gönderilebilen herhangi bir akış stilini modelleme özgürlüğü sağlar. Bu modelin etkinleştirmek için birkaç ortak akışları şunlardır:   

- Kontrol akışları:
    - Etkinlikler bir ardışık ardışık olarak zincirlenebilir.
    - Etkinlikler bir boru hattı içinde dallandırılabilir.
    - Parametreler:
        - Parametreler ardışık hatlar düzeyinde tanımlanabilir ve siz ardışık hattı isteğe bağlı olarak veya bir tetikleyiciden çağırırken bağımsız değişkenler geçirilebilir.
        - Etkinlikler işlem hattına geçirilen bağımsız değişkenleri kullanabilir.
    - Özel durum geçişi:
        - Durum da dahil olmak üzere etkinlik çıktıları, ardışık ardışık bir etkinlik tarafından tüketilebilir.
    - Döngü kapları:
        - Foreach etkinliği, bir döngüdeki belirli bir etkinlik koleksiyonu üzerinden yinelenecektir. 
- Tetik tabanlı akışlar:
    - Boru hatları isteğe bağlı olarak veya duvar saati saatinde tetiklenebilir.
- Delta akar:
    - Parametreler, verileri göle yüklemek için, şirket içinde veya bulutta, ilişkisel bir depodan boyut veya referans tabloları hareket ederken delta kopyalama için yüksek su işaretinizi tanımlamak için kullanılabilir. 

Daha fazla bilgi için [Bkz. Öğretici: Denetim akışları.](tutorial-control-flow.md)

### <a name="data-transformed-at-scale-with-code-free-pipelines"></a>Kodsuz boru hatlarıyla ölçekte dönüştürülen veriler
Yeni tarayıcı tabanlı araç deneyimi, kodsuz boru hattı yetkilendirme ve dağıtımını modern, etkileşimli web tabanlı bir deneyimle sağlar.

Görsel veri geliştiricileri ve veri mühendisleri için Veri Fabrikası web Arabirimi, ardışık hatlar oluşturmak için kullanacağınız kodsuz tasarım ortamıdır. Visual Studio Online Git ile tamamen entegre edilmiş ve hata ayıklama seçenekleriyle CI/CD ve yinelemeli geliştirme için entegrasyon sağlar.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>Gelişmiş kullanıcılar için zengin çapraz platform SDK'ları
Veri Fabrikası V2, en sevdiğiniz IDE'yi kullanarak ardışık hatları yazar, yönetmek ve izlemek için kullanılabilecek zengin bir SDK kümesi sağlar:
* Python SDK'sı
* PowerShell CLI
* C# SDK’sı

Kullanıcılar ayrıca Veri Fabrikası V2 ile arayüz için belgelenmiş REST API'lerini kullanabilir.

### <a name="iterative-development-and-debugging-by-using-visual-tools"></a>Görsel araçlar kullanarak yinelemeli geliştirme ve hata ayıklama
Azure Veri Fabrikası görsel araçları yinelemeli geliştirme ve hata ayıklama sağlar. Tek bir kod satırı yazmadan ardışık hatlar oluşturabilir ve boru hattı tuvalindeki **Hata Ayıklama** özelliğini kullanarak test çalıştırmaları yapabilirsiniz. Test çalışmalarınızın sonuçlarını, boru hattı tuvalinizin **Çıktı** penceresinde görüntüleyebilirsiniz. Test çalıştırmanız başarılı olduktan sonra, ardınıza daha fazla etkinlik ekleyebilir ve yinelemeli bir şekilde hata ayıklama devam edebilirsiniz. Ayrıca test çalışır devam ettikten sonra iptal edebilirsiniz. 

**Hata**Ayıklama'yı seçmeden önce değişikliklerinizi veri fabrikası hizmetinde yayımlamanız gerekmez. Bu, geliştirme, test veya üretim ortamlarında veri fabrikası iş akışlarınızı güncelleştirmeden önce yeni eklemelerin veya değişikliklerin beklendiği gibi çalışacağından emin olmak istediğiniz senaryolarda yararlıdır. 

### <a name="ability-to-deploy-ssis-packages-to-azure"></a>SSIS paketlerini Azure'a dağıtma olanağı 
SSIS iş yüklerinizi taşımak istiyorsanız, bir Veri Fabrikası oluşturabilir ve bir Azure-SSIS tümleştirme çalışma zamanı sağlayabilirsiniz. Azure-SSIS tümleştirme çalışma zamanı, SSIS paketlerinizi bulutta çalıştırmak için ayrılmış tam olarak yönetilen bir Azure VM kümesidir. Adım adım talimatlar için, [SSIS paketlerini Azure öğreticisine dağıtın' a](tutorial-create-azure-ssis-runtime-portal.md) bakın. 
 
### <a name="sdks"></a>SDK’lar
Gelişmiş bir kullanıcıysanız ve programlı bir arayüz arıyorsanız, Veri Fabrikası en sevdiğiniz IDE'yi kullanarak ardışık hatları yazar, yönetmek veya izlemek için kullanabileceğiniz zengin bir SDK kümesi sağlar. Dil desteği .NET, PowerShell, Python ve REST içerir.

### <a name="monitoring"></a>İzleme
Veri Fabrikalarınızı PowerShell, SDK veya tarayıcı kullanıcı arabirimindeki Görsel İzleme Araçları üzerinden izleyebilirsiniz. İsteğe bağlı, tetik tabanlı ve saat odaklı özel akışları etkin ve etkili bir şekilde izleyebilir ve yönetebilirsiniz. Varolan görevleri iptal edin, hataları bir bakışta görün, ayrıntılı hata iletileri almak için ayrıntıya inin ve bağlam değiştirmeden veya ekranlar arasında ileri geri gezinmeden tek bir cam bölmesinden sorunları hata ayıklayın. 

### <a name="new-features-for-ssis-in-data-factory"></a>Veri Fabrikasında SSIS için yeni özellikler
2017'deki ilk genel önizleme sürümünden bu yana, Data Factory SSIS için aşağıdaki özellikleri ekledi:

-    Projelerin/paketlerin SSIS veritabanını (SSISDB) barındırmak için Azure SQL Veritabanı'nın üç yapılandırması/varyantı için destek:
-    Sanal ağ hizmeti uç noktaları ile SQL Veritabanı
-    Yönetilen örnek
-    Elastik havuz
-    Gelecekte amortismana alınacak klasik bir sanal ağın üstündeki Azure Kaynak Yöneticisi sanal ağı desteği, Azure-SSIS tümleştirme çalışma sürenizi sanal ağ hizmeti bitiş noktaları/MI/şirket içi veri erişimi yle SQL Veritabanı için yapılandırılmış sanal bir ağa enjekte etmenizi/katılmanızı sağlar. Daha fazla bilgi için ayrıca bir [Azure-SSIS tümleştirme çalışma saatini sanal ağa katılın'](join-azure-ssis-integration-runtime-virtual-network.md)a bakın.
-    SSISDB'ye bağlanmak için Azure Active Directory (Azure AD) kimlik doğrulaması ve SQL kimlik doğrulaması desteği, Azure kaynakları için Veri Fabrikası yönetilen kimliğinizle Azure AD kimlik doğrulaması sağlar
-    Azure Karma Avantaj seçeneğinden önemli maliyet tasarrufu elde etmek için kendi şirket içi SQL Server lisansınızı getirme desteği
-    Gelişmiş/premium özellikleri, ek bileşenleri/uzantıları yüklemek için özel bir kurulum arabirimi ve iş ortağı ekosistemini kullanmanıza olanak tanıyan Azure-SSIS tümleştirme çalışma zamanının Enterprise Edition'ı için destek. Daha fazla bilgi için, [Ayrıca Enterprise Edition, Özel Kurulum ve ADF SSIS için 3 Parti Genişletilebilirlik](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/)bakın. 
-    Veri Fabrikası'nda SSIS'in daha derin entegrasyonu, Veri Fabrikası ardışık hatlarında birinci sınıf Execute SSIS Paketi etkinliklerini başlatmanızı/tetiklemenizi ve Bunları SSMS aracılığıyla zamanlamanızı sağlar. Daha fazla bilgi için, [ADF boru hatlarındaki SSIS faaliyetleriyle ETL/ELT iş akışlarınızı modernize](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)edin ve genişletin.


## <a name="what-is-the-integration-runtime"></a>Tümleştirme çalışma zamanı nedir?
Tümleştirme çalışma süresi, Azure Veri Fabrikası'nın çeşitli ağ ortamlarında aşağıdaki veri tümleştirme özelliklerini sağlamak için kullandığı bilgi işlem altyapısıdır:

- **Veri hareketi**: Veri hareketi için, tümleştirme çalışma zamanı, yerleşik bağlayıcılar, biçim dönüştürme, sütun eşleme ve performant ve ölçeklenebilir veri aktarımı için destek sağlarken, verileri kaynak ve hedef veri depoları arasında taşır.
- **Gönderme etkinlikleri**: Dönüştürme için, tümleştirme çalışma süresi SSIS paketlerini yerel olarak yürütme olanağı sağlar.
- **SSIS paketlerini yürütme**: Entegrasyon çalışma zamanı, SSIS paketlerini yönetilen bir Azure işlem ortamında doğal olarak yürütür. Tümleştirme çalışma süresi, Azure HDInsight, Azure Machine Learning, SQL Database ve SQL Server gibi çeşitli bilgi işlem hizmetlerinde çalışan dönüşüm etkinliklerini gönderme ve izleme yi de destekler.

Verileri taşımak ve dönüştürmek için gerektiğinde tümleştirme çalışma zamanının bir veya birkaç örneğini dağıtabilirsiniz. Tümleştirme çalışma süresi, Azure ortak ağında veya özel bir ağda (şirket içi, Azure Sanal Ağ veya Amazon Web Hizmetleri sanal özel bulut [VPC]) üzerinde çalıştırılabilir. 

Daha fazla bilgi için bkz. [Azure Data Factory'de tümleştirme çalışma zamanı](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Tümleştirme çalışma sürelerinin sınırı nedir?
Bir veri fabrikasında sahip olabileceğiniz tümleştirme çalışma zamanı örneklerinin sayısında sabit bir sınır yoktur. Ancak, tümleştirme çalışma süresinin SSIS paket yürütmesi için abonelik başına kullanabileceği VM çekirdek sayısı üzerinde bir sınır vardır. Daha fazla bilgi için [Bkz. Veri Fabrikası sınırları.](../azure-resource-manager/management/azure-subscription-service-limits.md#data-factory-limits)

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Azure Veri Fabrikası'nın üst düzey kavramları nelerdir?
Azure aboneliğinin bir veya birden çok Azure Data Factory örneği (veya veri fabrikası) olabilir. Azure Veri Fabrikası, verileri taşımak ve dönüştürmek için adımlarla veri tabanlı iş akışları oluşturabileceğiniz bir platform olarak birlikte çalışan dört temel bileşen içerir.

### <a name="pipelines"></a>İşlem hatları
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. Bir ardışık işlem birimi gerçekleştirmek için faaliyetlerin mantıksal bir gruplandırma. İşlem hattındaki etkinlikler birlikte bir görev gerçekleştirir. Örneğin, bir ardışık iş tonu, bir Azure blob'undan veri alan ve verileri bölmek için BIR HDInsight kümesinde Hive sorgusu çalıştıran bir etkinlik grubu içerebilir. Bunun yararı, her etkinliği ayrı ayrı yönetmek yerine etkinlikleri bir küme olarak yönetmek için bir ardışık etki hattı kullanabilirsiniz. Etkinlikleri sırayla çalıştırmak için bir ardışık olarak zincirleyebilir veya bağımsız olarak paralel olarak çalıştırabilirsiniz.

### <a name="data-flows"></a>Veri akışları
Veri akışları, Veri Fabrikası'nda görsel olarak oluşturduğunuz ve verileri arka uç Spark hizmetlerinde ölçekte dönüştüren nesnelerdir. Programlama veya Spark iç anlamak gerekmez. Grafikler (Haritalama) veya elektronik tabloları (Wrangling) kullanarak veri dönüştürme niyetinizi tasarlamanız yeterli.

### <a name="activities"></a>Etkinlikler
Etkinlikler bir işlem hattındaki işleme adımını temsil eder. Örneğin, verileri bir veri deposundan başka bir veri deposuna kopyalamak için Kopyalama etkinliğini kullanabilirsiniz. Benzer şekilde, verilerinizi dönüştürmek veya çözümlemek için Bir Azure HDInsight kümesinde Hive sorgusu çalıştıran bir Kovan etkinliği kullanabilirsiniz. Data Factory üç tür etkinliği destekler: veri taşıma etkinlikleri, veri dönüştürme etkinlikleri ve denetim etkinlikleri.

### <a name="datasets"></a>Veri kümeleri
Veri kümeleri, veri depoları içinde etkinliklerinizde giriş veya çıkış olarak kullanmak istediğiniz verilere işaret eden veya başvuruda bulunan veri yapılarını temsil eder. 

### <a name="linked-services"></a>Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Şu şekilde düşünün: Bağlantılı bir hizmet veri kaynağına olan bağlantıyı tanımlar ve veri kümesi verilerin yapısını temsil eder. Örneğin, Azure Depolama bağlantılı bir hizmet, Azure Depolama hesabına bağlanmak için bağlantı dizesini belirtir. Azure blob veri kümesi, blob kapsayıcısını ve verileri içeren klasörü belirtir.

Bağlantılı hizmetlerin Veri Fabrikası'nda iki amacı var:

- Şirket içi SQL Server örneği, Oracle veritabanı örneği, dosya paylaşımı veya Azure Blob depolama hesabı içeren ancak bununla sınırlı olmayan bir *veri deposunun* temsili. Desteklenen veri depolarının listesi için Azure [Veri Fabrikası'nda Etkinliği Kopyala'ya](copy-activity-overview.md)bakın.
- Bir etkinliğin yürütülmesini barındırabilecek bir *bilgi işlem kaynağını* temsil etmek. Örneğin, HDInsight Hive etkinliği bir HDInsight Hadoop kümesinde çalışır. Dönüşüm etkinlikleri ve desteklenen bilgi işlem ortamlarının listesi için Azure [Veri Fabrikası'nda verileri dönüştür'e](transform-data.md)bakın.

### <a name="triggers"></a>Tetikleyiciler
Tetikleyiciler, bir boru hattı yürütmesinin ne zaman devreye sayılmayan bir işlem birimini temsil eder. Farklı etkinlik türleri için farklı tetikleyici türleri vardır. 

### <a name="pipeline-runs"></a>İşlem hattı çalıştırmaları
Bir boru hattı çalıştırımı, bir boru hattı yürütme örneğidir. Genellikle bağımsız değişkenleri ardışık hatlar'da tanımlanan parametrelere geçirerek çalıştırılan bir ardışık Bağımsız değişkenleri el ile veya tetikleyici tanımı içinde geçirebilirsiniz.

### <a name="parameters"></a>Parametreler
Parametreler, salt okunur yapılandırmadaki anahtar değer çiftleridir.Bir ardışık ardışık alanda parametrelertanımlarsınız ve yürütme sırasında tanımlanan parametrelerin bağımsız değişkenlerini çalıştır bağlamından geçirirsiniz. Çalıştırılaç bağlamı bir tetikleyici veya el ile çalıştırdığınız bir ardışık kaynak tarafından oluşturulur. İşlem hattındaki etkinlikler parametre değerlerini kullanır.

Veri kümesi, güçlü bir şekilde yazılan bir parametre ve yeniden kullanabileceğiniz veya başvuruda bulunabileceğiniz bir varlıktır. Bir etkinlik veri kümeleri başvurur ve veri kümesi tanımında tanımlanan özellikleri tüketebilir.

Bağlantılı hizmet, aynı zamanda bir veri deposuna veya bilgi işlem ortamına bağlantı bilgilerini içeren güçlü bir şekilde yazılan bir parametredir. Aynı zamanda yeniden kullanabileceğiniz veya başvuruda bulunabileceğiniz bir varlıktır.

### <a name="control-flows"></a>Kontrol akışları
Denetim akışları, zincirleme etkinlikleri, dallanma, ardışık düzen düzeyinde tanımladığınız parametreler ve talep üzerine veya tetikleyiciden boru hattını çağırırken geçtiğiniz bağımsız değişkenleri içeren boru hattı etkinliklerini yönetir. Denetim akışları da özel durum geçiş ve döngü kapsayıcıları (yani, foreach yineleyiciler) içerir.


Data Factory kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md)
- [Boru hatları ve faaliyetleri](concepts-pipelines-activities.md)
- [Tümleştirme çalışma zamanı](concepts-integration-runtime.md)

## <a name="what-is-the-pricing-model-for-data-factory"></a>Veri Fabrikası için fiyatlandırma modeli nedir?
Azure Veri Fabrikası fiyatlandırma ayrıntıları için [Bkz. Veri Fabrikası fiyatlandırma ayrıntıları.](https://azure.microsoft.com/pricing/details/data-factory/)

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Veri Fabrikası hakkında nasıl bilgi ile güncel kalabilirim?
Azure Veri Fabrikası hakkında en güncel bilgiler için aşağıdaki sitelere gidin:

- [Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Dokümantasyon giriş sayfası](/azure/data-factory)
- [Ürün giriş sayfası](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Teknik derin dalış 

### <a name="how-can-i-schedule-a-pipeline"></a>Boru hattını nasıl ayarlayabilirim? 
Bir ardışık hatlar zamanlamak için zamanlayıcı tetikleyicisini veya zaman penceresi tetikleyicisini kullanabilirsiniz. Tetikleyici, ardışık hatları düzenli aralıklarla veya takvim tabanlı yinelenen desenlerde (örneğin, Pazartesi günleri 18:00'de ve Perşembe günleri saat 21:00'de) zamanlayabilirsiniz bir duvar saati takvim çizelgesi kullanır. Daha fazla bilgi için bkz. [İşlem hattı yürütme ve tetikleyiciler](concepts-pipeline-execution-triggers.md).

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>Parametreleri boru hattı çalışmasına geçirebilir miyim?
Evet, parametreler Veri Fabrikası'nda birinci sınıf, üst düzey bir kavramdır. İstem üzerine veya tetikleyici kullanarak boru hattı çalıştırırken parametreleri ardışık hatlar düzeyinde tanımlayabilir ve bağımsız değişkenleri geçirebilirsiniz.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Boru hattı parametreleri için varsayılan değerleri tanımlayabilir miyim? 
Evet. Denetim hatlarındaki parametreler için varsayılan değerleri tanımlayabilirsiniz. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Bir ardışık ardışık alandaki bir etkinlik, bir boru hattı çalıştırın çalıştırılabilen bağımsız değişkenleri tüketebilir mi? 
Evet. Ardışık etki alanındaki her etkinlik, ardışık ardışık yapıya geçirilen `@parameter` parametre değerini tüketebilir ve yapıyla birlikte çalıştırılabilir. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Bir etkinlik çıktı özelliği başka bir etkinlikte tüketilebilir mi? 
Evet. Bir etkinlik `@activity` çıktısı, yapıyla birlikte sonraki bir etkinlikte tüketilebilir.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Bir etkinlik çıkışında null değerlerini zarif bir şekilde nasıl işleyebilirim? 
Null değerlerini `@coalesce` düzgün bir şekilde işlemek için ifadelerdeki yapıyı kullanabilirsiniz. 

## <a name="mapping-data-flows"></a>Veri akışlarını eşleme

### <a name="i-need-help-troubleshooting-my-data-flow-logic-what-info-do-i-need-to-provide-to-get-help"></a>Veri akışı mantığımı gidermek için yardıma ihtiyacım var. Yardım almak için hangi bilgileri sağlamam gerekiyor?

Microsoft veri akışlarıyla ilgili yardım veya sorun giderme sağladığında, lütfen Veri Akışı Komut Dosyasını sağlayın. Bu, veri akışı grafiğinizdeki kod arkasındaki komut dosyasıdır. ADF UI'den veri akışınızı açın ve sağ üst köşedeki "Script" düğmesini tıklatın. Bu komut dosyasını kopyalayıp yapıştırın veya bir metin dosyasına kaydedin.

### <a name="how-do-i-access-data-by-using-the-other-90-dataset-types-in-data-factory"></a>Veri Fabrikası'ndaki diğer 90 veri kümesi türünü kullanarak verilere nasıl erişebilirim?

Eşleme veri akışı özelliği şu anda Azure SQL Veritabanı, Azure SQL Veri Ambarı, Azure Blob depolama veya Azure Veri Gölü Depolama Gen2 metin dosyaları ve Kaynak ve lavabo için yerel olarak Blob depolama veya Data Lake Storage Gen2 parke dosyaları sınırlıdır sağlar. 

Diğer bağlayıcılardan herhangi birinden veri sahnelemek için Kopyala etkinliğini kullanın ve sonra veri sahnelendikten sonra verileri dönüştürmek için bir Veri Akışı etkinliğini çalıştırın. Örneğin, ardınız önce Blob depolamaalanına kopyalanır ve ardından veri akışı etkinliği bu verileri dönüştürmek için kaynaktaki bir veri kümesini kullanır.

### <a name="is-the-self-hosted-integration-runtime-available-for-data-flows"></a>Kendi kendine barındırılan tümleştirme çalışma zamanı veri akışları için kullanılabilir mi?

Kendi kendine barındırılan IR, kopya etkinliğiyle birlikte kullanarak verileri ön hazırlık veya VM tabanlı veri kaynaklarına ve lavabolara ve verilere ve verilere aktarabileceğiniz bir ADF ardışık yapıdır. Verileri önce Kopya, sonra dönüşüm için Veri Akışı ve sonra da dönüştürülmüş verileri ön depoya geri taşımanız gerekiyorsa sonraki kopyayla aşama.

### <a name="does-the-data-flow-compute-engine-serve-multiple-tenants"></a>Veri akışı bilgi işlem motoru birden fazla kiracıya hizmet veriyor mu?
Kümeler hiçbir zaman paylaşılmaz. Üretim serilerinde çalışan her iş için izolasyon garantisi alıyoruz. Hata ayıklama senaryosu durumunda bir kişi bir küme alır ve tüm hata ayıklamaları bu kullanıcı tarafından başlatılan kümeye gider.

## <a name="wrangling-data-flows"></a>Çekişmeli veri akışları

### <a name="what-are-the-supported-regions-for-wrangling-data-flow"></a>Veri akışı için desteklenen bölgeler nelerdir?

Çekişmeli veri akışı şu anda aşağıdaki bölgelerde oluşturulan veri fabrikalarında desteklenir:

* Doğu Avustralya
* Orta Kanada
* Orta Hindistan
* Doğu ABD
* Doğu ABD 2
* Doğu Japonya
* Kuzey Avrupa
* Güneydoğu Asya
* Orta Güney ABD
* Güney Birleşik Krallık
* Orta Batı ABD
* Batı Avrupa
* Batı ABD
* Batı ABD 2

### <a name="what-are-the-limitations-and-constraints-with-wrangling-data-flow"></a>Çekişmeli veri akışıyla ilgili sınırlamalar ve kısıtlamalar nelerdir?

Dataset adları yalnızca alfa sayısal karakterler içerebilir. Aşağıdaki veri depoları desteklenir:

* Hesap anahtarı kimlik doğrulamasını kullanarak Azure Blob Depolama'da Sınırlı Metin veri kümesi
* Hesap anahtarı veya hizmet temel kimlik doğrulaması kullanarak Azure Veri Gölü Depolama gen2'deki Sınırlı Metin veri kümesi
* Hizmet temel kimlik doğrulamasını kullanarak Azure Veri Gölü Depolama gen1'inde Sınırlı Metin veri kümesi
* Sql kimlik doğrulaması kullanarak Azure SQL Veritabanı ve Veri Ambarı. Aşağıdaki desteklenen SQL türlerine bakın. Veri ambarı için PolyBase veya hazırlama desteği yoktur.

Şu anda, bağlantılı hizmet Key Vault tümleştirmesi çekişmeli veri akışlarında desteklenmez.

### <a name="what-is-the-difference-between-mapping-and-wrangling-data-flows"></a>Haritalama ve çekişmeli veri akışları arasındaki fark nedir?

Veri akışlarını eşleme, herhangi bir kodlama gerektirmeden verileri ölçekte dönüştürmenin bir yolunu sağlar. Bir dizi dönüşüm oluşturarak veri akışı tuvalinde bir veri dönüştürme işi tasarlayabilirsiniz. Veri dönüştürme adımlarını izleyen herhangi bir sayıda kaynak dönüşümüyle başlayın. Sonuçlarınızı bir hedefe çıkarmak için veri akışınızı bir lavabo ile tamamlayın. Veri akışını nakışlamak, lavabolarda ve kaynaklarda hem bilinen hem de bilinmeyen şemalarla veri eşleme ve dönüştürme de harikadır.

Wrangling veri akışları kıvılcım yürütme yoluyla ölçekte Power Query Online mashup düzenleyici kullanarak çevik veri hazırlama ve arama yapmak için izin verir. Veri göllerinin yükselişi ile bazen sadece bir veri kümesi keşfetmek veya gölde bir veri kümesi oluşturmak gerekir. Bilinen bir hedefin haritasını çıkarmadın. Çekişmeli veri akışları daha az resmi ve model tabanlı analiz senaryoları için kullanılır.

### <a name="what-is-the-difference-between-power-platform-dataflows-and-wrangling-data-flows"></a>Güç Platformu Veri Akışları ile çekişmeli veri akışları arasındaki fark nedir?

Power Platform Dataflows, kullanıcıların PowerApps uygulamaları, Power BI raporları veya Akış otomasyonları oluşturmak için çok çeşitli veri kaynaklarından ortak veri hizmetine ve Azure Veri Gölü'ne veri almalarına ve dönüştürmelerine olanak tanır. Power Platform Dataflows, Power BI ve Excel'e benzer şekilde kurulan Power Query veri hazırlama deneyimlerini kullanır. Power Platform Dataflows ayrıca bir kuruluş içinde kolay yeniden kullanıma olanak sağlar ve orkestrasyonu otomatik olarak işler (örn. eskisi yenilendiğinde başka bir veri akışına bağlı olan veri akışları otomatik olarak yenilenir).

Azure Veri Fabrikası (ADF), veri mühendislerinin ve vatandaş veri entegratörüne karmaşık karma ayıklama dönüştürme yükü (ETL) ve ayıklama-yük dönüştürme (ELT) iş akışları oluşturma olanağı sağlayan yönetilen bir veri tümleştirme hizmetidir. ADF'deki veri akışı, buluttaki veri hazırlamayı kolaylaştıran ve altyapı yönetimi gerektirmeden herhangi bir veri boyutuna ölçeklendiren, kodsuz, sunucusuz bir ortamla kullanıcılara güç verir. Verileri hazırlamak ve şekillendirmek için Power Query veri hazırlama teknolojisini (Power Platform veri akışlarında, Excel, Power BI'de de kullanılır) kullanır. Büyük veri entegrasyonunun tüm karmaşıklıklarını ve ölçek zorluklarını işlemek için tasarlanan veri akışları, kullanıcıların kıvılcım yürütme yoluyla hızlı bir şekilde verileri ölçekte hazırlamasına olanak sağlar. Kullanıcılar tarayıcı tabanlı arayüzümüzle erişilebilir bir görsel ortamda esnek veri ardışık hatları oluşturabilir ve ADF'nin Spark yürütmesinin karmaşıklığını ele almasına izin verebilir. Ardışık hatlar için zamanlamalar oluşturun ve Veri akışı yürütmelerinizi ADF izleme portalından izleyin. ADF'nin zengin kullanılabilirlik izleme ve uyarılarıyla veri kullanılabilirliği SLA'larını kolayca yönetin ve akışlarınızı yönetilen bir ortamda kaydetmek ve yönetmek için yerleşik sürekli tümleştirme ve dağıtım özelliklerinden yararlanın. Veri akışlarınızı ayarlarken mantığınızın planlandığı gibi performans gösterdiğini doğrulamak için uyarılar belirleyin ve yürütme planlarını görüntüleyin.

### <a name="supported-sql-types"></a>Desteklenen SQL Türleri

Wrangling veri akışı SQL'de aşağıdaki veri türlerini destekler. Desteklenmeyen bir veri türünü kullandığınız için bir doğrulama hatası alırsınız.

* short
* double
* gerçek
* float
* char
* Nchar
* varchar
* nvarchar
* integer
* int
* bit
* boole
* smallint
* tinyint
* bigint
* long
* metin
* date
* datetime
* datetime2
* Smalldatetime
* timestamp
* uniqueidentifier
* xml

Diğer veri türleri gelecekte desteklenir.

## <a name="next-steps"></a>Sonraki adımlar
Bir veri fabrikası oluşturmak için adım adım yönergeler için aşağıdaki öğreticilere bakın:

- [Quickstart: Veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md)
- [Öğretici: Verileri bulutta kopyalama](tutorial-copy-data-dot-net.md)
