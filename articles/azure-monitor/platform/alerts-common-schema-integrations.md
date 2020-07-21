---
title: Ortak uyarı şemasını Logic Apps ile tümleştirme
description: Tüm uyarılarınızı işlemek için ortak uyarı şemasıyla yararlanan bir mantıksal uygulama oluşturmayı öğrenin.
ms.topic: conceptual
ms.subservice: alerts
ms.date: 05/27/2019
ms.openlocfilehash: 05349554f4c5e076562a75d48d58e0849986d6cc
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86539508"
---
# <a name="how-to-integrate-the-common-alert-schema-with-logic-apps"></a>Ortak uyarı şemasını Logic Apps ile tümleştirme

Bu makalede, tüm uyarılarınızı işlemek için ortak uyarı şemasının kullanıldığı bir mantıksal uygulamanın nasıl oluşturulacağı gösterilmektedir.

## <a name="overview"></a>Genel Bakış

[Ortak uyarı şeması](https://aka.ms/commonAlertSchemaDocs) , tüm farklı uyarı türlerinizin genelinde standartlaştırılmış ve GENIŞLETILEBILIR bir JSON şeması sağlar. Ortak uyarı şeması, yararlanılabilir programlı olarak Web kancaları, runbook 'lar ve Logic Apps aracılığıyla yararlıdır. Bu makalede, tüm uyarılarınızı işlemek için tek bir Logic App 'in nasıl yazıldığı gösterilmektedir. Aynı ilkeler diğer programlama yöntemlerine de uygulanabilir. Bu makalede açıklanan mantıksal uygulama, [' Essential ' alanları](alerts-common-schema-definitions.md#essentials)için iyi tanımlanmış değişkenler oluşturur ve ayrıca [Uyarı türüne](alerts-common-schema-definitions.md#alert-context) özgü mantığı nasıl işleyebileceğinizi açıklar.


## <a name="prerequisites"></a>Önkoşullar 

Bu makalede, okuyucunun öğrenildiği varsayılmaktadır 
* Uyarı kurallarını ayarlama ([ölçüm](./alerts-metric.md), [günlük](./alerts-log.md), [etkinlik günlüğü](./alerts-activity-log.md))
* [Eylem gruplarını](./action-groups.md) ayarlama
* Eylem gruplarının içinden [ortak uyarı şemasının](./alerts-common-schema.md#how-do-i-enable-the-common-alert-schema) etkinleştirilmesi

## <a name="create-a-logic-app-leveraging-the-common-alert-schema"></a>Ortak uyarı şemasından yararlanan bir mantıksal uygulama oluşturma

1. [Mantıksal uygulamanızı oluşturmak için özetlenen adımları](./action-groups-logic-app.md)izleyin. 

1.  Tetikleyiciyi seçin: **BIR http isteği alındığında**.

    ![Mantıksal uygulama tetikleyicileri](media/action-groups-logic-app/logic-app-triggers.png "Mantıksal uygulama tetikleyicileri")

1.  HTTP istek tetikleyicisini değiştirmek için **Düzenle** ' yi seçin.

    ![HTTP istek Tetikleyicileri](media/action-groups-logic-app/http-request-trigger-shape.png "HTTP istek Tetikleyicileri")


1.  Aşağıdaki şemayı kopyalayıp yapıştırın:

    ```json
        {
            "type": "object",
            "properties": {
                "schemaId": {
                    "type": "string"
                },
                "data": {
                    "type": "object",
                    "properties": {
                        "essentials": {
                            "type": "object",
                            "properties": {
                                "alertId": {
                                    "type": "string"
                                },
                                "alertRule": {
                                    "type": "string"
                                },
                                "severity": {
                                    "type": "string"
                                },
                                "signalType": {
                                    "type": "string"
                                },
                                "monitorCondition": {
                                    "type": "string"
                                },
                                "monitoringService": {
                                    "type": "string"
                                },
                                "alertTargetIDs": {
                                    "type": "array",
                                    "items": {
                                        "type": "string"
                                    }
                                },
                                "originAlertId": {
                                    "type": "string"
                                },
                                "firedDateTime": {
                                    "type": "string"
                                },
                                "resolvedDateTime": {
                                    "type": "string"
                                },
                                "description": {
                                    "type": "string"
                                },
                                "essentialsVersion": {
                                    "type": "string"
                                },
                                "alertContextVersion": {
                                    "type": "string"
                                }
                            }
                        },
                        "alertContext": {
                            "type": "object",
                            "properties": {}
                        }
                    }
                }
            }
        }
    ```

1. **+** **Yeni adım** ' ı ve ardından **Eylem Ekle**' yi seçin.

    ![Eylem ekleme](media/action-groups-logic-app/add-action.png "Eylem ekleme")

1. Bu aşamada, belirli iş gereksinimlerinize göre çeşitli bağlayıcılar (Microsoft ekipleri, bolluk, Salesforce vb.) ekleyebilirsiniz. ' Temel alanlar ' kutusunu kullanabilirsiniz. 

    ![Gerekli alanlar](media/alerts-common-schema-integrations/logic-app-essential-fields.png "Gerekli alanlar")
    
    Alternatif olarak, ' Ifade ' seçeneğini kullanarak uyarı türüne göre koşullu mantık yazabilirsiniz.

    ![Mantıksal uygulama ifadesi](media/alerts-common-schema-integrations/logic-app-expressions.png "Mantıksal uygulama ifadesi")
    
     [' Monitoringservice ' alanı](alerts-common-schema-definitions.md#alert-context) , koşullu mantığı oluşturabileceğiniz uyarı türünü benzersiz olarak tanımlamanızı sağlar.

    
    Örneğin, aşağıdaki kod parçacığı uyarının Application Insights tabanlı bir günlük uyarısı olup olmadığını denetler ve bu durumda arama sonuçları yazdırılır. Aksi takdirde, ' NA ' yazdırır.

    ```text
      if(equals(triggerBody()?['data']?['essentials']?['monitoringService'],'Application Insights'),triggerBody()?['data']?['alertContext']?['SearchResults'],'NA')
    ```
    
     [Mantıksal uygulama ifadeleri yazma](../../logic-apps/workflow-definition-language-functions-reference.md#logical-comparison-functions)hakkında daha fazla bilgi edinin.

    


## <a name="next-steps"></a>Sonraki adımlar

* [Eylem grupları hakkında daha fazla bilgi edinin](../../azure-monitor/platform/action-groups.md).
* [Ortak uyarı şeması hakkında daha fazla bilgi edinin](https://aka.ms/commonAlertSchemaDocs).
