---
title: İstek kotası artar ve destek alın
description: Azure SYNAPSE Analytics için Azure portal bir destek isteği oluşturma. İstek kotası artar veya sorun çözümleme desteğini alır.
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/10/2020
author: kevinvngo
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: eaa6ceded9893b278b28d517cbddb303f8469e10
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80350891"
---
# <a name="request-quota-increases-and-get-support-for-azure-synapse-analytics"></a>İstek kotası artar ve Azure SYNAPSE Analytics desteği alınır

Bu makalede, Azure SYNAPSE Analytics için Azure portal bir destek bileti gönderme açıklanır. Bu işlem, bir kota artışı talep etmenizi veya mühendislik desteği ekibine yönelik teknik destek talebi göndermenizi sağlar.

## <a name="create-a-support-ticket"></a>Destek bileti oluşturma

Azure SYNAPSE Analytics için Azure portal yeni bir destek isteği oluşturmak için aşağıdaki adımları kullanın.

1. [Azure Portal](https://portal.azure.com) menüsünde **Yardım + Destek**' i seçin.

   ![Yardım + destek bağlantısı](./media/sql-data-warehouse-get-started-create-support-ticket/help-plus-support.png)


1. **Yardım + Destek**bölümünde **Yeni destek isteği**' ni seçin.

    ![Yeni bir destek isteği oluşturun](./media/sql-data-warehouse-get-started-create-support-ticket/new-support-request.png)

1. [Azure destek planınızı](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/)gözden geçirin.

   * Tüm destek düzeylerinde **faturalama, kota ve abonelik yönetimi** desteği sunulmaktadır.
   * **Onarım** desteği [Geliştirici](https://azure.microsoft.com/support/plans/developer/), [Standart](https://azure.microsoft.com/support/plans/standard/), [profesyonel doğrudan](https://azure.microsoft.com/support/plans/prodirect/)veya [Premier](https://azure.microsoft.com/support/plans/premier/) destek aracılığıyla sağlanır. Onarım sorunları, müşterilerin Azure'ı kullandığı sırada karşılaştıkları ve ilgili sorunun Microsoft'tan kaynaklandığına ilişkin makul bir olasılığın bulunduğu sorunlardır.
   * **Geliştirici rehberliği** ve **danışmanlık hizmetleri**, [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) ve [Premier](https://azure.microsoft.com/support/plans/premier/) destek düzeylerinde kullanılabilir.

   Premier Destek planınız varsa, [Microsoft Premier çevrimiçi portalındaki](https://premier.microsoft.com/)Azure SYNAPSE Analytics sorunlarını da bildirebilirsiniz. Kapsam, yanıt süreleri, fiyatlandırma vb. dahil olmak üzere çeşitli destek planları hakkında daha fazla bilgi edinmek için bkz. [Azure destek planları](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/) .  Azure desteği hakkında sık sorulan sorular için bkz. [Azure desteği SSS](https://azure.microsoft.com/support/faq/).

1. **Sorun türü**için uygun sorun türünü seçin. Onarım sorunları için **Teknik**' i seçin. Kota artışı istekleri için **hizmet ve abonelik sınırları (kotalar)** öğesini seçin.

   ![Sorun türü seçin](./media/sql-data-warehouse-get-started-create-support-ticket/select-quota-issue-type.png)  

   > [!NOTE]
   > Bu makalenin geri kalanı, kota artışı istekleri üzerine odaklandırırsınız. Ancak sorun çözümü destek istekleri için burada **Teknik** ' i de seçebilirsiniz. **Teknik**' i seçerseniz, bir Özet girmeniz ve ardından **sorun türünü seç**' i seçerek bir sorun türü belirlemeniz istenir. Sorununuzu gidermeye yardımcı olabilecek çözümler görebilirsiniz. Sunulan çözümler sorununuzu gidermezse, **İleri** ' yi seçin ve destek bileti göndermek için formu doldurun.

1. Kota artışı istekleri için **Kota türü**Için **Azure SYNAPSE Analytics** ' i seçin. Ardından Ileri ' yi seçin **: çözümler >>**.

   ![Kota türü seçin](./media/sql-data-warehouse-get-started-create-support-ticket/select-quota-type.png)

1. **Ayrıntılar** penceresinde, ek bilgi girmek Için **Ayrıntılar sağla** ' yı seçin.

   !["Ayrıntıları sağla" bağlantısı](./media/sql-data-warehouse-get-started-create-support-ticket/provide-details-link.png)

## <a name="quota-request-types"></a>Kota istek türleri

**Ayrıntıları sağla** 'ya tıkladığınızda ek bilgi eklemenize olanak tanıyan **Kota ayrıntıları** penceresi görüntülenir. Aşağıdaki bölümlerde, Azure SYNAPSE Analytics için kullanılabilen farklı kota istekleri açıklanır.

### <a name="data-warehouse-units-dwus-per-server"></a>Sunucu başına veri ambarı birimleri (DWU)

Sunucu başına DWU 'da artış istemek için aşağıdaki adımları kullanın.

1. Sunucu kota türü **başına veri ambarı birimlerini (DTU)** seçin.

1. **Kaynak** listesinde, hedeflenecek kaynağı seçin.

1. **İstek kotası** alanına istediğiniz yeni DWU limitini girin.

   ![DWU kota ayrıntıları](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-dwus.png)

### <a name="servers-per-subscription"></a>Abonelik başına sunucu sayısı

Abonelik başına sunucu sayısında artış istemek için aşağıdaki adımları kullanın.

1. **Abonelik başına sunucu** kotası türünü seçin.

1. **Konum** listesinde, kullanılacak Azure bölgesini seçin. Kota her bölgede abonelik başına olur.

1. **Yeni kota** alanına, bu bölgedeki en fazla sunucu sayısı için isteğinizi girin.

   ![Sunucu kotası ayrıntıları](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-servers.png)

### <a name="enable-subscription-access-to-a-region"></a>Bir bölgeye abonelik erişimini etkinleştirme

Bazı teklif türleri her bölgede kullanılamaz. Aşağıdakiler gibi bir hata görebilirsiniz:

`This location is not available for subscription`

Aboneliğiniz belirli bir bölgede erişime ihtiyacı varsa, lütfen erişim istemek için **diğer kota isteği** seçeneğini kullanın. İsteğiniz içinde, bölge için etkinleştirmek istediğiniz teklif ve SKU ayrıntılarını belirtin. Teklif ve SKU seçeneklerini araştırmak için bkz. [Azure SYNAPSE Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/synapse-analytics/).

![Diğer kota ayrıntıları](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-whitelisting.png)

## <a name="submit-your-request"></a>İsteğinizi gönderme

Son adım, SQL veritabanı destek isteğinizin geri kalan ayrıntılarını doldurmasıdır. Ardından Ileri ' yi seçin **: gözden geçir + oluştur>>**, istek ayrıntılarını inceledikten sonra, isteği göndermek için **Oluştur** ' a tıklayın.

## <a name="monitor-a-support-ticket"></a>Destek biletini izleme

Destek isteğini gönderdikten sonra, Azure destek ekibi sizinle iletişim kuracaktır. İstek durumunuzu ve ayrıntılarını kontrol etmek için panoda bulunan **Tüm destek istekleri** kutucuğuna tıklayın.

![Durumu kontrol etme](./media/sql-data-warehouse-get-started-create-support-ticket/monitor-ticket.png)

## <a name="other-resources"></a>Diğer kaynaklar

[Stack Overflow](https://stackoverflow.com/questions/tagged/azure-synapse+or+azure-sql-data-warehouse) veya [Azure SQL veri ambarı MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/)aracılığıyla Azure SYNAPSE Analytics Community ile de bağlanabilirsiniz.

