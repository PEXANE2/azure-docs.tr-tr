---
title: 'Performans akort: Storm, HDInsight & Azure Veri Gölü Depolama Gen2 | Microsoft Dokümanlar'
description: Azure Veri Gölü Depolama Gen2 Storm performans atonlama yönergeleri
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 125c583512f6bae34c2dd3c3dd76a1b96a181ac1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74327914"
---
# <a name="tune-performance-storm-hdinsight--azure-data-lake-storage-gen2"></a>Performans akort: Storm, HDInsight & Azure Veri Gölü Depolama Gen2

Bir Azure Storm topolojisinin performansını ayarlarken göz önünde bulundurulması gereken faktörleri anlayın. Örneğin, emzitler ve cıvatalar tarafından yapılan işin özelliklerini anlamak önemlidir (iş G/Ç veya bellek yoğun olsun). Bu makalede, sık karşılaşılan sorunları giderme de dahil olmak üzere bir dizi performans tuning yönergeleri ni kapsar.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Veri Gölü Depolama Gen2 hesabı.** Nasıl oluşturulacağına ilişkin talimatlar [için Bkz. Quickstart: Analitik için bir depolama hesabı oluşturun.](data-lake-storage-quickstart-create-account.md)
* Veri Gölü Depolama Gen2 hesabına erişim içeren **Azure HDInsight kümesi.** Bkz. [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanın.](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) Küme için Uzak Masaüstü'nü etkinleştirdiğinizden emin olun.
* **Veri Gölü Depolama Gen2'de Bir Fırtına kümesiçalıştırma.** Daha fazla bilgi için [HDInsight'ta Storm'a](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-overview)bakın.
* **Veri Gölü Depolama Gen2'de performans alamı yönergeleri.**  Genel performans kavramları için Bkz. [Veri Gölü Depolama Gen2 Performans Tuning Kılavuzu.](data-lake-storage-performance-tuning-guidance.md)   

## <a name="tune-the-parallelism-of-the-topology"></a>Topolojinin paralelliğini ayarlayın

Data Lake Storage Gen2'ye ve Bu'dan Gelen G/Ç'nin eşzamanlılıklarını artırarak performansı artırabilirsiniz. Fırtına topolojisi paralelliği belirleyen bir dizi yapılandırmaya sahiptir:
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

### <a name="get-the-best-performance-from-data-lake-storage-gen2"></a>Data Lake Storage Gen2'den en iyi performansı elde edin

Data Lake Storage Gen2 ile çalışırken, aşağıdakileri yaparsanız en iyi performansı elde elabilirsiniz:
* Küçük eklerinizi daha büyük boyutlarda biraraya getirin.
* Olabildiğince çok eşzamanlı istek yapın. Her cıvata iş parçacığı engelleme okuma yapıyor olduğundan, çekirdek başına 8-12 iş parçacığı aralığında bir yere sahip olmak istiyorum. Bu NIC ve CPU iyi kullanılmasını sağlar. Daha büyük bir VM daha eşzamanlı istekler sağlar.  

### <a name="example-topology"></a>Örnek topoloji

D13v2 Azure VM'li 8 altlık düğüm kümeniz olduğunu varsayalım. Bu VM 8 çekirdek, bu yüzden 8 işçi düğümleri arasında, 64 toplam çekirdek var.

Diyelim ki çekirdek başına 8 cıvata ipliği yapıyoruz. 64 çekirdek göz önüne alındığında, biz 512 toplam cıvata yürütme örnekleri (yani, iş parçacığı) istediğiniz anlamına gelir. Bu durumda, VM başına bir JVM ile başladığımızı ve eşzamanlılık elde etmek için esas olarak JVM içindeki iş parçacığı eşzamanlıbirimini kullandığımızı varsayalım. Bu, 8 alt görev (Azure VM başına bir) ve 512 cıvata yürütücüse ihtiyacımız olduğu anlamına gelir. Bu yapılandırma göz önüne alındığında, Storm her işçi düğümü 1 JVM vererek, işçi düğümleri (ayrıca süpervizör düğümleri olarak da bilinir) arasında eşit işçi dağıtmak için çalışır. Şimdi denetçiler içinde, Storm her bir süpervizör (yani, JVM) 8 konuları her vererek, amirler arasında eşit uygulayıcıları dağıtmak için çalışır.

## <a name="tune-additional-parameters"></a>Ek parametreleri ayarlama
Temel topolojiye sahip olduktan sonra, parametrelerden herhangi birini ayarlamak isteyip istemediğiniz göz önünde bulundurabilirsiniz:
* **İşçi düğümü başına JVM sayısı.** Bellekte barındırdığınız büyük bir veri yapınız (örneğin, bir arama tablosu) varsa, her JVM ayrı bir kopya gerektirir. Alternatif olarak, daha az JVM'niz varsa, veri yapısını birçok iş parçacığı nda kullanabilirsiniz. Cıvata G/Ç için, JVM sayısı bu JVM'ler arasında eklenen iş parçacığı sayısı kadar bir fark yaratmaz. Basitlik için, işçi başına bir JVM olması iyi bir fikirdir. Cıvatanızın ne yaptığına veya hangi uygulama işlemine gereksinim duyduğunuza bağlı olarak, bu numarayı değiştirmeniz gerekebilir.
* **Spout uygulayıcılarının sayısı.** Önceki örnek, Veri Gölü Depolama Gen2'ye yazmak için cıvatakullandığından, emzme sayısı cıvata performansıyla doğrudan ilgili değildir. Ancak, işlem veya G / 0 miktarına bağlı olarak en iyi performans için emzitler ayarlamak için iyi bir fikirdir. Cıvataları meşgul edebilmek için yeterli emzme sahip olduğundan emin olun. Emzitlerin çıkış oranları cıvataların verimi ile eşleşmelidir. Gerçek yapılandırma emzme bağlıdır.
* **Görev sayısı.** Her cıvata tek bir iplik olarak çalışır. Cıvata başına ek görevler herhangi bir ek eşzamanlılık sağlamaz. Onlar yarar sadece zaman tuple kabul süreci cıvata yürütme süresinin büyük bir kısmını alır olmasıdır. Cıvatadan bir onay göndermeden önce birçok tuples'ı daha büyük bir ekte gruplandırmak iyi bir fikirdir. Bu nedenle, çoğu durumda, birden çok görev ek bir avantaj sağlamaz.
* **Yerel veya karışık gruplandırma.** Bu ayar etkinleştirildiğinde, aynı alt işlem içinde cıvatalara tuples gönderilir. Bu, süreçler arası iletişimi ve ağ çağrılarını azaltır. Bu en topolojiler için tavsiye edilir.

Bu temel senaryo iyi bir başlangıç noktasıdır. En iyi performansı elde etmek için önceki parametrelerde değişiklik yapmak için kendi verilerinizle test edin.

## <a name="tune-the-spout"></a>Emziyi ayarlayın

Aşağıdaki ayarları emziyi ayarlamak için değiştirebilirsiniz.

- **Tuple zaman amı: topology.message.timeout.secs**. Bu ayar, bir iletinin başarısız olarak kabul edilmeden önce tamamlanması ve onay alması için gereken süreyi belirler.

- **İşçi başına maksimum bellek işlemi: işçi.childopts**. Bu ayar, Java çalışanlarına ek komut satırı parametreleri belirtmenizi sağlar. Burada en sık kullanılan ayar, JVM'nin yığınına ayrılan maksimum belleği belirleyen XmX'tir.

- **Max spout beklemede: topology.max.spout.pending**. Bu ayar, herhangi bir zamanda uç iş parçacığı başına uçarak (henüz topolojideki tüm düğümlerde kabul edilmeyen) uçabilen tupül sayısını belirler.

  Yapılacak iyi bir hesaplama, her bir tuples boyutunu tahmin etmektir. Sonra ne kadar bellek bir emmele iplik olduğunu anlamaya. Bu değere bölünen bir iş parçacığına ayrılan toplam bellek, size bekleyen maksimum parametre için üst sınırı vermelidir.

Varsayılan Veri Gölü Depolama Gen2 Storm cıvatası, bu parametreyi ayarlamak için kullanılabilecek bir boyut eşitleme ilkesi parametresi (fileBufferSize) vardır.

I/O-yoğun topolojilerde, her cıvata iş parçacığının kendi dosyasına yazılması ve bir dosya döndürme ilkesinin (fileRotationSize) ayarlamının iyi bir fikirdir. Dosya belirli bir boyuta ulaştığında, akış otomatik olarak temizlenir ve yeni bir dosya yazılır. Döndürme için önerilen dosya boyutu 1 GB'dır.

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

### <a name="data-lake-storage-gen2-throttling"></a>Veri Gölü Depolama Gen2 azaltma
Data Lake Storage Gen2 tarafından sağlanan bant genişliği sınırlarına ularsanız, görev hataları görebilirsiniz. Azaltma hataları için görev günlüklerini denetleyin. Kapsayıcı boyutunu artırarak paralelliği azaltabilirsiniz.    

Daraltılmış olup olmadığınızı kontrol etmek için istemci tarafında hata ayıklama günlüğe kaydetmeyi etkinleştirin:

1. **Ambari** > **Storm** > **Config** > Gelişmiş**fırtına-işçi-log4j**yılında, ** &lt;&gt; ** ** &lt;kök düzeyi="bilgi" kök&gt;düzeyi ="hata ayıklama"** değiştirin. Yapılandırmanın etkili olması için tüm düğümleri/hizmeti yeniden başlatın.
2. Veri Gölü Depolama Gen2 azaltma özel durumlar için işçi düğümleri (altında&lt;/var/log/storm/worker-artifalar/ TopolojiAd&gt;/&lt;portu&gt;/worker.log) üzerinde Fırtına topoloji günlükleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Fırtına için ek performans tuning [bu blogda](https://blogs.msdn.microsoft.com/shanyu/2015/05/14/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs/)başvurulabilir.

Çalıştırmak için ek bir örnek için, [GitHub bu](https://github.com/hdinsight/storm-performance-automation)bakın.
