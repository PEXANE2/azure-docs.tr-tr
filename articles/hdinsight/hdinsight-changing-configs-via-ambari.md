---
title: Azure HDInsight 'ta Apache ambarı ile kümeleri iyileştirme
description: Azure HDInsight kümelerini yapılandırmak ve iyileştirmek için Apache ambarı Web Kullanıcı arabirimini kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 05/04/2020
ms.openlocfilehash: 52eb1b6c89ff02cb44fe731c2463ab02c284f26c
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086458"
---
# <a name="optimize-clusters-with-apache-ambari-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache ambarı ile kümeleri iyileştirme

HDInsight, büyük ölçekli veri işleme uygulamaları için Apache Hadoop kümeleri sağlar. Bu karmaşık çok düğümlü kümelerin yönetilmesi, izlenmesi ve iyileştirgetirilmesi zor olabilir. Apache ambarı, HDInsight Linux kümelerini yönetmek ve izlemek için bir Web arabirimidir.

Ambarı Web Kullanıcı arabirimini kullanmaya giriş için bkz [. Apache ambarı Web Kullanıcı arabirimini kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)

Küme kimlik bilgilerinizle, ambarı ' nda oturum açın `https://CLUSTERNAME.azurehdidnsight.net` . İlk ekranda Genel Bakış Panosu görüntülenir.

![Apache ambarı Kullanıcı panosu görüntülendi](./media/hdinsight-changing-configs-via-ambari/apache-ambari-dashboard.png)

Ambarı Web Kullanıcı arabirimi, Konakları, Hizmetleri, uyarıları, konfigürasyonları ve görünümleri yönetmek için kullanılır. Bir HDInsight kümesi oluşturmak veya Hizmetleri yükseltmek için ambarı kullanılamaz. Ayrıca yığınları ve sürümleri yönetemez, ana bilgisayarları kullanımdan çıkarmayı veya yeniden komisyon veya kümeye hizmet ekleme.

## <a name="manage-your-clusters-configuration"></a>Kümenizin yapılandırmasını yönetme

Yapılandırma ayarları belirli bir hizmeti ayarlamanıza yardımcı olur. Hizmetin yapılandırma ayarlarını değiştirmek için **Hizmetler** kenar çubuğundan (solda) hizmeti seçin. Ardından hizmet ayrıntısı sayfasında **, yapılandırma sekmesine gidin** .

![Apache ambarı Hizmetleri kenar çubuğu](./media/hdinsight-changing-configs-via-ambari/ambari-services-sidebar.png)

## <a name="modify-namenode-java-heap-size"></a>Süs Code Java yığın boyutunu değiştir

Süs Code Java yığın boyutu, kümedeki yük gibi birçok faktöre bağlıdır. Ayrıca, dosya sayıları ve blok sayıları. 1 GB varsayılan boyutu çoğu küme ile iyi çalışmaktadır, ancak bazı iş yükleri daha fazla veya daha az bellek gerektirebilir.

Süs Yot Java yığın boyutunu değiştirmek için:

1. Hizmetler **kenar çubuğundan bir i seçin ve** **configs** sekmesine gidin.

    ![Apache ambarı, yapılandırma](./media/hdinsight-changing-configs-via-ambari/ambari-apache-hdfs-config.png)

1. **Süs Code Java yığın boyutu**ayarını bulun. Belirli bir ayarı yazmak ve bulmak için **filtre** metin kutusunu da kullanabilirsiniz. Ayar adının yanındaki **kalem** simgesini seçin.

    ![Apache ambarı süs Code Java yığın boyutu](./media/hdinsight-changing-configs-via-ambari/ambari-java-heap-size.png)

1. Metin kutusuna yeni değeri yazın ve ardından değişikliği kaydetmek için **ENTER** tuşuna basın.

    ![Ambarı düzenleme süs Code Java yığın size1](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edit1.png)

1. Süs Code Java yığın boyutu 2 GB 'tan 1 GB olarak değişir.

    ![Düzenlenmiş süs Code Java yığın size2](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edited.png)

1. Yapılandırma ekranının üstündeki yeşil **Kaydet** düğmesine tıklayarak yaptığınız değişiklikleri kaydedin.

    ![' Apache ambarı kayıt yapılandırması '](./media/hdinsight-changing-configs-via-ambari/ambari-save-changes1.png)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümelerini Apache ambarı Web Kullanıcı arabirimi ile yönetme](hdinsight-hadoop-manage-ambari.md)
* [Apache ambarı REST API](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Apache HBase’i iyileştirme](./optimize-hbase-ambari.md)
* [Apache Hive’ı iyileştirme](./optimize-hive-ambari.md)
* [Apache Pig’i iyileştirme](./optimize-pig-ambari.md)
