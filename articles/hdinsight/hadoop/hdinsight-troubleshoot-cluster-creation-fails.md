---
title: Azure HDInsight ile küme oluşturma hatalarıyla ilgili sorunları giderme
description: Azure HDInsight için Apache kümesi oluşturma sorunlarını giderme hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: troubleshooting
ms.date: 08/26/2019
ms.openlocfilehash: ea60faf5b5689fa674095201d3db18422d3e0f1b
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75980490"
---
# <a name="troubleshoot-cluster-creation-failures-with-azure-hdinsight"></a>Azure HDInsight ile küme oluşturma hatalarıyla ilgili sorunları giderme

Aşağıdaki sorunlar, küme oluşturma hatalarının en yaygın temel nedenlerdir:

- İzin sorunları
- Kaynak ilkesi kısıtlamaları
- Güvenlik Duvarları
- Kaynak kilitleri
- Desteklenmeyen bileşen sürümleri
- Depolama hesabı adı kısıtlamaları
- Hizmet kesintileri

## <a name="permissions-issues"></a>İzin sorunları

Azure Data Lake Storage 2. kullanıyorsanız ve ```AmbariClusterCreationFailedErrorCode```hata alıyorsanız ```Internal server error occurred while processing the request. Please retry the request or contact support.```Azure portal açın, depolama hesabınıza gidin ve Access Control (ıAM) altında, **Depolama Blobu veri katılımcısı** veya **Depolama Blobu veri sahibi** rolünün, abonelik için **Kullanıcı tarafından atanan yönetilen kimliğe** erişimi atandığından emin olun. Ayrıntılı yönergeler için [Data Lake Storage 2. Nesil hesabında yönetilen kimlik için izinleri ayarlama](../hdinsight-hadoop-use-data-lake-storage-gen2.md#set-up-permissions-for-the-managed-identity-on-the-data-lake-storage-gen2-account) konusuna bakın.

Azure Data Lake Storage 1. kullanıyorsanız, kurulum ve [yapılandırma yönergeleri bölümüne](../hdinsight-hadoop-use-data-lake-store.md)bakın. Data Lake Storage 1., HBase kümelerinde desteklenmez ve HDInsight sürüm 4,0 ' de desteklenmez.

Azure Storage kullanıyorsanız, küme oluşturma sırasında depolama hesabı adının geçerli olduğundan emin olun.

## <a name="resource-policy-restrictions"></a>Kaynak ilkesi kısıtlamaları

Abonelik tabanlı Azure ilkeleri, genel IP adreslerinin oluşturulmasını reddedebilir. HDInsight kümesi oluşturmak için iki genel IP gerekir.  

Genel olarak, aşağıdaki ilkeler küme oluşturmayı etkileyebilir:

* Abonelik içindeki yük dengeleyiciler & IP adresi oluşturulmasını engellemeye yönelik ilkeler.
* Depolama hesabı oluşturulmasını önleyecek ilke.
* Ağ kaynaklarının silinmesini önleyecek ilke (IP adresi/yük dengeleyiciler).

## <a name="firewalls"></a>Güvenlik Duvarları

Sanal ağ veya depolama hesabınızdaki güvenlik duvarları, HDInsight yönetim IP adresleriyle iletişimi reddedebilir.

Aşağıdaki tablodaki IP adreslerinden gelen trafiğe izin verin.

| Kaynak IP adresi | Hedef | Yön |
|---|---|---|
| 168.61.49.99 | *: 443 | Gelen |
| 23.99.5.239 | *: 443 | Gelen |
| 168.61.48.131 | *: 443 | Gelen |
| 138.91.141.162 | *: 443 | Gelen |

Ayrıca, kümenin oluşturulduğu bölgeye özgü IP adreslerini de ekleyin. Her bir Azure bölgesinin adres listesi için bkz. [HDInsight YÖNETIMI IP adresleri](../hdinsight-management-ip-addresses.md) .

Bir Express Route veya kendi özel DNS sunucunuz kullanıyorsanız bkz. [Azure HDInsight için bir sanal ağ planlayın-birden çok ağı bağlama](../hdinsight-plan-virtual-network-deployment.md#multinet).

## <a name="resources-locks"></a>Kaynak kilitleri  

[Sanal ağınızda ve kaynak grubunuzda kilit](../../azure-resource-manager/management/lock-resources.md)olmadığından emin olun.  

## <a name="unsupported-component-versions"></a>Desteklenmeyen bileşen sürümleri

Çözümünüzde [desteklenen bir Azure HDInsight sürümü](../hdinsight-component-versioning.md) ve [Apache Hadoop bileşenleri](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions) kullandığınızdan emin olun.  

## <a name="storage-account-name-restrictions"></a>Depolama hesabı adı kısıtlamaları

Depolama hesabı adları 24 karakterden uzun olamaz ve özel bir karakter içeremez. Bu kısıtlamalar depolama hesabındaki varsayılan kapsayıcı adı için de geçerlidir.

Diğer adlandırma kısıtlamaları da küme oluşturma için geçerlidir. Daha fazla bilgi için bkz. [küme adı kısıtlamaları](../hdinsight-hadoop-provision-linux-clusters.md#cluster-name).

## <a name="service-outages"></a>Hizmet kesintileri

Olası kesintiler veya hizmet sorunları için [Azure durumunu](https://status.azure.com/status) denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Sanal Ağını kullanarak Azure HDInsight kapsamını genişletme](../hdinsight-plan-virtual-network-deployment.md)
* [Azure HDInsight kümeleriyle Azure Data Lake Storage 2. Nesil hizmetini kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)  
* [Azure HDInsight kümeleri ile Azure Depolama'yı kullanma](../hdinsight-hadoop-use-blob-storage.md)
* [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight'ta küme oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
