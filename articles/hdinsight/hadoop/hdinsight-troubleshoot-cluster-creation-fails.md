---
title: Azure HDInsight ile küme oluşturma hatalarını giderme
description: Azure HDInsight için Apache küme oluşturma sorunlarını nasıl gidereceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: troubleshooting
ms.date: 04/14/2020
ms.openlocfilehash: a95c4d654650276c815c5b23fb2c6f8a6d06bc2e
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383464"
---
# <a name="troubleshoot-cluster-creation-failures-with-azure-hdinsight"></a>Azure HDInsight ile küme oluşturma hatalarını giderme

Küme oluşturma hataları için en yaygın kök nedenleri şunlardır:

- İzin sorunları
- Kaynak ilkesi kısıtlamaları
- Güvenlik Duvarları
- Kaynak kilitleri
- Desteklenmeyen bileşen sürümleri
- Depolama hesabı adı kısıtlamaları
- Hizmet kesintileri

## <a name="permissions-issues"></a>İzin sorunları

Azure Veri Gölü Depolama Gen2 kullanıyorsanız ve ```AmbariClusterCreationFailedErrorCode``` ```Internal server error occurred while processing the request. Please retry the request or contact support.```hata alıyorsanız , ', Azure portalını açın, Depolama hesabınıza gidin ve Access Control (IAM) altında, **Depolama Blob Veri Katılımcısı** veya **Depolama Blob Veri Sahibi** rolünün abonelik için atanan Kullanıcıya erişim **atadığından** emin olun. Ayrıntılı yönergeler için [Data Lake Storage 2. Nesil hesabında yönetilen kimlik için izinleri ayarlama](../hdinsight-hadoop-use-data-lake-storage-gen2.md#set-up-permissions-for-the-managed-identity-on-the-data-lake-storage-gen2-account) konusuna bakın.

Azure Veri Gölü Depolama Gen1 kullanıyorsanız, kurulum ve yapılandırma [yönergelerine buradan](../hdinsight-hadoop-use-data-lake-store.md)bakın. Veri Gölü Depolama Gen1 HBase kümeleri için desteklenmez ve HDInsight sürüm 4.0'da desteklenmez.

Azure Depolama'yı kullanıyorsanız, küme oluşturma sırasında depolama hesabı adının geçerli olduğundan emin olun.

## <a name="resource-policy-restrictions"></a>Kaynak ilkesi kısıtlamaları

Abonelik tabanlı Azure ilkeleri, genel IP adreslerinin oluşturulmasını reddedebilir. HDInsight kümesi oluşturmak için iki genel IP gerekir.  

Genel olarak, aşağıdaki ilkeler küme oluşturmayı etkileyebilir:

* Abonelik içinde IP Adresi & Yük bakiyelerinin oluşturulmasını engelleyen ilkeler.
* Depolama hesabıoluşturulmasını engelleyen ilke.
* Ağ kaynaklarının silinmesini önleyen ilke (IP Adresi /Yük Dengeleyicileri).

## <a name="firewalls"></a>Güvenlik Duvarları

Sanal ağınızdaki veya depolama hesabınızdaki güvenlik duvarları, HDInsight yönetimi IP adresleriyle iletişimi reddedebilir.

Aşağıdaki tablodaki IP adreslerinden gelen trafiğe izin verin.

| Kaynak IP adresi | Hedef | Yön |
|---|---|---|
| 168.61.49.99 | *:443 | Gelen |
| 23.99.5.239 | *:443 | Gelen |
| 168.61.48.131 | *:443 | Gelen |
| 138.91.141.162 | *:443 | Gelen |

Ayrıca, kümenin oluşturulduğu bölgeye özgü IP adreslerini de ekleyin. Her Azure bölgesiiçin adreslerin listesi için [HDInsight yönetim IP adreslerine](../hdinsight-management-ip-addresses.md) bakın.

Ekspres bir rota veya kendi özel DNS sunucunuz kullanıyorsanız, [birden çok ağı birbirine bağlayan Azure HDInsight için sanal ağ planı'na](../hdinsight-plan-virtual-network-deployment.md#multinet)bakın.

## <a name="resources-locks"></a>Kaynak kilitleri  

[Sanal ağınızda ve kaynak grubunuzda kilit](../../azure-resource-manager/management/lock-resources.md)olmadığından emin olun. Kaynak grubu kilitlenirse kümeler oluşturulamaz veya silinemez. 

## <a name="unsupported-component-versions"></a>Desteklenmeyen bileşen sürümleri

Çözümünüzde Azure [HDInsight'ın desteklenen](../hdinsight-component-versioning.md) bir sürümünü ve [tüm Apache Hadoop bileşenlerini](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions) kullandığınızdan emin olun.  

## <a name="storage-account-name-restrictions"></a>Depolama hesabı adı kısıtlamaları

Depolama hesabı adları 24 karakterden fazla olamaz ve özel bir karakter içeremez. Bu kısıtlamalar depolama hesabındaki varsayılan kapsayıcı adı için de geçerlidir.

Küme oluşturma için diğer adlandırma kısıtlamaları da geçerlidir. Daha fazla bilgi için [Küme adı kısıtlamalarına](../hdinsight-hadoop-provision-linux-clusters.md#cluster-name)bakın.

## <a name="service-outages"></a>Hizmet kesintileri

Olası kesintiler veya hizmet sorunları için [Azure durumunu](https://status.azure.com) denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Sanal Ağını kullanarak Azure HDInsight kapsamını genişletme](../hdinsight-plan-virtual-network-deployment.md)
* [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)  
* [Azure HDInsight kümeleri ile Azure Depolama'yı kullanma](../hdinsight-hadoop-use-blob-storage.md)
* [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight'ta küme oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
