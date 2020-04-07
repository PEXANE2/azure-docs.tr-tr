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
ms.openlocfilehash: b3a08eb351d29fd71889807c9a21d03b564117a7
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673757"
---
# <a name="data-discovery--classification-for-azure-sql-database-and-azure-synapse-analytics"></a>Azure SQL Veritabanı ve Azure Synapse Analytics için Veri Bulma & Sınıflandırması

Veri Bulma & Sınıflandırması, veritabanlarınızdaki hassas verileri **raporlamayı keşfetmek,** **sınıflandırmak,** **etiketlemek** & **reporting** için Azure SQL Veritabanı'nda yerleşik gelişmiş özellikler sağlar.

En hassas verilerinizi (iş, finans, sağlık, kişisel olarak tanımlanabilir veriler vb.) keşfetmek ve sınıflandırmak, kuruluş bilgi koruma boyunuzda önemli bir rol oynayabilir. Şunlara altyapı sağlayabilir:

- Veri gizliliği standartlarına uymaya ve mevzuat uyumluluğu gereksinimlerini karşılamaya yardımcı olma.
- Hassas verilere anormal erişim konusunda izleme (denetleme) ve uyarı gibi çeşitli güvenlik senaryoları.
- Son derece hassas veriler içeren veritabanlarının erişimini denetleme ve güvenliğini sağlamlaştırma.

Veri Bulma & Sınıflandırması, gelişmiş SQL güvenlik yetenekleri için birleşik bir paket olan [Gelişmiş Veri Güvenliği](sql-database-advanced-data-security.md) (ADS) teklifinin bir parçasıdır. veri bulma & sınıflandırması merkezi SQL ADS portalı üzerinden erişilebilir ve yönetilebilir.

> [!NOTE]
> Bu belge, Azure SQL Veritabanı ve Azure Synapse ile ilgilidir. Basitlik için, SQL Veritabanı hem SQL Veritabanı hem de Azure Synapse'ye atıfta bulunularak kullanılır. SQL Server için (şirket içinde), [bkz.](https://go.microsoft.com/fwlink/?linkid=866999)

## <a name="what-is-data-discovery--classification"></a><a id="what-is-dc"></a>Veri bulma & sınıflandırma nedir

Veri Bulma & Sınıflandırma, yalnızca veritabanını değil, verileri korumayı amaçlayan yeni bir SQL Bilgi Koruma paradigması oluşturarak bir dizi gelişmiş hizmet ve yeni SQL yetenekleri sunar:

- **Keşif & önerileri**

  Sınıflandırma altyapısı veritabanınızı tarar ve hassas olabilecek verileri içeren sütunları tanımlar. Ardından Azure portalı üzerinden uygun sınıflandırma önerilerini gözden geçirmek ve uygulamak için size kolay bir yol sağlar.

- **Etiketleme**

  Duyarlılık sınıflandırma etiketleri, SQL Engine'e getirilen yeni sınıflandırma meta veri öznitelikleri kullanılarak sütunlarda kalıcı olarak etiketlenebilir. Bu meta veriler daha sonra gelişmiş duyarlılık tabanlı denetim ve koruma senaryolarında kullanılabilir.

- **Sorgu sonucu ayarlama duyarlılığı**

  Sorgu sonuç kümesinin duyarlılığı denetim amaçları için gerçek zamanlı olarak hesaplanır.

- **Görünürlük**

  Veritabanı sınıflandırma durumu portaldaki ayrıntılı bir panoda görüntülenebilir. Buna ek olarak, hem uyumluluk ve denetim amacıyla hem de başka amaçlarla kullanmak üzere bir rapor (Excel biçiminde) indirebilirsiniz.

## <a name="discover-classify--label-sensitive-columns"></a><a id="discover-classify-columns"></a>Etikete duyarlı sütunları keşfedin, & sınıflandırma

Aşağıdaki bölümde, veritabanınızda hassas veriler içeren sütunları bulma, sınıflandırma ve etiketlemenin yanı sıra veritabanınızın geçerli sınıflandırma durumunu görüntüleme ve raporları dışa aktarma adımları açıklanmaktadır.

Sınıflandırma iki meta veri öznitelikleri içerir:

- **Etiketler** – Sütunda depolanan verilerin duyarlılık düzeyini tanımlamak için kullanılan ana sınıflandırma öznitelikleri.  
- **Bilgi Türleri** – Sütunda depolanan veri türüne ek parçalılık sağlayın.

## <a name="define-and-customize-your-classification-taxonomy"></a>Sınıflandırma taksonominizi tanımlayın ve özelleştirin

Veri Bulma & Sınıflandırması, yerleşik bir duyarlılık etiketleri kümesi ve yerleşik bir bilgi türleri kümesi ve bulma mantığıyla birlikte gelir. Artık bu taksonomi özelleştirme ve çevreniz için özel olarak sınıflandırma yapılarının bir kümesini ve sıralamasını tanımlayabilirsiniz.

Sınıflandırma taksonominizin tanımı ve özelleştirilmesi, tüm Azure kiracınız için tek bir merkezi yerde yapılır. Bu konum, Güvenlik İlkenizin bir parçası olarak Azure Güvenlik Merkezi'ndedir. [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) Bu görevi yalnızca Kiracı kök yönetim grubunda yönetim hakları olan biri gerçekleştirebilir.

SQL Bilgi Koruma ilkesi yönetiminin bir parçası olarak, özel etiketler tanımlayabilir, sıralayabilir ve bunları seçili bilgi türleri kümesiyle ilişkilendirebilirsiniz. Ayrıca kendi özel bilgi türlerinizi ekleyebilir ve bunları dize desenleriyle yapılandırabilirsiniz. Bunlar veritabanlarınızda bu türdeki verileri belirlemek üzere bulma mantığına eklenir.
[SQL Bilgi Koruma ilkesi nasıl yapılır kılavuzunda](https://go.microsoft.com/fwlink/?linkid=2009845&clcid=0x409)ilkenizi özelleştirme ve yönetme hakkında daha fazla bilgi edinin.

Kiracı genelindeki ilke tanımlandıktan sonra, özelleştirilmiş ilkenizi kullanarak tek tek veritabanlarının sınıflandırılmasına devam edebilirsiniz.

## <a name="classify-your-sql-database"></a>SQL Veritabanınızı Sınıflandırma

1. [Azure portalına](https://portal.azure.com)gidin.

2. Azure SQL Veritabanı bölmenizde Güvenlik başlığı altında **Gelişmiş Veri Güvenliği'ne** gidin. Gelişmiş veri güvenliğini etkinleştirmek için tıklatın ve ardından **Veri bulma & sınıflandırma** kartına tıklayın.

   ![Veritabanını tarayın](./media/sql-data-discovery-and-classification/data_classification.png)

3. **Genel Bakış** sekmesi, yalnızca belirli şema parçalarını, bilgi türlerini ve etiketleri görüntülemek için filtre uygulayabileceğiniz tüm gizli sütunların ayrıntılı bir listesini de içeren veritabanının geçerli sınıflandırma durumunun bir özetini içerir. Henüz sütun ları sınıflandırmadıysanız, [adım 5'e atlayın.](#step-5)

   ![Geçerli sınıflandırma durumunun özeti](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Excel biçiminde bir rapor indirmek için pencerenin üst menüsündeki **Dışa** Aktarma seçeneğini tıklatın.

5. <a id="step-5"></a>Verilerinizi sınıflandırmaya başlamak için pencerenin üst kısmındaki **Sınıflandırma sekmesine** tıklayın.

6. Sınıflandırma altyapısı veritabanınızı hassas olabilecek verileri içeren sütunlar için tarar ve **önerilen sütun sınıflandırmalarının**bir listesini sağlar. Sınıflandırma önerilerini görüntülemek ve uygulamak için:

   - Önerilen sütun sınıflandırmalarının listesini görüntülemek için pencerenin altındaki öneriler paneline tıklayın

   - Öneriler listesini gözden geçirin – belirli bir sütun için bir öneri kabul etmek için, ilgili satırın sol sütunundaki onay kutusunu işaretleyin. Ayrıca, öneriler tablosu üstbilgisinde onay kutusunu işaretleyerek *tüm önerileri* kabul edilmiş olarak işaretleyebilirsiniz.

       ![Öneri listesini gözden geçirin](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

   - Seçili önerileri uygulamak için mavi **Seçili Önerileri Kabul** et düğmesini tıklatın.

7. Ayrıca, sütunları öneri tabanlı sınıflandırmaya alternatif olarak veya ek olarak **el ile sınıflandırabilirsiniz:**

   - Pencerenin üst menüsündeki **Ekle sınıflandırmasını** tıklatın.

   - Açılan bağlam penceresinde, sınıflandırmak istediğiniz şema > tablo > sütunu ve bilgi türü ve duyarlılık etiketini seçin. Ardından bağlam penceresinin altındaki mavi **Ekle sınıflandırma** düğmesine tıklayın.

      ![Sınıflandırmak için sütunu seçin](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Sınıflandırmanızı tamamlamak ve veritabanı sütunlarını yeni sınıflandırma meta verileriyle sürekli etiketlemek (etiketlemek) için pencerenin üst menüsünde **Kaydet'i** tıklatın.

## <a name="auditing-access-to-sensitive-data"></a><a id="audit-sensitive-data"></a>Hassas verilere erişimi denetleme

Bilgi koruma paradigmasının önemli bir yönü, hassas verilere erişimi izleyebilme yeteneğidir. [Azure SQL Veritabanı Denetimi,](sql-database-auditing.md) sorgu tarafından döndürülen gerçek verilerin duyarlılık sınıflandırmalarını (etiketleri) günlüğe kaydeden *data_sensitivity_information*adlı denetim günlüğüne yeni bir alan içerecek şekilde geliştirilmiştir.

![Denetim günlüğü](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a name="permissions"></a><a id="permissions"></a>İzinler

Aşağıdaki yerleşik roller, Bir Azure SQL veritabanının veri `Owner` `Reader`sınıflandırmasını `SQL Security Manager` okuyabilir: , , `User Access Administrator` `Contributor`ve .

Aşağıdaki yerleşik roller, Bir Azure SQL veritabanının veri `Owner` `Contributor`sınıflandırmasını değiştirebilir: , . `SQL Security Manager`

[Azure kaynakları için RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview) hakkında daha fazla bilgi edinin

## <a name="manage-classifications"></a><a id="manage-classification"></a>Sınıflandırmaları yönetme

### <a name="using-t-sql"></a>T-SQL kullanma
Sütun sınıflandırmalarını eklemek/kaldırmak ve tüm veritabanı için tüm sınıflandırmaları almak için T-SQL'i kullanabilirsiniz.

> [!NOTE]
> Etiketleri yönetmek için T-SQL kullanılırken, kuruluş bilgi koruma ilkesinde (portal önerilerinde görünen etiket kümesi) bir sütuna eklenen etiketlerin bulunduğuna dair bir doğrulama yoktur. Bu nedenle bunu doğrulamak size kalmış.

- Bir veya daha fazla sütunun sınıflandırını ekleme/güncelleme: [HASSASİTE SINIFLANDIRILMASI EKLE](https://docs.microsoft.com/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
- Sınıflandırmayı bir veya daha fazla sütundan kaldırın: [DAMLA HASSASİyeSİ SINIFLANDIRILMASI](https://docs.microsoft.com/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
- Veritabanındaki tüm sınıflandırmaları görüntüleyin: [sys.sensitivity_classifications](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

### <a name="using-rest-api"></a>Dinlenme API'sini kullanma
Sınıflandırmaları ve önerileri programlı bir şekilde yönetmek için REST API'yi kullanabilirsiniz. Yayınlanan REST API aşağıdaki işlemleri destekler:

- [Oluştur veya Güncelle](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate) - Belirli bir sütunun duyarlılık etiketini oluşturur veya güncelleştirir
- [Sil](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete) - Belirli bir sütunun duyarlılık etiketini siler
- [Öneriyi Devre Dışı -](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/disablerecommendation) Belirli bir sütundaki duyarlılık önerilerini devre dışı
- [Öneriyi Etkinleştir](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/enablerecommendation) - Belirli bir sütunda duyarlılık önerilerini etkinleştirme (tüm sütunlarda varsayılan olarak öneriler etkinleştirilir)
- [Get](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get) - Belirli bir sütunun duyarlılık etiketini alır
- [Veritabanına Göre Geçerli liste](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listcurrentbydatabase) - Belirli bir veritabanının geçerli duyarlılık etiketlerini alır
- [Veritabanı Tarafından Önerilen Liste](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listrecommendedbydatabase) - Belirli bir veritabanının önerilen duyarlılık etiketlerini alır

### <a name="using-powershell-cmdlet"></a>PowerShell Cmdlet kullanma
Azure SQL Veritabanı ve Yönetilen Örnek için sınıflandırmaları ve önerileri yönetmek için PowerShell'i kullanabilirsiniz.

#### <a name="powershell-cmdlet-for-azure-sql-database"></a>Azure SQL Veritabanı için PowerShell Cmdlet
- [Get-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityclassification)
- [Set-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesensitivityclassification)
- [Remove-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesensitivityclassification)
- [Get-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityrecommendation)
- [Enable-AzSqlDatabaSesensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqldatabasesensitivityrecommendation)
- [Devre Dışı-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqldatabasesensitivityrecommendation)

#### <a name="powershell-cmdlets-for-managed-instance"></a>Yönetilen Örnek için PowerShell Cmdlets
- [Get-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityclassification)
- [Set-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasesensitivityclassification)
- [Remove-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabasesensitivityclassification)
- [Get-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityrecommendation)
- [Enable-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqlinstancedatabasesensitivityrecommendation)
- [Devre Dışı-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqlinstancedatabasesensitivityrecommendation)


## <a name="next-steps"></a><a id="next-steps"></a>Sonraki adımlar

- [Gelişmiş Veri Güvenliği](sql-database-advanced-data-security.md)hakkında daha fazla bilgi edinin.
- Sınıflandırılmış hassas verilerinize erişimi izlemek ve denetlemek için [Azure SQL Veritabanı Denetimi'ni](sql-database-auditing.md) yapılandırmayı düşünün.
- Veri Bulma & Sınıflandırması içeren bir sunu için bkz [&. Veri Maruz](https://www.youtube.com/watch?v=itVi9bkJUNc).
