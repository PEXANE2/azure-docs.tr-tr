---
title: Contoso geçiş Serisi | Microsoft Docs
description: Azure 'a geçiş için Contoso örnek geçiş senaryolarına bağlantılar.
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: raynew
ms.openlocfilehash: 2cc03462fd72db1a50434d3d6de0851660414cd9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84195844"
---
# <a name="contoso-migration-series"></a>Contoso geçiş serisi


Kurgusal kuruluşun, şirket içi altyapısını [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) buluta nasıl geçirdiğini gösteren bir dizi makalemiz vardır. 

Seriler, bir altyapı geçişi ayarlamayı ve farklı geçişlerin türlerini çalıştırmayı gösteren senaryolar içerir. Senaryolar, ilerleyen karmaşıklıklarla artar. Makaleler, contoso şirketinin geçişi nasıl işleyeceğini gösterir, ancak genelinde genel yönergeler ve işaretçiler sağlanır.

## <a name="migration-articles"></a>Geçiş makaleleri

Serideki makaleler aşağıdaki tabloda özetlenmiştir.  

- Her geçiş senaryosu, geçiş stratejisini belirten biraz farklı iş gereksinimleriyle çalıştırılır.
- Her dağıtım senaryosunda, iş sürücüleri ve hedefleri, önerilen bir mimari, geçişi gerçekleştirmeye yönelik adımlar ve geçiş tamamlandıktan sonra temizlik ve sonraki adımlar için öneriler sağlıyoruz.


**Makalenin** | **Ayrıntılar** 
--- | --- 
[Makale 1: genel bakış](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | Makale serisine, contoso 'nun geçiş stratejisine ve serideki kullanılan örnek uygulamalara genel bakış. 
[Makale 2: Azure altyapısını dağıtma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso, şirket içi altyapısını ve geçiş için Azure altyapısını hazırlar. Serideki tüm makaleler için aynı altyapı kullanılır. 
[3. Makale: Azure 'a geçiş için şirket içi kaynakları değerlendirme](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-migration-guide/assess?tabs=Tools)  | Contoso, VMware üzerinde çalışan şirket içi SmartHotel360 uygulamasının bir değerlendirmesini çalıştırır. Contoso değerlendirir uygulama VM 'Leri, Azure geçişi hizmetini ve uygulama SQL Server veritabanını kullanarak Data Migration Yardımcısı.
[Makale 4: Azure VM ve SQL yönetilen örneği üzerinde uygulamayı yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso, şirket içi SmartHotel360 uygulaması için Azure 'a bir yükseltme ve kaydırma geçişi çalıştırır. Contoso, [Azure geçişi](https://docs.microsoft.com/azure/migrate/migrate-services-overview)'ni kullanarak uygulama ön uç VM 'sini geçirir. Contoso, [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview)'ni kullanarak uygulama VERITABANıNı bir SQL yönetilen örneğine geçirir.
[Makale 5: Azure VM 'lerinde uygulamayı yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso, SmartHotel360 App VM 'lerini Azure geçişi hizmetini kullanarak Azure VM 'lerine geçirir. 
[Makale 6: Azure VM 'lerde ve bir SQL Server AlwaysOn kullanılabilirlik grubunda uygulamayı yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) | Contoso, SmartHotel360 uygulamasını geçirir. Contoso, uygulama VM 'lerini geçirmek için Azure geçişi kullanır. Uygulama veritabanını bir AlwaysOn kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için veritabanı geçiş hizmetini kullanır. 
[Makale 7: Azure VM 'lerinde bir Linux uygulamasını yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso, Linux Osbilet uygulamasının Azure sanal makinelerine, Azure geçişi 'ni kullanarak bir kaldırma ve kaydırma geçişi tamamlar.
[Makale 8: Azure VM 'lerinde Linux uygulamasını yeniden barındırma ve MySQL için Azure veritabanı](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso, Linux Osbilet uygulamasını Azure geçişi kullanarak Azure VM 'lerine geçirir. Azure veritabanı geçiş hizmeti 'ni kullanarak uygulama veritabanını MySQ için Azure veritabanı 'na geçirir (MySQL çalışma ekranı kullanılarak alternatif bir seçenek içerir).
[Makale 9: bir Azure Web uygulaması ve Azure SQL veritabanı 'nda uygulamayı yeniden düzenleme](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso, SmartHotel360 uygulamasını bir Azure Web uygulamasına geçirir ve Azure veritabanı geçiş hizmeti 'ni kullanarak uygulama veritabanını Azure SQL veritabanı 'na geçirir.
[Makale 10: Azure uygulama hizmetleri ve SQL yönetilen örneği kullanarak bir Windows uygulamasını yeniden düzenleme](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql-managed-instance) | Contoso, şirket içi Windows tabanlı bir uygulamayı bir Azure Web uygulamasına geçirir ve Azure veritabanı geçiş hizmeti 'ni kullanarak uygulama veritabanını bir Azure SQL yönetilen örneğine geçirir.
[Makale 11: bir Azure Web uygulamasındaki Linux uygulamasını ve MySQL için Azure veritabanı 'nı yeniden düzenleme](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql) | Contoso, Azure Traffic Manager kullanarak Linux Osbilet uygulamasını birden fazla Azure bölgesinde bir Azure Web uygulamasına geçirir ve sürekli teslim için GitHub ile tümleşiktir. Contoso, uygulama veritabanını bir MySQL için Azure veritabanı örneğine geçirir. 
[Makale 12: Azure DevOps Services Team Foundation Server yeniden düzenleme](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso, şirket içi Team Foundation Server dağıtımını Azure 'daki Azure DevOps Services geçirir.
[Makale 13: Azure 'da bir uygulamayı yeniden oluşturma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso, Azure App Service, Azure Kubernetes hizmeti (AKS), Azure Işlevleri, Azure bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere bir dizi Azure özelliği ve hizmeti kullanarak SmartHotel uygulamasını yeniden oluşturur.
[Makale 14: Azure 'a geçişi ölçeklendirme](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | Contoso, geçiş kombinasyonlarını tamamladıktan sonra Azure 'a tam geçişe göre ölçeklendirmeye hazırlar.



## <a name="next-steps"></a>Sonraki adımlar

- Bulut geçişi [hakkında bilgi edinin](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/) .
- [Veritabanı geçiş kılavuzundaki](https://datamigration.microsoft.com/)diğer senaryolar (kaynak/hedef çiftleri) için geçiş stratejileri hakkında bilgi edinin.
