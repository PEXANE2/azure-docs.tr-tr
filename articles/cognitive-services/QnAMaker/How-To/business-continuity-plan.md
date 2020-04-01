---
title: İş sürekliliği planı - QnA Maker
titleSuffix: Azure Cognitive Services
description: İş sürekliliği planının birincil amacı, Bot veya onu tüketen uygulama için hiçbir kesinti süresi sağlayacak esnek bir bilgi tabanı bitiş noktası oluşturmaktır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: 2100426923418104c31933bf9d1406a6441d3841
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410921"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>QnA Maker hizmetiniz için bir iş sürekliliği planı oluşturun

İş sürekliliği planının birincil amacı, Bot veya onu tüketen uygulama için hiçbir kesinti süresi sağlayacak esnek bir bilgi tabanı bitiş noktası oluşturmaktır.

![QnA Maker bcp planı](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Yukarıda temsil edildiği gibi üst düzey fikir aşağıdaki gibidir:

1. [Azure eşleştirilmiş bölgelerde](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)iki paralel [QnA Maker hizmeti](../How-To/set-up-qnamaker-service-azure.md) ayarlayın.

2. Birincil ve ikincil Azure arama dizinlerini eşit tutun. Azure dizinlerini yedeklemek için github örneğini [buradan](https://github.com/pchoudhari/QnAMakerBackupRestore) kullanın.

3. [Sürekli dışa aktarmayı](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)kullanarak Uygulama Öngörülerini yedekle.

4. Birincil ve ikincil yığınlar ayarlandıktan sonra, iki uç noktayı yapılandırmak ve yönlendirme yöntemi ayarlamak için [trafik yöneticisini](https://docs.microsoft.com/azure/traffic-manager/) kullanın.

5. Trafik yöneticisi bitiş noktanız için daha önce Güvenli Soketkatmanı (SSL) olarak bilinen bir Aktarım Katmanı Güvenliği (TLS) oluşturmanız gerekir. [TLS/SSL sertifikasını](https://docs.microsoft.com/azure/app-service/configure-ssl-bindings) Uygulama hizmetlerinizde bağla.

6. Son olarak, Bot veya Uygulamanızdaki trafik yöneticisi bitiş noktasını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Capactiy seçin](./improve-knowledge-base.md)
