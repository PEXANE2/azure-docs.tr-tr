---
title: Contoso göç serisi | Microsoft Dokümanlar
description: Contoso'nun şirket içi veri merkezlerini Azure'a geçirmek için kullandığı geçiş stratejisi ve senaryolarına genel bir bakış sağlar.
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: raynew
ms.openlocfilehash: d37bee589eb7ee2e6e30c8dcea2531dd1f063481
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78939194"
---
# <a name="contoso-migration-series"></a>Contoso geçiş serisi


Contoso'nun hayali kuruluşunun şirket içi altyapıyı [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) bulutuna nasıl geçirtdiğini gösteren bir dizi makalemiz var. 

Seri, altyapı geçişinin nasıl ayarlanacağını ve farklı geçiş türlerini çalıştırmayı gösteren bilgiler ve senaryolar içerir. Senaryolar ilerledikçe karmaşıklık içinde büyürler. Makaleler, Contoso şirketinin göç görevini nasıl tamamlar, ancak genel okuma ve özel talimatlar için işaretçilerin tüm bunlar boyunca sağlandığını göstermektedir.

## <a name="migration-articles"></a>Geçiş makaleleri

Serideki makaleler aşağıdaki tabloda özetlenmiştir.  

- Her bir geçiş senaryosu, geçiş stratejisini belirleyen biraz farklı iş hedefleri tarafından desteklenir.
- Her bir dağıtım senaryosu için, iş açısından teşvik eden noktalar ve hedefler hakkında bilgiler, önerilen bir mimari, geçişi gerçekleştirme adımları ve geçiş tamamlandıktan sonraki adımlar ve temizleme önerisi sağlarız.

**Makale** | **Şey** 
--- | --- 
[Madde 1: Genel Bakış](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | Makale serisine genel bakış, Contoso'nun geçiş stratejisi ve seride kullanılan örnek uygulamalar. 
[Madde 2: Azure altyapılarını dağıtma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso, şirket içi altyapısını ve Azure altyapısını geçişiçin hazırlar. Aynı altyapı, serideki tüm geçiş makaleleri için kullanılır. 
[Madde 3: Azure'a geçiş için şirket içi kaynakları değerlendirin](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-migration-guide/assess?tabs=Tools)  | Contoso, VMware'de çalışan şirket içi SmartHotel360 uygulamasının bir değerlendirmesini yapar. Contoso, Azure Geçiş hizmetini ve Uygulama SQL Server veritabanını Veri Geçişi Yardımcısı'nı kullanarak uygulama VM'lerini değerlendirir.
[Madde 4: Azure VM ve SQL Veritabanı Yönetilen Örneği'ndeki bir uygulamayı yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso, şirket içi SmartHotel360 uygulaması için Azure'a kaldırma ve kaydırma geçişi yürütüyor. Contoso, [Azure Site Kurtarma'yı](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)kullanarak uygulamanın ön uç VM'sini geçirin. Contoso, uygulama veritabanını [Azure Veritabanı Geçiş Hizmeti'ni](https://docs.microsoft.com/azure/dms/dms-overview)kullanarak Bir Azure SQL Veritabanı Yönetilen Örneği'ne geçirtir.
[Madde 5: Azure VM'lerde bir uygulamayı yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso, SmartHotel360 uygulaması VM'lerini Site Kurtarma hizmetini kullanarak Azure VM'lerine geçirmektedir. 
[Madde 6: Azure VM'lerde ve SQL Server AlwaysOn kullanılabilirlik grubunda bir uygulamayı yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) |Contoso, SmartHotel360 uygulamasını geçişe somaktadır. Contoso, uygulama VM'lerini geçirmek için Site Kurtarma'yı kullanır. Uygulama veritabanını AlwaysOn kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için Veritabanı Geçiş Hizmeti'ni kullanır. 
[Madde 7: Azure VM'lerde bir Linux uygulamasını yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso, Site Kurtarma hizmetini kullanarak Linux osTicket uygulamasının Azure VM'lerine geçişini tamamlar.
[Madde 8: MySQL için Azure VM'lerde ve Azure Veritabanı'nda bir Linux uygulamasını yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso, Linux osTicket uygulamasını Site Kurtarma'yı kullanarak Azure VM'lerine taşıyor. Bu, MySQL Workbench kullanarak uygulama veritabanını MySQL için Azure Veritabanı’na geçirir. 
[Madde 9: Bir Azure web uygulamasında ve Azure SQL Veritabanında bir uygulamayı yeniden düzenleme](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso, SmartHotel360 uygulamasını bir Azure web uygulamasına geçirip uygulama veritabanını Veritabanı Geçiş Yardımcısı ile birlikte Azure SQL Server örneğine geçirmektedir.     
[Madde 10: Bir Azure web uygulamasında ve MySQL için Azure Veritabanında bir Linux uygulamasını yeniden düzenleme](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql) | Contoso, Linux osTicket uygulamasını sürekli teslimat için GitHub ile entegre edilmiş Azure Trafik Yöneticisi'ni kullanarak birden çok Azure bölgesinde azure web uygulamasına geçirmektedir. Contoso, uygulama veritabanını MySQL örneği için bir Azure Veritabanı'na geçirtir. 
[Madde 11: Azure DevOps Hizmetlerinde Refactor Team Foundation Server](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso, şirket içi Team Foundation Server dağıtımını Azure'daki Azure DevOps Hizmetleri'ne geçirmektedir.
[Madde 12: Azure kapsayıcılarında ve Azure SQL Veritabanı'nda bir uygulamayı yeniden tasarlayın](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rearchitect-container-sql) | Contoso, SmartHotel uygulamasını Azure'a geçirmektedir. Ardından, uygulama web katmanını Azure Hizmet Dokusu'nda çalışan bir Windows kapsayıcısı ve Azure SQL Veritabanı ile veritabanı olarak yeniden yeniden yeniden bir hale yükler.
[Madde 13: Azure'da bir uygulamayı yeniden oluşturma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso, Azure Uygulama Hizmeti, Azure Kubernetes Hizmeti (AKS), Azure Fonksiyonları, Azure Bilişsel Hizmetler ve Azure Cosmos DB gibi çeşitli Azure özellikleri ve hizmetleri kullanarak SmartHotel uygulamasını yeniden oluşturur.
[Madde 14: Azure'a geçişi ölçeklendirin](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | Geçiş kombinasyonlarını denedikten sonra Contoso, Azure'a tam bir geçiş için ölçeklendirmeye hazırlanıyor.

## <a name="next-steps"></a>Sonraki adımlar

- Bulut geçişi [hakkında bilgi edinin.](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/)
- [Veritabanı Geçiş Kılavuzu'nda](https://datamigration.microsoft.com/)diğer senaryolar (kaynak/hedef çiftleri) için geçiş stratejileri hakkında bilgi edinin.
