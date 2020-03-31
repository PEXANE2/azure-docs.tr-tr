---
title: Kota artışı isteğinde bulunun
description: Bu sayfada, Azure SQL Veritabanı tek veritabanları, sunucular ve yönetilen örnekleriçin kotaları artırmak için bir destek isteği nasıl oluşturulacağı açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.topic: conceptual
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: sstein
ms.date: 02/04/2020
ms.openlocfilehash: ff2be6972bb4e8af266d0aa8a56d1879bc1b8b78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586164"
---
# <a name="request-quota-increases-for-azure-sql-database"></a>Azure SQL Veritabanı için kota isteği

Bu makalede, tek veritabanları, sunucular ve yönetilen örnekler için Azure SQL Veritabanı için kota artışı isteği nasıl açıklanmaktadır. Ayrıca, bir bölgeye abonelik erişiminin nasıl etkinleştirilen açıklanmaktadır.

## <a name="create-a-new-support-request"></a><a id="newquota"></a>Yeni bir destek isteği oluşturma

SQL Veritabanı için Azure portalından yeni bir destek isteği oluşturmak için aşağıdaki adımları kullanın.

1. Azure [portalı](https://portal.azure.com) menüsünde **Yardım + destek'i**seçin.

   ![Yardım + destek bağlantısı](./media/quota-increase-request/help-plus-support.png)

1. **Yardım + destek'** te Yeni **destek isteğini**seçin.

    ![Yeni bir destek isteği oluşturma](./media/quota-increase-request/new-support-request.png)

1. **Sorun türü için**Hizmet ve abonelik **limitlerini (kotalar)** seçin.

   ![Sorun türü seçin](./media/quota-increase-request/select-quota-issue-type.png)

1. **Abonelik**için, kotasını artırmak istediğiniz aboneliği seçin.

   ![Artırılmış kota için abonelik seçme](./media/quota-increase-request/select-subscription-support-request.png)

1. **Kota türü için**aşağıdaki kota türlerinden birini seçin:

   - Tek veritabanı ve elastik havuz kotaları için **SQL Veritabanı.**
   - Yönetilen örnekler için **SQL Veritabanı Yönetilen Örnek.**

   Sonra **Sonraki seçin: Çözümler >>. **

   ![Kota türü seçin](./media/quota-increase-request/select-quota-type.png)

1. **Ayrıntılar** penceresinde, ek bilgi girmek için Ayrıntıları Ver'i'ni seçin. **Provide details**

   !["Ayrıntıları ver" bağlantısı](./media/quota-increase-request/provide-details-link.png)

Ayrıntıları **Sağla'yı** tıklatmak, ek bilgi eklemenize olanak tanıyan **Kota ayrıntıları** penceresini görüntüler. Aşağıdaki bölümlerde **SQL Veritabanı** ve **SQL Veritabanı Yönetilen Örnek** kota türleri için farklı seçenekler açıklanmıştır.

## <a name="sql-database-quota-types"></a><a id="sqldbquota"></a>SQL Veritabanı kota türleri

Aşağıdaki bölümlerde **SQL Veritabanı** kota türleri için üç kota artış seçeneği açıklanmıştır:

- Sunucu başına veritabanı işlem birimleri (DTS)
- Abonelik başına sunucular
- Bir bölgeye abonelik erişimini etkinleştirme

### <a name="database-transaction-units-dtus-per-server"></a>Sunucu başına veritabanı işlem birimleri (DTS)

Sunucu başına DT'lerde artış istemek için aşağıdaki adımları kullanın.

1. Sunucu kota türü **başına Veritabanı hareket birimlerini (DTUs)** seçin.

1. **Kaynak** listesinde, hedeflenene kadar kaynağı seçin.

1. Yeni **kota** alanında, istediğiniz yeni DTU sınırını girin.

   ![DTÜ kota detayları](./media/quota-increase-request/quota-details-dtus.png)

Daha fazla bilgi için, [DTU satın alma modelini kullanarak tek veritabanları için Kaynak sınırları](sql-database-dtu-resource-limits-single-databases.md) ve [DTU satın alma modelini kullanan esnek havuzlar için Kaynaklar sınırlarına](sql-database-dtu-resource-limits-elastic-pools.md)bakın.

### <a name="servers-per-subscription"></a>Abonelik başına sunucular

Abonelik başına sunucu sayısında artış istemek için aşağıdaki adımları kullanın.

1. Abonelik kota türü **başına Sunucular'ı** seçin.

1. **Konum** listesinde, kullanmak üzere Azure bölgesini seçin. Kota her bölgedeki abonelik başına.

1. Yeni **kota** alanında, o bölgedeki en fazla sunucu sayısı için isteğinizi girin.

   ![Sunucular kota ayrıntıları](./media/quota-increase-request/quota-details-servers.png)

Daha fazla bilgi için SQL [Veritabanı kaynak sınırları ve kaynak yönetimi](sql-database-resource-limits-database-server.md)ne bürünme.

### <a name="enable-subscription-access-to-a-region"></a><a id="other"></a>Bir bölgeye abonelik erişimini etkinleştirme

Bazı teklif türleri her bölgede kullanılamaz. Aşağıdaki gibi bir hata görebilirsiniz:

`This location is not available for subscription`

Aboneliğinizin belirli bir bölgede erişmesi gerekiyorsa, erişim istemek için lütfen **Diğer kota isteği** seçeneğini kullanın. İsteğiniz, bölge için etkinleştirmek istediğiniz teklifi ve SKU ayrıntılarını belirtin. Teklifi ve SKU seçeneklerini incelemek için [Azure SQL Veritabanı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/sql-database/single/)bakın.

![Diğer kota ayrıntıları](./media/quota-increase-request/quota-details-whitelisting.png)

## <a name="managed-instance-quota-type"></a><a id="sqlmiquota"></a>Yönetilen örnek kota türü

SQL **Server Yönetilen Örnek** kota türü için aşağıdaki adımları kullanın:

1. **Bölge** listesinde, hedeflenebilmek için Azure bölgesini seçin.

1. **Subnet** ve **vCore**için talep ettiğiniz yeni sınırları girin.

   ![Yönetilen örnek kota ayrıntıları](./media/quota-increase-request/quota-details-managed-instance.png)

Daha fazla bilgi için [bkz.](sql-database-managed-instance-resource-limits.md)

## <a name="submit-your-request"></a>İsteğinizi gönderin

Son adım, SQL Veritabanı kota isteğinizin kalan ayrıntılarını doldurmaktır. Sonra **İleri seçin: Gözden Geçir +>>oluştur **ve istek ayrıntılarını inceledikten sonra, isteği göndermek için **Oluştur'u** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Talebinizi gönderdikten sonra, gözden geçirilecektir. Formda vermiş olduğunuz bilgilere dayalı bir cevap ile irtibata geçilecektir.

Diğer Azure sınırları hakkında daha fazla bilgi için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)abakın.
