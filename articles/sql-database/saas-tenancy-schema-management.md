---
title: Tek kiracılı bir uygulamada şema yönetme
description: Azure SQL Veritabanı'nı kullanan tek kiracılı bir uygulamada birden çok kiracı için Şema'yı yönetme
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
ms.date: 09/19/2018
ms.openlocfilehash: b6802d97b964b8863f6c2fce0cebfe16782b46fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269217"
---
# <a name="manage-schema-in-a-saas-application-using-the-database-per-tenant-pattern-with-azure-sql-database"></a>Azure SQL Veritabanı ile kiracı başına veritabanı deseni kullanarak SaaS uygulamasında şemayı yönetme
 
Bir veritabanı uygulaması geliştikçe, değişikliklerin kaçınılmaz olarak veritabanı şeması veya referans verilerine yapılması gerekir.  Veritabanı bakım görevleri de periyodik olarak gereklidir. Kiracı deseni başına veritabanını kullanan bir uygulamayı yönetmek, bu değişiklikleri veya bakım görevlerini kiracı veritabanları filosuna uygulamanızı gerektirir.

Bu öğretici, tüm kiracılar için başvuru veri güncelleştirmelerini dağıtma ve başvuru verilerini içeren tabloda bir dizin yeniden oluşturma gibi iki senaryoyu inceler. [Elastik işler](elastic-jobs-overview.md) özelliği, tüm kiracı veritabanlarında ve yeni kiracı veritabanları oluşturmak için kullanılan şablon veritabanında bu eylemleri yürütmek için kullanılır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> 
> * İş aracısı oluşturma
> * T-SQL işlerinin tüm kiracı veritabanlarında çalıştırılmasına neden olun
> * Tüm kiracı veritabanlarındaki başvuru verilerini güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip Tickets SaaS Database Per Tenant uygulaması dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için, [kiracı uygulaması başına Wingtip Tickets SaaS veritabanını dağıt'a](saas-dbpertenant-get-started-deploy.md) bakın ve keşfedin
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio’nun (SSMS) en son sürümünün yüklendiğinden. [SSMS’yi İndirin ve Yükleyin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> Bu öğretici, SQL Veritabanı hizmetinin sınırlı önizleme (Elastik Veritabanı işleri) olarak sunulan özelliklerini kullanır. Bu öğreticiyi yapmak isterseniz, abonelik kimliğinizi subject=Elastic Jobs Preview SaaSFeedback@microsoft.com ile sağlayın. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, SaaSFeedback@microsoft.com bu nedenle ilgili sorular veya destek için başvurun.

## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS şema yönetim kalıplarına giriş

Kiracı deseni başına veritabanı kiracı verilerini etkin bir şekilde yalıtmak, ancak yönetmek ve korumak için veritabanlarının sayısını artırır. [Elastik İşler,](elastic-jobs-overview.md) SQL veritabanlarının yönetimini ve yönetimini kolaylaştırır. İşler, bir veritabanı grubuna karşı güvenli ve güvenilir bir şekilde görevleri (T-SQL komut dosyaları) çalıştırmanızı sağlar. İşler, bir uygulamadaki tüm kiracı veritabanlarında şema ve yaygın başvuru verisi değişikliklerini dağıtabilir. Elastik İşler, yeni kiracılar oluşturmak için kullanılan bir *şablon* veritabanını korumak ve her zaman en son şema ve başvuru verilerine sahip olmasını sağlamak için de kullanılabilir.

![ekran](media/saas-tenancy-schema-management/schema-management-dpt.png)


## <a name="elastic-jobs-limited-preview"></a>Esnek İşler sınırlı önizlemesi

Azure SQL Veritabanı'nın tümleşik bir özelliği olan Elastik İşler'in yeni bir sürümü vardır. Esnek İşler’in bu yeni sürümü, şu anda sınırlı önizlemeyle sunulmaktadır. Bu sınırlı önizleme şu anda bir iş aracısı oluşturmak için PowerShell'in ve iş oluşturmak ve yönetmek için T-SQL'in kullanılmasını destekler.

> [!NOTE]
> Bu öğretici, SQL Veritabanı hizmetinin sınırlı önizleme (Elastik Veritabanı işleri) olarak sunulan özelliklerini kullanır. Bu öğreticiyi yapmak isterseniz, abonelik kimliğinizi subject=Elastic Jobs Preview SaaSFeedback@microsoft.com ile sağlayın. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, SaaSFeedback@microsoft.com bu nedenle ilgili sorular veya destek için başvurun.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Kiracı başvuru komut dosyaları başına Wingtip Tickets SaaS veritabanını edinin

Uygulama kaynak kodu ve yönetim komut leri [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub repo mevcuttur. Wingtip Tickets SaaS komut dosyalarını indirmek ve engelini kaldırmak için gereken adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) göz atın.

## <a name="create-a-job-agent-database-and-new-job-agent"></a>İş aracısı veritabanı ve yeni iş aracısı oluşturma

Bu öğretici, bir iş aracısı ve onun destek iş aracısı veritabanı oluşturmak için PowerShell kullanmanız gerektirir. İş aracısı veritabanı iş tanımlarını, iş durumunu ve geçmişini tutar. İş aracısı ve veritabanı oluşturulduktan sonra, işleri hemen oluşturabilir ve izleyebilirsiniz.

1. **In PowerShell ISE**, açık ... \\Öğrenme Modülleri\\Şema\\Yönetimi*Demo-SchemaManagement.ps1*.
1. Betiği çalıştırmak için **F5**'e basın.

*Demo-SchemaManagement.ps1* komut dosyası, katalog sunucusunda *osagent* adında bir SQL veritabanı oluşturmak için *Deploy-SchemaManagement.ps1* komut dosyasını çağırır. Daha sonra, veritabanını parametre olarak kullanarak iş aracısını oluşturur.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tüm kiracılara yeni başvuru verilerini dağıtmak için bir iş oluşturma

Wingtip Tickets uygulamasında, her kiracı veritabanı desteklenen mekan türlerini içerir. Her mekan, barındırılabilen olayların türünü tanımlayan ve uygulamada kullanılan arka plan görüntüsünü belirleyen belirli bir mekan türüne sahiptir. Uygulamanın yeni tür olayları desteklemesi için bu başvuru verilerinin güncellenmesi ve yeni yer türleri eklenmesi gerekir.  Bu alıştırmada, şu iki ek mekan türünü eklemek için tüm kiracı veritabanlarına yönelik bir güncelleştirmeyi dağıtacaksınız: *Motosiklet Yarışı* ve *Yüzme Kulübü*.

İlk olarak, her kiracı veritabanında yer alan mekan türlerini gözden geçirin. SQL Server Management Studio'daki (SSMS) kiracı veritabanlarından birine bağlanın ve VenueTypes tablosunu inceleyin.  Bu tabloyu, veritabanı sayfasından erişilen Azure portalındaki Sorgu düzenleyicisinde de sorgulayabilirsiniz. 

1. SSMS'i açın ve kiracı sunucusuna *bağlanın:&lt;&gt;kiracı1-dpt- kullanıcı .database.windows.net*
1. *Motosiklet Yarışı* ve *Yüzme Kulübü'nün* şu anda dahil **olmadığını** doğrulamak *için, kiracılar1-dpt kullanıcı&lt;&gt; * sunucusundaki _contosoconcerthall_ veritabanına göz atın ve *VenueTypes* tablosunu sorgulayın.

Şimdi, yeni mekan türlerini eklemek için tüm kiracı veritabanlarındaki *VenueTypes* tablosunu güncelleştirmek için bir iş oluşturalım.

Yeni bir iş oluşturmak için, iş aracısı oluşturulduğunda _iş aracısı_ veritabanında oluşturulan iş sistemi depolanan yordamlar kümesini kullanırsınız.

1. SSMS'te katalog sunucusuna bağlanın: *katalog-dpt-&lt;&gt;kullanıcı .database.windows.net* sunucusu 
1. SSMS olarak, dosyayı açın ... \\Öğrenme Modülleri\\Şema\\Yönetimi DeployReferenceData.sql
1. Bildirimi değiştirin: @wtpUser &lt;SET&gt; = kullanıcı ve Wingtip Tickets SaaS Veritabanı'nı Kiracı Başına dağıtTığınızda kullanılan Kullanıcı değerini değiştirin
1. _İş aracısı_ veritabanına bağlı olduğunuzu emin olun ve komut dosyasını çalıştırmak için **F5** tuşuna basın

*DeployReferenceData.sql* komut dosyasında aşağıdaki öğeleri gözlemleyin:
* **sp\_\_add\_hedef grubu** hedef grup adı DemoServerGroup oluşturur.
* **sp\_\_add\_\_target group üyesi** hedef veritabanları kümesini tanımlamak için kullanılır.  Önce _kiracı1-dpt-&lt;kullanıcı&gt; _ sunucusu eklenir.  Sunucuyu hedef olarak eklemek, iş yürütme sırasında o sunucudaki veritabanlarının işe dahil edilmesine neden olur. Daha sonra _basetenantdb_ veritabanı ve *adhocreporting* veritabanı (daha sonraki bir öğreticide kullanılan) hedef olarak eklenir.
* **sp\_\_ekle iş** Başvuru _Veri Dağıtımı_adlı bir iş oluşturur.
* **sp\_\_add jobstep,** Başvuru tablosu, VenueTypes'i güncelleştirmek için T-SQL komut metnini içeren iş adımı oluşturur.
* Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. İşin tüm hedef veritabanlarında ne zaman tamamlanabittiğini belirlemek için **yaşam döngüsü** sütunundaki durum değerini gözden geçirmek için bu sorguları kullanın.

Komut dosyası tamamlandıktan sonra, başvuru verilerinin güncelleştirildiğini doğrulayabilirsiniz.  SSMS'te, *kiracılar1-dpt&lt;kullanıcı&gt; * sunucusundaki *contosoconcerthall* veritabanına göz atın ve *VenueTypes* tablosunu sorgula.  Motosiklet *Yarışı* ve *Yüzme Kulübü'nün* şu anda mevcut olup **olmadığını** kontrol edin.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Başvuru tablosu dizinini yönetmek için bir iş oluşturma

Bu alıştırma, dizin başvuru tablosu birincil anahtarı üzerinde yeniden oluşturmak için bir iş kullanır.  Bu, büyük miktarda veri yüklendikten sonra yapılabilecek tipik bir veritabanı bakım işlemidir.

Aynı iş “sistem” saklı yordamlarını kullanarak bir iş oluşturun.

1. SSMS'i açın ve _katalog-dpt-&lt;&gt;kullanıcı .database.windows.net_ sunucusuna bağlanın
1. Dosyayı açın _... Öğrenme Modülleri\\Şema\\Yönetimi OnlineReindex.sql \\_
1. Sağ tıklatın, Bağlantı'yı seçin ve _katalog-dpt-kullanıcı&lt;&gt;.database.windows.net_ sunucusuna bağlayın, zaten bağlı değilse
1. _İş aracısı_ veritabanına bağlı olduğunuzu emin olun ve komut dosyasını çalıştırmak için **F5** tuşuna basın

_OnlineReindex.sql_ komut dosyasında aşağıdaki öğeleri gözlemleyin:
* **sp\_\_iş eklemek** "Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885" adlı yeni bir iş oluşturur
* **sp\_\_add jobstep** dizini güncelleştirmek için T-SQL komut metni içeren iş adımı oluşturur
* Komut dosyası izleme iş yürütme kalan görünümleri. İşin tüm hedef grup üyeleri üzerinde ne zaman başarıyla tamamlanabittiğini belirlemek için **yaşam döngüsü** sütunundaki durum değerini gözden geçirmek için bu sorguları kullanın.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> 
> * T-SQL iş çoklu veritabanları arasında çalıştırmak için bir iş aracısı oluşturma
> * Tüm kiracı veritabanlarındaki başvuru verilerini güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma

Ardından, kiracı veritabanları arasında dağıtılmış sorguları çalıştırmayı keşfetmek için [Ad hoc raporlama öğreticisini](saas-tenancy-cross-tenant-reporting.md) deneyin.


## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip Tickets SaaS Veritabanı Başına Kiracı uygulama dağıtımı üzerine inşa ek öğreticiler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Ölçeği artırılmış bulut veritabanlarını yönetme](elastic-jobs-overview.md)