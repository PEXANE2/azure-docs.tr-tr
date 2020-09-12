---
title: Azure SQL Edge (Önizleme) nedir?
description: Azure SQL Edge (Önizleme) hakkında bilgi edinin
keywords: SQL Edge 'e giriş, SQL Edge nedir, SQL Edge 'e genel bakış
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 20e709db9e6992f52b04934cb0f6eb65d3dcb44c
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89489536"
---
# <a name="what-is-azure-sql-edge-preview"></a>Azure SQL Edge (Önizleme) nedir?

Azure SQL Edge (Önizleme), IoT ve IoT Edge dağıtımlar için tasarlanmış, iyileştirilmiş bir ilişkisel veritabanı altyapısıdır. IoT uygulamaları ve çözümleri için yüksek performanslı veri depolama ve işleme katmanı oluşturma olanakları sağlar. Azure SQL Edge, çeşitli modern IoT uygulamaları için doğru seçim yapan, ilişkisel ve JSON, grafik ve zaman serisi verileri gibi ilişkisel olmayan verileri akışa almak, işlemek ve analiz etmek için yetenekler sağlar.

Azure SQL Edge, sektör lideri performans, güvenlik ve sorgu işleme yetenekleri sağlayan Microsoft SQL veritabanı altyapısının (/SQL/SQL-Server/SQL-Server-teknisyen al-documentation? TOC =/Azure/Azure-SQL-Edge/toc.json) en son sürümlerinde oluşturulmuştur. Azure SQL Edge, [SQL Server](/sql/sql-server/sql-server-technical-documentation?toc=/azure/azure-sql-edge/toc.json) ve [Azure SQL](https://docs.microsoft.com/azure/azure-sql/)ile aynı altyapıda oluşturulduğundan, uygulama ve çözümlerin geliştirilmesini daha kolay ve hızlı hale getiren T-SQL programlama yüzeyi alanını sağlar ve aynı zamanda uygulama taşınabilirliği, IoT Edge cihazlar, veri merkezleri ve bulut arasında doğrudan ileri sarma yapar.

> [!NOTE]
> Azure SQL Edge Şu anda önizleme aşamasındadır ve bu nedenle üretim ortamlarında kullanılmamalıdır.

## <a name="deployment-models"></a>Dağıtım modelleri

Azure SQL Edge, Azure Marketi 'nde kullanılabilir ve [Azure IoT Edge](../iot-edge/about-iot-edge.md)için bir modül olarak dağıtılabilir. Daha fazla bilgi için bkz. [Azure SQL Edge 'ı dağıtma](deploy-portal.md).<br>

![SQL Edge genel bakış Diyagramı](media/overview/overview.png)

## <a name="editions-of-sql-edge"></a>SQL Edge sürümleri

SQL Edge, iki farklı sürüm veya yazılım planlarıyla kullanılabilir. Bu sürümler aynı özellik kümelerine sahiptir ve yalnızca kullanım hakları ve destekledikleri CPU/bellek miktarı bakımından farklılık gösterir.

   |**Plan**  |**Açıklama**  |
   |---------|---------|
   |Azure SQL Edge geliştiricisi  |  Yalnızca geliştirme SKU 'su, her SQL Edge kapsayıcısı 4 çekirdeğe ve 32 GB bellekle sınırlıdır  |
   |Azure SQL Edge    |  Üretim SKU 'su olan her SQL Edge kapsayıcısı, 8 çekirdeğe ve 64 GB bellekle sınırlıdır. |

## <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Azure SQL Edge Şu anda önizleme aşamasındadır. Fiyatlandırma ve kullanılabilirlik hakkında daha fazla bilgi için bkz. [Azure SQL Edge](https://azure.microsoft.com/services/sql-edge/).

> [!IMPORTANT]
> Azure SQL Edge ve SQL Server arasındaki özellik farklarını ve farklı Azure SQL Edge seçenekleri arasındaki farkları anlamak için bkz. [Azure SQL Edge 'In desteklenen özellikleri](features.md).

## <a name="streaming-capabilities"></a>Akış özellikleri  

Azure SQL Edge, gerçek zamanlı analiz ve karmaşık olay işleme için yerleşik akış özellikleri sağlar. Akış özelliği, [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) ile aynı yapılar ve [IoT Edge üzerinde Azure Stream Analytics](../stream-analytics/stream-analytics-edge.md)benzer yetenekler kullanılarak oluşturulmuştur.

Azure SQL Edge için akış altyapısı, düşük gecikmeli, dayanıklılık, bant genişliği ve uyumluluk açısından verimli bir şekilde kullanılmak üzere tasarlanmıştır. 

SQL Edge 'de veri akışı hakkında daha fazla bilgi için bkz. [veri akışı](stream-data.md)

## <a name="machine-learning-and-artificial-intelligence-capabilities"></a>Machine Learning ve yapay zeka özellikleri

Azure SQL Edge, farklı çerçeveler arasında derin öğrenme ve sinir ağ modellerinin değişimine izin veren ONNX (Open sinir Network Exchange) çalışma zamanının açık biçimini tümleştirerek yerleşik makine öğrenimi ve analiz özellikleri sağlar. ONNX hakkında daha fazla bilgi için [buraya](https://onnx.ai/)bakın. ONNX çalışma zamanı, seçtiğiniz bir dilde veya bir araç içinde modeller geliştirme esnekliği sağlar ve daha sonra SQL Edge 'de yürütme için ONNX biçimine dönüştürülebilir. Daha fazla bilgi için bkz. [SQL Edge 'de ONNX ile Machine Learning ve yapay zeka](onnx-overview.md).

## <a name="working-with-azure-sql-edge"></a>Azure SQL Edge ile çalışma

Azure SQL Edge, uygulamaları geliştirmeyi ve uygulamayı daha kolay ve üretken hale getirir. Kullanıcılar IoT Edge gereksinimlerine yönelik harika uygulamalar ve çözümler oluşturmak için tüm tanıdık araç ve becerileri kullanabilir. Kullanıcı, aşağıdaki gibi araçları kullanarak SQL Edge 'de geliştirme yapabilir:

- [Azure Portal](https://portal.azure.com/) -tüm Azure hizmetlerini yönetmek için Web tabanlı bir uygulama.
- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms/) -SQL Server SQL veritabanı 'na HERHANGI bir SQL altyapısını yönetmeye yönelik ücretsiz, indirilebilir bir istemci uygulaması.
- [Visual Studio 'da SQL Server veri araçları](/sql/ssdt/download-sql-server-data-tools-ssdt/) -SQL Server ilişkisel VERITABANLARı, SQL veritabanları, Tümleştirme Hizmetleri paketleri, Analysis Services veri modelleri ve Raporlama Hizmetleri raporları geliştirmeye yönelik ücretsiz, indirilebilir bir istemci uygulaması.
- [Azure Data Studio](/sql/azure-data-studio/what-is/) -Windows, MacOS ve Linux 'ta Microsoft 'un Şirket içi ve bulut veri platformlarını kullanarak veri uzmanlarına yönelik ücretsiz, indirilebilir ve platformlar arası veritabanı aracı.
- [Visual Studio Code](https://code.visualstudio.com/docs) -Windows, MacOS ve Linux için ücretsiz, indirilebilir, açık kaynaklı kod Düzenleyicisi. Microsoft SQL Server, Azure SQL veritabanı ve Azure SYNAPSE Analytics 'i sorgulamak için [MSSQL uzantısı](https://aka.ms/mssql-marketplace) dahil olmak üzere uzantıları destekler.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal üzerinden SQL Edge dağıtma](deploy-portal.md)
- [SQL Edge ile Machine Learning ve yapay zeka](onnx-overview.md)
- [SQL Edge ile uçtan uca IoT çözümü oluşturma](tutorial-deploy-azure-resources.md)
