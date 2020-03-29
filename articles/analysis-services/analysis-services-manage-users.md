---
title: Azure Analiz Hizmetleri kimlik doğrulama ve kullanıcı izinleri| Microsoft Dokümanlar
description: Bu makalede, Azure Çözümleme Hizmetleri'nin kimlik yönetimi ve kullanıcı kimlik doğrulaması için Azure Etkin Dizinini (Azure AD) nasıl kullandığı açıklanmaktadır.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4a054c3c042e18f1679acd75e5ba5ad74f66edff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73572760"
---
# <a name="authentication-and-user-permissions"></a>Kimlik doğrulaması ve kullanıcı izinleri

Azure Analiz Hizmetleri, kimlik yönetimi ve kullanıcı kimlik doğrulaması için Azure Active Directory (Azure AD) kullanır. Azure Analiz Hizmetleri sunucusu oluşturan, yöneten veya bir Sunucu'ya bağlanan tüm kullanıcı, aynı abonelikteki bir [Azure AD kiracısında](../active-directory/fundamentals/active-directory-administer.md) geçerli bir kullanıcı kimliğine sahip olmalıdır.

Azure Analiz [Hizmetleri, Azure AD B2B işbirliğini](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)destekler. B2B ile, bir kuruluşun dışından gelen kullanıcılar Azure REKLAM dizininde konuk kullanıcı olarak davet edilebilir. Konuklar başka bir Azure AD kiracı dizininden veya geçerli bir e-posta adresinden olabilir. Davet edildikten ve kullanıcı Azure'dan e-posta ile gönderilen daveti kabul ettikten sonra, kullanıcı kimliği kiracı dizinine eklenir. Bu kimlikler güvenlik gruplarına veya sunucu yöneticisi veya veritabanı rolünün üyeleri olarak eklenebilir.

![Azure Analiz Hizmetleri kimlik doğrulama mimarisi](./media/analysis-services-manage-users/aas-manage-users-arch.png)

## <a name="authentication"></a>Kimlik doğrulaması

Tüm istemci uygulamaları ve araçları, bir sunucuya bağlanmak için Çözümleme Hizmetleri [istemci kitaplıklarından](analysis-services-data-providers.md) birini (AMO, MSOLAP, ADOMD) kullanır. 

Üç istemci kitaplıkları da hem Azure AD etkileşimli akışı hem de etkileşimli olmayan kimlik doğrulama yöntemlerini destekler. Etkileşimli olmayan iki yöntem, Active Directory Password ve Active Directory Integrated Authentication yöntemleri AMOMD ve MSOLAP kullanan uygulamalarda kullanılabilir. Bu iki yöntem hiçbir zaman açılır iletişim kutularıyla sonuçlanmaz.

Excel ve Power BI Desktop gibi istemci uygulamaları ve Visual Studio için SSMS ve Analiz Hizmetleri projeleri uzantısı gibi araçlar, en son sürümde güncelleştirildiğinde kitaplıkların en son sürümlerini yükler. Power BI Desktop, SSMS ve Analiz Hizmetleri proje uzantısı aylık olarak güncellenir. Excel [Office 365 ile güncelleştirilir.](https://support.office.com/article/When-do-I-get-the-newest-features-in-Office-2016-for-Office-365-da36192c-58b9-4bc9-8d51-bb6eed468516) Office 365 güncelleştirmeleri daha az sıklıktadır ve bazı kuruluşlar ertelenmiş kanalı kullanır, bu da güncelleştirmelerin üç aya kadar ertelendiği anlamına gelir.

Kullandığınız istemci uygulamasına veya araca bağlı olarak, kimlik doğrulamanın türü ve oturum açma şekliniz farklı olabilir. Her uygulama, Azure Analiz Hizmetleri gibi bulut hizmetlerine bağlanmak için farklı özellikleri destekleyebilir.

Power BI Desktop, Visual Studio ve SSMS, Azure Çok Faktörlü Kimlik Doğrulamayı (MFA) destekleyen etkileşimli bir yöntem olan Active Directory Universal Authentication'ı destekler. Azure MFA, basit bir oturum açma işlemi sağlarken verilere ve uygulamalara erişimi korumaya yardımcı olur. Çeşitli doğrulama seçenekleri (telefon görüşmesi, kısa mesaj, pinli akıllı kartlar veya mobil uygulama bildirimi) ile güçlü kimlik doğrulaması sağlar. Azure AD ile etkileşimli MFA, doğrulama için açılır iletişim kutusuyla sonuçlanabilir. **Evrensel Kimlik Doğrulama önerilir.**

Bir Windows hesabı kullanarak Azure'da oturum açma ve Evrensel Kimlik Doğrulama seçili veya kullanılabilir (Excel) yoksa, [Active Directory Federation Services (AD FS)](../active-directory/hybrid/how-to-connect-fed-azure-adfs.md) gereklidir. Federation ile Azure AD ve Office 365 kullanıcıları şirket içi kimlik bilgileri kullanılarak kimlik doğrulanır ve Azure kaynaklarına erişebilir.

### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)

Azure Çözümleme Hizmetleri sunucuları, Windows Kimlik Doğrulaması, Active Directory Password Authentication ve Active Directory Universal Authentication kullanarak [SSMS V17.1](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve üzeri bağlantılara destek sağlar. Genel olarak, Active Directory Universal Authentication'ı kullanmanız önerilir, çünkü:

*  Etkileşimli ve etkileşimli olmayan kimlik doğrulama yöntemlerini destekler.

*  Azure AS kiracısına davet edilen Azure B2B konuk kullanıcılarını destekler. Bir sunucuya bağlanırken, konuk kullanıcıların sunucuya bağlanırken Active Directory Universal Authentication'ı seçmesi gerekir.

*  Çok Faktörlü Kimlik Doğrulamayı (MFA) destekler. Azure MFA, telefon görüşmesi, kısa mesaj, pinli akıllı kartlar veya mobil uygulama bildirimi gibi çeşitli doğrulama seçenekleriyle verilere ve uygulamalara erişimi korumaya yardımcı olur. Azure AD ile etkileşimli MFA, doğrulama için açılır iletişim kutusuyla sonuçlanabilir.

### <a name="visual-studio"></a>Visual Studio

Visual Studio, MFA desteği ile Active Directory Universal Authentication'ı kullanarak Azure Analiz Hizmetleri'ne bağlanır. Kullanıcıların ilk dağıtımda Azure'da oturum açmaları istenir. Kullanıcıların, dağıtta oldukları sunucuda sunucu yöneticisi izinleri olan bir hesapla Azure'da oturum açmaları gerekir. Azure'da ilk kez oturum açıldığında bir belirteç atanır. Belirteç, gelecekteki yeniden bağlanmalar için bellekte önbelleğe alınmış.

### <a name="power-bi-desktop"></a>Power BI Desktop

Power BI Desktop, MFA desteği ile Active Directory Universal Authentication'ı kullanarak Azure Analiz Hizmetleri'ne bağlanır. Kullanıcıların ilk bağlantıda Azure'da oturum açmaları istenir. Kullanıcıların, sunucu yöneticisi veya veritabanı rolüne dahil edilmiş bir hesapla Azure'da oturum açmaları gerekir.

### <a name="excel"></a>Excel

Excel kullanıcıları bir Windows hesabı, kuruluş kimliği (e-posta adresi) veya harici bir e-posta adresi kullanarak bir sunucuya bağlanabilir. Azure AD'de konuk kullanıcı olarak harici e-posta kimlikleri bulunmalıdır.

## <a name="user-permissions"></a>Kullanıcı izinleri

**Sunucu yöneticileri,** Azure Çözümleme Hizmetleri sunucu örneğine özgür. Veritabanları ekleme ve kullanıcı rollerini yönetme gibi görevleri gerçekleştirmek için Azure portalı, SSMS ve Visual Studio gibi araçlarla bağlantı kurarlar. Varsayılan olarak, sunucuyu oluşturan kullanıcı otomatik olarak Çözümhizmetleri sunucu yöneticisi olarak eklenir. Diğer yöneticiler Azure portalı veya SSMS kullanılarak eklenebilir. Sunucu yöneticilerinin aynı abonelikte Azure AD kiracısında bir hesabı olmalıdır. Daha fazla bilgi için [sunucu yöneticilerini yönet'e](analysis-services-server-admins.md)bakın. 

**Veritabanı kullanıcıları** Excel veya Power BI gibi istemci uygulamalarını kullanarak model veritabanlarına bağlanır. Kullanıcıların veritabanı rollerine eklenmesi gerekir. Veritabanı rolleri, veritabanı için yönetici, işlem veya okuma izinlerini tanımlar. Yönetici izinleri olan bir roldeki veritabanı kullanıcılarının sunucu yöneticilerinden farklı olduğunu anlamak önemlidir. Ancak, varsayılan olarak, sunucu yöneticileri de veritabanı yöneticileridir. Daha fazla bilgi edinmek için [veritabanı rollerini ve kullanıcılarını yönet'e](analysis-services-database-users.md)bakın.

**Azure kaynak sahipleri.** Kaynak sahipleri, Azure aboneliği için kaynakları yönetir. Kaynak sahipleri, Azure portalında **Access denetimini** kullanarak veya Azure Kaynak Yöneticisi şablonlarıyla abonelik içindeki Sahibiveya Katılımcı Rolleri'ne Azure AD kullanıcı kimlikleri ekleyebilir. 

![Azure portalında erişim denetimi](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

Bu düzeydeki roller, portalda tamamlanabilecek görevleri gerçekleştirmesi gereken kullanıcılar veya hesaplar veya Azure Kaynak Yöneticisi şablonları kullanılarak geçerlidir. Daha fazla bilgi için [Rol Tabanlı Erişim Denetimi'ne](../role-based-access-control/overview.md)bakın. 

## <a name="database-roles"></a>Veritabanı rolleri

 Bir tabular model için tanımlanan roller veritabanı rolleridir. Diğer bir deyişle, roller, Azure AD kullanıcılarından ve bu üyelerin bir model veritabanında alabilecekleri eylemi tanımlayan belirli izinlere sahip güvenlik gruplarından oluşan üyeleri içerir. Veritabanı rolü, veritabanında ayrı bir nesne olarak oluşturulur ve yalnızca bu rolün oluşturulduğu veritabanı için geçerli olur.   
  
 Varsayılan olarak, yeni bir tabular model projesi oluşturduğunuzda, model projenin herhangi bir rolü yoktur. Roller Visual Studio'daki Role Manager iletişim kutusu kullanılarak tanımlanabilir. Roller model proje tasarımı sırasında tanımlandığında, yalnızca model çalışma alanı veritabanına uygulanır. Model dağıtıldığında, aynı roller dağıtılan modele uygulanır. Bir model dağıtıldıktan sonra, sunucu ve veritabanı yöneticileri SSMS kullanarak rolleri ve üyeleri yönetebilir. Daha fazla bilgi edinmek için [veritabanı rollerini ve kullanıcılarını yönet'e](analysis-services-database-users.md)bakın.
  
## <a name="next-steps"></a>Sonraki adımlar

[Azure Etkin Dizin gruplarıyla kaynaklara erişimi yönetme](../active-directory/fundamentals/active-directory-manage-groups.md)   
[Veritabanı rollerini ve kullanıcılarını yönetme](analysis-services-database-users.md)  
[Sunucu yöneticilerini yönetme](analysis-services-server-admins.md)  
[Rol Tabanlı Erişim Kontrolü](../role-based-access-control/overview.md)  