---
title: Sunucu güven grubu
titleSuffix: Azure SQL Managed Instance
description: Sunucu güven grubu ve Azure SQL yönetilen örnekleri arasındaki güveni yönetme hakkında bilgi edinin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: sasapopo
ms.author: sasapopo
ms.reviewer: sstein, bonova
ms.date: 10/08/2020
ms.openlocfilehash: 911d7ffa2b1d313147ca73d0ceb285ea2e84b1f7
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91979870"
---
# <a name="use-server-trust-groups-to-setup-and-manage-trust-between-sql-managed-instances"></a>Sunucu güven gruplarını kullanarak SQL yönetilen örnekleri arasında güveni ayarlama ve yönetme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Sunucu güven grubu, Azure SQL yönetilen örnekleri arasındaki güveni yönetmek için kullanılan bir kavramdır. Bir grup oluşturup BT sertifika tabanlı güven için örnek ekleyerek tüm üyeler arasında oluşturulur ve bu, farklı çapraz örnek senaryolar için kullanılabilir. Sunucuları gruptan kaldırma veya grup silme, sunucular arasındaki güveni kaldırma ile sonuçlanır. Sunucu güven grubu oluşturmak veya silmek için kullanıcının yönetilen örnek üzerinde yazma izinlerine sahip olması gerekir.
[Sunucu güven grubu](https://aka.ms/mi-server-trust-group-arm) , **SQL güven grubu**adlı Azure Portal varlığına karşılık gelen bir Azure Resource Manager nesnesidir.

> [!NOTE]
> Sunucu güven grubu, Azure SQL yönetilen örnekleri arasındaki dağıtılmış işlemlerin genel önizlemede kullanıma sunulmuştur ve şu anda bu makalede daha sonra açıklanacak bazı sınırlamalar vardır.

## <a name="server-trust-group-setup"></a>Sunucu güven grubu kurulumu

Aşağıdaki bölümde sunucu güven grubunun kurulumu açıklanmaktadır.

1. [Azure portalına](https://portal.azure.com/) gidin.

2. Yeni oluşturulan bir sunucu güven grubuna eklemeyi planladığınız Azure SQL yönetilen örneği ' ne gidin.

3. **Güvenlik** ayarları ' na **SQL güven grupları** sekmesini seçin.

   :::image type="content" source="./media/server-trust-group-overview/security-sql-trust-groups.png" alt-text="Sunucu güven grupları":::

4. Sunucu güven grubu yapılandırması sayfasında **Yeni Grup** simgesini seçin.

   :::image type="content" source="./media/server-trust-group-overview/server-trust-group-create-new-group.png" alt-text="Sunucu güven grupları":::

5. **SQL güven grubu** oluştur dikey penceresinde **Grup adı**' nı ayarlayın. Grup üyelerinin bulunduğu tüm bölgelerde benzersiz olması gerekir. **Güven kapsamı** , sunucu güven grubuyla etkinleştirilen ve önizleme aşamasında yalnızca geçerli güven kapsamının **Dağıtılmış işlemler**olduğu çapraz örnek senaryonun türünü tanımlar, bu nedenle önceden seçilir ve değiştirilemez. Tüm **Grup üyeleri** aynı **aboneliğe** ait olmalıdır, ancak farklı kaynak grupları altında olabilir. Grubun üyesi olacak Azure SQL yönetilen örneğini seçmek için **kaynak grubunu** ve **SQL Server/örneğini** seçin.
   :::image type="content" source="./media/server-trust-group-overview/server-trust-group-create-blade.png" alt-text="Sunucu güven grupları":::

6. Tüm gerekli alanlar doldurulduktan sonra **Kaydet**' e tıklayın.

## <a name="server-trust-group-maintenance-and-deletion"></a>Sunucu güven grubu bakımı ve silme

Sunucu güven grubunu düzenlemenin bir yolu yoktur. Yönetilen bir örneği bir gruptan kaldırmak için, grubu silip yeni bir tane oluşturmanız gerekir.

Aşağıdaki bölümde sunucu güven grubu silme işlemi açıklanmaktadır. 
1. Azure portala gidin.
2. Güven grubuna ait yönetilen bir örneğe gidin.
3. **Güvenlik** ayarları ' na **SQL güven grupları** sekmesini seçin.
4. Silmek istediğiniz güven grubunu seçin.
   :::image type="content" source="./media/server-trust-group-overview/server-trust-group-manage-select.png" alt-text="Sunucu güven grupları":::
5. **Grubu Sil**' e tıklayın.
   :::image type="content" source="./media/server-trust-group-overview/server-trust-group-manage-delete.png" alt-text="Sunucu güven grupları":::
6. Silmeyi onaylamak için sunucu güven grubu adını yazın ve **Sil**' e tıklayın.
   :::image type="content" source="./media/server-trust-group-overview/server-trust-group-manage-delete-confirm.png" alt-text="Sunucu güven grupları":::

> [!NOTE]
> Sunucu güven grubunun silinmesi, iki yönetilen örnek arasındaki güveni hemen kaldıramayabilir. Güven kaldırma, yönetilen örneklerin [yük devretmesi](https://docs.microsoft.com/powershell/module/az.sql/Invoke-AzSqlInstanceFailover) çağrılarak zorlanabilir. Bu sorunun en son güncelleştirmelerine yönelik [bilinen sorunları](https://docs.microsoft.com/azure/azure-sql/database/doc-changes-updates-release-notes?tabs=managed-instance#known-issues) denetleyin.

## <a name="limitations"></a>Sınırlamalar

Genel Önizleme sırasında, sunucu güven grupları için aşağıdaki sınırlamalar geçerlidir.
 * Sunucu güven grubunun adı, üyelerinin bulunduğu tüm bölgelerde benzersiz olmalıdır.
 * Grup yalnızca Azure SQL yönetilen örnekleri içerebilir ve aynı Azure aboneliği altında olmalıdır.
 * Grupta tam olarak iki yönetilen örnek olabilir. Yönetilen örneklerin her çifti için sunucu güven grubu oluşturularak yapılabilecek, ikiden fazla yönetilen örnek arasında dağıtılmış işlemler yürütmeniz gerekiyorsa.
 * Dağıtılmış işlemler, sunucu güven grupları için geçerli olan kapsamdır.
 * Sunucu güven grubu yalnızca Azure portal yönetilebilir. PowerShell ve CLı desteği daha sonra sunulacaktır.
 * Sunucu güven grubu Azure portal düzenlenemiyor. Yalnızca oluşturulabilir veya bırakılabilir.
 * Dağıtılmış işlemlerin ek sınırlamaları senaryonuzla ilgili olabilir. Çoğu önemli, sunucu güven grubuna katılan SQL yönetilen örneklerine özel uç noktalar aracılığıyla erişilebilmelidir veya diğer bağlantıların sanal ağ düzeyinde çalışması gerekir. Lütfen [Azure SQL yönetilen örneği için geçerli dağıtılmış işlem sınırlamalarını](https://docs.microsoft.com/azure/azure-sql/database/elastic-transactions-overview#limitations)farkında olduğunuzdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

* Azure SQL yönetilen örneği 'nde dağıtılmış işlemler hakkında daha fazla bilgi için bkz. [Dağıtılmış işlemler](../database/elastic-transactions-overview.md).
* Yayın güncelleştirmeleri ve bilinen sorunlar durumu için bkz. [yönetilen örnek sürüm notları](../database/doc-changes-updates-release-notes.md).
* Özellik istekleri için lütfen bunları [yönetilen örnek forumuna](https://feedback.azure.com/forums/915676-sql-managed-instance)ekleyin.