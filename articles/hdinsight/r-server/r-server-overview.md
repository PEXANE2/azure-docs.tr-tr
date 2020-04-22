---
title: Azure HDInsight'ta ML Hizmetlerine Giriş
description: Büyük veri analizi için uygulamalar oluşturmak için HDInsight'ta ML Hizmetlerini nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive
ms.date: 04/20/2020
ms.openlocfilehash: 76fcdb52df88be2c4033140f4bc71b28424d7f38
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687787"
---
# <a name="what-is-ml-services-in-azure-hdinsight"></a>Azure HDInsight'ta ML Hizmetleri Nedir

Microsoft Machine Learning Server, Azure'da HDInsight kümeleri oluşturduğunuzda dağıtım seçeneği olarak kullanılabilir. Bu seçeneği sağlayan küme **türüNE ML Hizmetleri**denir. Bu özellik, HDInsight'ta uyarlanabilir, dağıtılmış analiz yöntemlerine isteğe bağlı erişim sağlar.

HDInsight'taki ML Services, neredeyse her boyuttaki veri kümelerinde R tabanlı analitik için en son özellikleri sağlar. Veri kümeleri Azure Blob veya Veri Gölü depolama alanına yüklenebilir. R tabanlı uygulamalarınız 8000+ açık kaynak R paketini kullanabilir. Microsoft'un büyük veri analizi paketi ScaleR'daki rutinler de mevcuttur.

Kenar düğümü kümeye bağlanmak ve R komut dosyalarınızı çalıştırmak için kullanışlı bir yer sağlar. Kenar düğümü, ScaleR paralelleştirilmiş dağıtılmış işlevlerisunucunın çekirdekleri arasında çalıştırmayı sağlar. ScaleR'ın Hadoop Harita Azalt'ını kullanarak kümenin düğümleri üzerinde de çalıştırabilirsiniz. Apache Spark bilgi işlem bağlamlarını da kullanabilirsiniz.

Çözümlemeden kaynaklanan modeller veya öngörüler şirket içi kullanım için indirilebilir. Azure'un `operationalized` başka bir yerinde de olabilirler. Özellikle, [Azure Machine Learning Studio (klasik)](https://studio.azureml.net)ve web [hizmeti](../../machine-learning/studio/deploy-a-machine-learning-web-service.md)ile .

## <a name="get-started-with-ml-services-on-hdinsight"></a>HDInsight'ta ML Hizmetleri ile başlayın

HDInsight'ta bir ML Hizmetleri kümesi oluşturmak için **ML Hizmetleri** küme türünü seçin. ML Hizmetleri küme türü, veri düğümlerinde ML Server ve kenar düğümlerini içerir. Kenar düğümü ML Hizmetleri tabanlı analiziçin bir iniş bölgesi olarak hizmet vermektedir. Bkz. Kümenin nasıl oluşturulacağını öğrenmek için [Azure portalını kullanarak Apache Hadoop kümeleri](../hdinsight-hadoop-create-linux-clusters-portal.md) oluşturun.

## <a name="why-choose-ml-services-in-hdinsight"></a>NEDEN HDInsight'ta ML Hizmetleri'ni seçmelisiniz?

HDInsight'taki ML Hizmetleri aşağıdaki avantajları sağlar:

### <a name="ai-innovation-from-microsoft-and-open-source"></a>Microsoft ve açık kaynak tan ai yenilik

  ML [Hizmetleri, RevoscaleR,](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package)ve [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package)gibi yüksek uyarlanabilir, dağıtılmış algoritmalar kümesi içerir. Bu algoritmalar, fiziksel bellek boyutundan daha büyük veri boyutları üzerinde çalışabilir. Ayrıca dağıtılmış bir şekilde çok çeşitli platformlarda çalışırlar. Microsoft'un ürünle birlikte verilen özel [R paketleri](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) ve Python [paketlerinin](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) koleksiyonu hakkında daha fazla bilgi edinin.
  
  ML Services, bu Microsoft yeniliklerini ve açık kaynak topluluğundan (R, Python ve AI araç kitleri) gelen katkıları köprüler. Hepsi tek bir kurumsal sınıf platformun üstünde. Herhangi bir R veya Python açık kaynak makine öğrenme paketi, Microsoft'un herhangi bir özel yeniliğiyle yan yana çalışabilir.

### <a name="simple-secure-and-high-scale-operationalization-and-administration"></a>Basit, güvenli ve yüksek ölçekli operasyonelleştirme ve yönetim

  Geleneksel paradigmalara ve ortamlara güvenen işletmeler operasyonelleşme için çok fazla zaman ve çaba harvetmektedir. Bu eylem, çeviri süresi de dahil olmak üzere şişirilmiş maliyetlere ve gecikmelere neden olur: modeller, bunları geçerli ve güncel tutmak için yinelemeler, düzenleyici onay ve izinleri yönetme.

  ML Services kurumsal sınıf [operasyonelleştirme](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)sunmaktadır. Bir makine öğrenme modeli tamamlandıktan sonra, web hizmetleri API'leri oluşturmak için sadece birkaç tıklama alır. Bu [web hizmetleri](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) buluttaki bir sunucu ızgarasında barındırılır ve iş yeri uygulamalarıyla tümleştirilebilir. Elastik bir ızgaraya dağıtma yeteneği, hem toplu hem de gerçek zamanlı puanlama için işletmenizin gereksinimleriyle sorunsuz bir şekilde ölçeklendirmenize olanak tanır. Talimatlar için [HDInsight'ta ML Hizmetlerini Operasyonelleştir'e](r-server-operationalize.md)bakın.

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
| R özellikli | R ile yazılmış çözümler için [R paketleri,](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) R'nin açık kaynak dağıtımı ve komut dosyası yürütme için çalışma zamanı altyapısı. |
| Python özellikli | Python'da yazılan çözümler için [Python modülleri,](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) Python'un açık kaynak dağılımı ve komut dosyası yürütme için çalışma zamanı altyapısı.
| [Önceden eğitilmiş modeller](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Görsel analiz ve metin duyarlılığı analizi için, sağladığınız verileri puana hazır. |
| [Dağıtma ve tüket](r-server-operationalize.md) | `Operationalize`sunucunuz ve çözümleri bir web hizmeti olarak dağıtın. |
| [Uzaktan yürütme](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-services) | İstemci iş istasyonunuzdan ağınızdaki ML Hizmetleri kümesinde uzak oturumları başlatın. |

## <a name="data-storage-options-for-ml-services-on-hdinsight"></a>HDInsight'ta ML Hizmetleri için veri depolama seçenekleri

HDFS dosya sistemi için varsayılan depolama bir Azure Depolama hesabı veya Azure Veri Gölü Depolama olabilir. Analiz sırasında küme depolamasına yüklenen veriler kalıcı hale getirilir. Veriler küme silindikten sonra bile kullanılabilir. Çeşitli araçlar depolamaya veri aktarımını işleyebilir. Araçlar depolama hesabının portal tabanlı yükleme tesisini ve AzCopy yardımcı programını içerir.

Küme oluşturma sırasında ek Blob ve Veri göl depolarına erişimi etkinleştirebilirsiniz. Kullanımdaki birincil depolama seçeneği ile sınırlı değilsiniz.  Birden çok depolama hesabı kullanma hakkında daha fazla bilgi edinmek [için HDInsight makalesinde ML Hizmetleri için Azure Depolama seçeneklerine](./r-server-storage.md) bakın.

Kenar düğümünde kullanılmak üzere bir depolama seçeneği olarak Azure Dosyaları'nı da kullanabilirsiniz. Azure Files, Azure Depolama'da oluşturulan dosya paylaşımlarının Linux dosya sistemine kaydedilmesini sağlar. Daha fazla bilgi için [HDInsight'ta ML Hizmetleri için Azure Depolama seçeneklerine](r-server-storage.md)bakın.

## <a name="access-ml-services-edge-node"></a>Erişim ML Hizmetleri kenar düğümü

Microsoft ML Server'a kenar düğümünde bir tarayıcı veya SSH/PuTTY kullanarak bağlanabilirsiniz. Küme oluşturma sırasında Varsayılan olarak R konsolu yüklenir.  

## <a name="develop-and-run-r-scripts"></a>R komut dosyaları geliştirme ve çalıştırma

R komut dosyalarınız 8000' den fazla açık kaynak R paketinden herhangi birini kullanabilir. ScaleR kitaplığından paralelleştirilmiş ve dağıtılmış yordamları da kullanabilirsiniz. Komut dosyaları kenar düğümünde çalıştırın r yorumlayıcısı içinde bu düğüm üzerinde çalıştırın. Map Reduce (RxHadoopMR) veya Spark (RxSpark) bilgi işlem bağlamı yla ScaleR işlevlerini arayan adımlar dışında. İşlevler, verilerle ilişkili veri düğümleri arasında dağıtılmış bir şekilde çalıştırılır. Bağlam seçenekleri hakkında daha fazla bilgi [için, HDInsight'ta ML Hizmetleri için Bilgi İşlem bağlamı seçeneklerine](r-server-compute-contexts.md)bakın.

## <a name="operationalize-a-model"></a>`Operationalize`bir model

Veri modellemeniz tamamlandığında, `operationalize` Azure'dan veya şirket içinde yeni veriler için öngörülerde bulunmak için model. Bu işlem puanlama olarak bilinir. Puanlama HDInsight, Azure Machine Learning veya şirket içinde yapılabilir.

### <a name="score-in-hdinsight"></a>HDInsight'ta puan

HDInsight'ta puan almak için bir R fonksiyonu yazın. İşlev, depolama hesabınıza yüklediğiniz yeni bir veri dosyası için öngörülerde bulunmak için modelinizi çağırır. Ardından, tahminleri depolama hesabına geri kaydedin. Bu yordamı isteğe bağlı olarak kümenizin kenar düğümünde veya zamanlanmış bir iş kullanarak çalıştırabilirsiniz.

### <a name="score-in-azure-machine-learning-aml"></a>Azure Machine Learning'de Puan (AML)

Azure Machine Learning'i kullanarak puan almak için, modelinizi Azure web hizmeti olarak yayımlamak için [AzureML](https://cran.r-project.org/src/contrib/Archive/AzureML/) olarak bilinen açık kaynak Azure Machine Learning R paketini kullanın. Kolaylık sağlamak için, bu paket kenar düğümüne önceden yüklenir. Ardından, web hizmeti için bir kullanıcı arabirimi oluşturmak için Azure Machine Learning'deki tesisleri kullanın ve ardından puanlama için gerektiğinde web hizmetini arayın. Ardından ScaleR model nesnelerini web hizmetinde kullanılmak üzere eşdeğer açık kaynak model nesnelerine dönüştürün. Bu dönüştürme için topluluk tabanlı modeller `as.randomForest()` gibi ScaleR zorlama işlevlerini kullanın.

### <a name="score-on-premises"></a>Şirket içinde puan

Modelinizi oluşturduktan sonra şirket içinde puan elde etmek için: Modeli R'de seri hale getirmek, indirin, seri hale getirmek ten, sonra yeni veri puanlamak için kullanın. HDInsight'ta Daha önce Score'da açıklanan yaklaşımı kullanarak veya [web hizmetlerini](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services)kullanarak yeni veriler elde edebilirsiniz.

## <a name="maintain-the-cluster"></a>Kümeyi koruyun

### <a name="install-and-maintain-r-packages"></a>R paketlerini yükleme ve bakımını

R komut dosyalarınızın çoğu adımları burada çalıştırdığından, kullandığınız R paketlerinin çoğu kenar düğümünde gereklidir. Kenar düğümüne ek R paketleri yüklemek için, `install.packages()` Yöntemi R'de kullanabilirsiniz.

ScaleR kitaplık yordamlarını kullanıyorsanız, genellikle ek R paketlerine ihtiyacınız yoktur. Veri düğümlerinde **rxExec** veya **RxDataStep** yürütme için ek paketler gerekebilir.

Ek paketler, kümeyi oluşturduktan sonra komut dosyası eylemiyle yüklenebilir. Daha fazla bilgi için [HDInsight kümesinde ML Hizmetlerini Yönet'e](r-server-hdinsight-manage.md)bakın.

### <a name="change-apache-hadoop-mapreduce-memory-settings"></a>Apache Hadoop Haritasını DeğiştirBellek ayarlarını azaltın

ML Hizmetleri için kullanılabilir bellek, MapReduce işini çalıştırırken değiştirilebilir. Bir kümeyi değiştirmek için kümeniz için Apache Ambari UI'yi kullanın. For Ambari UI instructions, see [Manage HDInsight clusters using the Ambari Web UI](../hdinsight-hadoop-manage-ambari.md).

**RxHadoopMR'a**yapılan çağrıda Hadoop anahtarları kullanılarak ML Hizmetleri'ne sunulan bellek değiştirilebilir:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Kümenizi ölçeklendirin

HDInsight'taki mevcut bir ML Hizmetleri kümesi portal üzerinden yukarı veya aşağı ölçeklendirilebilir. Ölçekleme yaparak, daha büyük işleme görevleri için ek kapasite kazanırsınız. Bir küme boşta yken küçültebilirsiniz. Kümeyi nasıl ölçeklendirin [etüt](../hdinsight-administer-use-portal-linux.md)edin, bkz.

### <a name="maintain-the-system"></a>Sistemi koruyun

İşletim Sistemi Bakımı, mesai saatleri dışında bir HDInsight kümesinde temel linux VM'lerinde yapılır. Genellikle bakım her Pazartesi ve Perşembe günü 03:30'da (VM'nin yerel saati) yapılır. Güncelleştirmeler, kümenin dörtte birinden fazlasını aynı anda etkilemez.

İşleri çalıştırmak bakım sırasında yavaşlayabilir. Ancak, yine de tamamlanması için çalışması gerekir. Bir küme yeniden oluşturma gerektiren bir felaket hatası oluşmadığı sürece, sahip olduğunuz özel yazılımlar veya yerel veriler bu bakım olayları arasında korunur.

## <a name="ide-options-for-ml-services-on-hdinsight"></a>HDInsight'ta ML Hizmetleri için IDE seçenekleri

BIR HDInsight kümesinin Linux kenar düğümü, R tabanlı analiz için iniş bölgesidir. HDInsight'ın son sürümleri, kenar düğümünde RStudio Server'ın tarayıcı tabanlı bir IDE'sini sağlar. RStudio Server geliştirme ve yürütme için R konsolundan daha üretkendir.

Masaüstü IDE, kümeye uzak bir MapReduce veya Spark bilgi işlem bağlamında erişebilir. Seçenekler şunlardır: Visual Studio (RTVS), RStudio ve Walware's Eclipse tabanlı StatET için Microsoft'un [R Tools.](https://marketplace.visualstudio.com/items?itemName=MikhailArkhipov007.RTVS2019)

Komut istemine **R** yazarak kenar düğümündeki R konsoluna erişin. Konsol arabirimini kullanırken, metin düzenleyicisinde R komut dosyası geliştirmek uygundur. Daha sonra komut dosyanızın bölümlerini gerektiği gibi R konsoluna kesip yapıştırın.

## <a name="pricing"></a>Fiyatlandırma

ML Services HDInsight kümesiyle ilişkili fiyatlar diğer HDInsight küme türlerine benzer şekilde yapılandırılmıştır. Bunlar, ad, veri ve kenar düğümleri arasında temel VM'lerin boyutlandırılmasına dayanır. Çekirdek saati yükselmesi de. Daha fazla bilgi için [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/)için bkz.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight kümelerinde ML Hizmetlerini nasıl kullanacağınız hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [RStudio Server'ı kullanarak Azure HDInsight'ta ML Hizmetleri kümesinde Bir R komut dosyası çalıştırma](machine-learning-services-quickstart-job-rstudio.md)
* [HDInsight üzerinde ML Services kümesi için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight'ta ML Hizmetleri kümesi için depolama seçenekleri](r-server-storage.md)
