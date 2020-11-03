---
title: Azure HDInsight kümesinde oturum açılamıyor
description: Azure HDInsight 'ta neden Apache Hadoop kümesinde oturum geçemedik sorunlarını giderme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 222c48ab2b9b718bc373e710f37f906e68bd31d4
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93289015"
---
# <a name="scenario-unable-to-log-into-azure-hdinsight-cluster"></a>Senaryo: Azure HDInsight kümesinde oturum açılamıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Azure HDInsight kümesinde oturum açılamıyor.

## <a name="cause"></a>Nedeni

Nedenler farklılık gösterebilir. Küme veya uygulama panolarında oturum açarken, "küme oturum açma" veya HTTP kimlik bilgilerinizi kullanmayı unutmayın. Uzaktan bağlanırken Secure Shell (SSH) veya Uzak Masaüstü kimlik bilgilerinizi kullanın.

## <a name="resolution"></a>Çözüm

Sık karşılaşılan sorunları çözmek için aşağıdaki adımlardan birini veya daha fazlasını deneyin.

* Küme panosunu gizlilik modundaki yeni bir tarayıcıda açmayı deneyin.

* SSH kimlik bilgilerinizi hatırlayamıyorsanız, [ambarı Kullanıcı arabirimindeki kimlik bilgilerini sıfırlayabilirsiniz](../hdinsight-administer-use-portal-linux.md#change-passwords).

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]