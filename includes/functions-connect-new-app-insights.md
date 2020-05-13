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
İşlevler, [Azure Portal]bir işlev uygulamasına Application Insights tümleştirme eklemeyi kolaylaştırır.

1. [Azure portalında] [Azure Portal], **işlev uygulaması**' nı arayıp seçin, sonra işlev uygulamanızı seçin. 

1. Pencerenin üst kısmında **Application Insights yapılandırılmamış** başlık ' ı seçin. Bu başlığı görmüyorsanız, uygulamanız zaten Application Insights etkinleştirilmiştir.

    :::image type="content" source="media/functions-connect-new-app-insights/enable-application-insights.png" alt-text="Portaldan Application Insights etkinleştirme":::

1. Görüntünün altındaki tabloda belirtilen ayarları kullanarak bir Application Insights kaynağı oluşturun.

   :::image type="content" source="media/functions-connect-new-app-insights/ai-general.png" alt-text="Application Insights kaynağı oluşturma":::

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Adı** | Benzersiz uygulama adı | Kendi aboneliğinizde benzersiz olması gereken işlev uygulamanız ile aynı adı kullanmak en kolay yoldur. | 
    | **Konum** | Batı Avrupa | Mümkünse, işlev uygulamanız ile aynı [bölgeyi](https://azure.microsoft.com/regions/) veya bu bölgeye yakın olanı kullanın. |

1. **Uygula**’yı seçin. Application Insights kaynak, işlev uygulamanız ile aynı kaynak grubunda ve abonelikte oluşturulur. Kaynak oluşturulduktan sonra Application Insights penceresini kapatın.

1. İşlev uygulamanıza geri dönün, **Ayarlar**  >  **yapılandırması**' nı seçin ve ardından **uygulama ayarları**' nı seçin. Adlı bir ayar görürseniz `APPINSIGHTS_INSTRUMENTATIONKEY` , Azure 'da çalışan işlev uygulamanız için Application Insights tümleştirme etkinleştirilir.

[Azure portalı]: https://portal.azure.com
