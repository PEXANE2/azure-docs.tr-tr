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
ms.openlocfilehash: 42c1efa176a6dbf5294e19afe02fd74a53150272
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92538962"
---
# <a name="transport-layer-security-in-azure-hdinsight"></a>Azure HDInsight 'ta Aktarım Katmanı Güvenliği

Ortak küme uç noktası aracılığıyla HDInsight kümesine yapılan bağlantılar, `https://CLUSTERNAME.azurehdinsight.net` küme ağ geçidi düğümleri aracılığıyla proxy olarak kullanılır. Bu bağlantılar, TLS adlı bir protokol kullanılarak güvenli hale getirilir. Ağ geçitlerinde TLS 'in daha yüksek sürümlerini uygulamak, bu bağlantılar için güvenliği iyileştirir. TLS 'nin daha yeni sürümlerini kullanmanız gerektiği hakkında daha fazla bilgi için bkz. [tls 1,0 sorunu çözme](/security/solving-tls1-problem).

Varsayılan olarak, Azure HDInsight kümeleri genel HTTPS uç noktalarında TLS 1,2 bağlantılarını ve geriye dönük uyumluluk için eski sürümleri kabul eder. Küme oluşturma sırasında, Azure portal veya Kaynak Yöneticisi şablonunu kullanarak ağ geçidi düğümlerinde desteklenen en düşük TLS sürümünü denetleyebilirsiniz. Portal için, küme oluşturma sırasında **güvenlik + ağ** sekmesinden TLS sürümünü seçin. Dağıtım zamanında bir Kaynak Yöneticisi şablonu için **Minsupportedtlsversion** özelliğini kullanın. Örnek bir şablon için bkz. [HDInsight en düşük TLS 1,2 hızlı başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-minimum-tls). Bu özellik üç değeri destekler: TLS 1.0 +, TLS 1.1 + ve TLS 1.2 + ' ya karşılık gelen "1,0", "1,1" ve "1,2".


## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight için bir sanal ağ planlayın](./hdinsight-plan-virtual-network-deployment.md)
* [Azure HDInsight kümeleri için sanal ağlar oluşturun](hdinsight-create-virtual-network.md).
* [Ağ güvenlik grupları](../virtual-network/network-security-groups-overview.md).