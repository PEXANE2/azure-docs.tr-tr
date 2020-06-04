---
title: Bulut veritabanlarında dağıtılmış işlemler
description: Azure SQL veritabanı ile elastik veritabanı Işlemlerine genel bakış.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: cd0116a417d2710d330c4be406a5d9d770f76461
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344552"
---
# <a name="distributed-transactions-across-cloud-databases"></a>Bulut veritabanlarında dağıtılmış işlemler
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Azure SQL veritabanı için elastik veritabanı işlemleri SQL veritabanı 'nda birkaç veritabanına yayılan işlemleri çalıştırmanıza izin verir. SQL veritabanı için elastik veritabanı işlemleri, ADO .NET kullanan .NET uygulamalarında kullanılabilir ve [System. Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) sınıflarını kullanarak tanıdık programlama deneyimiyle tümleştirilebilir. Kitaplığı almak için, bkz. [.NET Framework 4.6.1 (Web Yükleyicisi)](https://www.microsoft.com/download/details.aspx?id=49981).

Şirket içinde bu tür bir senaryo genellikle Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) çalıştırmayı gerektirir. MSDTC, Azure 'da hizmet olarak platform uygulaması için kullanılamadığından, dağıtılmış işlemleri koordine etme özelliği artık SQL veritabanı ile doğrudan tümleşiktir. Uygulamalar, dağıtılmış işlemleri başlatmak için SQL veritabanındaki herhangi bir veritabanına bağlanabilir ve veritabanlarından biri, aşağıdaki şekilde gösterildiği gibi, dağıtılmış işlemi saydam şekilde koordine eder.

  ![Elastik veritabanı işlemlerini kullanarak Azure SQL veritabanı ile dağıtılmış işlemler ][1]

## <a name="common-scenarios"></a>Yaygın senaryolar

SQL veritabanı için elastik veritabanı işlemleri, uygulamaların SQL veritabanı 'nda birkaç farklı veritabanında depolanan verilerde atomik değişiklikler yapmasını sağlar. Önizleme, C# ve .NET içindeki istemci tarafı geliştirme deneyimlerine odaklanmaktadır. T-SQL kullanan sunucu tarafı deneyimi daha sonra planlanmaktadır.  
Elastik veritabanı işlemleri aşağıdaki senaryoları hedefler:

* Azure 'daki çok sunuculu uygulamalar: Bu senaryoyla, veriler farklı veritabanlarında farklı veri türleri gibi SQL veritabanı 'nda çeşitli veritabanları arasında dikey olarak bölümlendirilir. Bazı işlemler, iki veya daha fazla veritabanında tutulan verilerde değişiklik yapılmasını gerektirir. Uygulama, değişiklikleri veritabanları genelinde koordine etmek ve Atomicity sağlamak için elastik veritabanı işlemlerini kullanır.
* Azure 'da parçalı veritabanı uygulamaları: Bu senaryoyla veri katmanı, verileri SQL veritabanı 'ndaki birçok veritabanı arasında yatay olarak bölümlemek için [elastik veritabanı istemci kitaplığını](elastic-database-client-library.md) veya kendi kendine parçalı veritabanını kullanır. Büyük bir kullanım durumu, kiracıların yayılmış bir çok kiracılı uygulama için atomik değişiklikler gerçekleştirme gereksinimdir. Tek bir kiracının, her ikisi de farklı veritabanlarında bulunan bir aktarım örneğini düşünün. İkinci bir durumda büyük bir kiracıya yönelik kapasite ihtiyaçlarını karşılamak için ince ayar yapılır, bu da genellikle bazı atomik işlemlerin aynı kiracı için kullanılan birkaç veritabanına uzaması gerektiğini gösterir. Üçüncü bir durum, veritabanları arasında çoğaltılan verileri başvuru verilerine yönelik atomik güncelleştirmelerdir. Atomik, işlem temelli, bu satırlardaki işlemler artık önizleme kullanılarak birkaç veritabanı arasında koordine edilebilir.
  Elastik veritabanı işlemleri, veritabanları genelinde işlem kararlılığını sağlamak için iki aşamalı tamamlama kullanır. Tek bir işlem içinde 100 'den daha az veritabanı içeren işlemlere uygun bir işlemdir. Bu sınırlar zorlanmaz, ancak bir tane, elastik veritabanı işlemleri için bu sınırları aşmadığında performans ve başarı oranları beklemelisiniz.

## <a name="installation-and-migration"></a>Yükleme ve geçiş

SQL veritabanı 'ndaki elastik veritabanı işlemlerine yönelik yetenekler, .NET kitaplıkları System. Data. dll ve System. Transactions. dll güncelleştirmeleri aracılığıyla sağlanır. Dll 'Ler, kararlılık sağlamak için gerektiğinde iki aşamalı yürütmenin kullanılmasını sağlar. Elastik veritabanı işlemlerini kullanarak uygulama geliştirmeye başlamak için [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) veya sonraki bir sürümü yüklemek. .NET Framework 'ün önceki bir sürümünde çalışırken, işlemler dağıtılmış bir işleme yükseltilmeyecektir ve bir özel durum oluşur.

Yükleme sonrasında, System. Transactions içindeki dağıtılmış işlem API 'Lerini SQL veritabanı bağlantılarıyla kullanabilirsiniz. Bu API 'Leri kullanarak mevcut MSDTC uygulamalarınız varsa, 4.6.1 çerçevesini yükledikten sonra, .NET 4,6 için mevcut uygulamalarınızı yeniden oluşturmanız yeterlidir. Projeleriniz .NET 4,6 ' i hedeflerse, yeni çerçeve sürümünden güncelleştirilmiş dll 'Leri otomatik olarak kullanır ve SQL veritabanı bağlantıları ile birlikte dağıtılmış işlem API çağrıları artık başarılı olur.

Elastik veritabanı işlemlerinin MSDTC yüklemesi gerektirmediğinden emin olduğunu unutmayın. Bunun yerine, elastik veritabanı işlemleri doğrudan SQL veritabanı içinde ve tarafından yönetilir. Bu, bir MSDTC dağıtımının SQL veritabanı ile dağıtılmış işlemleri kullanması gerekmediğinden, bulut senaryolarını önemli ölçüde basitleştirir. Bölüm 4 ' te, elastik veritabanı işlemlerini ve gerekli .NET Framework 'ü bulut uygulamalarınızla birlikte Azure 'a dağıtma hakkında daha ayrıntılı bilgi verilmektedir.

## <a name="development-experience"></a>Geliştirme deneyimi

### <a name="multi-database-applications"></a>Çoklu veritabanı uygulamaları

Aşağıdaki örnek kod, .NET System. Transactions ile tanıdık programlama deneyimini kullanır. TransactionScope sınıfı .NET 'te bir ortam işlemi oluşturur. ("Çevresel işlem" geçerli iş parçacığında bulunan bir işlemdir.) TransactionScope içinde açılan tüm bağlantılar işleme katılır. Farklı veritabanları katılılırsa, işlem otomatik olarak dağıtılmış bir işleme yükseltilir. İşlemin sonucu, bir yürütmeyi göstermek için kapsamın tamamlanacağı şekilde ayarlanarak denetlenir.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Parçalı veritabanı uygulamaları

SQL veritabanı için elastik veritabanı işlemleri Ayrıca, genişletilmiş bir veri katmanının bağlantılarını açmak üzere elastik veritabanı istemci kitaplığının OpenConnectionForKey yöntemini kullandığınız dağıtılmış işlemleri koordine etme desteğini destekler. Birçok farklı parçalı anahtar değeri arasındaki değişiklikler için işlem tutarlılığını garanti etmeniz gereken durumları göz önünde bulundurun. Farklı parçalama anahtar değerlerini barındıran parçaların bağlantıları OpenConnectionForKey kullanılarak aracılı yapılır. Genel durumda, bağlantı, işlem garantilerinin dağıtılmış bir işlem gerektirdiğini sağlayan farklı parçalar olabilir.
Aşağıdaki kod örneğinde bu yaklaşım gösterilmektedir. Bu, shardmap adlı bir değişkenin, elastik veritabanı istemci kitaplığından parça haritasını temsil etmek için kullanıldığını varsayar:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

## <a name="net-installation-for-azure-cloud-services"></a>Azure Cloud Services için .NET yüklemesi

Azure, .NET uygulamalarını barındırmak için çeşitli teklifler sağlar. Farklı tekliflerin karşılaştırması [Azure App Service, Cloud Services ve sanal makine karşılaştırmasına](/azure/architecture/guide/technology-choices/compute-decision-tree)göre sunulmaktadır. Teklifin Konuk işletim sistemi elastik işlemler için gerekli .NET 4.6.1 'den küçükse Konuk işletim sistemini 4.6.1 'e yükseltmeniz gerekir.

Azure App Service için, Konuk işletim sistemi yükseltmeleri Şu anda desteklenmemektedir. Azure sanal makineler için, VM 'de oturum açıp en son .NET Framework için yükleyiciyi çalıştırmanız yeterlidir. Azure Cloud Services için, dağıtımınızın başlangıç görevlerine daha yeni bir .NET sürümünün yüklenmesini eklemeniz gerekir. Kavramlar ve adımlar, [.net 'i bir bulut hizmeti rolü üzerine yüklemiştir](../../cloud-services/cloud-services-dotnet-install-dotnet.md).  

.NET 4.6.1 yükleyicisinin, .NET 4,6 için yükleyiciden farklı olarak Azure Cloud Services 'ta önyükleme işlemi sırasında daha fazla geçici depolama gerektirebileceğini unutmayın. Yüklemenin başarılı olmasını sağlamak için, aşağıdaki örnekte gösterildiği gibi, LocalResources bölümündeki ServiceDefinition. csdef dosyanızdaki Azure bulut hizmetinizin geçici depolama alanını ve başlangıç göreviniz ortam ayarlarını artırmanız gerekir:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>

## <a name="transactions-across-multiple-servers"></a>Birden çok sunucu arasında işlem

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Esnek veritabanı işlemleri, Azure SQL veritabanı 'ndaki farklı sunucular arasında desteklenir. İşlemler çapraz sunucu sınırları sırasında, katılım sunucularının ilk olarak karşılıklı iletişim ilişkisine girilmesi gerekir. İletişim ilişkisi kurulduktan sonra, iki sunucudan herhangi bir veritabanı diğer sunucudaki veritabanları ile elastik işlemlere katılabilir. İkiden fazla sunucuyu kapsayan işlemler sayesinde, herhangi bir sunucu çifti için bir iletişim ilişkisinin olması gerekir.

Elastik veritabanı işlemleri için çapraz sunucu iletişim ilişkilerini yönetmek üzere aşağıdaki PowerShell cmdlet 'lerini kullanın:

* **New-AzSqlServerCommunicationLink**: Azure SQL veritabanı 'nda iki sunucu arasında yeni bir iletişim ilişkisi oluşturmak için bu cmdlet 'i kullanın. İlişki simetrik olduğundan, her iki sunucunun da diğer sunucu ile işlemleri başlatabileceği anlamına gelir.
* **Get-AzSqlServerCommunicationLink**: var olan iletişim ilişkilerini ve bunların özelliklerini almak için bu cmdlet 'i kullanın.
* **Remove-AzSqlServerCommunicationLink**: var olan bir iletişim ilişkisini kaldırmak için bu cmdlet 'i kullanın.

## <a name="monitoring-transaction-status"></a>İşlem durumunu izleme

Devam eden elastik veritabanı işlemlerinizin durumunu ve ilerlemesini izlemek için SQL veritabanı 'nda dinamik yönetim görünümlerini (DMVs) kullanın. İşlemler ile ilgili tüm DMVs 'ler SQL veritabanı 'nda dağıtılmış işlemler için geçerlidir. Bunlara karşılık gelen DMVs listesini buradan bulabilirsiniz: [Işlemle Ilgili dinamik yönetim görünümleri ve işlevleri (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

Bu DMVs 'ler özellikle yararlıdır:

* **sys.DM \_ Tran \_ etkin \_ işlemler**: etkin olan işlemleri ve bunların durumlarını listeler. UOW (Iş birimi) sütunu, aynı dağıtılmış işleme ait farklı alt işlemleri tanımlayabilir. Aynı dağıtılmış işlem içindeki tüm işlemler aynı UOW değerini taşır. Daha fazla bilgi için bkz. [DMV belgeleri](https://msdn.microsoft.com/library/ms174302.aspx) .
* **sys.DM \_ Tran \_ veritabanı \_ işlemleri**: işlemin günlüğe yerleştirilmesi gibi işlemler hakkında ek bilgiler sağlar. Daha fazla bilgi için bkz. [DMV belgeleri](https://msdn.microsoft.com/library/ms186957.aspx) .
* **sys.DM \_ Tran \_ kilitleri**: devam eden işlemler tarafından şu anda tutulan kilitler hakkında bilgi sağlar. Daha fazla bilgi için bkz. [DMV belgeleri](https://msdn.microsoft.com/library/ms190345.aspx) .

## <a name="limitations"></a>Sınırlamalar

Aşağıdaki sınırlamalar şu anda SQL veritabanındaki elastik veritabanı işlemleri için geçerlidir:

* Yalnızca SQL veritabanı 'ndaki veritabanları arasındaki işlemler desteklenir. Diğer [X/açık XA](https://en.wikipedia.org/wiki/X/Open_XA) kaynak SAĞLAYıCıLARı ve SQL veritabanı dışındaki veritabanları, elastik veritabanı işlemlerine katılamaz. Diğer bir deyişle, elastik veritabanı işlemlerinin şirket içi SQL Server ve Azure SQL veritabanı arasında genişlemeyeceği anlamına gelir. Şirket içi dağıtılmış işlemler için MSDTC kullanmaya devam edin.
* Yalnızca bir .NET uygulamasından alınan istemci ile eşgüdümlü işlemler desteklenir. BAŞLANGıÇ DAĞıTıM IŞLEMI gibi T-SQL için sunucu tarafı desteği planlanmaktadır, ancak henüz kullanılamamaktadır.
* WCF hizmetleri arasında işlem desteklenmez. Örneğin, bir işlemi yürüten bir WCF hizmeti yönteminiz vardır. Çağrıyı bir işlem kapsamı içinde kapsayan bir [System. ServiceModel. ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception)olarak başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

Sorularınız için lütfen [SQL veritabanı Için Microsoft Q&soru sayfasında](https://docs.microsoft.com/answers/topics/azure-sql-database.html)bize ulaşın. Özellik istekleri için lütfen bunları [SQL veritabanı geri bildirim forumuna](https://feedback.azure.com/forums/217321-sql-database/)ekleyin.

<!--Image references-->
[1]: ./media/elastic-transactions-overview/distributed-transactions.png
 