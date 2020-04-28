---
title: Azure HDInsight 'ta kaynak oluşturma hatalarıyla ilgili sorunları giderme
description: Genel kapasite sorunu hataları ve risk azaltma teknikleri Bu makalede verilmiştir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: seoapr2020
ms.date: 04/22/2020
ms.openlocfilehash: 527d2d8cb8086ed6b5e87417e2bc80dd52aa6e63
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82188421"
---
# <a name="troubleshoot-resource-creation-failures-in-azure-hdinsight"></a>Azure HDInsight 'ta kaynak oluşturma hatalarıyla ilgili sorunları giderme

Bu makalede, sorun giderme adımları ve Azure HDInsight ile etkileşim kurarken sorunlara yönelik olası çözümler açıklanmaktadır.

## <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>Hata: dağıtım ' 800 ' kotasını aşacak

Azure'da kaynak grubu başına 800 dağıtımlık kota sınırı vardır. Her kaynak grubu, abonelik, hesap veya başka kapsamlar için farklı kotalar uygulanır. Örneğin aboneliğiniz bir bölge için çekirdek sayısını sınırlandıracak şekilde yapılandırılmış olabilir. İzin verilenden daha fazla çekirdeğe sahip bir sanal makineyi, kotanın aşıldığını belirten hata iletisi ile dağıtma girişimi.

Bu sorunu çözmek için Azure portal, CLı veya PowerShell kullanarak artık gerekmeyen dağıtımları silin.

Daha fazla bilgi için bkz. [Kaynak kotaları hatalarını düzeltme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

## <a name="error-the-maximum-node-exceeded-the-available-cores-in-this-region"></a>Hata: en fazla düğüm, bu bölgedeki kullanılabilir çekirdekleri aştı

Aboneliğiniz bir bölge için çekirdek sayısını sınırlandıracak şekilde yapılandırılmış olabilir. Kaynağın izin verilenden daha fazla çekirdeğe sahip bir kaynağı dağıtmaya çalışırken, kotanın aşıldığını belirten hata iletisi.

Kota artırma isteğinde bulunmak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com)gidin ve **Yardım + Destek**' i seçin.

1. **Yeni destek isteği**’ni seçin.

1. **Yeni destek isteği** sayfasının **temel bilgiler** sekmesinde, aşağıdaki bilgileri sağlayın:

   * **Sorun türü:** **Hizmet ve abonelik sınırlarını (kotalar)** seçin.
   * **Abonelik:** Değiştirmek istediğiniz aboneliği seçin.
   * **Kota türü:** **HDInsight**' ı seçin.

Daha fazla bilgi için bkz. [Çekirdeği artırmak için destek bileti oluşturma](hdinsight-capacity-planning.md#quotas).

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
