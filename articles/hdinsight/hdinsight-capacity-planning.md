---
title: Azure HDInsight'ta küme kapasite planlaması
description: Azure HDInsight kümesinin kapasite ve performans planlaması için önemli soruları belirleyin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/07/2020
ms.openlocfilehash: 4ede8833fdbdbd57654e6c02147f53e58a17b1de
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887002"
---
# <a name="capacity-planning-for-hdinsight-clusters"></a>HDInsight kümeleri için kapasite planlaması

HDInsight kümesini dağıtmadan önce, gerekli performansı ve ölçeği belirleyerek amaçlanan küme kapasitesini planlayın. Bu planlama hem kullanılabilirliği hem de maliyetleri optimize etmesine yardımcı olur. Bazı küme kapasitesi kararları dağıtımdan sonra değiştirilemez. Performans parametreleri değişirse, bir küme sökülebilir ve depolanan verileri kaybetmeden yeniden oluşturulabilir.

Kapasite planlaması için sorulması gereken temel sorular şunlardır:

* Kümenizi hangi coğrafi bölgede dağıtmalısınız?
* Ne kadar depolama alanına ihtiyacınız var?
* Hangi küme türünü dağıtmalısınız?
* Küme düğümleriniz ne boyutta ve sanal makine (VM) kullanmalıdır?
* Kümenizde kaç işçi düğümü olmalıdır?

## <a name="choose-an-azure-region"></a>Azure bölgesi seçin

Azure bölgesi, kümenizin fiziksel olarak nerede sağlandığınızı belirler. Okuma ve yazma ların gecikmesini en aza indirmek için küme verilerinize yakın olmalıdır.

HDInsight birçok Azure bölgesinde kullanılabilir. En yakın bölgeyi bulmak için [bölgeye göre mevcut Ürünler'e](https://azure.microsoft.com/global-infrastructure/services/?products=hdinsight)bakın.

## <a name="choose-storage-location-and-size"></a>Depolama konumu ve boyutunu seçin

### <a name="location-of-default-storage"></a>Varsayılan depolama alanının konumu

Varsayılan depolama alanı (Azure Depolama hesabı veya Azure Veri Gölü Depolama) kümenizle aynı konumda olmalıdır. Azure Depolama tüm konumlarda kullanılabilir. Veri Gölü Depolama Gen1 bazı bölgelerde kullanılabilir - mevcut [Veri Gölü Depolama kullanılabilirliğine](https://azure.microsoft.com/global-infrastructure/services/?products=storage)bakın.

### <a name="location-of-existing-data"></a>Varolan verilerin konumu

Kümenizin varsayılan depolama alanı olarak varolan bir depolama hesabı veya Veri Gölü Depolamakullanmak istiyorsanız, kümenizi aynı konumda dağıtmanız gerekir.

### <a name="storage-size"></a>Depolama boyutu

Dağıtılan bir kümede, ek Azure Depolama hesapları ekleyebilir veya diğer Veri Gölü Depolama alanına erişebilirsiniz. Tüm depolama hesaplarınız kümenizle aynı konumda yaşamalı. Veri Gölü Depolama farklı bir konumda olabilir, ancak büyük mesafeler bazı gecikme lere neden olabilir.

Azure Depolama'nın bazı [kapasite sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits)vardır, Data Lake Storage Gen1 ise neredeyse sınırsızdır.

Küme, farklı depolama hesaplarının birleşimine erişebilir. Tipik örnekler şunlardır:

* Veri miktarı, tek bir blob depolama kapsayıcısının depolama kapasitesini aşması muhtemel olduğunda.
* Blob kapsayıcısına erişim hızı, azaltmanın gerçekleştiği eşiği aşabilir.
* Veri yapmak istediğinizde, kümenin kullanabileceği bir blob kapsayıcısına zaten yüklemiş siniz.
* Güvenlik nedeniyle depolama alanının farklı bölümlerini yalıtmak veya yönetimi basitleştirmek istediğinizde.

Daha iyi performans için depolama hesabı başına yalnızca bir kapsayıcı kullanın.

## <a name="choose-a-cluster-type"></a>Küme türü seçin

Küme türü, HDInsight kümenizin çalışacak şekilde yapılandırıldığı iş yükünü belirler. Türleri [Apache Hadoop](./hadoop/apache-hadoop-introduction.md)dahil , [Apache Storm](./storm/apache-storm-overview.md), [Apache Kafka](./kafka/apache-kafka-introduction.md), veya [Apache Spark](./spark/apache-spark-overview.md). Kullanılabilir küme türlerinin ayrıntılı bir açıklaması için azure [HDInsight'a Giriş bölümüne](hdinsight-overview.md#cluster-types-in-hdinsight)bakın. Her küme türü, düğüm boyutu ve sayısı için gereksinimleri içeren belirli bir dağıtım topolojisi vardır.

## <a name="choose-the-vm-size-and-type"></a>VM boyutunu ve türünü seçin

Her küme türünde bir düğüm türü kümesi vardır ve her düğüm türü, VM boyutu ve türü için belirli seçeneklere sahiptir.

Uygulamanız için en uygun küme boyutunu belirlemek için küme kapasitesini kıyaslayabilir ve belirtilen boyutu artırabilirsiniz. Örneğin, benzetilen bir iş yükü veya *kanarya sorgusu*kullanabilirsiniz. Benzetimli iş yüklerinizi farklı boyut kümelerinde çalıştırın. Amaçlanan performansa ulaşılıncaya kadar boyutu kademeli olarak artırın. Kümenin yeterli kaynağa sahip olup olmadığını göstermek için diğer üretim sorguları arasına düzenli aralıklarla bir kanarya sorgusu eklenebilir.

İş yükünüz için doğru VM ailesini nasıl seçeceğiniz hakkında daha fazla bilgi için [bünyeniz için doğru VM boyutunu seçme](hdinsight-selecting-vm-size.md)ye bakın.

## <a name="choose-the-cluster-scale"></a>Küme ölçeğini seçin

Bir kümenin ölçeği VM düğümlerinin miktarına göre belirlenir. Tüm küme türleri için, belirli bir ölçek ve ölçek-out destekleyen düğüm türleri vardır. Örneğin, bir küme tam olarak üç [Apache ZooKeeper](https://zookeeper.apache.org/) düğümleri veya iki Baş düğümleri gerektirebilir. Dağıtılmış bir şekilde veri işleme yapan alt düğümler ek alt düğümlerden yararlanır.

Küme türünüze bağlı olarak, alt düğüm sayısını artırmak ek hesaplama kapasitesi (daha fazla çekirdek gibi) ekler. Daha fazla düğüm, işlenen verilerin bellek içi depolamasını desteklemek için tüm küme için gereken toplam belleği artırır. VM boyutu ve türü seçiminde olduğu gibi, doğru küme ölçeğini seçmek genellikle ampirik olarak ulaşılır. Benzetimli iş yüklerini veya kanarya sorgularını kullanın.

En yüksek yük taleplerini karşılamak için kümenizi ölçeklendirebilirsiniz. Daha sonra bu ekstra düğümler artık gerekli olduğunda geri ölçeklendirin. [Otomatik Ölçeklendirme özelliği,](hdinsight-autoscale-clusters.md) önceden belirlenmiş ölçümlere ve zamanlamalara göre kümenizi otomatik olarak ölçeklendirmenize olanak tanır. Kümelerinizi el ile ölçeklendirme hakkında daha fazla bilgi için [HDInsight kümelerini ölçeklendir'e](hdinsight-scaling-best-practices.md)bakın.

### <a name="cluster-lifecycle"></a>Küme yaşam döngüsü

Bir kümenin ömrü için ücretlendirilirsiniz. Kümenize ihtiyacınız olan yalnızca belirli zamanlar varsa, [Azure Veri Fabrikası'nı kullanarak isteğe bağlı kümeler oluşturun.](hdinsight-hadoop-create-linux-clusters-adf.md) Ayrıca, kümenizi sağlayan ve silen PowerShell komut dosyaları oluşturabilir ve ardından [Azure Otomasyonu'nu](https://azure.microsoft.com/services/automation/)kullanarak bu komut dosyalarını zamanlayabilirsiniz.

> [!NOTE]  
> Bir küme silindiğinde, varsayılan Hive metastore'u da silinir. Bir sonraki küme yeniden oluşturma için metastore'u sürdürmek için Azure Veritabanı veya [Apache Oozie](https://oozie.apache.org/)gibi harici bir meta veri deposu kullanın.
<!-- see [Using external metadata stores](hdinsight-using-external-metadata-stores.md). -->

### <a name="isolate-cluster-job-errors"></a>Küme iş hatalarını yalıtma

Bazen hatalar, birden çok eşin paralel yürütülmesi nedeniyle oluşabilir ve çok düğümlü kümedeki bileşenleri azaltabilir. Sorunu yalıtmaya yardımcı olmak için dağıtılmış sınamayı deneyin. Tek bir işçi düğüm kümesinde eşzamanlı birden çok iş çalıştırın. Ardından, birden fazla düğüm içeren kümelerde aynı anda birden çok işi çalıştırmak için bu yaklaşımı genişletin. Azure'da tek düğümlü HDInsight kümesi oluşturmak *`Custom(size, settings, apps)`* için seçeneği kullanın ve portalda yeni bir küme sağlarken **Küme boyutu** bölümündeki *Çalışan Sayısı düğümleri* için 1 değerini kullanın.

## <a name="quotas"></a>Kotalar

Hedef küme VM boyutunu, ölçeğini ve türünü belirledikten sonra aboneliğinizin geçerli kota kapasite sınırlarını kontrol edin. Kota sınırına ulaştığınızda, yeni kümeler dağıtamazsınız. Veya daha fazla alt düğüm ekleyerek varolan kümeleri ölçeklendirin. Tek kota sınırı, her abonelik için bölge düzeyinde bulunan CPU Çekirdekleri kotasıdır. Örneğin, aboneliğinizin Doğu ABD bölgesinde 30 temel sınırı olabilir.

Kullanılabilir çekirdeklerinizi kontrol etmek için aşağıdaki adımları yapın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. HDInsight kümesi için **Genel Bakış** sayfasına gidin.
3. Sol menüde **Kota sınırlarını**seçin.

   Sayfada kullanılan çekirdek sayısı, kullanılabilir çekirdek sayısı ve toplam çekirdek görüntülenir.

Kota artışı istemeniz gerekiyorsa, aşağıdaki adımları yapın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Sayfanın sol alt tarafındaki **Yardım + destek'i** seçin.
1. **Yeni destek isteği**’ni seçin.
1. Temel **bilgiler** sekmesi altında **Yeni destek isteği** sayfasında aşağıdaki seçenekleri seçin:

   - **Sorun türü**: **Hizmet ve abonelik limitleri (kotalar)**
   - **Abonelik**: değiştirmek istediğiniz abonelik
   - **Kota türü**: **HDInsight**

     ![HDInsight çekirdek kotasını artırmak için bir destek isteği oluşturma](./media/hdinsight-capacity-planning/hdinsight-quota-support-request.png)

1. **Sonraki'ni seçin: Çözümler >>. **
1. **Ayrıntılar** sayfasında, sorunun açıklamasını girin, sorunun önem derecesini, tercih ettiğiniz kişi yöntemini ve diğer gerekli alanları seçin.
1. **Sonraki'ni Seçin: Gözden Geçirin + >>oluşturun. **
1. Gözden **Geçir + oluştur** sekmesinde **Oluştur'u**seçin.

> [!NOTE]  
> Özel bir bölgedeki HDInsight çekirdek kotasını artırmanız gerekiyorsa, [bir beyaz liste isteği gönderin.](https://aka.ms/canaryintwhitelist)

Kota [artışı istemek için desteğe başvurabilirsiniz.](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request)

Bazı sabit kota sınırları vardır. Örneğin, tek bir Azure aboneliğinde en fazla 10.000 çekirdek olabilir. Bu sınırlar la ilgili ayrıntılar için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Hadoop, Spark, Kafka ve daha fazlası ile HDInsight'ta kümeler ayarlayın](hdinsight-hadoop-provision-linux-clusters.md): HDInsight'ta kümeleri nasıl ayarlayıp yapılandıracağınızı öğrenin.
* [Küme performansını izleyin](hdinsight-key-scenarios-to-monitor.md): HDInsight kümeniz için kümenizin kapasitesini etkileyebilecek önemli senaryolar hakkında bilgi edinin.
