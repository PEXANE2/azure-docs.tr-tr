---
title: Azure HDInsight'ta katman güvenliğini taşıma
description: Aktarım katmanı güvenliği (TLS) ve güvenli soket katmanı (SSL), bilgisayar ağı üzerinden iletişim güvenliği sağlayan şifreleme protokolleridir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: fbe602581ebcea6385fb9cc9953d8e48272ce429
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81771969"
---
# <a name="transport-layer-security-in-azure-hdinsight"></a>Azure HDInsight'ta katman güvenliğini taşıma

Genel küme bitiş noktası `https://CLUSTERNAME.azurehdinsight.net` üzerinden HDInsight kümesine bağlantılar küme ağ geçidi düğümleri aracılığıyla yakınsadır. Bu bağlantılar TLS adı verilen bir protokol kullanılarak sabitlenir. TLS'nin ağ geçitlerinde daha yüksek sürümlerinin uygulanması, bu bağlantıların güvenliğini artırır. TLS'nin yeni sürümlerini neden kullanmanız gerektiği hakkında daha fazla bilgi için [TLS 1.0 Sorununu Çözme](https://docs.microsoft.com/security/solving-tls1-problem)bölümüne bakın.

Varsayılan olarak, Azure HDInsight kümeleri, genel HTTPS uç noktalarında TLS 1.2 bağlantılarını ve geriye dönük uyumluluk için eski sürümleri kabul eder. Azure portalı veya Kaynak Yöneticisi şablonu kullanarak küme oluşturma sırasında ağ geçidi düğümlerinde desteklenen minimum TLS sürümünü denetleyebilirsiniz. Portal için, küme oluşturma sırasında **Güvenlik + ağ** sekmesinden TLS sürümünü seçin. Dağıtım zamanında bir Kaynak Yöneticisi şablonu için **minSupportedTlsVersion** özelliğini kullanın. Örnek bir şablon için [HDInsight minimum TLS 1.2 Quickstart şablonuna](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-minimum-tls)bakın. Bu özellik üç değeri destekler: sırasıyla TLS 1.0+, TLS 1.1+ ve TLS 1.2+ karşılık gelen "1.0", "1.1" ve "1.2".

> [!IMPORTANT]
> Azure HDInsight, 30 Haziran 2020'den itibaren tüm HTTPS bağlantıları için TLS 1.2 veya sonraki sürümlerini uygular. Tüm müşterilerinizin TLS 1.2 veya sonraki sürümlerini işlemeye hazır olduğundan emin olmamızı öneririz. Daha fazla bilgi için Azure [HDInsight TLS 1.2 Uygulama](https://azure.microsoft.com/updates/azure-hdinsight-tls-12-enforcement/)'ya bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight için sanal ağ planlama](./hdinsight-plan-virtual-network-deployment.md)
* [Azure HDInsight kümeleri için sanal ağlar oluşturun.](hdinsight-create-virtual-network.md)
* [Ağ güvenlik grupları.](../virtual-network/security-overview.md)
