---
title: Azure HDInsight 'ta Aktarım Katmanı Güvenliği
description: Aktarım Katmanı Güvenliği (TLS) ve Güvenli Yuva Katmanı (SSL), bir bilgisayar ağı üzerinden iletişim güvenliği sağlayan şifreleme protokolleridir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: b74ca75b26d4d98c79091683f428eb39e5827665
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82183509"
---
# <a name="transport-layer-security-in-azure-hdinsight"></a>Azure HDInsight 'ta Aktarım Katmanı Güvenliği

Ortak küme uç noktası aracılığıyla HDInsight kümesine yapılan bağlantılar, `https://CLUSTERNAME.azurehdinsight.net` küme ağ geçidi düğümleri aracılığıyla proxy olarak kullanılır. Bu bağlantılar, TLS adlı bir protokol kullanılarak güvenli hale getirilir. Ağ geçitlerinde TLS 'in daha yüksek sürümlerini uygulamak, bu bağlantılar için güvenliği iyileştirir. TLS 'nin daha yeni sürümlerini kullanmanız gerektiği hakkında daha fazla bilgi için bkz. [tls 1,0 sorunu çözme](https://docs.microsoft.com/security/solving-tls1-problem).

Varsayılan olarak, Azure HDInsight kümeleri genel HTTPS uç noktalarında TLS 1,2 bağlantılarını ve geriye dönük uyumluluk için eski sürümleri kabul eder. Küme oluşturma sırasında, Azure portal veya Kaynak Yöneticisi şablonunu kullanarak ağ geçidi düğümlerinde desteklenen en düşük TLS sürümünü denetleyebilirsiniz. Portal için, küme oluşturma sırasında **güvenlik + ağ** sekmesinden TLS sürümünü seçin. Dağıtım zamanında bir Kaynak Yöneticisi şablonu için **Minsupportedtlsversion** özelliğini kullanın. Örnek bir şablon için bkz. [HDInsight en düşük TLS 1,2 hızlı başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-minimum-tls). Bu özellik üç değeri destekler: TLS 1.0 +, TLS 1.1 + ve TLS 1.2 + ' ya karşılık gelen "1,0", "1,1" ve "1,2".

> [!IMPORTANT]
> 30 Haziran 2020 ' den itibaren Azure HDInsight, tüm HTTPS bağlantıları için TLS 1,2 veya sonraki sürümlerini zorlar. Tüm istemcilerinizin TLS 1,2 veya sonraki sürümlerini işlemeye hazırlanmasını öneririz. Daha fazla bilgi için bkz. [Azure HDıNSIGHT TLS 1,2 zorlama](https://azure.microsoft.com/updates/azure-hdinsight-tls-12-enforcement/).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight için bir sanal ağ planlayın](./hdinsight-plan-virtual-network-deployment.md)
* [Azure HDInsight kümeleri için sanal ağlar oluşturun](hdinsight-create-virtual-network.md).
* [Ağ güvenlik grupları](../virtual-network/security-overview.md).
