---
title: Azure Data Lake Analytics’te kotaları ve limitleri ayarlama
description: Azure Veri Gölü Analizi (ADLA) hesaplarında kotaları ve sınırları nasıl ayarlayıp artıracağınızı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: omidm1
ms.author: omidm
ms.reviewer: jasonwhowell
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: c1d7c8cc4b50682a74ac88b2113f0d44ebc55199
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75644724"
---
# <a name="adjust-quotas-and-limits-in-azure-data-lake-analytics"></a>Azure Data Lake Analytics’te kotaları ve limitleri ayarlama

Azure Veri Gölü Analizi (ADLA) hesaplarındaki kota ve sınırları nasıl ayarlayıp artıracağınızı öğrenin. Bu sınırları bilmek, U-SQL iş davranışınızı anlamanıza yardımcı olur. Azure desteğine başvurarak maksimum sınırları artırabileceğinizden, tüm kota sınırları yumuşaktir.

## <a name="azure-subscriptions-limits"></a>Azure abonelikleri sınırları

**Bölge başına abonelik başına maksimum ADLA hesabı sayısı:** 5

Altıncı bir ADLA hesabı oluşturmaya çalışırsanız, "Abonelik adı altında bölgede izin verilen en fazla Veri Gölü Analizi hesabı sayısına (5) ulaştınız" hatası alırsınız.

Bu sınırı geçmek istiyorsanız, şu seçenekleri deneyebilirsiniz:
* uygunsa başka bir bölge seçin
* kota artışı istemek için [bir destek bileti açarak](#increase-maximum-quota-limits) Azure desteğine başvurun.

## <a name="default-adla-account-limits"></a>Varsayılan ADLA hesap sınırları

**Hesap başına maksimum Analiz Birimi (AUs) sayısı:** 250, varsayılan 32

Bu, hesabınızda aynı anda çalışabilen maksimum A'sayısıdır. Tüm işlerde toplam çalışan AU'larınız bu sınırı aşarsa, yeni işler otomatik olarak sıraya alınır. Örnek:

* 32 U'yla çalışan tek bir işiniz varsa, ikinci bir iş gönderdiğinde ilk iş tamamlanana kadar iş kuyruğunda bekler.
* Halihazırda çalışan dört işiniz varsa ve her biri 8 ANA kullanıyorsa, 8 ANA'ya ihtiyaç duyduğu beşinci bir iş gönderdiğinde, 8 ANA'lar kullanılabilir olana kadar iş kuyruğunda bekler.

    ![Azure Veri Gölü Analizi sınırları ve kota sayfası](./media/data-lake-analytics-quota-limits/adjust-quota-limits.png)

**İş başına maksimum Analiz Birimi (AUs) sayısı:** 250, varsayılan 32

Bu, her bir işin hesabınıza atanabildiği maksimum AUs sayısıdır. Gönderen, iş başına daha fazla AUs veren bir işlem ilkesinden (iş gönderme sınırı) etkilenmediği sürece, bu sınırdan daha fazla atanan işler reddedilir. Bu değerin üst sınırı, hesap için AU sınırıdır.

**Hesap başına maksimum eşzamanlı U-SQL iş sayısı:** 20

Bu, hesabınızda aynı anda çalıştırılabilen maksimum iş sayısıdır. Bu değerin üzerinde, yeni işler otomatik olarak sıralanır.

## <a name="adjust-adla-account-limits"></a>ADLA hesap limitlerini ayarlama

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Varolan bir ADLA hesabı seçin.
3. **Özellikler**'e tıklayın.
4. **Maksimum US**değerlerini, maksimum çalışan **iş sayısını**ve **İş gönderim sınırlarını** ihtiyaçlarınıza göre ayarlayın.

## <a name="increase-maximum-quota-limits"></a>Maksimum kota limitlerini artırma

[Azure hizmetine özel limitbelgelerinde](../azure-resource-manager/management/azure-subscription-service-limits.md#data-lake-analytics-limits)Azure sınırları hakkında daha fazla bilgi bulabilirsiniz.

1. Azure portalında bir destek isteği açın.

    ![Azure Veri Gölü Analizi portal sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Azure Veri Gölü Analizi portal sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Sorun türü **Kotasını**seçin.
3. **Aboneliğinizi** seçin (bunun bir "deneme" aboneliği olmadığından emin olun).
4. Kota türü **Veri Gölü Analizi'ni**seçin.

    ![Azure Veri Gölü Analizi portal sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. Sorun sayfasında, bu ekstra kapasiteye neden ihtiyacınız olduğunu **ayrıntılarıyla** birlikte istediğiniz artış sınırını açıklayın.

    ![Azure Veri Gölü Analizi portal sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. Kişi bilgilerinizi doğrulayın ve destek isteği oluşturun.

Microsoft isteğinizi inceler ve iş ihtiyaçlarınızı en kısa sürede karşılamaya çalışır.

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme](data-lake-analytics-manage-use-powershell.md)
* [Azure portalLarını kullanarak Azure Veri Gölü Analizi işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
