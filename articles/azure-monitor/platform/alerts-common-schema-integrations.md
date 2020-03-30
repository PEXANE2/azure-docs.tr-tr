---
title: Ortak uyarı şeması Mantık Uygulamaları ile nasıl entegre edilir?
description: Tüm uyarılarınızı işlemek için ortak uyarı şemasından yararlanan bir mantık uygulaması oluşturmayı öğrenin.
ms.topic: conceptual
ms.subservice: alerts
ms.date: 05/27/2019
ms.openlocfilehash: 9042ed8ddbb698192e638fa7538f74561574c262
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77668239"
---
# <a name="how-to-integrate-the-common-alert-schema-with-logic-apps"></a>Ortak uyarı şeması Mantık Uygulamaları ile nasıl entegre edilir?

Bu makalede, tüm uyarıları işlemek için ortak uyarı şema kaldıraçları bir mantık uygulaması oluşturmak için nasıl gösterir.

## <a name="overview"></a>Genel Bakış

[Ortak uyarı şeması,](https://aka.ms/commonAlertSchemaDocs) tüm farklı uyarı türlerinizde standartlaştırılmış ve genişletilebilir bir JSON şeması sağlar. Ortak uyarı şeması, webhook'lar, runbook'lar ve mantık uygulamaları aracılığıyla programlı olarak yararlanıldığında en kullanışlıdır. Bu makalede, tüm uyarılarınızı işlemek için tek bir mantık uygulamasının nasıl yazılacağını gösteriyoruz. Aynı ilkeler diğer programlı yöntemlere de uygulanabilir. Bu makalede açıklanan mantık uygulaması [, 'temel' alanlar](alerts-common-schema-definitions.md#essentials)için iyi tanımlanmış değişkenler oluşturur ve ayrıca [uyarı türüne](alerts-common-schema-definitions.md#alert-context) özgü mantığı nasıl işleyeceğinizaçıklanır.


## <a name="prerequisites"></a>Ön koşullar 

Bu makalede, okuyucu aşina olduğunu varsayar 
* Uyarı kurallarının ayarlanması ([metrik](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric), [günlük](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log), [etkinlik günlüğü](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log))
* Eylem [grupları](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) ayarlama
* Eylem grupları içinden [ortak uyarı şemasını](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema#how-do-i-enable-the-common-alert-schema) etkinleştirme

## <a name="create-a-logic-app-leveraging-the-common-alert-schema"></a>Ortak uyarı şemasını bir mantık uygulaması oluşturma

1. Mantık [uygulamanızı oluşturmak için ana hatlarıyla açıklanan adımları](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups-logic-app)izleyin. 

1.  Tetikleyiciyi seçin: **Bir HTTP isteği aldığında.**

    ![Mantıksal uygulama tetikleyicileri](media/action-groups-logic-app/logic-app-triggers.png "Mantıksal uygulama tetikleyicileri")

1.  HTTP isteği tetikleyicisini değiştirmek için **Edit'i** seçin.

    ![HTTP isteği tetikler](media/action-groups-logic-app/http-request-trigger-shape.png "HTTP isteği tetikler")


1.  Aşağıdaki şeayı kopyalayıp yapıştırın:

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

1. **+** **Yeni adım'ı** seçin ve ardından **eylem ekle'yi**seçin.

    ![Eylem ekleme](media/action-groups-logic-app/add-action.png "Eylem ekleme")

1. Bu aşamada, özel iş gereksinimlerinize bağlı olarak çeşitli bağlayıcılar (Microsoft Ekipleri, Bolluk, Satış Gücü, vb.) ekleyebilirsiniz. 'Temel alanlar'ı kutudan dışarı kullanabilirsiniz. 

    ![Temel alanlar](media/alerts-common-schema-integrations/logic-app-essential-fields.png "Temel alanlar")
    
    Alternatif olarak, 'İfade' seçeneğini kullanarak uyarı türüne göre koşullu mantık yazabilirsiniz.

    ![Mantık uygulaması ifadesi](media/alerts-common-schema-integrations/logic-app-expressions.png "Mantık uygulaması ifadesi")
    
     ['Izleme Hizmeti' alanı,](alerts-common-schema-definitions.md#alert-context) koşullu mantığı oluşturabileceğiniz uyarı türünü benzersiz olarak belirlemenize olanak tanır.

    
    Örneğin, aşağıdaki parçacık, uyarının Application Insights tabanlı bir günlük uyarısı olup olmadığını ve varsa arama sonuçlarını yazdırıp yazdırdığını denetler. Aksi takdirde, 'NA' yazdırır.

    ```text
      if(equals(triggerBody()?['data']?['essentials']?['monitoringService'],'Application Insights'),triggerBody()?['data']?['alertContext']?['SearchResults'],'NA')
    ```
    
     [Mantık uygulaması ifadeleri yazma](https://docs.microsoft.com/azure/logic-apps/workflow-definition-language-functions-reference#logical-comparison-functions)hakkında daha fazla bilgi edinin.

    


## <a name="next-steps"></a>Sonraki adımlar

* [Eylem grupları hakkında daha fazla bilgi edinin.](../../azure-monitor/platform/action-groups.md)
* [Ortak uyarı şeması hakkında daha fazla bilgi edinin.](https://aka.ms/commonAlertSchemaDocs)

