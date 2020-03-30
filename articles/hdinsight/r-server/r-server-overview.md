---
title: Azure HDInsight'ta ML Hizmetlerine Giriş
description: Büyük veri analizi için uygulamalar oluşturmak için HDInsight'ta ML Hizmetlerini nasıl kullanacağınızı öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: overview
ms.date: 02/10/2020
ms.openlocfilehash: a77771880da962298f6e80782e5f3e251f5f4641
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77122362"
---
# <a name="what-is-ml-services-in-azure-hdinsight"></a>Azure HDInsight'ta ML Hizmetleri Nedir

Microsoft Machine Learning Server, Azure'da HDInsight kümeleri oluşturduğunuzda dağıtım seçeneği olarak kullanılabilir. Bu seçeneği sağlayan küme **türüNE ML Hizmetleri**denir. Bu özellik, HDInsight'ta ölçeklenebilir, dağıtılmış analiz yöntemlerine isteğe bağlı erişim sağlayan veri bilimcileri, istatistikçiler ve R programcıları sağlar.

HDInsight'taki ML Hizmetleri, Azure Blob veya Data Lake depolamasına yüklenen hemen hemen her boyuttaki veri kümelerinde R tabanlı analitik için en son özellikleri sağlar. ML Hizmetleri kümesi açık kaynak R üzerine kurulduğundan, oluşturduğunuz R tabanlı uygulamalar 8000+ açık kaynak R paketinden herhangi birini kullanabilir. Microsoft'un büyük veri analizi paketi ScaleR'daki rutinler de mevcuttur.

Bir kümenin kenar düğümü kümeye bağlanmak ve R komut dosyalarınızı çalıştırmak için uygun bir yer sağlar. Kenar düğümü ile, ScaleR'ın paralelleştirilmiş dağıtılmış işlevlerini kenar düğümü sunucusunun çekirdekleri arasında çalıştırma seçeneğiniz vardır. ScaleR'ın Hadoop Harita Azaltma veya Apache Spark bilgi işlem bağlamlarını kullanarak kümenin düğümleri üzerinde de çalıştırabilirsiniz.

Çözümlemeden kaynaklanan modeller veya öngörüler şirket içi kullanım için indirilebilir. Ayrıca Azure'un başka bir yerinde, özellikle [Azure Machine Learning Studio (klasik)](https://studio.azureml.net) web [hizmeti](../../machine-learning/studio/deploy-a-machine-learning-web-service.md)aracılığıyla da işletilebilir.

## <a name="get-started-with-ml-services-on-hdinsight"></a>HDInsight'ta ML Hizmetleri ile başlayın

Azure HDInsight'ta bir ML Hizmetleri kümesi oluşturmak için, Azure portalını kullanarak bir HDInsight kümesi oluştururken **ML Hizmetleri** küme türünü seçin. ML Hizmetleri küme türü, kümenin veri düğümlerinde ve ML Hizmetleri tabanlı analizler için iniş bölgesi olarak hizmet veren bir kenar düğümünde ML Server'ı içerir. Bkz. Kümenin nasıl oluşturulacağını öğrenmek için [Azure portalını kullanarak Apache Hadoop kümeleri](../hdinsight-hadoop-create-linux-clusters-portal.md) oluşturun.

## <a name="why-choose-ml-services-in-hdinsight"></a>NEDEN HDInsight'ta ML Hizmetleri'ni seçmelisiniz?

HDInsight'taki ML Hizmetleri aşağıdaki avantajları sağlar:

### <a name="ai-innovation-from-microsoft-and-open-source"></a>Microsoft ve açık kaynak tan ai yenilik

  ML Hizmetleri, fiziksel belleğin boyutundan daha büyük veri boyutları üzerinde çalışabilen ve dağıtılmış bir şekilde çok çeşitli platformlarda çalışabilen [RevoscaleR,](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package)ve [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) gibi yüksek ölçeklenebilir, dağıtılabilir algoritmalar kümesi içerir. Microsoft'un ürünle birlikte verilen özel [R paketleri](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) ve Python [paketlerinin](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) koleksiyonu hakkında daha fazla bilgi edinin.
  
  ML Services, bu Microsoft yeniliklerini ve açık kaynak topluluğundan (R, Python ve AI araç kitleri) gelen katkıları tek bir kurumsal sınıf platformun üzerine köprüler kurabilir. Herhangi bir R veya Python açık kaynak makine öğrenme paketi, Microsoft'un herhangi bir özel yeniliğiyle yan yana çalışabilir.

### <a name="simple-secure-and-high-scale-operationalization-and-administration"></a>Basit, güvenli ve yüksek ölçekli operasyonelleştirme ve yönetim

  Geleneksel paradigmalara ve ortamlara güvenen işletmeler operasyonelleşme için çok fazla zaman ve çaba harvetmektedir. Bu, modellerin çeviri süresi, bunları geçerli ve güncel tutmak için yinelemeler, düzenleyici onay ve işletmeleştirme yoluyla izinlerin yönetilmesi de dahil olmak üzere şişirilmiş maliyetler eve gecikmelere neden olur.

  ML Hizmetleri kurumsal sınıf [operasyonelleştirme](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)sunuyor, bu, bir makine öğrenme modeli tamamlandıktan sonra, web hizmetleri API'leri oluşturmak için sadece birkaç tıklama alır. Bu [web hizmetleri](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) buluttaki bir sunucu ızgarasında barındırılır ve iş yeri uygulamalarıyla tümleştirilebilir. Elastik bir ızgaraya dağıtma yeteneği, hem toplu hem de gerçek zamanlı puanlama için işletmenizin gereksinimleriyle sorunsuz bir şekilde ölçeklendirmenize olanak tanır. Talimatlar için [HDInsight'ta ML Hizmetlerini Operasyonelleştir'e](r-server-operationalize.md)bakın.

<!---
* **Deep ecosystem engagements to deliver customer success with optimal total cost of ownership**

  Individuals embarking on the journey of making their applications intelligent or simply wanting to learn the new world of AI and machine learning, need the right resources to help them get started. In addition to this documentation, Microsoft provides several learning resources and has engaged several training partners to help you ramp up and become productive quickly.
--->

> [!NOTE]  
> HDInsight'taki ML Hizmetleri küme türü yalnızca HDInsight 3.6'da desteklenir. HDInsight 3.6'nın 31 Aralık 2020'de emekli olması planlanıyor.

## <a name="key-features-of-ml-services-on-hdinsight"></a>HDInsight'ta ML Hizmetlerinin temel özellikleri

Aşağıdaki özellikler HDInsight'taki ML Services'a dahildir.

| Özellik kategorisi | Açıklama |
|------------------|-------------|
| R özellikli | R ile yazılmış çözümler için [R paketleri,](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) R'nin açık kaynak dağılımı ve komut dosyası yürütme için çalışma zamanı altyapısı. |
| Python özellikli | Python'da yazılan çözümler için [Python modülleri,](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) Python'un açık kaynak dağılımı ve komut dosyası yürütme için çalışma zamanı altyapısı.
| [Önceden eğitilmiş modeller](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Görsel analiz ve metin duyarlılığı analizi için, sağladığınız verileri puana hazır. |
| [Dağıtma ve tüket](r-server-operationalize.md) | Sunucunuzu operasyonel hale getirin ve çözümleri bir web hizmeti olarak dağıtın. |
| [Uzaktan yürütme](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-services) | İstemci iş istasyonunuzdan ağınızdaki ML Hizmetleri kümesinde uzak oturumları başlatın. |


## <a name="data-storage-options-for-ml-services-on-hdinsight"></a>HDInsight'ta ML Hizmetleri için veri depolama seçenekleri

HDInsight kümelerinin HDFS dosya sistemi için varsayılan depolama, bir Azure Depolama hesabı veya Azure Veri Gölü Depolama ile ilişkilendirilebilir. Bu ilişkilendirme, çözümleme sırasında küme depolamasına yüklenen her türlü verinin kalıcı olmasını ve küme silindikten sonra bile verilerin kullanılabilir olmasını sağlar. Depolama hesabının portal tabanlı yükleme tesisi ve [AzCopy](../../storage/common/storage-use-azcopy.md) yardımcı programı da dahil olmak üzere, seçtiğiniz depolama seçeneğine veri aktarımını işlemek için çeşitli araçlar vardır.

Kullanılan birincil depolama seçeneğine bakılmaksızın küme sağlama işlemi sırasında ek Blob ve Veri göl depolarına erişim sağlama seçeneğiniz vardır.  Birden çok depolama hesabı kullanma hakkında daha fazla bilgi edinmek [için HDInsight makalesinde ML Hizmetleri için Azure Depolama seçeneklerine](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) bakın.

Kenar düğümünde kullanılmak üzere bir depolama seçeneği olarak [Azure Dosyaları'nı](../../storage/files/storage-how-to-use-files-linux.md) da kullanabilirsiniz. Azure Dosyaları, Azure Depolama'da oluşturulan bir dosya paylaşımını Linux dosya sistemine eklemenize olanak tanır. HDInsight kümesinde ML Hizmetleri için bu veri depolama seçenekleri hakkında daha fazla bilgi [için, HDInsight'ta ML Hizmetleri için Azure Depolama seçeneklerine](r-server-storage.md)bakın.

## <a name="access-ml-services-edge-node"></a>Erişim ML Hizmetleri kenar düğümü

Bir tarayıcı kullanarak kenar düğümünde Microsoft ML Server'a bağlanabilirsiniz. Küme oluşturma sırasında varsayılan olarak yüklenir.  Ayrıca R konsoluna erişmek için SSH/PuTTY'yi kullanarak komut satırından küme kenar düğümüne bağlanabilirsiniz.

## <a name="develop-and-run-r-scripts"></a>R komut dosyaları geliştirme ve çalıştırma

Oluşturduğunuz ve çalıştırdığınız R komut dosyaları, ScaleR kitaplığında bulunan paralelleştirilmiş ve dağıtılmış yordamlara ek olarak 8000'den fazla açık kaynak R paketinden herhangi birini kullanabilir. Genel olarak, kenar düğümünde ML Hizmetleri ile çalıştırılabilen bir komut dosyası, bu düğümüzerinde R yorumlayıcısı içinde çalışır. İstisnalar, Hadoop Harita Küçültme (RxHadoopMR) veya Kıvılcım (RxSpark) olarak ayarlanmış bir bilgi işlem bağlamı olan bir ScaleR işlevini çağırması gereken adımlardır. Bu durumda, işlev, başvurulan verilerle ilişkili kümenin bu veri (görev) düğümleri arasında dağıtılmış bir şekilde çalışır. Farklı bilgi işlem bağlamı seçenekleri hakkında daha fazla bilgi için, [HDInsight'ta ML Hizmetleri için Bilgi İşlem bağlamı seçeneklerine](r-server-compute-contexts.md)bakın.

## <a name="operationalize-a-model"></a>Bir modeli kullanıma hazır hale getirme

Veri modellemeniz tamamlandığında, azure'dan veya şirket içinde yeni veriler için öngörülerde bulunmak için modeli işlevsel hale getirebilirsiniz. Bu işlem puanlama olarak bilinir. Puanlama HDInsight, Azure Machine Learning veya şirket içinde yapılabilir.

### <a name="score-in-hdinsight"></a>HDInsight'ta puan

HDInsight'ta puan almak için, depolama hesabınıza yüklediğiniz yeni bir veri dosyası için öngörülerde bulunmak için modelinizi çağıran bir R işlevi yazın. Ardından, tahminleri depolama hesabına geri kaydedin. Bu yordamı isteğe bağlı olarak kümenizin kenar düğümünde veya zamanlanmış bir iş kullanarak çalıştırabilirsiniz.

### <a name="score-in-azure-machine-learning-aml"></a>Azure Machine Learning'de Puan (AML)

Azure Machine Learning'i kullanarak puan almak için, modelinizi Azure web hizmeti olarak yayımlamak için [AzureML](https://cran.r-project.org/src/contrib/Archive/AzureML/) olarak bilinen açık kaynak Azure Machine Learning R paketini kullanın. Kolaylık sağlamak için, bu paket kenar düğümüne önceden yüklenir. Ardından, web hizmeti için bir kullanıcı arabirimi oluşturmak için Azure Machine Learning'deki tesisleri kullanın ve ardından puanlama için gerektiğinde web hizmetini arayın.

Bu seçeneği seçerseniz, herhangi bir ScaleR model nesnelerini web hizmetinde kullanılmak üzere eşdeğer açık kaynak model nesnelerine dönüştürmeniz gerekir. Bu dönüştürme için topluluk tabanlı modeller `as.randomForest()` gibi ScaleR zorlama işlevlerini kullanın.

### <a name="score-on-premises"></a>Şirket içinde puan

Modelinizi oluşturduktan sonra şirket içinde puan elde etmek için modeli R'de serihale getirebilir, indirebilir, seri olarak de-serialize edebilir ve yeni veri puanlamak için kullanabilirsiniz. HDInsight'ta Daha önce [Score'da](#score-in-hdinsight) açıklanan yaklaşımı kullanarak veya [web hizmetlerini](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services)kullanarak yeni veriler elde edebilirsiniz.

## <a name="maintain-the-cluster"></a>Kümeyi koruyun

### <a name="install-and-maintain-r-packages"></a>R paketlerini yükleme ve bakımını

R komut dosyalarınızın çoğu adımları burada çalıştırdığından, kullandığınız R paketlerinin çoğu kenar düğümünde gereklidir. Kenar düğümüne ek R paketleri yüklemek için, `install.packages()` Yöntemi R'de kullanabilirsiniz.

Kümedeki ScaleR kitaplığından gelen yordamları yalnızca kullanıyorsanız, genellikle veri düğümlerine ek R paketleri yüklemeniz gerekmez. Ancak, veri düğümlerinde **rxExec** veya **RxDataStep** yürütme kullanımını desteklemek için ek paketler gerekebilir.

Bu gibi durumlarda, kümeyi oluşturduktan sonra ek paketler komut dosyası eylemiyle yüklenebilir. Daha fazla bilgi için [HDInsight kümesinde ML Hizmetlerini Yönet'e](r-server-hdinsight-manage.md)bakın.

### <a name="change-apache-hadoop-mapreduce-memory-settings"></a>Apache Hadoop Haritasını DeğiştirBellek ayarlarını azaltın

Bir küme, Bir MapReduce işini çalıştırırken ML Hizmetleri için kullanılabilen bellek miktarını değiştirmek için değiştirilebilir. Bir kümeyi değiştirmek için, kümeniz için Azure portal bıçak aracılığıyla kullanılabilen Apache Ambari UI'yi kullanın. Kümeniz için Ambari UI'ye nasıl erişeceğimize ilişkin talimatlar için [Ambari Web UI'yi kullanarak HDInsight kümelerini yönet'e](../hdinsight-hadoop-manage-ambari.md)bakın.

**RxHadoopMR'a** yapılan çağrıda Hadoop anahtarları kullanılarak ML Services'in kullanabileceği bellek miktarını aşağıdaki gibi değiştirmek de mümkündür:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Kümenizi ölçeklendirin

HDInsight'taki mevcut bir ML Hizmetleri kümesi portal üzerinden yukarı veya aşağı ölçeklendirilebilir. Ölçeklendirerek, daha büyük işleme görevleri için ihtiyaç duyabileceğiniz ek kapasiteyi kazanabilir veya boşta yken kümeyi ölçeklendirebilirsiniz. Kümeyi nasıl ölçeklendirin [etüt](../hdinsight-administer-use-portal-linux.md)edin, bkz.

### <a name="maintain-the-system"></a>Sistemi koruyun

İşletim sistemi yamaları ve diğer güncelleştirmeleri uygulamak için bakım, mesai saatleri dışında bir HDInsight kümesinde temel Linux VM'lerinde gerçekleştirilir. Genellikle, bakım her Pazartesi ve Perşembe günü 03:30 'da (VM için yerel saate göre) yapılır. Güncelleştirmeler, kümenin dörtte birinden fazlasını aynı anda etkilemeyebilecek şekilde gerçekleştirilir.

Kafa düğümleri gereksiz olduğundan ve tüm veri düğümleri etkilenmediğinden, bu süre içinde çalışan tüm işler yavaşlayabilir. Ancak, yine de tamamlanması için çalışması gerekir. Sahip olduğunuz özel yazılımlar veya yerel veriler, küme yeniden oluşturma gerektiren bir felaket hatası oluşmadığı sürece bu bakım olayları arasında korunur.

## <a name="ide-options-for-ml-services-on-hdinsight"></a>HDInsight'ta ML Hizmetleri için IDE seçenekleri

BIR HDInsight kümesinin Linux kenar düğümü, R tabanlı analiz için iniş bölgesidir. HDInsight'ın son sürümleri, rstudio server'ın kenar düğümünde tarayıcı tabanlı IDE olarak varsayılan olarak yüklenmesini sağlar. R komut dosyalarının geliştirilmesi ve yürütülmesi için RStudio Server'ın IDE olarak kullanılması, R konsolunu kullanmaktan çok daha verimli olabilir.

Ayrıca, bir masaüstü IDE yükleyebilir ve uzak bir MapReduce veya Spark bilgi işlem bağlamı kullanarak kümeye erişmek için kullanabilirsiniz. Seçenekler Visual Studio (RTVS), RStudio ve Walware's Eclipse tabanlı [StatET](http://www.walware.de/goto/statet)için Microsoft'un [R Tools](https://marketplace.visualstudio.com/items?itemName=MikhailArkhipov007.RTVS2019) içerir.

Ayrıca, SSH veya PuTTY üzerinden bağlandıktan sonra Linux komut istemine **R** yazarak kenar düğümündeki R konsoluna erişebilirsiniz. Konsol arabirimini kullanırken, başka bir pencerede R komut dosyası geliştirme için bir metin düzenleyicisi çalıştırmak ve gerektiğinde komut dosyanızın bölümlerini r konsoluna kesip yapıştırmak uygundur.

## <a name="pricing"></a>Fiyatlandırma

ML Services HDInsight kümesiyle ilişkili fiyatlar, diğer HDInsight küme türlerinin fiyatlarına benzer şekilde yapılandırılmıştır. Bunlar, temel vm'lerin ad, veri ve kenar düğümleri arasında boyutlandırılmasına ve çekirdek saat yükseltmesinin eklenmesine dayanır. Daha fazla bilgi için [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/)için bkz.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight kümelerinde ML Hizmetlerini nasıl kullanacağınız hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [RStudio Server'ı kullanarak Azure HDInsight'ta ML Hizmetleri kümesinde Bir R komut dosyası çalıştırma](machine-learning-services-quickstart-job-rstudio.md)
* [HDInsight üzerinde ML Services kümesi için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight'ta ML Hizmetleri kümesi için depolama seçenekleri](r-server-storage.md)
