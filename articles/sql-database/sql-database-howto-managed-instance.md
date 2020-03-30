---
title: Yönetilen bir örneği yapılandırma
description: Azure SQL Veritabanı yönetilen örneğini nasıl yapılandırıp yöneteceklerinizi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlr
ms.date: 04/16/2019
ms.openlocfilehash: 906ae2a970ce1d5b82302d0277ca45bd93c23011
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256919"
---
# <a name="how-to-use-a-managed-instance-in-azure-sql-database"></a>Azure SQL Veritabanı'nda yönetilen bir örnek nasıl kullanılır?

Bu makalede, yönetilen örneğini yönetmenize ve yapılandırmanıza yardımcı olabilecek çeşitli kılavuzlar, komut dosyaları ve açıklamalar bulabilirsiniz.

## <a name="migration"></a>Geçiş

- [Yönetilen bir örne geçir –](sql-database-managed-instance-migrate.md) Yönetilen bir örne geçiş için önerilen geçiş işlemi ve araçlar hakkında bilgi edinin.

- [TDE sertifikasını yönetilen bir örneğe geçir](sql-database-managed-instance-migrate-tde-certificate.md) – SQL Server veritabanınız saydam veri şifreleme (TDE) ile korunuyorsa, yönetilen bir örneğin Azure'da geri yüklemek istediğiniz yedeklemenin şifresini çözmek için kullanabileceği sertifikayı geçirmeniz gerekir.

## <a name="network-configuration"></a>Ağ yapılandırması

- [Yönetilen örnek alt ağının boyutunu belirleyin](sql-database-managed-instance-determine-size-vnet-subnet.md) – Yönetilen örnek, içindeki kaynakları ekledikten sonra yeniden boyutlandırılamayan adanmışlar alt ağına yerleştirilir. Bu nedenle, alt ağda dağıtmak istediğiniz örnek sayısına ve türlerine bağlı olarak alt ağ için hangi IP adreslerinin gerekli olacağını hesaplamanız gerekir.
- [Yönetilen bir örnek için yeni VNet ve alt ağ oluşturun](sql-database-managed-instance-create-vnet-subnet.md) – Yönetilen örneklerinizi dağıtmak istediğiniz Azure VNet ve alt ağ burada açıklanan ağ [gereksinimlerine](sql-database-managed-instance-connectivity-architecture.md#network-requirements)göre yapılandırılmalıdır. Bu kılavuzda, yönetilen örnekler için düzgün yapılandırılmış yeni VNet ve alt ağınızı oluşturmanın en kolay yolunu bulabilirsiniz.
- [Yönetilen bir örnek için varolan VNet'i ve alt ağı nızı yapılandırın](sql-database-managed-instance-configure-vnet-subnet.md) – mevcut VNet ve alt ağınızı yönetilen örnekleri içeride dağıtmak üzere yapılandırmak istiyorsanız, [burada ağ gereksinimlerini](sql-database-managed-instance-connectivity-architecture.md#network-requirements) denetleyen ve alt ağınızı gereksinimlere göre yapılandıran komut dosyasını bulabilirsiniz.
- [Özel DNS'yi yapılandırın](sql-database-managed-instance-custom-dns.md) – db posta profillerinin bağlantılı sunucusu aracılığıyla yönetilen örneğinizden özel etki alanlarındaki dış kaynaklara erişmek istiyorsanız özel DNS'yi yapılandırmanız gerekir.
- [Eşitleme ağı yapılandırması](sql-database-managed-instance-sync-network-configuration.md) - [Uygulamanızı bir Azure Sanal Ağıyla tümleştirseniz](../app-service/web-sites-integrate-with-vnet.md)de yönetilen bir örne bağlantı kuramadığınız&#39;olabilir. Deneyebileceğiniz tek şey, hizmet planınız için ağ yapılandırmasını yenilemektir.
- [Yönetim bitiş noktası IP adresini bul](sql-database-managed-instance-find-management-endpoint-ip-address.md) – Yönetilen örnek, yönetim amaçları için ortak bitiş noktasını kullanır. Burada açıklanan komut dosyasını kullanarak yönetim bitiş noktasının IP adresini belirleyebilirsiniz.
- [Yerleşik güvenlik duvarı korumasını doğrulayın](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md) – Yönetilen örnek, yalnızca gerekli bağlantı noktalarında ki trafiğe izin veren yerleşik güvenlik duvarıyla korunur. Bu kılavuzda açıklanan komut dosyasını kullanarak yerleşik güvenlik duvarı kurallarını denetleyebilir ve doğrulayabilirsiniz.
- [Uygulamaları bağlayın](sql-database-managed-instance-connect-app.md) - Yönetilen örnek, özel IP adresiyle kendi özel Azure VNet'inize yerleştirilir. Uygulamaları yönetilen örneğinize bağlamak için farklı desenler hakkında bilgi edinin.

## <a name="feature-configuration"></a>Özellik yapılandırması

- [İşlemsel çoğaltma,](replication-with-sql-database-managed-instance.md) verilerinizi yönetilen örnekler arasında veya şirket içi SQL Server'dan yönetilen bir örne veya tam tersi arasında çoğaltmanızı sağlar. Bu kılavuzda işlem çoğaltmanın nasıl kullanılacağı ve yapılandırılacağı hakkında daha fazla bilgi edinin.
- [Tehdit algılamayı yapılandırma](sql-database-managed-instance-threat-detection.md) – [tehdit algılama,](sql-database-threat-detection-overview.md) SQL Enjeksiyonu veya şüpheli konumlardan erişim gibi çeşitli olası saldırıları algılayan yerleşik bir Azure SQL Veritabanı özelliğidir. Bu kılavuzda, yönetilen bir örnek için [tehdit algılamayı](sql-database-threat-detection-overview.md) nasıl etkinleştirebileceğinizi ve yapılandırabileceğinizi öğrenebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Tek veritabanları için Nasıl YapIlenkılavuzları](sql-database-howto-single-database.md) hakkında daha fazla bilgi edinin