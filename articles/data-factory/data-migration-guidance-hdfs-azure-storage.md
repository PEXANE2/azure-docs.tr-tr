---
title: Şirket içi Hadoop kümesinden Azure Storage 'a veri geçirmek için Azure Data Factory kullanma | Microsoft Docs
description: Şirket içi Hadoop kümesinden Azure Storage 'a veri geçirmek için Azure Data Factory kullanın.
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
ms.date: 8/30/2019
ms.openlocfilehash: 1b26b22fa9cdf9f6671702ceb9640aa39a6ecf17
ms.sourcegitcommit: 8fea78b4521921af36e240c8a92f16159294e10a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2019
ms.locfileid: "70211603"
---
# <a name="use-azure-data-factory-to-migrate-data-from-on-premises-hadoop-cluster-to-azure-storage"></a>Şirket içi Hadoop kümesinden Azure Storage 'a veri geçirmek için Azure Data Factory kullanma 

Azure Data Factory, verileri Şirket içinden Azure Blob depolama alanına veya Azure Data Lake Storage 2. göre ölçeklendirmeye, sağlam ve uygun maliyetli bir mekanizma sağlar. Temel olarak Azure Data Factory, verileri Şirket içinden Azure 'a geçirmek için iki yaklaşım içerir. Senaryonuza göre bunların her birini seçebilirsiniz. 

- ADF DistCp modu. ADF, dosyaları Azure Blob ( [hazırlanan kopya](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy)dahil) veya Azure Data Lake Store olarak kopyalamak Için [distcp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) 'yi kullanarak, ADF Self-barındırılan TÜMLEŞTIRME çalışma zamanı (IR) üzerinde çalıştırmak yerine kümenizin gücünden tam olarak yararlanabilen bir durumdur. Özellikle kümeniz çok güçlü olduğunda daha iyi kopyalama performansı sağlar. Azure Data Factory yapılandırmanızda yapılandırmanıza bağlı olarak, kopyalama etkinliği otomatik olarak bir DistCp komutu oluşturur, Hadoop kümenize gönderir ve kopyalama durumunu izler. DistCp ile tümleşik ADF 'yi kullanarak müşteri, en iyi kopyalama verimini elde etmek için mevcut güçlü kümeinizden yararlanamaz ve ayrıca, ADF 'den esnek zamanlama ve Birleşik izleme deneyimi avantajını elde edebilir. Varsayılan olarak, ADF DistCp modu, verileri şirket içi Hadoop kümesinden Azure 'a geçirmek için önerilen yoldur.
- ADF yerel IR modu. Bazı senaryolarda, (örneğin, VNET ortamında, DistCp Aracı, Express Route özel eşlemesi + Azure Storage VNet uç noktası) için kullanılamaz. Bunun yanı sıra, mevcut Hadoop kümenizi kümenize daha fazla yük getirecek ve bu yana mevcut ETL işlerinin performansını etkileyebilecek şekilde, verileri geçirmek için motor olarak kullanmak istemezsiniz. Bu durumda, ADF tümleştirme çalışma zamanı (IR), verileri Şirket içinden Azure 'a kopyalamak için motor olabilecek ADF yerel özelliğini kullanabilirsiniz.

Bu makalede, veri mühendislerinin ve geliştiricilerin her iki yaklaşımının aşağıdaki bilgileri sağlanmaktadır:
> [!div class="checklist"]
> * Performans 
> * Esnekliği Kopyala
> * Ağ güvenliği
> * Üst düzey çözüm mimarisi 
> * En iyi uygulama uygulamaları  

## <a name="performance"></a>Performans

ADF DistCp modunda, üretilen iş, mevcut Hadoop kümenizin kapasitesini kullanan ayrı ayrı TCP aracı kullanımıyla aynıdır. DistCp (dağıtılmış kopya), büyük ölçekli ve küme içi kopyalama için kullanılan bir araçtır. MapReduce kullanarak dağıtımını, hata işlemeyi ve kurtarmayı ve raporlamayı etkiler. Bir dosya ve dizin listesini, her biri kaynak listesinde belirtilen dosyaların bir bölümünü kopyalayabileceği şekilde eşlemek için giriş olarak genişletir. DistCp ile tümleşik ADF 'yi kullanarak, ortamınız için veri taşıma aktarım hızını en üst düzeye çıkarmak için, ağ bant genişliğinizin yanı sıra depolama ıOPS ve bant genişliği ile tam olarak yararlanmak üzere işlem hatları oluşturabilirsiniz.  

ADF yerel IR modunda Ayrıca, otomatik olarak barındırılan IR makinesini el ile ölçeklendirerek veya ölçeğini genişletmek için ağ bant genişliğinizin yanı sıra depolama ıOPS ve bant genişliği ile veri taşıma aktarım hızını en üst düzeye çıkaran farklı düzeylerde paralellik sağlar. Şirket içinde barındırılan IR birden çok makine.

- Tek bir kopyalama etkinliği, ölçeklenebilir işlem kaynaklarından yararlanabilir. Şirket içinde barındırılan tümleştirme çalışma zamanı ile, makineyi el ile ölçeklendirebilir veya birden fazla makineye ([4 düğüme kadar](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)) ölçeklendirebilirsiniz ve tek bir kopyalama etkinliği dosya kümesini tüm düğümlerde bölümleyebilir. 
- Tek bir kopyalama etkinliği, birden çok iş parçacığı kullanarak veri deposundan okur ve yazar. 
- ADF denetim akışı, örneğin [her döngü için](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)kullanarak birden çok kopyalama etkinliğini paralel olarak başlatabilir. 

[Kopyalama etkinliği performans kılavuzlarından](https://docs.microsoft.com/azure/data-factory/copy-activity-performance) daha fazla ayrıntı edinebilirsiniz

## <a name="resilience"></a>Dayanıklılık

ADF DistCp modunda, farklı esnekliği seviyeleri elde etmek için farklı DistCp komut satırı parametrelerinden (örneğin,-ı, yoksayma sorunları;-güncelleştirme, kaynak dosya ve hedef dosya boyutu farklı olduğunda veri yaz) yararlanabilirsiniz.

ADF yerel IR modunda, tek bir kopyalama etkinliği çalıştırmasında, ADF, veri depolarında veya temel ağdaki belirli bir geçici başarısızlık düzeyini işleyebilmesi için yerleşik yeniden deneme mekanizmasına sahiptir. Şirket içi bir sunucudan blob 'a ve şirket içi olarak ADLS 2. 'e ikili kopyalama yaparken, ADF otomatik olarak büyük bir ölçüde işaret noktası gerçekleştirir. Bir kopyalama etkinliği çalıştırması başarısız olduysa veya zaman aşımına uğramışsa, sonraki yeniden denemelerde (> 1 sayısını yeniden denediğinizden emin olun), kopya baştan başlamak yerine son hata noktasından devam eder.

## <a name="network-security"></a>Ağ güvenliği 

ADF, varsayılan olarak şirket içi olarak Azure Blob Storage 'a veri aktarır veya HTTPS protokolü üzerinden şifreli bağlantı kullanarak Azure Data Lake Storage 2.  HTTPS, aktarım sırasında veri şifrelemesi sağlar ve gizlice dinleme ve ortadaki adam saldırıları önler. 

Alternatif olarak, verilerin genel Internet üzerinden aktarılmasını istemiyorsanız, verileri Azure Express Route aracılığıyla özel bir eşleme bağlantısı üzerinden aktararak daha yüksek bir güvenlik elde edebilirsiniz. Bu şekilde nasıl ulaşılabilecek aşağıda çözüm mimarisine bakın.

## <a name="solution-architecture"></a>Çözüm mimarisi

Verileri genel Internet üzerinden geçirme:

![çözüm-mimari-genel-ağ](media/data-migration-guidance-hdfs-to-azure-storage/solution-architecture-public-network.png)

- Bu mimaride, veriler genel Internet üzerinden HTTPS kullanılarak güvenli bir şekilde aktarılır. 
- ADF DistCp modunun ortak ağ ortamında kullanılması önerilir. Bunu yaparak, en iyi kopyalama aktarım hızını elde etmek için mevcut güçlü kümenizle yararlanamaz ve ayrıca, ADF 'den esnek zamanlama ve Birleşik izleme deneyimi avantajını elde edebilirsiniz.
- Hadoop kümenize Dıtcp komutu göndermek ve kopyalama durumunu izlemek için, şirket güvenlik duvarının arkasındaki bir Windows makinesine ADF Self-barındırılan tümleştirme çalışma zamanı 'nı yüklemeniz gerekir. Bu makine, veri taşıma altyapısı olmayacak (yalnızca denetim amacı için), makinenin kapasitesi veri hareketinin verimini etkilemez.
- DistCp komutunun mevcut parametreleri desteklenir.


Verileri özel bağlantı üzerinden geçir: 

![çözüm-mimari-özel-ağ](media/data-migration-guidance-hdfs-to-azure-storage/solution-architecture-private-network.png)

- Bu mimaride veri geçişi, Azure Express Route aracılığıyla özel bir eşleme bağlantısı üzerinden yapılır, böylece veriler hiçbir şekilde herkese açık Internet üzerinden geçmez.
- DistCp aracı Express Route özel eşlemesi + Azure Storage VNet uç noktasını desteklemez, bu nedenle verileri geçirmek için tümleştirme çalışma zamanı aracılığıyla ADF yerel özelliğini kullanmanız önerilir.
- Bu mimariye ulaşmak için Azure sanal ağınızdaki bir Windows sanal makinesine ADF şirket içinde barındırılan tümleştirme çalışma zamanı 'nı yüklemeniz gerekir. Ağınızı ve depolama ıOPS/bant genişliğinizi tam olarak kullanabilmeniz için sanal makinelerinizi el ile ölçeklendirebilir veya birden çok VM 'ye ölçeklendirebilirsiniz.
- Her bir Azure VM için (ADF şirket içinde barındırılan tümleştirme çalışma zamanı yüklü olan) yapılandırma önerilir. Standard_D32s_v3, 32 vCPU ve 128 GB bellek ile başlatılır. Veri geçişi sırasında sanal makinenin CPU ve bellek kullanımının izlenmesini, daha iyi performans için sanal makineyi daha iyi ölçeklendirmeniz veya maliyeti kazanmak için VM 'nin ölçeğini ölçeklendirmeniz gerektiğini görmek için izleyebilirsiniz.
- Ayrıca, tek bir şirket içinde barındırılan IR ile 4 VM düğümünü ilişkilendirerek da ölçeği genişletebilirsiniz. Şirket içinde barındırılan bir IR 'ye karşı çalışan tek bir kopyalama işi otomatik olarak dosya kümesini bölümleyip, dosyaları paralel olarak kopyalamak için tüm VM düğümlerinden faydalanır. Yüksek kullanılabilirlik için, veri geçişi sırasında tek hata noktasını önlemek üzere iki VM düğümüyle başlamanız önerilir.
- Bu mimari kullanılarak hem ilk anlık görüntü veri geçişi hem de Delta verileri geçişi elde edilebilir.


## <a name="implementation-best-practices"></a>En iyi uygulama uygulamaları 

### <a name="authentication-and-credential-management"></a>Kimlik doğrulama ve kimlik bilgisi yönetimi 

- Bu durumda, bir [Windows (Kerberos) veya anonim](https://docs.microsoft.com/azure/data-factory/connector-hdfs#linked-service-properties)olarak kimlik doğrulaması yapmak için kullanabilirsiniz. 
- Azure Blob depolamaya bağlanmak için birden çok kimlik doğrulama türü desteklenir.  [Azure kaynakları için yönetilen kimliklerin](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#managed-identity) kullanımı kesinlikle önerilir: Azure AD 'de otomatik olarak YÖNETILEN bir ADF tanımının üzerine inşa edilen, bağlantılı hizmet tanımında kimlik bilgileri sağlamadan işlem hatlarını yapılandırmanıza olanak tanır.  Alternatif olarak, [hizmet sorumlusu](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#service-principal-authentication), [paylaşılan erişim imzası](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication)veya [depolama hesabı anahtarı](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#account-key-authentication)kullanarak Azure Blob depolama alanında kimlik doğrulaması yapabilirsiniz. 
- Azure Data Lake Storage 2. bağlanmak için birden çok kimlik doğrulama türü de desteklenir.  [Hizmet sorumlusu](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication) veya [depolama hesabı anahtarı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#account-key-authentication) da kullanılabilir olsa da, [Azure kaynakları için yönetilen kimliklerin](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#managed-identity) kullanılması önemle önerilir. 
- Azure kaynakları için Yönetilen kimlikler kullanmıyorsanız, ADF bağlı hizmetleri değiştirmeden anahtarları merkezi olarak yönetmeyi ve döndürmeyi kolaylaştırmak için [Azure Key Vault kimlik bilgilerinin depolanması](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) kesinlikle önerilir.  Bu Ayrıca, [CI/CD için en iyi uygulamalardan](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd)biridir. 

### <a name="initial-snapshot-data-migration"></a>İlk anlık görüntü verilerini geçirme 

ADF DistCp modunda, ilk veri geçiş davranışını denetlemek için DistCp komutunu farklı parametrelerle göndermek üzere bir kopyalama etkinliği oluşturabilirsiniz. 

ADF yerel IR modunda, veri bölümü özellikle 10 TB 'den fazla veri geçirirken önerilir. Verileri bölümlemek için, 1 saat içinde Klasör adlarından yararlanın ve sonra her ADF kopyalama işi aynı anda bir klasör bölümünü kopyalayabilir. Daha iyi üretilen iş için birden çok ADF kopyalama işini eşzamanlı olarak çalıştırabilirsiniz.
Ağ veya veri deposu geçici sorunu nedeniyle kopyalama işlerinin herhangi biri başarısız olursa, belirli bir bölümü Ise 'den yeniden yüklemek için başarısız kopyalama işini yeniden çalıştırabilirsiniz. Diğer bölümleri yükleyen diğer tüm kopyalama işleri etkilenmeyecektir.

### <a name="delta-data-migration"></a>Delta verileri geçişi 

ADF DistCp modunda, Delta veri geçişine ulaşmak için "-güncelleştirme, kaynak dosya ve hedef dosyanın boyutu farklı olduğunda verileri yaz" ile uyumlu olmayan komut satırı parametrelerini kullanabilirsiniz.

ADF yerel IR modunda, yeni veya değiştirilmiş dosyaları, "Örneğin," dosya veya klasör adında zaman dilimi bilgileri ile bölümlenmiş olarak bölümlendiğinde zaman bölümlenmiş adlandırma kuralını kullanarak (örneğin,/yyyy/mm/dd/ dosya. csv), ardışık düzen, artımlı olarak hangi dosya/klasörlerin kopyalanacağını kolayca tanımlayabilir.
Alternatif olarak,,, Eğer,, eğer... Bu yöntem, ADF 'nin tüm dosyaları her zaman olarak taramasının yanı sıra, son değiştirilme zaman damgası belirli bir değerden daha büyük olan yeni ve güncelleştirilmiş dosyayı kopyalamasıdır. Bu durumda çok sayıda dosyanız varsa ilk dosya taramanın, filtre koşuluyla kaç dosya eşleşmesinden bağımsız olarak uzun sürdüğüne dikkat edin. Bu durumda, dosya taramanın paralel olarak gerçekleşebilmesi için ilk anlık görüntü geçişi için aynı bölümü kullanarak önce verileri bölümlenmesi önerilir.

### <a name="estimating-price"></a>Fiyat tahmini 

Azure Blob Storage 'a veri geçirmek için oluşturulan aşağıdaki işlem hattını göz önünde bulundurun: 

![fiyatlandırma-işlem hattı](media/data-migration-guidance-hdfs-to-azure-storage/pricing-pipeline.png)

Şunları kabul edelim: 

- Toplam veri hacmi 1 PB
- İkinci çözüm mimarisini kullanarak verileri geçirme (ADF yerel IR modu)
- 1 PB 1000 bölüme ayrılmıştır ve her bir kopya bir bölüm taşıdıkça
- Her kopyalama etkinliği, 4 makineyle ilişkili, şirket içinde barındırılan tek bir IR ile yapılandırılır ve 500 MBps aktarım hızına erişir.
- ForEach eşzamanlılık 4 olarak ayarlanır ve toplam verimlilik 2 GBps 'dir
- Toplamda, geçişin tamamlandığı 146 saat sürer.


Yukarıdaki varsayımlar temelinde tahmini fiyat aşağıda verilmiştir: 

![fiyatlandırma-tablo](media/data-migration-guidance-hdfs-to-azure-storage/pricing-table.png)

> [!NOTE]
> Bu bir kuramsal fiyatlandırma örneğidir.  Gerçek fiyatlandırağınız, ortamınızdaki gerçek işleme göre değişir.
> Azure Windows VM fiyatı (Şirket içinde barındırılan tümleştirme çalışma zamanı yüklü) dahil değildir.

### <a name="additional-references"></a>Ek başvurular 
- [HDFS Bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-hdfs)
- [Azure Blob Depolama Bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
- [Azure Data Lake Storage 2. Bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
- [Kopyalama etkinliği performans ayarlama Kılavuzu](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)
- [Şirket içinde barındırılan Integration Runtime oluşturma ve yapılandırma](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)
- [Şirket içinde barındırılan tümleştirme çalışma zamanı HA ve ölçeklenebilirliği](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [Veri taşıma güvenlik konuları](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)
- [Azure Key Vault kimlik bilgilerini depolama](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
- [Dosyayı, bölümlenmiş dosya adına göre artımlı olarak Kopyala](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-partitioned-file-name-copy-data-tool)
- [LastModifiedDate göre yeni ve değiştirilmiş dosyaları Kopyala](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-lastmodified-copy-data-tool)
- [ADF fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory ile birden çok kapsayıcıdan dosya kopyalama](solution-template-copy-files-multiple-containers.md)