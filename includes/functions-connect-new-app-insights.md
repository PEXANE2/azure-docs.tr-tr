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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132639"
---
İşlevler, [Azure portalındaki]bir işlev uygulamasına Uygulama Öngörüleri tümleştirmesini eklemeyi kolaylaştırır.

1. [PortalAzure]Portalı'nda, `Function Apps` sayfanın üst kısmındaki arama çubuğunu yazın, işlev uygulamanızı seçin ve ardından **Uygulama Öngörüleri'ni** seçin pencerenin üst kısmında yapılandırılan banner değildir.[Azure Portal] Bu banner'ı görmüyorsanız, uygulamanız zaten Uygulama Öngörüleri etkinleştirilmiştir.

    ![Portaldan Uygulama Öngörülerini Etkinleştir](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Resmin altındaki tabloda belirtilen ayarları kullanarak bir Uygulama Öngörüleri kaynağı oluşturun.

   ![Application Insights kaynağı oluşturma](media/functions-connect-new-app-insights/ai-general.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Adı** | Benzersiz uygulama adı | Aboneliğinizde benzersiz olması gereken işlev uygulamanızla aynı adı kullanmak en kolayıdır. | 
    | **Konum** | Batı Avrupa | Mümkünse, işlev uygulamanızla aynı [bölgeyi](https://azure.microsoft.com/regions/) veya o bölgeye yakın olan bölgeyi kullanın. |

1. **Tamam'ı**seçin. Application Insights kaynağı, işlev uygulamanızla aynı kaynak grubunda ve abonelikte oluşturulur. Kaynak oluşturulduktan sonra Uygulama Öngörüleri penceresini kapatın.

1. İşlev uygulamanızda Uygulama **ayarlarını**seçin ve ardından **Uygulama ayarlarına**gidin. Azure'da çalışan `APPINSIGHTS_INSTRUMENTATIONKEY`işlev uygulamanız için Application Insights entegrasyonu adlı bir ayar görürseniz etkinleştirilir.

[Azure Portalı]: https://portal.azure.com
