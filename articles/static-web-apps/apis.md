---
title: Azure Işlevleri ile Azure statik Web Apps API desteği
description: Azure statik Web Apps 'nin desteklediği API özelliklerini öğrenin
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 7f86ce9158b5b07b036c785c2973e8a5883ed686
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597145"
---
# <a name="api-support-in-azure-static-web-apps-preview-with-azure-functions"></a>Azure Işlevleri ile Azure statik Web Apps önizlemede API desteği

Azure statik Web Apps, [Azure işlevleri](../azure-functions/functions-overview.md)aracılığıyla SUNUCUSUZ API uç noktaları sağlar. Azure Işlevleri 'ni kullanarak API 'Lerin talebe göre dinamik olarak ölçeklendirilmesi ve aşağıdaki özellikleri içermesi gerekir:

- Kullanıcı [kimlik doğrulamasına ve rol tabanlı yetkilendirme](user-information.md) verilerine doğrudan erişimli **Tümleşik güvenlik** .
- _API_ yolunu özel CORS kuralları gerekmeden güvenli bir şekilde Web uygulaması için kullanılabilir hale getiren **sorunsuz yönlendirme** .
- **Azure işlevleri** v3 ve Node. js 12 ile uyumludur.
- **Http Tetikleyicileri** ve çıkış bağlamaları.

## <a name="configuration"></a>Yapılandırma

API uç noktaları, _API_ rotası aracılığıyla Web uygulaması tarafından kullanılabilir. Bu yol düzeltilirken, ilişkili Azure Işlevleri uygulamasını nerede bulabileceğiniz klasörü üzerinde denetiminiz vardır. Bu konumu, deponuzın _. GitHub/iş akışları_ klasöründe bulunan [Iş akışı YAML dosyasını düzenleyerek](github-actions-workflow.md#build-and-deploy) değiştirebilirsiniz.

## <a name="constraints"></a>Kısıtlamalar

Azure statik Web Apps, Azure Işlevleri aracılığıyla bir API sağlar. Azure Işlevlerinin özellikleri, Web uygulaması için bir API oluşturmanıza ve Web uygulamasının API 'ye güvenli bir şekilde bağlanmasına olanak tanıyan belirli bir özellik kümesine odaklanır. Bu özellikler aşağıdakiler dahil bazı kısıtlamalarla gelir:

- API yolu ön eki, _API_olmalıdır.
- Tetikleyiciler ve bağlamalar [http](../azure-functions/functions-bindings-http-webhook.md)ile sınırlıdır.
  - Çıkış bağlamaları hariç tüm diğer [Azure işlevleri Tetikleyicileri ve bağlamaları](../azure-functions/functions-triggers-bindings.md#supported-bindings) kısıtlıdır.
- Günlükler yalnızca Işlevler uygulamanıza [Application Insights](../azure-functions/functions-monitoring.md) eklerseniz kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API ekleme](add-api.md)
