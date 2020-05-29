---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/09/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3289ba03d0f613d004bc8bff4dbcf2bd434f3da3
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83121600"
---
İşlevler, [Azure Portalı] bir işlev uygulamasına Application Insights tümleştirmesini eklemeyi kolaylaştırır.

1. [Azure portalında][azure portalı] **işlev uygulamasını** arayıp seçin ve sonra işlev uygulamanızı belirleyin. 

1. Pencerenin üst kısmındaki **Application Insights yapılandırılmadı** başlığını seçin. Bu başlığı görmüyorsanız uygulamanızda zaten Application Insights etkinleştirilmiştir.

    :::image type="content" source="media/functions-connect-new-app-insights/enable-application-insights.png" alt-text="Application Insights’ı portaldan etkinleştirme":::

1. Görüntünün altındaki tabloda belirtilen ayarları kullanarak bir Application Insights kaynağı oluşturun.

   :::image type="content" source="media/functions-connect-new-app-insights/ai-general.png" alt-text="Application Insights kaynağı oluşturma":::

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Ad** | Benzersiz uygulama adı | Aboneliğinizde benzersiz olması gereken işlev uygulamanızın adıyla aynı adı kullanmak en kolayıdır. | 
    | **Konum** | Batı Avrupa | Mümkünse işlev uygulamanızla aynı [bölgeyi](https://azure.microsoft.com/regions/) veya bu bölgeye yakın bir bölgeyi kullanın. |

1. **Uygula**’yı seçin. Application Insights kaynağı, işlev uygulamanızla aynı kaynak grubunda ve abonelikte oluşturulur. Kaynak oluşturulduktan sonra Application Insights penceresini kapatın.

1. İşlev uygulamanıza dönüp **Ayarlar** > **Yapılandırma**’yı seçin ve sonra **Uygulama ayarları** seçeneğini belirleyin. `APPINSIGHTS_INSTRUMENTATIONKEY` adlı bir ayar görürseniz Application Insights tümleştirmesi Azure’da çalışan işlev uygulamanız için etkinleştirilmiş demektir.

[Azure Portalı]: https://portal.azure.com
