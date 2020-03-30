---
title: include dosyası
description: include dosyası
services: functions
author: jeffhollan
ms.service: azure-functions
ms.topic: include
ms.date: 10/25/2019
ms.author: jehollan, glenga
ms.custom: include file
ms.openlocfilehash: fca8c3b0b1bd4d22720a6d15313e297d05b7fac9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76021254"
---
1. Azure portalını[https://portal.azure.com](https://portal.azure.com)

1. Kaynak **Oluştur** düğmesini seçin

    ![Kaynak oluşturma](./media/functions-create-function-app-portal/function-app-create-resource.png)

1. **İşlem** > **Fonksiyonu Uygulamasını**seçin.

    ![Azure portalında işlev uygulaması oluşturma](./media/functions-premium-create/function-app-create-start.png)

1. Görüntünün altındaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    ![Temel Bilgiler](./media/functions-premium-create/function-app-create-basics.png)

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/management/overview.md)** |  *myResourceGroup* | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. |
    | **İşlev Uygulaması adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler `a-z` (büyük/küçük `0-9`harf `-`duyarsız) ve .  |
    |**Yayımlamak**| Kod | Kod dosyalarını veya Docker kapsayıcısını yayımlama seçeneği. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. C# ve F# için **.NET** işlevlerini seçin. |
    |**Bölge**| Tercih edilen bölge | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |

    **Sonraki'** ni seçin : Barındırma >düğmesini seçin.

1. Aşağıdaki barındırma ayarlarını girin.

    ![Barındırma](./media/functions-premium-create/function-app-premium-create-hosting.png)

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **[Depolama hesabı](../articles/storage/common/storage-account-create.md)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Depolama [hesabı gereksinimlerini](../articles/azure-functions/functions-scale.md#storage-account-requirements)karşılaması gereken varolan bir hesabı da kullanabilirsiniz. |
    |**İşletim sistemi**| Tercih edilen işletim sistemi | Çalışma zamanı yığını seçiminize göre bir işletim sistemi sizin için önceden seçilir, ancak gerekirse ayarı değiştirebilirsiniz. |
    | **[Planlama](../articles/azure-functions/functions-scale.md)** | Premium | Plan Türü için **Premium (Önizleme) seçeneğini** belirleyin ve Windows *Planı* ve *Sku ve boyut* seçimleri için varsayılanları seçin. |

    Sonraki **: İzleme >** düğmesini seçin.

1. Aşağıdaki izleme ayarlarını girin.

    ![İzleme](./media/functions-create-function-app-portal/function-app-create-monitoring.png)

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **[Uygulama Öngörüleri](../articles/azure-functions/functions-monitoring.md)** | Varsayılan | Desteklenen en yakın bölgede aynı *Uygulama adının* Uygulama Öngörüleri kaynağını oluşturur. Bu ayarı genişleterek, **Yeni kaynak adını** değiştirebilir veya verilerinizi depolamak istediğiniz Bir Azure [coğrafyasında](https://azure.microsoft.com/global-infrastructure/geographies/) farklı bir **Konum** seçebilirsiniz. |

    Uygulama yapılandırma seçimlerini incelemek için **Gözden Geçir + Oluştur'u** seçin.

1. İşlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'u seçin.