---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/06/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 266cd52117f36b282fdd4bc8615a15e451cc203f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80132639"
---
İşlevler, [Azure Portal]bir işlev uygulamasına Application Insights tümleştirme eklemeyi kolaylaştırır.

1. [Portal][Azure portalında], sayfanın üst kısmındaki `Function Apps` arama çubuğuna yazın, işlev uygulamanızı seçin ve ardından pencerenin üst kısmındaki **Application Insights yapılandırılmamış** başlık ' ı seçin. Bu başlığı görmüyorsanız, uygulamanız zaten Application Insights etkinleştirilmiştir.

    ![Portaldan Application Insights etkinleştirme](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Görüntünün altındaki tabloda belirtilen ayarları kullanarak bir Application Insights kaynağı oluşturun.

   ![Application Insights kaynağı oluşturma](media/functions-connect-new-app-insights/ai-general.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Adı** | Benzersiz uygulama adı | Kendi aboneliğinizde benzersiz olması gereken işlev uygulamanız ile aynı adı kullanmak en kolay yoldur. | 
    | **Konum** | Batı Avrupa | Mümkünse, işlev uygulamanız ile aynı [bölgeyi](https://azure.microsoft.com/regions/) veya bu bölgeye yakın olanı kullanın. |

1. **Tamam**’ı seçin. Application Insights kaynak, işlev uygulamanız ile aynı kaynak grubunda ve abonelikte oluşturulur. Kaynak oluşturulduktan sonra Application Insights penceresini kapatın.

1. İşlev uygulamanıza geri dönün, **uygulama ayarları**' nı seçin ve ardından **uygulama ayarları**' na kaydırın. Adlı `APPINSIGHTS_INSTRUMENTATIONKEY`bir ayar görürseniz, Azure 'da çalışan işlev uygulamanız için Application Insights tümleştirme etkinleştirilir.

[Azure portalı]: https://portal.azure.com
