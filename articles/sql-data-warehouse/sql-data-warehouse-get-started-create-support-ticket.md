---
title: Destek bileti oluşturma
description: Azure SYNAPSE Analytics 'te destek bileti oluşturma.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 9623a878ea009ec7363501c09c324baa6d0fa7ad
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78195728"
---
# <a name="how-to-create-a-support-ticket-for-azure-synapse-analytics"></a>Azure SYNAPSE Analytics için destek bileti oluşturma
Azure SYNAPSE Analytics ile ilgili herhangi bir sorun yaşıyorsanız, mühendislik desteği ekibinin size yardımcı olabilmesi için bir destek bileti oluşturun.

## <a name="create-a-support-ticket"></a>Destek bileti oluşturma
1. [Azure portalını](https://portal.azure.com/) açın.
1. Giriş ekranında **Yardım + destek** sekmesine tıklayın.
   
    ![Yardım + destek](./media/sql-data-warehouse-get-started-create-support-ticket/main-page.png)

1. [Azure destek planınızı](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/)gözden geçirin.
   
   * Tüm destek düzeylerinde **faturalama, kota ve abonelik yönetimi** desteği sunulmaktadır.
   * **Onarım** desteği [Geliştirici](https://azure.microsoft.com/support/plans/developer/), [Standart](https://azure.microsoft.com/support/plans/standard/), [profesyonel doğrudan](https://azure.microsoft.com/support/plans/prodirect/)veya [Premier](https://azure.microsoft.com/support/plans/premier/) destek aracılığıyla sağlanır. Onarım sorunları, müşterilerin Azure'ı kullandığı sırada karşılaştıkları ve ilgili sorunun Microsoft'tan kaynaklandığına ilişkin makul bir olasılığın bulunduğu sorunlardır.
   * **Geliştirici rehberliği** ve **danışmanlık hizmetleri**, [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) ve [Premier](https://azure.microsoft.com/support/plans/premier/) destek düzeylerinde kullanılabilir. 
     
     Bir Premier destek planınız varsa SQL Data Warehouse ile ilgili sorunları [Microsoft Premier çevrimiçi portalı](https://premier.microsoft.com/) üzerinden de bildirebilirsiniz. Kapsam, yanıt süreleri, fiyatlandırma vb. dahil olmak üzere çeşitli destek planları hakkında daha fazla bilgi edinmek için bkz. [Azure destek planları](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/) .  Azure desteği hakkında sık sorulan sorular için bkz. [Azure desteği SSS](https://azure.microsoft.com/support/faq/).
1. **Yardım + Destek** sayfasında **Yeni destek isteği**' ni seçin. Açılan menüden bir sorun türü seçin. Ardından **temel** bilgiler sekmesindeki bilgileri doldurmaya devam edin. sorununuzun bir **özetini** girin ve menüden bir **sorun türü** seçip Kaydet ' i seçin.

    ![Yardım + destek](./media/sql-data-warehouse-get-started-create-support-ticket/issue-type.png)

   > [!NOTE]
   > Varsayılan olarak her SQL sunucusu (örn. myserver.database.windows.net) 45.000’lik **DTU Kotası**’na sahiptir. Bu kota yalnızca bir güvenlik sınırıdır. Bir destek bileti oluşturarak ve istek türü olarak *Kota*’yı seçerek kotanızı artırabilirsiniz. DTU gereksinimlerinizi hesaplamak için gereken toplam [DWU](sql-data-warehouse-overview-what-is.md) ile 7,5 çarpımını artırın. Örneğin, tek bir SQL sunucusu üzerinde iki DW6000 barındırmak istiyorsanız, 90.000’lik DTU kotası istemeniz gerekir.  Geçerli DTU tüketiminizi portaldaki SQL server dikey penceresinden görüntüleyebilirsiniz. DTU kotasında hem duraklatılmış hem de duraklatılmamış veritabanları sayılır. 
   > 

1. Sorununuzu gidermeye yardımcı olabilecek çözümler görebilirsiniz. Sunulan çözümler sorununuzu gidermezse, Ileri ' yi seçin **: Ayrıntılar**. Sorununuzla ilgili ayrıntıları ve iletişim bilgilerinizi gönderin. **İleri ' yi seçin: İnceleme + oluşturma**
![ayrıntıları](./media/sql-data-warehouse-get-started-create-support-ticket/details.png)

    
1. Destek isteğini göndermek için bilgilerinizi gözden geçirin ve formun alt kısmındaki **Oluştur** ' u seçin.

## <a name="monitor-a-support-ticket"></a>Destek biletini izleme
Destek isteğini gönderdikten sonra, Azure destek ekibi sizinle iletişim kuracaktır. İstek durumunuzu ve ayrıntılarını kontrol etmek için panoda bulunan **Tüm destek istekleri** kutucuğuna tıklayın.

![Durumu kontrol etme](./media/sql-data-warehouse-get-started-create-support-ticket/monitor-ticket.png)

## <a name="other-resources"></a>Diğer kaynaklar
Ayrıca, [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw/) veya [Azure SQL veri ambarı MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/)üzerinden SQL veri ambarı topluluğuyla da bağlanabilirsiniz.

 
