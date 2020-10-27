---
title: Azure HDInsight 'ta Hive için güvenlik seçenekleri
description: Standart ve ESP kümelerinde Hive için güvenlik seçenekleri.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/02/2020
ms.openlocfilehash: 8573ba99b7aef13025b4f175640ac9583ad5a679
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92545965"
---
# <a name="security-options-for-hive-in-azure-hdinsight"></a>Azure HDInsight 'ta Hive için güvenlik seçenekleri

Bu belge, HDInsight 'ta Hive için önerilen güvenlik seçeneklerini açıklamaktadır. Bu seçenekler, ambarı aracılığıyla yapılandırılabilir.

![' Hive için güvenlik seçenekleri '](./media/hdinsight-security-options-for-hive/security-options-hive.png "Hive için güvenlik seçenekleri")

## <a name="hiveserver2-authentication"></a>HiveServer2 kimlik doğrulaması

Standart kümeler için, HiveServer2 kimlik doğrulaması için önerilen ayar, None olan varsayılan ayardır. Kimlik doğrulamasını etkinleştirmek için bir [ESP](../domain-joined/hdinsight-security-overview.md) (kurumsal güvenlik paketi) kümesine yükseltmeniz önerilir. 

ESP kümelerinde, [Kerberos](https://web.mit.edu/Kerberos/) kimlik doğrulaması varsayılan olarak etkindir. Takılabilir kimlik doğrulama modülleri (PAM) ve özel kimlik doğrulama şemaları desteklenmez.

## <a name="hiveserver2-authorization"></a>HiveServer2 yetkilendirmesi

Standart kümeler için varsayılan ayar None ' dır. [Sqlstdaduth (SQL standartları tabanlı yetkilendirme)](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+based+hive+authorization) etkinleştirilebilir. [Apache Ranger](https://ranger.apache.org/) aracılığıyla yetkilendirme standart kümeler için desteklenmez. Ranger yetkilendirmesi için bir ESP kümesine yükseltmeniz önerilir. 

ESP kümelerinde, Ranger aracılığıyla yetkilendirme varsayılan olarak etkindir. 


## <a name="ssl-encryption-for-hiveserver2"></a>HiveServer2 için SSL şifrelemesi

Hiveserver2 SSL 'nin etkinleştirilmesi, standart veya ESP kümeleri için önerilmez. Bunun yerine SSL, ağ geçidinde etkindir. [Aktarım sırasında şifreleme](../domain-joined/encryption-in-transit.md) , [Internet Protokolü güvenliği (IPSec)](https://en.wikipedia.org/wiki/IPsec)kullanan küme düğümleri arasındaki iletişimleri şifrelemek için etkinleştirilebilir.


## <a name="next-steps"></a>Sonraki adımlar
* [HiveServer2 kimlik doğrulamasına genel bakış](https://cwiki.apache.org/confluence/display/Hive/Setting+up+HiveServer2#SettingUpHiveServer2-Authentication/SecurityConfiguration)
* [HiveServer2 yetkilendirmesi 'ne genel bakış](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Authorization#:~:text=%20Overview%20of%20Authorization%20Modes%20%201%201,and%20Apache%20Sentry%20are%20apache%20projects...%20More%20)
* [SQL standartlarına dayalı Hive yetkilendirmesini etkinleştirme](https://community.cloudera.com/t5/Community-Articles/Getting-started-with-SQLStdAuth/ta-p/244263)
* [Hive ile Apache Ranger](../domain-joined/apache-domain-joined-run-hive.md)
