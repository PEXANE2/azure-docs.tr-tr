---
title: Veri Bulma ve Sınıflandırma
description: Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure SYNAPSE Analytics için veri bulma & sınıflandırması
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sqldbrb=1
titleSuffix: Azure SQL Database, Azure SQL Managed Instance, and Azure Synapse
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 04/28/2020
tags: azure-synapse
ms.openlocfilehash: 387ec3f792b5d61b6c909b4955a588146aa258f5
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84050684"
---
# <a name="data-discovery--classification"></a>Veri Bulma ve Sınıflandırma
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

Veri bulma & sınıflandırması Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure SYNAPSE Analytics 'te yerleşiktir. Veritabanlarınızdaki hassas verileri bulmak, sınıflandırmak, etiketlemek ve raporlamak için gelişmiş yetenekler sağlar.

En hassas verileriniz arasında işletme, finans, sağlık hizmetleri veya kişisel bilgiler bulunabilir. Bu verileri bulmak ve sınıflandırmak, kuruluşunuzun bilgi koruma yaklaşımında bir özetleme rolü oynayabilir. Şunlara altyapı sağlayabilir:

- Veri gizliliği ve yasal uyumluluk gereksinimleri için standartları karşılamanıza yardımcı olma.
- Hassas verilere yönelik anormal erişimlerde izleme (denetim) ve uyarı verme gibi çeşitli güvenlik senaryoları.
- Son derece hassas veriler içeren veritabanlarının güvenliğine erişimi denetleme ve güvenliği artırma.

Veri bulma & sınıflandırması, gelişmiş Azure SQL güvenlik özelliklerine yönelik Birleşik bir paket olan [Gelişmiş veri güvenliği](advanced-data-security.md) sunumunun bir parçasıdır. Azure portal Merkezi **SQL gelişmiş veri güvenliği** bölümü aracılığıyla veri bulma & sınıflandırmasına erişip yönetebilirsiniz.

> [!NOTE]
> SQL Server (Şirket içi) hakkında daha fazla bilgi için bkz. [SQL veri bulma ve sınıflandırma](https://go.microsoft.com/fwlink/?linkid=866999).

## <a name="what-is-data-discovery--classification"></a><a id="what-is-dc"></a>Veri bulma & sınıflandırması nedir?

Veri bulma & sınıflandırması, Azure 'da bir dizi gelişmiş hizmet ve yeni özelliği kullanıma sunuyor. Yalnızca veritabanını değil, verileri korumayı hedefleyen SQL veritabanı, SQL yönetilen örneği ve Azure SYNAPSE için yeni bir bilgi koruma paradigması oluşturur. Paradigma şunları içerir:

- **Bulma ve öneriler:** Sınıflandırma Altyapısı, veritabanınızı tarar ve potansiyel olarak hassas veriler içeren sütunları tanımlar. Daha sonra, Azure portal aracılığıyla önerilen sınıflandırmayı gözden geçirmek ve uygulamak için kolay bir yol sağlar.

- **Etiketleme:** SQL veritabanı altyapısına eklenen yeni meta veri özniteliklerini kullanarak, duyarlı sınıflandırma etiketlerini kalıcı olarak sütunlara uygulayabilirsiniz. Bu meta veriler daha sonra Gelişmiş, duyarlılık tabanlı denetim ve koruma senaryoları için kullanılabilir.

- **Sorgu sonucu-ayarlanan duyarlılık:** Sorgu sonuç kümesinin duyarlılığı, denetim amaçlarıyla gerçek zamanlı olarak hesaplanır.

- **Görünürlük:** Veritabanı sınıflandırması durumunu Azure portal ayrıntılı bir panoda görüntüleyebilirsiniz. Ayrıca, uyumluluk ve denetim amaçlarıyla ve diğer gereksinimlerde kullanmak üzere bir raporu Excel biçiminde indirebilirsiniz.

## <a name="discover-classify-and-label-sensitive-columns"></a><a id="discover-classify-columns"></a>Hassas sütunları bulma, sınıflandırma ve etiketleme

Bu bölümde, aşağıdaki adımlar açıklanmaktadır:

- Veritabanınızdaki hassas verileri içeren sütunları keşfetme, sınıflandırma ve etiketleme.
- Veritabanınızın geçerli sınıflandırma durumunu görüntüleme ve raporları dışarı aktarma.

Sınıflandırma iki meta veri özniteliği içerir:

- **Etiketler**: sütunda depolanan verilerin duyarlılık düzeyini tanımlamak için kullanılan ana sınıflandırma öznitelikleri.  
- **Bilgi türleri**: sütunda depolanan verilerin türü hakkında daha ayrıntılı bilgiler sağlayan öznitelikler.

### <a name="define-and-customize-your-classification-taxonomy"></a>Sınıflandırma taksonominizi tanımlama ve özelleştirme

Veri bulma & sınıflandırması, yerleşik bir duyarlılık etiketleri kümesi ve yerleşik bir bilgi türleri ve bulma mantığı kümesiyle gelir. Artık bu taksonomiyi özelleştirebilir ve özel olarak kendi ortamınız için sınıflandırma yapıları derecelendirme kümesi oluşturabilirsiniz.

Sınıflandırma taksonominizi Azure kuruluşunuzun tamamı için tek bir yerde tanımlayabilir ve özelleştirebilirsiniz. Bu konum, güvenlik ilkenizin bir parçası olarak [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)'nde bulunur. Yalnızca kuruluşun kök yönetim grubunda yönetici haklarına sahip bir kişi bu görevi gerçekleştirebilir.

SQL Information Protection ilke yönetiminin bir parçası olarak özel Etiketler tanımlayabilir, bunları derecelendirip bunları seçili bir bilgi türleri kümesiyle ilişkilendirebilirsiniz. Ayrıca, kendi özel bilgi türlerinizi ekleyebilir ve bunları dize desenleriyle yapılandırabilirsiniz. Desenler, veritabanlarınızdaki bu tür verileri tanımlamak için bulma mantığına eklenir.

[SQL Information Protection ilkesi nasıl yapılır kılavuzunda](https://go.microsoft.com/fwlink/?linkid=2009845&clcid=0x409)ilkenizi özelleştirme ve yönetme hakkında daha fazla bilgi edinin.

Kuruluş genelinde ilke tanımlandıktan sonra, özelleştirilmiş ilkenizi kullanarak ayrı veritabanlarını sınıflandırarak devam edebilirsiniz.

### <a name="classify-your-database"></a>Veritabanınızı sınıflandır

> [!NOTE]
> Aşağıdaki örnek Azure SQL veritabanı 'nı kullanır, ancak veri bulma & sınıflandırmasını yapılandırmak istediğiniz uygun ürünü seçmeniz gerekir.

1. [Azure Portal](https://portal.azure.com)gidin.

2. Azure SQL veritabanı bölmesinizdeki **güvenlik** başlığı altında **Gelişmiş veri güvenliği** ' ne gidin. **Gelişmiş veri güvenliği**' ni seçin ve ardından **veri bulma & sınıflandırma** kartını seçin.

   ![Azure portal gelişmiş veri güvenliği bölmesi](./media/data-discovery-and-classification-overview/data_classification.png)

3. **Veri bulma & sınıflandırması** sayfasında, **genel bakış** sekmesi veritabanının geçerli sınıflandırma durumunun bir özetini içerir. Özet, yalnızca belirli şema parçalarını, bilgi türlerini ve etiketleri göstermek üzere filtreleyebileceğiniz tüm sınıflandırılan sütunların ayrıntılı bir listesini içerir. Hiçbir sütunu henüz sınıflandırmadıysanız [5. adıma atlayın](#step-5).

   ![Geçerli sınıflandırma durumunun özeti](./media/data-discovery-and-classification-overview/2_data_classification_overview_dashboard.png)

4. Excel biçiminde bir rapor indirmek için bölmenin üst menüsünde **dışarı aktar** ' ı seçin.

5. <a id="step-5"></a>Verilerinizi **sınıflandırmayla** başlamak için **veri bulma & sınıflandırma** sayfasında sınıflandırma sekmesini seçin.

    Sınıflandırma Altyapısı, veritabanınızı potansiyel olarak hassas veriler içeren sütunlar için tarar ve önerilen sütun sınıflandırmalarının bir listesini sağlar.

6. Sınıflandırma önerilerini görüntüleyin ve uygulayın:

   - Önerilen sütun sınıflandırmalarının listesini görüntülemek için bölmenin alt kısmındaki öneriler panelini seçin.

   - Belirli bir sütunun önerisini kabul etmek için ilgili satırın sol sütunundaki onay kutusunu seçin. Tüm önerileri kabul edildi olarak işaretlemek için, öneriler tablosu üstbilgisindeki en soldaki onay kutusunu seçin.

       ![Sınıflandırma önerileri listesini gözden geçirin ve seçin](./media/data-discovery-and-classification-overview/6_data_classification_recommendations_list.png)

   - Seçili önerileri uygulamak için **Seçili önerileri kabul et**' i seçin.

7. Sütunları, alternatif olarak veya öneri tabanlı sınıflandırmanın yanı sıra el ile de sınıflandırabilirsiniz:

   1. Bölmenin üst menüsünde **Sınıflandırma Ekle** ' yi seçin.

   1. Açılan bağlam penceresinde, sınıflandırmak istediğiniz şemayı, tabloyu ve sütunu ve bilgi türünü ve duyarlılık etiketini seçin.

   1. Bağlam penceresinin alt kısmında **Sınıflandırma Ekle** ' yi seçin.

      ![Sınıflamak için bir sütun seçin](./media/data-discovery-and-classification-overview/9_data_classification_manual_classification.png)

8. Sınıflandırmanızı ve kalıcı olarak etiketlemesini (etiketleyerek) Yeni sınıflandırma meta verileri ile veritabanı sütunlarını kullanarak, pencerenin üst menüsünde **Kaydet** ' i seçin.

## <a name="audit-access-to-sensitive-data"></a><a id="audit-sensitive-data"></a>Gizli verilere erişimi denetleme

Bilgi koruma paradigmasının önemli bir yönü, hassas verilere erişimi izleme olanağıdır. [Azure SQL denetimi](../../azure-sql/database/auditing-overview.md) , adlı denetim günlüğüne yeni bir alan içerecek şekilde geliştirilmiştir `data_sensitivity_information` . Bu alan, bir sorgu tarafından döndürülen verilerin duyarlılık sınıflandırmalarını (etiketleri) günlüğe kaydeder. İşte bir örnek:

![Denetim günlüğü](./media/data-discovery-and-classification-overview/11_data_classification_audit_log.png)

## <a name="permissions"></a><a id="permissions"></a>İzinler

Bu yerleşik roller bir veritabanının veri sınıflandırmasını okuyabilir:

- Sahip
- Okuyucu
- Katılımcı
- SQL Güvenlik Yöneticisi
- Kullanıcı Erişimi Yöneticisi

Bu yerleşik roller bir veritabanının veri sınıflandırmasını değiştirebilir:

- Sahip
- Katılımcı
- SQL Güvenlik Yöneticisi

[Azure kaynakları Için RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)'de rol tabanlı izinler hakkında daha fazla bilgi edinin.

## <a name="manage-classifications"></a><a id="manage-classification"></a>Sınıflandırmaları Yönet

Sınıflandırmaları yönetmek için T-SQL, REST API veya PowerShell kullanabilirsiniz.

### <a name="use-t-sql"></a>T-SQL kullanma

T-SQL ' i sütun sınıflandırmaları eklemek veya kaldırmak ve tüm veritabanının sınıflandırmalarını almak için kullanabilirsiniz.

> [!NOTE]
> Etiketleri yönetmek için T-SQL kullandığınızda, bir sütuna eklediğiniz etiketlerin, kuruluşun bilgi koruma ilkesinde (portal önerilerinde görünen etiketler kümesi) var olduğunu belirten bir doğrulama yoktur. Bu nedenle, bunu doğrulamak sizin için en iyisidir.

Sınıflandırmalar için T-SQL kullanma hakkında daha fazla bilgi için aşağıdaki başvurulara bakın:

- Bir veya daha fazla sütunun sınıflandırmasını eklemek veya güncelleştirmek için: [DUYARLıLıK sınıflandırması Ekle](https://docs.microsoft.com/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
- Sınıflandırmayı bir veya daha fazla sütundan kaldırmak için: [DÜŞÜRÜLME DUYARLıLıK sınıflandırması](https://docs.microsoft.com/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
- Veritabanındaki tüm sınıflandırmaları görüntülemek için: [sys. sensitivity_classifications](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

### <a name="use-powershell-cmdlets"></a>PowerShell cmdlet'leri kullanma
PowerShell kullanarak Azure SQL veritabanı ve Azure SQL yönetilen örneği için sınıflandırmaları ve önerileri yönetin.

#### <a name="powershell-cmdlets-for-azure-sql-database"></a>Azure SQL veritabanı için PowerShell cmdlet 'leri

- [Get-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityclassification)
- [Set-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesensitivityclassification)
- [Remove-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesensitivityclassification)
- [Get-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityrecommendation)
- [Enable-AzSqlDatabaSesensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqldatabasesensitivityrecommendation)
- [Disable-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqldatabasesensitivityrecommendation)

#### <a name="powershell-cmdlets-for-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği için PowerShell cmdlet 'leri

- [Get-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityclassification)
- [Set-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasesensitivityclassification)
- [Remove-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabasesensitivityclassification)
- [Get-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityrecommendation)
- [Enable-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqlinstancedatabasesensitivityrecommendation)
- [Disable-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqlinstancedatabasesensitivityrecommendation)

### <a name="use-the-rest-api"></a>REST API 'YI kullanma

Sınıflandırmaları ve önerileri programlı bir şekilde yönetmek için REST API kullanabilirsiniz. Yayımlanan REST API aşağıdaki işlemleri destekler:

- [Oluştur veya Güncelleştir](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate): belirtilen sütunun duyarlılık etiketini oluşturur veya güncelleştirir.
- [Sil](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete): belirtilen sütunun duyarlılık etiketini siler.
- [Öneriyi devre dışı bırak](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/disablerecommendation): belirtilen sütunda duyarlılık önerilerini devre dışı bırakır.
- [Öneriyi etkinleştir](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/enablerecommendation): belirtilen sütunda duyarlılık önerilerini etkinleştirir. (Öneriler varsayılan olarak tüm sütunlarda etkindir.)
- [Get](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get): belirtilen sütunun duyarlılık etiketini alır.
- [Geçerli veritabanına göre Listele](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listcurrentbydatabase): belirtilen veritabanının geçerli duyarlılık etiketlerini alır.
- [Veritabanı tarafından önerilen liste](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listrecommendedbydatabase): belirtilen veritabanının önerilen duyarlılık etiketlerini alır.

## <a name="next-steps"></a><a id="next-steps"></a>Sonraki adımlar

- [Gelişmiş veri güvenliği](advanced-data-security.md)hakkında daha fazla bilgi edinin.
- Sınıflandırılan gizli verilerinize erişimi izlemek ve denetlemek için [Azure SQL denetimini](../../azure-sql/database/auditing-overview.md) yapılandırmayı düşünün.
- Veri bulma ve sınıflandırma içeren bir sunum için bkz. [SQL verilerini korumak & bulma, sınıflandırma ve etiketleme | Veriler kullanıma sunuldu](https://www.youtube.com/watch?v=itVi9bkJUNc).
