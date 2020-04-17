---
title: Azure Analiz Hizmetleri'nde veritabanı rollerini ve kullanıcılarını yönetme | Microsoft Dokümanlar
description: Azure'daki bir Analiz Hizmetleri sunucusunda veritabanı rollerini ve kullanıcılarını nasıl yöneteceklerini öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b7e3cc2b9d35eafcb875efa167821a8e9ad80146
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81454212"
---
# <a name="manage-database-roles-and-users"></a>Veritabanı rollerini ve kullanıcılarını yönetme

Model veritabanı düzeyinde, tüm kullanıcıların bir role ait olması gerekir. Roller, model veritabanı için belirli izinlere sahip kullanıcıları tanımlar. Bir role eklenen herhangi bir kullanıcı veya güvenlik grubunun, sunucuyla aynı abonelikte bir Azure AD kiracısında bir hesabı olmalıdır. 

Rolleri nasıl tanımladığınız kullandığınız araca bağlı olarak farklıdır, ancak efekt aynıdır.

Rol izinleri şunlardır:
*  **Yönetici** - Kullanıcılar veritabanı için tam izinlere sahiptir. Yönetici izinleri olan veritabanı rolleri sunucu yöneticilerinden farklıdır.
*  **İşlem** - Kullanıcılar veritabanına bağlanabilir ve işlem işlemleri gerçekleştirebilir ve model veritabanı verilerini analiz edebilir.
*  **Oku** - Kullanıcılar model veritabanı verilerine bağlanmak ve çözümlemek için bir istemci uygulamasını kullanabilir.

Bir tabular model projesi oluştururken, Analiz Hizmetleri projeleri ile Visual Studio Role Manager kullanarak rolleri oluşturur ve bu rollere kullanıcı veya gruplar eklersiniz. Bir sunucuya dağıtıldığında, rolleri ve kullanıcı üyelerini eklemek veya kaldırmak için SQL Server Management Studio (SSMS), [Analysis Services PowerShell cmdlets](https://docs.microsoft.com/analysis-services/powershell/analysis-services-powershell-reference)veya [Tabular Model Scripting Language](https://docs.microsoft.com/analysis-services/tmsl/tabular-model-scripting-language-tmsl-reference) (TMSL) kullanın.

Bir **güvenlik grubu**eklerken, kullanın. `obj:groupid@tenantid`

## <a name="to-add-or-manage-roles-and-users-in-visual-studio"></a>Visual Studio'da rol ve kullanıcı eklemek veya yönetmek için  
  
1.  **Tabular Model**Explorer'da, **Roller'i**sağ tıklatın.  
  
2.  **Rol Yöneticisi**'nde **Yeni**'ye tıklayın.  
  
3.  Rol için bir ad yazın.  
  
     Varsayılan olarak, varsayılan rolün adı her yeni rol için artımlı olarak numaralandırılır. Finans Yöneticileri veya İnsan Kaynakları Uzmanları gibi üye türünü açıkça tanımlayan bir ad yazmanız önerilir.  
  
4.  Aşağıdaki izinlerden birini seçin:  
  
    |İzin|Açıklama|  
    |----------------|-----------------|  
    |**Yok**|Üyeler model şeasını okuyamaz veya değiştiremez ve verileri sorgulayamaz.|  
    |**Okuma**|Üyeler verileri sorgulayabilir (satır filtrelerine göre) ancak model şeasını değiştiremez.|  
    |**Okuma ve İşlem**|Üyeler verileri sorgulayabilir (satır düzeyindeki filtrelere göre) ve Tüm işlemleri İşlem ve İşlem'i çalıştırabilir, ancak model şemasını değiştiremez.|  
    |**İşleme**|Üyeler Tüm Işlemleri İşlem ve İşlem'i çalıştırabilir. Model şeasını okuyamaz veya değiştiremez ve verileri sorgulayamaz.|  
    |**Yönetici**|Üyeler model şeasını değiştirebilir ve tüm verileri sorgulayabilir.|   
  
5.  Oluşturduğunuz rol okuma veya okuma ve işleme iznine sahipse, dax formülü kullanarak satır filtreleri ekleyebilirsiniz. Satır **Filtreleri** sekmesini tıklatın, sonra bir tablo seçin, sonra **DAX Filtre** alanını tıklatın ve sonra bir DAX formülü yazın.
  
6.  **Üye** > **Ekle Dış**'ı tıklatın.  
  
8.  **Dış Üye Ekle'de,** e-posta adresine göre kiracınız Azure AD'ınızdaki kullanıcıları veya grupları girin. Tamam'ı tıklatTıktan ve Rol Yöneticisi'ni kapattıktan sonra, Tabular Model Explorer'da roller ve rol üyeleri görünür. 
 
     ![Tabular Model Explorer'daki roller ve kullanıcılar](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. Azure Analiz Hizmetleri sunucunuza dağıtın.


## <a name="to-add-or-manage-roles-and-users-in-ssms"></a>SSMS'te rol ve kullanıcı eklemek veya yönetmek için

Dağıtılmış bir model veritabanına rol ve kullanıcı eklemek için sunucuya Sunucu yöneticisi olarak veya yönetici izinleriyle zaten bir veritabanı rolünde bağlı olmalısınız.

1. Nesne Exporer'da, **Roller** > **New Role'yi**sağ tıklatın.

2. **Rol Oluştur'da**bir rol adı ve açıklama girin.

3. İzin seçin.

   |İzin|Açıklama|  
   |----------------|-----------------|  
   |**Tam kontrol (Yönetici)**|Üyeler model şeasını değiştirebilir, işleyebilir ve tüm verileri sorgulayabilir.| 
   |**İşlem veritabanı**|Üyeler Tüm Işlemleri İşlem ve İşlem'i çalıştırabilir. Model şeasını değiştiremez ve verileri sorgulayamaz.|  
   |**Okuma**|Üyeler verileri sorgulayabilir (satır filtrelerine göre) ancak model şeasını değiştiremez.|  
  
4. **Üyelik'i**tıklatın, ardından e-posta adresine göre kiracınız Azure AD'ınıza bir kullanıcı veya grup girin.

     ![Kullanıcı ekle](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. Oluşturduğunuz rolokuma iznine sahipse, dax formülü kullanarak satır filtreleri ekleyebilirsiniz. **Satır Filtreleri'ni**tıklatın, bir tablo seçin ve ardından **DAX Filtresi** alanına bir DAX formülü yazın. 

## <a name="to-add-roles-and-users-by-using-a-tmsl-script"></a>TMSL komut dosyası kullanarak rol ve kullanıcı eklemek için

SSMS'teki XMLA penceresinde veya PowerShell kullanarak bir TMSL komut dosyası çalıştırabilirsiniz. [CreateOrReplace](https://docs.microsoft.com/analysis-services/tmsl/createorreplace-command-tmsl) komutunu ve [Roller](https://docs.microsoft.com/analysis-services/tmsl/roles-object-tmsl) nesnesini kullanın.

**Örnek TMSL komut dosyası**

Bu örnekte, SalesBI veritabanı için Okuma izinleri ile Analist rolüne bir B2B dış kullanıcısı ve bir grup eklenir. Hem dış kullanıcı hem de grup aynı kiracı Azure AD'de olmalıdır.

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@adventureworks.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="to-add-roles-and-users-by-using-powershell"></a>PowerShell'i kullanarak rol ve kullanıcı eklemek için

[SqlServer](https://docs.microsoft.com/analysis-services/powershell/analysis-services-powershell-reference) modülü, göreve özel veritabanı yönetimi cmdlets'i ve Tabular Model Komut Dosyası Dili (TMSL) sorgusu nu veya komut dosyasını kabul eden genel amaçlı Invoke-ASCmd cmdlet'i sağlar. Aşağıdaki cmdlets veritabanı rolleri ve kullanıcıları yönetmek için kullanılır.
  
|Cmdlet|Açıklama|
|------------|-----------------| 
|[Ekle-RolMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Veritabanı rolüne bir üye ekleyin.| 
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Bir üyeyi veritabanı rolünden kaldırın.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|Bir TMSL komut dosyası çalıştırın.|

## <a name="row-filters"></a>Satır filtreleri  

Satır filtreleri, bir tablodaki hangi satırların belirli bir rolün üyeleri tarafından sorgulanabileceğini tanımlar. Satır filtreleri DAX formülleri kullanılarak bir modeldeki her tablo için tanımlanır.  
  
Satır filtreleri yalnızca Okuma ve Okuma ve İşlem izinlerine sahip roller için tanımlanabilir. Varsayılan olarak, belirli bir tablo için bir satır filtresi tanımlanmamışsa, başka bir tablodan çapraz filtreleme uygulanmadığı sürece üyeler tablodaki tüm satırları sorgulayabilir.
  
 Satır filtreleri, belirli bir rolün üyeleri tarafından sorgulanabilecek satırları tanımlamak için DOĞRU/YANLıŞ değerine değer biçilmesi gereken bir DAX formülü gerektirir. DAX formülüne dahil olmayan satırlar sorgulanamaz. Örneğin, aşağıdaki satır filtreleri ifade, *=Müşteriler [Ülke] = "ABD"* ile Müşteriler tablosu, Satış rolü üyeleri yalnızca ABD'de müşterileri görebilirsiniz.  
  
Satır filtreleri belirtilen satırlara ve ilgili satırlara uygulanır. Bir tabloda birden çok ilişki olduğunda filtreler, etkin olan ilişki için güvenlik sağlar. Satır filtreleri, ilgili tablolar için tanımlanan diğer satır filianları ile kesişmiştir, örneğin:  
  
|Tablo|DAX ifadesi|  
|-----------|--------------------|  
|Bölge|=Bölge[Ülke]="ABD"|  
|ProductCategory|=ÜrünKategorisi[Adı]="Bisikletler"|  
|İşlemler|=İşlemler[Yıl]=2016|  
  
 Net etki, üyelerin müşterinin ABD'de olduğu, ürün kategorisinin bisiklet olduğu ve yılın 2016 olduğu veri satırlarını sorgulayabiliyor olmasıdır. Kullanıcılar, bu izinleri veren başka bir rolün üyesi olmadıkça ABD dışındaki hareketleri, bisiklet olmayan hareketleri veya 2016'da olmayan hareketleri sorgulayamaz.
  
 Tüm tablo için tüm satırlara erişimi reddetmek için filtreyi kullanabilirsiniz, *=FALSE()*

## <a name="next-steps"></a>Sonraki adımlar

  [Sunucu yöneticilerini yönetme](analysis-services-server-admins.md)   
  [Azure Analysis Services’ı PowerShell ile yönetme](analysis-services-powershell.md)  
  [Tabular Model Scripting Dili (TMSL) Referans](https://docs.microsoft.com/analysis-services/tmsl/tabular-model-scripting-language-tmsl-reference)

