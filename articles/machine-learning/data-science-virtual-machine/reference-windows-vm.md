---
title: 'Referans: Windows DSVM'
description: Windows Veri Bilimi VM'si dahil olan araçlarla ilgili ayrıntılar
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 09/11/2019
ms.topic: reference
ms.openlocfilehash: be4397de477891f2a698c9f7dcb131da79479529
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71200021"
---
# <a name="reference-windows-data-science-virtual-machine"></a>Referans: Windows Veri Bilimi Sanal Makinesi

Windows Veri Bilimi Sanal Makinesi kullanılabilen araçların listesi için aşağıya bakın. 

## <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Geliştirici sürümü

SANAL makinede Machine Learning Server Geliştirici sürümü yüklü olduğundan, analizinize yönelik Microsoft Kurumsal kitaplığı 'nı kullanabilirsiniz. Daha önce Microsoft R Server olarak bilinen Machine Learning Server, yaygın olarak dağıtılabilir bir analiz platformudur. Ölçeklenebilir ve ticari olarak desteklenmektedir.

Machine Learning sunucusu, çeşitli büyük veri istatistikleri, Tahmine dayalı modelleme ve makine öğrenimi görevlerini destekler. Analytics tam aralığını destekler: Keşif, analiz, Görselleştirme ve modelleme. Kullanarak ve açık kaynak R ve Python, R ve Python betikleri ve işlevler ile Machine Learning sunucusu uyumludur. Kurumsal ölçekte verileri çözümlemek için CRAN, pip ve Conda paketleri ile de uyumludur.

Machine Learning sunucusu ekleme paralel ve öbeklenmiş veri işleme tarafından açık kaynaklı R'nin bellek içi sınırlamaları ele alır. Bu, analizi, ana bellekte uygun olandan çok daha büyük veriler üzerinde çalıştırabileceğiniz anlamına gelir. 

Visual Studio Community, VM'de dahil edilir. Visual Studio için R araçları ve R veya Python ile çalışmak üzere tam IDE sağlayan Visual Studio için Python Araçları (PTV) uzantılarına sahiptir. Ayrıca, sanal makine üzerinde [rstudio](https://www.rstudio.com) ve [Pydüğme Community Edition](https://www.jetbrains.com/pycharm/) gibi diğer Ides 'ler de sunuyoruz.

## <a name="python"></a>Python

Python kullanarak geliştirme için, Anaconda Python dağıtımları 2,7 ve 3,6 yüklenir. Bu dağıtımların yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra temel Python var. Visual Studio Community 2017 ' de yüklü olan PTV 'leri kullanabilirsiniz. Ya da boş veya Spyder gibi Anaconda ile paketlenmiş Ides 'leri de kullanabilirsiniz. Bu paketlerden birini arayın ve açın (Windows logo tuşu + S).

> [!NOTE]
> Anaconda Python 2.7, Visual Studio için Python Tools işaret etmek için her sürüm için özel ortamlarda oluşturmanız gerekir. Bu ortam yollarını Visual Studio 2017 Community 'de ayarlamak için **Araçlar** > **Python araçları** > **Python ortamları**' na gidin. Ardından **+ özel**.

Anaconda Python 3,6, C:\anacondadin altına yüklenmiştir. Anaconda Python 2,7, C:\Anaconda\envs\python2. altına yüklendi Ayrıntılı adımlar için bkz. [PTV belgeleri](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters).

## <a name="the-jupyter-notebook"></a>Jupyter not defteri

Anaconda dağıtımı, kod ve analiz paylaşmak için bir ortam olan Jupyter Notebook de birlikte gelir. Jupyter Notebook sunucusu ile Python 2.7 ve Python 3.x, PySpark, Julia ve R defterleri önceden yapılandırılmıştır. Jupyter sunucusunu başlatmak ve Not defteri sunucusuna erişmek için tarayıcıyı açmak üzere **Jupyter Notebook** masaüstü simgesini kullanın.

Biz, Python ve R'dir birkaç örnek not defterleri paketi Jupyter eriştikten sonra dizüstü aşağıdaki teknolojilerle çalışmanıza işlemini gösterir:

* Machine Learning Server
* SQL Server Machine Learning Services, veritabanı içi analiz
* Python
* Microsoft Bilişsel Araç Seti
* TensorFlow
* Diğer Azure teknolojileri

Daha önce oluşturduğunuz parolayı kullanarak Jupyter Notebook kimlik doğrulaması yaptıktan sonra Not defteri giriş sayfasında örneklerin bağlantısını bulabilirsiniz.

## <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

DSVM, Visual Studio Community 'YI içerir. Bu, Microsoft 'un değerlendirme amaçları ve küçük takımlar için kullanabileceğiniz, popüler IDE 'nin ücretsiz bir sürümüdür. Bkz. [Microsoft yazılımı lisans koşulları](https://www.visualstudio.com/support/legal/mt171547).

Masaüstü simgesini veya **Başlat** menüsünü kullanarak Visual Studio 'yu açın. Programlar için arama yapın (Windows logo tuşu + S) ve ardından **Visual Studio**. Buradan, Python, R ve Node. js gibi C#dillerde projeler oluşturabilirsiniz. Yüklü eklentiler aşağıdaki Azure Hizmetleri ile çalışmak uygun yapın:

* Azure Veri Kataloğu
* Hadoop ve Spark için Azure HDInsight
* Azure Data Lake

Visual Studio Code için Azure Machine Learning adlı eklenti Ayrıca Azure Machine Learning ile tümleşir ve hızlı bir şekilde AI uygulamaları oluşturmanıza yardımcı olur.

> [!NOTE]
> Değerlendirme süreniz doldu bir ileti alabilirsiniz. Microsoft hesabı kimlik bilgilerinizi girin. Veya Visual Studio Community erişim elde etmek için yeni bir ücretsiz hesap oluşturun.

## <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer edition

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

## <a name="azure"></a>Azure

Çeşitli Azure Araçları VM'de yüklenir:

* Bir masaüstü kısayolu için Azure SDK Belgeleri gider.
* Azure depolama hesabınızda veri kopyalamak için AzCopy kullanın. Kullanımını görmek için girin **Azcopy** bir komut isteminde.
* Azure depolama hesabınızda depoladığınız nesnelere gözatmak için Azure Depolama Gezgini kullanın. Ayrıca, verileri Azure Storage 'a ve Azure depolama 'ya kopyalar. Bu araca erişmek için **arama** alanına **Depolama Gezgini** girin. Veya Windows üzerinde Bul **Başlat** menüsü.
* AdlCopy verileri Azure Data Lake kopyalar. Kullanımını görmek için girin **adlcopy** bir komut isteminde.
* Dbıı Aracı, bulutta bir NoSQL veritabanı olan ve Azure Cosmos DB verileri kopyalar. Girin **dtui** bir komut isteminde.
* Integration Runtime, verileri şirket içi veri kaynakları ve bulut arasında kopyalar. Azure Data Factory gibi araçları içinde kullanılır.
* PowerShell betik dilindeki Azure kaynaklarınızı yönetmek için Azure PowerShell kullanın. Ayrıca, sanal makinenizde de yüklenir.

## <a name="power-bi"></a>Power BI

DSVM, panolar ve görselleştirmeler oluşturmanıza yardımcı olmak için Power BI Desktop yüklenmiş olarak gelir. Bu aracı panolarınızı ve raporlarınızı yazmak ve bunları buluta yayımlama için farklı kaynaklardan veri çekmek için kullanın. Daha fazla bilgi için [Power BI sitesine](https://powerbi.microsoft.com)bakın. Power BI Desktop, **Başlat** menüsünde bulabilirsiniz.

> [!NOTE]
> Power BI hizmetine erişmek için bir Microsoft Office 365 hesabınız olması.

## <a name="azure-machine-learning-sdk-for-python"></a>Python için SDK Azure Machine Learning

Veri bilimcileri ve AI geliştiricileri, [Azure Machine Learning hizmeti](../service/overview-what-is-azure-ml.md)ile makine öğrenimi iş akışlarını derlemek ve çalıştırmak için Python IÇIN Azure Machine Learning SDK kullanır. TensorFlow ve scikit-öğren gibi açık kaynaklı çerçeveleri kullanarak Jupyıter not defterlerinde veya başka bir Python IDE 'de hizmetle etkileşim kurabilirsiniz.

Python SDK'sı, Microsoft Veri bilimi sanal makinesi üzerinde önceden yüklenir. Python SDK 'yı kullanmaya başlamak için bkz. [Azure Machine Learning ile çalışmaya başlamak Için Python kullanma](../service/quickstart-create-workspace-with-python.md).

## <a name="more-microsoft-development-tools"></a>Diğer Microsoft geliştirme araçları

Diğer Microsoft geliştirme araçlarını bulmak ve indirmek için [Microsoft Web Platformu Yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) kullanabilirsiniz. Ayrıca, Microsoft Veri Bilimi Sanal Makinesi Desktop 'ta aracın kısayolunu de bulabilirsiniz.  

## <a name="important-directories-on-the-virtual-machine"></a>Sanal makinedeki önemli dizinler

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

Başka sorularınız mı var? Bir [destek bileti](https://azure.microsoft.com/support/create-ticket/)oluşturmayı düşünün.
