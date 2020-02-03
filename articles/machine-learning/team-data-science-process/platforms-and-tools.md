---
title: Platform ve veri bilimi projeleri - Team Data Science Process için Araçlar
description: Maddeler halinde listeler ve üzerinde Team Data Science Process Standartlaştırma kuruluşların kullanabileceği veri ve analiz kaynaklarını ele alır.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: e3297319c67ad2b7c94371356cde49113c7ef737
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76718897"
---
# <a name="platforms-and-tools-for-data-science-projects"></a>Platformlar ve araçlar için veri bilimi projeleri

Microsoft, hem bulut hem de şirket içi platformlar için eksiksiz bir analiz kaynakları yelpazesi sağlar. Veri bilimi projelerinizi yürütülmesini etkili ve ölçeklenebilir hale getirmek için dağıtılabilir. Veri bilimi projelerini uygulayan takımlar için bir izleme, sürüm denetimli ve işbirliğine dayalı bir yöntem, [ekip veri bilimi işlemi](overview.md) (TDSP) tarafından sağlanır.  Personel rollerinin bir özeti ve bir veri bilimi ekibi tarafından işlenen ilişkili görevleri bu işlemi gerçekleştirirken, bkz. [Team Data Science işlem rolleri ve görevleri](roles-tasks.md).

TDSP kullanan veri bilimi ekipleri için kullanılabilen analiz kaynakları şunları içerir:

- Veri bilimi sanal makineleri (Windows ve Linux CentOS)
- HDInsight Spark kümeleri
- Synapse Analytics
- Azure Data Lake
- HDInsight Hive kümeleri
- Azure Dosya Depolama
- SQL Server 2019 R ve Python Hizmetleri
- Azure Databricks

Bu belgede, size kısaca kaynaklar açıklanır ve öğreticiler ve Kılavuzlar TDSP takımlar yayımladıktan bağlantılar sağlar. Bunlar, bunları adım adım kullanın ve bunları, akıllı uygulamalar oluşturmak için kullanmaya başlama öğrenmenize yardımcı olabilir. Bu kaynaklar hakkında daha fazla bilgi ürün sayfalarında kullanılabilir. 

## <a name="data-science-virtual-machine-dsvm"></a>Veri bilimi sanal makinesi (DSVM)

Veri bilimi sanal makinesi hem Windows hem de Linux, Microsoft tarafından sunulan, veri bilimi modelleme ve geliştirme etkinlikleri için popüler araçların yer. Gibi araçları içerir:

- Microsoft R Server Geliştirici sürümü 
- Anaconda Python dağıtım
- Python ve R için Jupyter Not Defterleri 
- Visual Studio Community sürümüyle Python ve R araçları Windows / Linux üzerinde Eclipse
- Windows için Power BI desktop
- SQL Server 2016 Developer Edition Windows / Linux üzerinde Postgres

Ayrıca, xgboost, mxnet ve Vowpal Wabbit gibi **ml ve AI araçları** da içerir.

Şu anda DSVM, **Windows** ve **Linux CentOS** işletim sistemlerinde kullanılabilir. Üzerinde yürütmek için planlama veri bilimi projeleri gereksinimlerine göre DSVM (CPU çekirdek sayısı) ve bellek boyutunu seçin. 

DSVM 'nin Windows sürümü hakkında daha fazla bilgi için bkz. Azure Marketi 'nde [Microsoft veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) . DSVM 'nin Linux sürümü için bkz. [linux veri bilimi sanal makinesi](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

DSVM 'de ortak veri bilimi görevlerinin bazılarının nasıl yürütüleceğini öğrenmek için bkz. [veri bilimi sanal makinesi üzerinde yapabileceğiniz 10 şey](../data-science-virtual-machine/vm-do-ten-things.md)


## <a name="azure-hdinsight-spark-clusters"></a>Azure HDInsight Spark kümeleri

Apache Spark, büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen altyapısıdır bir açık kaynak paralel olur. Spark işleme altyapısı hız, kullanım kolaylığı ve Gelişmiş analiz için oluşturulmuştur. Spark'ın bellek içi hesaplama özellikleri onu ve grafik hesaplamaları machine learning'de yinelemeli algoritmalar için iyi bir seçim haline getirir. Azure'da depolanan mevcut verilerinizi kolayca Spark kullanarak işleyebileceğiniz şekilde Spark Azure Blob Depolama (WASB) ile de uyumludur.

HDInsight’ta Spark kümesi oluşturduğunuzda, Spark yüklenmiş ve yapılandırılmış olarak Azure işlem kaynakları oluşturursunuz. HDInsight Spark kümesi oluşturmak için yaklaşık 10 dakika sürer. Azure Blob Depolama alanında işlenmek üzere data Store. Azure Blob depolamayı bir kümeyle kullanma hakkında daha fazla bilgi için bkz. [HDInsight 'Ta Hadoop ile uyumlu Azure Blob depolamayı kullanma](../../hdinsight/hdinsight-hadoop-use-blob-storage.md).

TDSP ekibinden Microsoft Veri bilimi çözümleri oluşturmak için Azure HDInsight Spark kümeleri kullanma hakkında iki uçtan uca izlenecek yollar yayımlanan bir Python ve diğer Scala kullanarak. Azure HDInsight **Spark kümeleri**hakkında daha fazla bilgi için bkz. [genel bakış: hdınsight Linux üzerinde Apache Spark](../../hdinsight/spark/apache-spark-overview.md). Azure HDInsight Spark kümesinde **Python** kullanarak veri bilimi çözümü oluşturmayı öğrenmek için bkz. [Azure HDInsight 'Ta Spark kullanarak veri bilimine genel bakış](spark-overview.md). Azure HDInsight Spark kümesinde **Scala** kullanarak bir veri bilimi çözümü oluşturmayı öğrenmek için bkz. [Azure 'da Scala ve Spark kullanan veri bilimi](scala-walkthrough.md). 


##  <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

Azure SQL veri ambarı, işlem kaynakları fazladan sağlama veya gereksiz ödeme yapmadan kolayca ve saniye cinsinden ölçeklendirmenize olanak tanır. Ayrıca, bulut maliyetlerinizi daha iyi yönetmek için özgürlüğünü işlem kaynaklarının kullanımı, duraklatmak için benzersiz seçeneği sunar. Ölçeklenebilir işlem kaynakları dağıtma olanağı, Azure SQL veri ambarı'na tüm verilerinizi getirin mümkün kılar. En düşük depolama maliyetleri ve analiz etmek istediğiniz veri kümeleri yalnızca kısımlarına işlem çalıştırabilirsiniz. 

Azure SQL veri ambarı hakkında daha fazla bilgi için bkz. [SQL veri ambarı](https://azure.microsoft.com/services/sql-data-warehouse) Web sitesi. SQL veri ambarı ile uçtan uca gelişmiş analiz çözümleri oluşturmayı öğrenmek için, bkz [: SQL veri ambarı 'nı kullanma](sqldw-walkthrough.md).


## <a name="azure-data-lake"></a>Azure Data Lake

Azure Data Lake, tek bir konumda toplanan tüm veri türlerinin kurumsal çapta bir deposudur, herhangi bir biçimsel gereksinimden önce veya uygulanan şemadan oluşur. Her türde verinin boyutu ya da yapı bağımsız olarak bir veri gölü veya ne kadar hızlı içe alındığından tutulması için bu esneklik sağlar. Kuruluşlar, ardından Hadoop kullanabilir veya bulmak için Gelişmiş analiz bu veri gölleri desen. Veri gölleri veri puanlamalar ve veri ambarı'na taşımadan önce daha düşük maliyetli veri hazırlığı için bir depo görebilir.

Azure Data Lake hakkında daha fazla bilgi için bkz. [tanıtımı Azure Data Lake](https://azure.microsoft.com/blog/introducing-azure-data-lake/). Azure Data Lake ile ölçeklenebilir bir uçtan uca veri bilimi çözümü oluşturmayı öğrenmek için bkz [. Azure Data Lake Içinde ölçeklenebilir veri bilimi: uçtan uca bir anlatım](data-lake-walkthrough.md)


## <a name="azure-hdinsight-hive-hadoop-clusters"></a>Azure HDInsight Hive (Hadoop) kümesi

Apache Hive bir veri ambarı için veri özetleme, sorgulama ve HiveQL, SQL'e benzer bir sorgu dili kullanarak veri analizi sağlayan, Hadoop sistemidir. Hive, verilerinizin etkileşimli olarak keşfetmek için ya da yeniden kullanılabilir toplu işleme işleri oluşturmak için kullanılabilir.

Hive, büyük ölçüde yapılandırılmamış veriler üzerinde Proje yapısı sağlar. Yapı tanımladıktan sonra kullanın veya bile biliyorsanız, Java veya MapReduce zorunda kalmadan bir Hadoop kümesi, verileri sorgulamak için Hive'ı kullanabilirsiniz. HiveQL (Hive sorgu dili), T-SQL benzer ifadelerle sorgular yazmaya olanak tanır.

Veri uzmanları için Hive Python User-Defined işlevler (UDF'ler) kayıtlarını işlemek için Hive sorguları çalıştırabilirsiniz. Bu özelliği, veri analizi Hive sorguları yeteneğini önemli ölçüde genişletir. Özellikle, bunlar çoğunlukla ile tanıdık dillerde ölçeklenebilir özellik Mühendisliği yürütmek veri bilimcilerine izin verir: Python ve SQL benzeri HiveQL. 

Azure HDInsight Hive kümeleri hakkında daha fazla bilgi için bkz. [HDInsight 'Ta Hadoop Ile Hive ve HiveQL kullanma](../../hdinsight/hadoop/hdinsight-use-hive.md). Azure HDInsight Hive kümeleriyle ölçeklenebilir uçtan uca bir veri bilimi çözümü oluşturmayı öğrenmek için bkz. [Team Data Science Process ın, HDInsight Hadoop kümelerini kullanma](hive-walkthrough.md).


## <a name="azure-file-storage"></a>Azure Dosya Depolama 

Azure dosya depolama standart sunucu ileti bloğu (SMB) protokolünü kullanarak bulutta dosya paylaşımları sağlayan bir hizmettir. SMB 2.1 ve SMB 3.0 desteklenir. Azure File Storage, Azure’a dosya paylaşımı kullanan eski uygulamaları maliyetli yeniden yazdırmaya ihtiyaç duymadan ve hızla taşıyabilmenizi sağlar. Azure Virtual Machines’de, Cloud Services’da veya şirket içi istemcilerde çalışan uygulamalar, bir masaüstü uygulamanın tipik SMB paylaşımı bağladığı gibi buluta bir dosya paylaşımı bağlayabilir. Ardından herhangi sayıda uygulama bileşeni eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

Proje verileri proje takım üyelerinizle paylaşın yer olarak bir Azure dosya depolama oluşturma özelliği, veri bilimi projeleri için özellikle yararlıdır. Her birinin ardından verilerin aynı kopyasını Azure dosya depolama alanına erişebilir. Bunlar ayrıca, bu dosya depolama proje yürütülmesi sırasında oluşturulan özellik kümeleri paylaşmak için kullanabilirsiniz. Bir istemci engagement projesiyse istemcilerinize özellikler ve proje verilerini sizinle paylaşmak için kendi Azure aboneliği kapsamında bir Azure dosya depolama oluşturabilirsiniz. Bu şekilde, istemci projesi veri varlıklarını tam denetime sahiptir. Azure dosya depolama hakkında daha fazla bilgi için bkz. [Windows 'Ta Azure dosya depolama ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files) ve [Linux Ile Azure dosya depolamayı kullanma](../../storage/files/storage-how-to-use-files-linux.md).


## <a name="sql-server-2019-r-and-python-services"></a>SQL Server 2019 R ve Python Hizmetleri

R Services (veritabanı Içi), yeni Öngörüler açığa çıkarmak için akıllı uygulamalar geliştirmeye ve dağıtmaya yönelik bir platform sağlar. R topluluk tarafından sağlanan birçok paketleri de dahil olmak üzere zengin ve güçlü R dil modelleri oluşturun ve SQL Server verilerinizden Öngörüler oluşturmak için kullanabilirsiniz. R Services (veritabanı Içi) R dilini SQL Server ile tümleştirdiğinden, analiz verileri yakın tutulur; bu da verileri taşıma ile ilişkili maliyetleri ve güvenlik risklerini ortadan kaldırır.

R Services (veritabanı Içi), kapsamlı bir SQL Server araçları ve teknolojileri kümesiyle açık kaynak R dilini destekler. Bunlar, üstün performans, güvenlik, güvenilirlik ve yönetilebilirlik sunar. Kolay ve bilindik araçları kullanarak R çözümleri dağıtabilirsiniz. Üretim uygulamalarınızı, R çalışma çağırın ve Öngörüler ve Transact-SQL kullanarak görselleri alma. Ayrıca R çözümlerinizin performansı ve ölçeği artırmak için ScaleR kitaplıkları kullanın. Daha fazla bilgi için bkz. [SQL Server R Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

TDSP takım Microsoft SQL Server 2016 R Services veri bilimi çözümlerini oluşturmak nasıl gösteren iki uçtan uca izlenecek yollar yayımladığı: biri R programcılarının, diğeri SQL geliştiricileri için. **R programcıları**için bkz. [veri bilimi uçtan uca izlenecek yol](https://docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough). **SQL geliştiricileri**için bkz. [SQL geliştiricileri Için veritabanı içi gelişmiş analiz (öğretici)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers).


## <a name="appendix"></a>Ek: veri bilimi projelerini ayarlamaya yönelik araçlar

### <a name="install-git-credential-manager-on-windows"></a>Windows üzerinde Git kimlik bilgisi Yöneticisi'ni yükleyin

**Windows**üzerinde TDSP 'yi takip ediyorsanız, git depoları ile iletişim kurmak Için **Git kimlik bilgileri YÖNETICISI 'ni (GCM)** yüklemeniz gerekir. GCM 'yi yüklemek için önce **chocolaty**yüklemeniz gerekir. Chocolaty ve GCM 'yi yüklemek için Windows PowerShell 'de **yönetici**olarak aşağıdaki komutları çalıştırın:  

    iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
    choco install git-credential-manager-for-windows -y
    

### <a name="install-git-on-linux-centos-machines"></a>(CentOS) Linux makinelerinde Git yükleme

(CentOS) Linux makinelerinde Git'i yüklemek için aşağıdaki bash komutu çalıştırın:

    sudo yum install git


### <a name="generate-public-ssh-key-on-linux-centos-machines"></a>(CentOS) Linux makinelerinde genel SSH anahtarı oluşturma

Git komutlarını çalıştırmak için Linux (CentOS) makineleri kullanıyorsanız, bu makine Azure DevOps Hizmetleri tarafından değerlendirilmiştir. böylece, makinenizin ortak SSH anahtarının, Azure DevOps hizmetlerinizi eklemeniz gerekir. İlk olarak, bir ortak SSH anahtarını oluşturun ve SSH ortak anahtarları, Azure DevOps Hizmetleri Güvenlik ayar sayfasında anahtarın eklemek gerekir. 

1. SSH anahtarı oluşturmak için aşağıdaki iki komutu çalıştırın: 

   ```
   ssh-keygen
   cat .ssh/id_rsa.pub
   ```
   
   ![SSH anahtarı oluşturmak için komutları](./media/platforms-and-tools/resources-1-generate_ssh.png)

1. SSH *-RSA*dahil tüm SSH anahtarını kopyalayın. 
1. Azure DevOps hizmetleriniz için oturum açın. 
1. Sayfanın sağ üst köşesindeki **adınızı\><** tıklayın ve **güvenlik**' e tıklayın. 
    
   ![Adınıza tıklayın ve ardından güvenlik öğesini tıklatın](./media/platforms-and-tools/resources-2-user-setting.png)

1. **SSH ortak anahtarlar**' a tıklayın ve **+ Ekle**' ye tıklayın. 

   ![SSH ortak anahtarları ve ardından tıklama + Ekle'e tıklayın](./media/platforms-and-tools/resources-3-add-ssh.png)

1. Metin kutusuna kopyalanmış olan SSH anahtarını yapıştırın ve kaydedin.


## <a name="next-steps"></a>Sonraki adımlar

**Belirli senaryolar** için işlemdeki adımların tümünü gösteren tam uçtan uca izlenecek yollar da sağlanmaktadır. Bunlar listelenmiş ve [örnek izlenecek yollar](walkthroughs.md) konusunun küçük resim açıklamalarıyla bağlantılandırılır. Bunlar, bulut, şirket içi araçları ve Hizmetleri, bir iş akışı veya akıllı bir uygulama oluşturmak için işlem hattı birleştirme işlemini göstermektedir. 

Azure Machine Learning Studio (klasik) kullanarak takım veri bilimi Işleminde adımların nasıl yürütüleceğini gösteren örnekler için, bkz. [Azure ML](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) öğrenme yolu.
