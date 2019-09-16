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
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: diberry
ms.openlocfilehash: 5c55084a57e46931049841f5011941b2115e9e69
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70961524"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Bilgi bankanız hakkında analizler alma

Soru-Cevap Oluşturma, [soru-cevap oluşturma hizmetinizin oluşturulması](./set-up-qnamaker-service-azure.md)sırasında Application Insights 'ı etkinleştirdiyseniz tüm sohbet günlüklerini ve diğer telemetrileri depolar. Sohbet günlüklerinizi App Insights 'tan almak için örnek sorguları çalıştırın.

1. App Insights kaynağına gidin.

    ![Application Insights kaynağınızı seçin](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. **Günlük (Analiz)** öğesini seçin. Soru-Cevap Oluşturma Telemetriyi Sorgulayabileceğiniz yeni bir pencere açılır.

3. Aşağıdaki sorguyu yapıştırın ve çalıştırın.

    ```kusto
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

    [![Kullanıcılardan gelen soruları, yanıtları ve puanı öğrenmek için sorguyu çalıştırın](../media/qnamaker-how-to-analytics-kb/run-query.png)](../media/qnamaker-how-to-analytics-kb/run-query.png#lightbox)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Soru-Cevap Oluşturma bilgi bankasında diğer analizler için sorgular çalıştırma

### <a name="total-90-day-traffic"></a>Toplam 90 günlük trafik

```kusto
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
```

### <a name="total-question-traffic-in-a-given-time-period"></a>Belirli bir dönemdeki toplam soru trafiği

```kusto
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

```kusto
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

```kusto
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Capactiy seçin](../tutorials/choosing-capacity-qnamaker-deployment.md)
