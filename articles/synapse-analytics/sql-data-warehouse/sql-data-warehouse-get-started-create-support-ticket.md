---
title: Kota artışları isteyin ve destek alın
description: Azure Synapse Analytics için Azure portalında destek isteği oluşturma. Kota artışlarını isteyin veya sorun çözme desteği alın.
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
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350891"
---
# <a name="request-quota-increases-and-get-support-for-azure-synapse-analytics"></a>Kota artışları isteyin ve Azure Synapse Analytics için destek alın

Bu makalede, Azure Synapse Analytics için Azure portalında destek biletinin nasıl gönderilen açıklanmaktadır. Bu işlem, kota artışı talep etmenizi veya mühendislik destek ekibi için teknik destek isteği göndermenizi sağlar.

## <a name="create-a-support-ticket"></a>Destek bileti oluşturma

Azure Synapse Analytics için Azure portalından yeni bir destek isteği oluşturmak için aşağıdaki adımları kullanın.

1. Azure [portalı](https://portal.azure.com) menüsünde **Yardım + destek'i**seçin.

   ![Yardım + destek bağlantısı](./media/sql-data-warehouse-get-started-create-support-ticket/help-plus-support.png)


1. **Yardım + destek'** te Yeni **destek isteğini**seçin.

    ![Yeni bir destek isteği oluşturma](./media/sql-data-warehouse-get-started-create-support-ticket/new-support-request.png)

1. Azure [destek planınızı](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/)gözden geçirin.

   * Tüm destek düzeylerinde **faturalama, kota ve abonelik yönetimi** desteği sunulmaktadır.
   * **Break-fix** desteği [Geliştirici,](https://azure.microsoft.com/support/plans/developer/) [Standart,](https://azure.microsoft.com/support/plans/standard/) [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/)veya [Premier](https://azure.microsoft.com/support/plans/premier/) desteği ile sağlanır. Onarım sorunları, müşterilerin Azure'ı kullandığı sırada karşılaştıkları ve ilgili sorunun Microsoft'tan kaynaklandığına ilişkin makul bir olasılığın bulunduğu sorunlardır.
   * **Geliştirici rehberliği** ve **danışmanlık hizmetleri**, [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) ve [Premier](https://azure.microsoft.com/support/plans/premier/) destek düzeylerinde kullanılabilir.

   Bir Premier destek planınız varsa, [Microsoft Premier çevrimiçi portalında](https://premier.microsoft.com/)Azure Synapse Analytics sorunlarını da bildirebilirsiniz. Azure [destek planlarına](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/) bakın kapsam, yanıt süreleri, fiyatlandırma vb. dahil olmak üzere çeşitli destek planları hakkında daha fazla bilgi edinin.  Azure desteği hakkında sık sorulan sorular için [Azure destek SSS'lerine](https://azure.microsoft.com/support/faq/)bakın.

1. **Sorun türü**için uygun sorun türünü seçin. Kesme düzeltme sorunları için **Teknik'i**seçin. Kota artış istekleri için **Hizmet ve abonelik limitlerini (kotalar)** seçin.

   ![Sorun türü seçin](./media/sql-data-warehouse-get-started-create-support-ticket/select-quota-issue-type.png)  

   > [!NOTE]
   > Bu makalenin geri kalanı kota artış isteklerine odaklanır. Ancak sorun giderme destek istekleri için **teknik** alanı da buradan seçebilirsiniz. **Teknik'i**seçerseniz, bir özet sağlamanız ve ardından sorun **türünü seç**seçeneğini seçerek bir sorun türünü belirlemeniz istenir. Sorununuzu çözmeye yardımcı olacak çözümler görebilirsiniz. Sunulan çözümler sorununuzu çözmezse, **Sonraki:Ayrıntılar'ı** seçin ve destek biletini göndermek için formu doldurun.

1. Kota artış istekleri için Kota türü için **Azure Synapse Analytics'i** seçin. **Quota type** Sonra **Sonraki seçin: Çözümler >>. **

   ![Kota türü seçin](./media/sql-data-warehouse-get-started-create-support-ticket/select-quota-type.png)

1. **Ayrıntılar** penceresinde, ek bilgi girmek için Ayrıntıları Ver'i'ni seçin. **Provide details**

   !["Ayrıntıları ver" bağlantısı](./media/sql-data-warehouse-get-started-create-support-ticket/provide-details-link.png)

## <a name="quota-request-types"></a>Kota istek türleri

Ayrıntıları **Sağla'yı** tıklatmak, ek bilgi eklemenize olanak tanıyan **Kota ayrıntıları** penceresini görüntüler. Aşağıdaki bölümlerde Azure Synapse Analytics için kullanılabilen farklı kota istekleri açıklanmaktadır.

### <a name="data-warehouse-units-dwus-per-server"></a>Sunucu başına Veri Ambarı Birimleri (DWUs)

Sunucu başına DW'lerde artış istemek için aşağıdaki adımları kullanın.

1. Sunucu kota **türübaşına Veri Ambarı Birimleri 'ni (DTUs)** seçin.

1. **Kaynak** listesinde, hedeflenene kadar kaynağı seçin.

1. İstek **kotası** alanına, istediğiniz yeni DWU sınırını girin.

   ![DWU kota ayrıntıları](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-dwus.png)

### <a name="servers-per-subscription"></a>Abonelik başına sunucular

Abonelik başına sunucu sayısında artış istemek için aşağıdaki adımları kullanın.

1. Abonelik kota türü **başına Sunucular'ı** seçin.

1. **Konum** listesinde, kullanmak üzere Azure bölgesini seçin. Kota her bölgedeki abonelik başına.

1. Yeni **kota** alanında, o bölgedeki en fazla sunucu sayısı için isteğinizi girin.

   ![Sunucular kota ayrıntıları](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-servers.png)

### <a name="enable-subscription-access-to-a-region"></a>Bir bölgeye abonelik erişimini etkinleştirme

Bazı teklif türleri her bölgede kullanılamaz. Aşağıdaki gibi bir hata görebilirsiniz:

`This location is not available for subscription`

Aboneliğinizin belirli bir bölgede erişmesi gerekiyorsa, erişim istemek için lütfen **Diğer kota isteği** seçeneğini kullanın. İsteğiniz, bölge için etkinleştirmek istediğiniz teklifi ve SKU ayrıntılarını belirtin. Teklifi ve SKU seçeneklerini keşfetmek için [Azure Synapse Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/synapse-analytics/)bölümüne bakın.

![Diğer kota ayrıntıları](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-whitelisting.png)

## <a name="submit-your-request"></a>İsteğinizi gönderin

Son adım, SQL Veritabanı destek isteğinizin kalan ayrıntılarını doldurmaktır. Sonra **İleri seçin: Gözden Geçir +>>oluştur **ve istek ayrıntılarını inceledikten sonra, isteği göndermek için **Oluştur'u** tıklatın.

## <a name="monitor-a-support-ticket"></a>Destek biletini izleme

Destek isteğini gönderdikten sonra Azure destek ekibi sizinle iletişim eve gelecektir. İstek durumunuzu ve ayrıntılarını kontrol etmek için panoda bulunan **Tüm destek istekleri** kutucuğuna tıklayın.

![Durumu kontrol etme](./media/sql-data-warehouse-get-started-create-support-ticket/monitor-ticket.png)

## <a name="other-resources"></a>Diğer kaynaklar

[Ayrıca Stack Taşma](https://stackoverflow.com/questions/tagged/azure-synapse+or+azure-sql-data-warehouse) veya [Azure SQL Veri Ambarı MSDN forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/)üzerinden Azure Synapse Analytics topluluğuna da bağlanabilirsiniz.

