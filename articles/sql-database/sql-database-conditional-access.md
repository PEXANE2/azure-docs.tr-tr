---
title: Koşullu Erişim
description: Azure SQL Veritabanı ve Azure Sinapse için Koşullu Erişimi nasıl yapılandırıştırmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.custom: sql-data-warehouse
ms.date: 02/06/2020
tag: azure-synpase
ms.openlocfilehash: cd56ccf2e6a4ceb0d81c25b5b9e795176be66b77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124902"
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-azure-synapse-analytics"></a>Azure SQL Veritabanı ve Azure Synapse Analytics ile Koşullu Erişim (MFA)

Azure [SQL Veritabanı](sql-database-technical-overview.md), Yönetilen [Örnek](sql-database-managed-instance.md)ve [Azure Synapse](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) Microsoft Koşullu Erişimi destekler. 

> [!NOTE]
> Bu konu Azure SQL sunucusu ve Azure SQL sunucusunda oluşturulan SQL Veritabanı ve Azure Synapse için geçerlidir. Basitlik için, SQL Veritabanı hem SQL Veritabanı hem de Azure Synapse'ye atıfta bulunularak kullanılır.

Aşağıdaki adımlar, Koşullu Erişim ilkesini uygulamak için SQL Veritabanı'nın nasıl yapılandırılabildiğini gösterir.  

## <a name="prerequisites"></a>Ön koşullar  
- Azure Sinaps'taki SQL Veritabanınızı veya SQL havuzunuzu Azure Active Directory kimlik doğrulamasını destekleyecek şekilde yapılandırmanız gerekir. Belirli adımlar için, [SQL Veritabanı veya Azure Sinaps'ı ile Azure Etkin Dizin kimlik doğrulamasını yapılandırma ve yönetme](sql-database-aad-authentication-configure.md)konusuna bakın.  
- Çok faktörlü kimlik doğrulaması etkinleştirildiğinde, en son SSMS gibi desteklenen araca bağlanmanız gerekir. Daha fazla bilgi için, [SQL Server Management Studio için Azure SQL Veritabanı'nı yapılandırma çok faktörlü kimlik doğrulama sı'na](sql-database-ssms-mfa-authentication-configure.md)bakın.  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Azure SQL DB/DW için CA'yı yapılandırma  
1. Portal'da oturum açın, **Azure Etkin Dizini'ni**seçin ve ardından **Koşullu Erişim'i**seçin. Daha fazla bilgi için Azure [Active Directory Koşullu Erişim teknik başvurusuna](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference)bakın.  
   ![Koşullu Erişim bıçağı](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2. **Koşullu Erişim İlkeleri** bıkuralında, **Yeni ilkeyi**tıklatın, bir ad sağlayın ve sonra **Kuralları Yapıla'yı**tıklatın.  
3. **Atamalar**altında, **Kullanıcıları ve grupları**seçin, kullanıcıları ve grupları **seçin**ve ardından Koşullu Erişim için kullanıcıyı veya grubu seçin. **Seçiminizi**kabul etmek için Seç'i ve ardından **Bitti'yi** tıklatın.  
   ![kullanıcıları ve grupları seçin](./media/sql-database-conditional-access/select-users-and-groups.png)  

4. **Bulut uygulamalarını**seçin , **Uygulamaları Seç'i**tıklatın. Koşullu Erişim için kullanılabilen tüm uygulamaları görürsünüz. **Azure SQL Veritabanı'nı**seçin , alttaki **Seç'i**tıklatın ve ardından **Bitti'yi**tıklatın.  
   ![SQL Veritabanı'nı seçin](./media/sql-database-conditional-access/select-sql-database.png)  
   Aşağıdaki üçüncü ekran görüntüsünde listelenen **Azure SQL Veritabanını** bulamazsanız, aşağıdaki adımları tamamlayın:   
   - AAD yönetici hesabıyla SSMS kullanarak Azure SQL DB/DW örneğinde oturum açın.  
   - Çalıştırın. `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`  
   - AAD'de oturum açın ve Azure SQL Veritabanı ve Azure Sinaps'ınA aad'nizdeki uygulamalarda listelenmiş olduğunu doğrulayın.  

5. **Access denetimlerini**seçin, **Hibe'yi**seçin ve ardından uygulamak istediğiniz ilkeyi denetleyin. Bu örnekiçin, **çok faktörlü kimlik doğrulaması gerektirir'i**seçeriz.  
   ![hibe erişimini seçin](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Özet  
Azure AD Premium kullanarak Azure SQL DB/DW'ye bağlanmaya olanak tanıyan seçili uygulama (Azure SQL Veritabanı), artık seçili Koşullu Erişim ilkesi olan **Gerekli çok faktörlü kimlik doğrulaması** uygulamasını uygular.  
Azure SQL Veritabanı ve Azure Synapse ile ilgili çok MFAforSQLDB@microsoft.comfaktörlü kimlik doğrulama ile ilgili sorularınız için başvurun.  

## <a name="next-steps"></a>Sonraki adımlar  

Bir öğretici için Azure [SQL Veritabanınızı Güvenli'ye](sql-database-security-tutorial.md)bakın.
