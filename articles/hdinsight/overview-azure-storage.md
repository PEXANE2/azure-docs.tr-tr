---
title: HDInsight'ta Azure Depolama'ya genel bakış
description: HDInsight'ta Azure Depolama'ya Genel Bakış.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: 43d948a2a98407bacc212ddc6e065c67a105f332
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81873387"
---
# <a name="azure-storage-overview-in-hdinsight"></a>HDInsight'ta Azure Depolama'ya genel bakış

Azure Depolama, HDInsight ile sorunsuz bir şekilde entegre olan güçlü bir genel amaçlı depolama çözümüdür. HDInsight, Azure Depolama’daki bir blob kapsayıcıyı kümenin varsayılan dosya sistemi olarak kullanabilir. HDFS arabirimi aracılığıyla, HDInsight'taki tüm bileşenler blob olarak depolanan yapılandırılmış veya yapılandırılmamış veriler üzerinde doğrudan çalışabilir.

Varsayılan küme depolama nız ve iş verileriniz için ayrı depolama kapsayıcıları kullanmanızı öneririz. Ayrım, HDInsight günlüklerini ve geçici dosyaları kendi iş verilerinizden yalıtmaktır. Ayrıca, depolama maliyetini azaltmak için her kullanımdan sonra uygulama ve sistem günlükleri içeren varsayılan blob kapsayıcısını silmenizi öneririz. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Güvenlik **Duvarları ve** **Seçili ağlardaki**sanal ağlar kısıtlamalarıyla depolama hesabınızı güvenli hale getirmeyi seçerseniz, özel durum **Güvenilen Microsoft hizmetlerine izin verin'i**etkinleştirmeyi etkinleştirdiğinizden emin olun... Bunun istisnası, HDInsight'ın depolama hesabınıza erişebilmeleridir.

## <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi

Aşağıdaki diyagram, Azure Depolama'nın HDInsight mimarisinin soyut bir görünümünü sağlar:

!['HDInsight Depolama Mimarisi'](./media/overview-azure-storage/storage-architecture.png "HDInsight Depolama Mimarisi")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

HDInsight aracılığıyla Azure Depolama'daki verilere de erişebilirsiniz. Söz dizimi aşağıdaki gibidir:

    wasb://<containername>@<accountname>.blob.core.windows.net/<path>

HDInsight kümelerine sahip bir Azure Depolama hesabı kullanırken aşağıdaki ilkeleri göz önünde bulundurun:

* **Bir kümeye bağlı depolama hesaplarındaki kapsayıcılar:** Oluşturma sırasında hesap adı ve anahtar kümeyle ilişkilendirildiğinden, bu kapsayıcılardaki blob'lara tam erişiminiz vardır.

* **Kümeye bağlı olmayan depolama hesaplarındaki ortak kapsayıcılar veya ortak lekeler:** Kaplarda ki lekeler için salt okunur izniniz var.
  
  > [!NOTE]  
  > Ortak kapsayıcılar, bu kapsayıcıda bulunan tüm lekelerin bir listesini almanızı ve kapsayıcı meta verilerini almanızı sağlar. Genel blob'lar, yalnızca tam URL'yi biliyorsanız blob erişiminize izin verir. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../storage/blobs/storage-manage-access-to-resources.md).

* Bir **kümeye bağlı olmayan depolama hesaplarındaki özel kapsayıcılar:** WebHCat işlerini gönderirken depolama hesabını tanımlamadığınız sürece kapsayıcılarda bulunan lekelere erişemezsiniz.

Oluşturma işleminde tanımlanan depolama hesapları ve bunların anahtarların %HADOOP_HOME%/conf/core-site.xml küme düğümlerinde depolanır. Varsayılan olarak, HDInsight çekirdek site.xml dosyasında tanımlanan depolama hesaplarını kullanır. Bu ayarı [Apache Ambari'yi](./hdinsight-hadoop-manage-ambari.md)kullanarak değiştirebilirsiniz.

Apache Hive dahil olmak üzere birden fazla WebHCat işi. Ve MapReduce, Apache Hadoop akışı ve Apache Pig, depolama hesapları ve meta verilerin bir açıklamasını taşır. (Bu yönü şu anda depolama hesapları ile Pig için geçerlidir ama meta veriler için geçerli değildir.) Daha fazla bilgi için bkz: [Alternatif depolama hesapları ve metastore'lar içeren bir HDInsight kümesi kullanma.](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx)

Bloblar yapılandırılmış ve yapılandırılmamış veriler için kullanılabilir. Blob kapsayıcıları verileri anahtar/değer çiftleri olarak depolar ve dizin hiyerarşisi yoktur. Ancak anahtar adı bir dizini yapısı içinde depolanmış gibi görünmesi için bir eğik çizgi karakteri ( / ) içerebilir. Örneğin, bir blob anahtarı olabilir `input/log1.txt`. Gerçek `input` bir dizin yoktur, ancak anahtar adındaki eğik çizgi karakteri nedeniyle anahtar bir dosya yoluna benzer.

## <a name="benefits-of-azure-storage"></a>Azure Depolamanın yararları

Bir araya gelebilen işlem kümeleri ve depolama kaynaklarının zımni performans maliyetleri vardır. Bu maliyetler, azure bölgesindeki depolama hesabı kaynaklarına yakın bir şekilde oluşturulan işlem kümeleri tarafından azaltılır. Bu bölgede, bilgi işlem düğümleri Verilere Azure Depolama'daki yüksek hızlı ağ üzerinden verimli bir şekilde erişebilir.

Verileri HDFS yerine Azure Depolama'da depoladiğinizde, aşağıdakiler gibi avantajlar elde elabilirsiniz:

* **Verileri yeniden kullanma ve paylaşma:** HDFS’deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Buna karşılık Azure Depolama'daki verilere HDFS API'leri veya Blob depolama REST API'leri aracılığıyla erişilebilir. Bu düzenleme sayesinde, verileri üretmek ve tüketmek için daha büyük bir uygulama kümesi (diğer HDInsight kümeleri dahil) ve araçlar kullanılabilir.

* **Veri arşivleme:** Veriler Azure Depolama'da depolandığında, hesaplama için kullanılan HDInsight kümeleri kullanıcı verilerini kaybetmeden güvenli bir şekilde silinebilir.

* **Veri depolama maliyeti:** Verileri uzun vadede DFS'de depolamak, verileri Azure Depolama'da depolamaktan daha maliyetlidir. Çünkü bir işlem kümesinin maliyeti Azure Depolama'nın maliyetinden daha yüksektir. Ayrıca, verilerin her bilgi işlem kümesi oluşturma için yeniden yüklenmesi gerekmedığından, veri yükleme maliyetlerinden de tasarruf emiş oluyorsunuz.

* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi sağlamakla birlikte, ölçek, kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, Azure Depolama'da otomatik olarak aldığınız elastik ölçekleme özelliklerinden daha karmaşık olabilir.

* **Coğrafi çoğaltma:** Azure Depolama alanınız coğrafi olarak çoğaltılabilir. Coğrafi çoğaltma size coğrafi kurtarma ve veri artıklığı verse de, coğrafi olarak çoğaltılan konumun üzerinde bir hata performansınızı ciddi şekilde etkiler ve ek maliyetlere neden olabilir. Bu nedenle, yalnızca verilerin değeri ek maliyeti haklı gösteriyorsa, coğrafi çoğaltmayı dikkatli bir şekilde seçin.

Belirli MapReduce işleri ve paketleri, Azure Depolama'da depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS'de depolamayı seçebilirsiniz. HDInsight, Kovan işlerinde ve diğer işlemlerde bu ara sonuçların birkaçı için DFS kullanır.

> [!NOTE]  
> Çoğu HDFS komutu (örneğin, `ls`, `copyFromLocal`ve `mkdir`) Azure Depolama'da beklendiği gibi çalışır. Yalnızca yerel HDFS uygulamasına özgü komutlar (DFS olarak adlandırılır) `fschk` ve `dfsadmin`Azure Depolama'da farklı davranışlar gösterir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gölü Depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md)
* [Azure Depolama’ya giriş](../storage/common/storage-introduction.md)
* [Azure Veri Gölü Depolama Gen1 genel bakış](./overview-data-lake-storage-gen1.md)