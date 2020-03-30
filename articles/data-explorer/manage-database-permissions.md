---
title: Azure Veri Gezgini'nde veritabanı izinlerini yönetme
description: Bu makalede, Azure Veri Gezgini'ndeki veritabanları ve tablolar için rol tabanlı erişim denetimleri açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: b4d5e56e990c0353f44209c6b19ae2d1727de27a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76030094"
---
# <a name="manage-azure-data-explorer-database-permissions"></a>Azure Veri Gezgini veritabanı izinlerini yönetme

Azure Veri Gezgini, *rol tabanlı* erişim denetim modelini kullanarak veritabanlarına ve tablolara erişimi denetlemenize olanak tanır. Bu modelde, *ilkeler* (kullanıcılar, gruplar ve uygulamalar) *rollere*eşlenir. Müdürler, atandıkları rollere göre kaynaklara erişebilirler.

Bu makalede, kullanılabilir roller ve Azure portalı ve Azure Veri Gezgini yönetim komutlarını kullanarak bu rollere nasıl ilkeler atayılanın açıklandığı açıklanmaktadır.

## <a name="roles-and-permissions"></a>Roller ve izinler

Azure Veri Gezgini aşağıdaki rollere sahiptir:

|Rol                       |İzinler                                                                        |
|---------------------------|-----------------------------------------------------------------------------------|
|Veritabanı yöneticisi             |Belirli bir veritabanı kapsamında her şeyi yapabilir.|
|Veritabanı kullanıcısı              |Veritabanındaki tüm verileri ve meta verileri okuyabilir. Ayrıca, tablolar (bu tablo için tablo yöneticisi olma) ve veritabanında işlevler oluşturabilirsiniz.|
|Veritabanı görüntüleyici            |Veritabanındaki tüm verileri ve meta verileri okuyabilir.|
|Veritabanı yutultörü          |Veritabanındaki varolan tüm tablolara veri alabilir, ancak verileri sorgulayamaz.|
|Veritabanı monitörü           |'.show ...' yürütmek olabilir veritabanı ve alt varlıkları bağlamında komutları.|
|Tablo yöneticisi                |Belirli bir tablo kapsamında her şeyi yapabilirsiniz. |
|Tablo yutucu             |Belirli bir tablo nun kapsamındaki verileri alabilir, ancak verileri sorgulayamaz.|

## <a name="manage-permissions-in-the-azure-portal"></a>Azure portalında izinleri yönetme

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Azure Veri Gezgini kümenize gidin.

1. Genel **Bakış** bölümünde, izinleri yönetmek istediğiniz veritabanını seçin.

    ![Veritabanı seçin](media/manage-database-permissions/select-database.png)

1. **İzinleri** seçin ve **sonra Ekle.**

    ![Veritabanı izinleri](media/manage-database-permissions/database-permissions.png)

1. **Veritabanı izinleri ekle**altında, asıl sorumluya atamak istediğiniz rolü seçin ve ardından **asılları seçin.**

    ![Veritabanı izinleri ekleme](media/manage-database-permissions/add-permission.png)

1. Anaparaya bakın, seçin ve sonra **seçin.**

    ![Azure portalında izinleri yönetme](media/manage-database-permissions/new-principals.png)

1. **Kaydet'i**seçin.

    ![Azure portalında izinleri yönetme](media/manage-database-permissions/save-permission.png)

## <a name="manage-permissions-with-management-commands"></a>Yönetim komutlarıyla izinleri yönetme

1. Oturum aç [https://dataexplorer.azure.com](https://dataexplorer.azure.com)ve kümenizi zaten kullanılamıyorsa ekleyin.

1. Sol bölmede, uygun veritabanını seçin.

1. Rollere `.add` ilkeler atamak için komutu kullanın: `.add database databasename rolename ('aaduser | aadgroup=user@domain.com')`. Veritabanı kullanıcı rolüne bir kullanıcı eklemek için, veritabanı adınızı ve kullanıcınızı yerine aşağıdaki komutu çalıştırın.

    ```Kusto
    .add database <TestDatabase> users ('aaduser=<user@contoso.com>')
    ```

    Komutun çıktısı, varolan kullanıcıların listesini ve veritabanında atandıkları rolleri gösterir.
    
    Azure Active Directory ve Kusto yetkilendirme modeliyle ilgili örnekler için lütfen [İlkeler ve Kimlik Sağlayıcıları](https://docs.microsoft.com/azure/kusto/management/access-control/principals-and-identity-providers)

## <a name="next-steps"></a>Sonraki adımlar

[Sorgu yazma](write-queries.md)
