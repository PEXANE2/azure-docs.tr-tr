---
title: Koşullu Erişim
description: Azure SQL veritabanı ve Azure SYNAPSE için koşullu erişimi yapılandırma hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sql-data-warehouse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 02/06/2020
tag: azure-synpase
ms.openlocfilehash: f2431ee7c62079a3691a5ea99e562460df8f9309
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78197581"
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-azure-synapse-analytics"></a>Azure SQL veritabanı ve Azure SYNAPSE Analytics ile koşullu erişim (MFA)

Azure [SQL veritabanı](sql-database-technical-overview.md), [yönetilen örnek](sql-database-managed-instance.md)ve [Azure SYNAPSE](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) , Microsoft koşullu erişimini destekler. 

> [!NOTE]
> Bu konu, Azure SQL Server ve Azure SQL Server 'da oluşturulan hem SQL veritabanı hem de Azure SYNAPSE için geçerlidir. Basitlik için SQL veritabanı hem SQL veritabanı hem de Azure SYNAPSE 'a başvurulduğunda kullanılır.

Aşağıdaki adımlarda, SQL Database 'in koşullu erişim ilkesini zorlamak üzere nasıl yapılandırılacağı gösterilmektedir.  

## <a name="prerequisites"></a>Önkoşullar  
- Azure Active Directory kimlik doğrulamasını desteklemek için Azure SYNAPSE 'de SQL veritabanınızı veya SQL havuzunu yapılandırmanız gerekir. Belirli adımlar için bkz. [SQL veritabanı veya Azure SYNAPSE ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](sql-database-aad-authentication-configure.md).  
- Multi-Factor Authentication etkin olduğunda, en son SSMS gibi desteklenen bir araçla bağlanmanız gerekir. Daha fazla bilgi için bkz. [SQL Server Management Studio Için Azure SQL veritabanı Multi-Factor Authentication 'ı yapılandırma](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Azure SQL DB/DW için CA 'yı yapılandırma  
1. Portalda oturum açın, **Azure Active Directory**' yi seçin ve **koşullu erişim**' i seçin. Daha fazla bilgi için bkz. [koşullu erişim teknik başvurusu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference).  
   Koşullu erişim dikey penceresini ![](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2. **Koşullu erişim ilkeleri** dikey penceresinde **Yeni ilke**' yi tıklatın, bir ad girin ve **kuralları Yapılandır**' ı tıklatın.  
3. **Atamalar**' ın altında **Kullanıcılar ve gruplar**' ı seçin, **Kullanıcılar ve gruplar**' ı seçin ve ardından koşullu erişim için kullanıcıyı veya grubu seçin. **Seç**' e ve ardından seçiminizi kabul etmek için **bitti** ' ye tıklayın.  
   ![kullanıcıları ve grupları seçin](./media/sql-database-conditional-access/select-users-and-groups.png)  

4. **Bulut uygulamaları**' nı seçin, **Uygulama Seç**' e tıklayın. Koşullu erişim için kullanılabilen tüm uygulamaları görürsünüz. **Azure SQL veritabanı**' nı seçin, alt kısımdaki **Seç**' e tıklayın ve ardından **bitti**' ye tıklayın.  
   SQL veritabanı](./media/sql-database-conditional-access/select-sql-database.png) ![seçin  
   Aşağıdaki üçüncü ekran görüntüsünde listelenen **Azure SQL veritabanı** 'nı bulamıyorsanız, aşağıdaki adımları izleyin:   
   - Bir AAD yönetici hesabı ile SSMS kullanarak Azure SQL DB/DW örneğinde oturum açın.  
   - `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`yürütün.  
   - AAD 'de oturum açın ve Azure SQL veritabanı ve Azure SYNAPSE ' nin AAD 'nizdeki uygulamalarda listelendiğini doğrulayın.  

5. **Erişim denetimleri**' ni seçin, **izin ver**' i seçin ve ardından uygulamak istediğiniz ilkeyi kontrol edin. Bu örnekte, **çok faktörlü kimlik doğrulaması gerektir**' i seçeceğiz.  
   ![erişim ver ' i seçin](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Özet  
Azure AD Premium kullanarak Azure SQL DB/DW 'ye bağlanmasına izin veren seçili uygulama (Azure SQL veritabanı), artık seçili Koşullu erişim ilkesini zorunlu kılar, **çok faktörlü kimlik doğrulaması gerekir.**  
Multi-Factor Authentication ile ilgili Azure SQL veritabanı ve Azure SYNAPSE hakkında sorularınız için MFAforSQLDB@microsoft.combaşvurun.  

## <a name="next-steps"></a>Sonraki adımlar  

Öğretici için bkz. [Azure SQL veritabanınızı güvenli hale getirme](sql-database-security-tutorial.md).
