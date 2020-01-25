---
title: Destek bileti oluşturma
description: Azure SQL Data Warehouse'da destek bileti oluşturma
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 04fd6267782ab89e12288dadb64d28d1ef1746b5
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76717830"
---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>SQL Data Warehouse için destek bileti oluşturma
SQL veri Ambarınızla ilgili herhangi bir sorun yaşıyorsanız, mühendislik desteği ekibinin size yardımcı olabilmesi için bir destek bileti oluşturun.

## <a name="create-a-support-ticket"></a>Destek bileti oluşturma
1. [Azure portalı](https://portal.azure.com/) açın.
2. Giriş ekranında **Yardım + destek** sekmesine tıklayın.
   
    ![Yardım + destek](./media/sql-data-warehouse-get-started-create-support-ticket/MainPage.PNG)
3. Yardım + Destek dikey penceresinde **Yeni destek isteği**’ne tıklayın ve **Temel Bilgiler** dikey penceresini doldurun.

   [Azure destek planınızı](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/) seçin.
   
   * Tüm destek düzeylerinde **faturalama, kota ve abonelik yönetimi** desteği sunulmaktadır.
   * **Onarım** desteği [Geliştirici](https://azure.microsoft.com/support/plans/developer/), [Standart](https://azure.microsoft.com/support/plans/standard/), [profesyonel doğrudan](https://azure.microsoft.com/support/plans/prodirect/)veya [Premier](https://azure.microsoft.com/support/plans/premier/) destek aracılığıyla sağlanır. Onarım sorunları, Azure kullanılırken müşteriler tarafından karşılaşılan, sorunun bir sebepten Microsoft'tan kaynaklandığının beklenildiği sorunlardır.
   * **Geliştirici rehberliği** ve **danışmanlık hizmetleri**, [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) ve [Premier](https://azure.microsoft.com/support/plans/premier/) destek düzeylerinde kullanılabilir. 
     
     Bir Premier destek planınız varsa SQL Data Warehouse ile ilgili sorunları [Microsoft Premier çevrimiçi portalı](https://premier.microsoft.com/) üzerinden de bildirebilirsiniz. Kapsam, yanıt süreleri, fiyatlandırma vb. dahil olmak üzere çeşitli destek planları hakkında daha fazla bilgi edinmek için bkz. [Azure destek planları](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/) .  Azure desteği hakkında sık sorulan sorular için bkz. [Azure desteği SSS](https://azure.microsoft.com/support/faq/).  
        
     ![Temel bilgiler dikey penceresi](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_1.PNG)
     ![Temel bilgiler dikey penceresi1](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_2.PNG)
4. **Sorun** dikey penceresini doldurun.

    ![Problem_blade](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_3.PNG)
   
   > [!NOTE]
   > Varsayılan olarak her SQL sunucusu (örn. myserver.database.windows.net) 45.000’lik **DTU Kotası**’na sahiptir. Bu kota yalnızca bir güvenlik sınırıdır. Bir destek bileti oluşturarak ve istek türü olarak *Kota* ’ yı seçerek kotanızı artırabilirsiniz. DTU gereksinimlerinizi hesaplamak için gereken toplam [DWU](sql-data-warehouse-overview-what-is.md) ile 7,5 çarpımını artırın. Örneğin, tek bir SQL sunucusu üzerinde iki DW6000 barındırmak istiyorsanız, 90.000’lik DTU kotası istemeniz gerekir.  Geçerli DTU tüketiminizi portaldaki SQL server dikey penceresinden görüntüleyebilirsiniz. DTU kotasında hem duraklatılmış hem de duraklatılmamış veritabanları sayılır. 
   > 
   > 
   
5. **İletişim bilgilerinizi** girin.

   ![Contact_information](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_4.PNG)

    
6. Destek isteğini göndermek için **Oluştur**'a tıklayın.

## <a name="monitor-a-support-ticket"></a>Destek biletini izleme
Destek isteğini gönderdikten sonra, Azure destek ekibi sizinle iletişim kuracaktır. İstek durumunuzu ve ayrıntılarını kontrol etmek için panoda bulunan **Tüm destek istekleri** kutucuğuna tıklayın.

![Durumu kontrol etme](./media/sql-data-warehouse-get-started-create-support-ticket/Monitor_ticket.PNG)

## <a name="other-resources"></a>Diğer kaynaklar
Ayrıca, [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw/) veya [Azure SQL veri ambarı MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/)üzerinden SQL veri ambarı topluluğuyla da bağlanabilirsiniz.

 
