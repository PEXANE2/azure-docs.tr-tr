---
title: Azure Iletişim hizmeti için Ölçüm tanımları
titleSuffix: An Azure Communication Services concept document
description: Bu belge Azure portal bulunan ölçümlerin tanımlarını içerir.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 05/19/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: b8d74f86fc9c3d2cc020628f67c36d8d67973fff
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92128678"
---
# <a name="metrics-overview"></a>Ölçümlere genel bakış

Azure Iletişim Hizmetleri şu anda sohbet ve SMS için ölçümler sunmaktadır. [Azure Ölçüm Gezgini](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started) , kendi grafiklerinizi çizmek, ölçüm değerlerinizde yer alan anormallikleri araştırmak ve sohbet ve SMS isteklerinin yayılacağı ölçüm VERILERINI kullanarak API trafiğinizi anlamak için kullanılabilir.

## <a name="where-to-find-metrics"></a>Ölçümlerin nerede bulunacağı

Azure Iletişim Hizmetleri 'ndeki sohbet ve SMS Hizmetleri, API istekleri için ölçümleri yaymalıdır. Bu ölçümler Iletişim Hizmetleri kaynağınızın altındaki ölçümler dikey penceresinde bulunabilir. Ayrıca Iletişim Hizmetleri kaynağınızın altındaki çalışma kitapları dikey penceresini kullanarak kalıcı panolar oluşturabilirsiniz.

## <a name="metric-definitions"></a>Ölçüm tanımları

Iletişim Hizmetleri ölçümleri içinde temsil edilen iki tür istek vardır: **sohbet API istekleri** ve **SMS API istekleri**.

Hem sohbet hem de SMS API istek ölçümleri, ölçüm verilerinizi filtrelemek için kullanabileceğiniz üç boyut içerir. Bu boyutlar toplama türü kullanılarak birlikte toplanabilir ve,, `Count` ve dahil tüm standart Azure toplama zaman serisini destekler `Sum` `Average` `Min` `Max` .

Desteklenen toplama türleri ve zaman serisi toplamaları hakkında daha fazla bilgi için [Azure Ölçüm Gezgini gelişmiş özellikleri](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts#changing-aggregation) bulunabilir

- **İşlem** -ACS sohbet ağ geçidinde çağrılabilecek tüm işlemler veya rotalar.
- **Durum kodu** -istekten sonra gönderilen durum kodu yanıtı.
- **Statusalt sınıfı** -yanıttan sonra gönderilen durum kodu serisi. 


### <a name="chat-api-request-metric-operations"></a>Sohbet API isteği ölçüm işlemleri

Sohbet API 'SI istek ölçümlerinde aşağıdaki işlemler mevcuttur:

| İşlem/rota    | Açıklama                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------- |
| GetChatMessage       | İleti kimliğine göre bir ileti alır. |
| ListChatMessages     | Bir iş parçacığından gelen sohbet iletilerinin listesini alır. |
| SendChatMessage      | Bir iş parçacığına sohbet iletisi gönderir. |
| UpdateChatMessage    | Sohbet iletisini güncelleştirir. |
| DeleteChatMessage    | Bir sohbet iletisini siler. |
| GetChatThread        | Bir sohbet iş parçacığı alır. |
| ListChatThreads      | Bir kullanıcının sohbet iş parçacıklarının listesini alır. |
| UpdateChatThread     | Bir sohbet iş parçacığının özelliklerini güncelleştirir. |
| CreateChatThread     | Sohbet iş parçacığı oluşturur. |
| DeleteChatThread     | Bir iş parçacığını siler. |
| Getreadalındılar      | Bir iş parçacığının okundu bilgilerini alır. |
| Sendreadalındısı      | Bir kullanıcı adına bir iş parçacığına okundu bilgisi olayı gönderir. |
| Sendtypingındicator           | Bir kullanıcı adına bir iş parçacığına yazma olayı gönderir. |
| ListChatThreadParticipants    | Bir iş parçacığının üyelerini alır. |
| AddChatThreadParticipants     | İş parçacığına iş parçacığı üyeleri ekler. Üyeler zaten varsa, hiçbir değişiklik gerçekleşmez. |
| RemoveChatThreadParticipant   | Bir üyeyi iş parçacığından kaldırın. |

:::image type="content" source="./media/chat-metric.png" alt-text="Sohbet API Isteği ölçümü.&quot;:::

Tanınmayan bir işleme bir istek yapılırsa, &quot;Hatalı yol" değeri yanıtını alırsınız.

### <a name="sms-api-requests"></a>SMS API istekleri

SMS API istek ölçümlerinde aşağıdaki işlemler mevcuttur:

| İşlem/rota    | Açıklama                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------- |
| Smsiletiçi       | SMS iletisi gönderir. |
| Smsdeliveryreportsalındı     | SMS teslim raporlarını alır |
| Smsiletiiletileri alındı      | SMS iletilerini alır. |


:::image type="content" source="./media/sms-metric.png" alt-text="Sohbet API Isteği ölçümü.&quot;:::

Tanınmayan bir işleme bir istek yapılırsa, &quot;Hatalı yol":::

## <a name="next-steps"></a>Sonraki Adımlar

- [Veri platformu ölçümleri](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics) hakkında daha fazla bilgi edinin
