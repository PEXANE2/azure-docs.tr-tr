---
title: Azure Veri Gezgini veritabanı izinlerini yönetme
description: Bu makalede, Azure Veri Gezgini veritabanları ve tabloları için rol tabanlı erişim denetimleri açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: b4d5e56e990c0353f44209c6b19ae2d1727de27a
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76030094"
---
# <a name="manage-azure-data-explorer-database-permissions"></a>Azure Veri Gezgini veritabanı izinlerini yönetme

Azure Veri Gezgini, *rol tabanlı erişim denetimi* modeli kullanarak veritabanlarına ve tablolara erişimi denetlemenize olanak sağlar. Bu modelin altında, *sorumlular* (kullanıcılar, gruplar ve uygulamalar) *rollerle*eşlenir. Sorumlular, atandıkları rollere göre kaynaklara erişebilir.

Bu makalede, kullanılabilir roller ve Azure portal ve Azure Veri Gezgini yönetim komutları kullanılarak bu rollere asıl adlar atama açıklanmaktadır.

## <a name="roles-and-permissions"></a>Roller ve izinler

Azure Veri Gezgini aşağıdaki rollere sahiptir:

|Rol                       |İzinler                                                                        |
|---------------------------|-----------------------------------------------------------------------------------|
|Veritabanı Yöneticisi             |Belirli bir veritabanının kapsamında her şeyi gerçekleştirebilir.|
|Veritabanı kullanıcısı              |, Veritabanındaki tüm verileri ve meta verileri okuyabilir. Bunlara ek olarak, tablolar (Bu tablo için tablo Yöneticisi haline geliyor) ve veritabanındaki işlevler oluşturabilirler.|
|Veritabanı Görüntüleyicisi            |, Veritabanındaki tüm verileri ve meta verileri okuyabilir.|
|Veritabanı alımı          |Verileri veritabanındaki tüm var olan tablolara alabilir, ancak verileri sorgulayamaz.|
|Veritabanı izleyicisi           |'. Show... ' yürütebilir veritabanı ve onun alt varlıkları bağlamındaki komutlar.|
|Tablo Yöneticisi                |Belirli bir tablonun kapsamında her şeyi gerçekleştirebilir. |
|Tablo alımı             |Verileri belirli bir tablonun kapsamına alabilir, ancak verileri sorgulayamaz.|

## <a name="manage-permissions-in-the-azure-portal"></a>Azure portal izinleri yönetme

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

1. Azure Veri Gezgini kümenize gidin.

1. **Genel bakış** bölümünde, izinleri yönetmek istediğiniz veritabanını seçin.

    ![Veritabanı seçin](media/manage-database-permissions/select-database.png)

1. **İzinler** ' i seçin ve ardından **ekleyin**.

    ![Veritabanı izinleri](media/manage-database-permissions/database-permissions.png)

1. **Veritabanı Izinleri Ekle**' nin altında, sorumluyu atamak istediğiniz rolü seçin ve ardından **sorumlular**' ı seçin.

    ![Veritabanı izinleri ekleme](media/manage-database-permissions/add-permission.png)

1. Sorumluyu bulun, seçin ve ardından öğesini **seçin**.

    ![Azure portal izinleri yönetme](media/manage-database-permissions/new-principals.png)

1. **Kaydet**’i seçin.

    ![Azure portal izinleri yönetme](media/manage-database-permissions/save-permission.png)

## <a name="manage-permissions-with-management-commands"></a>Yönetim komutlarıyla izinleri yönetme

1. [https://dataexplorer.azure.com](https://dataexplorer.azure.com)için oturum açın ve henüz yoksa kümenizi ekleyin.

1. Sol bölmede uygun veritabanını seçin.

1. Rollere asıl atamak için `.add` komutunu kullanın: `.add database databasename rolename ('aaduser | aadgroup=user@domain.com')`. Veritabanı kullanıcı rolüne bir kullanıcı eklemek için, veritabanı adınızı ve kullanıcısını değiştirerek aşağıdaki komutu çalıştırın.

    ```Kusto
    .add database <TestDatabase> users ('aaduser=<user@contoso.com>')
    ```

    Komutun çıktısı, var olan kullanıcıların ve veritabanında atandığı rollerin listesini gösterir.
    
    Azure Active Directory ve kusto yetkilendirme modeliyle ilgili örnekler için lütfen bkz. [ilkeler ve kimlik sağlayıcıları](https://docs.microsoft.com/azure/kusto/management/access-control/principals-and-identity-providers)

## <a name="next-steps"></a>Sonraki adımlar

[Sorgu yazma](write-queries.md)
