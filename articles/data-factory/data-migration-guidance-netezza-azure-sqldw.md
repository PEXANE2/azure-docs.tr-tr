---
title: Şirket içi Netezza Server 'dan Azure 'a veri geçirmek için Azure Data Factory kullanın | Microsoft Docs
description: Şirket içi Netezza Server 'dan Azure 'a veri geçirmek için Azure Data Factory kullanın.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 9/03/2019
ms.openlocfilehash: 4690fd81247035267861b06c204c6db7a052eba5
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259567"
---
# <a name="use-azure-data-factory-to-migrate-data-from-on-premises-netezza-server-to-azure"></a>Şirket içi Netezza Server 'dan Azure 'a veri geçirmek için Azure Data Factory kullanma 

Azure Data Factory, verileri şirket içi Netezza sunucusundan Azure depolama veya Azure SQL veri ambarı 'na geçirmeye yönelik bir performans, sağlam ve ekonomik bir mekanizma sağlar. Bu makalede veri mühendisleri ve geliştiriciler için aşağıdaki bilgiler sağlanmaktadır:

> [!div class="checklist"]
> * Performans 
> * Esnekliği Kopyala
> * Ağ güvenliği
> * Üst düzey çözüm mimarisi 
> * En iyi uygulama uygulamaları  

## <a name="performance"></a>Performans

Azure Data Factory, farklı düzeylerde paralellik sağlayan sunucusuz bir mimari sunar ve bu sayede, veri taşıma aktarım hızını en üst düzeye çıkarmak için geliştiricilerin, ağ bant genişliğinden ve veritabanı bant genişliğinden tamamen yararlanmak üzere işlem hatları oluşturmalarına olanak tanır. ortamınızın.

![performans](media/data-migration-guidance-netezza-azure-sqldw/performance.png)

- Tek bir kopyalama etkinliği ölçeklenebilir işlem kaynaklarından yararlanabilir: Azure Integration Runtime kullanırken, her bir kopyalama etkinliği için sunucusuz bir şekilde [en fazla 256 DIUs](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#data-integration-units) belirtebilirsiniz; Şirket içinde barındırılan Integration Runtime kullanılırken, makineyi el ile ölçeklendirebilir veya birden fazla makineye ([4 düğüme kadar](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)) ölçeklendirebilirsiniz ve tek bir kopyalama etkinliği bölümünü tüm düğümlerde dağıtır. 
- Tek bir kopyalama etkinliği, birden çok iş parçacığı kullanarak veri deposundan okur ve yazar. 
- Azure Data Factory denetim akışı, örneğin [her döngü için](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)kullanarak birden çok kopyalama etkinliğini paralel olarak başlatabilir. 

[Kopyalama etkinliği performans kılavuzlarından](https://docs.microsoft.com/azure/data-factory/copy-activity-performance) daha fazla ayrıntı edinebilirsiniz

## <a name="resilience"></a>Dayanıklılık

Tek bir kopyalama etkinliği çalıştırmasında Azure Data Factory, yerleşik yeniden deneme mekanizmasına sahiptir, bu nedenle veri depolarında veya temel ağdaki belirli bir geçici başarısızlık düzeyini işleyebilir.

Azure Data Factory kopyalama etkinliği, verileri kaynak ve havuz veri depoları arasında kopyalarken uyumsuz satırları işlemenin iki yolunu da sunar. Uyumsuz verilerle karşılaşıldığında kopyalama etkinliğini durdurabilir ve devreder veya uyumsuz veri satırlarını atlayarak Rest verilerini kopyalamaya devam edebilirsiniz. Ayrıca, hatanın nedenini öğrenmek, veri kaynağındaki verileri onarmak ve kopyalama etkinliğini yeniden denemek için Azure Blob depolamada veya Azure Data Lake Store uyumsuz satırları günlüğe kaydedebilirsiniz.

## <a name="network-security"></a>Ağ güvenliği 

Azure Data Factory, varsayılan olarak, HTTPS protokolü üzerinden şifrelenmiş bağlantı kullanarak şirket içi Netezza sunucusundan Azure depolama veya Azure SQL veri ambarı 'na veri aktarır. Aktarım sırasında veri şifrelemesi sağlar ve gizlice dinleme ve ortadaki adam saldırıları önler.

Alternatif olarak, verilerin genel Internet üzerinden aktarılmasını istemiyorsanız, verileri Azure Express Route aracılığıyla özel bir eşleme bağlantısı üzerinden aktararak daha yüksek bir güvenlik elde edebilirsiniz. Bu şekilde nasıl ulaşılabilecek aşağıda çözüm mimarisine bakın.

## <a name="solution-architecture"></a>Çözüm mimarisi

Verileri genel Internet üzerinden geçirme:

![çözüm-mimari-genel-ağ](media/data-migration-guidance-netezza-azure-sqldw/solution-architecture-public-network.png)

- Bu mimaride, veriler genel Internet üzerinden HTTPS kullanılarak güvenli bir şekilde aktarılır.
- Bu mimariye ulaşmak için kurumsal güvenlik duvarının arkasındaki bir Windows makinesinde şirket içinde barındırılan tümleştirme çalışma zamanı Azure Data Factory yüklemeniz gerekir. Windows makinesindeki Azure Data Factory şirket içinde barındırılan tümleştirme çalışma zamanının Netezza sunucunuza doğrudan erişmesini sağlayın. Ağınızı tamamen kullanabilmeniz için makinenizi el ile ölçeklendirebilir veya birden çok makineye ölçeklendirebilirsiniz, verileri kopyalamak için veri bant genişliği depolanır.
- Bu mimari kullanılarak hem ilk anlık görüntü veri geçişi hem de Delta verileri geçişi elde edilebilir.

Verileri özel bağlantı üzerinden geçir: 

![çözüm-mimari-özel-ağ](media/data-migration-guidance-netezza-azure-sqldw/solution-architecture-private-network.png)

- Bu mimaride veri geçişi, Azure Express Route aracılığıyla özel bir eşleme bağlantısı üzerinden yapılır, böylece veriler hiçbir şekilde herkese açık Internet üzerinden geçmez. 
- Bu mimariye ulaşmak için Azure sanal ağınızdaki bir Windows VM 'sine şirket içinde barındırılan tümleştirme çalışma zamanı Azure Data Factory yüklemeniz gerekir. Ağınızı tamamen kullanabilmeniz için sanal makinelerinizi el ile ölçeklendirebilir veya birden çok VM 'ye ölçeklendirebilirsiniz, verileri kopyalamak için veri bant genişliği depolanır.
- Bu mimari kullanılarak hem ilk anlık görüntü veri geçişi hem de Delta verileri geçişi elde edilebilir.

## <a name="implementation-best-practices"></a>En iyi uygulama uygulamaları 

### <a name="authentication-and-credential-management"></a>Kimlik doğrulama ve kimlik bilgisi yönetimi 

- Netezza kimlik doğrulaması yapmak için [bağlantı dizesi aracılığıyla ODBC kimlik doğrulamasını](https://docs.microsoft.com/azure/data-factory/connector-netezza#linked-service-properties)kullanabilirsiniz. 
- Azure Blob depolamaya bağlanmak için birden çok kimlik doğrulama türü desteklenir.  [Azure kaynakları için yönetilen kimliklerin](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#managed-identity) kullanımı kesinlikle önerilir: Azure AD 'de otomatik olarak yönetilen Azure Data Factory tanımlamak Için, bağlantılı hizmet tanımında kimlik bilgileri sağlamadan işlem hatlarını yapılandırmanıza olanak tanır.  Alternatif olarak, [hizmet sorumlusu](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#service-principal-authentication), [paylaşılan erişim imzası](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication)veya [depolama hesabı anahtarı](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#account-key-authentication)kullanarak Azure Blob depolama alanında kimlik doğrulaması yapabilirsiniz. 
- Azure Data Lake Storage 2. bağlanmak için birden çok kimlik doğrulama türü de desteklenir.  [Hizmet sorumlusu](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication) veya [depolama hesabı anahtarı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#account-key-authentication) da kullanılabilir olsa da, [Azure kaynakları için yönetilen kimliklerin](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#managed-identity) kullanılması önemle önerilir. 
- Azure SQL veri ambarı 'na bağlanmak için birden çok kimlik doğrulama türü de desteklenir. [Azure kaynakları için yönetilen kimliklerin](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#managed-identity) kullanılması, [hizmet sorumlusu](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#service-principal-authentication) veya [SQL kimlik doğrulamasının](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#sql-authentication) de kullanılabilmesine rağmen, son derece önerilir.
- Azure kaynakları için Yönetilen kimlikler kullanmıyorsanız, [Azure Key Vault kimlik bilgilerinin depolanması](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) , Azure Data Factory bağlı hizmetleri değiştirmeden anahtarları merkezi olarak yönetmeyi ve döndürmeyi kolaylaştırmak için kesinlikle önerilir.  Bu Ayrıca, [CI/CD için en iyi uygulamalardan](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd)biridir. 

### <a name="initial-snapshot-data-migration"></a>İlk anlık görüntü verilerini geçirme 

Küçük tablolar için, birim boyutu 100 GB 'tan küçük olduğunda veya 2 saat içinde Azure 'a geçirilebiliyorsa, her bir kopyalama işi tablo başına veri yükleme yapabilirsiniz. Daha iyi aktarım hızı için farklı tabloları eşzamanlı olarak yüklemek üzere birden çok Azure Data Factory kopyalama işi çalıştırabilirsiniz. 

Her kopyalama işi içinde, paralel sorgular çalıştırmak ve verileri bölümlere göre kopyalamak için veri bölümü ile [Parallelcopy ayarını](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#parallel-copy) kullanarak da bazı paralellik düzeyine ulaşabilirsiniz. Aşağıdaki ayrıntılarla seçilecek iki veri bölümü seçeneği vardır.
- Daha verimli olduğundan veri diliminizden başlamanız önerilir.  Parallelkopyalar ayarında paralellik sayısının, Netezza sunucusundaki tablonuzdaki toplam veri dilimi bölümlerinin altında olduğundan emin olun.  
- Her bir veri dilimi bölümünün birim boyutu hala büyükse (örneğin, 10 GB 'den büyük), Dinamik Aralık bölümüne geçmeniz önerilir, burada bölüm sayısını ve her bölüm için birimin boyutunu tanımlamanız için daha fazla esneklik elde edersiniz. Bölüm sütununa göre, üst sınır ve alt sınır.

Büyük tablolar için, birim boyutu 100 GB 'den büyük olduğunda veya 2 saat içinde Azure 'a geçirilemeyen bir durumda, verileri özel sorgu ile bölümleyip her bir kopya işinin aynı anda bir bölüm kopyalaması önerilir. Daha iyi üretilen iş için birden çok Azure Data Factory kopyalama işini eşzamanlı olarak çalıştırabilirsiniz. Her bir kopyalama işi hedefinin özel sorgu tarafından bir bölümü yüklemesi için, aktarım hızını artırmak için veri dilimi veya Dinamik Aralık aracılığıyla paralellik özelliğini etkinleştirmeyi unutmayın. 

Ağ veya veri deposu geçici sorunu nedeniyle kopyalama işlerinin herhangi biri başarısız olursa, ilgili bölümü tablodan yeniden yüklemek için başarısız kopyalama işini yeniden çalıştırabilirsiniz. Diğer bölümleri yükleyen diğer tüm kopyalama işleri etkilenmeyecektir.

Azure SQL veri ambarı 'na veri yüklerken, PolyBase 'in hazırlama sırasında Azure Blob depolama ile kopyalama işi içinde etkinleştirilmesi önerilir.

### <a name="delta-data-migration"></a>Delta verileri geçişi 

Tabloınızdan yeni veya güncelleştirilmiş satırları belirlemenin yolu, bir zaman damgası sütunu veya şema içindeki bir anahtarı artırılarak, son değeri, bir sonraki veri yüklemesi için Delta verileri filtrelemek üzere kullanılabilecek bir dış tabloda üst eşik olarak depolar. 

Farklı tablolar, yeni veya güncelleştirilmiş satırları tanımlamak için farklı bir filigran sütunu kullanabilir. Her bir satırın, belirli bir filigran sütun adı ve yüksek eşik değeri ile Netezza sunucusundaki bir tabloyu temsil ettiğini belirten bir dış denetim tablosu oluşturmanız önerilir. 

### <a name="self-hosted-integration-runtime-configuration-on-azure-vm-or-machine"></a>Azure VM veya makinesinde şirket içinde barındırılan tümleştirme çalışma zamanı yapılandırması

Netezza Server 'dan Azure 'a veri geçirdiğinizden bağımsız olarak, Netezza Server, şirket güvenlik duvarınız veya VNET ortamında herhangi bir taahhüdde yer alan hiçbir şekilde, Windows makinesinde veya VM 'de, taşınacak motor olan şirket içinde barındırılan tümleştirme çalışma zamanı yüklemeniz gerekir verileri.

- Her makine veya VM için ile başlamak için önerilen yapılandırma, 32 vCPU ve 128 GB bellektir. Veri geçişi sırasında IR makinesinin izleme CPU ve bellek kullanımını, daha iyi performans için makineye daha fazla ölçeklendirme yapmanız gerektiğini ve maliyeti kazanmak için makinenin ölçeğini ölçeklendirmeniz gerektiğini görmek üzere tutabilirsiniz.
- Ayrıca, tek bir kendinden konak IR ile 4 ' e kadar düğüm ilişkilendirerek da ölçeği genişletebilirsiniz. Şirket içinde barındırılan bir IR 'ye karşı çalışan tek bir kopyalama işi, verileri paralel olarak kopyalamak için tüm VM düğümlerinden otomatik olarak yararlanır. Yüksek kullanılabilirlik için, veri geçişi sırasında tek hata noktasını önlemek üzere 2 VM düğümü ile başlamanız önerilir.

### <a name="rate-limiting"></a>Hız sınırlandırma

En iyi uygulama olarak, her kopyalama etkinliği için uygun bir bölüm boyutunu belirleyebilmeniz için, temsili örnek veri kümesiyle bir performans POC 'si gerçekleştirin. Her bölümün Azure 'a 2 saat içinde yüklenmesini öneririz.  

Bir tabloyu kopyalamak için tek bir şirket içinde barındırılan IR makinesiyle tek bir kopyalama etkinliği ile başlayın. Tablonuzdaki veri dilimi bölümlerinin sayısına bağlı olarak Parallelcopy ayarını kademeli olarak artırın ve kopyalama işinden gördüğünüz aktarım hızına göre tüm tablonun 2 saat içinde Azure 'a yüklenip yüklenememesine bakın. 

Sağlanamadıysanız ve aynı zamanda şirket içinde barındırılan IR düğümünün ve veri deposunun kapasitesi tam olarak kullanılmadığından, ağınızın sınırlarına veya veri depolarının bant genişliği sınırına ulaşana kadar eşzamanlı kopyalama etkinliği sayısını kademeli olarak artırın. 

Şirket içinde barındırılan IR makinesinde CPU/bellek kullanımının izlenmesini ve CPU/belleğin tam olarak kullanıldığını gördüğünüzde makinenin ölçeğini ölçeklendirmeye veya birden çok makineye ölçeğe hazır olmaya devam edin. 

Azure Data Factory kopyalama etkinliği tarafından bildirilen azaltma hatalarıyla karşılaştığınızda, Azure Data Factory eşzamanlılık veya Parallelcopy ayarını azaltın ya da ağ ve veri depolarının bant genişliği/ıOPS sınırlarını artırmayı düşünün. 


### <a name="estimating-price"></a>Fiyat tahmini 

Şirket içi Netezza sunucusundan Azure SQL veri ambarı 'na veri geçirmek için oluşturulan aşağıdaki işlem hattını göz önünde bulundurun:

![fiyatlandırma-işlem hattı](media/data-migration-guidance-netezza-azure-sqldw/pricing-pipeline.png)

Şunları kabul edelim: 

- Toplam veri hacmi 50 TB 'tır. 
- İlk çözüm mimarisini kullanarak verileri geçirme (Netezza Server, güvenlik duvarının arkasındaki şirket içi)
- 50 TB, 500 bölüme bölünmüştür ve her kopyalama etkinliği bir bölüm taşıyor.
- Her kopyalama etkinliği, 4 makineye karşı şirket içinde barındırılan bir IR ile yapılandırılır ve 20 MBps aktarım hızına erişir. (Kopyalama etkinliği içinde Parallelcopy 'lar 4 olarak ayarlanır ve tablodaki verileri yüklemeye yönelik her iş parçacığı 5 MB/sn 'Lik aktarım hızına erişir)
- ForEach eşzamanlılık 3 olarak ayarlanır ve toplam verimlilik 60 MB/sn 'dir.
- Toplamda, geçişin tamamlandığı 243 saat sürer.

Yukarıdaki varsayımlar temelinde tahmini fiyat aşağıda verilmiştir: 

![fiyatlandırma-tablo](media/data-migration-guidance-netezza-azure-sqldw/pricing-table.png)

> [!NOTE]
> Bu bir kuramsal fiyatlandırma örneğidir. Gerçek fiyatlandırağınız, ortamınızdaki gerçek işleme göre değişir. Windows makinenin fiyatı (Şirket içinde barındırılan tümleştirme çalışma zamanı yüklü) dahil değildir. 

### <a name="additional-references"></a>Ek başvurular 
- [Azure Data Factory kullanarak şirket içi ilişkisel veri ambarından Azure 'a veri taşıma](https://azure.microsoft.com/mediahandler/files/resourcefiles/data-migration-from-on-premise-relational-data-warehouse-to-azure-data-lake-using-azure-data-factory/Data_migration_from_on-prem_RDW_to_ADLS_using_ADF.pdf)
- [Netezza Bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-netezza)
- [ODBC Bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-odbc)
- [Azure Blob Depolama Bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
- [Azure Data Lake Storage 2. Bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
- [Azure SQL veri ambarı Bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse)
- [Kopyalama etkinliği performans ayarlama Kılavuzu](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)
- [Şirket içinde barındırılan Integration Runtime oluşturma ve yapılandırma](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)
- [Şirket içinde barındırılan tümleştirme çalışma zamanı HA ve ölçeklenebilirliği](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [Veri taşıma güvenlik konuları](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)
- [Azure Key Vault kimlik bilgilerini depolama](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
- [Verileri bir tablodan artımlı olarak kopyalama](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-portal)
- [Verileri birden çok tablodan artımlı olarak kopyalama](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-multiple-tables-portal)
- [Azure Data Factory fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory ile birden çok kapsayıcıdan dosya kopyalama](solution-template-copy-files-multiple-containers.md)