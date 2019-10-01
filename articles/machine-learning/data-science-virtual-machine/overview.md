---
title: Azure Veri Bilimi Sanal Makinesi nedir?
description: Windows ve Linux veri bilimi sanal makineleri için anahtar analizi senaryoları ve bileşenleri.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi araçları, Linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: overview
ms.date: 02/22/2019
ms.openlocfilehash: f40ad01d74df26e5a6d90c348a706e8a3c666f2f
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71675069"
---
# <a name="what-is-the-azure-data-science-virtual-machine-for-linux-and-windows"></a>Linux ve Windows için Azure Veri Bilimi Sanal Makinesi nedir?

Veri Bilimi Sanal Makinesi (DSVM), veri bilimi yapmak için özel olarak oluşturulan Azure bulut platformunda özelleştirilmiş bir VM görüntüsüdür. Gelişmiş analiz için akıllı uygulamalar oluşturmaya başlamak üzere önceden yüklenmiş ve önceden yapılandırılmış birçok popüler veri bilimi ve diğer araçlar vardır. 

Araç konfigürasyonları, Microsoft 'ta ve daha geniş veri bilimi topluluğuyla veri bilimcileri ve geliştiricileri tarafından dikkatli bir şekilde test edilmiştir. Bu test, kararlılık ve genel vinme sağlanmasına yardımcı olur.

DSVM şu şirket üzerinde kullanılabilir:
+ Windows Server 2016, Windows Server 2012
+ Ubuntu 16,04 LTS ve CentOS 7,4

> [!NOTE]
> Derin öğrenme için tüm VM araçları Veri Bilimi Sanal Makinesi katlanmıştır. 


## <a name="what-can-i-do-with-the-dsvm"></a>DSVM ile ne yapabilirim?
Veri Bilimi Sanal Makinesi amacı, tüm beceri seviyelerinin ve sektörlerin tamamında, daha önceden yapılandırılmış ve tamamen tümleşik bir veri bilimi ortamıyla veri uzmanları sağlamaktır. Kendi kendinize benzer bir çalışma alanı kullanmak yerine, bir DSVM sağlayabilirsiniz. Bu seçenek, yükleme, yapılandırma ve paket yönetim işlemlerinde günlerinizi veya hatta _haftaları_ tasarruf edebilir. DSVM 'niz ayrıldıktan sonra, veri bilimi projenizde çalışmaya hemen başlayabilirsiniz.

DSVM, çok çeşitli kullanım senaryolarıyla çalışmak üzere tasarlanır ve yapılandırılır. Proje gereksinimleriniz değiştikçe ortamınızın ölçeğini değiştirebilir veya azaltabilirsiniz. Ayrıca, veri bilimi görevlerini programlamak ve sistem gereksinimlerinize göre özelleştirmek için diğer araçları yüklemek üzere tercih ettiğiniz dili de kullanabilirsiniz.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Bulutta önceden yapılandırılmış analiz Masaüstü
DSVM, yerel masaüstlerini yönetilen bir bulut Desktop ile değiştirmek isteyen veri bilimi takımları için bir temel yapılandırma sağlar. Bu taban çizgisi, bir takımda bulunan tüm veri bilimcilerinin denemeleri doğrulamak ve işbirliğini yükseltmek için tutarlı bir kuruluma sahip olmasını sağlar. Ayrıca, sysadmin yükünü azaltarak maliyetleri düşürür. Bu yük azaltma, gelişmiş analizler için yazılım paketlerini değerlendirmek, yüklemek ve sürdürmek için gereken zamana kaydedilir.

### <a name="data-science-training-and-education"></a>Veri bilimi eğitimi ve eğitimi
Veri bilimi sınıflarını öğreten kurumsal traers ve eğitimciler, genellikle bir sanal makine görüntüsü sağlar. Görüntü, öğrencilerin tutarlı bir kuruluma sahip olmasını ve örneklerin öngörülebilir bir şekilde çalışmasını sağlar. 

DSVM, destek ve uyumsuzluk sorunlarını ele alan tutarlı bir kurulum ile isteğe bağlı bir ortam oluşturur. Özellikle daha kısa eğitim sınıfları için bu ortamların sıklıkla oluşturulması gereken durumlar önemli ölçüde avantaj sağlar.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Büyük ölçekli projeler için isteğe bağlı elastik kapasite
Veri bilimi hack maratonları/yarışmaları veya büyük ölçekli veri modelleme ve araştırma, genellikle kısa süre için ölçekli donanım kapasitesi gerektirir. DSVM, yüksek performanslı bilgi işlem kaynaklarının çalışmasına izin veren ölçeği genişletilmiş sunucularda veri bilimi ortamını hızla çoğaltmaya yardımcı olabilir.

### <a name="custom-compute-power-for-azure-notebooks"></a>Azure Notebooks için özel işlem gücü
[Azure Notebooks](../../notebooks/azure-notebooks-overview.md) , bulutta JUPITER not defterlerini geliştirmek, çalıştırmak ve paylaşmak için ücretsiz bir barındırılan hizmettir. Ücretsiz hizmet katmanı, 4 GB bellek ve 1 GB veri ile sınırlıdır. 

Tüm limitleri serbest bırakmak için bir DSVM 'ye veya jupi sunucusu üzerinde çalışan başka bir VM 'ye bir not defteri projesi ekleyebilirsiniz. Azure Active Directory (örneğin, bir kurumsal hesap) kullanarak bir hesapla Azure Notebooks oturum açarsanız, Not defterleri bu hesapla ilişkili tüm aboneliklerde otomatik olarak DSVMs 'yi gösterir. Kullanılabilir işlem gücünü genişletmek için [Azure Notebooks BIR DSVM](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier) ekleyebilirsiniz.

### <a name="short-term-experimentation-and-evaluation"></a>Kısa vadeli deneme ve değerlendirme
En düşük kurulum çabasıyla bunlar gibi araçları değerlendirmek veya öğrenmek için DSVM 'yi kullanabilirsiniz:

- Microsoft Machine Learning Server
- SQL Server
- Visual Studio Araçları
- Jupyter
- Derin öğrenme ve makine öğrenimi araç takımları
- Topluluk içinde popüler yeni araçlar 

DSVM 'yi hızlıca ayarlayabildiğinden, diğer kısa vadeli kullanım senaryolarında bunu uygulayabilirsiniz. Bu senaryolar yayımlanmış denemeleri çoğaltma, tanıtımları yürütme ve çevrimiçi oturumlarda ve konferans öğreticilerinde aşağıdaki izlenecek yolların çoğaltılmasını içerir.

### <a name="deep-learning-with-gpus"></a>GPU 'Lar ile derin öğrenme
DSVM 'de, eğitim modelleriniz grafik işleme birimlerine (GPU 'Lar) dayalı donanımlar üzerinde derin öğrenme algoritmaları kullanabilir. DSVM, Azure platformunun VM ölçeklendirme özelliğinden yararlanarak, gereksinimlerinize göre bulutta GPU tabanlı donanımlar kullanmanıza yardımcı olur. Büyük modellere eğitim yaparken veya aynı işletim sistemi diskini korurken yüksek hızda hesaplamalar yapmanız gerektiğinde GPU tabanlı bir VM 'ye geçiş yapabilirsiniz. DSVM ile N serisi GPU etkin sanal makine SKU 'Larını seçebilirsiniz. Lütfen Azure Ücretsiz hesapları GPU etkin sanal makine SKU 'Larını desteklemez.

DSVM 'nin Windows Server 2016 sürümü, derin öğrenme çerçevelerinin GPU sürücüleri, çerçeveleri ve GPU sürümleriyle önceden yüklenmiş olarak gelir. Linux sürümünde, GPU 'Larda derin öğrenme hem CentOS hem de Ubuntu DSVMs üzerinde etkinleştirilmiştir. 

Ayrıca, DSVM 'nin Ubuntu, CentOS veya Windows 2016 sürümünü GPU 'lara dayalı olmayan bir Azure sanal makinesine dağıtabilirsiniz. Bu durumda, tüm derin öğrenme çerçeveleri CPU moduna geri dönecektir.
 
[Kullanılabilir derin öğrenme ve AI çerçeveleri hakkında daha fazla bilgi edinin](dsvm-deep-learning-ai-frameworks.md).

<a name="included"></a>

## <a name="whats-included-on-the-dsvm"></a>DSVM 'ye neler dahildir?
Veri Bilimi Sanal Makinesi çok sayıda popüler veri bilimi ve ayrıntılı öğrenme araçları zaten yüklü ve yapılandırılmış. Ayrıca, çeşitli Azure veri ve analiz ürünleriyle çalışmayı kolaylaştıran araçlar da içerir. Bu ürünler, tahmine dayalı modeller oluşturmak için Microsoft Machine Learning Server (R, Python) ve veri kümelerinin büyük ölçekli araştırması için 2017 SQL Server içerir. DSVM, açık kaynaklı topluluklardan ve Microsoft 'tan, [örnek kod ve Not defterleriyle](dsvm-samples-and-walkthroughs.md)birlikte diğer araçları içerir. 

Araç ve platformların bir listesi aşağıda verilmiştir:
+ [Desteklenen programlama dilleri](dsvm-languages.md)

+ [Desteklenen veri platformları](dsvm-data-platforms.md)

+ [Geliştirme araçları ve Ides](dsvm-tools-development.md)

+ [Derin öğrenme ve AI çerçeveleri](dsvm-deep-learning-ai-frameworks.md)

+ [Machine Learning ve veri bilimi araçları](dsvm-ml-data-science-tools.md)

+ [Veri alımı araçları](dsvm-tools-ingestion.md)

+ [Veri keşif ve görselleştirme araçları](dsvm-tools-explore-and-visualize.md)

Aşağıdaki tabloda Veri Bilimi Sanal Makinesi Windows ve Linux sürümlerindeki ana bileşenler yer almaktadır ve karşılaştırılır.

| **Aracı**                                                           | **Windows sürümü** | **Linux sürümü** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| Popüler paketlerle önceden yüklenmiş [Microsoft R Open](https://mran.microsoft.com/open/)   |Y                      | Y             |
| [Microsoft Machine Learning Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) Geliştirici sürümü şunları içerir: <br />  &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [iptal/Iptal eden](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) paralel ve dağıtılmış yüksek performanslı çerçeve (R ve Python)<br />  &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Microsoftml](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package), Microsoft 'tan yeni bir son teknoloji makine öğrenimi algoritmaları <br />  &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [R ve Python işlemleştirme](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |Y                      | Y |
| [Microsoft Office](https://products.office.com/business/office-365-proplus-business-software) Paylaşılan etkinleştirme ile ProPlus: Excel, Word ve PowerPoint   |Y                      |N              |
| Popüler paketler önceden yüklenmiş [Anaconda Python](https://www.continuum.io/) 2,7 ve 3,5    |Y                      |Y              |
| Julia dil için popüler paketler önceden yüklenmiş olarak [JuliaPro](https://juliacomputing.com/products/juliapro.html)                         |Y                      |Y              |
| İlişkisel veritabanları                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Geliştirici sürümü| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Geliştirici sürümü (Ubuntu) |
| Veritabanı araçları                                                       |  SQL Server Management Studio <br/> SQL Server Integration Services<br/> [BCP, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br />  ODBC/JDBC sürücüleri|  [SQUIRREL SQL](http://squirrel-sql.sourceforge.net/) (Sorgulama Aracı), <br />  BCP, sqlcmd <br />  ODBC/JDBC sürücüleri|
| SQL Server Machine Learning Hizmetleri (R, Python) ile ölçeklenebilir veritabanı Analizi | Y     |N              |
| Aşağıdaki çekirdekler ile [sunucu Jupyter Notebook](https://jupyter.org/) :                                  | Y     | Y |
|     &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 R | Y | Y |
|     &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 Python | Y | Y |
|     &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 Julia | Y | Y |
|     &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 PySpark | Y | Y |
|     &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [mini sihirli](https://github.com/jupyter-incubator/sparkmagic) | N | Y (yalnızca Ubuntu) |
|     &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 parlak r     | N | Y |
| JupyterHub (çok kullanıcılı Not defteri sunucusu)| N | Y |
| JupyterLab (çok kullanıcılı Not defteri sunucusu) | N | Y (yalnızca Ubuntu) |
| Geliştirme araçları, IDEs ve kod düzenleyicileri:| | |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Visual Studio 2019 (Community Edition)](https://www.visualstudio.com/community/) with git eklentisi, Azure HDInsight (Hadoop), Azure Data Lake, SQL Server veri araçları, [Node. js](https://github.com/Microsoft/nodejstools), [Python](https://aka.ms/ptvs)ve [Visual Studio için R araçları (rtvs)](https://microsoft.github.io/RTVS-docs/) | Y | N |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Visual Studio Code](https://code.visualstudio.com/) | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [rstudio masaüstü](https://www.rstudio.com/products/rstudio/#Desktop) | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [rstudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Pydüğmesinden topluluk sürümü](https://www.jetbrains.com/pycharm/) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [atom](https://atom.io/) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Juno (JULIA IDE)](https://junolab.org/)| Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 VIM ve Emacs | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 git ve git Bash | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 OpenJDK | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 .NET Framework | Y | N |
| Power BI Desktop | Y | N |
| Azure ve hizmet Cortana Intelligence Suite erişim için SDK 'lar | Y | Y |
| Veri taşıma ve yönetim araçları: | | |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 Azure Depolama Gezgini | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Azure CLI](https://docs.microsoft.com/cli/azure) | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 Azure PowerShell | Y | N |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | Y | N |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [BLOB sigortası sürücüsü](https://github.com/Azure/azure-storage-fuse) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [AdlCopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | Y | N |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Azure Cosmos DB veri geçiş aracı](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | Y | N |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Microsoft veri yönetimi Gateway](https://msdn.microsoft.com/library/dn879362.aspx): şirket içi ile bulut arasında veri taşıma | Y | N |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 UNIX/Linux komut satırı araçları | Y | Y |
| Veri araştırması için [Apache detaya gitme](https://drill.apache.org) | Y | Y |
| Machine Learning araçları: |||
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 tümleştirmesi [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Xgboost](https://github.com/dmlc/xgboost) | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [WEKA](https://www.cs.waikato.ac.nz/ml/weka/) | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Rattle](https://togaware.com/rattle/) | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Lightgbm](https://github.com/Microsoft/LightGBM) | N | Y (yalnızca Ubuntu) |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Catboost](https://tech.yandex.com/catboost/) | N | Y (yalnızca Ubuntu) |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [H2O](https://www.h2o.ai/h2o/), [parlak su](https://www.h2o.ai/sparkling-water/) | N | Y (yalnızca Ubuntu) |
| GPU veya CPU üzerinde çalışan derinlemesine öğrenme araçları: |  |  |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) (Windows 2016) | Y | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [TensorFlow](https://www.tensorflow.org/) | Y (Windows 2016) | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Horovod](https://github.com/uber/horovod) | N | Y (Ubuntu) |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Mxnet](https://mxnet.io/) | Y (Windows 2016) | Y|
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Caffe ve Caffe2](https://github.com/caffe2/caffe2) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Chainer](https://chainer.org/) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Torch](http://torch.ch/) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Theano](https://github.com/Theano/Theano) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [keras](https://keras.io/)| N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Pytorch](https://pytorch.org/)| N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [NVIDIA basamak](https://github.com/NVIDIA/DIGITS) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Mxnet model sunucusu](https://github.com/awslabs/mxnet-model-server) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [TensorFlow sunma](https://www.tensorflow.org/serving/) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [Tensorrt](https://developer.nvidia.com/tensorrt) | N | Y |
| &nbsp; @ no__t-1 @ no__t-2 @ no__t-3 [CUDA, cuDNN, NVIDIA sürücü](https://developer.nvidia.com/cuda-toolkit) | Y | Y |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerle daha fazla bilgi edinin:

+ Pencerelerin
  + [Windows DSVM ayarlama](provision-vm.md)
  + [Windows DSVM üzerinde yapabileceğiniz on şey](vm-do-ten-things.md)

+ 'Un
  + [Linux DSVM 'YI ayarlama (Ubuntu)](dsvm-ubuntu-intro.md)
  + [Linux DSVM ayarlama (CentOS)](linux-dsvm-intro.md)
  + [Linux DSVM üzerinde veri bilimi](linux-dsvm-walkthrough.md)
