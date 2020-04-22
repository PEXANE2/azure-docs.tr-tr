---
title: Contoso göç serisi | Microsoft Dokümanlar
description: Azure'a geçiş için Contoso örnek geçiş senaryolarına bağlantılar.
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: raynew
ms.openlocfilehash: c57a9f85e8b12bd4e1e66a4fcd5d08ab5f7b9118
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81676319"
---
# <a name="contoso-migration-series"></a>Contoso geçiş serisi


Contoso'nun hayali kuruluşunun şirket içi altyapısını [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) bulutuna nasıl geçirttisini gösteren bir dizi makalemiz var. 

Dizi, altyapı geçişinin nasıl ayarlanacağını ve farklı geçiş türlerinin nasıl çalıştırılabildiğini gösteren senaryolar içerir. Senaryolar ilerledikçe karmaşıklık içinde büyürler. Makaleler, Contoso şirketinin geçişi nasıl ele adadığını gösterir, ancak genel talimatlar ve işaretçiler boyunca sağlanır.

## <a name="migration-articles"></a>Geçiş makaleleri

Serideki makaleler aşağıdaki tabloda özetlenmiştir.  

- Her geçiş senaryosu, geçiş stratejisini belirleyen biraz farklı iş gereksinimleri tarafından yönlendirilir.
- Her dağıtım senaryosu için, iş sürücüleri ve hedefleri, önerilen mimari, geçişi gerçekleştirmek için adımlar ve geçiş tamamlandıktan sonra temizleme ve sonraki adımlar hakkında öneriler sağlarız.


**Makale** | **Şey** 
--- | --- 
[Madde 1: Genel Bakış](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | Makale serisine genel bakış, Contoso'nun geçiş stratejisi ve seride kullanılan örnek uygulamalar. 
[Madde 2: Azure altyapılarını dağıtma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso, şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Aynı altyapı serideki tüm makaleler için kullanılır. 
[Madde 3: Azure'a geçiş için şirket içi kaynakları değerlendirin](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-migration-guide/assess?tabs=Tools)  | Contoso, VMware'de çalışan şirket içi SmartHotel360 uygulamasının bir değerlendirmesini yapar. Contoso, Azure Geçiş hizmetini ve Uygulama SQL Server veritabanını Veri Geçişi Yardımcısı'nı kullanarak uygulama VM'lerini değerlendirir.
[Madde 4: Azure VM ve SQL Veritabanı Yönetilen Örneği'ndeki bir uygulamayı yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso, şirket içi SmartHotel360 uygulaması için Azure'a kaldırma ve kaydırma geçişi yürütüyor. Contoso, [Azure Geçiş'i](https://docs.microsoft.com/azure/migrate/migrate-services-overview)kullanarak uygulamanın ön uç VM'sini geçirin. Contoso, uygulama veritabanını [Azure Veritabanı Geçiş Hizmeti'ni](https://docs.microsoft.com/azure/dms/dms-overview)kullanarak Bir Azure SQL Veritabanı Yönetilen Örneği'ne geçirtir.
[Madde 5: Azure VM'lerde bir uygulamayı yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso, SmartHotel360 uygulaması VM'lerini Azure Geçiş hizmetini kullanarak Azure VM'lerine geçirmektedir. 
[Madde 6: Azure VM'lerde ve SQL Server AlwaysOn kullanılabilirlik grubunda bir uygulamayı yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) | Contoso, SmartHotel360 uygulamasını geçişe somaktadır. Contoso, uygulama VM'lerini geçirmek için Azure Geçir'i kullanır. Uygulama veritabanını AlwaysOn kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için Veritabanı Geçiş Hizmeti'ni kullanır. 
[Madde 7: Azure VM'lerde bir Linux uygulamasını yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso, Azure Geçişi'ni kullanarak Linux osTicket uygulamasının Azure VM'lerine kaldırma ve kaydırma geçişini tamamlar.
[Madde 8: MySQL için Azure VM'lerde ve Azure Veritabanı'nda bir Linux uygulamasını yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso, Linux osTicket uygulamasını Azure Geçiş'i kullanarak Azure VM'lerine geçirmektedir. Uygulama veritabanını Azure Veritabanı Geçiş Hizmeti'ni kullanarak MySQ için Azure Veritabanı'na aktarır (MySQL Workbench'i kullanarak alternatif bir seçenek içerir).
[Madde 9: Bir Azure web uygulamasında ve Azure SQL Veritabanında bir uygulamayı yeniden düzenleme](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso, SmartHotel360 uygulamasını bir Azure web uygulamasına geçirip, uygulama veritabanını Azure Veritabanı Geçiş Hizmeti'ni kullanarak Bir Azure SQL Server örneğine geçirmektedir.
[Madde 10: Azure Uygulama Hizmetleri ve SQL Yönetilen Örnek'i kullanarak bir Windows uygulamasını refactor](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql-managed-instance) | Contoso, şirket içi Windows tabanlı bir uygulamayı Azure web uygulamasına geçirtir ve uygulama veritabanını Azure Veritabanı Geçiş Hizmeti'ni kullanarak bir Azure SQL Yönetilen Örneği'ne geçirmektedir.
[Madde 11: Bir Azure web uygulamasında ve MySQL için Azure Veritabanında bir Linux uygulamasını yeniden düzenleme](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql) | Contoso, Linux osTicket uygulamasını sürekli teslimat için GitHub ile entegre edilmiş Azure Trafik Yöneticisi'ni kullanarak birden çok Azure bölgesinde bulunan bir Azure web uygulamasına geçirmektedir. Contoso, uygulama veritabanını MySQL örneği için bir Azure Veritabanı'na geçirtir. 
[Madde 12: Azure DevOps Hizmetlerinde Refactor Team Foundation Server](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso, şirket içi Team Foundation Server dağıtımını Azure'daki Azure DevOps Hizmetleri'ne geçirmektedir.
[Madde 13: Azure'da bir uygulamayı yeniden oluşturma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso, SmartHotel uygulamasını Azure Uygulama Hizmeti, Azure Kubernetes Hizmeti (AKS), Azure Fonksiyonları, Azure Bilişsel Hizmetler ve Azure Cosmos DB gibi çeşitli Azure özellikleri ve hizmetlerini kullanarak yeniden oluşturur.
[Madde 14: Azure'a geçişi ölçeklendirin](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | Geçiş kombinasyonlarını denedikten sonra Contoso, Azure'a tam bir geçiş için ölçeklendirmeye hazırlanıyor.



## <a name="next-steps"></a>Sonraki adımlar

- Bulut geçişi [hakkında bilgi edinin.](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/)
- [Veritabanı Geçiş Kılavuzu'nda](https://datamigration.microsoft.com/)diğer senaryolar (kaynak/hedef çiftleri) için geçiş stratejileri hakkında bilgi edinin.
