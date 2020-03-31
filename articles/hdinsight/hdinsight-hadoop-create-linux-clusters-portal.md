---
title: Web tarayıcısı, Azure HDInsight kullanarak Apache Hadoop kümeleri oluşturun
description: Bir web tarayıcısı ve Azure portalı kullanarak HDInsight için Linux'ta Apache Hadoop, Apache HBase, Apache Storm veya Apache Spark kümelerini nasıl oluşturacağınızhakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/19/2020
ms.openlocfilehash: bcc71b7244ba3498b2fcde023d372e67f41d6117
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77623254"
---
# <a name="create-linux-based-clusters-in-hdinsight-by-using-the-azure-portal"></a>Azure portalını kullanarak HDInsight'ta Linux tabanlı kümeler oluşturun

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure portalı, Microsoft Azure bulutunda barındırılan hizmetler ve kaynaklar için web tabanlı bir yönetim aracıdır. Bu makalede, portalı kullanarak Linux tabanlı Azure HDInsight kümelerini nasıl oluşturabileceğinizi öğrenirsiniz. [HDInsight kümeleri oluştur'dan](./hdinsight-hadoop-provision-linux-clusters.md)ek ayrıntılar alabilirsiniz.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Azure portalı küme özelliklerinin çoğunu ortaya çıkarır. Azure Kaynak Yöneticisi şablonlarını kullanarak birçok ayrıntıyı gizleyebilirsiniz. Daha fazla bilgi için, [Kaynak Yöneticisi şablonlarını kullanarak HDInsight'ta Apache Hadoop kümeleri oluşturun'a](hdinsight-hadoop-create-linux-clusters-arm-templates.md)bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-clusters"></a>Küme oluşturma

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Üst menüden **+ Kaynak oluştur'u**seçin.

    ![Azure portalında yeni bir küme oluşturma](./media/hdinsight-hadoop-create-linux-clusters-portal/azure-portal-create-resource.png "Azure portalında yeni bir küme oluşturma")

1. **HDInsight oluştur küme** sayfasına gitmek için **Analytics** > **Azure HDInsight'ı** seçin.

## <a name="basics"></a>Temel Bilgiler

![HDInsight küme temelleri oluşturmak](./media/hdinsight-hadoop-create-linux-clusters-portal/azure-portal-cluster-basics.png "Azure portalında yeni bir küme oluşturma")

Temel **Bilgiler** sekmesinden aşağıdaki bilgileri sağlayın:

|Özellik |Açıklama |
|---|---|
|Abonelik|Açılan listeden, küme için kullanılan Azure aboneliğini seçin.|
|Kaynak grubu|Açılan listeden, varolan kaynak grubunuzu seçin veya **yeni oluştur'u**seçin.|
|Küme adı|Genel olarak benzersiz bir ad girin.|
|Bölge|Açılan listeden, kümenin oluşturulduğu bir bölge seçin.|
|Küme türü|Liste açmak için **küme türünü seçin'i** seçin. Listeden istediğiniz küme türünü seçin. HDInsight kümeleri çeşitli türlerde gelir. Bunlar, kümenin ayarlı olduğu iş yüküne veya teknolojiye karşılık gelir. Birden çok türü birleştiren bir küme oluşturmak için desteklenen bir yöntem yoktur.|
|Sürüm|Açılan listeden bir **sürüm**seçin. Ne seçeceğinizi bilmiyorsanız varsayılan sürümü kullanın. Daha fazla bilgi için bkz. [HDInsight küme sürümleri](hdinsight-component-versioning.md).|
|Küme oturum açma kullanıcı adı|Kullanıcı adı sağlayın, varsayılan **yönetici**.|
|Küme oturum açma parolası|Parolayı girin.|
|Küme giriş parolalarını onaylama|Parolayı yeniden girin|
|Secure Shell (SSH) kullanıcı adı|Kullanıcı adı sağlayın, varsayılan **sshuser** olduğunu|
|SSH için küme giriş parolası kullanma|Daha önce belirttiğiniz yönetici parolasıyla aynı SSH parolasını istiyorsanız, SSH onay kutusu **için küme giriş parolasını kullanın'ı** seçin. Değilse, SSH kullanıcısının kimliğini doğrulamak için bir **PAROLA** veya **PUBLIC KEY** sağlayın. Ortak anahtar, önerdiğimiz yaklaşımdır. Kimlik bilgileri yapılandırmasını kaydetmek için en altta **Seç'i** seçin.  Daha fazla bilgi için, [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](hdinsight-hadoop-linux-use-ssh-unix.md)bakın.|

**Sonraki'ni seçin:** Bir sonraki sekmeye ilerlemek için depolama >>.

## <a name="storage"></a>Depolama

![HDInsight küme depolama oluşturmak](./media/hdinsight-hadoop-create-linux-clusters-portal/azure-portal-cluster-storage.png "Azure portalında yeni bir küme oluşturma - depolama")

### <a name="primary-storage"></a>Birincil depolama

**Birincil depolama türü**için açılan listeden varsayılan depolama türünü seçin. Tamamlanacak sonraki alanlar seçiminize bağlı olarak değişir. **Azure Depolama**için:

1. **Seçim yöntemi için** **listeden seç**veya **erişim anahtarını kullan'ı**seçin.
    * **Listeden Seç**için açılan listeden **Birincil depolama hesabınızı** seçin veya yeni **oluştur'u**seçin.
    * **Erişim anahtarını kullan** **için, Depolama hesabı adınızı**girin. Ardından **Access tuşunu**sağlayın.

1. **Kapsayıcı**için varsayılan değeri kabul edin veya yeni bir değer girin.

### <a name="additional-azure-storage"></a>Ek Azure Depolama

İsteğe bağlı: Ek küme depolama alanı için **Azure Depolama Ekle'yi** seçin. HDInsight kümesinden farklı bir bölgede ek bir depolama hesabı kullanmak desteklenmez.

### <a name="metastore-settings"></a>Metastore Ayarları

İsteğe bağlı: Apache Hive, Apache Oozie ve/veya Apache Ambari meta verilerini kümenin dışında kaydetmek için varolan bir SQL Veritabanı belirtin. Metastore için kullanılan Azure SQL Veritabanı, Azure HDInsight da dahil olmak üzere diğer Azure hizmetlerine bağlantı sağlar. Bir metastore oluşturduğunuzda, tire veya tire içeren bir veritabanına ad vermeyin. Bu karakterler küme oluşturma işleminin başarısız olması için neden olabilir.

**Sonraki'ni seçin: Bir** sonraki sekmeye ilerlemek için Güvenlik + ağ >>.

## <a name="security--networking"></a>Güvenlik + ağ

![HDInsight küme güvenlik ağı oluşturmak](./media/hdinsight-hadoop-create-linux-clusters-portal/azure-portal-cluster-security-networking.png "HDInsight küme güvenlik ağı oluşturmak")

Güvenlik **+ ağ** sekmesinden aşağıdaki bilgileri sağlayın:

|Özellik |Açıklama |
|---|---|
|Kurumsal güvenlik paketi|İsteğe bağlı: Kurumsal **Güvenlik Paketini**kullanmak için onay kutusunu seçin. Daha fazla bilgi için Azure [Active Directory Etki Alanı Hizmetlerini kullanarak Kurumsal Güvenlik Paketi ile HDInsight kümesini yapılandırma'ya](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)bakın.|
|TLS|İsteğe bağlı: Açılan listeden bir TLS sürümü seçin. Daha fazla bilgi için [Taşıma Katmanı Güvenliği'ne](./hdinsight-plan-virtual-network-deployment.md#transport-layer-security)bakın.|
|Sanal ağ|İsteğe bağlı: Açılan listeden varolan bir sanal ağ ve alt ağı seçin. Daha fazla bilgi için Azure [HDInsight kümeleri için sanal ağ dağıtımı planı'na](hdinsight-plan-virtual-network-deployment.md)bakın. Makale, sanal ağ için belirli yapılandırma gereksinimlerini içerir.|
|Disk şifreleme ayarları|İsteğe bağlı: Şifreleme kullanmak için onay kutusunu seçin. Daha fazla bilgi için [Müşteri tarafından yönetilen anahtar disk şifrelemesi'ne](./disk-encryption.md)bakın.|
|Kafka REST ara sunucusu|Bu ayar yalnızca Kafka kümesi türü için kullanılabilir. Daha fazla bilgi için REST [proxy'si kullanma'ya](./kafka/rest-proxy.md)bakın.|
|Kimlik|İsteğe bağlı: Açılan listeden kullanıcı tarafından atanmış bir hizmet kimliği seçin. Daha fazla bilgi için Azure [HDInsight'ta Yönetilen kimlikler'e](./hdinsight-managed-identities.md)bakın.|

**Sonraki'ni seçin: Yapılandırma + fiyatlandırma >>** sonraki sekmeye ilerlemek için.

## <a name="configuration--pricing"></a>Yapılandırma + fiyatlandırma

![HDInsight küme yapılandırması oluşturmak](./media/hdinsight-hadoop-create-linux-clusters-portal/azure-portal-cluster-configuration.png "Yapılandırma ve fiyatlandırma sekmesi")

Yapılandırma **+ fiyatlandırma** sekmesinden aşağıdaki bilgileri sağlayın:

|Özellik |Açıklama |
|---|---|
|+ Uygulama ekle|İsteğe bağlı: İstediğiniz uygulamaları seçin. Microsoft, bağımsız yazılım satıcıları (ISV) veya bu uygulamaları geliştirebilirsiniz. Daha fazla bilgi için [küme oluşturma sırasında uygulamaları yükle'ye](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation)bakın.|
|Düğüm boyutu|İsteğe bağlı: Farklı boyutlu bir düğüm seçin.|
|Düğüm sayısı|İsteğe bağlı: Belirtilen düğüm türü için düğüm sayısını girin. 32'den fazla işçi düğümü planlıyorsanız, en az sekiz çekirdekli ve 14 GB RAM'li bir baş düğümü boyutu seçin. Düğümleri küme oluşturmada veya oluşturulduktan sonra kümeyi ölçekleyerek planlayın.|
|Otomatik ölçeklendirmeyi etkinleştirme|İsteğe bağlı: Özelliği etkinleştirmek için onay kutusunu seçin. Daha fazla bilgi için bkz: [Azure HDInsight kümelerini otomatik olarak ölçeklendirin.](./hdinsight-autoscale-clusters.md)|
|+ Komut dosyası eylemi ekleme|İsteğe bağlı: Küme oluşturulurken, kümeyi özelleştirmek için özel bir komut dosyası kullanmak istiyorsanız bu seçenek çalışır. Komut dosyası eylemleri hakkında daha fazla bilgi için, [komut dosyası eylemlerini kullanarak Linux tabanlı HDInsight kümelerini özelleştir'e](hdinsight-hadoop-customize-cluster-linux.md)bakın.|

Küme yapılandırmasını doğrulamak ve son sekmeye ilerlemek için **Gözden Geçir + >>oluşturun'u** seçin.

## <a name="review--create"></a>İnceleme + oluşturma

![HDInsight küme özeti oluşturma](./media/hdinsight-hadoop-create-linux-clusters-portal/azure-portal-cluster-review-create-hadoop.png "Küme düğümlerinin sayısını belirtin")

Ayarları gözden geçirin. **Oluştur**’u seçerek kümeyi oluşturun.

Kümenin oluşturulması genellikle yaklaşık 20 dakika sürer. Sağlama işlemini denetlemek için **Bildirimleri** izleyin.

## <a name="post-creation"></a>Oluşturma sonrası

Oluşturma işlemi bittikten sonra, **Dağıtım'dan** **Kaynağa Git'i** seçin. Küme penceresi aşağıdaki bilgileri sağlar.

![HDI Azure portal kümesine genel bakış](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-completed.png "Küme özellikleri")

Penceredeki simgelerden bazıları aşağıdaki gibi açıklanmıştır:

|Özellik | Açıklama |
|---|---|
|Genel Bakış|Küme hakkında tüm temel bilgileri sağlar. Örnekler, ait olduğu kaynak grubu, konumu, işletim sistemi ve küme panosunun URL'sidir.|
|Küme panoları|Sizi kümeyle ilişkili Ambari portalına yönlendirir.|
|SSH + Cluster giriş|SSH kullanarak kümeye erişmek için gereken bilgileri sağlar.|
|Sil|HDInsight kümesini siler.|

## <a name="customize-clusters"></a>Kümeleri özelleştirme

* [Bootstrap kullanarak HDInsight kümelerini özelleştirin](hdinsight-hadoop-customize-cluster-bootstrap.md)
* [Komut dosyası eylemlerini kullanarak Linux tabanlı HDInsight kümelerini özelleştirin](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="delete-the-cluster"></a>Küme silme

Bkz. [Tarayıcınızı, PowerShell'i veya Azure CLI'nizi kullanarak bir HDInsight kümesini sil.](./hdinsight-delete-cluster.md)

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](./hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="next-steps"></a>Sonraki adımlar

Bir HDInsight kümesini başarıyla oluşturdunuz. Şimdi kümenizle nasıl çalışacağınızı öğrenin.

### <a name="apache-hadoop-clusters"></a>Apaçi Hadoop kümeleri

* [HDInsight ile Apache Hive'ı kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile MapReduce'ı kullanın](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase kümeleri

* [HDInsight'ta Apache HBase ile başlayın](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight'ta Apache HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apaçi Fırtına kümeleri

* [HDInsight'ta Apache Storm için Java topolojileri geliştirin](storm/apache-storm-develop-java-topology.md)
* [HDInsight'ta Apache Storm'da Python bileşenlerini kullanma](storm/apache-storm-develop-python-topology.md)
* [HDInsight'ta Apache Storm ile topolojileri dağıtın ve izleyin](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Apache Kıvılcım kümeleri

* [Scala'yı kullanarak bağımsız bir uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [Apache Livy'yi kullanarak işleri Apache Spark kümesinde uzaktan çalıştırın](spark/apache-spark-livy-rest-interface.md)
* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi gerçekleştirin](spark/apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](spark/apache-spark-machine-learning-mllib-ipython.md)
