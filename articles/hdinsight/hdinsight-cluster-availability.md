---
title: 'İzleme: Apache Ambari & Azure Monitör günlükleri - Azure HDInsight'
description: Küme durumunu ve kullanılabilirliğini izlemek için Ambari ve Azure Monitor günlüklerini nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/06/2020
ms.openlocfilehash: 383366fa3e436c79bed28a7c47f1e9daa5f0d9de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060192"
---
# <a name="how-to-monitor-cluster-availability-with-apache-ambari-and-azure-monitor-logs"></a>Apache Ambari ve Azure Monitor günlükleriyle küme kullanılabilirliğini izleme

HDInsight kümeleri, hem bir bakışta sistem durumu bilgileri sağlayan Apache Ambari'yi hem de önceden tanımlanmış uyarıları nyanıtır, sorgulanabilir ölçümler ve günlükler sağlayan Azure Monitor günlükleri tümleştirmesini ve yapılandırılabilir uyarıları içerir.

Bu doküman, kümenizi izlemek için bu araçları nasıl kullanacağınızı gösterir ve ambari uyarısını yapılandırmak, düğüm kullanılabilirlik oranını izlemek ve bir veya daha fazla düğümden sinyal alınmadığında ateş leyen bir Azure Monitöruyarısı oluşturmak için bazı örnekler arasında yürür Beş saat içinde.

## <a name="ambari"></a>Ambari

### <a name="dashboard"></a>Pano

Ambari panosuna, Azure portalında HDInsight Genel Bakış bölümündeki **Ambari** **ana** bağlantısı seçilerek erişilebilir. Alternatif olarak, CLUSTERNAME kümenizin `https://CLUSTERNAME.azurehdinsight.net` adı olan bir tarayıcıda gezinerek erişilebilir.

![HDInsight kaynak portalı görünümü](media/hdinsight-cluster-availability/azure-portal-dashboard-ambari.png)

Daha sonra bir küme giriş kullanıcı adı ve parola istenir. Kümeyi oluşturduğunuzda seçtiğiniz kimlik bilgilerini girin.

Daha sonra, HDInsight kümenizin sağlığına hızlı bir genel bakış sağlamak için bir avuç metrik gösteren widget'lar içeren Ambari panosuna götürülürsünüz. Bu widget'lar, Canlı DataNodes (işçi düğümleri) ve JournalNodes (zookeeper düğümleri), NameDüğümler (kafa düğümleri) çalışma süresi gibi ölçümlerin yanı sıra Kıvılcım ve Hadoop kümeleri için İPN ResourceManager çalışma süresi gibi belirli küme türlerine özgü ölçümleri gösterir.

![Apache Ambari kullanım panosu ekranı](media/hdinsight-cluster-availability/apache-ambari-dashboard.png)

### <a name="hosts--view-individual-node-status"></a>Ana bilgisayarlar – tek tek düğüm durumunu görüntüle

Ayrıca tek tek düğümler için durum bilgilerini görüntüleyebilirsiniz. Kümenizdeki tüm düğümlerin listesini görüntülemek ve her düğüm le ilgili temel bilgileri görmek için **Ana Bilgisayarlar** sekmesini seçin. Her düğüm adının solundaki yeşil onay, tüm bileşenlerin düğümüzerinde olduğunu gösterir. Bir bileşen düğüm üzerinde yse, yeşil onay yerine kırmızı uyarı üçgeni görürsünüz.

![HDInsight Apache Ambari görünümü barındıracak](media/hdinsight-cluster-availability/apache-ambari-hosts1.png)

Daha sonra, belirli bir düğüm için daha ayrıntılı ana bilgisayar ölçümlerini görüntülemek için bir düğümün **adını** seçebilirsiniz. Bu görünüm, her bir bileşenin durumunu/kullanılabilirliğini gösterir.

![Apache Ambari tek düğüm görünümüne ev sahipliği yapıyor](media/hdinsight-cluster-availability/apache-ambari-hosts-node.png)

### <a name="ambari-alerts"></a>Ambari uyarıları

Ambari ayrıca, belirli olayların bildirimini sağlayabilecek birkaç yapılandırılabilir uyarı da sunar. Uyarılar tetiklendiğinde, Ambari'nin sol üst köşesinde uyarı sayısını içeren kırmızı bir rozetle gösterilirler. Bu rozeti seçmek, geçerli uyarıların bir listesini gösterir.

![Apache Ambari güncel uyarıları sayısı](media/hdinsight-cluster-availability/apache-ambari-alerts.png)

Uyarı tanımlarının ve durumlarının listesini görüntülemek için, aşağıda gösterildiği gibi **Uyarılar** sekmesini seçin.

![Ambari tanımları görünüm uyarıları](media/hdinsight-cluster-availability/ambari-alerts-definitions.png)

Ambari, kullanılabilirlik ile ilgili olarak:

| Uyarı Adı                        | Açıklama   |
|---|---|
| DataNode Sağlık Özeti           | Sağlıksız DataNodes varsa bu hizmet düzeyi uyarısı tetiklenir|
| NameNode Yüksek Kullanılabilirlik Sağlık | Etkin NameNode veya Bekleme NameNode çalışmıyorsa bu hizmet düzeyi uyarısı tetiklenir.|
| Yüzde JournalNodes Kullanılabilir    | Kümedeki aşağı JournalNodes sayısı yapılandırılmış kritik eşikten büyükse bu uyarı tetiklenir. JournalNode işlem kontrollerinin sonuçlarını toplar. |
| Mevcut Yüzde DataNodes       | Kümedeki aşağı DataNode sayısı yapılandırılan kritik eşikten büyükse bu uyarı tetiklenir. DataNode işlem kontrollerinin sonuçlarını toplar.|

Bir kümenin kullanılabilirliğini izlemeye yardımcı ambari uyarılarıtam listesi [burada](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-linux#ambari-web-ui)bulunabilir ,

Bir uyarının ayrıntılarını görüntülemek veya ölçütleri değiştirmek için uyarının **adını** seçin. **DataNode Sistem Durumu Özeti'ni** örnek alın. Uyarının açıklamasının yanı sıra 'uyarı' veya 'kritik' uyarıyı tetikleyecek belirli ölçütleri ve ölçütler için denetim aralığını görebilirsiniz. Yapılandırmayı düzenlemek için Yapılandırma kutusunun sağ üst köşesindeki **Düzenleme** düğmesini seçin.

![Apache Ambari uyarı yapılandırması](media/hdinsight-cluster-availability/ambari-alert-configuration.png)

Burada, açıklamayı ve daha da önemlisi, uyarı veya kritik uyarılar için denetim aralığını ve eşikleri edebilirsiniz.

![Ambari uyarı yapılandırmaları görünümü düzenleme](media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png)

Bu örnekte, 2 sağlıksız DataNode'un kritik bir uyarıyı tetikletirdi ve 1 sağlıksız DataNode yalnızca bir uyarıyı tetikleyebilir. Düzenlemeyi bitirdiğinde **Kaydet'i** seçin.

### <a name="email-notifications"></a>E-posta bildirimleri

Ambari uyarıları için isteğe bağlı olarak e-posta bildirimlerini yapılandırabilirsiniz. Bunu yapmak **için, Uyarılar** sekmesinde sol üstteki **Eylemler** düğmesini tıklatın ve ardından **Bildirimleri Yönet' i tıklatın.**

![Ambari bildirimleri yönetme eylemi](media/hdinsight-cluster-availability/ambari-manage-notifications.png)

Uyarı bildirimlerini yönetmek için bir iletişim açılır. **+** İletişim kutusunun altındaki bölümü seçin ve Ambari'ye e-posta göndermek için e-posta sunucusu bilgilerini sağlamak için gerekli alanları doldurun.

> [!TIP]
> Ambari e-posta bildirimlerini ayarlamak, birçok HDInsight kümesini yönetirken uyarıları tek bir yerde almanın iyi bir yolu olabilir.

## <a name="azure-monitor-logs-integration"></a>Azure Monitor tümleştirmegünlüklerini kaydeder

Azure Monitor günlükleri, HDInsight kümeleri gibi birden çok kaynak tarafından oluşturulan verilerin tek bir yerde toplanmasını ve bir araya toplanmasını sağlayarak birleştirilmiş bir izleme deneyimi elde eder.

Ön koşul olarak, toplanan verileri depolamak için bir Log Analytics Çalışma Alanına ihtiyacınız vardır. Henüz bir tane oluşturmadıysanız, yönergeleri buradan takip edebilirsiniz: [Log Analytics Çalışma Alanı oluşturun.](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)

### <a name="enable-hdinsight-azure-monitor-logs-integration"></a>HDInsight Azure Monitör günlükleri tümleştirmesini etkinleştirme

Portaldaki HDInsight küme kaynak sayfasından **Azure Monitör'ü**seçin. Ardından, **etkinleştir'i** seçin ve açılan yerden Log Analytics çalışma alanınızı seçin.

![HDInsight Operasyon Yönetimi Paketi](media/hdinsight-cluster-availability/azure-portal-monitoring.png)

### <a name="query-metrics-and-logs-tables"></a>Sorgu ölçümleri ve günlük tabloları

Azure Monitor günlük tümleştirmesi etkinleştirildikten sonra (bu işlem birkaç dakika sürebilir), **Günlük Analizi Çalışma Alanı** kaynağınıza gidin ve **Günlükler'i**seçin.

![Günlük Analytics çalışma alanı günlükleri](media/hdinsight-cluster-availability/hdinsight-portal-logs.png)

Günlükler, örneğin örnek sorgular, bir dizi liste:

| Sorgu Adı                      | Açıklama                                                               |
|---------------------------------|---------------------------------------------------------------------------|
| Bilgisayarların kullanılabilirliği bugün    | Günlükgönderen bilgisayar sayısını her saat grafik                     |
| Sinyalatlarını listele                 | Son bir saatteki tüm bilgisayar sinyallerini listele                           |
| Her bilgisayarın son kalp atışı | Her bilgisayar tarafından gönderilen son sinyali gösterme                             |
| Kullanılamayan bilgisayarlar           | Son 5 saat içinde kalp atışı göndermeyen bilinen tüm bilgisayarları listele |
| Kullanılabilirlik oranı               | Bağlı her bilgisayarın kullanılabilirlik oranını hesaplama                |

Örnek olarak, yukarıdaki ekran görüntüsünde gösterildiği gibi, bu sorguda **Çalıştır'ı** seçerek **Kullanılabilirlik oranı** örnek sorgusunu çalıştırın. Bu, kümenizdeki her düğümün kullanılabilirlik oranını yüzde olarak gösterir. Aynı Log Analytics çalışma alanına metrik göndermek için birden çok HDInsight kümesini etkinleştirdiyseniz, görüntülenen kümelerde görüntülenen tüm düğümlerin kullanılabilirlik oranını görürsünüz.

![Log Analytics çalışma alanı günlükleri 'kullanılabilirlik oranı' örnek sorgu](media/hdinsight-cluster-availability/portal-availability-rate.png)

> [!NOTE]  
> Kullanılabilirlik oranı 24 saatlik bir süre içinde ölçülür, bu nedenle kümenizin doğru kullanılabilirlik oranlarını görmeden önce en az 24 saat çalışması gerekir.

Sağ üst köşedeki **Pin'i** tıklatarak bu tabloyu paylaşılan bir panoya sabitleyebilirsiniz. Herhangi bir yazılabilir paylaşılan panolarınız yoksa, burada nasıl oluşturulabileceğinizi görebilirsiniz: [Azure portalında pano oluşturma ve paylaşma.](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards#publish-and-share-a-dashboard)

### <a name="azure-monitor-alerts"></a>Azure Monitör uyarıları

Bir metnin değeri veya sorgu sonuçları belirli koşulları karşıladığında tetikleyecek Azure Monitor uyarıları da ayarlayabilirsiniz. Örnek olarak, bir veya daha fazla düğüm 5 saat içinde sinyal göndermediğinde (yani kullanılamadığı varsayıldığında) e-posta göndermek için bir uyarı oluşturalım.

**Günlükler'den,** aşağıda gösterildiği gibi, bu sorguda **Çalıştır'ı** seçerek **kullanılamayan bilgisayarlar** örnek sorgusunu çalıştırın.

![Log Analytics çalışma alanı günlükleri 'kullanılamayan bilgisayarlar' örneği](media/hdinsight-cluster-availability/portal-unavailable-computers.png)

Tüm düğümler kullanılabilirse, bu sorgu şimdilik sıfır sonuç döndürmelidir. Bu sorgu için uyarınızı yapılandırmaya başlamak için **Yeni uyarı kuralını** tıklatın.

![Log Analytics çalışma alanı yeni uyarı kuralı](media/hdinsight-cluster-availability/portal-logs-new-alert-rule.png)

Bir uyarının üç bileşeni vardır: kuralı oluşturmak için *kaynak* (bu durumda Log Analytics çalışma alanı), uyarıyı tetikleme *koşulu* ve uyarı tetiklendiğinde ne olacağını belirleyen *eylem grupları.*
Sinyal mantığını yapılandırmayı bitirmek için aşağıda gösterildiği gibi **koşul başlığına**tıklayın.

![Portal uyarısı kural koşulu oluşturmak](media/hdinsight-cluster-availability/portal-condition-title.png)

Bu, **Yapılaşı sinyal mantığını**açar.

Uyarı **mantığı** bölümünü aşağıdaki gibi ayarlayın:

*Based: Sonuç sayısı, Koşul: Büyük, Eşik: 0.*

Bu sorgu yalnızca kullanılamaz düğümleri sonuç olarak döndürdüklerinden, sonuç sayısı 0'dan büyükse, uyarı nın yanması gerekir.

Değerlendirme **bölümüne göre,** kullanılabilir olmayan düğümleri denetlemek istediğiniz sıklıkla **dönemi** ve **sıklığı** ayarlayın.

Bu uyarının amacı **için, Period=Frequency'den** emin olmak istersiniz. Dönem, frekans ve diğer uyarı parametreleri hakkında daha fazla bilgiyi [burada](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log#log-search-alert-rule---definition-and-types)bulabilirsiniz.

Sinyal mantığını yapılandırmayı bitirdiğinizde **Bitti'yi** seçin.

![Uyarı kuralı sinyal mantığını yapılandırır](media/hdinsight-cluster-availability/portal-configure-signal-logic.png)

Zaten varolan bir eylem grubunuz yoksa, Eylem **Grupları** bölümünün altında **Yeni Oluştur'u** tıklatın.

![Uyarı kuralı yeni eylem grubu oluşturur](media/hdinsight-cluster-availability/portal-create-new-action-group.png)

Bu, **eylem grubu ekle'yi**açar. Bir **Eylem grubu adı,** **Kısa ad,** **Abonelik**ve Kaynak **grubu seçin.** **Eylemler** bölümünde bir **Eylem Adı** seçin ve Eylem Türü olarak **E-posta/SMS/Push/Voice'u** **seçin.**

> [!NOTE]
> Azure İşlevi, LogicApp, Webhook, ITSM ve Otomasyon Runbook gibi bir uyarının E-posta/SMS/Push/Voice dışında tetikleyebileceği başka eylemler de vardır. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#action-specific-information)

Bu **E-posta / SMS / Push / Ses**açılacaktır. Alıcı için bir **Ad** seçin, **E-posta** kutusunu **işaretleyin** ve uyarının gönderilmesini istediğiniz bir e-posta adresi yazın. Eylem grubunuzu yapılandırmayı bitirmek için **E-posta/SMS/Push/Voice'da**Tamam'ı, ardından **eylem grubunu ekle'yi** seçin. **OK**

![Uyarı kuralı eylem grubu ekleme oluşturur](media/hdinsight-cluster-availability/portal-add-action-group.png)

Bu bıçaklar kapandıktan sonra **Eylem Grupları** bölümünün altında eylem grubunuzu görmeniz gerekir. Son olarak, **Uyarı Kuralı Adı** ve **Açıklaması** yazarak ve **önem derecesi**seçerek Uyarı **Ayrıntıları** bölümünü tamamlayın. Bitirmek için **Uyarı Kuralı Oluştur'u** tıklatın.

![Portal uyarı kuralı bitiş oluşturur](media/hdinsight-cluster-availability/portal-create-alert-rule-finish.png)

> [!TIP]
> **Önem Derecesi** belirtme yeteneği, birden çok uyarı oluştururken kullanılabilecek güçlü bir araçtır. Örneğin, tek bir baş düğümü inerse Uyarı yükseltmek için bir uyarı (Sev 1) ve her iki baş düğümleri aşağı gitmek olası durumda Kritik (Sev 0) yükseltir başka bir uyarı oluşturabilirsiniz.

Bu uyarının koşulu karşılandığında, uyarı çalışacak ve aşağıdaki gibi uyarı ayrıntılarını içeren bir e-posta alırsınız:

![Azure Monitor uyarı e-posta örneği](media/hdinsight-cluster-availability/portal-oms-alert-email.png)

Ayrıca, **Günlük Analizi Çalışma Alanınızda** **Uyarılar'a** giderek önem derecesine göre gruplanmış olan tüm uyarıları görüntüleyebilirsiniz.

![Log Analytics çalışma alanı uyarıları](media/hdinsight-cluster-availability/hdi-portal-oms-alerts.png)

Önem derecesi gruplandırmasını seçmek (örneğin, yukarıda vurgulandığı gibi **Sev 1)** aşağıdaki gibi ateş eden bu önem derecesine ilişkin tüm uyarıların kayıtlarını gösterir:

![Log Analytics çalışma alanı sev bir uyarı](media/hdinsight-cluster-availability/portal-oms-alerts-sev1.png)

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight'ta Apache Hadoop kümelerinin kullanılabilirliği ve güvenilirliği](hdinsight-high-availability-linux.md)
