---
title: Kota artışı iste
description: Bu sayfa, Azure SQL veritabanı tek veritabanı, sunucu ve yönetilen örnekler için kotaları artırmak üzere bir destek isteği oluşturmayı açıklar.
services: sql-database
ms.service: sql-database
ms.topic: conceptual
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: sstein
ms.date: 02/04/2020
ms.openlocfilehash: ff2be6972bb4e8af266d0aa8a56d1879bc1b8b78
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77586164"
---
# <a name="request-quota-increases-for-azure-sql-database"></a>Azure SQL veritabanı için istek kotası artışı

Bu makalede, Azure SQL veritabanı için tek veritabanlarına, sunuculara ve yönetilen örneklere yönelik bir kota artışı isteme açıklanmaktadır. Ayrıca, bir bölgeye abonelik erişiminin nasıl etkinleştirileceğini açıklar.

## <a name="create-a-new-support-request"></a><a id="newquota"></a>Yeni bir destek isteği oluşturun

SQL veritabanı için Azure portal yeni bir destek isteği oluşturmak için aşağıdaki adımları kullanın.

1. [Azure Portal](https://portal.azure.com) menüsünde **Yardım + Destek**' i seçin.

   ![Yardım + destek bağlantısı](./media/quota-increase-request/help-plus-support.png)

1. **Yardım + Destek**bölümünde **Yeni destek isteği**' ni seçin.

    ![Yeni bir destek isteği oluşturun](./media/quota-increase-request/new-support-request.png)

1. **Sorun türü**için **hizmet ve abonelik sınırları (kotalar)** öğesini seçin.

   ![Sorun türü seçin](./media/quota-increase-request/select-quota-issue-type.png)

1. **Abonelik**için, kotasını artırmak istediğiniz aboneliği seçin.

   ![Daha fazla kota için bir abonelik seçin](./media/quota-increase-request/select-subscription-support-request.png)

1. **Kota türü**için aşağıdaki kota türlerinden birini seçin:

   - Tek veritabanı ve elastik havuz kotaları için **SQL veritabanı** .
   - Yönetilen örnekler için **SQL veritabanı yönetilen örneği** .

   Ardından Ileri ' yi seçin **: çözümler >>**.

   ![Kota türü seçin](./media/quota-increase-request/select-quota-type.png)

1. **Ayrıntılar** penceresinde, ek bilgi girmek Için **Ayrıntılar sağla** ' yı seçin.

   !["Ayrıntıları sağla" bağlantısı](./media/quota-increase-request/provide-details-link.png)

**Ayrıntıları sağla** 'ya tıkladığınızda ek bilgi eklemenize olanak tanıyan **Kota ayrıntıları** penceresi görüntülenir. Aşağıdaki bölümlerde **SQL veritabanı** ve **SQL veritabanı yönetilen örnek** kota türleri için farklı seçenekler açıklanır.

## <a name="sql-database-quota-types"></a><a id="sqldbquota"></a>SQL veritabanı kota türleri

Aşağıdaki bölümlerde, **SQL veritabanı** kota türleri için üç kota artışı seçeneği açıklanır:

- Sunucu başına veritabanı işlem birimi (DTU)
- Abonelik başına sunucu sayısı
- Bir bölgeye abonelik erişimini etkinleştirme

### <a name="database-transaction-units-dtus-per-server"></a>Sunucu başına veritabanı işlem birimi (DTU)

Sunucu başına DTU 'Lar için bir artış istemek üzere aşağıdaki adımları kullanın.

1. Sunucu kota türü **başına veritabanı işlem birimleri (DTU)** seçin.

1. **Kaynak** listesinde, hedeflenecek kaynağı seçin.

1. **Yeni kota** alanına, ISTEDIĞINIZ yeni DTU limitini girin.

   ![DTU kota ayrıntıları](./media/quota-increase-request/quota-details-dtus.png)

Daha fazla bilgi için, DTU satın alma [modelini kullanan elastik havuzlar IÇIN](sql-database-dtu-resource-limits-elastic-pools.md) [DTU satın alma modeli ve kaynak sınırlarını kullanan tek veritabanlarına yönelik kaynak limitleri](sql-database-dtu-resource-limits-single-databases.md) bölümüne bakın.

### <a name="servers-per-subscription"></a>Abonelik başına sunucu sayısı

Abonelik başına sunucu sayısında artış istemek için aşağıdaki adımları kullanın.

1. **Abonelik başına sunucu** kotası türünü seçin.

1. **Konum** listesinde, kullanılacak Azure bölgesini seçin. Kota her bölgede abonelik başına olur.

1. **Yeni kota** alanına, bu bölgedeki en fazla sunucu sayısı için isteğinizi girin.

   ![Sunucu kotası ayrıntıları](./media/quota-increase-request/quota-details-servers.png)

Daha fazla bilgi için bkz. [SQL veritabanı kaynak limitleri ve kaynak](sql-database-resource-limits-database-server.md)İdaresi.

### <a name="enable-subscription-access-to-a-region"></a><a id="other"></a>Bir bölgeye abonelik erişimini etkinleştirme

Bazı teklif türleri her bölgede kullanılamaz. Aşağıdakiler gibi bir hata görebilirsiniz:

`This location is not available for subscription`

Aboneliğiniz belirli bir bölgede erişime ihtiyacı varsa, lütfen erişim istemek için **diğer kota isteği** seçeneğini kullanın. İsteğiniz içinde, bölge için etkinleştirmek istediğiniz teklif ve SKU ayrıntılarını belirtin. Teklif ve SKU seçeneklerini araştırmak için bkz. [Azure SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/single/).

![Diğer kota ayrıntıları](./media/quota-increase-request/quota-details-whitelisting.png)

## <a name="managed-instance-quota-type"></a><a id="sqlmiquota"></a>Yönetilen örnek kota türü

**SQL Server yönetilen örnek** kota türü için aşağıdaki adımları kullanın:

1. **Bölge** listesinde, hedeflenecek Azure bölgesini seçin.

1. **Alt ağ** ve **sanal çekirdek**için istediğiniz yeni limitleri girin.

   ![Yönetilen örnek kotası ayrıntıları](./media/quota-increase-request/quota-details-managed-instance.png)

Daha fazla bilgi için bkz. [Azure SQL veritabanı yönetilen örneği kaynak sınırlarına genel bakış](sql-database-managed-instance-resource-limits.md).

## <a name="submit-your-request"></a>İsteğinizi gönderme

Son adım, SQL veritabanı kota isteğinizin kalan ayrıntılarını doldurmasıdır. Ardından Ileri ' yi seçin **: gözden geçir + oluştur>>**, istek ayrıntılarını inceledikten sonra, isteği göndermek için **Oluştur** ' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

İsteğinizi gönderdikten sonra gözden geçirilir. Formda verdiğiniz bilgilere göre bir Yanıt ile iletişim kurulacaksınız.

Diğer Azure limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md).
