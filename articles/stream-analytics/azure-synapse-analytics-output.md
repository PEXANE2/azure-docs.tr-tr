---
title: Azure Stream Analytics Azure SYNAPSE Analytics çıkışı
description: Bu makalede, Azure SYNAPSE Analytics 'i Azure Stream Analytics çıktı olarak açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 5d281bf33542354e2904805deed2f1deaf2725a2
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88875920"
---
# <a name="azure-synapse-analytics-output-from-azure-stream-analytics-preview"></a>Azure Stream Analytics 'den Azure SYNAPSE Analytics çıkışı (Önizleme)

[Azure SYNAPSE Analytics](https://azure.microsoft.com/services/synapse-analytics) (eskı adıyla SQL veri ambarı), kurumsal veri depolama ve büyük veri analizlerini birlikte getiren sınırsız bir analiz hizmetidir. 

Azure Stream Analytics işleri, Azure SYNAPSE Analytics 'teki bir SQL havuz tablosuna çıkış yapabilir ve işleme hızını 200 MB/sn 'ye kadar işleyebilir. Bu, raporlama ve oluşturma gibi iş yükleri için en zorlu gerçek zamanlı analiz ve etkin yol veri işleme ihtiyaçlarını destekler.  

SQL havuzu tablosu, Stream Analytics işinize çıkış olarak ekleyebilmeniz için mevcut olmalıdır. Tablonun şeması, iş çıktısındaki alanlarla ve türleriyle eşleşmelidir. 

Azure SYNAPSE 'i çıkış olarak kullanmak için, depolama hesabının yapılandırıldığından emin olmanız gerekir. Depolama hesabını yapılandırmak için depolama hesabı ayarları ' na gidin. Yalnızca tabloları destekleyen depolama hesabı türlerine izin verilir: genel amaçlı v2 ve genel amaçlı v1. Yalnızca Standart katman ' ı seçin. Premium katman desteklenmez.

## <a name="output-configuration"></a>Çıkış yapılandırması

Aşağıdaki tabloda, har Azure SYNAPSE Analytics çıkışı oluşturmaya yönelik özellik adları ve açıklamaları listelenmektedir.

|Özellik adı|Açıklama|
|-|-|
|Çıktı diğer adı |Sorgu çıkışını bu veritabanına yönlendirmek için sorgularda kullanılan kolay bir ad. |
|Veritabanı |Çıktunuzu gönderdiğiniz SQL havuzu adı. |
|Sunucu adı |Azure SYNAPSE sunucu adı.  |
|Kullanıcı adı |Veritabanına yazma erişimi olan Kullanıcı adı. Stream Analytics yalnızca SQL kimlik doğrulamasını destekler. |
|Parola |Veritabanına bağlanmak için parola. |
|Tablo  | Çıktının yazıldığı tablo adı. Tablo adı büyük/küçük harfe duyarlıdır. Bu tablonun şeması, alan sayısı ve iş çıktılarınızın oluşturduğu türleri ile tam olarak eşleşmelidir.|

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı başlangıç: Azure CLı kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-cli.md)
* [Hızlı başlangıç: ARM şablonu kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-resource-manager.md)
* [Hızlı başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Hızlı başlangıç: Visual Studio Code Azure Stream Analytics iş oluşturma](quick-create-vs-code.md)