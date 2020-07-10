---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/10/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 87af50c5b5e5b69fd175ac4a570c4b6f659b97e6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84731051"
---
Azure Işlevleri, [Azure Portal]bir işlev uygulamasına Application Insights tümleştirme eklemeyi kolaylaştırır.

1. [Azure portalında] [Azure Portal], **işlev uygulaması**' nı arayıp seçin, sonra işlev uygulamanızı seçin. 

1. Pencerenin üst kısmında **Application Insights yapılandırılmamış** başlık ' ı seçin. Bu başlığı görmüyorsanız, uygulamanız zaten Application Insights etkinleştirilmiş olabilir.

    :::image type="content" source="media/functions-connect-new-app-insights/enable-application-insights.png" alt-text="Portaldan Application Insights etkinleştirme":::

1. Aşağıdaki tabloda belirtilen ayarları kullanarak **kaynağınızın değiştirme** ve bir Application Insights kaynağı oluşturma ' yı genişletin.  

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Yeni kaynak adı** | Benzersiz uygulama adı | Kendi aboneliğinizde benzersiz olması gereken işlev uygulamanız ile aynı adı kullanmak en kolay yoldur. | 
    | **Konum** | West Europe | Mümkünse, işlev uygulamanız ile aynı [bölgeyi](https://azure.microsoft.com/regions/) veya bu bölgeye yakın olanı kullanın. |

    :::image type="content" source="media/functions-connect-new-app-insights/ai-general.png" alt-text="Application Insights kaynağı oluşturma":::

1. **Uygula**’yı seçin. 

   Application Insights kaynak, işlev uygulamanız ile aynı kaynak grubunda ve abonelikte oluşturulur. Kaynak oluşturulduktan sonra Application Insights penceresini kapatın.

1. İşlev uygulamanızda **Ayarlar**' ın altında **yapılandırma** ' yı seçin ve ardından **uygulama ayarları**' nı seçin. Adlı bir ayar görürseniz `APPINSIGHTS_INSTRUMENTATIONKEY` , Azure 'da çalışan işlev uygulamanız için Application Insights tümleştirme etkinleştirilir.

[Azure Portalı]: https://portal.azure.com
