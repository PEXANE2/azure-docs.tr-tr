---
title: Azure SQL Veritabanı Kenarı nedir? | Microsoft Belgeleri
description: Azure SQL Veritabanı Kenarı hakkında bilgi edinin
keywords: sql veritabanı kenarına giriş,sql veritabanı kenarı nedir, sql veritabanı kenarına genel bakış
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 03/24/2020
ms.openlocfilehash: d5c48b6036065f6182912c21c144cab80fc3cfbf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246713"
---
# <a name="what-is-azure-sql-database-edge-preview"></a>Azure SQL Veritabanı Kenar Önizlemesi nedir?

Azure SQL Veritabanı Kenarı Önizleme, IoT ve IoT Edge dağıtımları için optimize edilmiş bir ilişkisel veritabanı motorudur. IoT uygulamaları ve çözümleri için yüksek performanslı bir veri depolama ve işleme katmanı oluşturmak için yetenekler sağlar. Azure SQL Database Edge, JSON, grafik ve zaman serisi verileri gibi ilişkisel ve ilişkisel olmayan verileri akış, işleme ve çözümleme özellikleri sağlar ve bu da onu çeşitli modern IoT uygulamaları için doğru seçim yapar.

Azure SQL Database Edge, endüstri lideri performans, güvenlik ve sorgu işleme özellikleri sağlayan [Microsoft SQL Server Database Engine'in](/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database-edge/toc.json)en son sürümleriüzerine kurulmuştur. Azure SQL Database Edge, SQL Server ve Azure SQL Veritabanı ile aynı altyapı üzerine kurulduğundan, uygulamaların veya çözümlerin geliştirilmesini daha kolay ve hızlı hale getiren aynı T-SQL programlama yüzey alanını sağlar ve aynı zamanda uygulama yapar IoT Edge aygıtları, veri merkezleri ve bulut arasında taşınabilirlik.

## <a name="deployment-models"></a>Dağıtım Modelleri

Azure SQL Veritabanı Kenarı Azure Marketi'nde kullanılabilir ve [Azure IoT Edge](../iot-edge/about-iot-edge.md)için modül olarak dağıtılabilir. Daha fazla bilgi için [bkz.](deploy-portal.md)<br>

![SQL Veritabanı Kenarı genel bakış diyagramı](media/overview/overview.png)

## <a name="editions-of-sql-database-edge"></a>SQL Veritabanı Edge Sürümleri

SQL Database Edge üç farklı sürümleri veya yazılım planları ile kullanılabilir. Bu sürümler aynı özellik kümelerine sahiptir ve yalnızca kullanım hakları ve destekledikleri işlemci/bellek miktarı açısından farklılık gösterir.

   |**Planlama**  |**Açıklama**  |
   |---------|---------|
   |Azure SQL Veritabanı Edge Geliştiricisi  |  Geliştirme sadece sku, her SQL Database Edge konteyner en fazla 4 çekirdek ve 32 GB Bellek ile sınırlıdır  |
   |Uç Cihazlar için Azure SQL Veritabanı    |  Üretim sku, her SQL Database Edge konteyner 8 çekirdek ve 64 GB Bellek ile sınırlıdır. |

## <a name="pricing-and-availability"></a>Fiyatlandırma ve Kullanılabilirlik

Azure SQL Veritabanı şu anda önizlemede. Fiyatlandırma ve kullanılabilirlik hakkında daha fazla bilgi için [Azure SQL Veritabanı Kenarı'na](https://azure.microsoft.com/services/sql-database-edge/)bakın.

> [!IMPORTANT]
> Azure SQL Veritabanı Kenarı ve SQL Server arasındaki özellik farklılıklarının yanı sıra farklı Azure SQL Veritabanı Kenarı seçenekleri arasındaki farkları anlamak için [SQL Database Edge Veritabanı özelliklerine](https://azure.microsoft.com/services/sql-database-edge/)bakın.

## <a name="streaming-capabilities"></a>Akış Özellikleri  

Azure SQL Database Edge, gerçek zamanlı analiz ve karmaşık olay işleme için yerleşik akış özellikleri sağlar. Akış özelliği, [Azure Akış Analitiği](../stream-analytics/stream-analytics-introduction.md) ile aynı yapılar kullanılarak oluşturulur ve [IoT Edge'deki Azure Akış Analizi](../stream-analytics/stream-analytics-edge.md)gibi benzer özellikler sağlar.

Azure SQL Database Edge'in akış altyapısı düşük gecikme, esneklik, bant genişliğinin verimli kullanımı ve uyumluluk için tasarlanmıştır.

## <a name="machine-learning-and-artificial-intelligence-capabilities"></a>Makine Öğrenimi ve Yapay Zeka Yetenekleri

Azure SQL Database Edge, açık format ONNX (Open Nöral Ağ Exchange) çalışma süresini entegre ederek yerleşik makine öğrenimi ve analitik yetenekleri sağlar ve bu da farklı çerçeveler arasında derin öğrenme ve sinir ağı modellerinin değişimine olanak tanır. ONNX hakkında daha fazla bilgi için [buraya](https://onnx.ai/)bakın. ONNX çalışma süresi, seçtiğiniz bir dilde veya araçlarda modeller geliştirme esnekliği sağlar ve bu da daha sonra SQL Database Edge içinde yürütülmesi için ONNX biçimine dönüştürülebilir. Daha fazla bilgi için [SQL Database Edge'de ONNX ile Machine Learning ve Yapay Zeka'ya](onnx-overview.md)bakın.

## <a name="working-with-azure-sql-database-edge"></a>Azure SQL Veritabanı Kenarı ile Çalışma

Azure SQL Database Edge, uygulama geliştirmeyi ve bakımını daha kolay ve verimli hale getirir. Kullanıcılar, IoT Edge ihtiyaçları için harika uygulamalar ve çözümler oluşturmak için tüm tanıdık araçları ve becerileri kullanabilir. Kullanıcı aşağıdaki gibi araçları kullanarak SQL Database Edge geliştirebilir:

- [Azure portalı](https://portal.azure.com/) - Tüm Azure hizmetlerini yönetmek için web tabanlı bir uygulamadır.
- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms/) - SQL Server'dan SQL Veritabanı'na kadar herhangi bir SQL altyapısını yönetmek için ücretsiz, indirilebilir istemci uygulaması.
- [Visual Studio'da SQL Server Veri Araçları](/sql/ssdt/download-sql-server-data-tools-ssdt/) - SQL Server ilişkisel veritabanları, SQL veritabanları, Entegrasyon Hizmetleri paketleri, Analiz Hizmetleri veri modelleri ve Raporlama Hizmetleri raporları geliştirmek için ücretsiz, indirilebilir istemci uygulaması.
- [Azure Data Studio](/sql/azure-data-studio/what-is/) - Windows, MacOS ve Linux'taki Microsoft şirket içi ve bulut veri platformları ailesini kullanarak veri profesyoneli için ücretsiz, indirilebilir, platformlar arası veritabanı aracı.
- [Visual Studio Code](https://code.visualstudio.com/docs) - Windows, macOS ve Linux için ücretsiz, indirilebilir, açık kaynak kod düzenleyicisi. Microsoft SQL Server, Azure SQL Veritabanı ve Azure SQL Veri Ambarı sorgulamaiçin [mssql uzantısı](https://aka.ms/mssql-marketplace) da dahil olmak üzere uzantıları destekler.


## <a name="next-steps"></a>Sonraki adımlar

- Fiyatlandırma ve kullanılabilirlikle ilgili ayrıntılar için [Azure SQL Veritabanı Kenarı'na](https://azure.microsoft.com/services/sql-database-edge/)bakın.
- Aboneliğiniz için Azure SQL Database Edge'i etkinleştirme isteğinde bulunun.
- Başlamak için aşağıdakilere bakın:
  - [Azure portalı üzerinden SQL Veritabanı Kenarı dağıtma](deploy-portal.md)
  - [SQL Veritabanı Edge ile Makine Öğrenimi ve Yapay Zeka](onnx-overview.md)
