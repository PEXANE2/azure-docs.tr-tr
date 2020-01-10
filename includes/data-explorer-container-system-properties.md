---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 01/08/2020
ms.author: orspodek
ms.openlocfilehash: f9788e4623ce60ad55d79558d1d77a17eb2a9f26
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75779968"
---
### <a name="event-system-properties-mapping"></a>Olay sistemi özellikleri eşleme

Yukarıdaki tablonun **veri kaynağı** bölümünde **olay sistem özellikleri** ' ni seçtiyseniz, uygun eşleme oluşturma için ilgili KQL komutunu çalıştırmak üzere [Web Kullanıcı arabirimine](https://dataexplorer.azure.com/) gidin.

   **CSV eşlemesi için:**

    ```kusto
    .create table MyTable ingestion csv mapping "CsvMapping1"
    '['
    '   { "column" : "messageid", "DataType":"string", "Properties":{"Ordinal":"0"}},'
    '   { "column" : "userid", "DataType":"string", "Properties":{"Ordinal":"1"}},'
    '   { "column" : "other", "DataType":"int", "Properties":{"Ordinal":"2"}}'
    ']'
    ```
 
   **JSON eşlemesi için:**

    ```kusto
    .create table MyTable ingestion json mapping "JsonMapping1"
    '['
    '    { "column" : "messageid", "datatype" : "string", "Properties":{"Path":"$.message-id"}},'
    '    { "column" : "userid", "Properties":{"Path":"$.user-id"}},'
    '    { "column" : "other", "Properties":{"Path":"$.other"}}'
    ']'
    ```

   > [!TIP]
   > * Tüm seçili özellikleri eşlemeye dahil etmeniz gerekir. 
   > * Özellikler sırası CSV eşlemesinde önemlidir. Sistem Özellikleri, diğer tüm özelliklerden önce ve **olay sistemi özellikleri** açılır listesinde göründükleri sırada listelenmelidir.