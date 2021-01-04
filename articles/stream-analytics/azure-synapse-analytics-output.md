---
title: Azure Stream Analytics Azure SYNAPSE Analytics çıkışı
description: Bu makalede, Azure SYNAPSE Analytics 'i Azure Stream Analytics çıktı olarak açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 65752321f94bfa1403839889ead284d46b5f7191
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97740071"
---
# <a name="azure-synapse-analytics-output-from-azure-stream-analytics"></a>Azure Stream Analytics Azure SYNAPSE Analytics çıkışı

[Azure SYNAPSE Analytics](https://azure.microsoft.com/services/synapse-analytics) , kurumsal veri depolama ve büyük veri analizlerini birlikte getiren sınırsız bir analiz hizmetidir. 

Azure Stream Analytics işleri, Azure SYNAPSE Analytics 'te adanmış bir SQL havuzu tablosuna çıkış yapabilir ve işleme hızını 200 MB/sn 'ye kadar işleyebilir. Bu, raporlama ve oluşturma gibi iş yükleri için en zorlu gerçek zamanlı analiz ve etkin yol veri işleme ihtiyaçlarını destekler.  

Adanmış SQL havuzu tablosu, Stream Analytics işinize çıkış olarak ekleyebilmeniz için mevcut olmalıdır. Tablonun şeması, iş çıktısındaki alanlarla ve türleriyle eşleşmelidir. 

Azure SYNAPSE 'i çıkış olarak kullanmak için, depolama hesabının yapılandırıldığından emin olmanız gerekir. Depolama hesabını yapılandırmak için depolama hesabı ayarları ' na gidin. Yalnızca tabloları destekleyen depolama hesabı türlerine izin verilir: genel amaçlı v2 ve genel amaçlı v1. Yalnızca Standart katman ' ı seçin. Premium katman desteklenmez.

## <a name="output-configuration"></a>Çıkış yapılandırması

Aşağıdaki tabloda, har Azure SYNAPSE Analytics çıkışı oluşturmaya yönelik özellik adları ve açıklamaları listelenmektedir.

|Özellik adı|Açıklama|
|-|-|
|Çıktı diğer adı |Sorgu çıkışını bu veritabanına yönlendirmek için sorgularda kullanılan kolay bir ad. |
|Veritabanı |çıktunuzu gönderdiğiniz adanmış SQL havuzu adı. |
|Sunucu adı |Azure SYNAPSE sunucu adı.  |
|Kullanıcı adı |Veritabanına yazma erişimi olan Kullanıcı adı. Stream Analytics yalnızca SQL kimlik doğrulamasını destekler. |
|Parola |Veritabanına bağlanmak için parola. |
|Tablo  | Çıktının yazıldığı tablo adı. Tablo adı büyük/küçük harfe duyarlıdır. Bu tablonun şeması, alan sayısı ve iş çıktılarınızın oluşturduğu türleri ile tam olarak eşleşmelidir.|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics işinden Azure SQL veritabanı 'na veya Azure SYNAPSE Analytics 'e erişmek için Yönetilen kimlikler kullanma (Önizleme)](sql-database-output-managed-identity.md)
* [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)