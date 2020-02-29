---
title: Kimlik Doğrulaması
description: Azure Active Directory (AAD) veya SQL Server kimlik doğrulaması kullanarak Azure SYNAPSE Analytics 'te kimlik doğrulaması yapmayı öğrenin.
services: sql-data-warehouse
author: julieMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/02/2019
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019
tag: azure-synapse
ms.openlocfilehash: 235d13cfd6f33830d832a6a79e3bc1c78bbfe53e
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78195983"
---
# <a name="authenticate-to-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te kimlik doğrulama
Azure Active Directory (AAD) veya SQL Server kimlik doğrulaması kullanarak Azure 'da SQL Analytics 'te kimlik doğrulaması yapmayı öğrenin.

Bir SQL havuzuna bağlanmak için kimlik doğrulama amacıyla güvenlik kimlik bilgilerini geçirmeniz gerekir. Bir bağlantı kurulduktan sonra, sorgu oturumunuzu oluşturma işleminin parçası olarak belirli bağlantı ayarları yapılandırılır.  

Güvenlik ve veri Ambarınızla bağlantıları etkinleştirme hakkında daha fazla bilgi için bkz. [veritabanı belgelerinin güvenliğini sağlama](sql-data-warehouse-overview-manage-security.md).

## <a name="sql-authentication"></a>SQL kimlik doğrulaması
SQL havuzuna bağlanmak için aşağıdaki bilgileri sağlamanız gerekir:

* Tam nitelenmiş ServerName
* SQL kimlik doğrulaması belirtin
* Kullanıcı adı
* Parola
* Varsayılan veritabanı (isteğe bağlı)

Varsayılan olarak, bağlantınız Kullanıcı veritabanınıza değil *ana* veritabanına bağlanır. Kullanıcı veritabanınıza bağlanmak için iki işlemlerden birini seçebilirsiniz:

* Sunucunuzu SSDT, SSMS veya uygulama bağlantı dizeniz içindeki SQL Server Nesne Gezgini kaydederken varsayılan veritabanını belirtin. Örneğin, bir ODBC bağlantısı için InitialCatalog parametresini ekleyin.
* SSDT 'de oturum oluşturmadan önce kullanıcı veritabanını vurgulayın.

> [!NOTE]
> Transact-SQL ifadesinin **MyDatabase; kullanımı** , bir bağlantının veritabanını değiştirmek için desteklenmez. SSDT ile bir SQL havuzuna bağlanma Kılavuzu için, [Visual Studio Ile sorgu](sql-data-warehouse-query-visual-studio.md) makalesini inceleyin.
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory (AAD) kimlik doğrulaması
[Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) kimlik doğrulaması, Azure Active Directory (Azure AD) KIMLIKLERI kullanılarak SQL havuzuna bağlanma mekanizmasıdır. Azure Active Directory kimlik doğrulaması ile, veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin kimliklerini tek bir merkezi konumda merkezi olarak yönetebilirsiniz. Merkezi KIMLIK yönetimi, Azure SYNAPSE kullanıcılarını yönetmek ve izin yönetimini basitleştireceğinizi sağlayan tek bir yer sağlar. 

### <a name="benefits"></a>Avantajlar
Azure Active Directory avantajları şunlardır:

* SQL Server kimlik doğrulaması için bir alternatif sağlar.
* Veritabanı sunucuları arasında kullanıcı kimliklerinin uzamasını durdurmaya yardımcı olur.
* Tek konumda parola dönüşüne olanak sağlar
* Dış (AAD) gruplarını kullanarak veritabanı izinlerini yönetin.
* Tümleşik Windows kimlik doğrulamasını ve Azure Active Directory tarafından desteklenen diğer kimlik doğrulama biçimlerini etkinleştirerek parolaların depolanmasını ortadan kaldırır.
* , Veritabanı düzeyinde kimliklerin kimliğini doğrulamak için kapsanan veritabanı kullanıcılarını kullanır.
* SQL havuzuna bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
* [SQL Server Management Studio](../sql-database/sql-database-ssms-mfa-authentication.md) ve [SQL Server veri araçları](https://docs.microsoft.com/sql/ssdt/azure-active-directory?toc=/azure/sql-data-warehouse/toc.json)gibi çeşitli araçlar Için Active Directory evrensel kimlik doğrulaması aracılığıyla çok faktörlü kimlik doğrulamasını destekler.

> [!NOTE]
> Azure Active Directory nispeten yenidir ve bazı sınırlamalar vardır. Azure Active Directory ortamınıza uygun olduğundan emin olmak için bkz. [Azure AD özellikleri ve sınırlamaları](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), özellikle de ek konular.
> 
> 

### <a name="configuration-steps"></a>Yapılandırma adımları
Azure Active Directory kimlik doğrulamasını yapılandırmak için aşağıdaki adımları izleyin.

1. Azure Active Directory oluşturma ve doldurma
2. İsteğe bağlı: Şu anda Azure aboneliğinizle ilişkili olan Active Directory 'yi Ilişkilendir veya Değiştir
3. Azure SYNAPSE için Azure Active Directory Yöneticisi oluşturma
4. İstemci bilgisayarlarınızı yapılandırma
5. Veritabanınızda Azure AD kimlikleriyle eşlenen kapsanan veritabanı kullanıcıları oluşturun
6. Azure AD kimliklerini kullanarak SQL havuzunuza bağlanma

Şu anda Azure Active Directory kullanıcılar SSDT Nesne Gezgini içinde gösterilmez. Geçici bir çözüm olarak, [sys. database_principals](https://msdn.microsoft.com/library/ms187328.aspx)içindeki kullanıcıları görüntüleyin.

### <a name="find-the-details"></a>Ayrıntıları bul
* Azure Active Directory kimlik doğrulamasını yapılandırma ve kullanma adımları Azure SYNAPSE Azure SQL veritabanı ve SQL Analytics için neredeyse aynıdır. [Azure Active Directory kimlik doğrulaması kullanarak SQL veritabanı 'na veya SQL Pool 'A bağlanma](../sql-database/sql-database-aad-authentication.md)konusundaki ayrıntılı adımları izleyin.
* Özel veritabanı rolleri oluşturun ve rollere kullanıcı ekleyin. Ardından rollere ayrıntılı izinler verin. Daha fazla bilgi için bkz. [veritabanı altyapısı izinleri Ile çalışmaya](https://msdn.microsoft.com/library/mt667986.aspx)başlama.

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio ve diğer uygulamalarla sorgulamaya başlamak için bkz. [Visual Studio Ile sorgulama](sql-data-warehouse-query-visual-studio.md).
