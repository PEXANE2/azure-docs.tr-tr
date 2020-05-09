---
title: Azure HDInsight 'ta Apache ambarı ile küme kullanılabilirliğini izleme
description: Küme durumunu ve kullanılabilirliğini izlemek için Apache ambarı 'nı nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,seoapr2020
ms.date: 05/01/2020
ms.openlocfilehash: 4b26128b794a6a667edc578f56ad0bc9fb8303a7
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691145"
---
# <a name="how-to-monitor-cluster-availability-with-apache-ambari-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache ambarı ile küme kullanılabilirliğini izleme

HDInsight kümeleri, bir bakışta sağlık bilgileri ve önceden tanımlanmış uyarılar sağlayan Apache ambarı 'nı içerir.

Bu makalede, kümenizi izlemek ve bir bir veya daha fazla düğümden beş saat içinde bir sinyal alınmadığında başlatılan bir Azure Izleyici uyarısı oluşturmak için ambarı 'nın nasıl kullanılacağı gösterilmektedir.

## <a name="dashboard"></a>Pano

**Ambarı panosuna** aşağıda gösterildiği gibi, HDInsight genel bakış konusunun **küme panoları** bölümünde bulunan Azure Portal. Alternatif olarak, CLUSTERNAME öğesinin Kümenizin adı olduğu bir `https://CLUSTERNAME.azurehdinsight.net` tarayıcıda gezinilerek erişilebilir.

![HDInsight kaynak portalı görünümü](media/hdinsight-cluster-availability/azure-portal-dashboard-ambari.png)

Daha sonra bir küme oturum açma Kullanıcı adı ve parolası istenir. Kümeyi oluştururken seçtiğiniz kimlik bilgilerini girin.

Daha sonra, HDInsight kümenizin sistem durumuna hızlı bir genel bakış sunmak için bir dizi ölçümü gösteren pencere öğeleri içeren, daha sonra bir ambarı panosuna götürülürsünüz. Bu pencere öğeleri, Spark ve Hadoop kümeleri için YARN ResourceManager çalışma süresi gibi belirli küme türlerine özgü ölçümler ve canlı düğüm sayısı (çalışan düğümleri) ve JournalNodes (Zookeeper node), Üçlü iş çalışma süresi (örneğin, çok sayıda) gibi ölçümleri gösterir.

![Apache ambarı kullanım panosu görüntüleme](media/hdinsight-cluster-availability/apache-ambari-dashboard.png)

## <a name="hosts--view-individual-node-status"></a>Konaklar – tek tek düğüm durumunu görüntüle

Tek tek düğümlerin durum bilgilerini de görüntüleyebilirsiniz. Kümenizdeki tüm düğümlerin listesini görüntülemek için **konaklar** sekmesini seçin ve her düğüm hakkında temel bilgileri görüntüleyin. Her düğüm adının solunda yeşil onay işareti, tüm bileşenlerin düğüm üzerinde olduğunu gösterir. Bir bileşen düğüm üzerinde kapalıysa yeşil onay yerine kırmızı bir uyarı üçgeni görürsünüz.

![HDInsight Apache ambarı Konakları görünümü](media/hdinsight-cluster-availability/apache-ambari-hosts1.png)

Daha sonra söz konusu düğüme ait daha ayrıntılı konak ölçümlerini görüntülemek için bir düğüm **adı** ' nı seçebilirsiniz. Bu görünüm her bir bileşenin durumunu/kullanılabilirliğini gösterir.

![Apache ambarı tek düğümlü görünüm barındırır](media/hdinsight-cluster-availability/apache-ambari-hosts-node.png)

## <a name="ambari-alerts"></a>Ambarı uyarıları

Ayrıca, belirli olaylara yönelik bildirim sağlayabilen birkaç yapılandırılabilir uyarı da sunar. Uyarılar tetiklendiğinde, bu uyarılar, bu uyarı sayısını içeren kırmızı bir rozet içinde, ambarı 'nın sol üst köşesinde gösterilir. Bu rozet seçildiğinde geçerli uyarıların bir listesi gösterilir.

![Apache ambarı geçerli uyarı sayısı](media/hdinsight-cluster-availability/apache-ambari-alerts.png)

Uyarı tanımlarının ve durumlarının listesini görüntülemek için, aşağıda gösterildiği gibi **Uyarılar** sekmesini seçin.

![Ambarı uyarı tanımları görünümü](media/hdinsight-cluster-availability/ambari-alerts-definitions.png)

Ambarı aşağıdakiler dahil olmak üzere kullanılabilirliğiyle ilgili çok sayıda önceden tanımlı uyarı sunar:

| Uyarı Adı                        | Açıklama   |
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

## <a name="email-notifications"></a>E-posta bildirimleri

Ayrıca, isteğe bağlı olarak, ambarı uyarıları için e-posta bildirimleri yapılandırabilirsiniz. Bunu yapmak için, **Uyarılar** sekmesinde, sol üst köşedeki **Eylemler** düğmesine ve ardından **Bildirimleri Yönet** ' e tıklayın.

![Ambarı yönetimi bildirimleri eylemi](media/hdinsight-cluster-availability/ambari-manage-notifications.png)

Uyarı bildirimlerini yönetmek için bir iletişim kutusu açılır. İletişim kutusunun **+** alt kısmındaki öğesini seçin ve e-posta göndermek için e-posta sunucusu ayrıntılarını kullanarak, ambarı sağlamak üzere gerekli alanları doldurun.

> [!TIP]
> Ambarı e-posta bildirimlerinin kurulması, birçok HDInsight kümesini yönetirken tek bir yerde uyarı almanın iyi bir yoludur.

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight 'ta Apache Hadoop kümelerinin kullanılabilirliği ve güvenilirliği](hdinsight-high-availability-linux.md)
- [Küme kullanılabilirliği-Azure Izleyici günlükleri](./cluster-availability-monitor-logs.md)
- [Azure Izleyici günlüklerini kullanma](hdinsight-hadoop-oms-log-analytics-tutorial.md)
- [Apache Ambari e-posta bildirimleri](apache-ambari-email.md)
