---
title: Veri bilimi projeleri için platformlar ve araçlar-ekip veri bilimi Işlemi
description: Ekip veri bilimi sürecinde bulunan kuruluşlar için kullanılabilen veri ve analiz kaynaklarını sıralar ve ele alır.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a9528b31bfd1be40b478f3c47fd0a16047a7f6ff
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86028128"
---
# <a name="platforms-and-tools-for-data-science-projects"></a>Veri bilimi projeleri için platformlar ve araçlar

Microsoft, hem bulut hem de şirket içi platformlar için eksiksiz bir analiz kaynakları yelpazesi sağlar. Veri bilimi projelerinizin yürütülmesi verimli ve ölçeklenebilir hale getirmek için dağıtılabilecek. Veri bilimi projelerini uygulayan takımlar için bir izleme, sürüm denetimli ve işbirliğine dayalı bir yöntem, [ekip veri bilimi işlemi](overview.md) (TDSP) tarafından sağlanır.  Personel rollerinin bir özeti ve bir veri bilimi ekibi tarafından işlenen ilişkili görevleri bu işlemi gerçekleştirirken, bkz. [Team Data Science işlem rolleri ve görevleri](roles-tasks.md).

TDSP kullanan veri bilimi ekipleri için kullanılabilen analiz kaynakları şunları içerir:

- Veri bilimi sanal makineleri (hem Windows hem de Linux CentOS)
- HDInsight Spark kümeleri
- Synapse Analytics
- Azure Data Lake
- HDInsight Hive kümeleri
- Azure Dosya Depolama
- SQL Server 2019 R ve Python Hizmetleri
- Azure Databricks

Bu belgede, kaynakları kısaca anladık ve TDSP takımlarının yayımladığı öğreticilere ve izlenecek yollara bağlantılar sağlıyoruz. Bunlar adım adım adımları nasıl kullanacağınızı ve akıllı uygulamalarınızı oluşturmak için kullanmaya nasıl başlayacağınızı öğrenmenize yardımcı olabilirler. Bu kaynaklarla ilgili daha fazla bilgiyi ürün sayfalarında bulabilirsiniz. 

## <a name="data-science-virtual-machine-dsvm"></a>Veri Bilimi Sanal Makinesi (DSVM)

Microsoft tarafından hem Windows hem de Linux üzerinde sunulan veri bilimi sanal makinesi, veri bilimi modelleme ve geliştirme etkinlikleri için popüler araçları içerir. Aşağıdakiler gibi araçlar içerir:

- Microsoft R Server Developer Edition 
- Anaconda Python dağıtımı
- Python ve R için jupyıter Not defterleri 
- Windows üzerinde Python ve R araçları ile Visual Studio Community Edition/Linux üzerinde tutulma
- Windows için masaüstü Power BI
- Linux üzerinde Windows/Postgres üzerinde 2016 SQL Server Geliştirici sürümü

Ayrıca, xgboost, mxnet ve Vowpal Wabbit gibi **ml ve AI araçları** da içerir.

Şu anda DSVM, **Windows** ve **Linux CentOS** işletim sistemlerinde kullanılabilir. Üzerinde yürütmeyi planladığınız veri bilimi projelerinin ihtiyaçlarına göre DSVM 'nizin boyutunu (CPU çekirdeği sayısı ve bellek miktarı) seçin. 

DSVM 'nin Windows sürümü hakkında daha fazla bilgi için bkz. Azure Marketi 'nde [Microsoft veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-win-2019) . DSVM 'nin Linux sürümü için bkz. [linux veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.ubuntu-1804).

DSVM 'de ortak veri bilimi görevlerinin bazılarının nasıl yürütüleceğini öğrenmek için bkz. [veri bilimi sanal makinesi üzerinde yapabileceğiniz 10 şey](../data-science-virtual-machine/vm-do-ten-things.md)


## <a name="azure-hdinsight-spark-clusters"></a>Azure HDInsight Spark kümeleri

Apache Spark, Büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen açık kaynaklı bir paralel işleme altyapısıdır. Spark işleme altyapısı hız, kullanım kolaylığı ve gelişmiş analiz için oluşturulmuştur. Spark 'ın bellek içi hesaplama özellikleri, Machine Learning ve grafik hesaplamaları için yinelemeli algoritmalar için iyi bir seçim yapar. Spark, Azure Blob depolama (IŞB) ile de uyumludur, bu nedenle Azure 'da depolanan mevcut verileriniz Spark kullanılarak kolayca işlenebilir.

HDInsight’ta Spark kümesi oluşturduğunuzda, Spark yüklenmiş ve yapılandırılmış olarak Azure işlem kaynakları oluşturursunuz. HDInsight 'ta Spark kümesi oluşturmak yaklaşık 10 dakika sürer. İşlenecek verileri Azure Blob depolama alanında depolayın. Azure Blob depolamayı bir kümeyle kullanma hakkında daha fazla bilgi için bkz. [HDInsight 'Ta Hadoop ile uyumlu Azure Blob depolamayı kullanma](../../hdinsight/hdinsight-hadoop-use-blob-storage.md).

Microsoft tarafından yapılan TDSP ekibi, tek bir Python ve diğer Scala kullanarak veri bilimi çözümleri oluşturmak için Azure HDInsight Spark kümelerinin nasıl kullanılacağına ilişkin iki uçtan uca izlenecek yol yayımladı. Azure HDInsight **Spark kümeleri**hakkında daha fazla bilgi için bkz. [genel bakış: hdınsight Linux üzerinde Apache Spark](../../hdinsight/spark/apache-spark-overview.md). Azure HDInsight Spark kümesinde **Python** kullanarak veri bilimi çözümü oluşturmayı öğrenmek için bkz. [Azure HDInsight 'Ta Spark kullanarak veri bilimine genel bakış](spark-overview.md). Azure HDInsight Spark kümesinde **Scala** kullanarak bir veri bilimi çözümü oluşturmayı öğrenmek için bkz. [Azure 'da Scala ve Spark kullanan veri bilimi](scala-walkthrough.md). 


##  <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

Azure SQL veri ambarı, sağlama veya aşırı ödeme olmadan işlem kaynaklarını kolayca ve saniyeler içinde ölçeklendirmenize olanak tanır. Ayrıca, işlem kaynaklarının kullanımını duraklatmaya yönelik benzersiz seçeneği sunar ve böylece bulut maliyetlerinizi daha iyi yönetebilirsiniz. Ölçeklenebilir işlem kaynaklarını dağıtma özelliği, tüm verilerinizi Azure SQL veri ambarı 'na getirmeyi mümkün kılar. Depolama maliyetleri en düşük düzeydedir ve yalnızca çözümlemek istediğiniz veri kümelerinin bölümlerinde işlem gerçekleştirebilirsiniz. 

Azure SQL veri ambarı hakkında daha fazla bilgi için bkz. [SQL veri ambarı](https://azure.microsoft.com/services/sql-data-warehouse) Web sitesi. SQL veri ambarı ile uçtan uca gelişmiş analiz çözümleri oluşturmayı öğrenmek için, bkz [: SQL veri ambarı 'nı kullanma](sqldw-walkthrough.md).


## <a name="azure-data-lake"></a>Azure Data Lake

Azure Data Lake, tek bir konumda toplanan tüm veri türlerinin kurumsal çapta bir deposudur, herhangi bir biçimsel gereksinimden önce veya uygulanan şemadan oluşur. Bu esneklik, boyutu veya yapısı ne olursa olsun, Veri Gölü her türden verilerin tutulmasını sağlar. Daha sonra kuruluşlar, bu veri lalardaki desenleri bulmak için Hadoop veya gelişmiş analiz kullanabilir. Veri Lakes Ayrıca verileri kapatmadan ve veri ambarına taşımadan önce düşük maliyetli veri hazırlığı için bir depo olarak da kullanılabilir.

Azure Data Lake hakkında daha fazla bilgi için bkz. [tanıtımı Azure Data Lake](https://azure.microsoft.com/blog/introducing-azure-data-lake/). Azure Data Lake ile ölçeklenebilir bir uçtan uca veri bilimi çözümü oluşturmayı öğrenmek için bkz [. Azure Data Lake Içinde ölçeklenebilir veri bilimi: uçtan uca bir anlatım](data-lake-walkthrough.md)


## <a name="azure-hdinsight-hive-hadoop-clusters"></a>Azure HDInsight Hive (Hadoop) kümeleri

Apache Hive, SQL 'e benzer bir sorgu dili olan HiveQL kullanarak veri özetlemesini, sorgulamayı ve veri analizini sağlayan Hadoop için veri ambarı sistemidir. Hive, verilerinizi etkileşimli olarak araştırmak veya yeniden kullanılabilir toplu işleme işleri oluşturmak için kullanılabilir.

Hive büyük ölçüde yapılandırılmamış veriler üzerinde proje yapısı oluşturmanızı sağlar. Yapıyı tanımladıktan sonra, kullanmak zorunda kalmadan veya hatta bilmeniz, Java veya MapReduce 'nin kullanılması gerekmeden bu verileri bir Hadoop kümesinde sorgulamak için Hive kullanabilirsiniz. HiveQL (Hive sorgu dili), T-SQL ' e benzeyen deyimlerle sorgu yazmanıza olanak sağlar.

Veri bilimcileri için Hive, kayıtları işlemek üzere Hive sorgularında Python Kullanıcı tanımlı Işlevleri (UDF 'ler) çalıştırabilir. Bu özellik veri analizinde Hive sorgularının yeteneklerini önemli ölçüde genişletir. Özellikle, veri bilimcilerinin, büyük ölçüde alışkın oldukları dillerde ölçeklenebilir Özellik Mühendisliği yapmasına olanak sağlar: SQL benzeri HiveQL ve Python. 

Azure HDInsight Hive kümeleri hakkında daha fazla bilgi için bkz. [HDInsight 'Ta Hadoop Ile Hive ve HiveQL kullanma](../../hdinsight/hadoop/hdinsight-use-hive.md). Azure HDInsight Hive kümeleriyle ölçeklenebilir uçtan uca bir veri bilimi çözümü oluşturmayı öğrenmek için bkz. [Team Data Science Process ın, HDInsight Hadoop kümelerini kullanma](hive-walkthrough.md).


## <a name="azure-file-storage"></a>Azure Dosya Depolama 

Azure dosya depolama, standart sunucu Ileti bloğu (SMB) protokolünü kullanarak bulutta dosya paylaşımları sunan bir hizmettir. SMB 2.1 ve SMB 3.0 desteklenir. Azure File Storage, Azure’a dosya paylaşımı kullanan eski uygulamaları maliyetli yeniden yazdırmaya ihtiyaç duymadan ve hızla taşıyabilmenizi sağlar. Azure Virtual Machines’de, Cloud Services’da veya şirket içi istemcilerde çalışan uygulamalar, bir masaüstü uygulamanın tipik SMB paylaşımı bağladığı gibi buluta bir dosya paylaşımı bağlayabilir. Ardından herhangi sayıda uygulama bileşeni eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

Özellikle veri bilimi projeleri için yararlı olan proje ekip üyelerinizle proje verilerini paylaşmak için bir Azure dosya deposu oluşturma olanağıdır. Bunların her biri, Azure dosya depolama alanındaki verilerin aynı kopyasına erişebilir. Ayrıca, projenin yürütülmesi sırasında oluşturulan özellik kümelerini paylaşmak için bu dosya depolama alanını kullanabilir. Proje bir istemci katılımı ise, istemcileriniz proje verilerini ve özelliklerini sizinle paylaşmak için kendi Azure abonelikleri altında bir Azure dosya depolama alanı oluşturabilir. Bu şekilde, istemci proje veri varlıkları üzerinde tam denetime sahiptir. Azure dosya depolama hakkında daha fazla bilgi için bkz. [Windows 'Ta Azure dosya depolama ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files) ve [Linux Ile Azure dosya depolamayı kullanma](../../storage/files/storage-how-to-use-files-linux.md).


## <a name="sql-server-2019-r-and-python-services"></a>SQL Server 2019 R ve Python Hizmetleri

R Services (veritabanı Içi), yeni Öngörüler açığa çıkarmak için akıllı uygulamalar geliştirmeye ve dağıtmaya yönelik bir platform sağlar. R topluluğu tarafından sunulan birçok paket de dahil olmak üzere zengin ve güçlü R dilini kullanarak modeller oluşturabilir ve SQL Server verilerinize tahmin oluşturabilirsiniz. R Services (veritabanı Içi) R dilini SQL Server ile tümleştirdiğinden, analiz verileri yakın tutulur; bu da verileri taşıma ile ilişkili maliyetleri ve güvenlik risklerini ortadan kaldırır.

R Services (veritabanı Içi), kapsamlı bir SQL Server araçları ve teknolojileri kümesiyle açık kaynak R dilini destekler. Üstün performans, güvenlik, güvenilirlik ve yönetilebilirlik sunar. Kullanışlı ve tanıdık araçları kullanarak R çözümlerini dağıtabilirsiniz. Üretim uygulamalarınız R çalışma zamanını çağırabilir ve Transact-SQL ' y i kullanarak tahminleri ve görselleri alabilir. Ayrıca, R çözümlerinizin ölçeğini ve performansını geliştirmek için ScaleR kitaplıklarını da kullanabilirsiniz. Daha fazla bilgi için bkz. [SQL Server R Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

Microsoft 'un TDSP ekibi, SQL Server 2016 R hizmetlerinde veri bilimi çözümlerinin nasıl oluşturulduğunu gösteren iki uçtan uca izlenecek yol yayımladı: bir R programcıları ve bir SQL geliştiricileri için. **R programcıları**için bkz. [veri bilimi uçtan uca izlenecek yol](https://docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough). **SQL geliştiricileri**için bkz. [SQL geliştiricileri Için veritabanı içi gelişmiş analiz (öğretici)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers).


## <a name="appendix-tools-to-set-up-data-science-projects"></a><a name="appendix"></a>Ek: veri bilimi projelerini ayarlamaya yönelik araçlar

### <a name="install-git-credential-manager-on-windows"></a>Windows 'a git kimlik bilgileri Yöneticisi 'Ni yükler

**Windows**üzerinde TDSP 'yi takip ediyorsanız, git depoları ile iletişim kurmak Için **Git kimlik bilgileri YÖNETICISI 'ni (GCM)** yüklemeniz gerekir. GCM 'yi yüklemek için önce **chocolaty**yüklemeniz gerekir. Chocolaty ve GCM 'yi yüklemek için Windows PowerShell 'de **yönetici**olarak aşağıdaki komutları çalıştırın:  

```powershell
iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
choco install git-credential-manager-for-windows -y
```  

### <a name="install-git-on-linux-centos-machines"></a>Linux (CentOS) makinelerine git 'i yükler

Linux (CentOS) makinelerine git yüklemek için aşağıdaki Bash komutunu çalıştırın:

```powershell
sudo yum install git
```

### <a name="generate-public-ssh-key-on-linux-centos-machines"></a>Linux (CentOS) makinelerinde ortak SSH anahtarı oluşturma

Git komutlarını çalıştırmak için Linux (CentOS) makineleri kullanıyorsanız, bu makinenin Azure DevOps Services tanınması için makinenizin ortak SSH anahtarını Azure DevOps Services eklemeniz gerekir. İlk olarak, ortak bir SSH anahtarı oluşturmanız ve anahtarı Azure DevOps Services güvenlik ayarı sayfanızda SSH ortak anahtarlarına eklemeniz gerekir. 

1. SSH anahtarı oluşturmak için aşağıdaki iki komutu çalıştırın: 

   ```
   ssh-keygen
   cat .ssh/id_rsa.pub
   ```
   
   ![SSH anahtarı oluşturma komutları](./media/platforms-and-tools/resources-1-generate_ssh.png)

1. SSH *-RSA*dahil tüm SSH anahtarını kopyalayın. 
1. Azure DevOps Services oturum açın. 
1. Sayfanın sağ üst köşesindeki ** \> adınızı<** tıklayın ve **güvenlik**' e tıklayın. 
    
   ![Adınızın ardından Güvenlik ' e tıklayın.](./media/platforms-and-tools/resources-2-user-setting.png)

1. **SSH ortak anahtarlar**' a tıklayın ve **+ Ekle**' ye tıklayın. 

   ![SSH ortak anahtarlar ' a ve ardından + Ekle ' ye tıklayın.](./media/platforms-and-tools/resources-3-add-ssh.png)

1. Metin kutusuna kopyalanmış olan SSH anahtarını yapıştırın ve kaydedin.


## <a name="next-steps"></a>Sonraki adımlar

**Belirli senaryolar** için işlemdeki adımların tümünü gösteren tam uçtan uca izlenecek yollar da sağlanmaktadır. Bunlar listelenmiş ve [örnek izlenecek yollar](walkthroughs.md) konusunun küçük resim açıklamalarıyla bağlantılandırılır. Bulut, şirket içi araçları ve hizmetleri akıllı bir uygulama oluşturmak için bir iş akışı veya işlem hattı halinde nasıl birleştirebileceğinizi gösterir. 

Azure Machine Learning Studio (klasik) kullanarak takım veri bilimi Işleminde adımların nasıl yürütüleceğini gösteren örnekler için, bkz. [Azure ML](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) öğrenme yolu.
