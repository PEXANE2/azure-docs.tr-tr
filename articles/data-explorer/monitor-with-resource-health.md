---
title: Kaynak Durumu'nun kullanımını kullanarak Azure Veri Gezginini Izleme
description: Azure Veri Gezgini kaynaklarını izlemek için Azure Kaynak Durumu'nun kullanımını kullanın.
author: orspod
ms.author: orspodek
ms.reviewer: prvavill
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/31/2020
ms.openlocfilehash: 9b8b1b40e972d7719ef2cf495ed12cb4ed3478fd
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80479376"
---
# <a name="monitor-azure-data-explorer-using-resource-health-preview"></a>Kaynak Durumu 'ni kullanarak Azure Veri Gezgini'ni izleme (Önizleme)

Azure Veri Gezgini için [Kaynak Durumu,](/azure/service-health/resource-health-overview) Azure Veri Gezgini kaynağınızın sistem durumu hakkında sizi bilgilendirir ve sorunları gidermek için uygulanabilir öneriler sağlar. Kaynak durumu her 1-2 dakikada bir güncellenir ve kaynaklarınızın geçerli ve geçmiş durumunu bildirir. 

Kaynak Durumu, aşağıdakiler gibi çeşitli sistem durumu durum denetimlerini inceleyerek Azure Veri Gezgini kaynağınızın durumunu belirler:
* Kaynak kullanılabilirliği
* Sorgu hataları

## <a name="access-resource-health-reporting"></a>Access Kaynak Durumu raporlaması

1. Azure [portalında,](https://portal.azure.com/)hizmetler listesinden **Monitör'ü** seçin.
1. **Hizmet Durumu** > **Kaynak durumunu**seçin.
1. **Abonelik** açılır düşüşünde aboneliğinizi seçin. Kaynak türü açılır düşüşünde **Azure Veri Gezgini'ni**seçin. **Resource type**
1. Ortaya çıkan tablo, seçilen abonelikteki tüm kaynakları listeler. Her kaynağın, kaynak durumunu gösteren bir sistem durumu simgesi olacaktır.
1. Kaynak durumu durumunu ve önerilerini görüntülemek için [kaynağınızı](#resource-health-status) seçin.

    ![Genel Bakış](media/monitor-with-resource-health/resource-health-overview.png)

## <a name="resource-health-status"></a>Kaynak durumu durumu

Bir kaynağın durumu, kullanılabilir, kullanılamayan ve bilinmeyen aşağıdaki durumlardan biriyle görüntülenir.

### <a name="available"></a>Kullanılabilir

**Kullanılabilir'in** sistem durumu, Azure Veri Gezgini kaynağınızın sağlıklı olduğunu ve herhangi bir sorunu olmadığını gösterir.

![Kullanılabilir](media/monitor-with-resource-health/available.png)

### <a name="unavailable"></a>Kullanılamaz

**Kullanılamayan'ın** sistem durumu, Azure Veri Gezgini kaynağınızda sorgular ve alım için kullanılamamasına neden olan devam eden bir sorun olduğunu gösterir. Örneğin, Azure Veri Gezgini kaynağınızdaki düğümler beklenmedik bir şekilde yeniden başlatılmış olabilir. Azure Veri Gezgini kaynağınız bu durumda uzun bir süre kalırsa, [desteğe]()başvurun.

![Kullanılamaz](media/monitor-with-resource-health/unavailable.png)

### <a name="unknown"></a>Bilinmiyor

**Bilinmeyenin** sistem durumu, **Kaynak Durumu'nun** bu Azure Veri Gezgini kaynağı hakkında 10 dakikadan uzun süredir bilgi almadığını gösterir. Bu durum, Azure Veri Gezgini kaynak durumunun kesin bir göstergesi değildir, ancak sorun giderme işleminde önemli bir veri noktasıdır. Azure Veri Gezgini kümeniz beklendiği gibi çalışıyorsa, durum birkaç dakika içinde **Kullanılabilir** olarak değişir. **Bilinmeyen** sistem durumu, platformdaki bir olayın kaynağı etkilediğini düşündürebilir. 

> [!TIP]
> Azure Veri Gezgini küme durumu, durdurulmuş durumdaysa **bilinmeyen** olur.

![Bilinmiyor](media/monitor-with-resource-health/unknown.png)

## <a name="historical-information"></a>Tarihsel bilgiler

**Kaynak Durumu** bölmesi > **Sağlık geçmişi,** kaynak durumu durumu bilgileri dört haftaya kadar erişin. Bu bölmede bildirilen sistem durumu olayı sorunları hakkında ek bilgi için oku seçin. 

![Geçmiş](media/monitor-with-resource-health/healthhistory.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak Durumu uyarılarını yapılandırma](https://docs.microsoft.com/azure/service-health/resource-health-alert-arm-template-guide)
* [Öğretici: Azure Veri Gezgini'nde izleme verilerini alma ve sorgulama](ingest-data-no-code.md)
* [Azure Veri Gezgini performansını, sistem durumunu ve kullanımını ölçümlerle izleme](using-metrics.md)