---
title: 'Hızlı Başlangıç: Windows oluştur'
titleSuffix: Azure Data Science Virtual Machine
description: Yapılandırma ve analiz için Azure'da bir veri bilimi sanal makinesi oluşturma ve makine öğrenimi.
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: quickstart
ms.date: 02/22/2019
ms.openlocfilehash: 6c0abc20775e604f9ea1c6ba882e5a9ff1d89e90
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195299"
---
# <a name="quickstart-set-up-a-windows-data-science-virtual-machine-on-azure"></a>Hızlı Başlangıç: Azure 'da Windows Veri Bilimi Sanal Makinesi ayarlama

Microsoft Windows Veri Bilimi Sanal Makinesi (DSVM), Azure 'da bir Windows Server 2016 sanal makinesi (VM) görüntüsüdür. Bu, önceden yüklenmiş ve veri analizi ve makine öğrenimi araçları ile yapılandırılmış olarak gelir.

## <a name="included-data-science-tools"></a>Dahil edilen veri bilimi araçları

Aşağıdaki araçlar DSVM 'ye eklenmiştir:

* [Azure Machine Learning hizmeti](../service/index.yml)IÇIN Python SDK.
* [Microsoft Machine Learning sunucusu](https://docs.microsoft.com/machine-learning-server/index) Geliştirici sürümü.
* Anaconda Python dağıtımı.
* R, Python ve PySpark çekirdekleri ile Jupyter not defteri.
* Microsoft Visual Studio topluluğu.
* Microsoft Power BI Desktop.
* Microsoft SQL Server 2017 Developer edition.
* Tek başına Apache Spark örneğinde yerel geliştirme ve test için.
* [JuliaPro](https://juliacomputing.com/products/juliapro.html).
* Makine öğrenimi ve veri analizi araçları:
  * Derin öğrenme çerçeveleri: [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), Mxnet ve KERAS AI çerçeveleri VM 'ye dahildir.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Hızlı makine çevrimiçi karma, allreduce, indirimleri, learning2search ve etkin ve etkileşimli öğrenme tekniklerle destekleyen sistemi öğrenme.
  * [Xgboost](https://xgboost.readthedocs.org/en/latest/): Hızlı ve doğru artırmalı ağaç uygulaması sağlayan bir araç.
  * [Rattle](https://togaware.com/rattle/). R 'de veri analizi ve makine öğrenimini kullanmaya başlamanızı sağlayan R analitik aracı. Otomatik R kod üretimi ile GUI tabanlı veri araştırması ve modelleme içerir.
  * [WEKA](https://www.cs.waikato.ac.nz/ml/weka/): Görsel verileri araştırma ve makine öğrenimi Java'da yazılım.
  * [Apache detaya gitme](https://drill.apache.org/): Bir şemasız SQL sorgu alt Apache Hadoop, NoSQL ve bulut depolama. ODBC ve JDBC arabirimleri, NoSQL ve Power BI, Microsoft Excel ve Tableau gibi standart BI araçlarından dosyaları'nı sorgulamak için destekler.
* Kitaplıklarında, R ve Python için Azure Machine Learning ve diğer Azure hizmetlerini kullanın.
* GitHub ve Azure DevOps gibi kaynak kodu depolarıyla çalışmak için git Bash dahil git. Git, git Bash içinde ve bir komut isteminde erişilebilen çeşitli popüler Linux komut satırı araçları sağlar. Awk, sed, perl, grep, bulma, wget ve curl örnek verilebilir.
* Geliştirme araçları ve düzenleyiciler (RStudio, Pyı).

### <a name="about-data-science"></a>Veri bilimi hakkında

Veri bilimi görevleri bir dizi üzerinde yineleme içerir:

1. Bulma, yükleme ve verileri ön işleme.
1. Yapı ve model test edin.
1. Akıllı uygulamalarda kullanılmak modelleri dağıtın.

Veri bilimcileri bu görevler için çeşitli araçlar kullanın. Zaman yazılım uygun sürümleri bulun ve sonra da indirin ve yükleyin alıcı olabilir. DSVM, birkaç popüler araç önceden yüklenmiş ve yapılandırılmış olarak Azure 'da sağlanabilen kullanıma hazır bir görüntü sağlayarak zaman kazandırır.

DSVM hızlı bir şekilde analiz projenizi başlatır. R, Python, SQL ve C#gibi çeşitli dillerdeki görevler üzerinde çalışabilirsiniz. Visual Studio geliştirme ve kodunuzu test etmek için kullanımı kolay tümleşik geliştirme ortamı (IDE) sağlar. Azure SDK, VM 'ye dahildir, bu sayede uygulamalarınızı Microsoft bulut platformunda Hizmetleri kullanarak oluşturabilirsiniz.

Bu DSVM görüntüsü için yazılım ücreti yok. Yalnızca Azure kullanım ücretleri ödeme yaparsınız. Bunlar, sağladığınız sanal makinenin boyutuna bağlıdır. Daha fazla bilgi için [veri bilimi sanal makinesi sayfasına](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows)bakın.

### <a name="other-dsvm-versions"></a>Diğer DSVM sürümleri

* Bir [Ubuntu](dsvm-ubuntu-intro.md) görüntü. DSVM 'ye benzer araçlara ve çok sayıda derin öğrenme çerçevelerine sahiptir.
* A [Linux CentOS](linux-dsvm-intro.md) görüntü.
* [Windows Server 2012 sürümü](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) , veri bilimi sanal makinesi. Bazı araçlar, yalnızca Windows Server 2016 sürümünde kullanılabilir. Aksi takdirde, bu makalede, Windows Server 2012 sürümüne de geçerlidir.

## <a name="prerequisite"></a>Önkoşul

Bir Microsoft Veri bilimi sanal makinesi oluşturmak için bir Azure aboneliğinizin olması gerekir. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.com/free).

## <a name="create-your-dsvm"></a>DSVM 'nizi oluşturma

DSVM örneği oluşturmak için:

1. [Azure Portal](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016)sanal makine listesine gidin. Henüz oturum açmadıysanız Azure hesabınızda oturum açmanız istenebilir.
1. Alt kısımdaki **Oluştur** düğmesini seçin.

   ![Azure portal, Oluştur düğmesi ile, sanal makine listesi](./media/provision-vm/configure-data-science-virtual-machine.png)

1. Ekran görüntüsünün sağ bölmesinde gösterilen adımların her birini yapılandırmak için aşağıdaki bilgileri girin:

   1. **Temel**:
      * **Ad**: DSVM 'nin adını girin.
      * **VM disk türü**: **SSD** veya **HDD**seçin. NVIDIA Tesla K80 tabanlı gibi bir NC_v1 GPU örneği için disk türü olarak **HDD** 'yi seçin.
      * **Kullanıcı adı**: Yönetici hesap KIMLIĞINI girin.
      * **Parola**: Yönetici hesabı parolasını girin.
      * **Abonelik**: Birden fazla aboneliğiniz varsa makinenin oluşturulacağı ve faturalandırılabileceği bir tane seçin.
      * **Kaynak grubu**: Yeni bir grup oluşturun veya var olan bir grubu kullanın.
      * **Konum**: En uygun veri merkezini seçin. En hızlı ağ erişimi için bu, verilerinizin çoğunu veya fiziksel konumunuza en yakın olan veri merkezdir.
   1. **Boyutu**: İşlevsel gereksinimlerinizi ve maliyet kısıtlamalarını karşılayan sunucu türünü seçin. Daha fazla VM boyutları için seçim **görünümü tüm**.
   1. **Ayarları**:  
      * **Yönetilen diskleri kullanma**. Seçin **yönetilen** VM için diskleri yönetmek için Azure istiyorsanız. Aksi takdirde, yeni veya mevcut bir depolama hesabı belirtmeniz gerekir.  
      * **Diğer parametreler**. Varsayılan değerleri kullanabilirsiniz. Varsayılan olmayan değerler kullanmak istiyorsanız, belirli alanlarla ilgili yardım için bilgilendirici bağlantının üzerine gelin.
   1. **Özet**: Girdiğiniz tüm bilgilerin doğru olduğundan emin olun. **Oluştur**’u seçin.

> [!NOTE]
> * VM, **Boyut** adımında seçtiğiniz sunucu boyutu için işlem maliyetinden daha fazla ücret vermez.
> * Sağlama 10 ila 20 dakika sürer. Azure portal sanal makinenizin durumunu görüntüleyebilirsiniz.

## <a name="access-the-dsvm"></a>DSVM 'ye erişme

VM oluşturulup sağlandıktan sonra, bir Uzak Masaüstü bağlantısı üzerinden erişebilirsiniz. Bir sanal makine oluşturmanın **temelleri** adımında yapılandırdığınız Yönetici hesabı kimlik bilgilerini kullanın. 

Yüklenmiş ve yapılandırılmış VM'de araçları kullanmaya başlamak hazırsınız demektir. Araçların çoğuna **Başlangıç** menüsü kutucukları ve Masaüstü simgeleri aracılığıyla erişilebilir.

Ayrıca, sanal makine üzerinde jupi not defterlerini çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Notebooks bir DSVM ekleyebilirsiniz. Daha fazla bilgi için bkz. [Not defteri projelerini yönetme ve yapılandırma](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

<a name="tools"></a>

## <a name="tools-installed-on-the-dvsm"></a>DVSM üzerine yüklenen araçlar

Aşağıdaki bölümlerde, Veri Bilimi Sanal Makinesi yüklenen araçlar hakkında daha fazla bilgi edinin.

### <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Geliştirici sürümü

SANAL makinede Machine Learning Server Geliştirici sürümü yüklü olduğundan, analizinize yönelik Microsoft Kurumsal kitaplığı 'nı kullanabilirsiniz. Daha önce Microsoft R Server olarak bilinen Machine Learning Server, yaygın olarak dağıtılabilir bir analiz platformudur. Ölçeklenebilir ve ticari olarak desteklenmektedir.

Machine Learning sunucusu, çeşitli büyük veri istatistikleri, Tahmine dayalı modelleme ve makine öğrenimi görevlerini destekler. Analytics tam aralığını destekler: Keşif, analiz, Görselleştirme ve modelleme. Kullanarak ve açık kaynak R ve Python, R ve Python betikleri ve işlevler ile Machine Learning sunucusu uyumludur. Kurumsal ölçekte verileri çözümlemek için CRAN, pip ve Conda paketleri ile de uyumludur.

Machine Learning sunucusu ekleme paralel ve öbeklenmiş veri işleme tarafından açık kaynaklı R'nin bellek içi sınırlamaları ele alır. Bu, analizi, ana bellekte uygun olandan çok daha büyük veriler üzerinde çalıştırabileceğiniz anlamına gelir. 

Visual Studio Community, VM'de dahil edilir. Visual Studio için R araçları ve R veya Python ile çalışmak üzere tam IDE sağlayan Visual Studio için Python Araçları (PTV) uzantılarına sahiptir. Ayrıca, sanal makine üzerinde [rstudio](https://www.rstudio.com) ve Pydüğme [Community Edition](https://www.jetbrains.com/pycharm/) gibi diğer Ides 'ler de sunuyoruz.

### <a name="python"></a>Python

Python kullanarak geliştirme için, Anaconda Python dağıtımları 2,7 ve 3,6 yüklenir. Bu dağıtımların yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra temel Python var. Visual Studio Community 2017 ' de yüklü olan PTV 'leri kullanabilirsiniz. Ya da boş veya Spyder gibi Anaconda ile paketlenmiş Ides 'leri de kullanabilirsiniz. Bu paketlerden birini arayın ve açın (Windows logo tuşu + S).

> [!NOTE]
> Anaconda Python 2.7, Visual Studio için Python Tools işaret etmek için her sürüm için özel ortamlarda oluşturmanız gerekir. Bu ortam yollarını Visual Studio 2017 Community 'de ayarlamak için **Araçlar** > **Python araçları** > **Python ortamları**' na gidin. Ardından **+ özel**.

Anaconda Python 3,6, C:\anacondadin altına yüklenmiştir. Anaconda Python 2,7, C:\Anaconda\envs\python2. altına yüklendi Ayrıntılı adımlar için bkz. [PTV belgeleri](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters).

### <a name="the-jupyter-notebook"></a>Jupyter not defteri

Anaconda dağıtımı, kod ve analiz paylaşmak için bir ortam olan Jupyter Notebook de birlikte gelir. Jupyter Notebook sunucusu ile Python 2.7 ve Python 3.x, PySpark, Julia ve R defterleri önceden yapılandırılmıştır. Jupyter sunucusunu başlatmak ve Not defteri sunucusuna erişmek için tarayıcıyı açmak üzere **Jupyter Notebook** masaüstü simgesini kullanın.

Biz, Python ve R'dir birkaç örnek not defterleri paketi Jupyter eriştikten sonra dizüstü aşağıdaki teknolojilerle çalışmanıza işlemini gösterir:

* Machine Learning Server
* SQL Server Machine Learning Services, veritabanı içi analiz
* Python
* Microsoft Bilişsel Araç Seti
* TensorFlow
* Diğer Azure teknolojileri

Daha önce oluşturduğunuz parolayı kullanarak Jupyter Notebook kimlik doğrulaması yaptıktan sonra Not defteri giriş sayfasında örneklerin bağlantısını bulabilirsiniz.

### <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

DSVM, Visual Studio Community 'YI içerir. Bu, Microsoft 'un değerlendirme amaçları ve küçük takımlar için kullanabileceğiniz, popüler IDE 'nin ücretsiz bir sürümüdür. Bkz. [Microsoft yazılımı lisans koşulları](https://www.visualstudio.com/support/legal/mt171547).

Masaüstü simgesini veya **Başlat** menüsünü kullanarak Visual Studio 'yu açın. Programlar için arama yapın (Windows logo tuşu + S) ve ardından **Visual Studio**. Buradan, Python, R ve Node. js gibi C#dillerde projeler oluşturabilirsiniz. Yüklü eklentiler aşağıdaki Azure Hizmetleri ile çalışmak uygun yapın:

* Azure Veri Kataloğu
* Hadoop ve Spark için Azure HDInsight
* Azure Data Lake

Visual Studio Code için Azure Machine Learning adlı eklenti Ayrıca Azure Machine Learning ile tümleşir ve hızlı bir şekilde AI uygulamaları oluşturmanıza yardımcı olur.

> [!NOTE]
> Değerlendirme süreniz doldu bir ileti alabilirsiniz. Microsoft hesabı kimlik bilgilerinizi girin. Veya Visual Studio Community erişim elde etmek için yeni bir ücretsiz hesap oluşturun.

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer edition

DSVM, Machine Learning Services SQL Server 2017 'in bir geliştirici sürümü ile birlikte gelir. Bu SQL Server sürümü R veya Python 'da gelir ve veritabanı içi analizler çalıştırabilir. 

Machine Learning Hizmetleri akıllı uygulama geliştirme ve dağıtma için bir platform sunar. Bu diller ve topluluktan gelen çok sayıda paketleri, modeller oluşturun ve SQL Server verilerinizi Öngörüler oluşturmak için kullanabilirsiniz. Machine Learning Hizmetleri veritabanında, SQL Server R ve Python dilleri tümleşir çünkü verilere yaklaştırılmasıyla analytics tutabilirsiniz. Bu tümleştirme, maliyet ve veri taşıma ile ilişkili güvenlik riskleri ortadan kaldırır.

> [!NOTE]
> SQL Server Developer sürümü yalnızca geliştirme ve test amaçları içindir. Üretim ortamında çalıştırmak için lisansa ihtiyacınız var.

SQL Server, Microsoft SQL Server Management Studio açarak erişebilirsiniz. VM adınız **sunucu adı**olarak doldurulur. Windows üzerinde yönetici olarak oturum açtığınızda, Windows kimlik doğrulaması kullanın. SQL Server Management Studio'da olduğunuzda, diğer kullanıcılar oluşturma, veritabanları oluşturma, veri içeri aktarın ve SQL sorgularını çalıştırın.

Veritabanı içi analizler SQL Server Machine Learning Services kullanarak etkinleştirmek için, sunucu yöneticisi olarak oturum açtıktan sonra SQL Server Management Studio ' de bir kerelik eylem olarak aşağıdaki komutu çalıştırın:

```
CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS
```

Sanal `%COMPUTERNAME%` makinenizin adıyla değiştirin.

### <a name="azure"></a>Azure

Çeşitli Azure Araçları VM'de yüklenir:

* Bir masaüstü kısayolu için Azure SDK Belgeleri gider.
* Azure depolama hesabınızda veri kopyalamak için AzCopy kullanın. Kullanımını görmek için girin **Azcopy** bir komut isteminde.
* Azure depolama hesabınızda depoladığınız nesnelere gözatmak için Azure Depolama Gezgini kullanın. Ayrıca, verileri Azure Storage 'a ve Azure depolama 'ya kopyalar. Bu araca erişmek için **arama** alanına **Depolama Gezgini** girin. Veya Windows üzerinde Bul **Başlat** menüsü.
* AdlCopy verileri Azure Data Lake kopyalar. Kullanımını görmek için girin **adlcopy** bir komut isteminde.
* Dbıı Aracı, bulutta bir NoSQL veritabanı olan ve Azure Cosmos DB verileri kopyalar. Girin **dtui** bir komut isteminde.
* Integration Runtime, verileri şirket içi veri kaynakları ve bulut arasında kopyalar. Azure Data Factory gibi araçları içinde kullanılır.
* PowerShell betik dilindeki Azure kaynaklarınızı yönetmek için Azure PowerShell kullanın. Ayrıca, sanal makinenizde de yüklenir.

### <a name="power-bi"></a>Power BI

DSVM, panolar ve görselleştirmeler oluşturmanıza yardımcı olmak için Power BI Desktop yüklenmiş olarak gelir. Bu aracı panolarınızı ve raporlarınızı yazmak ve bunları buluta yayımlama için farklı kaynaklardan veri çekmek için kullanın. Daha fazla bilgi için [Power BI sitesine](https://powerbi.microsoft.com)bakın. Power BI Desktop, **Başlat** menüsünde bulabilirsiniz.

> [!NOTE]
> Power BI hizmetine erişmek için bir Microsoft Office 365 hesabınız olması.

### <a name="azure-machine-learning-sdk-for-python"></a>Python için SDK Azure Machine Learning

Veri bilimcileri ve AI geliştiricileri, [Azure Machine Learning hizmeti](../service/overview-what-is-azure-ml.md)ile makine öğrenimi iş akışlarını derlemek ve çalıştırmak için Python IÇIN Azure Machine Learning SDK kullanır. TensorFlow ve scikit-öğren gibi açık kaynaklı çerçeveleri kullanarak Jupyıter not defterlerinde veya başka bir Python IDE 'de hizmetle etkileşim kurabilirsiniz.

Python SDK'sı, Microsoft Veri bilimi sanal makinesi üzerinde önceden yüklenir. Python SDK 'yı kullanmaya başlamak için bkz. [Azure Machine Learning ile çalışmaya başlamak Için Python kullanma](../service/quickstart-create-workspace-with-python.md).

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
| Python için Microsoft Machine Learning Server (tek başına) | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Varsayılan R örneği, Machine Learning Server (tek başına) | C:\Program Files\Microsoft\ML Server\R_SERVER |
| Veritabanı içi örnek dizinini SQL Server Machine Learning Services | C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Çeşitli araçlar | C:\dsvm\tools |

> [!NOTE]
> DSVM ve Windows Server 2016 sürümünün Windows Server 2012 sürümünde, 2018 Mart 'tan önce, varsayılan Anaconda ortamı Python 2,7 ' dir. İkincil ortam, C:\anacondad\envs\py35yolunda bulunan Python 3,5 ' dir.

## <a name="next-steps"></a>Sonraki adımlar

* **Başlat** menüsünü açarak dsvm 'deki araçları araştırın.
* [Azure Machine Learning hizmeti nedir?](../service/overview-what-is-azure-ml.md) ve hızlı başlangıçlara [ve öğreticilere](../service/index.yml)çalışırken Azure Machine Learning hizmeti hakkında bilgi edinin.
* Dosya Gezgini 'nde, kurumsal ölçekte veri analizlerini destekleyen R 'deki Iptal edilmiş bir kitaplığı kullanan örnekler için C:\Program Files\Microsoft\ML Server\r_server\library\unsıscaler\demoscripts konumuna göz atın. 
* [Veri bilimi sanal makinesi için kullanabileceğiniz on şey](https://aka.ms/dsvmtenthings)makalesini okuyun.
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](../team-data-science-process/index.yml).
* Ziyaret [Azure AI Gallery](https://gallery.cortanaintelligence.com) Azure'da Azure Machine Learning ve ilgili verileri kullanan makine öğrenimi ve veri analizi için örnekleri Hizmetleri. Ayrıca, bu Galeri için **Başlat** menüsünde ve sanal makinenin masaüstüne bir simge sağladık.
