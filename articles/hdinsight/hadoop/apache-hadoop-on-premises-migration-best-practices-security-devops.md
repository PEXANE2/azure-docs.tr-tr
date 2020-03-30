---
title: "Güvenlik: Şirket içinde Apache Hadoop'u Azure HDInsight'a geçirin"
description: Şirket içi Hadoop kümelerini Azure HDInsight'a geçirmek için güvenliği ve DevOps'leri en iyi uygulamaları öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: ashishth
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/19/2019
ms.openlocfilehash: 4ceefcbbbb53e3ae13f8ced930ae8417fb00965f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75974416"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---security-and-devops-best-practices"></a>Şirket içi Apache Hadoop kümelerini Azure HDInsight'a geçirin - güvenlik ve DevOps en iyi uygulamaları

Bu makalede, Azure HDInsight sistemlerinde güvenlik ve DevOps için öneriler sunulur. Şirket içi Apache Hadoop sistemlerini Azure HDInsight'a geçirmede yardımcı olmak için en iyi uygulamaları sağlayan bir serinin parçasıdır.

## <a name="secure-and-govern-cluster-with-enterprise-security-package"></a>Kurumsal Güvenlik Paketi ile cluster'ı güvenli ve yönet

Kurumsal Güvenlik Paketi (ESP), Etkin Dizin tabanlı kimlik doğrulamasını, çok kullanıcılı desteği ve rol tabanlı erişim denetimini destekler. Seçilen ESP seçeneği ile HDInsight kümesi Active Directory etki alanına katılır ve kurumsal yönetici Apache Ranger kullanarak Apache Hive güvenliği için rol tabanlı erişim denetimini (RBAC) yapılandırabilir. Yönetici ayrıca çalışanların veri erişimini ve denetim ilkelerine erişmek için yapılan değişiklikleri denetleyebilir.

ESP aşağıdaki küme türlerinde kullanılabilir: Apache Hadoop, Apache Spark, Apache HBase, Apache Kafka ve Interactive Query (Hive LLAP).

Etki alanına birleştirilmiş HDInsight kümesini dağıtmak için aşağıdaki adımları kullanın:

- Alan Adını geçerek Azure Etkin Dizini'ni (AAD) dağıtın.
- Azure Etkin Dizin Etki Alanı Hizmetlerini (AAD DS) dağıtın.
- Gerekli Sanal Ağ ve alt ağı oluşturun.
- AAD DS'yi yönetmek için Sanal Ağ'da bir VM dağıtın.
- VM'ye etki alanına katılın.
- AD ve DNS araçlarını yükleyin.
- AAD DS Yöneticisinin bir Kuruluş Birimi (OU) oluşturmasını istiyorum.
- AAD DS için LDAPS'yi etkinleştirin.
- Azure Active Directory'de, atamalı okuma & OU'ya yönetici izni yazarak bir hizmet hesabı oluşturun, böylece bu hesap. Bu hizmet hesabı daha sonra makineyi etki alanına katılabilir ve makine müdürlerini OU'nun içine yerleştirir. Ayrıca, küme oluşturma sırasında belirttiğiniz OU içinde hizmet ilkeleri oluşturabilir.

    > [!Note]
    > Hizmet hesabının AD etki alanı yöneticisi hesabı olması gerekmez.

- Aşağıdaki parametreleri ayarlayarak HDInsight ESP kümesini dağıtın:

    |Parametre |Açıklama |
    |---|---|
    |Etki alanı adı|Azure AD DS ile ilişkili alan adı.|
    |Etki alanı kullanıcı adı|Önceki bölümde oluşturduğunuz Azure AD DS DC yönetilen etki alanında ki `hdiadmin@contoso.onmicrosoft.com`hizmet hesabı, örneğin: . Bu etki alanı kullanıcısı bu HDInsight kümesinin yöneticisi olacaktır.|
    |Etki alanı parolası|Hizmet hesabının şifresi.|
    |Kurum birimi|Örneğin, HDInsight kümesiyle kullanmak istediğiniz OU'nun ayırt edici `OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com`adı: . Bu OU yoksa, HDInsight kümesi hizmet hesabının ayrıcalıklarını kullanarak OU oluşturmaya çalışır.|
    |LDAPS URL|örneğin, `ldaps://contoso.onmicrosoft.com:636`.|
    |Erişim kullanıcı grubu|Kullanıcıları kümeyle eşitlemek istediğiniz güvenlik grupları, örneğin: `HiveUsers`. Birden çok kullanıcı grubu belirtmek istiyorsanız, bunları yarı sütun ';' olarak ayırın. ESP kümesini oluşturmadan önce grup(lar) dizinde bulunmalıdır.|

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Etki alanı birleştirilmiş HDInsight kümeleri ile Apache Hadoop güvenliğine giriş](../domain-joined/hdinsight-security-overview.md)
- [HDInsight'ta Azure etki alanına katılan Apache Hadoop kümelerini planlayın](../domain-joined/apache-domain-joined-architecture.md)
- [Azure Active Directory Etki Alanı Hizmetlerini kullanarak etki alanı birleştirilmiş HDInsight kümesini yapılandırma](../domain-joined/apache-domain-joined-configure-using-azure-adds.md)
- [Azure Active Directory kullanıcılarını HDInsight kümesine eşitleme](../hdinsight-sync-aad-users-to-cluster.md)
- [Etki Alanına katılan HDInsight'ta Apache Hive ilkelerini yapılandırma](../domain-joined/apache-domain-joined-run-hive.md)
- [Etki alanı birleştirilmiş HDInsight Hadoop kümelerinde Apache Oozie'yi çalıştırın](../domain-joined/hdinsight-use-oozie-domain-joined-clusters.md)

## <a name="implement-end-to-end-enterprise-security"></a>Kuruluş güvenliğini sona erdirmek için son uygulama

Uçuca kurumsal güvenlik aşağıdaki denetimler kullanılarak elde edilebilir:

**Özel ve korumalı veri ardışık alanı (çevre düzeyi güvenliği)**
    - Çevre düzeyinde Güvenlik, Azure Sanal Ağlar, Ağ Güvenlik Grupları ve Ağ Geçidi hizmeti aracılığıyla elde edilebilir.

**Veri erişimi için kimlik doğrulama ve yetkilendirme**
    - Azure Active Directory Domain Services'ı kullanarak Etki Alanı ile birleştirilmiş HDInsight kümesi oluşturun. (Kurumsal Güvenlik Paketi).
    - AD kullanıcıları için küme kaynaklarına Rol tabanlı erişim sağlamak için Ambari'yi kullanın.
    - Tablo / sütun / satır düzeyinde Hive için erişim denetim ilkeleri ayarlamak için Apache Ranger kullanın.
    - Kümeye SSH erişimi yalnızca yöneticiyle sınırlandırılabilir.

**Denetim**
    - HDInsight küme kaynaklarına ve verilerine tüm erişimi görüntüleyin ve bildirin.
    - Erişim denetimi ilkelerindeki tüm değişiklikleri görüntüleyin ve bildirin.

**Şifreleme**
    - Microsoft tarafından yönetilen anahtarları veya müşteri tarafından yönetilen anahtarları kullanarak Saydam Sunucu Tarafı şifrelemesi.
    - İstemci Tarafı şifrelemesi, https ve TLS kullanarak Aktarım şifrelemesinde.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Sanal Ağlara genel bakış](../../virtual-network/virtual-networks-overview.md)
- [Azure Ağ Güvenlik Gruplarına genel bakış](../../virtual-network/security-overview.md)
- [Azure Sanal Ağ eşleme](../../virtual-network/virtual-network-peering-overview.md)
- [Azure Depolama güvenlik kılavuzu](../../storage/blobs/security-recommendations.md)
- [Azure Depolama Hizmeti Şifrelemesi](../../storage/common/storage-service-encryption.md)

## <a name="use-monitoring--alerting"></a>İzleme & uyarı kullanma

Daha fazla bilgi için makaleye bakın:

[Azure İzleyiciye Genel Bakış](../../azure-monitor/overview.md)

## <a name="upgrade-clusters"></a>Yükseltme kümeleri

En son özelliklerden yararlanmak için düzenli olarak en son HDInsight sürümüne yükseltin. Kümeyi en son sürüme yükseltmek için aşağıdaki adımlar kullanılabilir:

1. Mevcut en son HDInsight sürümünü kullanarak yeni bir TEST HDInsight kümesi oluşturun.
1. İşlerin ve iş yüklerinin beklendiği gibi çalıştığından emin olmak için yeni kümeüzerinde test edin.
1. İşleri, uygulamaları veya iş yüklerini gerektiği gibi değiştirin.
1. Küme düğümlerinde yerel olarak depolanan geçici verileri yedekleyin.
1. Varolan kümeyi silin.
1. Önceki kümeyle aynı varsayılan verileri ve meta deposunu kullanarak aynı sanal ağ alt ağında ki en son HDInsight sürümünden oluşan bir küme oluşturun.
1. Yedeklenen geçici verileri içe aktarın.
1. Yeni kümeyi kullanarak işleri başlatın/işleme devam edin.

Daha fazla bilgi için makaleye bakın: [HDInsight kümesini yeni bir sürüme yükseltin.](../hdinsight-upgrade-cluster.md)

## <a name="patch-cluster-operating-systems"></a>Yama kümesi işletim sistemleri

Yönetilen bir Hadoop hizmeti olarak HDInsight, HDInsight kümeleri tarafından kullanılan VM'lerin işletim sistemi yle ilgilenir.

Daha fazla bilgi için makaleye bakın: [HDInsight için işletim sistemi yaması.](../hdinsight-os-patching.md)

## <a name="post-migration"></a>Göç Sonrası

1. **İyileştirici uygulamalar** - İşlerde, işlemlerde ve komut dosyalarında gerekli değişiklikleri yinelemeli olarak yapın.
2. **Testleri Gerçekleştir** - Yinelemeli olarak işlevsel ve performans testleri çalıştırın.
3. **Optimize Etme** - Yukarıdaki test sonuçlarına dayalı performans sorunlarını giderin ve ardından performans iyileştirmelerini onaylamak için yeniden test edin.

## <a name="next-steps"></a>Sonraki adımlar

[HDInsight 4.0](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-introduction)hakkında daha fazla bilgi edinin.
