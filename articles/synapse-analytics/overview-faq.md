---
title: SSS - Azure Synapse Analytics
description: Azure Synapse Analytics için SSS
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: ec2791911d1461ae47d9353629d23a5d7dc30791
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424653"
---
# <a name="azure-synapse-analytics-workspace-preview-frequently-asked-questions"></a>Azure Synapse Analytics (çalışma alanı önizleme) sık sorulan sorular

Bu kılavuzda, Synapse Analytics için en sık sorulan soruları bulacaksınız.

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="general"></a>Genel

### <a name="q-what-is-azure-synapse-analytics"></a>S: Azure Synapse Analytics nedir

C: Azure Synapse, BI, AI ve sürekli zeka için entegre bir veri platformudur. SQL ve Spark gibi çeşitli analiz çalışma sürelerini aşağıdakilere birleşik bir yol sağlayan tek bir platform üzerinden bağlar:

- Ağ da dahil olmak üzere analitik kaynaklarınızı güvence altına alın, havuza, verilere ve geliştirme yapılarına tek oturum açma erişimi yönetin.
- Çalışma alanı etkinliklerinizde herhangi bir katmanda meydana gelen olayları kolayca izleyin ve hızlı bir şekilde optimize edin, tepkie getirin ve hata ayıklayın.
- Meta verilerinizi motorlar arasında yönetin. Bir Kıvılcım tablosu oluşturun ve Azure Synapse veritabanlarınızda otomatik olarak kullanılabilir.
- Birleşik bir kullanıcı deneyimi aracılığıyla verilerle etkileşim kurun. Synapse Studio, Büyük Veri Geliştiricileri, Veri Mühendisleri, DAB'ler, Veri Analistleri ve Veri Bilimcileri'ni aynı platforma getiriyor.

### <a name="q-how-do-i-get-started-with-azure-synapse-analytics"></a>S: Azure Synapse Analytics ile nasıl başlarım?

C: Azure Synapse Analytics'i kullanmaya başlamak için bir [Synapse çalışma alanı](https://portal.azure.com) oluşturun (ücretsiz!) ve bu çalışma alanı altında istediğiniz kaynakları oluşturun. [Synapse SQL havuzu oluşturma](quickstart-create-sql-pool.md) veya basit kullanım durumunda size yol gösteren [bir çalışma alanı oluşturma](quickstart-create-workspace.md)gibi hızlı başlangıç eğitimlerimizden birini takip edebilirsiniz. 

Ayrıca, örnek not defterlerini ve SQL komut dosyalarını [depomuzda](https://github.com/Azure/azure-synapse-analytics/tree/master/samples)da bulabilirsiniz. Ortak bir veri kümesine bağlanmanız gerekiyorsa, aşağıdaki özniteliklere sahip yeni bir bağlantılı hizmet oluşturun:

- azure_storage_account_name = "azureopendatastorage"
- azure_storage_sas_token = "" **(""** yaz)

### <a name="q-what-are-the-main-components-of-azure-synapse-analytics"></a>S: Azure Synapse Analytics'in ana bileşenleri nelerdir

C: Azure Synapse aşağıdaki özelliklere sahiptir:

- Analytics yetenekleri SQL havuzu veya isteğe bağlı SQL (önizleme) (Serverless) aracılığıyla sunulur.
- Scala, Python, SparkSQL ve C için tam destek içeren Apache Spark havuzu (önizleme) #
- Kodsuz büyük veri dönüşümü deneyimi sunan Veri Akışı
- Veri Entegrasyonu & Verilerinizi tümleştirmek ve tüm kod geliştirme işlemlerinizi operasyonel hale getirmek için Orkestrasyon
- Stüdyo tek bir Web UI üzerinden tüm bu yetenekleri erişmek için

### <a name="q-how-does-azure-synapse-analytics-relate-to-azure-sql-data-warehouse"></a>S: Azure Synapse Analytics'in Azure SQL Veri Ambarı ile ilişkisi nedir?

C: Azure Synapse Analytics, Azure SQL Veri Ambarı'nın veri ambarı çözümü olarak SQL havuzunu içeren bir analitik platforma dönüştürür. Bu platform veri arama, yutma, dönüştürme, hazırlama ve servis analizi katmanını birleştirir.

## <a name="use-cases"></a>Uygulama alanları

### <a name="q-what-is-a-good-use-case-for-synapse-sql-pool"></a>S: Synapse SQL havuzu için iyi kullanım örneği nedir

C: SQL havuzu, veri ambarı ihtiyaçlarınızın kalbidir. [Fiyat/performans](https://azure.microsoft.com/services/sql-data-warehouse/compare/)alanında lider veri ambarı çözümüdür. SQL havuzu sektör lideri bulut veri ambarı çözümüdür, çünkü şunları yapabilirsiniz:

- yüksek eşzamanlılık ve iş yükü yalıtımı sayesinde performansta etkilenmeden çok çeşitli iş yüklerine hizmet
- Ağ güvenliğinden ince tahıl erişimine kadar çeşitli gelişmiş özelliklerle verilerinizi kolayca güvenli hale
- geniş bir eko-sistem yelpazesinden yararlanın

### <a name="q-what-is-a-good-use-case-for-spark-in-synapse"></a>S: Ssynapse'de Kıvılcım için iyi kullanım durumu nedir

C: İlk hedefimiz, verileri toplu olarak veya akış halinde göl üzerinde dönüştürmek için harika bir Veri Mühendisliği deneyimi sunmaktır. Veri orkestrasyonumuza sıkı ve basit entegrasyonu, geliştirme çalışmanızın işlevselleştirilmesini kolaylaştırır.

Spark için başka bir kullanım örneği bir Veri Bilimcisi için:

- bir özellik ayıklamak
- verileri keşfedin
- bir model eğitmek

### <a name="q-what-is-a-good-use-case-for-sql-on-demand-in-synapse"></a>S: Synapse'de isteğe bağlı SQL için iyi kullanım durumu nedir

C: SQL isteğe bağlı, veri gölünüzdeki veriler üzerinde bir sorgu hizmetidir. Verileri özel bir depoya kopyalamaya veya yüklemeye gerek kalmadan, verileri yerinde sorgulamak için tanıdık bir T-SQL sözdizimini sağlayarak tüm verilerinize erişimi demokratikleştirmenizi sağlar.

Kullanım örneği örnekleri şunlardır:

- temel keşif ve keşif - veri analistleri, gelişmekte olan veri bilim adamları ve veri mühendisleri okuma T-SQL sorguları şema ile veri gölünde yaşayan veri içine ilk içgörü kolay bir yol sağlar
- mantıksal veri ambarı - veri analistleri, Azure Depolama'da bulunan verileri doğrudan sorgulamak ve analiz etmek ve Starlight Sorgu sorgularını yeniden çalıştırarak panoları yenilemek için tanıdık BI araçlarını (örneğin, Azure Analiz Hizmetleri, Power BI Premium, vb.) kullanmak için T-SQL dilinin tam ifade sini çalıştırabilir
- "tek sorgu" ETL - veri mühendislerinin Azure Depolama tabanlı verileri bir formattan diğerine dönüştürmesine, filtrelemesine, toplamasına vb. paralel işlem biçiminde dönüştürmesine, sorgu sonuçlarını Azure Depolama'ya devam etmesine ve Starlight Sorgusu'nda veya ilgi çekici diğer hizmetlerde daha fazla işleme için hemen kullanılabilir hale getirmelerine olanak tanır

### <a name="q-what-is-a-good-use-case-for-data-flow-in-synapse"></a>S: Synapse veri akışı için iyi bir kullanım durumu nedir

C: Veri akışı, veri mühendislerinin kod yazmadan grafiksel veri dönüştürme mantığı geliştirmelerine olanak tanır. Elde edilen veri akışları, Veri Tümleştirme & Orkestrasyon'da etkinlik olarak yürütülür. Veri akışı etkinlikleri, varolan zamanlama, kontrol, akış ve izleme yetenekleri aracılığıyla operasyonel hale getirilebilir.

## <a name="security-and-access"></a>Güvenlik ve Erişim

C: Uçlardan uca tek oturum açma deneyimi, Synapse Analytics'te önemli bir kimlik doğrulama işlemidir. Tam bir AAD entegrasyonu ile kimlik üzerinden yönetmek ve geçmek şarttır.

### <a name="q-how-do-i-get-access-to-files-and-folders-in-the-adlsg2"></a>S: ADLSg2'deki dosya ve klasörlere nasıl erişebilirim?

C: Dosya ve klasörlere erişim şu anda ADLSg2 üzerinden yönetilir. Daha fazla bilgi için [Bkz. Veri Gölü depolama erişim denetimi.](../storage/blobs/data-lake-storage-access-control.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

### <a name="q-can-i-use-third-party-business-intelligence-tools-to-access-azure-synapse-analytics"></a>S: Azure Synapse Analytics'e erişmek için üçüncü taraf iş zekası araçlarını kullanabilir miyim?

C: Evet, SQL havuzuna ve isteğe bağlı SQL'e bağlanmak için Tableau ve Power BI gibi üçüncü taraf iş uygulamalarınızı kullanabilirsiniz. Kıvılcım, IntelliJ'i destekliyor.

## <a name="next-steps"></a>Sonraki adımlar

- [Çalışma alanı oluşturma](quickstart-create-workspace.md)
- [Synapse Studio'u kullanın](quickstart-synapse-studio.md)
- [SQL havuzu oluşturma](quickstart-create-sql-pool.md)
- [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
- [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool.md) 