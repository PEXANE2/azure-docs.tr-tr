---
title: Azure Izleyici sorun giderme günlükleri (Önizleme)
description: Azure Izleyici 'yi kullanarak sorunları hızlıca, düzenli aralıklarla araştırın, kod veya yapılandırma sorunlarını giderin veya destek çalışmalarını, genellikle belirli Öngörüler için yüksek hacimlere göre arayarak kullanırlar.
author: osalzberg
ms.author: bwren
ms.reviewer: bwren
ms.subservice: logs
ms.topic: conceptual
ms.date: 12/29/2020
ms.openlocfilehash: 816cdddc1f3d0a9bc9ebc3f277bc223a688cba31
ms.sourcegitcommit: 2488894b8ece49d493399d2ed7c98d29b53a5599
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98067374"
---
# <a name="azure-monitor-troubleshooting-logs-preview"></a>Azure Izleyici sorun giderme günlükleri (Önizleme)
Azure Izleyici 'yi kullanarak sorunları hızlı ve/veya düzenli aralıklarla araştırın, kod veya yapılandırma sorunlarını giderin veya destek çalışmalarını, genellikle belirli Öngörüler için yüksek hacimlere yönelik olarak arayarak yararlanır.

>[!NOTE]
> * Sorun giderme günlükleri önizleme modundadır.
>* Herhangi bir soru ile [Log Analytics ekibine](mailto:orens@microsoft.com) başvurun veya özelliği uygulayın.
## <a name="troubleshoot-and-query-your-code-or-configuration-issues"></a>Kodunuzun veya yapılandırma sorunlarınızı giderme ve sorgulama
Azure Izleyici sorun giderme günlüklerini kullanarak kayıtlarınızı getirip sorunları ve sorunları KQL kullanarak daha basit ve daha ucuz bir şekilde araştırın.
Sorun giderme günlükleri, sorun giderme için temel yetenekler sunarak ücretlerinizi azaltır.

> [!NOTE]
>* Sorun giderme modu için karar yapılandırılabilir.
>* Sorun giderme günlükleri, şu anda "kapsayıcı günlükleri" ve "uygulama Izlemeleri" tablolarında belirli tablolara uygulanabilir.
>* 4 gün ücretsiz saklama süresi mevcuttur, ek maliyet olarak genişletilebilir.
>* Tablolar, varsayılan olarak çalışma alanı bekletmesini devralır. Ek ücretlerden kaçınmak için, bu tablo bekletmenin değiştirilmesi önerilir. [Tablo bekletmesini değiştirme hakkında bilgi edinmek için buraya tıklayın](https://docs.microsoft.com//azure/azure-monitor/platform/manage-cost-storage).

## <a name="turn-on-troubleshooting-logs-on-your-tables"></a>Tablolarınızda sorun giderme günlüklerini açma

Çalışma alanınızda sorun giderme günlüklerini açmak için aşağıdaki API çağrısını kullanmanız gerekir.
```http
PUT https://PortalURL/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/tables/{tableName}

(With body in the form of a GET single table request response)

Response:

{
        "properties": {
          "retentionInDays": 40,
          "isTroubleshootingAllowed": true,
          "isTroubleshootEnabled": true
        },
        "id": "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/tables/{tableName}",
        "name": "{tableName}"
      }
```
## <a name="check-if-the-troubleshooting-logs-feature-is-enabled-for-a-given-table"></a>Belirli bir tablo için sorun giderme günlükleri özelliğinin etkinleştirilip etkinleştirilmediğini denetle
Belirli bir tablo için sorun giderme günlüğünün etkinleştirilip etkinleştirilmediğini denetlemek için aşağıdaki API çağrısını kullanabilirsiniz.

```http
GET https://PortalURL/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/tables/{tableName}

Response: 
"properties": {
          "retentionInDays": 30,
          "isTroubleshootingAllowed": true,
          "isTroubleshootEnabled": true,
          "lastTroubleshootDate": "Thu, 19 Nov 2020 07:40:51 GMT"
        },
        "id": "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.operationalinsights/workspaces/{workspaceName}/tables/{tableName}",
        "name": " {tableName}"

```
## <a name="check-if-the-troubleshooting-logs-feature-is-enabled-for-all-of-the-tables-in-a-workspace"></a>Çalışma alanındaki tüm tablolar için sorun giderme günlükleri özelliğinin etkinleştirilip etkinleştirilmediğini denetleyin
Sorun giderme günlüğünün etkinleştirildiği tabloları denetlemek için aşağıdaki API çağrısını kullanabilirsiniz.

```http
GET "https://PortalURL/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/tables"

Response: 
{
          "properties": {
            "retentionInDays": 30,
            "isTroubleshootingAllowed": true,
            "isTroubleshootEnabled": true,
            "lastTroubleshootDate": "Thu, 19 Nov 2020 07:40:51 GMT"
          },
          "id": "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.operationalinsights/workspaces/{workspaceName}/tables/table1",
          "name": "table1"
        },
        {
          "properties": {
            "retentionInDays": 7,
            "isTroubleshootingAllowed": true
          },
          "id": "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.operationalinsights/workspaces/{workspaceName}/tables/table2",
          "name": "table2"
        },
        {
          "properties": {
            "retentionInDays": 7,
            "isTroubleshootingAllowed": false
          },
          "id": "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.operationalinsights/workspaces/{workspaceName}/tables/table3",
          "name": "table3"
        }
```
## <a name="turn-off-troubleshooting-logs-on-your-tables"></a>Tablolarınızdaki sorun giderme günlüklerini kapatma

Çalışma alanınızdaki sorun giderme günlüklerini devre dışı bırakmak için aşağıdaki API çağrısını kullanmanız gerekir.
```http
PUT https://PortalURL/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/tables/{tableName}

(With body in the form of a GET single table request response)

Response:

{
        "properties": {
          "retentionInDays": 40,
          "isTroubleshootingAllowed": true,
          "isTroubleshootEnabled": false
        },
        "id": "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/tables/{tableName}",
        "name": "{tableName}"
      }
```
>[!TIP]
>* Komutları çalıştırmak için herhangi bir REST API aracı kullanabilirsiniz. [Daha fazla bilgi](https://docs.microsoft.com/rest/api/azure/)
>* Kimlik doğrulaması için taşıyıcı belirtecini kullanmanız gerekir. [Daha fazla bilgi](https://social.technet.microsoft.com/wiki/contents/articles/51140.azure-rest-management-api-the-quickest-way-to-get-your-bearer-token.aspx)

>[!NOTE]
>* "Isgiderileceği Tingallowed" bayrağı – tabloya hizmette izin verilip verilmeyeceğini açıklar
>* "Isınyatılan Tenabled" özelliği, özelliğin tablo için etkin olup olmadığını gösterir (true veya false)
>* Belirli bir tablo için "Iscumu Tenabled" bayrağını devre dışı bırakırken, önceki etkinleştirme tarihinden sonra yalnızca bir hafta olabilir.
>* Şu anda bu yalnızca altındaki tablolar için desteklenir (gelecekte bazı STB 'ler de desteklenecektir)- [fiyatlandırma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/services-hub/health/azure_pricing).

## <a name="query-limitations-for-troubleshooting"></a>Sorun giderme için sorgu sınırlamaları
"Sorun giderme günlükleri" olarak işaretlenen bir tablo için birkaç sınırlama vardır:
*   Daha az işlem kaynağı alır ve bu nedenle büyük panolar, karmaşık analizler veya çok sayıda eşzamanlı API çağrısı için uygun olmayacaktır.
*   Sorgular iki günün zaman aralığıyla sınırlandırılmıştır.
* Temizleme işlemi çalışmayacak – [temizleme hakkında daha fazla bilgi edinin](https://docs.microsoft.com/rest/api/loganalytics/workspacepurge/purge).
* Uyarılar Bu hizmet aracılığıyla desteklenmez.
## <a name="next-steps"></a>Sonraki adımlar
* [Sorgu yazma](https://docs.microsoft.com/azure/data-explorer/write-queries)


