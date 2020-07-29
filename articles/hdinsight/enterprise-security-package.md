---
title: Azure HDInsight için Kurumsal Güvenlik Paketi
description: Azure HDInsight 'ta Kurumsal Güvenlik Paketi bileşenleri ve sürümleri hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: 07210350826001bd3e0be6e04be211c9de43695a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82997262"
---
# <a name="enterprise-security-package-for-azure-hdinsight"></a>Azure HDInsight için Kurumsal Güvenlik Paketi

Kurumsal Güvenlik, HDInsight kümenize küme iş akışı oluşturma işlemi kapsamında ekleyebileceğiniz isteğe bağlı bir pakettir. Kurumsal Güvenlik Paketi şunları destekler:

* Kimlik doğrulaması için Active Directory ile tümleştirme.

    Geçmişte, yerel yönetici kullanıcısı ve yerel SSH kullanıcısı ile HDInsight kümeleri oluşturdunuz. Yerel yönetici kullanıcı tüm dosyalara, klasörlere, tablolara ve sütunlara erişebilir.  Kurumsal Güvenlik Paketi, HDInsight 'ı Active Directory ile tümleştirerek rol tabanlı erişim denetimini etkinleştirirsiniz. Şirket içi Active Directory Azure Active Directory Domain Services içerir. IaaS sanal makinesinde veya Active Directory. Kümedeki etki alanı yöneticisi, kullanıcılara kendi şirket (etki alanı) Kullanıcı adını ve parolasını kullanmasını sağlayabilir.

    Daha fazla bilgi için bkz.

    * [Etki alanına katılmış HDInsight kümeleriyle güvenliği Apache Hadoop bir giriş](./domain-joined/hdinsight-security-overview.md)

    * [HDInsight 'ta Azure etki alanına katılmış Apache Hadoop kümeleri planlayın](./domain-joined/apache-domain-joined-architecture.md)

    * [Etki alanına katılmış korumalı alan ortamını yapılandırma](./domain-joined/apache-domain-joined-configure.md)

    * [Azure Active Directory Domain Services kullanarak etki alanına katılmış HDInsight kümelerini yapılandırma](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

* Veri yetkilendirmesi

  * Hive, Spark SQL ve Yarn kuyrukları için yetkilendirme için Apache Ranger ile tümleştirme.
  * Dosya ve klasörlerde erişim denetimi ayarlayabilirsiniz.

    Daha fazla bilgi için bkz. [etki alanına katılmış HDInsight 'ta Apache Hive Ilkelerini yapılandırma](./domain-joined/apache-domain-joined-run-hive.md)

* Erişimleri ve yapılandırılan ilkeleri izlemek için Denetim günlüklerini görüntüleyin.

## <a name="supported-cluster-types"></a>Desteklenen küme türleri

Şu anda yalnızca aşağıdaki küme türleri Kurumsal Güvenlik Paketi destekler:

* Hadoop (yalnızca HDInsight 3,6)
* Spark
* Kafka
* HBase
* Interactive Query

## <a name="support-for-azure-data-lake-storage"></a>Azure Data Lake Storage için destek

Kurumsal Güvenlik Paketi, hem birincil depolama hem de eklenti depolama alanı olarak Azure Data Lake Storage kullanımını destekler.

## <a name="pricing-and-service-level-agreement-sla"></a>Fiyatlandırma ve hizmet düzeyi sözleşmesi (SLA)

Kurumsal Güvenlik Paketi için fiyatlandırma ve SLA hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Hadoop, Spark ve daha fazlası için küme kurulumu](hdinsight-hadoop-provision-linux-clusters.md)
* [Windows BILGISAYARDAN HDInsight üzerinde Apache Hadoop çalışma](hdinsight-hadoop-windows-tools.md)
* [Azure HDInsight sürümleriyle ilişkili hortonçalışmalar sürüm notları](./hortonworks-release-notes.md)
* [HDInsight üzerindeki Apache bileşenleri](./hdinsight-component-versioning.md)