---
title: Azure Synapse Analytics (eski adıyla SQL DW) Sık Sorulan Sorular
description: Bu makalede, müşterilerden ve geliştiricilerden Azure Synapse Analytics (eski adıyla SQL DW) hakkında sık sorulan sorular listele
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: dcefe6a6144c9a8f9c6ab7daf5b34b2d81a73c3f
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743009"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-frequently-asked-questions"></a>Azure Synapse Analytics (eski adıyla SQL DW) Sık sorulan sorular

## <a name="general"></a>Genel

S. Azure Synapse nedir?

A. Azure Synapse, veri depolama ve Büyük Veri analitiğini bir araya getiren sınırsız bir analiz hizmetidir. Sunucusuz isteğe bağlı veya sağlanan kaynakları ölçekte kullanarak, verileri koşullarınızla sorgulama özgürlüğü sağlar. Azure Synapse, anında BI ve makine öğrenimi ihtiyaçları için verileri yutmak, hazırlamak, yönetmek ve sunmak için bu iki dünyayı birleşik bir deneyimle bir araya getirir. Daha fazla bilgi için bkz: [Azure Synapse Analytics nedir.](sql-data-warehouse-overview-what-is.md)

S. Azure SQL Veri Ambarı'na ne oldu?

A. Azure Synapse, Azure SQL Veri Ambarı'dır (SQL DW) geliştirildi. Aynı endüstri lideri veri ambarını yepyeni bir performans ve yetenek düzeyine taşıdık. Mevcut veri ambarı iş yüklerinizi Azure Synapse ile üretimde çalıştırmaya devam edebilir ve önizlemede bulunan yeni özelliklerden otomatik olarak yararlanabilirsiniz. Daha fazla bilgi için Azure [Synapse Analytics nedir'e](sql-data-warehouse-overview-what-is.md)bakın.

S. Synapse SQL havuzu nedir?

A. Synapse SQL havuzu, genellikle Azure Synapse ile kullanılabilen kurumsal veri depolama özelliklerini ifade eder. Daha fazla bilgi için bkz: [Azure Synapse Analytics nedir.](sql-data-warehouse-overview-what-is.md)

S. Azure Synapse'ye nasıl başlarım?

A. Daha fazla bilgi için [Azure ücretsiz hesabıyla](https://azure.microsoft.com/free/sql-data-warehouse/) veya [satışlarla iletişim kurabilirsiniz.](https://info.microsoft.com/ww-landing-azure-sql-data-warehouse-contactme.html)

S. Azure Synapse veri güvenliği için neler sunar?

A. Azure Synapse, TDE ve denetim gibi verileri korumak için çeşitli çözümler sunar. Daha fazla bilgi için [Güvenlik'e](sql-data-warehouse-overview-manage-security.md)bakın.

S. Azure Synapse'nin hangi yasal veya iş standartlarına uygun olduğunu nereden öğrenebilirim?

A. SOC ve ISO gibi ürüne göre çeşitli uyumluluk teklifleri için [Microsoft Uyumluluk](https://www.microsoft.com/trustcenter/compliance/complianceofferings) sayfasını ziyaret edin.
İlk olarak, Uyumluluk başlığına göre seçin. Ardından, hangi hizmetlerin Azure Synapse uyumlu olduğunu görmek için sayfanın sağ tarafındaki Microsoft kapsam içi bulut hizmetleri bölümünde Azure'u genişletin.

S. Power BI'ye bağlanabilir miyim?

A. Evet! Power BI, Azure Synapse ile doğrudan sorguyı desteklese de, çok sayıda kullanıcı veya gerçek zamanlı veri için tasarlanmamıştır. Power BI performansını daha da optimize etmek için Azure Analiz Hizmetleri veya Analiz Hizmeti IaaS'ın üzerinde Power BI'yi kullanmayı düşünün.

S. Synapse SQL havuz kapasite sınırları nelerdir?

A. Geçerli [kapasite limitleri](sql-data-warehouse-service-capacity-limits.md) sayfamıza bakın.

S. Ölçek/Duraklatma/Özgeçmişim neden bu kadar uzun sürüyor?

A. Çeşitli etkenler, işlem yönetimi işlemlerinin zamanını etkileyebilir. Uzun süren işlemler için yaygın bir durum işlem geri almadır. Bir ölçek veya duraklatma işlemi başlatıldığında, gelen tüm oturumlar engellenir ve sorgular boşaltılır. Sistemi istikrarlı bir durumda bırakmak için, bir işlemin başlatılabilmesi için işlemlerin geri alınması gerekir. Hareketlerin günlük boyutu ne kadar büyük ve sayı ne kadar büyükse, işlem sistemi kararlı bir duruma geri getirmek için o kadar uzun süre durdurulacaktır.

## <a name="user-support"></a>Kullanıcı desteği

S. Bir özellik isteğim var, nereye gönderebilirim?

A. Bir özellik isteğiniz varsa, [kullanıcı](https://feedback.azure.com/forums/307516-sql-data-warehouse) sesi sayfamıza gönderin

S. X'i nasıl yapabilirim?

A. Azure Synapse ile geliştirme konusunda yardım için [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw) sayfamızda sorular sorabilirsiniz.

S. Destek biletini nasıl gönderirim?

A. [Destek Biletleri](sql-data-warehouse-get-started-create-support-ticket.md) Azure portalı üzerinden dosyalanabilir.

## <a name="sql-languagefeature-support"></a>SQL dil/özellik desteği

S. Hangi veri türleri desteklenir?

A. [Bkz. veri türleri.](sql-data-warehouse-tables-data-types.md)

S. Hangi tablo özelliklerini destekliyorsunuz?

A. Birçok özellik desteklenir. [Desteklenmeyen Tablo Özellikleri'nde desteklenmeyen](sql-data-warehouse-tables-data-types.md)özellikler bulunabilir.

## <a name="tooling-and-administration"></a>Takım lama ve yönetim

S. Synapse SQL havuzu REST API'lerini destekliyor mu?

A. Evet. SQL Veritabanı ile kullanılabilecek rest işlevlerinin çoğu Synapse SQL havuzunda da kullanılabilir. API bilgilerini REST dokümantasyon sayfalarında veya [Veritabanlarında](/rest/api/sql/databases?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)bulabilirsiniz.

## <a name="loading"></a>Yükleme

S. Hangi istemci sürücüleri destekliyorsunuz?

A. Synapse SQL havuzu için sürücü desteği [Bağlantı Dizeleri](sql-data-warehouse-connection-strings.md) sayfasında bulunabilir

S: Hangi dosya biçimleri PolyBase tarafından desteklenir?

C: Orc, RC, Parke ve düz sınırlı metin

S: PolyBase kullanarak hangi veri kaynaklarına bağlanabilirim?

C: [Azure Veri Gölü Depolama](sql-data-warehouse-load-from-azure-data-lake-store.md) ve Azure Depolama [Blobs](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

S: Azure Depolama Blobs veya ADLS'ye bağlanırken hesaplama geri tepmesi mümkün mü?

C: Hayır, PolyBase yalnızca depolama bileşenleriyle etkileşime geçer.

S: HDI'ye bağlanabilir miyim?

C: HDI, ADFS katmanı olarak ADLS veya WASB'yi kullanabilir. HDFS katmanınız olarak varsa, bu verileri bir Synapse SQL havuzuna yükleyebilirsiniz. Ancak, HDI örneğine pushdown hesaplama oluşturamazsınız.

## <a name="next-steps"></a>Sonraki adımlar

Azure Sinapse hakkında daha fazla bilgi için [Genel Bakış](sql-data-warehouse-overview-faq.md) sayfamıza bakın.
