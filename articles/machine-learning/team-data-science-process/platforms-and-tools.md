---
title: Veri bilimi projeleri için platformlar ve araçlar - Ekip Veri Bilimi Süreci
description: Ekip Veri Bilimi Süreci'nde standartlaştırılabilen işletmelerin mevcut veri ve analiz kaynaklarını itemizes ve tartışır.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: d26dc55cfd0f2ef0bb78e4153fffe3f1cb910b73
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80474877"
---
# <a name="platforms-and-tools-for-data-science-projects"></a>Veri bilimi projeleri için platformlar ve araçlar

Microsoft, hem bulut hem de şirket içi platformlar için tam bir analiz kaynağı yelpazesi sağlar. Bunlar, veri bilimi projelerinizin yürütülmesini verimli ve ölçeklenebilir hale getirmek için dağıtılabilir. Veri bilimi projelerini izlenebilir, sürüm kontrollü ve işbirliğine dayalı bir şekilde uygulayan ekipler için kılavuz, [Team Data Science Process](overview.md) (TDSP) tarafından sağlanır.  Personel rollerinin anahat ve bu süreci standartlaştıran bir veri bilimi ekibi tarafından işlenen ilişkili görevleri [için, Takım Veri Bilimi Süreci rolleri ve görevleri](roles-tasks.md)bölümüne bakın.

TDSP'yi kullanan veri bilimi ekiplerinin kullanabileceği analiz kaynakları şunlardır:

- Veri Bilimi Sanal Makineler (hem Windows hem de Linux CentOS)
- HDInsight Kıvılcım Kümeleri
- Synapse Analytics
- Azure Data Lake
- HDInsight Kovan Kümeleri
- Azure Dosya Depolama
- SQL Server 2019 R ve Python Hizmetleri
- Azure Databricks

Bu belgede, kaynakları kısaca açıklar ve TDSP ekiplerinin yayımladıkları öğreticilere ve izlerle bağlantılar sağlarız. Bunları adım adım kullanmayı öğrenmenize ve akıllı uygulamalarınızı oluşturmak için kullanmaya başlamanıza yardımcı olabilir. Bu kaynaklar hakkında daha fazla bilgi ürün sayfalarında mevcuttur. 

## <a name="data-science-virtual-machine-dsvm"></a>Veri Bilimi Sanal Makine (DSVM)

Microsoft tarafından hem Windows hem de Linux'ta sunulan veri bilimi sanal makinesi, veri bilimi modelleme ve geliştirme etkinlikleri için popüler araçlar içerir. Bu gibi araçlar içerir:

- Microsoft R Server Developer Edition 
- Anaconda Python dağıtımı
- Python ve R için Jupyter dizüstü bilgisayarlar 
- Windows / Eclipse Linux üzerinde Python ve R Araçları ile Visual Studio Community Edition
- Windows için Power BI masaüstü
- WINDOWS'da SQL Server 2016 Developer Edition / Linux'ta Postgres

Ayrıca xgboost, mxnet ve Vowpal Wabbit gibi ML ve **AI araçları** içerir.

Şu anda DSVM **Windows** ve **Linux CentOS** işletim sistemlerinde kullanılabilir. Üzerinde yürütmeyi planladığınız veri bilimi projelerinin gereksinimlerine göre DSVM'nizin boyutunu (CPU çekirdeği sayısı ve bellek miktarı) seçin. 

DSVM'nin Windows sürümü hakkında daha fazla bilgi için Azure Marketi'ndeki [Microsoft Data Science Virtual Machine](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-win-2019) bölümüne bakın. DSVM Linux sürümü için, [Linux Veri Bilimi Sanal Makine](https://azure.microsoft.com/marketplace/apps/microsoft-dsvm.linux-data-science-vm-ubuntu)bakın.

DSVM'deki bazı yaygın veri bilimi görevlerini niçin verimli bir şekilde yürütebileceğinizi öğrenmek için, [Veri bilimi Sanal Makine'de yapabileceğiniz 10 şeye](../data-science-virtual-machine/vm-do-ten-things.md) bakın


## <a name="azure-hdinsight-spark-clusters"></a>Azure HDInsight Spark kümeleri

Apache Spark, Büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen açık kaynaklı bir paralel işleme altyapısıdır. Spark işleme motoru hız, kullanım kolaylığı ve gelişmiş analizler için üretilmiştir. Spark'ın bellek içi hesaplama yetenekleri, makine öğreniminde ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçim dir. Spark, Azure Blob depolama alanıyla (WASB) da uyumludur, böylece Azure'da depolanan mevcut verileriniz Spark kullanılarak kolayca işlenebilir.

HDInsight’ta Spark kümesi oluşturduğunuzda, Spark yüklenmiş ve yapılandırılmış olarak Azure işlem kaynakları oluşturursunuz. HDInsight'ta bir Kıvılcım kümesi oluşturmak yaklaşık 10 dakika sürer. İşlenecek verileri Azure Blob depolama alanında saklayın. Azure Blob Depolama'yı bir kümeyle kullanma hakkında daha fazla bilgi için, [HDInsight'ta Hadoop ile HDFS uyumlu Azure Blob depolama alanını kullanın'a](../../hdinsight/hdinsight-hadoop-use-blob-storage.md)bakın.

Microsoft'tan TDSP ekibi, biri Python ve diğeri Scala kullanılarak veri bilimi çözümleri oluşturmak için Azure HDInsight Spark Kümelerinin nasıl kullanılacağı hakkında iki uçtan uca izlik yayımladı. Azure HDInsight **Spark Kümeleri**hakkında daha fazla bilgi için genel [bakış: HDInsight Linux'ta Apache Spark'](../../hdinsight/spark/apache-spark-overview.md)a bakın. Azure HDInsight Spark Kümesi'nde **Python'u** kullanarak veri bilimi çözümü oluşturmayı öğrenmek için [Azure HDInsight'ta Kıvılcım kullanarak Veri Bilimine Genel Bakış](spark-overview.md)bölümüne bakın. Azure HDInsight Spark Cluster'da **Scala'yı** kullanarak veri bilimi çözümü oluşturmayı öğrenmek için [Azure'da Scala ve Spark'ı kullanarak Veri Bilimi'ne](scala-walkthrough.md)bakın. 


##  <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

Azure SQL Veri Ambarı, hesaplama kaynaklarını aşırı sağlama veya aşırı ödeme yapmadan kolayca ve saniyeler içinde ölçeklendirmenize olanak tanır. Ayrıca, bilgi işlem kaynaklarının kullanımını duraklatmak için benzersiz bir seçenek sunarak bulut maliyetlerinizi daha iyi yönetmenize izin verir. Ölçeklenebilir bilgi işlem kaynaklarını dağıtma özelliği, tüm verilerinizi Azure SQL Veri Ambarı'na getirmenizi mümkün kılar. Depolama maliyetleri en azdüzeydedir ve hesaplamayı yalnızca çözümlemek istediğiniz veri kümelerinin bölümlerinde çalıştırabilirsiniz. 

Azure SQL Veri Ambarı hakkında daha fazla bilgi için [SQL Veri Ambarı](https://azure.microsoft.com/services/sql-data-warehouse) web sitesine bakın. SQL Veri Ambarı ile uçlardan uca gelişmiş analiz çözümleri oluşturmayı öğrenmek [için, Team Data Science Process'e iş başında bakın: SQL Veri Ambarı'nı kullanarak.](sqldw-walkthrough.md)


## <a name="azure-data-lake"></a>Azure Data Lake

Azure Veri Gölü, herhangi bir resmi gereksinim veya şema dayatılmadan önce, tek bir konumda toplanan her tür verinin kuruluş çapında bir deposudur. Bu esneklik, boyutu veya yapısı ne kadar hızlı yutulurolursa olsun, her tür verinin bir veri gölünde tutulmasını sağlar. Kuruluşlar daha sonra bu veri göllerinde desenler bulmak için Hadoop veya gelişmiş analitik kullanabilirsiniz. Veri gölleri, verileri küratörlük yapmadan ve veri ambarına taşımadan önce daha düşük maliyetli veri hazırlama için bir depo olarak da hizmet verebilir.

Azure Veri Gölü hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/blog/introducing-azure-data-lake/) Azure Veri Gölü ile ölçeklenebilir uçtan uca veri bilimi çözümü oluşturmayı öğrenmek için Azure [Veri Gölü'nde Ölçeklenebilir Veri Bilimi: Uçtan uca Bir Geçiş](data-lake-walkthrough.md)


## <a name="azure-hdinsight-hive-hadoop-clusters"></a>Azure HDInsight Kovanı (Hadoop) kümeleri

Apache Hive, Sql'e benzer bir sorgu dili olan HiveQL kullanarak veri özetleme, sorgulama ve veri analizini sağlayan Hadoop için bir veri ambarı sistemidir. Kovan, verilerinizi etkileşimli olarak keşfetmek veya yeniden kullanılabilir toplu iş yapmak için kullanılabilir.

Kovan, yapıyı büyük ölçüde yapılandırılmamış veriler üzerinde yansıtmanızı sağlar. Yapıyı tanımladıktan sonra Hive'ı kullanarak Bu verileri Bir Hadoop kümesinde Java veya MapReduce'ı kullanmak zorunda kalmadan sorgulayabilirsiniz. HiveQL (Hive sorgu dili) T-SQL benzer ifadeler ile sorgular yazmanızı sağlar.

Veri bilimciler için Hive, kayıtları işlemek için Hive sorgularında Python Kullanıcı Tanımlı İşlevler (UDF'ler) çalıştırabilir. Bu özellik, veri analizinde Hive sorgularının yeteneğini önemli ölçüde genişletir. Özellikle, veri bilimcilerin çoğunlukla aşina oldukları dillerde ölçeklenebilir özellik mühendisliği yürütmelerine olanak tanır: SQL benzeri HiveQL ve Python. 

Azure HDInsight Hive Kümeleri hakkında daha fazla bilgi için [HDInsight'ta Hadoop ile Hive ve HiveQL'yi kullanın'](../../hdinsight/hadoop/hdinsight-use-hive.md)a bakın. Azure HDInsight Hive Kümeleri ile ölçeklenebilir uçtan uca veri bilimi çözümü oluşturmayı öğrenmek [için, Ekip Veri Bilimi Süreci'ni iş başında görün: HDInsight Hadoop kümelerini kullanarak.](hive-walkthrough.md)


## <a name="azure-file-storage"></a>Azure Dosya Depolama 

Azure Dosya Depolama, standart Sunucu İleti Bloğu (SMB) Protokolü'nü kullanarak bulutta dosya paylaşımları sunan bir hizmettir. SMB 2.1 ve SMB 3.0 desteklenir. Azure File Storage, Azure’a dosya paylaşımı kullanan eski uygulamaları maliyetli yeniden yazdırmaya ihtiyaç duymadan ve hızla taşıyabilmenizi sağlar. Azure Virtual Machines’de, Cloud Services’da veya şirket içi istemcilerde çalışan uygulamalar, bir masaüstü uygulamanın tipik SMB paylaşımı bağladığı gibi buluta bir dosya paylaşımı bağlayabilir. Ardından herhangi sayıda uygulama bileşeni eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

Özellikle veri bilimi projeleri için kullanışlı olan, proje verilerini proje ekibi üyelerinizle paylaşılabilme yeri olarak bir Azure dosya deposu oluşturma yeteneğidir. Daha sonra her biri Azure dosya depolamasındaki verilerin aynı kopyasına erişebilir. Ayrıca, projenin yürütülmesi sırasında oluşturulan özellik kümelerini paylaşmak için bu dosya depolama alanını da kullanabilirler. Proje bir müşteri etkileşimiyse, müşterileriniz proje verilerini ve özelliklerini sizinle paylaşmak için kendi Azure abonelikleri altında bir Azure dosya depolama alanı oluşturabilir. Bu şekilde, istemci proje veri varlıklarının tam denetimine sahiptir. Azure Dosya Depolama hakkında daha fazla bilgi için [Windows'daki Azure Dosya depolama alanına başlayın](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files) ve Linux ile Azure Dosya [Depolamasını nasıl kullanacağız](../../storage/files/storage-how-to-use-files-linux.md)'a bakın.


## <a name="sql-server-2019-r-and-python-services"></a>SQL Server 2019 R ve Python Hizmetleri

R Hizmetleri (Veritabanı İçi), yeni öngörüler ortaya çıkarabilecek akıllı uygulamalar geliştirmek ve dağıtmak için bir platform sağlar. Model oluşturmak ve SQL Server verilerinizden öngörüler oluşturmak için R topluluğu tarafından sağlanan birçok paket de dahil olmak üzere zengin ve güçlü R dilini kullanabilirsiniz. R Hizmetleri (Veritabanı İçi) R dilini SQL Server ile tümleştirdiği için, analizler verilere yakın tutulur ve bu da verilerin taşınmasıyla ilişkili maliyetleri ve güvenlik risklerini ortadan kaldırır.

R Services (Veritabanı içi) açık kaynak R dilini kapsamlı bir SQL Server araçları ve teknolojileri kümesiyle destekler. Üstün performans, güvenlik, güvenilirlik ve yönetilebilirlik sunarlar. R çözümlerini kullanışlı ve tanıdık araçları kullanarak dağıtabilirsiniz. Üretim uygulamalarınız, Transact-SQL kullanarak R çalışma saatini arayabilir ve öngörüleri ve görselleri alabilir. Ayrıca, R çözümlarınızın ölçeğini ve performansını artırmak için ScaleR kitaplıklarını da kullanırsınız. Daha fazla bilgi için [SQL Server R Hizmetleri'ne](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services)bakın.

Microsoft'un TDSP ekibi, SQL Server 2016 R Services'da veri bilimi çözümlerinin nasıl geliştirilmeye ne kadar yol açabildiğini gösteren iki uçtan uca izçıkış yayımladı: biri R programcıları için, diğeri de SQL geliştiricileri için. **R Programcıları**için [Bkz. Veri Bilimi Uçtan Uca Walkthrough.](https://docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough) **SQL Geliştiricileri** [için, SQL Geliştiricileri için Veritabanı Içi Gelişmiş Analitik (Öğretici)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers)bölümüne bakın.


## <a name="appendix-tools-to-set-up-data-science-projects"></a><a name="appendix"></a>Ek: Veri bilimi projeleri kurmak için araçlar

### <a name="install-git-credential-manager-on-windows"></a>Windows'da Git Kimlik Bilgileri Yöneticisi'ni yükleme

**Windows'da**TDSP'yi takip ediyorsanız, Git depolarıyla iletişim kurmak için **Git Kimlik Bilgileri Yöneticisi'ni (GCM)** yüklemeniz gerekir. GCM yüklemek için öncelikle **Chocolaty'yi**yüklemeniz gerekir. Chocolaty ve GCM'yi yüklemek için Windows PowerShell'de **yönetici**olarak aşağıdaki komutları çalıştırın:  

    iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
    choco install git-credential-manager-for-windows -y
    

### <a name="install-git-on-linux-centos-machines"></a>Git'i Linux (CentOS) makinelere yükleyin

Git'i Linux (CentOS) makinelerine yüklemek için aşağıdaki bash komutunu çalıştırın:

    sudo yum install git


### <a name="generate-public-ssh-key-on-linux-centos-machines"></a>Linux (CentOS) makinelerde ortak SSH anahtarı oluşturma

Git komutlarını çalıştırmak için Linux (CentOS) makinelerini kullanıyorsanız, bu makinenin Azure DevOps Hizmetleri tarafından tanınması için makinenizin ortak SSH anahtarını Azure DevOps Hizmetleri'nize eklemeniz gerekir. İlk olarak, ortak bir SSH anahtarı oluşturmanız ve Azure DevOps Hizmetleri güvenlik ayarı sayfanızdaki SSH ortak anahtarlarına anahtarı eklemeniz gerekir. 

1. SSH anahtarını oluşturmak için aşağıdaki iki komutu çalıştırın: 

   ```
   ssh-keygen
   cat .ssh/id_rsa.pub
   ```
   
   ![SSH anahtarını oluşturmak için komutlar](./media/platforms-and-tools/resources-1-generate_ssh.png)

1. *Ssh-rsa*dahil olmak üzere tüm ssh tuşunu kopyalayın. 
1. Azure DevOps Hizmetlerinizde oturum açın. 
1. Sayfanın sağ üst köşesinde **adınızı\><** ve **güvenlik'e**tıklayın. 
    
   ![Adınızı tıklatın ve sonra güvenlik tıklayın](./media/platforms-and-tools/resources-2-user-setting.png)

1. **SSH ortak tuşlarını**tıklatın ve **+Ekle'ye**tıklayın. 

   ![SSH ortak tuşlarını tıklatın ve sonra +Ekle'yi tıklatın](./media/platforms-and-tools/resources-3-add-ssh.png)

1. Metin kutusuna kopyalanan ssh tuşunu yapıştırın ve kaydedin.


## <a name="next-steps"></a>Sonraki adımlar

**Belirli senaryolar** için işlemdeki tüm adımları gösteren tam uçtan uca walkthroughs da sağlanır. Bunlar, [Örnek izthroughs](walkthroughs.md) konusundaki küçük resim açıklamalarıyla listelenir ve bunlarla bağlantılıdır. Bunlar, akıllı bir uygulama oluşturmak için bulut, şirket içi araçlar ve hizmetleri iş akışı veya ardışık ardışık yollarla nasıl birleştirebileceklerini gösterir. 

Azure Machine Learning Studio'yu (klasik) kullanarak Team Data Science Process'deki adımların nasıl yürütüleceklerini gösteren örnekler için Azure [ML](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) ile öğrenme yolunu görün.
