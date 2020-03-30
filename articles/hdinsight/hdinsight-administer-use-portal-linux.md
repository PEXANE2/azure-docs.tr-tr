---
title: Azure portalını kullanarak HDInsight'ta Apache Hadoop kümelerini yönetme
description: Azure portalını kullanarak Azure HDInsight kümelerini nasıl oluşturup yöneteceklerini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/12/2020
ms.openlocfilehash: b9d923b3272f9d8b3da39d7cdb771a766eee4eab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272740"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Azure portalını kullanarak HDInsight'ta Apache Hadoop kümelerini yönetme

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure [portalını](https://portal.azure.com)kullanarak Azure HDInsight'ta [Apache Hadoop](https://hadoop.apache.org/) kümelerini yönetebilirsiniz. Diğer araçları kullanarak HDInsight'taki Hadoop kümelerini yönetme hakkında bilgi edinmek için yukarıdaki sekme seçiciyi kullanın.

## <a name="prerequisites"></a>Ön koşullar

HDInsight'ta varolan bir Apache Hadoop kümesi.  Azure [portalını kullanarak HDInsight'ta Linux tabanlı kümeler oluşturma'ya](hdinsight-hadoop-create-linux-clusters-portal.md)bakın.

## <a name="getting-started"></a>Başlarken

Oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="list-and-show-clusters"></a><a name="showClusters"></a>Kümeleri listele ve göster

**HDInsight kümeleri** sayfası varolan kümelerinizi listeler.  Portaldan:
1. Sol menüden **Tüm hizmetleri** seçin.
2. **ANALYTICS**altında **HDInsight kümelerini** seçin.

## <a name="cluster-home-page"></a><a name="homePage"></a>Ana sayfayı kümele

[**HDInsight kümeleri**](#showClusters) sayfasından küme adınızı seçin.  Bu, aşağıdaki resme benzeyen **Genel Bakış** görünümünü açar:

![Azure portalı HDInsight küme temelleri](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials2.png)

**Üst menü:**  

| Öğe| Açıklama |
|---|---|
|Taşı|Kümeyi başka bir kaynak grubuna veya başka bir aboneye taşır.|
|Sil|Kümeyi siler. |
|Yenile|Görünümü yeniler.|

**Sol menü:**  

  - **Sol üst menü**

    | Öğe| Açıklama |
    |---|---|
    |Genel Bakış|Kümeniz için genel bilgiler sağlar.|
    |Etkinlik günlüğü|Etkinlik günlüklerini göster ve sorgula.|
    |Erişim denetimi (IAM)|Rol atamaları kullanın.  Bkz. [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamaları](../role-based-access-control/role-assignments-portal.md)kullanın.|
    |Etiketler|Bulut hizmetlerinizin özel taksonomisini tanımlamak için anahtar/değer çiftleri ayarlamanızı sağlar. Örneğin, proje adında bir anahtar oluşturabilir ve ardından belirli bir **projeyle**ilişkili tüm hizmetler için ortak bir değer kullanabilirsiniz.|
    |Sorunları tanılayın ve çözün|Sorun giderme bilgilerini görüntüleyin.|
    |Hızlı Başlangıç|HDInsight'ı kullanmaya başlamanıza yardımcı olan bilgileri görüntüler.|
    |Araçlar|HDInsight ile ilgili araçlar için yardım bilgileri.|

  - **Ayarlar menüsü**  

    | Öğe| Açıklama |
    |---|---|
    |Küme boyutu|Küme altdüğüm düğümlerinin sayısını denetleyin, artırın ve azaltın. Bkz. [Ölçek kümeleri.](hdinsight-administer-use-portal-linux.md#scale-clusters)|
    |Kota sınırları|Aboneliğiniz için kullanılan ve kullanılabilir çekirdekleri görüntüleyin.|
    |SSH + Cluster giriş|Güvenli Kabuk (SSH) bağlantısını kullanarak kümeye bağlanma yönergelerini gösterir. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).|
    |Data Lake Storage Gen1|Erişim Veri Gölü Depolama Gen1 yapılandırılması.  [Bkz. Hızlı Başlangıç: HDInsight'ta kümeler ayarlayın.](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)|
    |Depolama hesapları|Depolama hesaplarını ve anahtarları görüntüleyin. Depolama hesapları küme oluşturma işlemi sırasında yapılandırılır.|
    |Uygulamalar|HDInsight uygulamalarını ekleme/kaldırma.  Bkz. [Özel HDInsight uygulamalarını yükle.](hdinsight-apps-install-custom-applications.md)|
    |Komut dosyası eylemleri|Kümeüzerinde Bash komut dosyalarını çalıştırın. [Bkz. Script Action'ı kullanarak Linux tabanlı HDInsight kümelerini özelleştirin.](hdinsight-hadoop-customize-cluster-linux.md)|
    |Dış metastore'lar|[Apache Hive](https://hive.apache.org/) ve [Apache Oozie](https://oozie.apache.org/) metastore'larını görüntüleyin. Metamağazalar yalnızca küme oluşturma işlemi sırasında yapılandırılabilir.|
    |HDInsight iş ortağı|Geçerli HDInsight İş Ortağını ekleyin/kaldırın.|
    |Özellikler|Küme [özelliklerini](#properties)görüntüleyin.|
    |Kilitler|Kümenin değiştirilmesini veya silinmesini önlemek için bir kilit ekleyin.|
    |Şablonu dışarı aktarma|Küme için Azure Kaynak Yöneticisi şablonu görüntüleyin ve dışa aktarın. Şu anda yalnızca bağımlı Azure depolama hesabını dışa aktarabilirsiniz. Azure [Kaynak Yöneticisi şablonlarını kullanarak HDInsight'ta Linux tabanlı Apache Hadoop kümeleri oluşturun'a](hdinsight-hadoop-create-linux-clusters-arm-templates.md)bakın.|

  - **İzleme menüsü**

    | Öğe| Açıklama |
    |---|---|
    |Uyarılar|Uyarıları ve eylemleri yönetin.|
    |Ölçümler|Azure Monitor günlüklerinde küme ölçümlerini izleyin.|
    |Tanı ayarları|Tanı ölçümlerinin nerede depolandığına ilişkin ayarlar.|
    |Azure İzleyici|Cluster'ınızı Azure Monitor'da izleyin.|

  - **Destek + sorun giderme menüsü**

    | Öğe| Açıklama |
    |---|---|
    |Kaynak durumu|Bkz. [Azure kaynak durumu genel bakışı.](../service-health/resource-health-overview.md)|
    |Yeni destek isteği|Microsoft desteği ile bir destek bileti oluşturmanıza olanak tanır.|

## <a name="cluster-properties"></a><a name="properties"></a>Küme Özellikleri

Küme [ana sayfasından](#homePage) **Ayarlar'ın** altında **Özellikler'i**seçin.

|Öğe | Açıklama |
|---|---|
|Hostname|Küme adı.|
|KÜME URL|Ambari web arayüzü için URL.|
|Özel Uç Nokta|Küme için özel bitiş noktası.|
|Güvenli kabuk (SSH)|SSH üzerinden kümeye erişirken kullanılacak kullanıcı adı ve ana bilgisayar adı.|
|DURUM|Bunlardan biri: İptal, Kabul Edilen, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, Operational, Running, Error, Silme, Silinmiş, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, Yeniden Boyutlandı veya ClusterCustomization.|
|BÖLGE|Azure konumu. Desteklenen Azure konumlarının listesi için [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/)için **Bölge** açılır liste kutusuna bakın.|
|OLUŞTURMA TARİHİ|Kümenin dağıtılan tarih.|
|İşletim Sistemi|Ya **Windows** veya **Linux**.|
|TÜR|Hadoop, HBase, Fırtına, Kıvılcım.|
|Sürüm|[HDInsight sürümlerine](hdinsight-component-versioning.md)bakın.|
|Minimum TLS sürümü|TLS sürümü.|
|ABONELİK|Abonelik adı.|
|VARSAYıLAN VERI Kaynağı|Varsayılan küme dosya sistemi.|
|İşçi düğümleri boyutları|İşçi düğümlerinin seçili VM boyutu.|
|Baş düğüm boyutu|Kafa düğümlerinin seçili VM boyutu.|
|Sanal ağ|Dağıtım sırasında seçiliyse, kümenin dağıtıldığı Sanal Ağ'ın adı.|

## <a name="move-clusters"></a>Kümeleri taşıma

BIR HDInsight kümesini başka bir Azure kaynak grubuna veya başka bir aboneye taşıyabilirsiniz.

Küme [ana sayfasından:](#homePage)

1. Üst menüden **Taşı'yı** seçin.
2. **Başka bir kaynak grubuna taşı** veya başka bir **aboneye taşı'nı**seçin.
3. Yeni sayfadaki yönergeleri izleyin.

## <a name="delete-clusters"></a>Kümeleri silme

Kümesilme, varsayılan depolama hesabını veya bağlantılı depolama hesaplarını silmez. Aynı depolama hesaplarını ve aynı meta depoları kullanarak kümeyi yeniden oluşturabilirsiniz. Kümeyi yeniden oluştururken yeni bir varsayılan Blob kapsayıcısı kullanmanızı öneririz.

Küme [ana sayfasından:](#homePage)

1. Üst menüden **Sil'i** seçin.
2. Yeni sayfadaki yönergeleri izleyin.

[Ayrıca](#pauseshut-down-clusters)bkz.

## <a name="add-additional-storage-accounts"></a>Başka depolama hesapları ekleme

Küme oluşturulduktan sonra ek Azure Depolama hesapları ve Azure Veri Gölü Depolama hesapları ekleyebilirsiniz. Daha fazla bilgi için bkz. [HDInsight’a başka depolama hesapları ekleme](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Ölçek kümeleri

Küme ölçekleme özelliği, bir Azure HDInsight kümesi tarafından kullanılan alt düğüm sayısını, kümeyi yeniden oluşturmak zorunda kalmadan değiştirmenize olanak tanır.

Tam bilgi için [HDInsight kümelerini](./hdinsight-scaling-best-practices.md) ölçeklendir'e bakın.

## <a name="pauseshut-down-clusters"></a>Kümeleri duraklatma/kapatma

Hadoop işlerinin çoğu sadece ara sıra çalıştırılabilen toplu işlerdir. Çoğu Hadoop kümesi için, kümenin işleme için kullanılmadığı büyük zaman dilimleri vardır. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz.
Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

İşlemi programlamanın birçok yolu vardır:

- Kullanıcı Azure Veri Fabrikası. İsteğe bağlı HDInsight bağlantılı hizmetler oluşturmak için [Azure Veri Fabrikası'nı kullanarak HDInsight'ta isteğe bağlı Linux tabanlı Apache Hadoop kümeleri](hdinsight-hadoop-create-linux-clusters-adf.md) oluşturun'a bakın.
- Azure PowerShell'i kullanın.  Bkz. [Uçuş gecikme verilerini analiz edin.](./interactive-query/interactive-query-tutorial-analyze-flight-data.md)
- Azure CLI'yı kullanın. Bkz. [Azure CLI kullanarak Azure HDInsight kümelerini yönet.](hdinsight-administer-use-command-line.md)
- HDInsight .NET SDK'yı kullanın. Bkz. [Apache Hadoop işlerini gönder.](hadoop/submit-apache-hadoop-jobs-programmatically.md)

Fiyatlandırma bilgileri için [HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/)fiyatlandırması'na bakın. Portaldan bir kümeyi silmek için [bkz.](#delete-clusters)

## <a name="upgrade-clusters"></a>Yükseltme kümeleri

HdInsight [kümesini daha yeni bir sürüme yükseltin'](./hdinsight-upgrade-cluster.md)e bakın.

## <a name="open-the-apache-ambari-web-ui"></a>Apache Ambari web UI'yi açın

Ambari, RESTful API'leri ile desteklenen sezgisel, kullanımı kolay Bir Hadoop yönetim web UI sağlar. Ambari, sistem yöneticilerinin Hadoop kümelerini yönetmesini ve izlemesini sağlar.

Küme [ana sayfasından:](#homePage)

1. **Küme panolarını**seçin.

    ![HDInsight Apache Hadoop küme menüsü](./media/hdinsight-administer-use-portal-linux/hdinsight-azure-portal-cluster-menu2.png)

1. Yeni sayfadan **Ambari'yi ana sayfaseçin.**
1. Küme kullanıcı adı ve parolasını girin.  Varsayılan küme kullanıcı adı _yöneticidir._

Daha fazla bilgi [için, Apache Ambari Web UI'yi kullanarak HDInsight kümelerini yönet'e](hdinsight-hadoop-manage-ambari.md)bakın.

## <a name="change-passwords"></a>Parolaları değiştirme

BIR HDInsight kümesinin iki kullanıcı hesabı olabilir. Oluşturma işlemi sırasında HDInsight küme kullanıcı hesabı (HTTP kullanıcı hesabı) ve SSH kullanıcı hesabı oluşturulur. Portalı küme kullanıcı hesabı parolasını değiştirmek için ve SSH kullanıcı hesabını değiştirmek için komut dosyası eylemlerini kullanabilirsiniz.

### <a name="change-the-cluster-user-password"></a>Küme kullanıcı parolasını değiştirme

> [!NOTE]  
> Küme kullanıcı (yönetici) parolasını değiştirmek, bu kümeye karşı çalışan komut dosyası eylemlerinin başarısız lığa neden olabilir. Alt düğümleri hedefleyen kalıcı komut dosyası eylemleriniz varsa, yeniden boyutlandırma işlemleri yoluyla kümeye düğüm ler eklediğinizde bu komut dosyaları başarısız olabilir. Komut dosyası eylemleri hakkında daha fazla bilgi için, [komut dosyası eylemlerini kullanarak HDInsight kümelerini özelleştir'e](hdinsight-hadoop-customize-cluster-linux.md)bakın.

Küme [ana sayfasından:](#homePage)
1. **Ayarlar**altında **SSH + Cluster girişi'ni** seçin.
2. **Kimlik bilgisi sıfırla'yı**seçin.
3. Metin kutularına yeni parola girin ve onaylayın.
4. **Tamam'ı**seçin.

Parola kümedeki tüm düğümlerde değiştirilir.

### <a name="change-the-ssh-user-password"></a>SSH kullanıcı parolasını değiştirme

1. Metin düzenleyicisi kullanarak, aşağıdaki metni **changepassword.sh**adlı bir dosya olarak kaydedin.

    > [!IMPORTANT]  
    > Satır bitişi olarak LF kullanan bir düzenleyici kullanmanız gerekir. Düzenleyici CRLF kullanıyorsa, komut dosyası çalışmıyor.

    ```bash
    #! /bin/bash
    USER=$1
    PASS=$2
    usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
    ```

2. Dosyayı, HTTP veya HTTPS adresini kullanarak HDInsight'tan erişilebilen bir depolama konumuna yükleyin. Örneğin, OneDrive veya Azure Blob depolama alanı gibi genel bir dosya deposu. Bir sonraki adımda bu URI gerektiğinden URI'yi (HTTP veya HTTPS adresi) dosyaya kaydedin.
3. Küme [ana sayfasından](#homePage) **Ayarlar**altında **Komut Dosyası eylemlerini** seçin.
4. Komut **Dosyası eylemleri** sayfasından **yeni gönder'i**seçin.
5. Komut dosyası eylem in **sayfasından** aşağıdaki bilgileri girin:

   | Alan | Değer |
   | --- | --- |
   | Komut dosyası türü | Seçin - Açılan listeden **özel.**|
   | Adı |"ssh şifreni değiştir" |
   | Bash script URI |changepassword.sh dosyasına URI |
   | Düğüm türü(ler): (Baş, İşçi, Nimbus, Süpervizör veya Zookeeper.) |✓ listelenen tüm düğüm türleri için |
   | Parametreler |SSH kullanıcı adını ve ardından yeni parolayı girin. Kullanıcı adı ve parola arasında bir boşluk olmalıdır. |
   | Bu komut dosyası eylem devam ... |Bu alanı işaretsiz bırakın. |

6. Komut dosyasını uygulamak için **Oluştur'u** seçin. Komut dosyası tamamlandığında, yeni parolayla SSH kullanarak kümeye bağlanabilirsiniz.

## <a name="find-the-subscription-id"></a>Abonelik kimliğini bulma

Her küme bir Azure aboneliğine bağlıdır.  Azure abonelik kimliği küme [giriş sayfasından](#homePage)görülebilir.

## <a name="find-the-resource-group"></a>Kaynak grubunu bulma

Azure Kaynak Yöneticisi modunda, her HDInsight kümesi bir Azure Kaynak Yöneticisi grubuyla oluşturulur. Kaynak Yöneticisi grubu küme [ana sayfasından](#homePage)görünür.

## <a name="find-the-storage-accounts"></a>Depolama hesaplarını bulma

HDInsight kümeleri, verileri depolamak için bir Azure Depolama hesabı veya Azure Veri Gölü Depolaması kullanır. Her HDInsight kümesinde bir varsayılan depolama hesabı ve bir dizi bağlantılı depolama hesabı olabilir. **Ayarlar**altındaki küme ana [sayfasından](#homePage) depolama hesaplarını listelemek için **Depolama hesaplarını**seçin.

## <a name="monitor-jobs"></a>İşleri izleme

Bkz. [Apache Ambari Web UI'yi kullanarak HDInsight kümelerini yönetin.](hdinsight-hadoop-manage-ambari.md#monitoring)

## <a name="cluster-size"></a>Küme boyutu

[Küme ana sayfasındaki Küme](#homePage) **boyutu** döşemesi, bu kümeye ayrılan çekirdek sayısını ve bu kümedeki düğümler için nasıl ayrıldıklarını görüntüler.

> [!IMPORTANT]  
> HDInsight kümesi tarafından sağlanan hizmetleri izlemek için Ambari Web veya Ambari REST API'yi kullanmanız gerekir. Ambari'yi kullanma hakkında daha fazla bilgi için [Apache Ambari kullanarak HDInsight kümelerini yönet'e](hdinsight-hadoop-manage-ambari.md) bakın

## <a name="connect-to-a-cluster"></a>Kümeye bağlanma

- [HDInsight ile Apache Hive'ı kullanma](hadoop/apache-hadoop-use-hive-ambari-view.md)
- [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bazı temel yönetim işlevleri öğrendim. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure PowerShell kullanarak HDInsight'ı yönetin](hdinsight-administer-use-powershell.md)
- [Azure CLI'yi kullanarak HDInsight'ı yönetin](hdinsight-administer-use-command-line.md)
- [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)
- [Apache Ambari REST API'nin kullanımı ile ilgili ayrıntılar](hdinsight-hadoop-manage-ambari-rest-api.md)
- [HDInsight'ta Apache Hive'ı kullanma](hadoop/hdinsight-use-hive.md)
- [HDInsight'ta Apache Sqoop'u kullanın](hadoop/hdinsight-use-sqoop.md)
- [HDInsight'ta Apache Hive ve Apache Pig ile Python Kullanıcı Tanımlı Fonksiyonlar (UDF) kullanın](hadoop/python-udf-hdinsight.md)
- [Azure HDInsight'ta Apache Hadoop'un hangi sürümü var?](hdinsight-component-versioning.md)
