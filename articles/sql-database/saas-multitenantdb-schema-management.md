---
title: Çok kiracılı bir uygulamada şemayı yönetme
description: Azure SQL Veritabanı’nı kullanan çok kiracılı bir uygulamada birden fazla kiracı için Şemayı yönetme
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib, sstein
ms.date: 12/18/2018
ms.openlocfilehash: 6f660426c41b37dd27438c28cbf603bdbf1e58b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269204"
---
# <a name="manage-schema-in-a-saas-application-that-uses-sharded-multi-tenant-sql-databases"></a>Çok kiracılı çok kiracılı SQL veritabanlarını kullanan bir SaaS uygulamasında şemayı yönetme

Bu öğretici, Bir Hizmet Olarak Yazılım (SaaS) uygulamasında veritabanı filosunu korumanın zorluklarını inceler. Çözümler veritabanı filosu genelinde şema değişiklikleri dışarı fanning için gösterilmiştir.

Herhangi bir uygulama gibi, Wingtip Tickets SaaS uygulaması da zamanla gelişecek ve veritabanında değişiklik yapılmasını gerektirecektir. Değişiklikler şema veya başvuru verilerini etkileyebilir veya veritabanı bakım görevlerini uygulayabilir. Kiracı deseni başına bir veritabanı kullanan bir SaaS uygulamasıyla, değişiklikler potansiyel olarak büyük bir kiracı veritabanları filosunda koordine edilmelidir. Ayrıca, oluşturuldukları sırada yeni veritabanlarına dahil edildiklerinden emin olmak için bu değişiklikleri veritabanı sağlama işlemine dahil etmeniz gerekir.

#### <a name="two-scenarios"></a>İki senaryo

Bu öğretici aşağıdaki iki senaryoyu inceler:
- Tüm kiracılar için başvuru veri güncelleştirmelerini dağıtın.
- Başvuru verilerini içeren tabloda bir dizin yeniden oluştur.

Azure SQL Veritabanı'nın [Elastik İşler](elastic-jobs-overview.md) özelliği, bu işlemleri kiracı veritabanları nda yürütmek için kullanılır. İşler ayrıca 'şablon' kiracı veritabanında da çalışır. Wingtip Biletleri örnek uygulamasında, bu şablon veritabanı yeni bir kiracı veritabanı sağlamak üzere kopyalanır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir iş aracısı oluşturun.
> * Birden çok kiracı veritabanlarında bir T-SQL sorgusu çalıştırın.
> * Tüm kiracı veritabanlarındaki başvuru verilerini güncelleştirin.
> * Tüm kiracı veritabanlarında tabloda bir dizin oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- Wingtip Tickets çok kiracılı veritabanı uygulaması zaten dağıtılmış olmalıdır:
    - Talimatlar için Wingtip Tickets SaaS çok kiracılı veritabanı uygulamasını tanıtan ilk öğreticiye bakın:<br />[Azure SQL Veritabanı'nı kullanan çok kiracılı bir uygulamayı dağıtın ve keşfedin.](saas-multitenantdb-get-started-deploy.md)
        - Dağıtım işlemi beş dakikadan kısa bir süre çalışır.
    - Wingtip'in *çok kiracılı parçalı* sürümünü yüklemiş olmalısınız. *Kiracı başına* *Bağımsız* ve Veritabanı sürümleri bu öğreticiyi desteklemez.

- SQL Server Management Studio'nun (SSMS) en son sürümü yüklenmelidir. [SSMS'i indirin ve yükleyin.](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

- Azure PowerShell yüklü olmalıdır. Ayrıntılar için Azure [PowerShell ile başlarken](https://docs.microsoft.com/powershell/azure/get-started-azureps)bakın.

> [!NOTE]
> Bu öğretici, sınırlı bir önizleme[(Elastik Veritabanı işleri)](sql-database-elastic-database-client-library.md)olan Azure SQL Veritabanı hizmetinin özelliklerini kullanır. Bu öğreticiyi yapmak istiyorsanız, *\@saasfeedback microsoft.com'na* abonelik kimliğinizi subject=Elastic Jobs Preview ile sağlayın. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle ilgili sorular veya destek için *\@SaaSFeedback microsoft.com* ile iletişime geçin.

## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS şema yönetim kalıplarına giriş

Bu örnekte kullanılan kırık çok kiracıveritabanı modeli, kiracı veritabanının bir veya daha fazla kiracı içermesini sağlar. Bu örnek, çok kiracı ve tek kiracı veritabanlarının bir karışımını kullanma potansiyelini inceleyerek *karma* kiracı yönetim modelini etkinleştirir. Bu veritabanlarındaki değişiklikleri yönetmek karmaşık olabilir. [Elastik İşler,](elastic-jobs-overview.md) çok sayıda veritabanının yönetimini ve yönetimini kolaylaştırır. İşler, bir kiracı veritabanı grubuna karşı Transact-SQL komut dosyalarını güvenli ve güvenilir bir şekilde görev olarak çalıştırmanızı sağlar. Görevler kullanıcı etkileşiminden veya girdiden bağımsızdır. Bu yöntem, değişiklikleri şemada veya yaygın başvuru verilerine dağıtmak için bir uygulamadaki tüm kiracılar arasında kullanılabilir. Elastik İşler, veritabanının altın şablon kopyasını korumak için de kullanılabilir. Şablon, her zaman en son şema ve başvuru verilerinin kullanımda olmasını sağlayarak yeni kiracılar oluşturmak için kullanılır.

![ekran](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Esnek İşler sınırlı önizlemesi

Azure SQL Veritabanı'nın tümleşik bir özelliği olan Elastik İşler'in yeni bir sürümü vardır. Esnek İşler’in bu yeni sürümü, şu anda sınırlı önizlemeyle sunulmaktadır. Sınırlı önizleme şu anda bir iş aracısı oluşturmak için PowerShell ve iş oluşturmak ve yönetmek için T-SQL'i destekler.
> [!NOTE]
> Bu öğretici, SQL Veritabanı hizmetinin sınırlı önizleme (Elastik Veritabanı işleri) olarak sunulan özelliklerini kullanır. Bu öğreticiyi yapmak isterseniz, abonelik kimliğinizi subject=Elastic Jobs Preview SaaSFeedback@microsoft.com ile sağlayın. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, en son ön sürüm işleri cmdlet’lerini indirip yükleyin. Bu önizleme sınırlıdır, SaaSFeedback@microsoft.com bu nedenle ilgili sorular veya destek için başvurun.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip Biletleri SaaS Çok Kiracı Veritabanı uygulama kaynak kodu ve komut alın

Wingtip Biletleri SaaS Çok kiracılı Veritabanı komut dosyaları ve uygulama kaynak kodu GitHub'daki [WingtipTicketsSaaS-MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) deposunda mevcuttur. Wingtip Tickets SaaS komut dosyalarını indirmek ve engelini kaldırmak için gereken adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) bakın.

## <a name="create-a-job-agent-database-and-new-job-agent"></a>İş aracısı veritabanı ve yeni iş aracısı oluşturma

Bu öğretici, iş aracısı veritabanı ve iş aracısı oluşturmak için PowerShell kullanmanızgerektirir. SQL Agent tarafından kullanılan MSDB veritabanı gibi, bir iş aracısı da iş tanımlarını, iş durumunu ve geçmişi depolamak için bir Azure SQL veritabanı kullanır. İş aracısı oluşturulduktan sonra, işleri hemen oluşturabilir ve izleyebilirsiniz.

1. **In PowerShell ISE**, açık *... Öğrenme Modülleri\\Şema\\Yönetimi Demo-SchemaManagement.ps1 . \\*
2. Betiği çalıştırmak için **F5**'e basın.

*Demo-SchemaManagement.ps1* komut dosyası, katalog sunucusunda _jobagent_ adlı bir veritabanı oluşturmak için *Deploy-SchemaManagement.ps1* komut dosyasını çağırır. Komut dosyası daha sonra _iş_ aracısı veritabanını parametre olarak geçirerek oluşturur.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tüm kiracılara yeni başvuru verilerini dağıtmak için bir iş oluşturma

#### <a name="prepare"></a>Hazırlama

Her kiracının veritabanı, **VenueTypes** tablosunda bir dizi mekan türü içerir. Her mekan türü, bir mekanda düzenlenebilen etkinliklerin türünü tanımlar. Bu mekan türleri, kiracı etkinlikleri uygulamasında gördüğünüz arka plan resimlerine karşılık gelir.  Bu alıştırmada, iki ek mekan türü eklemek için tüm veritabanlarına bir güncelleştirme dağıtırsınız: *Motosiklet Yarışı* ve *Yüzme Kulübü.*

İlk olarak, her kiracı veritabanında yer alan mekan türlerini gözden geçirin. SQL Server Management Studio'daki (SSMS) kiracı veritabanlarından birine bağlanın ve VenueTypes tablosunu inceleyin.  Bu tabloyu, veritabanı sayfasından erişilen Azure portalındaki Sorgu düzenleyicisinde de sorgulayabilirsiniz.

1. SSMS'i açın ve kiracı sunucusuna *bağlanın:&lt;&gt;kiracı1-dpt- kullanıcı .database.windows.net*
1. *Motosiklet Yarışı* ve *Yüzme Kulübü'nün* şu anda dahil **olmadığını** doğrulamak *için, kiracılar1-dpt kullanıcı&lt;&gt; * sunucusundaki *contosoconcerthall* veritabanına göz atın ve *VenueTypes* tablosunu sorgulayın.



#### <a name="steps"></a>Adımlar

Şimdi, iki yeni mekan türünü ekleyerek, her kiracı veritabanındaki **VenueTypes** tablosunu güncelleştirmek için bir iş oluşturursunuz.

Yeni bir iş oluşturmak için, _iş aracısı_ veritabanında oluşturulan iş sistemi depolanan yordamlar kümesini kullanırsınız. Depolanan yordamlar, iş aracısı oluşturulduğunda oluşturuldu.

1. SSMS'te kiracı sunucusuna bağlanın: kiracı1-mt-&lt;kullanıcı&gt;.database.windows.net

2. *Kiracılar1* veritabanına göz atın.

3. *Motosiklet Yarışı* ve *Yüzme Kulübü'nün* henüz sonuç listesinde olmadığını doğrulamak için *VenueTypes* tablosunu sorgulayın.

4. *Katalog-mt-&lt;&gt;kullanıcı .database.windows.net*olan katalog sunucusuna bağlanın.

5. Katalog sunucusundaki _jobagent_ veritabanına bağlanın.

6. SSMS olarak, dosyayı açın *... Öğrenme Modülleri\\Şema\\Yönetimi DeployReferenceData.sql . \\*

7. İfadeyi değiştirin: &lt;&gt; Wingtip Tickets SaaS Çok kiracılı Veritabanı uygulamasını dağıtırken kullanılan Kullanıcı değerini ayarlayın @User = kullanıcı ve değiştirin.

8. Betiği çalıştırmak için **F5**'e basın.

#### <a name="observe"></a>Gözlemle -mek

*DeployReferenceData.sql* komut dosyasında aşağıdaki öğeleri gözlemleyin:

- **sp\_\_add\_target group** hedef grup adı *DemoServerGroup*oluşturur ve gruba hedef üyeler ekler.

- **sp\_\_hedef\_\_grup üyesi eklemek** aşağıdaki öğeleri ekler:
    - Sunucu *server* hedef üye türü.
        - Bu kiracı veritabanları içeren *kiracı1-mt-kullanıcı&lt;&gt; * sunucusudur.
        - Sunucu dahil, işin yürütüldİğİ anda var olan kiracı veritabanlarını içerir.
    - *&lt;&gt; Katalog-mt-kullanıcı* sunucusunda bulunan şablon veritabanı *(basetenantdb)* için bir *veritabanı* hedef üye türü,
    - Daha sonraki bir öğreticide kullanılan *adhocreporting* veritabanını içerecek bir *veritabanı* hedef üye türü.

- **sp\_\_add job,** *Başvuru Veri Dağıtımı*adlı bir iş oluşturur.

- **sp\_\_add jobstep,** Başvuru tablosu, VenueTypes'i güncelleştirmek için T-SQL komut metnini içeren iş adımı oluşturur.

- Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. İşin ne zaman tamamlanabittiğini belirlemek için **yaşam döngüsü** sütunundaki durum değerini gözden geçirmek için bu sorguları kullanın. İş, kiracı veritabanını güncelleştirir ve başvuru tablosunu içeren iki ek veritabanını güncelleştirir.

SSMS'te, *kiracı1-mt-user&lt;&gt; * sunucusundaki kiracı veritabanına göz atın. *Motosiklet Yarışı* ve *Yüzme Kulübü'nün* artık masaya ekolduğunu doğrulamak için *VenueTypes* tablosunu sorgulayın. Mekan türlerinin toplam sayısı iki artmış olmalıdır.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Başvuru tablosu dizinini yönetmek için bir iş oluşturma

Bu alıştırma, tüm kiracı veritabanlarında başvuru tablosubirincil anahtarı dizin yeniden oluşturmak için bir iş oluşturur. Dizin yeniden oluşturma, performansı artırmak için yöneticinin büyük miktarda veri yükü yükledikten sonra çalıştırabileceği tipik bir veritabanı yönetimi işlemidir.

1. SSMS'te, *&lt;catalog-mt- User&gt;.database.windows.net* sunucusunda _jobagent_ veritabanına bağlanın.

2. SSMS olarak, açık *... Öğrenme Modülleri\\Şema\\Yönetimi OnlineReindex.sql . \\*

3. Betiği çalıştırmak için **F5**'e basın.

#### <a name="observe"></a>Gözlemle -mek

*OnlineReindex.sql* komut dosyasında aşağıdaki öğeleri gözlemleyin:

* **sp\_\_iş eklemek** *Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885*adlı yeni bir iş oluşturur.

* **sp\_\_add jobstep,** dizin güncelleştirmek için T-SQL komut metnini içeren iş adımı oluşturur.

* Komut dosyası izleme iş yürütme kalan görünümleri. İşin tüm hedef grup üyeleri üzerinde ne zaman başarıyla tamamlanabittiğini belirlemek için **yaşam döngüsü** sütunundaki durum değerini gözden geçirmek için bu sorguları kullanın.

## <a name="additional-resources"></a>Ek kaynaklar

<!-- TODO: Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->
* [Ölçeği artırılmış bulut veritabanlarını yönetme](elastic-jobs-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Birden çok veritabanında T-SQL işlerini çalıştırmak için bir iş aracısı oluşturma
> * Tüm kiracı veritabanlarındaki başvuru verilerini güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma

Ardından, kiracı veritabanları arasında dağıtılmış sorguları çalıştırmayı keşfetmek için [Ad hoc raporlama öğreticisini](saas-multitenantdb-adhoc-reporting.md) deneyin.

