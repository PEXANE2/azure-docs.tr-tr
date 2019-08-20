---
title: Linux ve Windows için Azure Veri Bilimi Sanal Makinesi’ne Giriş | Microsoft Docs
description: Windows ve Linux Veri Bilimi Sanal Makineleri için önemli analiz senaryoları ve bileşenler.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/22/2019
ms.author: vijetaj
ms.openlocfilehash: 2814ad51d2f0f868833cf9c6964b7ea4a8424435
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69574917"
---
# <a name="what-is-azure-data-science-virtual-machine-for-linux-and-windows"></a>Linux ve Windows için Azure Veri Bilimi Sanal Makinesi nedir?

Veri Bilimi Sanal Makinesi (DSVM) veri bilimi için Microsoft Azure bulutunda derlenmiş olan özel bir sanal makine görüntüsüdür. Gelişmiş analiz için akıllı uygulamalar derlemeye hızlı giriş yapmak için önceden yüklenmiş ve önceden yapılandırılmış birçok popüler veri bilimi araçlarına ve diğer araçlara sahiptir. 

Aracı yapılandırmalarını, veri bilimcilerine ve geliştiricilere Microsoft'ta ve kararlılık ve genel letim gereksinimlerinin emin olmak için daha geniş veri bilimi topluluğu tarafından tarafından titizlikle test edilmez.

DSVM şu şirket üzerinde kullanılabilir:
+ Windows Server 2016, Windows Server 2012
+ Ubuntu 16,04 LTS ve CentOS 7,4

**Tüm derin öğrenme VM** araçları veri bilimi VM'si katlanmıştır. 


## <a name="what-can-i-do-with-dsvm"></a>DSVM ile ne yapabilirim?
Veri Bilimi Sanal Makinesi’nin (DSVM) amacı, tüm beceri düzeylerindeki ve farklı sektörlerdeki veri uzmanlarına sorunsuz, önceden yapılandırılmış ve tam tümleşik bir veri bilimi ortamı sağlamaktır. Kendi başınıza benzer bir çalışma alanını kullanıma sunmak yerine bir DSVM sağlayabilir, böylece yükleme, yapılandırma ve paket yönetimi süreçlerinde günler ve hatta _haftalar_ kazanabilirsiniz. DSVM ayrıldıktan sonra veri bilimi projenizin üzerinde çalışmaya hemen başlayabilirsiniz.

Veri Bilimi Sanal Makinesi çok çeşitli kullanım senaryoları ile birlikte çalışacak şekilde tasarlanmış ve yapılandırılmıştır. Proje gereksinimleriniz değiştikçe ortamınızın ölçeğini değiştirebilir veya azaltabilirsiniz. Ayrıca, veri bilimi görevlerini programlamak ve sistemi tam gereksinimlerinize göre özelleştirmek için diğer araçları yüklemek üzere tercih ettiğiniz dili de kullanabilirsiniz.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Bulutta önceden yapılandırılmış analiz masaüstü
Veri Bilimi Sanal Makinesi, yerel masaüstlerini yönetilen bir bulut masaüstü ile değiştirmek isteyen veri bilimi ekipleri için bir temel yapılandırma sağlar. Bu temel, bir ekipteki tüm veri bilimcilerinin denemeleri doğrulamak ve iş birliğini desteklemek üzere tutarlı bir kuruluma sahip olmasını sağlar. Ayrıca, sysadmin yükünü azaltarak maliyetleri düşürür. Bu yük azaltma, gelişmiş analizler için gereken çeşitli yazılım paketlerini değerlendirmek, yüklemek ve sürdürmek için gereken zamana kaydedilir.

### <a name="data-science-training-and-education"></a>Veri bilimi eğitimi
Veri bilimi sınıfları öğreten kurumsal traipers ve eğitimciler, genellikle bir sanal makine görüntüsü sağlar. Öğrenciler tutarlı bir kuruluma sahip olduğundan ve örneklerin öngörülebilir şekilde çalıştığından emin olmak için görüntü sağlarlar. Veri Bilimi Sanal Makinesi, destek ve uyumsuzluk sorunlarını kolaylaştıran tutarlı bir kurulumla isteğe bağlı bir ortam oluşturur. Başta daha kısa eğitim sınıfları olmak üzere bu ortamların sıklıkla oluşturulması gereken durumlar önemli ölçüde avantajlıdır.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Büyük ölçekli projeler için isteğe bağlı esnek kapasite
Veri bilimi yazılım yarışmaları/müsabakaları veya büyük ölçekli veri modelleme ve araştırmaları, genellikle kısa süreler için ölçeği genişletilmiş donanım kapasitesi gerektirir. Veri Bilimi VM'si, yüksek performanslı bilgi işlem kaynaklarının çalıştırılmasına izin veren genişleme sunucularında veri bilimi ortamının isteğe bağlı olarak hızla çoğaltılmasına yardımcı olabilir.

### <a name="custom-compute-power-for-azure-notebooks"></a>Azure Notebooks için özel işlem gücü
[Azure Notebooks](../../notebooks/azure-notebooks-overview.md) , bulutta JUPITER not defterlerini geliştirmek, çalıştırmak ve paylaşmak için ücretsiz bir barındırılan hizmettir. Ancak ücretsiz hizmet katmanı, 4 GB bellek ve 1 GB veri ile sınırlıdır. Tüm limitleri serbest bırakmak için bir Veri Bilimi VM'si veya Jupyıter sunucusu çalıştıran başka bir VM 'ye bir not defteri projesi ekleyebilirsiniz. Azure Active Directory (örneğin, bir kurumsal hesap) kullanarak Azure Notebooks oturum açarsanız, Not defterleri bu hesapla ilişkili tüm aboneliklerdeki veri bilimi sanal makinelerini otomatik olarak gösterir. Kullanılabilir işlem gücünü genişletmek için [Azure Notebooks veri bilimi VM'si](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier) ekleyebilirsiniz.

### <a name="short-term-experimentation-and-evaluation"></a>Kısa süreli deneme ve değerlendirme
Veri Bilimi Sanal Makinesi; Microsoft ML Server, SQL Server, Visual Studio araçları, Jupyter, derin öğrenme / ML araç setleri gibi araçları ve toplulukta popüler olan yeni araçları çok az kurulum çabasıyla değerlendirmek veya öğrenmek için kullanılabilir. Veri Bilimi VM'si hızlı bir şekilde ayarlanamadığından, bu, diğer kısa vadeli kullanım senaryolarında uygulanabilir. Bu senaryolar, çevrimiçi oturumlardaki ve konferans öğreticilerinde bulunan yayımlanmış denemeleri, tanıtımları yürüten bir çoğaltmayı içerir.

### <a name="deep-learning"></a>Derin öğrenme
Veri bilimi sanal makinesi, GPU (Grafik işleme birimleri) tabanlı donanımlar üzerinde derin öğrenme algoritmalarını kullanan eğitim modelleri için kullanılabilir. Azure bulutunun VM ölçeklendirme özelliklerinden yararlanan DSVM, gereksinime göre bulutta GPU tabanlı donanımlar kullanmanıza yardımcı olur. Büyük modellerin eğitimi veya aynı işletim sistemi diskini tutarken yüksek hızlı hesaplamalara ihtiyaç duyulması halinde GPU tabanlı bir sanal makineye geçiş yapılabilir.  DSVM’nin Windows Server 2016 sürümünde GPU sürücüleri, çerçeveleri ve derin öğrenme çerçevelerinin GPU sürümleri önceden yüklenmiş olarak gelir. Linux sürümünde, GPU üzerinde derin öğrenme hem CentOS hem de Ubuntu DSVM’leri üzerinde etkindir. Veri Bilimi VM'si Ubuntu, CentOS veya Windows 2016 sürümünü GPU tabanlı olmayan bir Azure sanal makinesine dağıtabilirsiniz. Bu durumda, tüm derin öğrenme çerçeveleri CPU moduna geri dönecektir. [Kullanılabilir derin öğrenme ve AI çerçeveleri](dsvm-deep-learning-ai-frameworks.md)hakkında daha fazla bilgi edinin.
 
[Kullanılabilir derin öğrenme ve AI çerçeveleri](dsvm-deep-learning-ai-frameworks.md)hakkında daha fazla bilgi edinin.

<a name="included"></a>

## <a name="whats-included-on-dsvm"></a>DSVM 'ye neler dahildir?
Veri Bilimi Sanal Makinesi halihazırda yüklenmiş ve yapılandırılmış olan çok sayıda popüler veri bilimi ve derin öğrenme aracı içerir. Ayrıca tahmine dayalı model derlemek için Microsoft ML Server (R, Python) veya büyük ölçekli veri kümeleriyle araştırma yapmak için SQL Server 2017 gibi çeşitli Azure veri ve analiz ürünleriyle çalışmayı kolaylaştıran araçlar da içerir. Veri Bilimi VM'si, açık kaynaklı topluluktaki ve Microsoft 'tan diğer araçların bir konağını, [örnek kod ve not defterlerini](dsvm-samples-and-walkthroughs.md)da içerir. 

Araçlar ve platformlar:
+ [Desteklenen programlama dilleri](dsvm-languages.md)

+ [Desteklenen veri platformları](dsvm-data-platforms.md)

+ [Geliştirme araçları ve Ides](dsvm-tools-development.md)

+ [Derin öğrenme ve AI çerçeveleri](dsvm-deep-learning-ai-frameworks.md).

+ [Machine Learning ve veri bilimi araçları](dsvm-ml-data-science-tools.md)

+ [Veri alımı araçları](dsvm-tools-ingestion.md)

+ [Veri keşif ve görselleştirme araçları](dsvm-tools-explore-and-visualize.md)

Aşağıdaki tabloda Veri Bilimi Sanal Makinesi’nin Windows ve Linux sürümlerine dahil olan ana bileşenler maddeler halinde verilmiş ve karşılaştırılmıştır.

| **Araç**                                                           | **Windows Sürümü** | **Linux Sürümü** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| Popüler paketlerin önceden yüklü olduğu [Microsoft R Open](https://mran.microsoft.com/open/)   |E                      | E             |
| [Microsoft ML Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) Geliştirici Sürümünün içeriği, <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) paralel ve dağıtılmış yüksek performanslı çerçeve (R ve Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package) - Microsoft’un yeni modern ML algoritmaları <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [R ve Python İşlemleştirme](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |E                      | E |
| [Microsoft Office](https://products.office.com/business/office-365-proplus-business-software) Profesyonel ve paylaşılan etkinleştirme ile Excel, Word ve PowerPoint   |E                      |N              |
| Popüler paketlerin önceden yüklü olduğu [Anaconda Python](https://www.continuum.io/) 2.7, 3.5    |E                      |E              |
| Julia diline yönelik popüler paketlerin önceden yüklü olduğu [JuliaPro](https://juliacomputing.com/products/juliapro.html)                         |E                      |E              |
| İlişkisel Veritabanları                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition (Ubuntu) |
| Veritabanı araçları                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * ODBC/JDBC sürücüleri| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (sorgulama aracı), <br /> * bcp, sqlcmd <br /> * ODBC/JDBC sürücüleri|
| SQL Server ML hizmetleri (R, Python) ile ölçeklenebilir veritabanı içi analiz | E     |N              |
| Aşağıdaki çekirdeklere sahip **[Jupyter Notebook Sunucusu](https://jupyter.org/),**                                  | E     | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Julia | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (sadece Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | E |
| JupyterHub (Çok kullanıcılı not defteri sunucusu)| N | E |
| JupyterLab (Çok kullanıcılı not defteri sunucusu) | N | Y (sadece Ubuntu) |
| **Geliştirme araçları, IDEs ve kod düzenleyicileri**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git eklentisi ile [Visual Studio 2019 (Community Edition)](https://www.visualstudio.com/community/) , Azure HDInsight (Hadoop), Data Lake, SQL Server veri araçları, [Node. js](https://github.com/Microsoft/nodejstools), [Python](https://aka.ms/ptvs)ve [Visual Studio için R araçları (rtvs)](https://microsoft.github.io/RTVS-docs/) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm Community Edition](https://www.jetbrains.com/pycharm/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Julia IDE)](https://junolab.org/)| E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim ve Emacs | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git ve GitBash | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* .NET Framework | E | N |
| Power BI Desktop | E | N |
| Azure ve Cortana Intelligence Hizmet paketine erişim SDK’ları | E | E |
| **Veri Taşıma ve yönetim araçları** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Depolama Gezgini | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azure CLI](https://docs.microsoft.com/cli/azure) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Blob FUSE sürücüsü](https://github.com/Azure/azure-storage-fuse) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy(Azure Data Lake Depolama)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [DocDB Veri Taşıma Aracı](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft veri yönetimi ağ geçidi](https://msdn.microsoft.com/library/dn879362.aspx): Şirket Içi ve bulut arasında veri taşıma | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Unix/Linux Komut Satırı Yardımcı Programları | E | E |
| Veri keşfi için [Apache Drill](https://drill.apache.org) | E | E |
| **Machine Learning Araçları** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) ile tümleştirme (R, Python) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](https://www.cs.waikato.ac.nz/ml/weka/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](https://togaware.com/rattle/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (sadece Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CatBoost](https://tech.yandex.com/catboost/) | N | Y (sadece Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/), [parlak su](https://www.h2o.ai/sparkling-water/) | N | Y (sadece Ubuntu) |
| **Derin Öğrenme Araçları** <br>Tüm araçlar GPU veya CPU üzerinde çalışır |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) (Windows 2016) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow](https://www.tensorflow.org/) | Y (Windows 2016) | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Horovod](https://github.com/uber/horovod) | N | Y (Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](https://mxnet.io/) | Y (Windows 2016) | E|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe & Caffe2](https://github.com/caffe2/caffe2) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Chainer](https://chainer.org/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyTorch](https://pytorch.org/)| N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVidia Digits](https://github.com/NVIDIA/DIGITS) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet Model Server](https://github.com/awslabs/mxnet-model-server) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow Serving](https://www.tensorflow.org/serving/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorRT](https://developer.nvidia.com/tensorrt) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CUDA, cuDNN, NVIDIA Sürücüsü](https://developer.nvidia.com/cuda-toolkit) | E | E |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerle daha fazla bilgi edinin:

+ Windows:
  + [Windows DSVM ayarlama](provision-vm.md)
  + [Windows DSVM üzerinde yapabileceğiniz on şey](vm-do-ten-things.md)

+ Linux:
  + [Linux DSVM 'YI ayarlama (Ubuntu)](dsvm-ubuntu-intro.md)
  + [Linux DSVM ayarlama (CentOS)](linux-dsvm-intro.md)
  + [Linux DSVM üzerinde veri bilimi](linux-dsvm-walkthrough.md)
