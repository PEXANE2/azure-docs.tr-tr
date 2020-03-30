---
title: Bulut veritabanlarında dağıtılmış işlemler
description: Azure SQL Veritabanı ile Esnek Veritabanı İşlemlerine Genel Bakış
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: 3ca3e9074f28d66068d49b80915e98600759d9be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68568290"
---
# <a name="distributed-transactions-across-cloud-databases"></a>Bulut veritabanlarında dağıtılmış işlemler

Azure SQL Veritabanı (SQL DB) için esnek veritabanı hareketleri, SQL DB'de çeşitli veritabanlarına yayılan işlemleri çalıştırmanızı sağlar. SQL DB için elastik veritabanı işlemleri ADO .NET kullanarak .NET uygulamaları için kullanılabilir ve [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) sınıflarını kullanarak tanıdık programlama deneyimi ile tümleştirebilirsiniz. Kitaplığı almak için [.NET Framework 4.6.1 (Web Installer)](https://www.microsoft.com/download/details.aspx?id=49981)adresine bakın.

Şirket içinde, böyle bir senaryo genellikle Microsoft Distributed Transaction Koordinatörü (MSDTC) çalıştırılması gerekir. MSDTC Azure'da Hizmet Olarak Platform uygulaması için kullanılamadığından, dağıtılmış işlemleri koordine etme özelliği artık doğrudan SQL DB'ye entegre edilmiştir. Uygulamalar dağıtılmış hareketleri başlatmak için herhangi bir SQL Veritabanına bağlanabilir ve veritabanlarından biri aşağıdaki şekilde gösterildiği gibi dağıtılmış hareketi saydam bir şekilde koordine eder. 

  ![Elastik veritabanı hareketlerini kullanarak Azure SQL Veritabanı ile dağıtılmış işlemler ][1]

## <a name="common-scenarios"></a>Genel senaryolar

SQL DB için elastik veritabanı hareketleri, uygulamaların birkaç farklı SQL Veritabanında depolanan verilerde atomik değişiklikler yapılmasını sağlar. Önizleme, C# ve .NET'teki istemci tarafındaki geliştirme deneyimlerine odaklanır. T-SQL kullanarak sunucu tarafı deneyimi daha sonra planlanır.  
Esnek veritabanı hareketleri aşağıdaki senaryoları hedefler:

* Azure'daki çoklu veritabanı uygulamaları: Bu senaryoyla, veriler SQL DB'deki çeşitli veritabanlarıarasında farklı veri türlerinde farklı veritabanlarında bulunan dikey olarak bölümlere ayrılmıştır. Bazı işlemler, iki veya daha fazla veritabanında tutulan verilerde değişiklik yapılmasını gerektirir. Uygulama, veritabanları ndaki değişiklikleri koordine etmek ve atomikliği sağlamak için elastik veritabanı hareketlerini kullanır.
* Azure'da parçalanmış veritabanı uygulamaları: Bu senaryoda veri katmanı, SQL DB'deki birçok veritabanında verileri yatay olarak bölmek için [Elastik Veritabanı istemci kitaplığını](sql-database-elastic-database-client-library.md) veya kendi kendini parçalamayı kullanır. Önemli kullanım örneklerinden biri, değişiklikler kiracıları kapsadığında, parçalanmış çok kiracılı bir uygulama için atomik değişiklikler gerçekleştirme gereksinimidir. Her ikisi de farklı veritabanlarında bulunan bir kiracıdan diğerine aktarım örneğini düşünün. İkinci bir durum, büyük bir kiracının kapasite gereksinimlerini karşılamak için ince taneli parçalamadır ve bu da genellikle bazı atomik işlemlerin aynı kiracı için kullanılan çeşitli veritabanlarına yayılması gerektiğini ima eder. Üçüncü bir durum, veritabanları arasında çoğaltılan referans verileri atomik güncelleştirmeleridir. Atomik, işlem, bu doğrultuda ki işlemler artık önizleme kullanılarak çeşitli veritabanları arasında koordine edilebilir.
  Elastik veritabanı hareketleri veritabanları arasında işlem atomikliğini sağlamak için iki aşamalı taahhüt kullanır. Tek bir işlem içinde aynı anda 100'den az veritabanları içeren hareketler için uygundur. Bu sınırlar uygulanmaz, ancak bu sınırları aştığında elastik veritabanı işlemleri için performans ve başarı oranları beklenmelidir.

## <a name="installation-and-migration"></a>Kurulum ve geçiş

SQL DB'deki esnek veritabanı işlemlerinin yetenekleri .NET kitaplıklar System.Data.dll ve System.Transactions.dll güncellemeleri ile sağlanır. DL'ler, atomikliği sağlamak için gerektiğinde iki fazlı adak kullanılmasını sağlar. Esnek veritabanı hareketlerini kullanarak uygulama geliştirmeye başlamak için [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) veya daha sonraki bir sürümü yükleyin. .NET çerçevesinin önceki bir sürümünde çalışırken, hareketler dağıtılmış bir hareketi tanıtamaz ve bir özel durum yükseltilir.

Kurulumdan sonra, SQL DB'ye bağlantı içeren System.Transactions'da dağıtılmış hareket API'lerini kullanabilirsiniz. Bu API'leri kullanarak mevcut MSDTC uygulamalarınız varsa, 4.6.1 Çerçevesini yükledikten sonra .NET 4.6 için mevcut uygulamalarınızı yeniden oluşturmanız yeterlidir. Projeleriniz .NET 4.6'yı hedefliyorsa, yeni Framework sürümünden güncelleştirilmiş DL'leri otomatik olarak kullanır lar ve sql DB bağlantılarıyla birlikte dağıtılan işlem API çağrıları artık başarılı olur.

Elastik veritabanı hareketlerinin MSDTC yüklemesi gerektirmediğini unutmayın. Bunun yerine, elastik veritabanı hareketleri doğrudan ve SQL DB içinde yönetilir. Bu, SQL DB ile dağıtılmış hareketleri kullanmak için MSDTC dağıtımı gerekli olmadığından bulut senaryolarını önemli ölçüde basitleştirir. Bölüm 4, elastik veritabanı hareketlerini niçin dağıtılanın ve bulut uygulamalarınız ile birlikte gerekli .NET çerçevesini Azure'a nasıl dağıtılayabildiğini daha ayrıntılı olarak açıklar.

## <a name="development-experience"></a>Geliştirme deneyimi

### <a name="multi-database-applications"></a>Çoklu veritabanı uygulamaları

Aşağıdaki örnek kod .NET System.Transactions ile tanıdık programlama deneyimini kullanır. TransactionScope sınıfı .NET'te bir ortam hareketi kurar. (Bir "ortam hareketi" geçerli iş parçacığı yaşayan biridir.) TransactionScope içinde açılan tüm bağlantılar işlemle katılır. Farklı veritabanları katılırsa, hareket otomatik olarak dağıtılmış bir hareket e yükseltilir. İşlemin sonucu, bir commit'i belirtmek için kapsamı tamamlayabilmek üzere ayarlayarak denetlenir.

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

### <a name="sharded-database-applications"></a>Parçalanmış veritabanı uygulamaları

SQL DB için esnek veritabanı hareketleri, ölçeklenmiş bir veri katmanı için bağlantıları açmak için esnek veritabanı istemci kitaplığı OpenConnectionForKey yöntemini kullandığınız dağıtılmış hareketleri koordine etmeyi de destekler. Birkaç farklı parçalama anahtar değeri arasında değişiklikler için işlem tutarlılığı garanti etmek için gereken durumları düşünün. Farklı parçalama anahtar değerlerini barındıran kırıklara bağlantılar OpenConnectionForKey kullanılarak aracılık edilir. Genel durumda, bağlantılar işlem garantilerinin sağlanması için dağıtılmış bir işlem gerektiren farklı kırıklara olabilir. Aşağıdaki kod örneği bu yaklaşımı göstermektedir. Elastik veritabanı istemci kitaplığından bir parça eşlemi temsil etmek için shardmap adlı bir değişkenin kullanıldığını varsayar:

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


## <a name="net-installation-for-azure-cloud-services"></a>.Azure Bulut Hizmetleri için NET yüklemesi

Azure, .NET uygulamalarını barındırmak için çeşitli teklifler sunar. Azure [Uygulama Hizmeti, Bulut Hizmetleri ve Sanal Makineler karşılaştırmasında](/azure/architecture/guide/technology-choices/compute-decision-tree)farklı tekliflerin karşılaştırılması mevcuttur. Teklifin konuk işletim sistemi elastik işlemler için gereken .NET 4.6.1'den küçükse, konuk işletim sistemi 4.6.1'e yükseltmeniz gerekir. 

Azure Uygulama Hizmetleri için konuk işletim sistemi yükseltmeleri şu anda desteklenmez. Azure Sanal Makineler için VM'ye giriş yapmanız ve en son .NET çerçevesi için yükleyiciyi çalıştırmanız yeterlidir. Azure Bulut Hizmetleri için, dağıtımınızın başlangıç görevlerine daha yeni bir .NET sürümünün yüklenmesini eklemeniz gerekir. Kavramlar ve adımlar Bulut [Hizmeti Rolüne Yükle .NET'te](../cloud-services/cloud-services-dotnet-install-dotnet.md)belgelenmiştir.  

.NET 4.6.1'in yükleyicisinin Azure bulut hizmetlerinde önyükleme işlemi sırasında .NET 4.6'nın yükleyicisinden daha fazla geçici depolama alanı gerektirebileceğini unutmayın. Başarılı bir yükleme sağlamak için, Yerel Kaynaklar bölümündeki ServiceDefinition.csdef dosyanızda ve başlangıç görevinizin ortam ayarlarında aşağıdaki örnekte gösterildiği gibi Azure bulut hizmetiniz için geçici depolama alanını artırmanız gerekir:

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

## <a name="transactions-across-multiple-servers"></a>Birden çok sunucudaki hareketler

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Esnek veritabanı işlemleri Azure SQL Veritabanı'ndaki farklı SQL Veritabanı sunucularında desteklenir. İşlemler SQL Veritabanı sunucu sınırlarını aştığında, katılımcı sunucuların öncelikle karşılıklı iletişim ilişkisine girilmesi gerekir. İletişim ilişkisi kurulduktan sonra, iki sunucudan herhangi birinde bulunan herhangi bir veritabanı, diğer sunucudaki veritabanlarıyla esnek işlemlere katılabilir. İkiden fazla SQL Veritabanı sunucusuna yayılan işlemlerde, herhangi bir SQL Veritabanı sunucusu çifti için bir iletişim ilişkisinin olması gerekir.

Esnek veritabanı işlemleri için sunucular arası iletişim ilişkilerini yönetmek için aşağıdaki PowerShell cmdlets'i kullanın:

* **New-AzSqlServerCommunicationLink**: Azure SQL Veritabanı'ndaki iki SQL Veritabanı sunucusu arasında yeni bir iletişim ilişkisi oluşturmak için bu cmdlet'i kullanın. İlişki simetriktir, bu da her iki sunucunun da diğer sunucuyla işlem başlatabileceği anlamına gelir.
* **Get-AzSqlServerCommunicationLink**: Varolan iletişim ilişkilerini ve özelliklerini almak için bu cmdlet'i kullanın.
* **Remove-AzSqlServerCommunicationLink**: Varolan bir iletişim ilişkisini kaldırmak için bu cmdlet'i kullanın. 

## <a name="monitoring-transaction-status"></a>Hareket durumunu izleme

Devam eden esnek veritabanı işlemlerinizin durumunu ve ilerlemesini izlemek için SQL DB'de Dinamik Yönetim Görünümleri 'ni (DMV) kullanın. İşlemler ile ilgili tüm DMV'ler SQL DB'deki dağıtılmış işlemlerle ilgilidir. Burada DMVs ilgili listesini bulabilirsiniz: [İşlem İlgili Dinamik Yönetim Görünümleri ve Fonksiyonları (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

Bu DMV'ler özellikle yararlıdır:

* **sys.dm\_\_tran\_etkin hareketleri**: Şu anda etkin hareketleri ve durumlarını listeler. UOW (Birim İş) sütunu, aynı dağıtılmış işlemiçin farklı alt hareketleri tanımlayabilir. Aynı dağıtılmış işlemdeki tüm işlemler aynı UOW değerini taşır. Daha fazla bilgi için [DMV belgelerine](https://msdn.microsoft.com/library/ms174302.aspx) bakın.
* **sys.dm\_\_tran\_veritabanı işlemleri**: İşlemin günlükte yerleştirilmesi gibi hareketler hakkında ek bilgiler sağlar. Daha fazla bilgi için [DMV belgelerine](https://msdn.microsoft.com/library/ms186957.aspx) bakın.
* **sys.dm\_tran\_kilitleri**: Şu anda devam eden işlemler tarafından tutulan kilitler hakkında bilgi sağlar. Daha fazla bilgi için [DMV belgelerine](https://msdn.microsoft.com/library/ms190345.aspx) bakın.

## <a name="limitations"></a>Sınırlamalar

Aşağıdaki sınırlamalar şu anda SQL DB'deki esnek veritabanı işlemleri için geçerlidir:

* Yalnızca SQL DB'deki veritabanları ndaki hareketler desteklenir. SQL DB dışındaki diğer [X/Open XA](https://en.wikipedia.org/wiki/X/Open_XA) kaynak sağlayıcıları ve veritabanları elastik veritabanı işlemlerine katılamaz. Bu, esnek veritabanı işlemlerinin şirket içinde SQL Server ve Azure SQL Veritabanı'na kadar uzanamayacağı anlamına gelir. Şirket içinde dağıtılmış işlemler için MSDTC kullanmaya devam edin. 
* Bir .NET uygulamasından yalnızca istemci eşgüdümlü hareketler desteklenir. BEGIN DISTRIBUTED TRANSACTION gibi T-SQL için sunucu tarafı desteği planlanır, ancak henüz kullanılamaz. 
* WCF hizmetleri genelindeki hareketler desteklenmez. Örneğin, bir hareketi yürüten bir WCF hizmet yönteminiz vardır. Bir işlem kapsamı içinde arama çevreleyen bir [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception)olarak başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

Sorularınız için lütfen [SQL Veritabanı forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) bize ulaşın ve özellik istekleri için lütfen bunları [SQL Veritabanı geri bildirim forumuna](https://feedback.azure.com/forums/217321-sql-database/)ekleyin.

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png
