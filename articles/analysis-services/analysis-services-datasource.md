---
title: Azure Analysis Services desteklenen veri kaynakları | Microsoft Docs
description: Azure Analysis Services veri modelleri için desteklenen veri kaynaklarını açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 81fc73ffd61a49eae1c4f107733b6f9f53efbb4f
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70993382"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Azure Analysis Services desteklenen veri kaynakları

Visual Studio 'da veri alma veya Içeri aktarma Sihirbazı 'nda gösterilen veri kaynakları ve bağlayıcılar hem Azure Analysis Services hem de SQL Server Analysis Services için gösterilmektedir. Ancak, gösterilen tüm veri kaynakları ve bağlayıcılar Azure Analysis Services desteklenmez. Bağlantı kurmak için kullanabileceğiniz veri kaynağı türleri model uyumluluk düzeyi, kullanılabilir veri bağlayıcıları, kimlik doğrulama türü, sağlayıcılar ve şirket içi veri ağ geçidi desteği gibi birçok faktöre bağlıdır. 

## <a name="azure-data-sources"></a>Azure veri kaynakları

|Veri kaynağı  |Bellek içi  |DirectQuery  |
|---------|---------|---------|
|Azure SQL veritabanı<sup>[2](#azsqlmanaged)</sup>     |   Evet      |    Evet      |
|Azure SQL Veri Ambarı     |   Evet      |   Evet       |
|Azure Blob depolama<sup>[1](#tab1400a)</sup>     |   Evet       |    Hayır      |
|Azure Tablo depolama<sup>[1](#tab1400a)</sup>    |   Evet       |    Hayır      |
|Azure Cosmos DB<sup>[1](#tab1400a)</sup>     |  Evet        |  Hayır        |
|Azure Data Lake Store (Gen1)<sup>[1](#tab1400a)</sup>, <sup>[4](#gen2)</sup>      |   Evet       |    Hayır      |
|Azure HDInsight,<sup>[1](#tab1400a) .</sup>     |     Evet     |   Hayır       |
|Azure HDInsight Spark<sup>[1](#tab1400a)</sup>, <sup>[3](#databricks)</sup>     |   Evet       |   Hayır       |
||||

yalnızca <a name="tab1400a">1</a> tablolu 1400 ve daha yüksek modeller.   
<a name="azsqlmanaged">2</a> -Azure SQL veritabanı yönetilen örneği desteklenir. Yönetilen örnek, Azure VNet 'te özel bir IP adresi ile çalıştığı için, örnekte ortak uç noktanın etkinleştirilmesi gerekir. Etkinleştirilmemişse, şirket içi veri ağ geçidi gerekir.    
<a name="databricks">3</a> -Spark bağlayıcısını kullanan Azure Databricks Şu anda desteklenmiyor.   
<a name="gen2">4</a> -ADLS 2. Şu anda desteklenmiyor.


**Sağlayıcısını**   
Azure veri kaynaklarına bağlanan bellek içi ve DirectQuery modelleri SQL Server için .NET Framework Veri Sağlayıcısı kullanır.

## <a name="other-data-sources"></a>Diğer veri kaynakları

Ve Azure 'dan sunucuya şirket içi veri kaynaklarına bağlanmak için şirket içi ağ geçidi gerekir. Bir ağ geçidi kullanırken, 64 bit sağlayıcılar gereklidir.

### <a name="in-memory-and-directquery"></a>Bellek içi ve DirectQuery

|Veri kaynağı | Bellek içi sağlayıcı | DirectQuery sağlayıcısı |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11,0, SQL Server için Microsoft OLE DB sağlayıcısı, Veri Sağlayıcısı için .NET Framework SQL Server | SQL Server için .NET Framework Veri Sağlayıcısı |
| SQL Server veri ambarı |SQL Server Native Client 11,0, SQL Server için Microsoft OLE DB sağlayıcısı, Veri Sağlayıcısı için .NET Framework SQL Server | SQL Server için .NET Framework Veri Sağlayıcısı |
| Oracle | Oracle için OLE DB sağlayıcı, .NET için Oracle Veri Sağlayıcısı |.NET için Oracle Veri Sağlayıcısı |
| Teradata |Teradata için OLE DB sağlayıcısı, .NET için Teradata Veri Sağlayıcısı |.NET için Teradata Veri Sağlayıcısı |
| | | |

### <a name="in-memory-only"></a>Yalnızca bellek içi

|Veri kaynağı  |  
|---------|
|Veritabanına erişin     |  
|Active Directory<sup>[1](#tab1400b)</sup>     |  
|Analysis Services     |  
|Analytics platform sistemi     |  
|CSV dosyası  |
|Dynamics CRM<sup>[1](#tab1400b)</sup>     |  
|Excel çalışma kitabı     |  
|Exchange<sup>[1](#tab1400b)</sup>     |  
|Klasör<sup>[1](#tab1400b)</sup>     |
|IBM Informix<sup>[1](#tab1400b)</sup> (Beta) |
|JSON belgesi<sup>[1](#tab1400b)</sup>     |  
|İkili 1 ' den satırlar<sup>[](#tab1400b)</sup>     | 
|MySQL Veritabanı     | 
|OData akışı<sup>[1](#tab1400b)</sup>     |  
|ODBC sorgusu     | 
|OLE DB     |   
|PostgreSQL veritabanı<sup>[1](#tab1400b)</sup>    | 
|Salesforce nesneleri<sup>[1](#tab1400b)</sup> |  
|Salesforce raporları<sup>[1](#tab1400b)</sup> |
|SAP HANA<sup>[1](#tab1400b)</sup>    |  
|SAP Business Warehouse<sup>[1](#tab1400b)</sup>    |  
|SharePoint listesi<sup>[1](#tab1400b)</sup>, <sup>[2](#filesSP)</sup>     |   
|Sybase Veritabanı     |  
|TXT dosyası  |
|XML tablosu<sup>[1](#tab1400b)</sup>    |  
||
 
yalnızca <a name="tab1400b">1</a> tablolu 1400 ve daha yüksek modeller.   
<a name="filesSP">2</a> -şirket içi SharePoint 'teki dosyalar desteklenmez.

## <a name="specifying-a-different-provider"></a>Farklı bir sağlayıcı belirtme

Azure Analysis Services veri modelleri, belirli veri kaynaklarına bağlanırken farklı veri sağlayıcıları gerektirebilir. Bazı durumlarda, SQL Server Native Client (SQLNCLI11) gibi yerel sağlayıcıları kullanarak veri kaynaklarına bağlanan tablo modelleri bir hata döndürebilir. SQLOLEDB dışında yerel sağlayıcılar kullanılıyorsa, şu hata iletisini görebilirsiniz: **' SQLNCLI 11.1 ' sağlayıcısı kayıtlı değil**. Ya da şirket içi veri kaynaklarına bağlanan bir DirectQuery modeliniz varsa ve yerel sağlayıcılar kullanıyorsanız, şu hata iletisini görebilirsiniz: **OLE DB satır kümesi oluşturulurken hata oluştu. ' LIMIT '** yakınında yanlış sözdizimi.

Şirket içi SQL Server Analysis Services tablolu modeli Azure Analysis Services ' e geçirirken sağlayıcının değiştirilmesi gerekebilir.

**Sağlayıcı belirtmek için**

1. SSDT > **tablosal Model Gezgini** > **veri kaynakları**' nda, bir veri kaynağı bağlantısına sağ tıklayın ve ardından **veri kaynağını Düzenle**' ye tıklayın.
2. **Bağlantıyı Düzenle**' de Gelişmiş ' e tıklayarak **İleri** Özellikler penceresini açın.
3. **Gelişmiş Özellikler** > **sağlayıcılarını**ayarla bölümünde uygun sağlayıcıyı seçin.

## <a name="impersonation"></a>Kimliğe bürünme
Bazı durumlarda, farklı bir kimliğe bürünme hesabı belirtmek gerekli olabilir. Kimliğe bürünme hesabı, Visual Studio 'da (SSDT) veya SSMS 'de belirtilebilir.

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

