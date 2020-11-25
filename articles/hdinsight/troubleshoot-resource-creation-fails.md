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
ms.openlocfilehash: 2e9f6898dba559f557f49cc0ee9ff39644da243f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96000700"
---
# <a name="troubleshoot-resource-creation-failures-in-azure-hdinsight"></a>Azure HDInsight 'ta kaynak oluşturma hatalarıyla ilgili sorunları giderme

Bu makalede, sorun giderme adımları ve Azure HDInsight ile etkileşim kurarken sorunlara yönelik olası çözümler açıklanmaktadır.

## <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>Hata: dağıtım ' 800 ' kotasını aşacak

Azure'da kaynak grubu başına 800 dağıtımlık kota sınırı vardır. Her kaynak grubu, abonelik, hesap veya başka kapsamlar için farklı kotalar uygulanır. Örneğin aboneliğiniz bir bölge için çekirdek sayısını sınırlandıracak şekilde yapılandırılmış olabilir. İzin verilenden daha fazla çekirdeğe sahip bir sanal makineyi, kotanın aşıldığını belirten hata iletisi ile dağıtma girişimi.

Bu sorunu çözmek için Azure portal, CLı veya PowerShell kullanarak artık gerekmeyen dağıtımları silin.

Daha fazla bilgi için bkz. [Kaynak kotaları hatalarını düzeltme](../azure-resource-manager/templates/error-resource-quota.md).

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

[!INCLUDE [troubleshooting next steps](../../includes/hdinsight-troubleshooting-next-steps.md)]