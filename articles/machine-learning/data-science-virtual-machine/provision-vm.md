---
title: Windows veri bilimi sanal makinesi oluşturma
titleSuffix: Azure
description: Yapılandırma ve analiz için Azure'da bir veri bilimi sanal makinesi oluşturma ve makine öğrenimi.
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: quickstart
ms.date: 02/22/2019
ms.author: vijetaj
ms.openlocfilehash: dfb3e9f6390d4c80b8f3c37b87f2659c671fa823
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68591831"
---
# <a name="provision-a-windows-data-science-virtual-machine-on-azure"></a>Azure 'da bir Windows Veri Bilimi Sanal Makinesi sağlama

Microsoft Windows Veri Bilimi Sanal Makinesi (DSVM), önceden yüklenmiş ve veri analizi ve makine öğrenimi araçları ile yapılandırılmış bir Windows Server 2016 sanal makinesi (VM) görüntüsüdür.

## <a name="included-data-science-tools"></a>Dahil edilen veri bilimi araçları

Aşağıdaki araçlar DSVM 'ye eklenmiştir:

* [Azure Machine Learning hizmeti](../service/index.yml) Python SDK 'Sı
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/index) Geliştirici sürümü
* Anaconda Python dağıtım
* R, Python ve pyspark çekirdekler ile Jupyter Notebook
* Microsoft Visual Studio Community
* Microsoft Power BI Desktop
* Microsoft SQL Server 2017 geliştirici sürümü
* Yerel geliştirme ve test için tek başına Apache Spark örneği
* [JuliaPro](https://juliacomputing.com/products/juliapro.html)
* Makine öğrenimi ve veri analizi araçları:
  * Derin öğrenme çerçeveleri-VM 'ye bir zengin AI çerçeveleri kümesi dahildir: [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxnet ve keras
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) -çevrimiçi karma, allazaltma, indirimleri, learning2search ve etkin ve etkileşimli öğrenme gibi teknikleri destekleyen hızlı bir makine öğrenimi sistemi
  * [Xgboost](https://xgboost.readthedocs.org/en/latest/) -hızlı ve doğru şekilde artırılmış ağaç uygulama sağlayan bir araç
  * [Rattle](https://togaware.com/rattle/) -r 'de veri analizi ve makine öğrenimini kullanmaya başlamanızı sağlayan r analitik aracı. Otomatik R kod üretimi ile GUI tabanlı veri araştırması ve modelleme içerir.
  * Java 'da [WEKA](https://www.cs.waikato.ac.nz/ml/weka/) -görsel veri madenciliği ve makine öğrenimi yazılımı
  * [Apache detaya gitme](https://drill.apache.org/) -Apache Hadoop, NoSQL ve bulut depolama için şema ücretsiz SQL sorgu altyapısı. ODBC ve JDBC arabirimleri, NoSQL ve Power BI, Microsoft Excel ve Tableau gibi standart BI araçlarından dosyaları'nı sorgulamak için destekler.
* Kitaplıklarında, R ve Python için Azure Machine Learning ve diğer Azure Hizmetleri kullanma
* Git, GitHub ve Azure DevOps içeren kaynak kodu depoları ile çalışmak için Git Bash dahil. Git, Git Bash ve bir komut istemi erişilebilir çeşitli popüler Linux komut satırı yardımcı programları sağlar. Awk, sed, perl, grep, bulma, wget ve curl örnek verilebilir.

### <a name="about-data-science"></a>Veri bilimi hakkında

Veri bilimi görevleri bir dizi üzerinde yineleme içerir:

1. Verileri bulma, yükleme ve ön işleme
1. Yapı ve test modelleri
1. Akıllı uygulamalarda tüketim için modelleri dağıtma

Veri bilimcileri bu görevler için çeşitli araçlar kullanın. Zaman yazılım uygun sürümleri bulun ve sonra da indirin ve yükleyin alıcı olabilir. DSVM, önceden yüklenmiş ve yapılandırılmış çeşitli popüler araçlarla Azure 'da sağlanabilen kullanıma hazır bir görüntü sağlayarak zaman kazandırır.

DSVM hızlı bir şekilde analiz projenizi başlatır. R, Python, SQL ve C# gibi çeşitli dillerde görevler üzerinde çalışabilir. Visual Studio geliştirme ve kodunuzu test etmek için kullanımı kolay tümleşik geliştirme ortamı (IDE) sağlar. Microsoft 'un bulut platformunda çeşitli hizmetleri kullanarak uygulamalarınızı oluşturabilmeniz için Azure SDK, VM 'ye dahildir.

Bu veri bilimi VM görüntüsü için hiçbir yazılım ücreti yoktur. Yalnızca Azure kullanım ücretleri ödeme yaparsınız. Bunlar, sağlamanız sanal makinenin boyutuna bağlıdır. Daha ayrıntılı bilgi işlem ücretleri bulunan **fiyatlandırma ayrıntıları** bölümünde [veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) sayfası.

### <a name="other-dsvm-versions"></a>Diğer DSVM sürümleri

* Bir [Ubuntu](dsvm-ubuntu-intro.md) görüntü. Bunun yanı sıra bazı ek derin öğrenme çerçeveleri DSVM'ye benzer birçok araç vardır.
* A [Linux CentOS](linux-dsvm-intro.md) görüntü.
* [Windows Server 2012 sürümü](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) , veri bilimi sanal makinesi. Bazı araçlar, yalnızca Windows Server 2016 sürümünde kullanılabilir. Aksi takdirde, bu makalede, Windows Server 2012 sürümüne de geçerlidir.

## <a name="prerequisite"></a>Önkoşul

Bir Microsoft Veri bilimi sanal makinesi oluşturmak için bir Azure aboneliğinizin olması gerekir. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.com/free).

## <a name="create-your-dsvm"></a>DSVM 'nizi oluşturma

DSVM örneği oluşturmak için:

1. [Azure Portal](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016)sanal makine listesine gidin. Henüz oturum açmadıysanız Azure hesabınızda oturum açmanız istenebilir.
1. Alt kısımdaki **Oluştur** düğmesini seçin.

   ![Yapılandırma-data-bilimi-vm](./media/provision-vm/configure-data-science-virtual-machine.png)

1. Ekran görüntüsünün sağ bölmesinde gösterilen adımların her birini yapılandırmak için aşağıdaki bilgileri girmeniz gerekir:

   1. **Temel**:
      * **Ad**: dsvm 'nin adı
      * **VM disk türü**: **SSD** veya **HDD**. NVIDIA Tesla K80 tabanlı gibi NC_v1 GPU örnek için Seç **HDD** disk türü.
      * **Kullanıcı adı**: yönetıcı hesap kimliği
      * **Parola**: yönetici hesabı parolası
      * **Abonelik**: Birden fazla aboneliğiniz varsa, bir makine oluşturulması ve fatura olduğu seçin.
      * **Kaynak Grubu**. Yeni bir tane oluşturabilir veya varolan bir grubu kullanın.
      * **Konum**. En uygun veri merkezi seçin. En hızlı ağ erişimi için verilerinizden en iyi veya fiziksel konumunuza en yakın veri merkezidir.
   1. **Boyutu**: Maliyet kısıtlamaları ve işlevsel gereksinimlerini karşılayan sunucusu türlerinden birini seçin. Daha fazla VM boyutları için seçim **görünümü tüm**.
   1. **Ayarları**:  
      * **Yönetilen diskleri kullanma**. Seçin **yönetilen** VM için diskleri yönetmek için Azure istiyorsanız. Aksi takdirde, yeni veya mevcut bir depolama hesabı belirtmeniz gerekir.  
      * **Diğer parametreler**. Varsayılan değerleri kullanabilirsiniz. Varsayılan olmayan değerler kullanmak istiyorsanız, belirli alanlarla ilgili yardım için bilgilendirici bağlantının üzerine gelin.
   1. **Özet**: Girdiğiniz tüm bilgilerin doğru olduğundan emin olun. **Oluştur**’u seçin.

> [!NOTE]
> * VM, **Boyut** adımında seçtiğiniz sunucu boyutu için işlem maliyetinden daha fazla ücret ödemez.
> * Sağlama yaklaşık 10-20 dakika sürer. Azure portal sanal makinenizin durumunu görüntüleyebilirsiniz.

## <a name="how-to-access-the-dsvm"></a>DSVM 'ye erişme

Sanal makine oluşturulup sağlanan sonra Uzak Masaüstü uygulamasına önceki yapılandırdığınız yönetici hesabının kimlik bilgilerini kullanarak yapabilecekleriniz **Temelleri** bölümü. Yüklenmiş ve yapılandırılmış VM'de araçları kullanmaya başlamak hazırsınız demektir. Araçların çoğuna başlangıç menüsü kutucukları ve Masaüstü simgeleri aracılığıyla erişilebilir.

Ayrıca, sanal makine üzerinde jupi not defterlerini çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Notebooks Veri Bilimi VM'si ekleyebilirsiniz. Daha fazla bilgi için bkz. [Not defteri projelerini yönetme ve yapılandırma-işlem katmanı](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makinede yüklü araçları

DSVM 'de yüklü olan araçlar hakkında daha fazla bilgi edinin:

### <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Geliştirici sürümü

Machine Learning Server Developer edition VM üzerinde yüklü olmadığından, Microsoft Enterprise Library ölçeklenebilir R veya Python için analiz için kullanabilirsiniz. Daha önce Microsoft R Server olarak bilinen Machine Learning Server, yaygın olarak dağıtılabilir bir kurumsal sınıf analiz platformudur. Hem R hem de Python için kullanılabilir. Ayrıca ölçeklenebilir, ticari olarak desteklenmiş ve güvenli hale gelir.

Machine Learning sunucusu, çeşitli büyük veri istatistikleri, Tahmine dayalı modelleme ve makine öğrenimi görevlerini destekler. Analytics tam aralığını destekler: Keşif, analiz, Görselleştirme ve modelleme. Kullanarak ve açık kaynak R ve Python, R ve Python betikleri ve işlevler ile Machine Learning sunucusu uyumludur. Kurumsal ölçekte verileri çözümlemek için CRAN, pip ve Conda paketleri ile de uyumludur.

Machine Learning sunucusu ekleme paralel ve öbeklenmiş veri işleme tarafından açık kaynaklı R'nin bellek içi sınırlamaları ele alır. Bu, analizi, ana bellekte uygun olandan çok daha büyük veriler üzerinde çalıştırabileceğiniz anlamına gelir. Visual Studio Community, VM'de dahil edilir. Visual Studio için R araçları ve R veya Python ile çalışmak üzere tam IDE sağlayan Visual Studio için Python Araçları (PTV) uzantılarına sahiptir. Ayrıca, diğer Ide'leri ister bildirimde [RStudio](https://www.rstudio.com) ve [PyCharm Community edition](https://www.jetbrains.com/pycharm/) VM üzerinde.

### <a name="python"></a>Python

Python kullanarak geliştirme için, Anaconda Python dağıtımları 2,7 ve 3,6 yüklenir. Bu dağıtımların yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra temel Python var. Visual Studio Community 2017 içinde yüklenen PTVS kullanabilirsiniz. Veya boşta veya Spyder gibi Anaconda birlikte IDE'ler birini kullanabilirsiniz. Arayın ve bu paketlerin (Win + S) başlatın.

> [!NOTE]
> Anaconda Python 2.7, Visual Studio için Python Tools işaret etmek için her sürüm için özel ortamlarda oluşturmanız gerekir. Visual Studio 2017 Community bu ortam yollarını ayarlamak için gidin **Araçları** > **Python Araçları** > **Python ortamları**. Ardından **+ özel**.

Anaconda Python 3.6 altında yüklü **C:\Anaconda**. Anaconda Python 2.7 altında yüklü **c:\Anaconda\envs\python2**. Ayrıntılı adımlar için bkz. [PTVS dokümantasyonu](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters).

### <a name="the-jupyter-notebook"></a>Jupyter not defteri

Anaconda dağıtım Jupyter not defteri ile kod ve analiz paylaşmak için bir ortam da gelir. Jupyter Notebook sunucusu ile Python 2.7 ve Python 3.x, PySpark, Julia ve R defterleri önceden yapılandırılmıştır. Jupyter sunucusunu başlatmak ve Not defteri sunucusuna erişmek için tarayıcıyı başlatmak üzere **Jupyter Notebook** masaüstü simgesini kullanın.

Biz, Python ve R'dir birkaç örnek not defterleri paketi Jupyter eriştikten sonra dizüstü aşağıdaki teknolojilerle çalışmanıza işlemini gösterir:

* Machine Learning Server
* SQL Server Machine Learning Services, veritabanı içi analiz
* Python
* Microsoft bilişsel araç seti
* Tensorflow
* Diğer Azure teknolojileri

Daha önceki bir adımda oluşturduğunuz parolayı kullanarak Jupyter Notebook kimlik doğrulaması yaptıktan sonra Not defteri giriş sayfasında örneklerin bağlantısını bulabilirsiniz.

### <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

DSVM, Visual Studio Community 'YI içerir. Bu, Microsoft 'un değerlendirme amaçları ve küçük takımlar için kullanabileceğiniz, popüler IDE 'nin ücretsiz bir sürümüdür. Bkz: [lisanslama koşulları](https://www.visualstudio.com/support/legal/mt171547).

Masaüstü simgesini veya **Başlat** menüsünü kullanarak Visual Studio 'yu açın. Programlar (Win + S), arayın ve ardından **Visual Studio**. Burada, C#, Python, R ve node.js gibi dillerde projeleri oluşturabilirsiniz. Yüklü eklentiler aşağıdaki Azure Hizmetleri ile çalışmak uygun yapın:

* Azure Veri Kataloğu
* Azure HDInsight Hadoop ve Spark
* Azure Data Lake

Ayrıca, Azure Machine Learning ile sorunsuz bir şekilde tümleşen **Visual Studio Code Azure Machine Learning** adlı bir eklenti ve hızlı BIR şekilde AI uygulamaları oluşturmanıza yardımcı olur.

> [!NOTE]
> Değerlendirme süreniz doldu bir ileti alabilirsiniz. Microsoft hesabı kimlik bilgilerinizi girin. Veya Visual Studio Community erişim elde etmek için yeni bir ücretsiz hesap oluşturun.

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer edition

DSVM, Machine Learning Services SQL Server 2017 'in bir geliştirici sürümü ile birlikte gelir. Bu SQL Server sürümü R veya Python 'da gelir ve veritabanı içi analizler çalıştırabilir. Machine Learning Hizmetleri akıllı uygulama geliştirme ve dağıtma için bir platform sunar. Bu diller ve topluluktan gelen çok sayıda paketleri, modeller oluşturun ve SQL Server verilerinizi Öngörüler oluşturmak için kullanabilirsiniz. Machine Learning Hizmetleri veritabanında, SQL Server R ve Python dilleri tümleşir çünkü verilere yaklaştırılmasıyla analytics tutabilirsiniz. Bu tümleştirme, maliyet ve veri taşıma ile ilişkili güvenlik riskleri ortadan kaldırır.

> [!NOTE]
> SQL Server Developer sürümü yalnızca geliştirme ve test amaçları içindir. Üretim ortamında çalıştırmak için lisansa ihtiyacınız var.

SQL Server, Microsoft SQL Server Management Studio'yu başlatarak erişebilirsiniz. Sanal Makinenizin adını olarak doldurulur **sunucu adı**. Windows üzerinde yönetici olarak oturum açtığınızda, Windows kimlik doğrulaması kullanın. SQL Server Management Studio'da olduğunuzda, diğer kullanıcılar oluşturma, veritabanları oluşturma, veri içeri aktarın ve SQL sorgularını çalıştırın.

Sunucu Yöneticisi oturum açtıktan sonra veritabanı içi analiz SQL Machine Learning hizmetlerini kullanarak etkinleştirmek için aşağıdaki komutu SQL Server Management Studio'da tek seferlik bir eylem olarak çalıştırın:

```
CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS
```

Sanal `%COMPUTERNAME%` makinenizin adıyla değiştirin.

### <a name="azure"></a>Azure

Çeşitli Azure Araçları VM'de yüklenir:

* Bir masaüstü kısayolu için Azure SDK Belgeleri gider.
* Azure depolama hesabınızda veri kopyalamak için **AzCopy** kullanın. Kullanımını görmek için girin **Azcopy** bir komut isteminde.
* Kullanım **Azure Depolama Gezgini** Azure depolama hesabınızdaki depoladığınız nesnelerin gidin. Ayrıca, verileri Azure Storage 'a ve Azure depolama 'ya kopyalar. Bu araca erişmek için **arama** alanına **Depolama Gezgini** girin. Veya Windows **Başlat** menüsünde bulabilirsiniz.
* **AdlCopy** verileri Azure Data Lake kopyalar. Kullanımını görmek için girin **adlcopy** bir komut isteminde.
* **dtuı** , bulutta bir NoSQL veritabanı olan ve Azure Cosmos DB verileri kopyalar. Girin **dtui** bir komut isteminde.
* **Azure Data Factory Integration Runtime** şirket içi veri kaynakları ile bulut arasında veri kopyalar. Azure Data Factory gibi araçları içinde kullanılır.
* PowerShell betik dilindeki Azure kaynaklarınızı yönetmek için **Microsoft Azure PowerShell** kullanın. Ayrıca, sanal makinenizde de yüklenir.

### <a name="power-bi"></a>Power BI

DSVM, panolar ve görselleştirmeler oluşturmanıza yardımcı olmak için **Power BI Desktop** yüklenmiş olarak gelir. Bu aracı panolarınızı ve raporlarınızı yazmak ve bunları buluta yayımlama için farklı kaynaklardan veri çekmek için kullanın. Daha fazla bilgi için [Power BI](https://powerbi.microsoft.com) site. Power BI Desktop, **Başlat** menüsünde bulabilirsiniz.

> [!NOTE]
> Power BI hizmetine erişmek için bir Microsoft Office 365 hesabınız olması.

### <a name="azure-machine-learning-service-python-sdk"></a>Azure Machine Learning hizmeti Python SDK'sı

Veri bilimcileri ve AI geliştiricileri, [Azure Machine Learning hizmeti](../service/overview-what-is-azure-ml.md)ile makine öğrenimi iş akışlarını derlemek ve çalıştırmak için Python IÇIN Azure Machine Learning SDK kullanır. TensorFlow ve scikit-öğren gibi açık kaynaklı çerçeveleri kullanarak Jupyıter not defterlerinde veya başka bir Python IDE 'de hizmetle etkileşim kurabilirsiniz.

Python SDK'sını kullanmaya başlamak için bkz: [Python kullanarak Azure Machine Learning'i kullanmaya başlamak için](../service/quickstart-create-workspace-with-python.md).

Python SDK'sı, Microsoft Veri bilimi sanal makinesi üzerinde önceden yüklenir.

## <a name="more-microsoft-development-tools"></a>Daha fazla Microsoft geliştirme araçları

Diğer Microsoft geliştirme araçlarını bulmak ve indirmek için [Microsoft Web Platformu Yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) kullanabilirsiniz. Ayrıca, Microsoft Veri Bilimi Sanal Makinesi Desktop 'ta aracın kısayolunu de bulabilirsiniz.  

## <a name="important-directories-on-the-vm"></a>VM üzerinde önemli dizinleri

| Öğe | Dizin |
| --- | --- |
| Jupyter not defteri sunucusu yapılandırmaları | C:\ProgramData\jupyter |
| Jupyter not defteri örnekleri giriş dizini | C:\dsvm\notebook ve c:\Users\\< kullanıcıadı\>\Not defterleri |
| Diğer örnekleri | C:\dsvm\samples |
| Anaconda, varsayılan: Python 3,6 | C:\Anaconda |
| Anaconda Python 2.7 ortamı | C:\Anaconda\envs\python2 |
| Microsoft Machine Learning sunucusu (tek başına) Python | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Varsayılan R örnek, Machine Learning sunucusu (tek başına) | C:\Program Files\Microsoft\ML Server\R_SERVER |
| SQL Machine Learning Hizmetleri veritabanında örneği dizini | C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Çeşitli araçlar | C:\dsvm\tools |

> [!NOTE]
> DSVM ve Windows Server 2016 sürümü Mart 2018'den önce Windows Server 2012 sürümünde varsayılan Anaconda Python 2.7 ortamıdır. İkincil ortam, **C:\anacondad\envs\py35**konumunda bulunan Python 3,5 ' dir.

## <a name="next-steps"></a>Sonraki adımlar

* Veri bilimi VM'si seçerek keşfedin **Başlat** menüsü.
* Azure Machine Learning hizmeti hakkında bilgi edinmek [Azure Machine Learning hizmeti nedir?](../service/overview-what-is-azure-ml.md) ve aşımına [hızlı başlangıçlar ve öğreticiler](../service/index.yml) kullanılabilir.
* Dosya Gezgini 'nde, kurumsal ölçekte veri analizlerini destekleyen R 'deki Iptal edilmiş bir kitaplığı kullanan örnekler için **C:\Program Files\Microsoft\ML Server\r_server\library\unsıscaler\demoscripts** konumuna gidin.  
* Makaleyi okuyun [veri bilimi sanal makinesi üzerinde yapabileceğiniz on işlem](https://aka.ms/dsvmtenthings).
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](../team-data-science-process/index.yml).
* Ziyaret [Azure AI Gallery](https://gallery.cortanaintelligence.com) Azure'da Azure Machine Learning ve ilgili verileri kullanan makine öğrenimi ve veri analizi için örnekleri Hizmetleri. Ayrıca bir simge için Bu galeri üzerinde sağladık **Başlat** menüsünde ve masaüstünde sanal makinenin.
