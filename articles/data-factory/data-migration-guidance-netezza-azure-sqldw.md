---
title: Şirket içi Netezza sunucusundaki verileri Azure'a geçirme
description: Verileri şirket içi Netezza sunucusundan Azure'a geçirmek için Azure Veri Fabrikası'nı kullanın.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 9/03/2019
ms.openlocfilehash: 80c9929f37b4890387a7625f04db6ce3e37f0cdd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74922127"
---
# <a name="use-azure-data-factory-to-migrate-data-from-an-on-premises-netezza-server-to-azure"></a>Verileri şirket içi Netezza sunucusundan Azure'a geçirmek için Azure Veri Fabrikası'nı kullanın 

Azure Veri Fabrikası, verileri şirket içi Netezza sunucusundan Azure depolama hesabınıza veya Azure SQL Veri Ambarı veritabanınıza ölçekte geçirmek için performans, sağlam ve uygun maliyetli bir mekanizma sağlar. 

Bu makalede, veri mühendisleri ve geliştiriciler için aşağıdaki bilgiler sağlar:

> [!div class="checklist"]
> * Performans 
> * Esnekliği kopyalama
> * Ağ güvenliği
> * Üst düzey çözüm mimarisi 
> * Uygulama en iyi uygulamaları  

## <a name="performance"></a>Performans

Azure Veri Fabrikası, çeşitli düzeylerde paralellik sağlayan sunucusuz bir mimari sunar. Geliştiriciyseniz, bu, ortamınız için veri hareketi veri kullanımını en üst düzeye çıkarmak için hem ağ hem de veritabanı bant genişliğini tam olarak kullanmak için ardışık hatlar oluşturabileceğiniz anlamına gelir.

![Performans diyagramı](media/data-migration-guidance-netezza-azure-sqldw/performance.png)

Önceki diyagram aşağıdaki gibi yorumlanabilir:

- Tek bir kopyalama etkinliği ölçeklenebilir işlem kaynaklarından yararlanabilir. Azure Tümleştirme Çalışma Zamanı'nı kullandığınızda, her kopyalama etkinliği için sunucusuz bir şekilde [en fazla 256 DIUs](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#data-integration-units) belirtebilirsiniz. Kendi kendine barındırılan tümleştirme çalışma süresi (kendi kendine barındırılan IR) ile makineyi el ile ölçeklendirebilir veya birden çok makineye[(en fazla dört düğüm)](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)ölçeklendirebilirsiniz ve tek bir kopyalama etkinliği tüm düğümlere bölümlerini dağıtır. 

- Tek bir kopyalama etkinliği birden çok iş parçacığı kullanarak veri deposundan okur ve yazar. 

- Azure Veri Fabrikası denetim akışı paralel olarak birden çok kopyalama faaliyetini başlatabilir. Örneğin, her döngü [için](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)bir döngü kullanarak başlatabilirsiniz. 

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)

## <a name="resilience"></a>Esnek -lik

Azure Veri Fabrikası, tek bir kopyalama etkinliği çalışması içinde, veri depolarında veya temel ağdaki belirli düzeyde geçici hataları işlemesini sağlayan yerleşik bir yeniden deneme mekanizmasına sahiptir.

Azure Veri Fabrikası kopyalama etkinliği yle, kaynak ve lavabo veri depoları arasında veri kopyaladiğinizde, uyumsuz satırları işlemek için iki yolunuz vardır. Kopyalama etkinliğini iptal edebilir ve başarısız olabilir veya uyumsuz veri satırlarını atlayarak verilerin geri kalanını kopyalamaya devam edebilirsiniz. Ayrıca, hatanın nedenini öğrenmek için, uyumsuz satırları Azure Blob depolama veya Azure Veri Gölü Deposu'nda günlüğe kaydedebilir, veri kaynağındaki verileri düzeltebilir ve kopyalama etkinliğini yeniden deneyebilirsiniz.

## <a name="network-security"></a>Ağ güvenliği 

Varsayılan olarak, Azure Veri Fabrikası, Hypertext Transfer Protocol Secure (HTTPS) üzerinden şifreli bir bağlantı kullanarak verileri şirket içi Netezza sunucusundan bir Azure depolama hesabına veya Azure SQL Veri Ambarı veritabanına aktarmaktadır. HTTPS aktarım sırasında veri şifrelemesağlar ve gizlice dinlemeyi ve ortadaki adam saldırılarını önler.

Alternatif olarak, verilerin genel internet üzerinden aktarılmasını istemiyorsanız, Azure Express Route üzerinden özel bir izleme bağlantısı üzerinden veri aktarımı yaparak daha yüksek güvenlik elde edilmesine yardımcı olabilirsiniz. 

Bir sonraki bölümde daha yüksek güvenlik nasıl elde ediletilir.

## <a name="solution-architecture"></a>Çözüm mimarisi

Bu bölümde, verilerinizi geçirmenin iki yolu anlatılmaktadır.

### <a name="migrate-data-over-the-public-internet"></a>Verileri genel internet üzerinden geçirme

![Verileri genel internet üzerinden geçirme](media/data-migration-guidance-netezza-azure-sqldw/solution-architecture-public-network.png)

Önceki diyagram aşağıdaki gibi yorumlanabilir:

- Bu mimaride, genel internet üzerinden HTTPS'yi kullanarak güvenli bir şekilde veri aktarımı yapabilirsiniz.

- Bu mimariye ulaşmak için, kurumsal güvenlik duvarının arkasındaki Windows makinesine Azure Veri Fabrikası tümleştirme çalışma süresini (kendi kendine barındırılan) yüklemeniz gerekir. Bu tümleştirme çalışma zamanının Netezza sunucusuna doğrudan erişebilmesini unutmayın. Verileri kopyalamak için ağınızı ve veri depolarınızı tam olarak kullanmak için makinenizi el ile ölçeklendirebilir veya birden çok makineye ölçeklendirebilirsiniz.

- Bu mimariyi kullanarak, hem ilk anlık görüntü verilerini hem de delta verilerini geçirebilirsiniz.

### <a name="migrate-data-over-a-private-network"></a>Verileri özel bir ağ üzerinden geçirme 

![Verileri özel bir ağ üzerinden geçirme](media/data-migration-guidance-netezza-azure-sqldw/solution-architecture-private-network.png)

Önceki diyagram aşağıdaki gibi yorumlanabilir:

- Bu mimaride, verileri Azure Express Rotası üzerinden özel bir bakış bağlantısı üzerinden aktarırsınız ve veriler hiçbir zaman genel internet üzerinden geçkalmaz. 

- Bu mimariye ulaşmak için Azure sanal ağınızdaki Bir Windows sanal makinesine (VM) Azure Veri Fabrikası tümleştirme çalışma süresini (kendi kendine barındırılan) yüklemeniz gerekir. Verileri kopyalamak için ağınızı ve veri depolarınızı tam olarak kullanmak için VM'nizi el ile ölçeklendirebilir veya birden çok VM'ye ölçeklendirebilirsiniz.

- Bu mimariyi kullanarak, hem ilk anlık görüntü verilerini hem de delta verilerini geçirebilirsiniz.

## <a name="implement-best-practices"></a>En iyi uygulamaları uygulayın 

### <a name="manage-authentication-and-credentials"></a>Kimlik doğrulamayı ve kimlik bilgilerini yönetme 

- Netezza'ya kimlik doğrulamak için [bağlantı dizesi aracılığıyla ODBC kimlik doğrulamasını](https://docs.microsoft.com/azure/data-factory/connector-netezza#linked-service-properties)kullanabilirsiniz. 

- Azure Blob depolama alanına kimlik doğrulamak için: 

   - Azure kaynakları [için yönetilen kimlikleri](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#managed-identity)kullanmanızı şiddetle öneririz. Azure Etkin Dizini'nde (Azure AD) otomatik olarak yönetilen bir Azure Veri Fabrikası kimliğinin üzerine inşa edilen yönetilen kimlikler, Bağlantılı Hizmet tanımında kimlik bilgileri sağlamak zorunda kalmadan ardışık hatları yapılandırmanıza olanak tanır.  

   - Alternatif olarak, [hizmet ilkesini,](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#service-principal-authentication) [paylaşılan erişim imzasını](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication)veya [depolama hesabı anahtarını](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#account-key-authentication)kullanarak Azure Blob depolama alanına kimlik doğrulayabilirsiniz. 

- Azure Veri Gölü Depolama Gen2'de kimlik doğrulaması yapmak için: 

   - Azure kaynakları [için yönetilen kimlikleri](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#managed-identity)kullanmanızı şiddetle öneririz.
   
   - Ayrıca hizmet [sorumlusu](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication) veya [depolama hesabı anahtarı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#account-key-authentication)kullanabilirsiniz. 

- Azure SQL Veri Ambarı'nda kimlik doğrulaması yapmak için:

   - Azure kaynakları [için yönetilen kimlikleri](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#managed-identity)kullanmanızı şiddetle öneririz.
   
   - Ayrıca hizmet [anapara](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#service-principal-authentication) veya [SQL kimlik doğrulaması](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#sql-authentication)kullanabilirsiniz.

- Azure kaynakları için yönetilen kimlikler kullanmıyorsanız, Azure Veri Fabrikası bağlantılı hizmetlerini değiştirmek zorunda kalmadan tuşları merkezi olarak yönetmeyi ve döndürmeyi kolaylaştırmak için [kimlik bilgilerini Azure Key Vault'ta saklamanızı](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) öneririz. Bu aynı zamanda [CI / CD için en iyi uygulamalardan](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd)biridir. 

### <a name="migrate-initial-snapshot-data"></a>İlk anlık görüntü verilerini geçirme 

Küçük tablolar için (diğer bir süre, hacmi 100 GB'dan az olan veya iki saat içinde Azure'a geçirilebilen tablolar) için, her bir kopya iş yükü verilerini tablo başına yapabilirsiniz. Daha fazla iş için, ayrı tabloları aynı anda yüklemek için birden çok Azure Veri Fabrikası kopya işleri çalıştırabilirsiniz. 

Paralel sorguları çalıştırmak ve verileri bölümlere kopyalamak için her kopyalama işinde, aşağıdaki veri bölümleme seçeneklerinden biriyle [ `parallelCopies` özellik ayarını](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#parallel-copy) kullanarak paralellik düzeyine de ulaşabilirsiniz:

- Daha fazla verimlilik elde etmeye yardımcı olmak için, bir veri diliminden başlamanızı öneririz.  `parallelCopies` Ayarda değeri Netezza sunucusunda tablonuzdaki toplam veri dilimi bölüm lerinin toplam sayısından daha az olduğundan emin olun.  

- Her veri dilimi bölümünün hacmi hala büyükse (örneğin, 10 GB veya daha büyükse), dinamik aralık bölümüne geçmenizi öneririz. Bu seçenek, bölüm sayısını ve her bölümün hacmini bölüm sütunu, üst sınır ve alt sınır ile tanımlamak için daha fazla esneklik sağlar.

Daha büyük tablolar için (diğer bir şekilde, hacmi 100 GB veya üzerinde olan veya iki saat içinde *Azure'a geçirilmeyen* tablolar), verileri özel sorguya göre bölmenizi ve ardından her kopya-iş kopyasını aynı anda bir bölüm yapmanızı öneririz. Daha iyi bir iş için, birden çok Azure Veri Fabrikası kopyalama işini aynı anda çalıştırabilirsiniz. Özel sorguyla bir bölüm yüklemeyi hedefleyen her kopyalama işi hedefi için, veri dilimi veya dinamik aralık üzerinden paralellik sağlayarak iş verimini artırabilirsiniz. 

Herhangi bir kopyalama işi bir ağ veya veri deposu geçici sorunu nedeniyle başarısız olursa, tablodan belirli bir bölümü yeniden yüklemek için başarısız kopyalama işini yeniden çalıştırabilirsiniz. Diğer bölümleri yükleyen diğer kopyalama işleri etkilenmez.

Verileri Bir Azure SQL Veri Ambarı veritabanına yüklediğinizde, Düzenleme olarak Azure Blob depolama alanıyla kopyalama işinde PolyBase'i etkinleştirmenizi öneririz.

### <a name="migrate-delta-data"></a>Delta verilerini geçirin 

Tablonuzdaki yeni veya güncelleştirilmiş satırları tanımlamak için şema içinde bir zaman damgası sütunu veya artış anahtarı kullanın. Daha sonra dış tabloda yüksek filigran olarak en son değeri saklayabilir ve sonra verileri bir sonraki yüklediğinizde delta verilerini filtrelemek için kullanabilirsiniz. 

Her tablo, yeni veya güncelleştirilmiş satırlarını tanımlamak için farklı bir filigran sütunu kullanabilir. Harici bir kontrol tablosu oluşturmanızı öneririz. Tabloda, her satır netezza sunucusunda belirli filigran sütun adı ve yüksek filigran değeri ile bir tablo temsil eder. 

### <a name="configure-a-self-hosted-integration-runtime"></a>Kendi kendine barındırılan tümleştirme çalışma süresini yapılandırma

Verileri Netezza sunucusundan Azure'a aktarıyorsanız, sunucu ister şirket güvenlik duvarınızın arkasında ister sanal ağ ortamında olsun, windows makinesine veya VM'ye kendi kendine barındırılan bir IR yüklemeniz gerekir, bu da kullanılan motordur verileri taşıyın. Kendi kendine barındırılan IR'yi yüklerken, aşağıdaki yaklaşımı öneririz:

- Her Windows makinesi veya VM için 32 vCPU ve 128 GB bellek yapılandırmasıyla başlayın. Veri geçişi sırasında IR makinesinin CPU ve bellek kullanımını izlemeye devam ederek, daha iyi performans için makineyi daha fazla ölçeklendirmeniz mi yoksa maliyetten tasarruf etmek için makineyi küçültmeniz mi gerektiğini görebilirsiniz.

- Ayrıca, kendi kendine barındırılan tek bir IR ile en fazla dört düğüm ilişkilendirerek ölçeklendirebilirsiniz. Kendi barındırılan BIR IR'ye karşı çalışan tek bir kopyalama işi, verileri paralel olarak kopyalamak için tüm VM düğümlerini otomatik olarak uygular. Yüksek kullanılabilirlik için, veri geçişi sırasında tek bir hata noktasını önlemek için dört VM düğümüyle başlayın.

### <a name="limit-your-partitions"></a>Bölümlerinizi sınırlandırın

En iyi uygulama olarak, her kopyalama etkinliği için uygun bir bölüm boyutu belirleyebilmeniz için temsili bir örnek veri kümesiyle bir performans kanıtı (POC) gerçekleştirin. Her bölümü iki saat içinde Azure'a yüklemenizi öneririz.  

Bir tabloyu kopyalamak için, tek bir kopya etkinliğiyle, kendi kendine barındırılan tek bir IR makinesiyle başlayın. Tablonuzdaki `parallelCopies` veri dilimi bölümlerinin sayısına bağlı olarak ayarı kademeli olarak artırın. Kopyalama işinden elde edilen iş geleni göre, tablonun tamamının iki saat içinde Azure'a yüklenebiliyor mu olduğunu görün. 

İki saat içinde Azure'a yüklenemezse ve kendi barındırılan IR düğümü ve veri deposunun kapasitesi tam olarak kullanılmazsa, ağınızın sınırına veya verilerin bant genişliği sınırına ulaşana kadar eşzamanlı kopyalama etkinliklerinin sayısını kademeli olarak artırın Mağaza. 

CPU ve bellek kullanımını kendi kendine barındırılan IR makinesinde izlemeye devam edin ve CPU ve belleğin tam olarak kullanıldığını gördüğünüzde makineyi büyütmeye veya birden fazla makineye ölçeklendirmeye hazır olun. 

Azure Veri Fabrikası kopyalama etkinliği tarafından bildirilen azaltma hatalarıyla karşılaştığınızda, Azure `parallelCopies` Veri Fabrikası'ndaki eşzamanlılığı veya ayarı azaltın veya ağ ve veri depolarının saniyebaşına bant genişliği veya G/Ç işlemlerini (IOPS) sınırlamalarını artırmayı düşünün. 


### <a name="estimate-your-pricing"></a>Fiyatlandırmanızı tahmin edin 

Şirket içi Netezza sunucusundan azure SQL Veri Ambarı veritabanına veri geçirmek için oluşturulmuş aşağıdaki ardışık yapıyı göz önünde bulundurun:

![Fiyatlandırma boru hattı](media/data-migration-guidance-netezza-azure-sqldw/pricing-pipeline.png)

Aşağıdaki ifadelerin doğru olduğunu varsayalım: 

- Toplam veri hacmi 50 terabayt (TB) 'dir. 

- İlk çözüm mimarisini kullanarak verileri aktarıyoruz (Netezza sunucusu güvenlik duvarının arkasında şirket içindedir).

- 50-TB birim 500 bölüme ayrılır ve her kopyalama etkinliği bir bölüm taşır.

- Her kopyalama etkinliği, dört makineye karşı kendi kendine barındırılan bir IR ile yapılandırılır ve saniyede 20 megabayt (MBp) bir iş elde eder. (Kopyalama etkinliği `parallelCopies` içinde, 4 olarak ayarlanır ve tablodan veri yüklemek için her iş parçacığı 5-MBps iş bir elde eder.)

- ForEach eşzamanlılık 3 olarak ayarlanır ve toplam iş toplamı 60 MBps'dir.

- Göçü tamamlamak toplamda 243 saat sürüyor.

Önceki varsayımlara dayanarak, tahmini fiyat aşağıda verilmiştir: 

![Fiyatlandırma tablosu](media/data-migration-guidance-netezza-azure-sqldw/pricing-table.png)

> [!NOTE]
> Önceki tabloda gösterilen fiyatlandırma varsayımsal. Gerçek fiyatlandırmanız ortamınızdaki gerçek üretim başına bağlıdır. Windows makinesinin fiyatı (kendi kendine barındırılan IR yüklü) dahil değildir. 

### <a name="additional-references"></a>Ek başvurular

Daha fazla bilgi için aşağıdaki makalelere ve kılavuzlara bakın:

- [Azure Veri Fabrikası'nı kullanarak şirket içi ilişkisel Veri Ambarı veritabanındaki verileri Azure'a geçirme](https://azure.microsoft.com/resources/data-migration-from-on-premise-relational-data-warehouse-to-azure-data-lake-using-azure-data-factory/)
- [Netezza konektörü](https://docs.microsoft.com/azure/data-factory/connector-netezza)
- [ODBC bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-odbc)
- [Azure Blob depolama konektörü](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
- [Azure Data Lake Storage 2. Nesil bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
- [Azure SQL Veri Ambarı bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse)
- [Etkinlik performans atonlama kılavuzunu kopyalama](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)
- [Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma ve yapılandırma](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)
- [Kendi kendine barındırılan entegrasyon çalışma süresi HA ve ölçeklenebilirlik](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [Veri taşırken güvenlikle ilgili dikkat edilmesi gerekenler](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)
- [Kimlik bilgilerini Azure Key Vault’ta depolama](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
- [Verileri tek bir tablodan artımlı olarak kopyalama](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-portal)
- [Verileri birden çok tablodan artımlı olarak kopyalama](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-multiple-tables-portal)
- [Azure Veri Fabrikası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Fabrikası'nı kullanarak dosyaları birden çok kapsayıcıdan kopyalama](solution-template-copy-files-multiple-containers.md)
