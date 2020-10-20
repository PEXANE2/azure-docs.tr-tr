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
ms.openlocfilehash: 14a41365640439ff99861bbb22cc04a40f35da5e
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92222972"
---
# <a name="security-options-for-hive-in-azure-hdinsight"></a>Azure HDInsight 'ta Hive için güvenlik seçenekleri

Bu belge, HDInsight 'ta Hive için önerilen güvenlik seçeneklerini açıklamaktadır. Bu seçenekler, ambarı aracılığıyla yapılandırılabilir.

![' Hive için güvenlik seçenekleri '](./media/hdinsight-security-options-for-hive/security-options-hive.png "Hive için güvenlik seçenekleri")

## <a name="hiveserver2-authentication"></a>HiveServer2 kimlik doğrulaması

Standart kümeler için, HiveServer2 kimlik doğrulaması için önerilen ayar, None olan varsayılan ayardır. Kimlik doğrulamasını etkinleştirmek için bir [ESP](https://docs.microsoft.com/azure/hdinsight/domain-joined/hdinsight-security-overview) (kurumsal güvenlik paketi) kümesine yükseltmeniz önerilir. 

ESP kümelerinde, [Kerberos](https://web.mit.edu/Kerberos/) kimlik doğrulaması varsayılan olarak etkindir. Takılabilir kimlik doğrulama modülleri (PAM) ve özel kimlik doğrulama şemaları desteklenmez.

## <a name="hiveserver2-authorization"></a>HiveServer2 yetkilendirmesi

Standart kümeler için varsayılan ayar None ' dır. [Sqlstdaduth (SQL standartları tabanlı yetkilendirme)](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+based+hive+authorization) etkinleştirilebilir. [Apache Ranger](https://ranger.apache.org/) aracılığıyla yetkilendirme standart kümeler için desteklenmez. Ranger yetkilendirmesi için bir ESP kümesine yükseltmeniz önerilir. 

ESP kümelerinde, Ranger aracılığıyla yetkilendirme varsayılan olarak etkindir. 


## <a name="ssl-encryption-for-hiveserver2"></a>HiveServer2 için SSL şifrelemesi

Hiveserver2 SSL 'nin etkinleştirilmesi, standart veya ESP kümeleri için önerilmez. Bunun yerine SSL, ağ geçidinde etkindir. [Aktarım sırasında şifreleme](https://docs.microsoft.com/azure/hdinsight/domain-joined/encryption-in-transit) , [Internet Protokolü güvenliği (IPSec)](https://en.wikipedia.org/wiki/IPsec)kullanan küme düğümleri arasındaki iletişimleri şifrelemek için etkinleştirilebilir.


## <a name="next-steps"></a>Sonraki adımlar
* [HiveServer2 kimlik doğrulamasına genel bakış](https://cwiki.apache.org/confluence/display/Hive/Setting+up+HiveServer2#SettingUpHiveServer2-Authentication/SecurityConfiguration)
* [HiveServer2 yetkilendirmesi 'ne genel bakış](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Authorization#:~:text=%20Overview%20of%20Authorization%20Modes%20%201%201,and%20Apache%20Sentry%20are%20apache%20projects...%20More%20)
* [SQL standartlarına dayalı Hive yetkilendirmesini etkinleştirme](https://community.cloudera.com/t5/Community-Articles/Getting-started-with-SQLStdAuth/ta-p/244263)
* [Hive ile Apache Ranger](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-run-hive#:~:text=Create%20Hive%20ODBC%20data%20source%20%20%20,Enter%20hiveuser1%40contoso158.onmicrosoft.c%20...%20%205%20more%20rows%20)
