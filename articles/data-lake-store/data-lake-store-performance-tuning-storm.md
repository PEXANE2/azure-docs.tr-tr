---
title: Azure Veri Gölü Depolama Gen1 Storm performans atonlama yönergeleri | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1 Storm performans atonlama yönergeleri
services: data-lake-store
documentationcenter: ''
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 8066a759cf80be6e9ca232bcd3693a5fa4d2f2f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61436486"
---
# <a name="performance-tuning-guidance-for-storm-on-hdinsight-and-azure-data-lake-storage-gen1"></a>HDInsight ve Azure Veri Gölü Depolama Gen1'de Storm için performans alamı kılavuzu

Bir Azure Storm topolojisinin performansını ayarlarken göz önünde bulundurulması gereken faktörleri anlayın. Örneğin, emzitler ve cıvatalar tarafından yapılan işin özelliklerini anlamak önemlidir (iş G/Ç veya bellek yoğun olsun). Bu makalede, sık karşılaşılan sorunları giderme de dahil olmak üzere bir dizi performans tuning yönergeleri ni kapsar.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Veri Gölü Depolama Gen1 hesabı.** Nasıl oluşturulacağına ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md)
* Veri Gölü Depolama Gen1 hesabına erişimi olan bir **Azure HDInsight kümesi.** Bkz. [Veri Gölü Depolama Gen1 ile hdinsight kümesi oluşturun.](data-lake-store-hdinsight-hadoop-use-portal.md) Küme için Uzak Masaüstü'nü etkinleştirdiğinizden emin olun.
* **Veri Gölü Depolama Gen1'de Bir Fırtına kümesiçalıştırma.** Daha fazla bilgi için [HDInsight'ta Storm'a](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-overview)bakın.
* **Veri Gölü Depolama Gen1'de performans alamı yönergeleri.**  Genel performans kavramları için Bkz. [Veri Gölü Depolama Gen1 Performans Tuning Kılavuzu.](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance)  

## <a name="tune-the-parallelism-of-the-topology"></a>Topolojinin paralelliğini ayarlayın

Data Lake Storage Gen1'e ve Bu G)'nin eşzamanlılıkını artırarak performansı artırabilirsiniz. Fırtına topolojisi paralelliği belirleyen bir dizi yapılandırmaya sahiptir:
* İşçi işlemlerinin sayısı (çalışanlar VM'ler arasında eşit olarak dağıtılır).
* Spout uygulayıcı örneklerinin sayısı.
* Cıvata uygulayıcı örneklerinin sayısı.
* Emzme görevlerinin sayısı.
* Cıvata görev sayısı.

Örneğin, 4 VM ve 4 alt işlem, 32 spout yürütücüsü ve 32 spout görevi ve 256 cıvata yürütücüsü ve 512 cıvata görevi olan bir kümede aşağıdakileri göz önünde bulundurun:

Bir işçi düğümü olan her gözetmen, tek bir işçi Java sanal makine (JVM) işlemi vardır. Bu JVM işlemi 4 emzme ipliği ve 64 cıvata iş parçacığı yönetir. Her iş parçacığı içinde görevler sırayla çalıştırılır. Önceki yapılandırmada, her spout iş parçacığının 1 görevi vardır ve her cıvata iş parçacığının 2 görevi vardır.

Storm'da, burada ilgili çeşitli bileşenler ve bunların sahip olduğunuz paralellik düzeyini nasıl etkilediği şunlardır:
* Baş düğümü (Fırtınada Nimbus olarak adlandırılır) işleri göndermek ve yönetmek için kullanılır. Bu düğümlerin paralellik derecesi üzerinde hiçbir etkisi yoktur.
* Gözetmen düğümleri. HDInsight'ta bu, bir alt düğüm Azure VM'ye karşılık gelir.
* Alt görevler, VM'lerde çalışan Fırtına işlemleridir. Her işçi görevi bir JVM örneğine karşılık gelir. Storm, belirttiğiniz işçi işlem sayısını mümkün olduğunca eşit olarak işçi düğümlerine dağıtır.
* Spout ve cıvata uygulayıcı örnekleri. Her uygulayıcı örnek, işçiler (JVM) içinde çalışan bir iş parçacığına karşılık gelir.
* Fırtına görevleri. Bunlar, bu iş parçacıklarının her birinin çalıştırdığı mantıksal görevlerdir. Bu paralellik düzeyini değiştirmez, bu nedenle uygulayıcı başına birden çok görev gerekip gerekmediğini değerlendirmeniz gerekir.

### <a name="get-the-best-performance-from-data-lake-storage-gen1"></a>Data Lake Storage Gen1'den en iyi performansı elde edin

Data Lake Storage Gen1 ile çalışırken, aşağıdakileri yaparsanız en iyi performansı elde elabilirsiniz:
* Küçük eklerinizi daha büyük boyutlarda (ideal olarak 4 MB) biraraya getirin.
* Olabildiğince çok eşzamanlı istek yapın. Her cıvata iş parçacığı engelleme okuma yapıyor olduğundan, çekirdek başına 8-12 iş parçacığı aralığında bir yere sahip olmak istiyorum. Bu NIC ve CPU iyi kullanılmasını sağlar. Daha büyük bir VM daha eşzamanlı istekler sağlar.  

### <a name="example-topology"></a>Örnek topoloji

D13v2 Azure VM'li 8 altlık düğüm kümeniz olduğunu varsayalım. Bu VM 8 çekirdek, bu yüzden 8 işçi düğümleri arasında, 64 toplam çekirdek var.

Diyelim ki çekirdek başına 8 cıvata ipliği yapıyoruz. 64 çekirdek göz önüne alındığında, biz 512 toplam cıvata yürütme örnekleri (yani, iş parçacığı) istediğiniz anlamına gelir. Bu durumda, VM başına bir JVM ile başladığımızı ve eşzamanlılık elde etmek için esas olarak JVM içindeki iş parçacığı eşzamanlıbirimini kullandığımızı varsayalım. Bu, 8 alt görev (Azure VM başına bir) ve 512 cıvata yürütücüse ihtiyacımız olduğu anlamına gelir. Bu yapılandırma göz önüne alındığında, Storm her işçi düğümü 1 JVM vererek, işçi düğümleri (ayrıca süpervizör düğümleri olarak da bilinir) arasında eşit işçi dağıtmak için çalışır. Şimdi denetçiler içinde, Storm her bir süpervizör (yani, JVM) 8 konuları her vererek, amirler arasında eşit uygulayıcıları dağıtmak için çalışır.

## <a name="tune-additional-parameters"></a>Ek parametreleri ayarlama
Temel topolojiye sahip olduktan sonra, parametrelerden herhangi birini ayarlamak isteyip istemediğiniz göz önünde bulundurabilirsiniz:
* **İşçi düğümü başına JVM sayısı.** Bellekte barındırdığınız büyük bir veri yapınız (örneğin, bir arama tablosu) varsa, her JVM ayrı bir kopya gerektirir. Alternatif olarak, daha az JVM'niz varsa, veri yapısını birçok iş parçacığı nda kullanabilirsiniz. Cıvata G/Ç için, JVM sayısı bu JVM'ler arasında eklenen iş parçacığı sayısı kadar bir fark yaratmaz. Basitlik için, işçi başına bir JVM olması iyi bir fikirdir. Cıvatanızın ne yaptığına veya hangi uygulama işlemine gereksinim duyduğunuza bağlı olarak, bu numarayı değiştirmeniz gerekebilir.
* **Spout uygulayıcılarının sayısı.** Önceki örnek, Veri Gölü Depolama Gen1'e yazmak için cıvatakullandığından, emzme sayısı cıvata performansıyla doğrudan ilgili değildir. Ancak, işlem veya G / 0 miktarına bağlı olarak en iyi performans için emzitler ayarlamak için iyi bir fikirdir. Cıvataları meşgul edebilmek için yeterli emzme sahip olduğundan emin olun. Emzitlerin çıkış oranları cıvataların verimi ile eşleşmelidir. Gerçek yapılandırma emzme bağlıdır.
* **Görev sayısı.** Her cıvata tek bir iplik olarak çalışır. Cıvata başına ek görevler herhangi bir ek eşzamanlılık sağlamaz. Onlar yarar sadece zaman tuple kabul süreci cıvata yürütme süresinin büyük bir kısmını alır olmasıdır. Cıvatadan bir onay göndermeden önce birçok tuples'ı daha büyük bir ekte gruplandırmak iyi bir fikirdir. Bu nedenle, çoğu durumda, birden çok görev ek bir avantaj sağlamaz.
* **Yerel veya karışık gruplandırma.** Bu ayar etkinleştirildiğinde, aynı alt işlem içinde cıvatalara tuples gönderilir. Bu, süreçler arası iletişimi ve ağ çağrılarını azaltır. Bu en topolojiler için tavsiye edilir.

Bu temel senaryo iyi bir başlangıç noktasıdır. En iyi performansı elde etmek için önceki parametrelerde değişiklik yapmak için kendi verilerinizle test edin.

## <a name="tune-the-spout"></a>Emziyi ayarlayın

Aşağıdaki ayarları emziyi ayarlamak için değiştirebilirsiniz.

- **Tuple zaman amı: topology.message.timeout.secs**. Bu ayar, bir iletinin başarısız olarak kabul edilmeden önce tamamlanması ve onay alması için gereken süreyi belirler.

- **İşçi başına maksimum bellek işlemi: işçi.childopts**. Bu ayar, Java çalışanlarına ek komut satırı parametreleri belirtmenizi sağlar. Burada en sık kullanılan ayar, JVM'nin yığınına ayrılan maksimum belleği belirleyen XmX'tir.

- **Max spout beklemede: topology.max.spout.pending**. Bu ayar, herhangi bir zamanda uç iş parçacığı başına uçarak (henüz topolojideki tüm düğümlerde kabul edilmeyen) uçabilen tupül sayısını belirler.

  Yapılacak iyi bir hesaplama, her bir tuples boyutunu tahmin etmektir. Sonra ne kadar bellek bir emmele iplik olduğunu anlamaya. Bu değere bölünen bir iş parçacığına ayrılan toplam bellek, size bekleyen maksimum parametre için üst sınırı vermelidir.

## <a name="tune-the-bolt"></a>Cıvatayı ayarlayın
Veri Gölü Depolama Gen1'e yazarken, 4 MB'a bir boyut eşitleme ilkesi (istemci tarafında arabellek) ayarlayın. Bir yıkama veya hsync() sonra yalnızca arabellek boyutu bu değerde olduğunda gerçekleştirilir. İşçi VM'deki Veri Gölü Depolama Gen1 sürücüsü, açıkça bir hsync() gerçekleştirmediğiniz sürece bu arabelleği otomatik olarak yapar.

Varsayılan Veri Gölü Depolama Gen1 Storm cıvatası, bu parametreyi ayarlamak için kullanılabilecek bir boyut eşitleme ilkesi parametresi (fileBufferSize) vardır.

I/O-yoğun topolojilerde, her cıvata iş parçacığının kendi dosyasına yazılması ve bir dosya döndürme ilkesinin (fileRotationSize) ayarlamının iyi bir fikirdir. Dosya belirli bir boyuta ulaştığında, akış otomatik olarak temizlenir ve yeni bir dosya yazılır. Döndürme için önerilen dosya boyutu 1 GB'dır.

### <a name="handle-tuple-data"></a>Tuple verilerini işleme

Storm'da, bir spout cıvata tarafından açıkça kabul edilene kadar bir tuple üzerinde tutar. Cıvata tarafından bir tuple okunduysa ancak henüz kabul edilmemişse, emzit Veri Gölü Depolama Gen1 arka uçta kalıcı olmayabilir. Bir tuple kabul edildikten sonra, emzme cıvata tarafından kalıcılığı garanti edilebilir ve daha sonra hangi kaynaktan okunan kaynaktan kaynak verileri silebilir.  

Veri Gölü Depolama Gen1'de en iyi performans için, 4 MB tuple verisi cıvata arabelleği var. Sonra Bir 4-MB yazmak olarak Veri Gölü Depolama Gen1 arka ucuna yazın. Veriler mağazaya başarıyla yazıldıktan sonra (hflush()'ı arayarak), cıvata verileri emzmeye geri dönebilir. Burada verilen örnek cıvata budur. Ayrıca hflush() araması yapılmadan ve tuples kabul edilmeden önce daha fazla sayıda tuples tutmak da kabul edilebilir. Ancak, bu, emzittutması gereken uçuştaki tupül sayısını artırır ve bu nedenle JVM başına gereken bellek miktarını artırır.

> [!NOTE]
> Uygulamalar, diğer performans dışı nedenlerden dolayı tuples'ı daha sık (veri boyutlarında 4 MB'dan az) kabul etme gereksinimi olabilir. Ancak, bu depolama arka sonuna G/Ç iş buzunu etkileyebilir. Bu tradeoff'u cıvatanın G/Ç performansına göre dikkatlice tartmaya devam edin.

Tuples gelen oranı yüksek değilse, bu nedenle 4-MB arabellek doldurmak için uzun bir zaman alır, bu tarafından hafifletici düşünün:
* Cıvata sayısını azaltmak, böylece doldurmak için daha az arabellek vardır.
* Bir hflush() her x floş veya her y milisaniye tetiklenir ve tuples şimdiye kadar birikmiş geri kabul edilir zaman tabanlı veya sayı tabanlı bir ilke olması.

Bu durumda iş verimi daha düşük olduğunu unutmayın, ancak olayların yavaş bir oranda, maksimum iş bölümü zaten en büyük hedefi değildir. Bu azaltıcı etkenler, bir tuple'ın mağazaya akması için gereken toplam süreyi azaltmanıza yardımcı olur. Düşük bir olay oranıyla bile gerçek zamanlı bir ardışık hat lar istiyorsanız, bu önemli olabilir. Ayrıca, gelen tuple oranınız düşükse, topology.message.timeout_secs parametresini ayarlamanız gerektiğini, böylece tuples'ın arabelleğe alınırken veya işlenirken zaman ları uzatmadığını unutmayın.

## <a name="monitor-your-topology-in-storm"></a>Fırtına'da topolojinizi izleyin  
Topolojiniz çalışırken, Fırtına kullanıcı arabiriminde izleyebilirsiniz. Burada bakmak için ana parametreler şunlardır:

* **Toplam işlem yürütme gecikmesi.** Bu, bir tuple'ın yayMak için aldığı ortalama süredir, cıvata tarafından işlenir ve kabul edilir.

* **Toplam cıvata işlemi gecikmesi.** Bu, bir onay alana kadar cıvatada tuple tarafından harcanan ortalama süredir.

* **Toplam cıvata gecikmesi çalıştırın.** Bu, cıvata nın yürütme yönteminde harcadığı ortalama süredir.

* **Başarısızlık sayısı.** Bu, zaman dolmadan önce tam olarak işlenemeyen tuples sayısını ifade eder.

* **Kapasite.** Bu, sisteminizin ne kadar yoğun olduğunu niçin ölçtüğünüzdür. Bu sayı 1 ise, cıvatalarınız olabildiğince hızlı çalışıyor. 1'den az ise, paralelliği artırın. 1'den büyükse, paralelliği azaltın.

## <a name="troubleshoot-common-problems"></a>Sık karşılaşılan sorunları giderme
Sık karşılaşılan birkaç sorun giderme senaryosu aşağıda verilmiştir.
* **Birçok tuples dışarı zamanlama vardır.** Darboğaz nerede olduğunu belirlemek için topolojideki her düğüme bakın. Bunun en yaygın nedeni cıvataların emzilere ayak uydurabilmeleridir. Bu, işlenmeyi beklerken iç arabellekleri tıkayan tuples yol açar. Zaman açığı değerini artırıcı veya bekleyen en büyük spout' u düşü

* **Yüksek toplam işlem yürütme gecikmesi vardır, ancak düşük cıvata işlemi gecikmesi.** Bu durumda, tuples yeterince hızlı kabul edilmemektedir mümkündür. Yeterli sayıda bildirimde olup olmadığını kontrol edin. Başka bir olasılık da cıvatalar işlemeye başlamadan önce kuyrukta çok uzun süre beklemeleridir. Bekleyen maksimum spout azaltın.

* **Yüksek bir cıvata yürütme gecikmesi vardır.** Bu, cıvatanızın yürütme() yönteminin çok uzun sürdüğü anlamına gelir. Kodu optimize edin veya yazma boyutlarına ve floş davranışına bakın.

### <a name="data-lake-storage-gen1-throttling"></a>Veri Gölü Depolama Gen1 azaltma
Data Lake Storage Gen1 tarafından sağlanan bant genişliği sınırlarına ularsanız, görev hataları görebilirsiniz. Azaltma hataları için görev günlüklerini denetleyin. Kapsayıcı boyutunu artırarak paralelliği azaltabilirsiniz.    

Daraltılmış olup olmadığınızı kontrol etmek için istemci tarafında hata ayıklama günlüğe kaydetmeyi etkinleştirin:

1. **Ambari** > **Storm** > **Config** > Gelişmiş**fırtına-işçi-log4j**yılında, ** &lt;&gt; ** ** &lt;kök düzeyi="bilgi" kök&gt;düzeyi ="hata ayıklama"** değiştirin. Yapılandırmanın etkili olması için tüm düğümleri/hizmeti yeniden başlatın.
2. Veri Gölü Depolama Gen1 azaltma özel durumlar için işçi düğümleri (altında&lt;/var/log/storm/worker-artifalar/ TopolojiAd&gt;/&lt;portu&gt;/worker.log) üzerinde Fırtına topoloji günlükleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Fırtına için ek performans tuning [bu blogda](https://blogs.msdn.microsoft.com/shanyu/2015/05/14/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs/)başvurulabilir.

Çalıştırmak için ek bir örnek için, [GitHub bu](https://github.com/hdinsight/storm-performance-automation)bakın.
