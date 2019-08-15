---
title: Azure Güvenlik Merkezi 'nde Saydam Veri Şifrelemesi etkinleştirme | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi önerisi **Saydam veri şifrelemesi etkinleştirme**' nin nasıl uygulanacağını gösterir.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: d5ddec40a1b20e377ec18ce871018f674557e7b4
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "60704041"
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde Saydam Veri Şifrelemesi etkinleştirme
Azure Güvenlik Merkezi, TDE zaten etkinleştirilmemişse SQL veritabanlarında Saydam Veri Şifrelemesi (TDE) etkinleştirmenizi önerir. TDE verilerinizi korur ve uygulamanızda değişiklik yapmanıza gerek kalmadan veritabanınızı, ilişkili yedeklerinizi ve bekleyen işlem günlüğü dosyalarını şifreleyerek uyumluluk gereksinimlerini karşılamanıza yardımcı olur. Daha fazla bilgi edinmek için bkz. [Azure SQL veritabanı saydam veri şifrelemesi](https://msdn.microsoft.com/library/dn948096).

Bu öneri yalnızca Azure SQL hizmeti için geçerlidir; , sanal makinelerinizde çalışan SQL içermez.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulayın
1. **Öneriler** dikey penceresinde **Saydam veri şifrelemesi etkinleştir**' i seçin.
   ![Saydam Veri Şifrelemesini Etkinleştirme][1]
2. Bu, **SQL veritabanlarında saydam veri şifrelemesi etkinleştir** dikey penceresinde açılır. TDE ' yi etkinleştirmek için bir SQL veritabanı seçin.
   ![TDE 'nın etkin olması için SQL DB 'yi seçin][2]
3. **Saydam veri şifrelemesi** dikey penceresinde veri şifreleme altında **Açık** ' ı seçin ve dikey pencerenin üst şeridinde **Kaydet** ' i seçin.
   ![TDE açma][3]

   Seçili SQL veritabanında TDE etkinleştirildikten sonra **şifreleme durumu** **şifreli**olarak değişir.    

   ![Şifreleme durumu][4]

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, "etkinleştirme Saydam Veri Şifrelemesi" Güvenlik Merkezi önerisi nasıl uygulanır. SQL TDE hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Azure SQL veritabanı ile Saydam Veri Şifrelemesi](https://msdn.microsoft.com/library/dn948096)
* [Saydam Veri Şifrelemesi ile çalışmaya başlama (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --en son Azure Güvenlik haberlerini ve bilgilerini alın.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
