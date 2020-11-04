---
title: Azure Güvenlik Merkezi 'nde SQL Information Protection ilkesi
description: Azure Güvenlik Merkezi 'nde bilgi koruma ilkelerini özelleştirmeyi öğrenin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2ebf2bc7-232a-45c4-a06a-b3d32aaf2500
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2020
ms.author: memildin
ms.openlocfilehash: 0a487f778693e87e680033edd0d80c55d1a85f66
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93348636"
---
# <a name="sql-information-protection-policy-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde SQL Information Protection ilkesi
 
SQL Information Protection 'ın [veri bulma ve sınıflandırma mekanizması](../azure-sql/database/data-discovery-and-classification-overview.md) , veritabanlarınızdaki hassas verileri bulmak, sınıflandırmak, etiketlemek ve raporlamak için gelişmiş yetenekler sağlar. [Azure SQL veritabanı](../azure-sql/database/sql-database-paas-overview.md), [Azure SQL yönetilen örneği](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)ve [Azure SYNAPSE Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)'te yerleşiktir.

Sınıflandırma mekanizması aşağıdaki iki öğeyi temel alır:

- **Etiketler** : sütunda depolanan *verilerin duyarlılık düzeyini* tanımlamak için kullanılan ana sınıflandırma öznitelikleri. 
- **Bilgi türleri** : sütunda depolanan *verilerin türüne* ek ayrıntı düzeyi sağlar.

Güvenlik Merkezi 'ndeki Information Protection ilkesi seçenekleri, Sınıflandırma Altyapısı için varsayılanlar olarak işlev gösteren önceden tanımlanmış bir Etiketler ve bilgi türleri kümesi sağlar. İlkeyi, kuruluşunuzun ihtiyaçlarına göre, aşağıda açıklandığı gibi özelleştirebilirsiniz.

> [!IMPORTANT]
> Azure kiracınız için Information Protection ilkesini özelleştirmek üzere kiracının kök yönetim grubunda yönetici ayrıcalıklarına sahip olmanız gerekir. [Azure Güvenlik Merkezi için kiracı genelinde görünürlük elde](security-center-management-groups.md)edin hakkında daha fazla bilgi edinin.

:::image type="content" source="./media/security-center-info-protection-policy/sql-information-protection-policy-page.png" alt-text="SQL Information Protection ilkenizi gösteren sayfa":::
 



## <a name="how-do-i-access-the-sql-information-protection-policy"></a>Nasıl yaparım? SQL Information Protection ilkesine erişin mi?

Bilgi koruma ilkesine erişmenin üç yolu vardır:

- **(Önerilen)** Güvenlik Merkezi 'nin fiyatlandırma ve Ayarlar sayfasından
- Güvenlik Önerisi "SQL veritabanlarınızdaki hassas veriler" sınıflandırılmalıdır "
- Azure SQL DB veri bulma sayfasından

Bunların her biri aşağıdaki ilgili sekmede gösterilmiştir.



### <a name="from-security-centers-settings"></a>[**Güvenlik Merkezi 'nin ayarlarından**](#tab/sqlip-tenant)

### <a name="access-the-policy-from-security-centers-pricing-and-settings-page"></a>İlkeye Güvenlik Merkezi 'nin fiyatlandırma ve Ayarlar sayfasından erişin <a name="sqlip-tenant"></a>

Güvenlik Merkezi 'nin **fiyatlandırma ve ayarlar** sayfasından **SQL Information Protection** ' ı seçin.

> [!NOTE]
> Bu seçenek yalnızca kiracı düzeyinde izinlere sahip kullanıcılar için görüntülenir. 

:::image type="content" source="./media/security-center-info-protection-policy/pricing-settings-link-to-information-protection.png" alt-text="Azure Güvenlik Merkezi 'nin fiyatlandırma ve Ayarlar sayfasından SQL Information Protection ilkesine erişme":::



### <a name="from-security-centers-recommendation"></a>[**Güvenlik Merkezi 'nin önerisine**](#tab/sqlip-db)

### <a name="access-the-policy-from-the-security-center-recommendation"></a>İlkeye Güvenlik Merkezi öneriden erişin <a name="sqlip-db"></a>

Veritabanınızın veri bulma ve sınıflandırma sayfasını görüntülemek için Güvenlik Merkezi 'nin önerilerini kullanın, "SQL veritabanlarınızdaki hassas veriler sınıflandırılmalıdır". Burada, sınıflandırmanızı önerdiğimiz bilgileri içeren sütunları da görürsünüz.

1. Güvenlik Merkezi 'nin **öneriler** sayfasından **SQL veritabanlarınızdaki önerinin hassas verilerinin aranması gerekir**.

    :::image type="content" source="./media/security-center-info-protection-policy/sql-sensitive-data-recommendation.png" alt-text="SQL Information Protection ilkelerine erişim sağlayan öneriyi bulma":::

1. Öneri ayrıntıları sayfasında, **sağlıklı** veya **sağlıksız** sekmelerinden bir veritabanı seçin.

1. **Veri bulma & sınıflandırma** sayfası açılır. **Yapılandır** 'ı seçin.

    :::image type="content" source="./media/security-center-info-protection-policy/access-policy-from-security-center-recommendation.png" alt-text="Azure Güvenlik Merkezi 'nde ilgili öneriden SQL Information Protection ilkesini açma":::



### <a name="from-azure-sql"></a>[**Azure SQL 'den**](#tab/sqlip-azuresql)

### <a name="access-the-policy-from-azure-sql"></a>İlkeye Azure SQL 'den erişin <a name="sqlip-azuresql"></a>

1. Azure portal Azure SQL ' i açın.

    :::image type="content" source="./media/security-center-info-protection-policy/open-azure-sql.png" alt-text="Azure portal Azure SQL 'i açma":::

1. Herhangi bir veritabanını seçin.

1. Menünün **güvenlik** alanından **veri bulma & sınıflandırma** sayfasını (1) açın ve **Yapılandır** ' ı (2) seçin.

    :::image type="content" source="./media/security-center-info-protection-policy/access-policy-from-azure-sql.png" alt-text="Azure SQL 'de SQL Information Protection ilkesini açma":::

--- 


## <a name="customize-your-information-types"></a>Bilgi türlerinizi özelleştirin

Bilgi türlerini yönetmek ve özelleştirmek için:

1. **Bilgi türlerini Yönet** ' i seçin.

    :::image type="content" source="./media/security-center-info-protection-policy/manage-types.png" alt-text="Bilgi koruma ilkenize yönelik bilgi türlerini yönetme":::

1. Yeni bir tür eklemek için **bilgi türü oluştur** ' u seçin. Bilgi türü için bir ad, açıklama ve arama deseninin dizelerini yapılandırabilirsiniz. Arama desenli dizeler, isteğe bağlı olarak joker karakterlerle ('% ' karakterini kullanarak) anahtar sözcükleri kullanabilir ve bu da otomatik bulma altyapısının, veritabanınızdaki gizli verileri, sütunların meta verilerine göre belirlemek için kullanır.
 
    :::image type="content" source="./media/security-center-info-protection-policy/configure-new-type.png" alt-text="Bilgi koruma ilkeniz için yeni bir bilgi türü yapılandırma":::

1. Ayrıca, ek arama model dizeleri ekleyerek, var olan dizelerin bazılarını devre dışı bırakarak veya açıklamayı değiştirerek yerleşik türleri değiştirebilirsiniz. 

    > [!TIP]
    > Yerleşik türleri silemez veya adlarını değiştiremezsiniz. 

1. **Bilgi türleri** , artan bulma derecelendirmesi sırasına göre listelenmiştir, yani listede daha yüksek olan türlerin birinciden eşleştirmeye çalışır. Bilgi türleri arasındaki derecelendirmeyi değiştirmek için, türleri tablodaki doğru noktaya sürükleyin veya sıralamayı değiştirmek için **Yukarı taşı** ve **aşağı taşı** düğmelerini kullanın. 

1. İşiniz bittiğinde **Tamam ' ı** seçin.

1. Bilgi türlerinizi yönetmeyi tamamladıktan sonra, belirli bir etiket için **Yapılandır** ' a tıklayarak ve bilgi türlerini uygun şekilde ekleyerek veya silerek ilgili türleri ilgili etiketlerle ilişkilendirdiğinizden emin olun.

1. Değişikliklerinizi uygulamak için ana **Etiketler** sayfasında **Kaydet** ' i seçin.
 

## <a name="exporting-and-importing-a-policy"></a>İlke dışarı ve içeri aktarma

Tanımlı Etiketler ve bilgi türlerinizle bir JSON dosyası indirebilir, dosyayı seçtiğiniz düzenleyicide düzenleyebilir ve ardından güncelleştirilmiş dosyayı içeri aktarabilirsiniz. 

:::image type="content" source="./media/security-center-info-protection-policy/export-import.png" alt-text="Bilgi koruma ilkenizi dışarı ve içeri aktarma":::

> [!NOTE]
> Bir ilke dosyasını içeri aktarmak için kiracı düzeyi izinlerine sahip olmanız gerekir. 


## <a name="manage-sql-information-protection-using-azure-powershell"></a>Azure PowerShell kullanarak SQL Information Protection 'ı yönetme

- [Get-AzSqlInformationProtectionPolicy](/powershell/module/az.security/get-azsqlinformationprotectionpolicy): ETKIN kiracı SQL Information Protection ilkesini alır.
- [Set-AzSqlInformationProtectionPolicy](/powershell/module/az.security/set-azsqlinformationprotectionpolicy): ETKIN kiracı SQL Information Protection ilkesini ayarlar.
 

## <a name="next-steps"></a>Sonraki adımlar
 
Bu makalede, Azure Güvenlik Merkezi 'nde bir bilgi koruma ilkesi tanımlamayı öğrendiniz. SQL veritabanlarındaki hassas verileri sınıflandırmak ve korumak için SQL Information Protection kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure SQL veritabanı veri bulma ve sınıflandırma](../azure-sql/database/data-discovery-and-classification-overview.md).

Güvenlik Merkezi 'nde güvenlik ilkeleri ve veri güvenliği hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
 
- [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md): Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md): Güvenlik Merkezi 'nin verileri nasıl yönettiğini ve korumalarını öğrenin
