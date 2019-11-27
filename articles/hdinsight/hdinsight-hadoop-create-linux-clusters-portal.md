---
title: Web tarayıcısı, Azure HDInsight kullanarak Apache Hadoop kümeleri oluşturma
description: Bir Web tarayıcısı ve Azure portal kullanarak HDInsight için Linux 'ta Apache Hadoop, Apache HBase, Apache Storm veya Apache Spark kümeleri oluşturmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: 820ddb8d06cfd2aac2b053305f23ad330e4fd7c3
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74215898"
---
# <a name="create-linux-based-clusters-in-hdinsight-by-using-the-azure-portal"></a>Azure portal kullanarak HDInsight 'ta Linux tabanlı kümeler oluşturun

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure portal, Microsoft Azure bulutta barındırılan hizmetler ve kaynaklar için Web tabanlı bir yönetim aracıdır. Bu makalede, portalı kullanarak Linux tabanlı Azure HDInsight kümeleri oluşturmayı öğreneceksiniz.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği**. [HDInsight 'Ta Hadoop test Için Azure Ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)konusuna bakın.
* **Modern bir Web tarayıcısı**. Azure portal HTML5 ve JavaScript kullanır. Daha eski web tarayıcılarında düzgün çalışmayabilir.

## <a name="create-clusters"></a>Küme oluşturma

Azure portal, küme özelliklerinin çoğunu gösterir. Azure Resource Manager şablonları kullanarak, birçok ayrıntıyı gizleyebilirsiniz. Daha fazla bilgi için bkz. [HDInsight 'ta Kaynak Yöneticisi şablonları kullanarak Apache Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Sol menüden + **Azure hdınsight** > kaynak >  **Analizi** **Oluştur ' a** gidin.

    ![Azure portal yeni bir küme oluşturun](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster.png "Azure portal yeni bir küme oluşturma")

1. **HDInsight kümesi oluştur** sayfasında, **Klasik oluşturma deneyimine git**' i seçin.

    ![Klasik oluşturma deneyimine gidin](./media/hdinsight-hadoop-create-linux-clusters-portal/azure-portal-cluster-create-classic.png)

1. **HDInsight** sayfasında **özel (boyut, ayarlar, uygulamalar)** öğesini seçin.

1. **1 temel bilgiler**' i seçin. Ardından aşağıdaki bilgileri girin:

    |Özellik |Açıklama |
    |---|---|
    |Küme adı|Bu adın küresel olarak benzersiz olması gerekir.|
    |Abonelik|Aşağı açılan listeden, küme için kullanılan Azure aboneliğini seçin.|
    |Küme türü|Oluşturmak istediğiniz küme türünü seçin. Hadoop ve Apache Spark örnekler aşağıda verilmiştir. **İşletim sistemi** **Linux**olur. Sonra bir küme türü sürümü seçin. Ne seçeceğimizi bilmiyorsanız varsayılan sürümü kullanın. Daha fazla bilgi için bkz. [HDInsight küme sürümleri](hdinsight-component-versioning.md).|
    |Küme oturum açma kullanıcı adı|Kullanıcı adını belirtin, varsayılan olarak **admin**' dir.|
    |Küme oturum açma parolası|Parolayı girin.|
    |Secure Shell (SSH) kullanıcı adı|Varsayılan olarak **sshuser**. Daha önce belirttiğiniz yönetici parolasıyla aynı SSH parolasını istiyorsanız **SSH için küme oturum açma parolasını kullan** onay kutusunu işaretleyin. Aksi durumda, SSH kullanıcısının kimliğini doğrulamak için bir **parola** veya **ortak anahtar** sağlayın. Ortak anahtar, önerdiğimiz yaklaşımdır. Kimlik bilgileri yapılandırmasını kaydetmek için en altta **Seç ' i** seçin.  Daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'a (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).|
    |Kaynak grubu|Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin.|
    |Konum|Kümenin oluşturulduğu bir veri merkezi belirtin.|

    ![HDInsight küme oluşturma temelleri](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-basics.png "Azure portal yeni bir küme oluşturma")

    > [!IMPORTANT]  
    > HDInsight kümeleri çeşitli türlerde gelir. Bunlar, kümenin ayarlanmış olduğu iş yüküne veya teknolojiye karşılık gelir. Birden çok türü birleştiren bir küme oluşturmak için desteklenen bir yöntem yoktur. Örnekler, bir kümede fırtınası ve HBase örnekleridir.

    Sonraki sayfaya geçmek için **İleri ' yi** seçin.

1. **2 güvenlik + ağ**üzerinden, belirtilen açılan menüyü kullanarak kümenizi bir sanal ağa bağlayabilirsiniz. Kümeyi bir sanal ağa yerleştirmek istiyorsanız bir Azure sanal ağı ve alt ağı seçin. Bir sanal ağla HDInsight kullanma hakkında daha fazla bilgi için bkz. [Azure HDInsight kümeleri için sanal ağ dağıtımı planlaması](hdinsight-plan-virtual-network-deployment.md). Bu makale, sanal ağ için belirli yapılandırma gereksinimlerini içerir.

    **Kurumsal güvenlik paketi**kullanmak istiyorsanız şu yönergeleri izleyin: [Azure Active Directory Domain Services kullanarak kurumsal güvenlik paketi bir HDInsight kümesi yapılandırma](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds).

    Sonraki sayfaya geçmek için **İleri ' yi** seçin.

1. **Depolama hesabı ayarları**Için **3 depolama**alanından Azure Storage veya Azure Data Lake Storage varsayılan depolama alanı olarak isteyip istemediğinizi belirtin. Daha fazla bilgi için aşağıdaki tabloya bakın.

    | Birincil depolama türü | Açıklama |
    |------------------|-------------|
    | Azure Depolama   |  * **Seçim yöntemi**Için, Azure aboneliğinizin bir parçası olan bir depolama hesabı belirtmek Istiyorsanız **Aboneliklerim** ' i seçin. Ardından Depolama hesabını seçin. Aksi takdirde, **erişim anahtarı**' nı seçin. Ardından, Azure aboneliğinizin dışından seçim yapmak istediğiniz depolama hesabı için bilgileri sağlayın.</br></br> * **Varsayılan kapsayıcı**için Portal tarafından önerilen varsayılan kapsayıcı adını seçin ya da kendinizinkini belirtin.</br></br> * Azure Blob depolama alanı varsayılan depolarınızda, kümeyle ilişkilendirilecek ek depolama hesapları belirtmek için **ek depolama hesapları** da seçebilirsiniz. **Azure depolama anahtarları**için **depolama anahtarı Ekle**' yi seçin. Daha sonra, Azure aboneliklerinizden veya diğer aboneliklerden bir depolama hesabı sağlayabilirsiniz. Depolama hesabı erişim anahtarı sağlayın.</br></br> * BLOB depolama alanı varsayılan depolarınız ise, ek depolama alanı olarak Azure Data Lake Storage belirtmek için **Data Lake Storage erişim** ' i de seçebilirsiniz. Daha fazla bilgi için bkz. [hızlı başlangıç: HDInsight 'ta kümeleri ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
    | Azure Data Lake Storage | **Azure Data Lake Storage 1.** veya **Azure Data Lake Storage 2.** seçin. Daha sonra yönergeler için [hızlı başlangıç: HDInsight 'ta kümeleri ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) makalesine başvurun. |

    **Meta veri deposu ayarları (isteğe bağlı)**

    Bir seçenek olarak, Apache Hive kaydedilecek bir SQL veritabanı ve kümeyle ilişkili Apache Oozie meta verilerini belirtin. **Hive için BIR SQL veritabanı seçin**IÇIN bir SQL veritabanı seçin. Ardından veritabanı için Kullanıcı adını ve parolayı belirtin. Oozie meta verileri için bu adımları tekrarlayın.

    Meta depolar için Azure SQL veritabanı 'nı kullanmayla ilgili bazı noktalar şunlardır:
    * Meta veri deposu için kullanılan Azure SQL veritabanı, Azure HDInsight da dahil olmak üzere diğer Azure hizmetleriyle bağlantı kurulmasına izin vermelidir. Azure SQL veritabanı panosunun sağ tarafında sunucu adını seçin. Bu sunucu, SQL veritabanı örneğinin üzerinde çalıştığı bir veritabanıdır. Sunucu Görünümü ' nde olduktan sonra **Yapılandır**' ı seçin. Ardından **Azure hizmetleri**için **Evet**' i seçin. Daha sonra **Kaydet**’e tıklayın.
    * Bir meta veri deposu oluşturduğunuzda bir veritabanını tireler veya kısa çizgilerden adlandırma. Bu karakterler, küme oluşturma işleminin başarısız olmasına neden olabilir.

    ![HDInsight küme depolaması oluştur](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-storage.png "Azure portal yeni bir küme oluşturma")

    > [!WARNING]  
    > HDInsight kümesinden farklı bir konumda ek depolama hesabı kullanılması desteklenmez.

    Sonraki sayfaya geçmek için **İleri ' yi** seçin.

1. **4 uygulamadan (isteğe bağlı)** istediğiniz uygulamaları seçin. Microsoft, bağımsız yazılım satıcıları (ISV) veya bu uygulamaları geliştirebilirsiniz. Daha fazla bilgi için bkz. [küme oluşturma sırasında uygulamaları yüklemeyi](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).

    Sonraki sayfaya geçmek için **İleri ' yi** seçin.

1. **5 küme boyutu** , bu küme için kullanılan düğümlerle ilgili bilgileri görüntüler. Küme için gereken çalışan düğümlerinin sayısını ayarlayın. Küme çalıştırmanın tahmini maliyeti de gösterilir.

    ![HDInsight küme düğümleri oluştur](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-nodes.png "Küme düğümlerinin sayısını belirtin")

   > [!IMPORTANT]  
   > 32 ' den fazla çalışan düğümünü planlıyorsanız en az sekiz çekirdek ve 14 GB RAM içeren bir baş düğüm boyutu seçin. Düğümleri küme oluşturma sırasında veya oluşturulduktan sonra kümeyi ölçeklendirerek planlayın.
   >
   > Düğüm boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz. [Azure HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).

    Sonraki sayfaya geçmek için **İleri ' yi** seçin.

1. **6 betik eylemleriyle**, özel bileşenleri yüklemek için bir kümeyi özelleştirebilirsiniz. Küme oluşturulurken bir kümeyi özelleştirmek için özel bir komut dosyası kullanmak istiyorsanız bu seçenek kullanılır. Betik eylemleri hakkında daha fazla bilgi için bkz. [betik eylemlerini kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md).

   Sonraki sayfaya geçmek için **İleri ' yi** seçin.

1. **7 özetinden**, daha önce girdiğiniz bilgileri doğrulayın. Ardından **Oluştur**’u seçin.

    ![HDInsight küme Özeti oluştur](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-summary.png "Küme düğümlerinin sayısını belirtin")

    > [!NOTE]  
    > Kümenin oluşturulması genellikle yaklaşık 20 dakika sürer. Sağlama işlemini denetlemek için **bildirimleri** izleyin.

1. Oluşturma işlemi bittikten sonra **dağıtım başarılı** bildiriminde **Kaynağa Git** ' i seçin. Küme penceresi aşağıdaki bilgileri sağlar.

    ![HDI Azure portal kümesine genel bakış](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-completed.png "Küme özellikleri")

    Penceredeki simgelerden bazıları şu şekilde açıklanmıştır:

    |Özellik | Açıklama |
    |---|---|
    |Genel Bakış|Kümeyle ilgili tüm gerekli bilgileri sağlar. Örnek olarak ad, ait olduğu kaynak grubu, konum, işletim sistemi ve küme panosunun URL 'SI verilebilir.|
    |Küme panoları|Sizi kümeyle ilişkili olan ambarı portalına yönlendirir.|
    |SSH + küme oturumu açma|SSH kullanarak kümeye erişmek için gereken bilgileri sağlar.|
    |Sil|HDInsight kümesini siler.|

## <a name="customize-clusters"></a>Kümeleri özelleştirme

* [Önyükleme kullanarak HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-bootstrap.md)
* [Betik eylemlerini kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir HDInsight kümesini başarıyla oluşturdunuz. Şimdi kümenizle nasıl çalışacağınızı öğrenin.

### <a name="apache-hadoop-clusters"></a>Apache Hadoop kümeleri

* [HDInsight ile Apache Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase kümeleri

* [HDInsight 'ta Apache HBase ile çalışmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight 'ta Apache HBase için Java uygulamaları geliştirme](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apache Storm kümeleri

* [HDInsight üzerinde Apache Storm için Java topolojileri geliştirme](storm/apache-storm-develop-java-topology.md)
* [HDInsight üzerinde Apache Storm Python bileşenlerini kullanma](storm/apache-storm-develop-python-topology.md)
* [HDInsight üzerinde Apache Storm topolojilerini dağıtma ve izleme](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Apache Spark kümeleri

* [Scala kullanarak tek başına uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak Apache Spark kümesinde işleri uzaktan çalıştırma](spark/apache-spark-livy-rest-interface.md)
* [BI ile Apache Spark: bı araçlarıyla HDInsight 'ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](spark/apache-spark-use-bi-tools.md)
* [Machine Learning Apache Spark: yemek İnceleme sonuçlarını tahmin etmek için HDInsight 'ta Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)
