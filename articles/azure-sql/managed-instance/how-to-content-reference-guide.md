---
title: İçerik başvurusunu yapılandırma & Yönet
titleSuffix: Azure SQL Managed Instance
description: Azure SQL yönetilen örneğinizi yapılandırmayı ve yönetmeyi öğretir bir içerik Kılavuzu.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlr
ms.date: 04/16/2019
ms.openlocfilehash: bc0c30e234310a4ee1d013d7a11ca556edd071c6
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84041119"
---
# <a name="azure-sql-managed-instance-content-reference"></a>Azure SQL yönetilen örnek içerik başvurusu
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, Azure SQL yönetilen örneğinizi yönetmenize ve yapılandırmanıza yardımcı olan çeşitli kılavuzlar, betikler ve açıklamalar için bir İçerik başvurusu bulabilirsiniz.

## <a name="load-data"></a>Veri yükleme

- [Azure SQL yönetilen örneğine](migrate-to-instance-from-sql-server.md) geçiş – Azure SQL yönetilen örneği 'ne geçiş için önerilen geçiş işlemi ve araçları hakkında bilgi edinin.
- [TDE CERT 'Yi Azure SQL yönetilen örneği 'Ne geçirme](tde-certificate-migrate.md) – SQL Server veritabanınız saydam veri şifrelemesi (tde) ile korunuyorsa, bir SQL yönetilen örneğinin Azure 'da geri yüklemek istediğiniz yedeğin şifresini çözmek için kullanabileceği sertifikayı geçirmeniz gerekir.
- [BACPAC’ten veritabanını içeri aktarma](../database/database-import.md)
- [Veritabanını BACPAC’e dışarı aktarma](../database/database-export.md)
- [BCP ile veri yükleme](../load-from-csv-with-bcp.md)
- [ADF ile veri yükleme](../../data-factory/connector-azure-sql-database.md?toc=/azure/sql-database/toc.json)

## <a name="network-configuration"></a>Ağ yapılandırması

- [Alt ağ boyutunu belirle](vnet-subnet-determine-size.md) SQL yönetilen örneği dağıtıldıktan sonra alt ağ yeniden boyutlandırılamadığı için, alt ağa dağıtmayı planladığınız SQL yönetilen örnek sayısı ve türleri için hangi IP adresi aralığının gerektiğini hesaplamanız gerekir. 
- [Yeni VNET ve alt ağ oluştur](virtual-network-subnet-create-arm-template.md) Sanal ağı ve alt ağı [burada açıklanan ağ gereksinimlerine](connectivity-architecture-overview.md#network-requirements) göre yapılandırın 
- [Mevcut VNET ve alt ağı yapılandırma](vnet-existing-add-subnet.md) SQL yönetilen örneğini dağıtmak için ağ gereksinimlerini doğrulayın ve var olan sanal ağınızı ve alt ağınızı yapılandırın. 
- [Özel DNS yapılandırma](custom-dns-configure.md) Özel DNS 'yi, SQL yönetilen örneğinizden gelen ve bir DB posta profillerinin bağlı sunucusu aracılığıyla özel etki alanlarına erişim izni verecek şekilde yapılandırın. 
- [Ağ yapılandırmasını Eşitle](azure-app-sync-network-configuration.md) [Uygulamanızı bir Azure sanal ağı ile tümleştirdikten](../../app-service/web-sites-integrate-with-vnet.md) sonra bağlantı kuramazsa ağ yapılandırma planını yenileyin
- [Yönetim uç noktası IP adresini bul](management-endpoint-find-ip-address.md) SQL yönetilen örneğinin yönetim amacıyla kullandığı genel uç noktayı belirleme. 
- [Yerleşik güvenlik duvarı korumasını doğrulama](management-endpoint-verify-built-in-firewall.md) SQL yönetilen örneğinin yalnızca gerekli bağlantı noktalarında trafiğe izin verdiğini ve diğer yerleşik güvenlik duvarı kurallarını doğrulayın. 
- [Uygulamaları bağlama](connect-application-instance.md) Uygulamaları SQL yönetilen örneğinize bağlamak için farklı desenler hakkında bilgi edinin.

## <a name="feature-configuration"></a>Özellik yapılandırması

- [Azure AD kimlik doğrulamasını yapılandırma](../database/authentication-aad-configure.md)
- [Koşullu erişimi yapılandırma](../database/conditional-access-configure.md)
- [Çok faktörlü AAD kimlik doğrulaması](../database/authentication-mfa-ssms-overview.md)
- [Çok faktörlü kimlik doğrulamasını yapılandırma](../database/authentication-mfa-ssms-configure.md)
- [Zamana bağlı saklama ilkesini yapılandırma](../database/temporal-tables-retention-policy.md)
- [KAG ile TDE Yapılandırması](../database/transparent-data-encryption-byok-configure.md)
- [TDE KAG anahtarlarını döndürme](../database/transparent-data-encryption-byok-key-rotation.md)
- [TDE koruyucusunu kaldırma](../database/transparent-data-encryption-byok-remove-tde-protector.md)
- [Bellek içi OLTP’yi yapılandırma](../in-memory-oltp-configure.md)
- [Azure Otomasyonu 'Nu yapılandırma](../database/automation-manage.md)
- [İşlemsel çoğaltma](replication-between-two-instances-configure-tutorial.md) , VERILERINIZI Azure SQL yönetilen örnekleri arasında veya şirket içi SQL Server bir SQL yönetilen örneğine çoğaltmanıza olanak sağlar ve tam tersi de geçerlidir.
- [Tehdit algılamayı yapılandırma](threat-detection-configure.md) – [TEHDIT algılama](../database/threat-detection-overview.md) , SQL ekleme veya şüpheli konumlardan erişim gibi çeşitli olası saldırıları ALGıLAYAN yerleşik bir Azure SQL yönetilen örnek özelliğidir. 
- [Uyarı oluşturma](alerts-create.md) , SQL yönetilen ÖRNEĞI için CPU kullanımı, depolama alanı TÜKETIMI, IOPS ve diğerleri gibi izlenen ölçümler üzerinde uyarı ayarlamanıza olanak sağlar. 

## <a name="monitoring-and-tuning"></a>İzleme ve ayarlama

- [El ile ayarlama](../database/performance-guidance.md)
- [Performansı izlemek için DMV’leri kullanma](../database/monitoring-with-dmvs.md)
- [Performansı izlemek için Sorgu deposunu kullanma](https://docs.microsoft.com/sql/relational-databases/performance/best-practice-with-the-query-store#Insight)
- [Akıllı İçgörüler ile performans sorunlarını giderme](../database/intelligent-insights-troubleshoot-performance.md)
- [Akıllı İçgörüler tanılama günlüğünü kullanma](../database/intelligent-insights-use-diagnostics-log.md)
- [Bellek İçi OLTP alanını izleme](../in-memory-oltp-monitor-space.md)

### <a name="extended-events"></a>Genişletilmiş olaylar

- [Genişletilmiş olaylar](../database/xevent-db-diff-from-svr.md)
- [Genişletilmiş olayları olay dosyasına depola](../database/xevent-code-event-file.md)
- [Genişletilmiş olayları halka arabelleğine depola](../database/xevent-code-ring-buffer.md)

## <a name="develop-applications"></a>Uygulama geliştirme

- [Bağlantı](../database/connect-query-content-reference-guide.md#libraries)
- [Spark Bağlayıcısı kullanma](../../cosmos-db/spark-connector.md)
- [Uygulamanın kimliğini doğrulama](../database/application-authentication-get-client-id-keys.md)
- [Daha iyi performans için toplu işlem kullanma](../performance-improve-use-batching.md)
- [Bağlantı kılavuzu](../database/troubleshoot-common-connectivity-issues.md)
- [DNS diğer adları](../database/dns-alias-overview.md)
- [DNS diğer ad PowerShell 'i ayarla](../database/dns-alias-powershell-create.md)
- [Bağlantı noktaları - ADO.NET](../database/adonet-v12-develop-direct-route-ports.md)
- [C ve C++](../database/develop-cplusplus-simple.md)
- [Excel](../database/connect-excel.md)

## <a name="design-applications"></a>Uygulamaları tasarlama

- [Olağanüstü durum kurtarmaya yönelik tasarım](../database/designing-cloud-solutions-for-disaster-recovery.md)
- [Elastik havuzlara yönelik tasarım](../database/disaster-recovery-strategies-for-applications-with-elastic-pool.md)
- [Uygulama yükseltmelerine yönelik tasarım](../database/manage-application-rolling-upgrade.md)

### <a name="design-multi-tenant-saas-applications"></a>Çok kiracılı SaaS uygulamaları tasarlama

- [SaaS tasarım desenleri](../database/saas-tenancy-app-design-patterns.md)
- [SaaS video dizinleyicisi](../database/saas-tenancy-video-index-wingtip-brk3120-20171011.md)
- [SaaS uygulama güvenliği](../database/saas-tenancy-elastic-tools-multi-tenant-row-level-security.md)



## <a name="next-steps"></a>Sonraki adımlar

[SQL yönetilen örneğinizi dağıtarak](instance-create-quickstart.md) çalışmaya başlayın
