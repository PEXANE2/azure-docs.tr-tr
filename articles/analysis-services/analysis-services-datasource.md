---
title: Azure Analiz Hizmetlerinde desteklenen veri kaynakları | Microsoft Dokümanlar
description: Azure Analiz Hizmetleri'nde 1200 ve daha yüksek veri modelleri için desteklenen veri kaynaklarını ve bağlayıcılarını açıklar.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f65d8fa2c2e522c718c637e32defc4c56fca8364
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461666"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Azure Analysis Services’ta desteklenen veri kaynakları

Analiz Hizmetleri projeleri ile Visual Studio'da Veri Alma veya Tablo Alma Sihirbazı'nda gösterilen veri kaynakları ve bağlayıcılar hem Azure Analiz Hizmetleri hem de SQL Server Analysis Services için gösterilir. Ancak, gösterilen tüm veri kaynakları ve bağlayıcılar Azure Çözümleme Hizmetlerinde desteklenmez. Bağlanabileceğiniz veri kaynakları türleri, model uyumluluk düzeyi, kullanılabilir veri bağlayıcıları, kimlik doğrulama türü ve şirket içi veri ağ geçidi desteği gibi birçok etkene bağlıdır. Aşağıdaki tablolar, Azure Çözümleme Hizmetleri için desteklenen veri kaynaklarını açıklayınız.

## <a name="azure-data-sources"></a>Azure veri kaynakları

|Veri kaynağı  |Bellek içi  |DirectQuery  |Notlar |
|---------|---------|---------|---------|
|Azure SQL Veritabanı      |   Evet      |    Evet      |<sup>[2](#azprovider)</sup>, <sup> [3](#azsqlmanaged)</sup>|
|Azure Synapse Analytics (SQL Veri Ambarı)      |   Evet      |   Evet       |<sup>[2](#azprovider)</sup>|
|Azure Blob Depolama      |   Evet       |    Hayır      | <sup>[1](#tab1400a)</sup> |
|Azure Table Storage     |   Evet       |    Hayır      | <sup>[1](#tab1400a)</sup>|
|Azure Cosmos DB     |  Evet        |  Hayır        |<sup>[1](#tab1400a)</sup> |
|Azure Veri Gölü Deposu Gen1      |   Evet       |    Hayır      |<sup>[1](#tab1400a)</sup> |
|Azure Veri Gölü Deposu Gen2       |   Evet       |    Hayır      |<sup>[1](#tab1400a)</sup>, <sup> [5](#gen2)</sup>|
|Azure HDInsight HDFS    |     Evet     |   Hayır       |<sup>[1](#tab1400a)</sup> |
|Azure HDInsight Spark     |   Evet       |   Hayır       |<sup>[1](#tab1400a)</sup>, <sup> [4](#databricks)</sup>|
||||

**Notlar:**   
<a name="tab1400a">1</a> - Tabular 1400 ve üzeri modeller sadece.  
<a name="azprovider">2</a> - Tabular 1200 ve daha yüksek modellerde *sağlayıcı* veri kaynağı olarak belirtildiğinde, hem bellek hem de DirectQuery modelleri SQL Server MSOLEDBSQL (önerilir), SQL Server Native Client 11.0 veya .NET Framework Data Provider for SQL Server için Microsoft OLE DB Driver'ı gerektirir.    
<a name="azsqlmanaged">3</a> - Azure SQL Veritabanı Yönetilen Örneği desteklenir. Yönetilen örnek Azure VNet içinde özel bir IP adresiyle çalıştığından, örnekte ortak bitiş noktasının etkinleştirilmesi gerekir. Etkin değilse, [şirket içi veri ağ geçidi](analysis-services-gateway.md) gerekir.    
<a name="databricks">4</a> - Kıvılcım konektörünü kullanan Azure Veri Tuğlaları şu anda desteklenmez.   
<a name="gen2">5</a> - ADLS Gen2 konektörü şu anda desteklenmez, ancak Azure Blob Depolama konektörü ADLS Gen2 veri kaynağı ile kullanılabilir.   

## <a name="other-data-sources"></a>Diğer veri kaynakları

|Veri kaynağı | Bellek içi | DirectQuery |Notlar   |
|  --- | --- | --- | --- |
|Access Veritabanı     |  Evet | Hayır |  |
|Active Directory     |  Evet | Hayır | <sup>[6](#tab1400b)</sup>  |
|Analysis Services     |  Evet | Hayır |  |
|Analitik Platform Sistemi     |  Evet | Hayır |  |
|CSV dosyası  |Evet | Hayır |  |
|Dynamics 365     |  Evet | Hayır | <sup>[6](#tab1400b)</sup> |
|Excel çalışma kitabı     |  Evet | Hayır |  |
|Exchange      |  Evet | Hayır | <sup>[6](#tab1400b)</sup> |
|Klasör      |Evet | Hayır | <sup>[6](#tab1400b)</sup> |
|IBM Informix  |Evet | Hayır |  |
|JSON belgesi      |  Evet | Hayır | <sup>[6](#tab1400b)</sup> |
|İkili çizgiler      | Evet | Hayır | <sup>[6](#tab1400b)</sup> |
|MySQL Veritabanı     | Evet | Hayır |  |
|OData Akışı      |  Evet | Hayır | <sup>[6](#tab1400b)</sup> |
|ODBC sorgusu     | Evet | Hayır |  |
|OLE DB     |   Evet | Hayır |  |
|Oracle  | Evet  |Evet  | <sup>[9](#oracle)</sup> |
|PostgreSQL Veritabanı   | Evet | Hayır | <sup>[6](#tab1400b)</sup> |
|Salesforce Nesneleri|  Evet | Hayır | <sup>[6](#tab1400b)</sup> |
|Salesforce Raporları |Evet | Hayır | <sup>[6](#tab1400b)</sup> |
|SAP HANA     |  Evet | Hayır |  |
|SAP Business Warehouse    |  Evet | Hayır | <sup>[6](#tab1400b)</sup> |
|SharePoint Listesi      |   Evet | Hayır | <sup>[6](#tab1400b)</sup>, <sup> [11](#filesSP)</sup> |
|SQL Server |Evet   | Evet  | <sup>[7](#sqlim)</sup>, <sup> [8](#instgw)</sup> | 
|SQL Server Veri Ambarı |Evet   | Evet  | <sup>[7](#sqlim)</sup>, <sup> [8](#instgw)</sup> |
|Sybase Veritabanı     |  Evet | Hayır |  |
|Teradata | Evet  | Evet  | <sup>[10](#teradata)</sup> |
|TXT dosyası  |Evet | Hayır |  |
|XML tablosu    |  Evet | Hayır | <sup>[6](#tab1400b)</sup> |
| | | |

**Notlar:**   
<a name="tab1400b">6</a> - Tabular 1400 ve üzeri modeller sadece.  
<a name="sqlim">7</a> - 1200 ve üzeri modellerde *sağlayıcı* veri kaynağı olarak belirtildiğinde, SQL Server MSOLEDBSQL (önerilir), SQL Server Native Client 11.0 veya SQL Server için .NET Framework Data Provider için Microsoft OLE DB Sürücüsü'nü belirtin.  
<a name="instgw">8</a> - Veri sağlayıcısı olarak MSOLEDBSQL belirtilmesi durumunda, SQL [Server için Microsoft OLE DB Sürücüsünü](https://docs.microsoft.com/sql/connect/oledb/oledb-driver-for-sql-server) şirket içi veri ağ geçidi yle aynı bilgisayara indirmeniz ve yüklemeniz gerekebilir.  
<a name="oracle">9</a> - Tabular 1200 modelleri için veya tabular 1400+ modellerinde *sağlayıcı* veri kaynağı olarak .NET için Oracle Veri Sağlayıcısı'nı belirtin.  
<a name="teradata">10</a> - Tabular 1200 modelleri için veya tabular 1400+ modellerinde *sağlayıcı* veri kaynağı olarak .NET için Teradata Veri Sağlayıcısı'nı belirtin.   
<a name="filesSP">11</a> - Şirket içi SharePoint'teki dosyalar desteklenmez.

Azure Çözümleme Hizmetleri sunucusundan şirket içi veri kaynaklarına bağlanmak için [şirket içi ağ geçidi](analysis-services-gateway.md)gerekir. Ağ geçidi kullanırken, 64 bit sağlayıcılar gereklidir. 

## <a name="understanding-providers"></a>Sağlayıcıları anlama

Visual Studio'da 1400 ve daha yüksek model projeleri oluştururken, varsayılan olarak **Veri Al'ı**kullanarak bir veri kaynağına bağlanırken bir veri sağlayıcısı belirtmezsiniz. Tabular 1400 ve daha yüksek modeller, veri kaynağı ve Analiz Hizmetleri arasındaki bağlantıları, veri sorgularını ve karışımları yönetmek için [Güç Sorgusu](/power-query/power-query-what-is-power-query) bağlayıcılarını kullanır. Bunlar bazen bağlantı özelliği ayarlarısizin için ayarlanmış *yapılandırılmış* veri kaynağı bağlantıları olarak adlandırılır. Ancak, eski veri kaynaklarını etkinleştirebilirsiniz. Etkinleştirildiğinde, tablo 1200 ve daha düşük modellerde geleneksel olarak desteklenen belirli veri kaynaklarına eski veya *sağlayıcı* veri kaynakları olarak bağlanmak için **Tablo Alma Sihirbazı'nı** kullanabilirsiniz. *legacy* Sağlayıcı veri kaynağı olarak belirtildiğinde, belirli bir veri sağlayıcısı ve diğer gelişmiş bağlantı özelliklerini belirtebilirsiniz. Örneğin, eski bir veri kaynağı olarak şirket içi BIR SQL Server Veri Ambarı'na ve hatta bir Azure SQL Veritabanına bağlanabilirsiniz. Daha sonra SQL Server MSOLEDBSQL veri sağlayıcısı için OLE DB Sürücüsü'nü seçebilirsiniz. Bu durumda, bir OLE DB veri sağlayıcısı seçmek Güç Sorgusu bağlayıcısı üzerinde geliştirilmiş performans sağlayabilir. 

Visual Studio'da Tablo İçe Alma Sihirbazı kullanılırken, herhangi bir veri kaynağına bağlantılar bir veri sağlayıcısı gerektirir. Sizin için varsayılan bir veri sağlayıcısı seçilir. Gerekirse veri sağlayıcısını değiştirebilirsiniz. Seçtiğiniz sağlayıcı türü performansa, modelin bellek içi depolama veya DirectQuery kullanıp kullanmadığına ve modelinizi hangi Çözümleme Hizmetleri platformuna dağıttığınıza bağlı olabilir.

### <a name="specify-provider-data-sources-in-tabular-1400-and-higher-model-projects"></a>Tabular 1400 ve daha yüksek model projelerde sağlayıcı veri kaynaklarını belirtin

Sağlayıcı veri kaynaklarını etkinleştirmek için Visual Studio'da **Araçlar** > **Seçenekleri** > **Çözümleme Hizmetleri Tabular** > Data**Import**'u tıklatın, eski veri **kaynaklarını etkinleştir'i**seçin.

![Eski veri kaynaklarını etkinleştirme](media/analysis-services-datasource/aas-enable-legacy-datasources.png)

Eski veri kaynakları etkinleştirildiğinde, **Tabular Model**Explorer'da,**Veri Kaynaklarından (Eski)** Veri **Kaynakları** > Alma'yı sağ tıklatın.

![Tabular Model Explorer'daki eski veri kaynakları](media/analysis-services-datasource/aas-import-legacy-datasources.png)

Tablo 1200 model projelerinde olduğu gibi, bir veri kaynağına bağlanmak için **Tablo Alma Sihirbazı'nı** kullanın. Bağlantı sayfasında **Gelişmiş'i**tıklatın. Gelişmiş Özellikleri Ayarla'da veri sağlayıcısını ve diğer bağlantı ayarlarını **belirtin.**

![Eski veri kaynakları Gelişmiş özellikler](media/analysis-services-datasource/aas-import-legacy-advanced.png)


## <a name="impersonation"></a>Kimliğe bürünme
Bazı durumlarda, farklı bir kimliğe bürünme hesabı belirtmek gerekebilir. Kimliğe bürünme hesabı Visual Studio veya SSMS'te belirtilebilir.

Şirket içi veri kaynakları için:

* SQL kimlik doğrulaması kullanıyorsanız, kimliğe bürünme Service Hesabı olmalıdır.
* Windows kimlik doğrulamasını kullanıyorsanız, Windows kullanıcı/parolayı ayarlayın. SQL Server için, belirli bir kimliğe bürünme hesabı yla Windows kimlik doğrulaması yalnızca bellek içi veri modelleri için desteklenir.

Bulut veri kaynakları için:

* SQL kimlik doğrulaması kullanıyorsanız, kimliğe bürünme Service Hesabı olmalıdır.

## <a name="oauth-credentials"></a>OAuth kimlik bilgileri

Bellek içi mod, Azure SQL Veritabanı, Azure Synapse Analytics (SQL Data Warehouse), Dynamics 365 ve SharePoint List kullanarak 1400 ve daha yüksek uyumluluk düzeyindeki tablolu modeller için OAuth kimlik bilgilerini destekler. Azure Analiz Hizmetleri, uzun süren yenileme işlemleri için zaman ayarı önlemek için OAuth veri kaynakları için belirteç yenilemeişlemlerini yönetir. Geçerli belirteçler oluşturmak için, SSMS kullanarak kimlik bilgilerini ayarlayın.

Doğrudan Sorgu modu OAuth kimlik bilgileriyle desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi ağ geçidi](analysis-services-gateway.md)   
[Sunucunuzu yönetme](analysis-services-manage.md)   

