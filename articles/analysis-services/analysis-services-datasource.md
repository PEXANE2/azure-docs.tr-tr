---
title: Azure Analysis Services desteklenen veri kaynakları | Microsoft Docs
description: Azure Analysis Services tablo 1200 ve daha yüksek veri modelleri için desteklenen veri kaynaklarını ve bağlayıcıları açıklar.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b08a124ade6e2db8ca27ef61c7f5a6b3fe839885
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75442773"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Azure Analysis Services desteklenen veri kaynakları

Visual Studio 'da Analysis Services projelerine sahip veri veya tablo Içeri aktarma Sihirbazı ' nda gösterilen veri kaynakları ve bağlayıcılar hem Azure Analysis Services hem de SQL Server Analysis Services için gösterilmektedir. Ancak, gösterilen tüm veri kaynakları ve bağlayıcılar Azure Analysis Services desteklenmez. Bağlantı alabileceğiniz veri kaynağı türleri model uyumluluk düzeyi, kullanılabilir veri bağlayıcıları, kimlik doğrulama türü ve şirket içi veri ağ geçidi desteği gibi birçok faktöre bağlıdır. Aşağıdaki tablolarda Azure Analysis Services için desteklenen veri kaynakları açıklanır.

## <a name="azure-data-sources"></a>Azure veri kaynakları

|Veri kaynağı  |Bellek içi  |DirectQuery  |Notlar |
|---------|---------|---------|---------|
|Azure SQL Veritabanı      |   Evet      |    Evet      |<sup>[2](#azprovider)</sup>, <sup> [3](#azsqlmanaged)</sup>|
|Azure SQL Veri Ambarı      |   Evet      |   Evet       |<sup>[iki](#azprovider)</sup>|
|Azure Blob Depolama      |   Evet       |    Hayır      | <sup>[1](#tab1400a)</sup> |
|Azure Table Storage     |   Evet       |    Hayır      | <sup>[1](#tab1400a)</sup>|
|Azure Cosmos DB     |  Evet        |  Hayır        |<sup>[1](#tab1400a)</sup> |
|Azure Data Lake Store Gen1      |   Evet       |    Hayır      |<sup>[1](#tab1400a)</sup> |
|Azure Data Lake Store Gen2       |   Evet       |    Hayır      |<sup>[1](#tab1400a)</sup>, <sup> [5](#gen2)</sup>|
|Azure HDInsight.    |     Evet     |   Hayır       |<sup>[1](#tab1400a)</sup> |
|Azure HDInsight Spark     |   Evet       |   Hayır       |<sup>[1](#tab1400a)</sup>, <sup> [4](#databricks)</sup>|
||||

**Notlar:**    
yalnızca <a name="tab1400a">1</a> tablolu 1400 ve daha yüksek modeller.  
<a name="azprovider">2</a> -tablo 1200 ve daha yüksek modellerde bir *sağlayıcı* veri kaynağı olarak belirtildiğinde, hem bellek içi hem de DirectQuery modelleri, SQL Server msoledbsql (önerilen), SQL Server Native Client 11,0 veya veri sağlayıcısı Için .NET Framework SQL Server için Microsoft OLE DB sürücüsü gerektirir.    
<a name="azsqlmanaged">3</a> -Azure SQL veritabanı yönetilen örneği desteklenir. Yönetilen örnek, Azure VNet 'te özel bir IP adresi ile çalıştığı için, örnekte ortak uç noktanın etkinleştirilmesi gerekir. Etkinleştirilmemişse, Şirket [içi veri ağ geçidi](analysis-services-gateway.md) gerekir.    
<a name="databricks">4</a> -Spark bağlayıcısını kullanan Azure Databricks Şu anda desteklenmiyor.   
<a name="gen2">5</a> -ADLS 2. Bağlayıcısı Şu anda desteklenmiyor, ancak Azure Blob depolama bağlayıcısı bir ADLS 2. veri kaynağıyla birlikte kullanılabilir.   

## <a name="other-data-sources"></a>Diğer veri kaynakları

|Veri kaynağı | Bellek içi | DirectQuery |Notlar   |
|  --- | --- | --- | --- |
|Access Veritabanı     |  Evet | Hayır |  |
|Active Directory     |  Evet | Hayır | <sup>[inç](#tab1400b)</sup>  |
|Analysis Services     |  Evet | Hayır |  |
|Analytics platform sistemi     |  Evet | Hayır |  |
|CSV dosyası  |Evet | Hayır |  |
|Dynamics 365     |  Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
|Excel çalışma kitabı     |  Evet | Hayır |  |
|Değiştirin      |  Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
|Klasör      |Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
|IBM Informix  |Evet | Hayır |  |
|JSON belgesi      |  Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
|İkili dosyadan satırlar      | Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
|MySQL Veritabanı     | Evet | Hayır |  |
|OData Akışı      |  Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
|ODBC sorgusu     | Evet | Hayır |  |
|OLE DB     |   Evet | Hayır |  |
|Oracle  | Evet  |Evet  | <sup>[tuşlarına](#oracle)</sup> |
|PostgreSQL Veritabanı   | Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
|Salesforce Nesneleri|  Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
|Salesforce Raporları |Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
|SAP HANA     |  Evet | Hayır |  |
|SAP Business Warehouse    |  Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
|SharePoint Listesi      |   Evet | Hayır | <sup>[6](#tab1400b)</sup>, <sup> [11](#filesSP)</sup> |
|SQL Server |Evet   | Evet  | <sup>[7](#sqlim)</sup>, <sup> [8](#instgw)</sup> | 
|SQL Server veri ambarı |Evet   | Evet  | <sup>[7](#sqlim)</sup>, <sup> [8](#instgw)</sup> |
|Sybase Veritabanı     |  Evet | Hayır |  |
|Teradata | Evet  | Evet  | <sup>[(](#teradata)</sup> |
|TXT dosyası  |Evet | Hayır |  |
|XML tablosu    |  Evet | Hayır | <sup>[inç](#tab1400b)</sup> |
| | | |

**Notlar:**    
yalnızca <a name="tab1400b">6</a> tablolu 1400 ve daha yüksek modeller.  
<a name="sqlim">7</a> -tabular 1200 ve üzeri modellerde *sağlayıcı* veri kaynağı olarak belirtildiğinde, SQL Server Msoledbsql için Microsoft OLE DB sürücüsü (önerilen), SQL Server Native Client 11,0 veya veri sağlayıcısı için .NET Framework SQL Server belirtin.  
<a name="instgw">8</a> -veri sağlayıcısı olarak MSOLEDBSQL belirtildiğinde, [SQL Server Için Microsoft OLE DB sürücüsünü](https://docs.microsoft.com/sql/connect/oledb/oledb-driver-for-sql-server) şirket içi veri ağ geçidiyle aynı bilgisayara indirip yüklemek gerekebilir.  
<a name="oracle">9</a> -tabular 1200 modelleri için ya da tablosal 1400 + modelleriyle bir *sağlayıcı* veri kaynağı olarak, .NET için Oracle veri sağlayıcısı belirtin.  
<a name="teradata">10</a> tablolu 1200 modellerdeki veya tablo 1400 + modelleriyle bir *sağlayıcı* veri kaynağı olarak, .NET için Teradata veri sağlayıcısı belirtin.   
<a name="filesSP">11</a> -şirket içi SharePoint 'teki dosyalar desteklenmez.

Bir Azure Analysis Services sunucusundan şirket içi veri kaynaklarına bağlanmak için şirket [içi ağ geçidi](analysis-services-gateway.md)gerekir. Bir ağ geçidi kullanırken, 64 bit sağlayıcılar gereklidir. 

## <a name="understanding-providers"></a>Sağlayıcıları anlama

Visual Studio 'da tablo 1400 ve üzeri model projeleri oluştururken, varsayılan olarak veri **Al**seçeneğini kullanarak bir veri kaynağına bağlanırken bir veri sağlayıcısı belirtmeyin. Tablo 1400 ve üzeri modeller, veri kaynağı ile Analysis Services arasında bağlantıları, veri sorgularını ve karmalar yönetmek için [Power Query](/power-query/power-query-what-is-power-query) bağlayıcıları kullanır. Bunlar bazen bu bağlantı özelliği ayarlarındaki *yapılandırılmış* veri kaynağı bağlantıları sizin için ayarlanmış olarak adlandırılır. Ancak, eski veri kaynaklarını etkinleştirebilirsiniz. Etkinleştirildiğinde, tablo 1200 ' de desteklenen belirli veri kaynaklarına bağlanmak için **tablo Içeri aktarma Sihirbazı** ' nı ve *eski*veya *sağlayıcı* veri kaynakları olarak daha düşük modelleri kullanabilirsiniz. Sağlayıcı veri kaynağı olarak belirtildiğinde, belirli bir veri sağlayıcısı ve diğer gelişmiş bağlantı özelliklerini belirtebilirsiniz. Örneğin, şirket içi SQL Server veri ambarına veya hatta Azure SQL veritabanı 'na eski veri kaynağı olarak bağlanabilirsiniz. Daha sonra SQL Server MSOLEDBSQL veri sağlayıcısı OLE DB sürücüsünü seçebilirsiniz. Bu durumda, bir OLE DB veri sağlayıcısı seçildiğinde Power Query Bağlayıcısı üzerinden geliştirilmiş performans sağlayabilirsiniz. 

Visual Studio 'da tablo Içeri aktarma Sihirbazı 'nı kullanırken, herhangi bir veri kaynağına yönelik bağlantılar bir veri sağlayıcısı gerektirir. Sizin için varsayılan bir veri sağlayıcısı seçilidir. Gerekirse veri sağlayıcısını değiştirebilirsiniz. Seçtiğiniz sağlayıcının türü, modelin bellek içi depolama alanı veya DirectQuery kullanıp kullanmadığını ve modelinize dağıttığınız Analysis Services platformunu kullanarak, performansa bağlı olabilir.

### <a name="specify-provider-data-sources-in-tabular-1400-and-higher-model-projects"></a>Tablo 1400 ve daha yüksek model projelerinde sağlayıcı veri kaynaklarını belirtin

Sağlayıcı veri kaynaklarını etkinleştirmek için, Visual Studio 'da **araçlar** > **Seçenekler** > **Analysis Services tablolu** > **verileri içeri aktar**' a tıklayın, **eski veri kaynaklarını etkinleştir**' i seçin.

![Eski veri kaynaklarını etkinleştir](media/analysis-services-datasource/aas-enable-legacy-datasources.png)

Eski veri kaynakları etkinken **tablosal Model Gezgini**' nde veri **kaynağına (eski) Içeri aktarma** > **veri kaynakları** ' na sağ tıklayın.

![Tablosal model Gezgininde eski veri kaynakları](media/analysis-services-datasource/aas-import-legacy-datasources.png)

Tablolu 1200 model projelerinde olduğu gibi, bir veri kaynağına bağlanmak için **tablo Içeri aktarma Sihirbazı 'nı** kullanın. Bağlan sayfasında **Gelişmiş**' e tıklayın. Veri sağlayıcısını ve diğer bağlantı ayarlarını **Gelişmiş Özellikler ayarla**' da belirtin.

![Eski veri kaynakları gelişmiş özellikler](media/analysis-services-datasource/aas-import-legacy-advanced.png)


## <a name="impersonation"></a>Kimliğe bürünme
Bazı durumlarda, farklı bir kimliğe bürünme hesabı belirtmek gerekli olabilir. Kimliğe bürünme hesabı, Visual Studio veya SSMS 'de belirtilebilir.

Şirket içi veri kaynakları için:

* SQL kimlik doğrulaması kullanılıyorsa, kimliğe bürünme hizmet hesabı olmalıdır.
* Windows kimlik doğrulaması kullanıyorsanız, Windows Kullanıcı/parola ayarlayın. SQL Server için, belirli bir kimliğe bürünme hesabıyla Windows kimlik doğrulaması yalnızca bellek içi veri modelleri için desteklenir.

Bulut veri kaynakları için:

* SQL kimlik doğrulaması kullanılıyorsa, kimliğe bürünme hizmet hesabı olmalıdır.

## <a name="oauth-credentials"></a>OAuth kimlik bilgileri

1400 ve üzeri uyumluluk düzeyindeki tablo modelleriyle ilgili olarak, Azure SQL veritabanı, Azure SQL veri ambarı, Dynamics 365 ve SharePoint listesi OAuth kimlik bilgilerini destekler. Azure Analysis Services uzun süre çalışan yenileme işlemleri için zaman aşımlarını önlemek üzere OAuth veri kaynakları için belirteç yenilemeyi yönetir. Geçerli belirteçler oluşturmak için SSMS kullanarak kimlik bilgilerini ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi ağ geçidi](analysis-services-gateway.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)   

