---
title: Veri Bulma ve Sınıflandırma
description: Azure SQL Veritabanı ve Azure Synapse Analytics için Veri Bulma & Sınıflandırması
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
titleSuffix: Azure SQL Database and Azure Synapse
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 02/05/2020
tags: azure-synapse
ms.openlocfilehash: 599b2a280e386e49eb114f448f55b17ed7e823d7
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81616763"
---
# <a name="data-discovery--classification-for-azure-sql-database-and-azure-synapse-analytics"></a>Azure SQL Veritabanı ve Azure Synapse Analytics için Veri Bulma & Sınıflandırması

Veri Bulma & Sınıflandırması Azure SQL Veritabanı'nda yerleşiktir. Veritabanlarınızdaki hassas verileri keşfetmek, sınıflandırmak, etiketlemek ve raporlamak için gelişmiş özellikler sağlar.

En hassas verileriniz iş, mali, sağlık veya kişisel bilgileri içerebilir. Bu verileri keşfetmek ve sınıflandırmak, kuruluşunuzun bilgi koruma yaklaşımında önemli bir rol oynayabilir. Şunlara altyapı sağlayabilir:

- Veri gizliliği standartlarının ve mevzuata uygunluk gerekliliklerinin karşısına çıkmasına yardımcı olmak.
- Hassas verilere anormal erişim konusunda izleme (denetleme) ve uyarı gibi çeşitli güvenlik senaryoları.
- Son derece hassas veriler içeren veritabanlarının güvenliğini denetleme ve sertleştirme.

Veri Bulma & Sınıflandırması, gelişmiş SQL güvenlik yetenekleri için birleşik bir paket olan [Gelişmiş Veri Güvenliği](sql-database-advanced-data-security.md) teklifinin bir parçasıdır. Azure portalının merkezi **SQL Gelişmiş Veri Güvenliği** bölümünden Veri Bulma & Sınıflandırması'na erişebilir ve yönetebilirsiniz.

> [!NOTE]
> Bu makale, Azure SQL Veritabanı ve Azure Synapse Analytics ile ilgilidir. Basitlik için, sql veritabanı nı hem de Azure Synapse'yi ifade etmek için burada *SQL Veritabanı'nı* kullanırız. SQL Server (şirket içi) hakkında bilgi için [SQL Veri Bulma ve Sınıflandırma'ya](https://go.microsoft.com/fwlink/?linkid=866999)bakın.

## <a name="what-is-data-discovery--classification"></a><a id="what-is-dc"></a>Veri Bulma & Sınıflandırması nedir?

Veri Bulma & Sınıflandırması bir dizi gelişmiş hizmet ve yeni SQL Veritabanı özellikleri sunar. SQL Veritabanı için yalnızca veritabanını değil, verileri de korumayı amaçlayan yeni bir bilgi koruma paradigması oluşturur. Paradigma şunları içerir:

- **Keşif ve öneriler:** Sınıflandırma altyapısı veritabanınızı tarar ve hassas olabilecek veriler içeren sütunları tanımlar. Daha sonra Azure portalı üzerinden önerilen sınıflandırmayı gözden geçirmeniz ve uygulamanız için kolay bir yol sağlar.

- **Etiketleme:** SQL veritabanı altyapısına eklenen yeni meta veri özniteliklerini kullanarak sütunlara kalıcı olarak duyarlılık sınıflandırma etiketleri uygulayabilirsiniz. Bu meta veriler daha sonra gelişmiş, duyarlılığa dayalı denetim ve koruma senaryoları için kullanılabilir.

- **Sorgu sonucu ayarlı duyarlılık:** Sorgu sonuç kümesinin duyarlılığı, denetim amacıyla gerçek zamanlı olarak hesaplanır.

- **Görünürlük:** Veritabanı sınıflandırma durumunu Azure portalındaki ayrıntılı bir panoda görüntüleyebilirsiniz. Ayrıca, uyumluluk ve denetim amaçları ve diğer ihtiyaçlar için kullanmak üzere Excel biçiminde bir rapor indirebilirsiniz.

## <a name="discover-classify-and-label-sensitive-columns"></a><a id="discover-classify-columns"></a>Hassas sütunları keşfedin, sınıflandırın ve etiketle

Bu bölümde aşağıdaki adımları açıklanır:

- Veritabanınızda hassas veriler içeren sütunları bulma, sınıflandırma ve etiketleme.
- Veritabanınızın geçerli sınıflandırma durumunu görüntüleme ve raporları dışa aktarma.

Sınıflandırma iki meta veri öznitelikleri içerir:

- **Etiketler**: Sütunda depolanan verilerin duyarlılık düzeyini tanımlamak için kullanılan ana sınıflandırma öznitelikleri.  
- **Bilgi türleri**: Sütunda depolanan veri türü hakkında daha ayrıntılı bilgi sağlayan öznitelikler.

### <a name="define-and-customize-your-classification-taxonomy"></a>Sınıflandırma taksonominizi tanımlayın ve özelleştirin

Veri Bulma & Sınıflandırması, yerleşik bir duyarlılık etiketleri kümesi ve yerleşik bir bilgi türleri kümesi ve bulma mantığıyla birlikte gelir. Artık bu taksonomiyi özelleştirebilir ve özel olarak kendi ortamınız için sınıflandırma yapıları derecelendirme kümesi oluşturabilirsiniz.

Sınıflandırma taksonomisini tüm Azure kuruluşunuz için tek bir merkezi yerde tanımlar ve özelleştirin. Bu konum, güvenlik politikanızın bir parçası olarak Azure Güvenlik Merkezi'ndedir. [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) Bu görevi yalnızca kuruluşun kök yönetim grubunda yönetim hakları olan biri yapabilir.

SQL bilgi koruması için ilke yönetiminin bir parçası olarak, özel etiketler tanımlayabilir, sıralayabilir ve bunları seçili bilgi türleri kümesiyle ilişkilendirebilirsiniz. Ayrıca kendi özel bilgi türleri ekleyebilirsiniz ve dize desenleri ile bunları yapılandırmak. Desenler, veritabanlarınızdaki bu veri türünü tanımlamak için bulma mantığına eklenir.

[SQL Bilgi Koruma ilkesi nasıl yapılır kılavuzunda](https://go.microsoft.com/fwlink/?linkid=2009845&clcid=0x409)ilkenizi özelleştirme ve yönetme hakkında daha fazla bilgi edinin.

Kuruluş genelinde ilke tanımlandıktan sonra, özelleştirilmiş ilkenizi kullanarak tek tek veritabanlarını sınıflandırmaya devam edebilirsiniz.

### <a name="classify-your-sql-database"></a>SQL veritabanınızı sınıflandırma

1. [Azure portalına](https://portal.azure.com)gidin.

2. Azure SQL Veritabanı bölmenizde **Güvenlik** başlığı altında **Gelişmiş Veri Güvenliği'ne** gidin. **Gelişmiş veri güvenliği'ni**seçin ve ardından Veri Bulma **& Sınıflandırma** kartını seçin.

   ![Azure portalında Gelişmiş Veri Güvenliği bölmesi](./media/sql-data-discovery-and-classification/data_classification.png)

3. Veri **bulma & sınıflandırma** sayfasında, **Genel Bakış** sekmesi veritabanının geçerli sınıflandırma durumunun bir özetini içerir. Özet, yalnızca belirli şema parçalarını, bilgi türlerini ve etiketleri göstermek için filtre uygulayabileceğiniz tüm gizli sütunların ayrıntılı bir listesini içerir. Henüz herhangi bir sütun sınıflandırdıysanız, [adım 5'e atlayın.](#step-5)

   ![Geçerli sınıflandırma durumunun özeti](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Excel biçiminde bir rapor indirmek için bölmenin üst menüsünde **Dışa** Aktar'ı seçin.

5. <a id="step-5"></a>Verilerinizi sınıflandırmaya başlamak **için, Veri bulma & sınıflandırma** sayfasındaki **Sınıflandırma** sekmesini seçin.

    Sınıflandırma altyapısı veritabanınızı hassas olabilecek verileri içeren sütunlar için tarar ve önerilen sütun sınıflandırmalarının bir listesini sağlar.

6. Sınıflandırma önerilerini görüntüleyin ve uygulayın:

   - Önerilen sütun sınıflandırmaları listesini görüntülemek için bölmenin altındaki öneriler panelini seçin.

   - Belirli bir sütun için bir öneri kabul etmek için, ilgili satırın sol sütunundaki onay kutusunu seçin. Tüm önerileri kabul edilmiş olarak işaretlemek için, öneriler tablosu üstbilgisinde en soldaki onay kutusunu seçin.

       ![Sınıflandırma önerileri listesinden gözden geçirme ve seçme](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

   - Seçili önerileri uygulamak için **seçili önerileri kabul et'i**seçin.

7. Sütunları el ile, alternatif olarak veya öneri tabanlı sınıflandırmaya ek olarak da sınıflandırabilirsiniz:

   1. Bölmenin üst menüsünde **sınıflandırma ekle'yi** seçin.

   1. Açılan bağlam penceresinde, sınıflandırmak istediğiniz şema, tablo ve sütunile bilgi türü ve duyarlılık etiketini seçin.

   1. Bağlam penceresinin altındaki **sınıflandırmayı ekle'yi** seçin.

      ![Sınıflandırmak için bir sütun seçin](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Sınıflandırmanızı tamamlamak ve veritabanı sütunlarını yeni sınıflandırma meta verileriyle sürekli etiketlemek (etiketlemek) için pencerenin üst menüsünde **Kaydet'i** seçin.

## <a name="audit-access-to-sensitive-data"></a><a id="audit-sensitive-data"></a>Hassas verilere erişimi denetleme

Bilgi koruma paradigmasının önemli bir yönü, hassas verilere erişimi izleyebilme yeteneğidir. [Azure SQL Veritabanı Denetimi,](sql-database-auditing.md) denetim günlüğüne yeni bir `data_sensitivity_information`alan içerecek şekilde geliştirildi. Bu alan, sorgu tarafından döndürülen verilerin duyarlılık sınıflandırmalarını (etiketleri) kaydeder. Bir örneği aşağıda verilmiştir:

![Denetim günlüğü](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a name="permissions"></a><a id="permissions"></a>İzinler

Bu yerleşik roller, Azure SQL veritabanının veri sınıflandırmasını okuyabilir:

- Sahip
- Okuyucu
- Katılımcı
- SQL Güvenlik Yöneticisi
- Kullanıcı Erişimi Yöneticisi

Bu yerleşik roller, Azure SQL veritabanının veri sınıflandırmasını değiştirebilir:

- Sahip
- Katılımcı
- SQL Güvenlik Yöneticisi

[Azure kaynakları için RBAC'daki](https://docs.microsoft.com/azure/role-based-access-control/overview)rol tabanlı izinler hakkında daha fazla bilgi edinin.

## <a name="manage-classifications"></a><a id="manage-classification"></a>Sınıflandırmaları yönetme

Sınıflandırmaları yönetmek için T-SQL, REST API veya PowerShell'i kullanabilirsiniz.

### <a name="use-t-sql"></a>T-SQL'i kullanma

Sütun sınıflandırmaları eklemek veya kaldırmak ve tüm veritabanı için tüm sınıflandırmaları almak için T-SQL'i kullanabilirsiniz.

> [!NOTE]
> Etiketleri yönetmek için T-SQL'i kullandığınızda, bir sütuna eklediğiniz etiketlerin kuruluşun bilgi koruma ilkesinde (portal önerilerinde görünen etiket kümesi) bulunduğuna dair bir doğrulama yoktur. Yani, bunu doğrulamak size kalmış.

Sınıflandırmalar için T-SQL kullanımı hakkında bilgi için aşağıdaki referanslara bakın:

- Bir veya daha fazla sütunun sınıflandırMasını eklemek veya güncellemek için: [HASSASİTE SINIFLANDIRILMASI EKLE](https://docs.microsoft.com/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
- Sınıflandırmayı bir veya daha fazla sütundan kaldırmak için: [DAMLA HASSASİyeSİ SINIFLANDIRILMASI](https://docs.microsoft.com/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
- Veritabanındaki tüm sınıflandırmaları görüntülemek için: [sys.sensitivity_classifications](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

### <a name="use-the-rest-api"></a>Geri Kalan API'yi kullanın

Sınıflandırmaları ve önerileri programlı bir şekilde yönetmek için REST API'sini kullanabilirsiniz. Yayınlanan REST API aşağıdaki işlemleri destekler:

- [Oluştur Veya Güncelleştir](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate): Belirtilen sütunun duyarlılık etiketini oluşturur veya güncelleştirir.
- [Delete](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete): Belirtilen sütunun duyarlılık etiketini siler.
- [Öneriyi Devre Dışı:](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/disablerecommendation)Belirtilen sütundaki duyarlılık önerilerini devre dışı kılabilir.
- [Öneriyi Etkinleştir](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/enablerecommendation): Belirtilen sütunda duyarlılık önerilerini sağlar. (Öneriler varsayılan olarak tüm sütunlarda etkinleştirilir.)
- [Get](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get): Belirtilen sütunun duyarlılık etiketini alır.
- [Liste Geçerli Veritabanı](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listcurrentbydatabase): Belirtilen veritabanının geçerli duyarlılık etiketlerini alır.
- [Veritabanı Tarafından Önerilen Liste](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listrecommendedbydatabase): Belirtilen veritabanının önerilen duyarlılık etiketlerini alır.

### <a name="use-powershell-cmdlets"></a>PowerShell cmdlet'leri kullanma
PowerShell'i Azure SQL Veritabanı ve yönetilen örnekler için sınıflandırmaları ve önerileri yönetmek için kullanabilirsiniz.

#### <a name="powershell-cmdlets-for-sql-database"></a>SQL Veritabanı için PowerShell cmdlets

- [Get-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityclassification)
- [Set-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesensitivityclassification)
- [Remove-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesensitivityclassification)
- [Get-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityrecommendation)
- [Enable-AzSqlDatabaSesensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqldatabasesensitivityrecommendation)
- [Devre Dışı-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqldatabasesensitivityrecommendation)

#### <a name="powershell-cmdlets-for-managed-instances"></a>Yönetilen örnekler için PowerShell cmdlets

- [Get-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityclassification)
- [Set-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasesensitivityclassification)
- [Remove-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabasesensitivityclassification)
- [Get-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityrecommendation)
- [Enable-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqlinstancedatabasesensitivityrecommendation)
- [Devre Dışı-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqlinstancedatabasesensitivityrecommendation)


## <a name="next-steps"></a><a id="next-steps"></a>Sonraki adımlar

- [Gelişmiş Veri Güvenliği](sql-database-advanced-data-security.md)hakkında daha fazla bilgi edinin.
- Sınıflandırılmış hassas verilerinize erişimi izlemek ve denetlemek için [Azure SQL Veritabanı Denetimi'ni](sql-database-auditing.md) yapılandırmayı düşünün.
- Veri bulma ve sınıflandırma içeren bir sunu için bkz [&. Veri Maruz](https://www.youtube.com/watch?v=itVi9bkJUNc).
