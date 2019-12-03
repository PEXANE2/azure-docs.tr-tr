---
title: 'İzleme: Apache ambarı & Azure Izleyici günlükleri-Azure HDInsight'
description: Küme durumunu ve kullanılabilirliğini izlemek için ambarı ve Azure Izleyici günlüklerini nasıl kullanacağınızı öğrenin.
keywords: izleme, ambarı, izleyici, Log Analytics, uyarı, kullanılabilirlik, sistem durumu
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/25/2019
ms.openlocfilehash: a21610fefcfe1632dffbfd8e055497476f7e59c1
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74687813"
---
# <a name="how-to-monitor-cluster-availability-with-apache-ambari-and-azure-monitor-logs"></a>Apache ambarı ve Azure Izleyici günlükleri ile küme kullanılabilirliğini izleme

HDInsight kümeleri hem bir bakışta, hem de önceden tanımlanmış uyarılarda sağlık bilgileri sağlayan ve ayrıca, sorgulanabilir ölçümler ve Günlükler ve yapılandırılabilir uyarılar sağlayan Azure Izleyici günlükleri tümleştirmesinin yanı sıra hem Apache ambarı içerir.

Bu belge, kümenizi izlemek ve bir veya daha fazla düğümden sinyal alınmadığında harekete geçen bir Azure Izleyici uyarısı oluşturmak için bu araçların nasıl kullanılacağını gösterir. beş saat içinde.

## <a name="ambari"></a>Ambari

### <a name="dashboard"></a>Pano

**Ambarı panosuna** aşağıda gösterildiği gibi, HDInsight genel bakış konusunun **küme panoları** bölümünde bulunan Azure Portal. Alternatif olarak, CLUSTERNAME 'in Kümenizin adı olduğu bir tarayıcıda `https://CLUSTERNAME.azurehdinsight.net` gezilerek erişilebilir.

![HDInsight kaynak portalı görünümü](media/hdinsight-cluster-availability/azure-portal-dashboard-ambari.png)

Daha sonra bir küme oturum açma Kullanıcı adı ve parolası istenir. Kümeyi oluştururken seçtiğiniz kimlik bilgilerini girin.

Daha sonra, HDInsight kümenizin sistem durumuna hızlı bir genel bakış sunmak için bir dizi ölçümü gösteren pencere öğeleri içeren, daha sonra bir ambarı panosuna götürülürsünüz. Bu pencere öğeleri, Spark ve Hadoop kümeleri için YARN ResourceManager çalışma süresi gibi belirli küme türlerine özgü ölçümler ve canlı düğüm sayısı (çalışan düğümleri) ve JournalNodes (Zookeeper node), Üçlü iş çalışma süresi (örneğin, çok sayıda) gibi ölçümleri gösterir.

![Apache ambarı kullanım panosu görüntüleme](media/hdinsight-cluster-availability/apache-ambari-dashboard.png)

### <a name="hosts--view-individual-node-status"></a>Konaklar – tek tek düğüm durumunu görüntüle

Tek tek düğümlerin durum bilgilerini de görüntüleyebilirsiniz. Kümenizdeki tüm düğümlerin listesini görüntülemek için **konaklar** sekmesini seçin ve her düğüm hakkında temel bilgileri görüntüleyin. Her düğüm adının solunda yeşil onay işareti, tüm bileşenlerin düğüm üzerinde olduğunu gösterir. Bir bileşen düğüm üzerinde kapalıysa yeşil onay yerine kırmızı bir uyarı üçgeni görürsünüz.

![HDInsight Apache ambarı Konakları görünümü](media/hdinsight-cluster-availability/apache-ambari-hosts1.png)

Daha sonra söz konusu düğüme ait daha ayrıntılı konak ölçümlerini görüntülemek için bir düğüm **adı** ' nı seçebilirsiniz. Bu görünüm her bir bileşenin durumunu/kullanılabilirliğini gösterir.

![Apache ambarı tek düğümlü görünüm barındırır](media/hdinsight-cluster-availability/apache-ambari-hosts-node.png)

### <a name="ambari-alerts"></a>Ambarı uyarıları

Ayrıca, belirli olaylara yönelik bildirim sağlayabilen birkaç yapılandırılabilir uyarı da sunar. Uyarılar tetiklendiğinde, bu uyarılar, bu uyarı sayısını içeren kırmızı bir rozet içinde, ambarı 'nın sol üst köşesinde gösterilir. Bu rozet seçildiğinde geçerli uyarıların bir listesi gösterilir.

![Apache ambarı geçerli uyarı sayısı](media/hdinsight-cluster-availability/apache-ambari-alerts.png)

Uyarı tanımlarının ve durumlarının listesini görüntülemek için, aşağıda gösterildiği gibi **Uyarılar** sekmesini seçin.

![Ambarı uyarı tanımları görünümü](media/hdinsight-cluster-availability/ambari-alerts-definitions.png)

Ambarı aşağıdakiler dahil olmak üzere kullanılabilirliğiyle ilgili çok sayıda önceden tanımlı uyarı sunar:

| Uyarı adı                        | Açıklama   |
|---|---|
| Dadtanode sistem durumu Özeti           | Sağlıksız bir kades varsa, bu hizmet düzeyi uyarı tetiklenir|
| Süs Yot yüksek kullanılabilirlik durumu | Bu hizmet düzeyi uyarı, etkin bir süs Code veya standby süs ODE çalışmıyorsa tetiklenir.|
| Kullanılabilir JournalNodes yüzdesi    | Bu uyarı, kümedeki aşağı yönelik JournalNodes sayısı yapılandırılan kritik eşikten fazlaysa tetiklenir. Bu, JournalNode işlem denetimlerinin sonuçlarını toplar. |
| Kullanılabilir gün yüzdesi       | Bu uyarı, kümedeki aşağı doğru eşik sayısı yapılandırılan kritik eşikten büyükse tetiklenir. Bu, Davtanode işlem denetimlerinin sonuçlarını toplar.|

Bir kümenin kullanılabilirliğini izlemeye yardımcı olan bir dizi uyarı listesinin tam listesi [burada](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-linux#ambari-web-ui)bulunabilir.

Bir uyarının veya değişiklik ölçütlerinin ayrıntılarını görüntülemek için uyarının **adını** seçin. Örnek olarak, **Davode sistem durumu özetini** alın. Bir ' uyarı ' veya ' kritik ' uyarı ve kriterlerin denetim aralığı tetiklenecek belirli ölçütlere ek olarak uyarının açıklamasını görebilirsiniz. Yapılandırmayı düzenlemek için yapılandırma kutusunun sağ üst köşesindeki **Düzenle** düğmesini seçin.

![Apache ambarı uyarı yapılandırması](media/hdinsight-cluster-availability/ambari-alert-configuration.png)

Burada, açıklamayı ve uyarı ya da kritik uyarılar için denetim aralığını ve eşikleri düzenleyebilirsiniz.

![Ambarı uyarı yapılandırması düzenleme görünümü](media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png)

Bu örnekte, önemli bir uyarı tetikleyebilmeniz ve 1 sağlıksız bir ıtanode yalnızca uyarı tetikleyebilmeniz gerekir. Düzenlemeden sonra **Kaydet** ' i seçin.

### <a name="email-notifications"></a>E-posta bildirimleri

Ayrıca, isteğe bağlı olarak, ambarı uyarıları için e-posta bildirimleri yapılandırabilirsiniz. Bunu yapmak için, **Uyarılar** sekmesinde, sol üst köşedeki **Eylemler** düğmesine ve ardından **Bildirimleri Yönet** ' e tıklayın.

![Ambarı yönetimi bildirimleri eylemi](media/hdinsight-cluster-availability/ambari-manage-notifications.png)

Uyarı bildirimlerini yönetmek için bir iletişim kutusu açılır. İletişim kutusunun alt kısmındaki **+** seçin ve e-postaların gönderileceği e-posta sunucusu ayrıntılarını kullanarak, ambarı sağlamak için gerekli alanları doldurun.

> [!TIP]
> Ambarı e-posta bildirimlerinin kurulması, birçok HDInsight kümesini yönetirken tek bir yerde uyarı almanın iyi bir yoludur.

## <a name="azure-monitor-logs-integration"></a>Azure Izleyici günlük tümleştirmesi

Azure Izleyici günlükleri, HDInsight kümeleri gibi birden çok kaynak tarafından oluşturulan verilerin, birleştirilmiş bir izleme deneyimi elde etmek için tek bir yerde toplanmasını ve toplanmasını sağlar.

Bir önkoşul olarak, toplanan verileri depolamak için bir Log Analytics çalışma alanına ihtiyacınız olacaktır. Henüz bir tane oluşturmadıysanız, buradaki yönergeleri izleyebilirsiniz: [Log Analytics çalışma alanı oluşturun](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

### <a name="enable-hdinsight-azure-monitor-logs-integration"></a>HDInsight Azure Izleyici günlükleri tümleştirmesini etkinleştirme

Portaldaki HDInsight küme kaynağı sayfasında, **Operations Management Suite**' i seçin. Ardından, **Etkinleştir** ' i seçin ve açılan listeden Log Analytics çalışma alanınızı seçin.

![HDInsight Operations Management Suite](media/hdinsight-cluster-availability/hdi-portal-oms-enable.png)

### <a name="query-metrics-and-logs-tables"></a>Sorgu ölçümleri ve günlük tabloları

Azure Izleyici günlük tümleştirmesi etkinleştirildikten sonra (Bu işlem birkaç dakika sürebilir) **Log Analytics çalışma alanı** kaynağına gidin ve **Günlükler**' i seçin.

![Log Analytics çalışma alanı günlükleri](media/hdinsight-cluster-availability/hdinsight-portal-logs.png)

Günlükler bir dizi örnek sorgu listeler, örneğin:

| Sorgu adı                      | Açıklama                                                               |
|---------------------------------|---------------------------------------------------------------------------|
| Günümüzde kullanılabilirlik bilgisayarları    | Günlük gönderen bilgisayarların sayısını, her saat                     |
| Sinyalleri Listele                 | Son saatin tüm bilgisayar sinyalleriyle listeleme                           |
| Her bilgisayarın son sinyali | Her bilgisayar tarafından gönderilen son sinyali gösterme                             |
| Kullanılamayan bilgisayarlar           | Son 5 saat içinde sinyal göndermediği bilinen tüm bilgisayarları Listele |
| Kullanılabilirlik oranı               | Bağlı her bilgisayarın kullanılabilirlik oranını hesapla                |

Örnek olarak, yukarıdaki ekran görüntüsünde gösterildiği gibi bu sorguda **Çalıştır** ' ı seçerek **kullanılabilirlik oranı** örnek sorgusunu çalıştırın. Bu, kümenizde her bir düğümün kullanılabilirlik oranını yüzde olarak gösterir. Aynı Log Analytics çalışma alanına ölçümleri göndermek için birden çok HDInsight kümesi etkinleştirdiyseniz, bu kümelerdeki tüm düğümlerin kullanılabilirlik oranını görürsünüz.

![Log Analytics çalışma alanı günlüklerinin kullanılabilirlik oranı ' örnek sorgu](media/hdinsight-cluster-availability/portal-availability-rate.png)

> [!NOTE]  
> Kullanılabilirlik oranı, 24 saatlik bir dönemde ölçülür, bu sayede doğru kullanılabilirlik ücretleri görüntülenmeden önce kümenizin en az 24 saat boyunca çalışması gerekir.

Sağ üst köşedeki **sabitle** ' ye tıklayarak bu tabloyu paylaşılan bir panoya sabitleyebilirsiniz. Yazılabilir bir paylaşılan panonuz yoksa, nasıl bir tane oluşturabileceğiniz hakkında bilgi edinebilirsiniz: [Azure Portal panoları oluşturma ve paylaşma](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards#publish-and-share-a-dashboard).

### <a name="azure-monitor-alerts"></a>Azure Izleyici uyarıları

Ayrıca, bir ölçüm değeri veya bir sorgu sonuçlarının belirli koşullara uyması durumunda tetiklenecek Azure Izleyici uyarılarını da ayarlayabilirsiniz. Örnek olarak, bir veya daha fazla düğüm 5 saat içinde bir sinyal göndermediği zaman bir e-posta göndermek için bir uyarı oluşturalım (yani, kullanılamaz olarak kabul edilir).

**Günlüklerde**, aşağıda gösterildiği gibi, bu sorguda **Çalıştır** ' ı seçerek **kullanılamayan bilgisayarlar** örnek sorgusunu çalıştırın.

![Log Analytics çalışma alanı günlükleri ' kullanılamayan bilgisayarlar ' örneği](media/hdinsight-cluster-availability/portal-unavailable-computers.png)

Tüm düğümler varsa, bu sorgu şimdilik sıfır sonuç döndürmelidir. Bu sorgu için uyarınızı yapılandırmaya başlamak üzere **Yeni uyarı kuralı** ' na tıklayın.

![Log Analytics çalışma alanı yeni uyarı kuralı](media/hdinsight-cluster-availability/portal-logs-new-alert-rule.png)

Bir uyarının üç bileşeni vardır: kuralın oluşturulacağı *kaynak* (bu durumda Log Analytics çalışma alanı), uyarının tetiklendiği *koşul* ve uyarı tetiklendiğinde ne olacağını belirleyen *eylem grupları* .
Sinyal mantığını yapılandırmayı tamamlaması için aşağıda gösterildiği gibi **koşul başlığına**tıklayın.

![Portal uyarısı kural oluşturma koşulu](media/hdinsight-cluster-availability/portal-condition-title.png)

Bu işlem, **sinyal mantığını Yapılandır**' ını açar.

**Uyarı mantığı** bölümünü aşağıdaki şekilde ayarlayın:

*Temel alan: sonuç sayısı, koşul: büyüktür, eşik: 0.*

Bu sorgu yalnızca sonuç olarak kullanılamayan düğümleri döndürdüğünden, sonuç sayısı 0 ' dan büyük olursa uyarının tetiklenmesi gerekir.

**Değerlendirilen temelinde** bölümünde, kullanılamayan düğümleri ne sıklıkta denetlemek istediğinize göre **dönemi** ve **sıklığı** ayarlayın.

Bu uyarının amacı için **period = Frequency** olduğundan emin olmak istiyorsunuz. Period, sıklık ve diğer uyarı parametreleri hakkında daha fazla bilgi [burada](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log#log-search-alert-rule---definition-and-types)bulunabilir.

Sinyal mantığını yapılandırmayı bitirdiğinizde **bitti** ' yi seçin.

![Uyarı kuralı, sinyal mantığını yapılandırır](media/hdinsight-cluster-availability/portal-configure-signal-logic.png)

Zaten mevcut bir eylem grubunuz yoksa, **eylem grupları** bölümünde **Yeni oluştur** ' a tıklayın.

![Uyarı kuralı yeni eylem grubu oluşturuyor](media/hdinsight-cluster-availability/portal-create-new-action-group.png)

Bu işlem, **eylem grubu Ekle**' ye açılır. Bir **eylem grubu adı**, **kısa ad**, **abonelik**ve **kaynak grubu seçin.** **Eylemler** bölümünde, **eylem adı** ' nı seçin ve **eylem türü** olarak **e-posta/SMS/Push/ses'** i seçin.

> [!NOTE]
> Bir uyarının bir Azure Işlevi, LogicApp, Web kancası, ıTSM ve Otomasyon Runbook 'u gibi bir e-posta/SMS/Push/sesden farklı olarak tetikleyebileceği birkaç başka eylem vardır. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#action-specific-information)

Bu, **e-posta/SMS/Push/seslendirmeyi**açar. Alıcı için bir **ad** seçin, **e-posta** kutusunu **işaretleyin** ve uyarının gönderilmesini istediğiniz e-posta adresini yazın. Eylem grubunuzu yapılandırmayı tamamlaymak için **e-posta/SMS/Push/sesde** **Tamam** ' ı ve ardından **eylem grubu Ekle** ' yi seçin.

![Uyarı kuralı ekleme eylem grubu oluşturur](media/hdinsight-cluster-availability/portal-add-action-group.png)

Bu dikey pencereler kapatıldıktan sonra eylem **grupları** bölümünün altında listelenmiş eylem grubunuzu görmeniz gerekir. Son olarak, uyarı **ayrıntıları** bölümünü bir **Uyarı kuralı adı** ve **açıklaması** yazıp bir **önem derecesi**seçerek doldurun. Bitiş için **Uyarı kuralı oluştur** ' a tıklayın.

![Portal Uyarı kuralı sonu oluşturuyor](media/hdinsight-cluster-availability/portal-create-alert-rule-finish.png)

> [!TIP]
> **Önem derecesi** belirtme özelliği, birden çok uyarı oluştururken kullanılabilecek güçlü bir araçtır. Örneğin, tek bir baş düğüm aşağı gittiğinde bir uyarı oluşturmak için bir uyarı (sev 1) ve her iki baş düğümün da önemli olmayan olayda kritik (sev 0) oluşturan başka bir uyarı oluşturabilirsiniz.

Bu uyarının koşulu karşılandığında, uyarı harekete geçeceğiz ve uyarı ayrıntılarına şu şekilde bir e-posta alacaksınız:

![Azure Izleyici uyarı e-postası örneği](media/hdinsight-cluster-availability/portal-oms-alert-email.png)

Ayrıca, **Log Analytics çalışma alanınızdaki** **uyarılara** giderek, önem derecesine göre gruplandırılan tüm uyarıları da görüntüleyebilirsiniz.

![Log Analytics çalışma alanı uyarıları](media/hdinsight-cluster-availability/hdi-portal-oms-alerts.png)

Önem düzeyi gruplandırmada (yukarıda vurgulanan gibi **sev 1** ) seçilmesi, bu önem derecesindeki tüm uyarıların kayıtlarını aşağıda gösterildiği gibi gösterir:

![Log Analytics çalışma alanı önem derecesi bir uyarı](media/hdinsight-cluster-availability/portal-oms-alerts-sev1.png)

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight 'ta Apache Hadoop kümelerinin kullanılabilirliği ve güvenilirliği](hdinsight-high-availability-linux.md)
