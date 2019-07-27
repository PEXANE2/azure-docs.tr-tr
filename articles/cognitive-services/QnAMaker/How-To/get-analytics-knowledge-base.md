---
title: Bilgi Bankası 'nda analiz-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma, Soru-Cevap Oluşturma hizmetinizin oluşturulması sırasında Application Insights 'ı etkinleştirdiyseniz tüm sohbet günlüklerini ve diğer telemetrileri depolar. Sohbet günlüklerinizi App Insights 'tan almak için örnek sorguları çalıştırın.
services: cognitive-services
author: diberry
manager: nitinme
displayName: chat history, history, chat logs, logs
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: 5c729065076f5dc9f25189632f42ed565a72df8a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563098"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Bilgi bankanız hakkında analizler alma

Soru-Cevap Oluşturma, [soru-cevap oluşturma hizmetinizin oluşturulması](./set-up-qnamaker-service-azure.md)sırasında Application Insights 'ı etkinleştirdiyseniz tüm sohbet günlüklerini ve diğer telemetrileri depolar. Sohbet günlüklerinizi App Insights 'tan almak için örnek sorguları çalıştırın.

1. App Insights kaynağına gidin.

    ![Application Insights kaynağınızı seçin](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. **Analiz**' ı seçin. Soru-Cevap Oluşturma Telemetriyi Sorgulayabileceğiniz yeni bir pencere açılır.

    ![Analiz Seç](../media/qnamaker-how-to-analytics-kb/analytics.png)

3. Aşağıdaki sorguyu yapıştırın ve çalıştırın.

    ```query
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration, performanceBucket
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId
    ) on id
    | extend question = tostring(customDimensions['Question'])
    | extend answer = tostring(customDimensions['Answer'])
    | extend score = tostring(customDimensions['Score'])
    | project timestamp, resultCode, duration, id, question, answer, score, performanceBucket,KbId 
    ```

    Sorguyu çalıştırmak için **Çalıştır** ' ı seçin.

    ![Sorguyu çalıştır](../media/qnamaker-how-to-analytics-kb/run-query.png)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Soru-Cevap Oluşturma bilgi bankasında diğer analizler için sorgular çalıştırma

### <a name="total-90-day-traffic"></a>Toplam 90 günlük trafik

```query
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
```

### <a name="total-question-traffic-in-a-given-time-period"></a>Belirli bir dönemdeki toplam soru trafiği

```query
    //Total Question Traffic in a given time period
    let startDate = todatetime('2018-02-18');
    let endDate = todatetime('2018-03-12');
    requests
    | where timestamp <= endDate and timestamp >=startDate
    | where url endswith "generateAnswer" and name startswith "POST" 
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Kullanıcı trafiği

```query
    //User Traffic
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId 
    ) on id
    | extend UserId = tostring(customDimensions['UserId'])
    | summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Soruların gecikme dağılımı

```query
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Anahtarları Yönet](./key-management.md)
