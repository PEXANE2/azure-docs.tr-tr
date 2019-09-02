---
title: Azure Veri Bilimi Sanal Makinesi nedir?
description: Windows ve Linux Veri Bilimi Sanal Makineleri için önemli analiz senaryoları ve bileşenler.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: overview
ms.date: 02/22/2019
ms.openlocfilehash: 3295a59ee4496d332f0d886c89ca900ab6b4bcd1
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70191902"
---
# <a name="what-is-the-azure-data-science-virtual-machine-for-linux-and-windows"></a>Linux ve Windows için Azure Veri Bilimi Sanal Makinesi nedir?

Veri Bilimi Sanal Makinesi (DSVM), veri bilimi yapmak için özel olarak oluşturulan Azure bulut platformunda özelleştirilmiş bir VM görüntüsüdür. Gelişmiş analiz için akıllı uygulamalar derlemeye hızlı giriş yapmak için önceden yüklenmiş ve önceden yapılandırılmış birçok popüler veri bilimi araçlarına ve diğer araçlara sahiptir. 

Araç konfigürasyonları, Microsoft 'ta ve daha geniş veri bilimi topluluğuyla veri bilimcileri ve geliştiricileri tarafından dikkatli bir şekilde test edilmiştir. Bu test, kararlılık ve genel vinme sağlanmasına yardımcı olur.

DSVM şu şirket üzerinde kullanılabilir:
+ Windows Server 2016, Windows Server 2012
+ Ubuntu 16,04 LTS ve CentOS 7,4

> [!NOTE]
> Derin öğrenme için tüm VM araçları Veri Bilimi Sanal Makinesi katlanmıştır. 


## <a name="what-can-i-do-with-the-dsvm"></a>DSVM ile ne yapabilirim?
Veri Bilimi Sanal Makinesi amacı, tüm beceri seviyelerinin ve sektörlerin tamamında, daha önceden yapılandırılmış ve tamamen tümleşik bir veri bilimi ortamıyla veri uzmanları sağlamaktır. Kendi kendinize benzer bir çalışma alanı kullanmak yerine, bir DSVM sağlayabilirsiniz. Bu seçenek, yükleme, yapılandırma ve paket yönetim işlemlerinde günlerinizi veya hatta _haftaları_ tasarruf edebilir. DSVM ayrıldıktan sonra veri bilimi projenizin üzerinde çalışmaya hemen başlayabilirsiniz.

DSVM, çok çeşitli kullanım senaryolarıyla çalışmak üzere tasarlanır ve yapılandırılır. Proje gereksinimleriniz değiştikçe ortamınızın ölçeğini değiştirebilir veya azaltabilirsiniz. Ayrıca, veri bilimi görevlerini programlamak ve sistem gereksinimlerinize göre özelleştirmek için diğer araçları yüklemek üzere tercih ettiğiniz dili de kullanabilirsiniz.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Bulutta önceden yapılandırılmış analiz masaüstü
DSVM, yerel masaüstlerini yönetilen bir bulut Desktop ile değiştirmek isteyen veri bilimi takımları için bir temel yapılandırma sağlar. Bu temel, bir ekipteki tüm veri bilimcilerinin denemeleri doğrulamak ve iş birliğini desteklemek üzere tutarlı bir kuruluma sahip olmasını sağlar. Ayrıca, sysadmin yükünü azaltarak maliyetleri düşürür. Bu yük azaltma, gelişmiş analizler için yazılım paketlerini değerlendirmek, yüklemek ve sürdürmek için gereken zamana kaydedilir.

### <a name="data-science-training-and-education"></a>Veri bilimi eğitimi
Veri bilimi sınıflarını öğreten kurumsal traers ve eğitimciler, genellikle bir sanal makine görüntüsü sağlar. Görüntü, öğrencilerin tutarlı bir kuruluma sahip olmasını ve örneklerin öngörülebilir bir şekilde çalışmasını sağlar. 

DSVM, destek ve uyumsuzluk sorunlarını ele alan tutarlı bir kurulum ile isteğe bağlı bir ortam oluşturur. Başta daha kısa eğitim sınıfları olmak üzere bu ortamların sıklıkla oluşturulması gereken durumlar önemli ölçüde avantajlıdır.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Büyük ölçekli projeler için isteğe bağlı esnek kapasite
Veri bilimi hack maratonları/yarışmaları veya büyük ölçekli veri modelleme ve araştırma, genellikle kısa süre için ölçekli donanım kapasitesi gerektirir. DSVM, yüksek performanslı bilgi işlem kaynaklarının çalışmasına izin veren ölçeği genişletilmiş sunucularda veri bilimi ortamını hızla çoğaltmaya yardımcı olabilir.

### <a name="custom-compute-power-for-azure-notebooks"></a>Azure Notebooks için özel işlem gücü
[Azure Notebooks](../../notebooks/azure-notebooks-overview.md) , bulutta JUPITER not defterlerini geliştirmek, çalıştırmak ve paylaşmak için ücretsiz bir barındırılan hizmettir. Ücretsiz hizmet katmanı, 4 GB bellek ve 1 GB veri ile sınırlıdır. 

Tüm limitleri serbest bırakmak için bir DSVM 'ye veya jupi sunucusu üzerinde çalışan başka bir VM 'ye bir not defteri projesi ekleyebilirsiniz. Azure Active Directory (örneğin, bir kurumsal hesap) kullanarak bir hesapla Azure Notebooks oturum açarsanız, Not defterleri bu hesapla ilişkili tüm aboneliklerde otomatik olarak DSVMs 'yi gösterir. Kullanılabilir işlem gücünü genişletmek için [Azure Notebooks BIR DSVM](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier) ekleyebilirsiniz.

### <a name="short-term-experimentation-and-evaluation"></a>Kısa süreli deneme ve değerlendirme
En düşük kurulum çabasıyla bunlar gibi araçları değerlendirmek veya öğrenmek için DSVM 'yi kullanabilirsiniz:

- Microsoft Machine Learning Sunucusu
- SQL Server
- Visual Studio araçları
- Jupyter
- Derin öğrenme ve makine öğrenimi araç takımları
- Topluluk içinde popüler yeni araçlar 

DSVM 'yi hızlıca ayarlayabildiğinden, diğer kısa vadeli kullanım senaryolarında bunu uygulayabilirsiniz. Bu senaryolar yayımlanmış denemeleri çoğaltma, tanıtımları yürütme ve çevrimiçi oturumlarda ve konferans öğreticilerinde aşağıdaki izlenecek yolların çoğaltılmasını içerir.

### <a name="deep-learning"></a>Derin öğrenme
DSVM 'de, eğitim modelleriniz grafik işleme birimlerine (GPU 'Lar) dayalı donanımlar üzerinde derin öğrenme algoritmaları kullanabilir. DSVM, Azure platformunun VM ölçeklendirme özelliğinden yararlanarak, gereksinimlerinize göre bulutta GPU tabanlı donanımlar kullanmanıza yardımcı olur. Büyük modellere eğitim yaparken veya aynı işletim sistemi diskini korurken yüksek hızda hesaplamalar yapmanız gerektiğinde GPU tabanlı bir VM 'ye geçiş yapabilirsiniz.  

DSVM 'nin Windows Server 2016 sürümü, derin öğrenme çerçevelerinin GPU sürücüleri, çerçeveleri ve GPU sürümleriyle önceden yüklenmiş olarak gelir. Linux sürümünde, GPU 'Larda derin öğrenme hem CentOS hem de Ubuntu DSVMs üzerinde etkinleştirilmiştir. 

Ayrıca, DSVM 'nin Ubuntu, CentOS veya Windows 2016 sürümünü GPU 'lara dayalı olmayan bir Azure sanal makinesine dağıtabilirsiniz. Bu durumda, tüm derin öğrenme çerçeveleri CPU moduna geri dönecektir.
 
[Kullanılabilir derin öğrenme ve AI çerçeveleri hakkında daha fazla bilgi edinin](dsvm-deep-learning-ai-frameworks.md).

<a name="included"></a>

## <a name="whats-included-on-the-dsvm"></a>DSVM 'ye neler dahildir?
Veri Bilimi Sanal Makinesi halihazırda yüklenmiş ve yapılandırılmış olan çok sayıda popüler veri bilimi ve derin öğrenme aracı içerir. Ayrıca, çeşitli Azure ve analiz ürünleriyle çalışmayı kolaylaştıran araçlara sahiptir. Bu ürünler, tahmine dayalı modeller oluşturmak için Microsoft Machine Learning Server (R, Python) ve veri kümelerinin büyük ölçekli araştırması için 2017 SQL Server içerir. DSVM, açık kaynaklı topluluklardan ve Microsoft 'tan, [örnek kod ve Not defterleriyle](dsvm-samples-and-walkthroughs.md)birlikte diğer araçları içerir. 

Araç ve platformların bir listesi aşağıda verilmiştir:
+ [Desteklenen programlama dilleri](dsvm-languages.md)

+ [Desteklenen veri platformları](dsvm-data-platforms.md)

+ [Geliştirme araçları ve Ides](dsvm-tools-development.md)

+ [Derin öğrenme ve AI çerçeveleri](dsvm-deep-learning-ai-frameworks.md)

+ [Machine Learning ve veri bilimi araçları](dsvm-ml-data-science-tools.md)

+ [Veri alımı araçları](dsvm-tools-ingestion.md)

+ [Veri keşif ve görselleştirme araçları](dsvm-tools-explore-and-visualize.md)

Aşağıdaki tabloda Veri Bilimi Sanal Makinesi’nin Windows ve Linux sürümlerine dahil olan ana bileşenler maddeler halinde verilmiş ve karşılaştırılmıştır.

| **Araç**                                                           | **Windows sürümü** | **Linux sürümü** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| Popüler paketlerin önceden yüklü olduğu [Microsoft R Open](https://mran.microsoft.com/open/)   |E                      | E             |
| [Microsoft Machine Learning Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) Geliştirici sürümü şunları içerir: <br />  &nbsp;&nbsp;&nbsp;&nbsp;Geri [alınamaz/iptal edilebilir](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) paralel ve dağıtılmış yüksek performanslı çerçeve (R ve Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp;[Microsoftml](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package), Microsoft 'un yeni son teknoloji ürünü makine öğrenimi algoritmaları <br />  &nbsp;&nbsp;&nbsp;&nbsp;[R ve Python işlemleştirme](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |E                      | E |
| [Microsoft Office](https://products.office.com/business/office-365-proplus-business-software) Paylaşılan etkinleştirme ile ProPlus: Excel, Word ve PowerPoint   |E                      |N              |
| Popüler paketler önceden yüklenmiş [Anaconda Python](https://www.continuum.io/) 2,7 ve 3,5    |E                      |E              |
| Julia diline yönelik popüler paketlerin önceden yüklü olduğu [JuliaPro](https://juliacomputing.com/products/juliapro.html)                         |E                      |E              |
| İlişkisel veritabanları                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition (Ubuntu) |
| Veritabanı araçları                                                       |  SQL Server Management Studio <br/> SQL Server Integration Services<br/> [BCP, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br />  ODBC/JDBC sürücüleri|  [SQUIRREL SQL](http://squirrel-sql.sourceforge.net/) (Sorgulama Aracı), <br />  BCP, sqlcmd <br />  ODBC/JDBC sürücüleri|
| SQL Server Machine Learning Hizmetleri (R, Python) ile ölçeklenebilir veritabanı Analizi | E     |N              |
| Aşağıdaki çekirdekler ile [sunucu Jupyter Notebook](https://jupyter.org/) :                                  | E     | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;SAĞ | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;Python | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;J | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;PySpark | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;[Mini sihirli](https://github.com/jupyter-incubator/sparkmagic) | N | Y (sadece Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;SparkR     | N | E |
| JupyterHub (çok kullanıcılı Not defteri sunucusu)| N | E |
| JupyterLab (çok kullanıcılı Not defteri sunucusu) | N | Y (sadece Ubuntu) |
| Geliştirme araçları, IDEs ve kod düzenleyicileri:| | |
| &nbsp;&nbsp;&nbsp;&nbsp;Git eklentisi ile [Visual Studio 2019 (Community Edition)](https://www.visualstudio.com/community/) , Azure HDInsight (Hadoop), Azure Data Lake, SQL Server veri araçları, [Node. js](https://github.com/Microsoft/nodejstools), [Python](https://aka.ms/ptvs)ve [Visual Studio için R araçları (rtvs)](https://microsoft.github.io/RTVS-docs/) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;[Visual Studio Code](https://code.visualstudio.com/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Rstudio masaüstü](https://www.rstudio.com/products/rstudio/#Desktop) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Rstudio sunucusu](https://www.rstudio.com/products/rstudio/#Server) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Pyın Community sürümü](https://www.jetbrains.com/pycharm/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Atom](https://atom.io/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Juno (JULIA IDE)](https://junolab.org/)| E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;VIM ve Emacs | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;Git ve git Bash | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;OpenJDK | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;.NET Framework | E | N |
| Power BI Desktop | E | N |
| Azure ve Cortana Intelligence Hizmet paketine erişim SDK’ları | E | E |
| Veri taşıma ve yönetim araçları: | | |
| &nbsp;&nbsp;&nbsp;&nbsp;Azure Depolama Gezgini | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Azure CLI](https://docs.microsoft.com/cli/azure) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;Azure PowerShell | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;[AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;[BLOB sigorta sürücüsü](https://github.com/Azure/azure-storage-fuse) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[AdlCopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;[Azure Cosmos DB veri geçiş aracı](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;[Microsoft veri yönetimi Gateway](https://msdn.microsoft.com/library/dn879362.aspx): şirket içi ve bulut arasında veri taşıma | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;UNIX/Linux komut satırı araçları | E | E |
| Veri araştırması için [Apache detaya gitme](https://drill.apache.org) | E | E |
| Machine Learning araçları: |||
| &nbsp;&nbsp;&nbsp;&nbsp;[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) ile tümleştirme (R, Python) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Xgboost](https://github.com/dmlc/xgboost) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[WEKA](https://www.cs.waikato.ac.nz/ml/weka/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Rattle](https://togaware.com/rattle/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Lightgbm](https://github.com/Microsoft/LightGBM) | N | Y (sadece Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Catboost](https://tech.yandex.com/catboost/) | N | Y (sadece Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;[H2O](https://www.h2o.ai/h2o/), [parlak su](https://www.h2o.ai/sparkling-water/) | N | Y (sadece Ubuntu) |
| GPU veya CPU üzerinde çalışan derinlemesine öğrenme araçları: |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;[Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) (Windows 2016) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[TensorFlow](https://www.tensorflow.org/) | Y (Windows 2016) | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Horovod](https://github.com/uber/horovod) | N | Y (Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Mxnet](https://mxnet.io/) | Y (Windows 2016) | E|
| &nbsp;&nbsp;&nbsp;&nbsp;[Caffe ve Caffe2](https://github.com/caffe2/caffe2) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Chainer](https://chainer.org/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Torch](http://torch.ch/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Theano](https://github.com/Theano/Theano) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Keras](https://keras.io/)| N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Pytorch](https://pytorch.org/)| N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[NVIDIA rakamları](https://github.com/NVIDIA/DIGITS) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Mxnet model sunucusu](https://github.com/awslabs/mxnet-model-server) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[TensorFlow hizmeti](https://www.tensorflow.org/serving/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[Tensorrt](https://developer.nvidia.com/tensorrt) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;[CUDA, cuDNN, NVIDIA sürücüsü](https://developer.nvidia.com/cuda-toolkit) | E | E |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerle daha fazla bilgi edinin:

+ Windows:
  + [Windows DSVM’si ayarlama](provision-vm.md)
  + [Windows DSVM üzerinde yapabileceğiniz on şey](vm-do-ten-things.md)

+ Linux:
  + [Linux DSVM 'YI ayarlama (Ubuntu)](dsvm-ubuntu-intro.md)
  + [Linux DSVM ayarlama (CentOS)](linux-dsvm-intro.md)
  + [Linux DSVM üzerinde veri bilimi](linux-dsvm-walkthrough.md)
