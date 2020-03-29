---
title: Şirket içi Hadoop kümesinden verileri Azure Depolama'ya geçirme
description: Şirket içi Hadoop kümesinden Azure Depolama'ya veri geçirmek için Azure Veri Fabrikası'nı nasıl kullanacağınızı öğrenin.
services: data-factory
ms.author: yexu
author: dearandyxu
ms.reviewer: ''
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 8/30/2019
ms.openlocfilehash: afccbdbbfd5b8ddeefa621448d6170d937b518f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931442"
---
# <a name="use-azure-data-factory-to-migrate-data-from-an-on-premises-hadoop-cluster-to-azure-storage"></a>Verileri şirket içi Hadoop kümesinden Azure Depolama'ya geçirmek için Azure Veri Fabrikası'nı kullanın 

Azure Veri Fabrikası, şirket içi HDFS'den Azure Blob depolamasına veya Azure Veri Gölü Depolama Gen2'ye kadar ölçekte veri geçirmek için performans, sağlam ve uygun maliyetli bir mekanizma sağlar. 

Veri Fabrikası, şirket içi HDFS'den Azure'a veri geçirmek için iki temel yaklaşım sunar. Senaryonuza göre yaklaşımı seçebilirsiniz. 

- **Veri Fabrikası DistCp modu** (önerilir): Veri Fabrikası'nda, Dosyaları Azure Blob depolamasına (aşamalı kopya dahil) [(aşamalı kopya](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy)dahil) veya Azure Data Lake Store Gen2'ye olduğu gibi kopyalamak için [DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) (dağıtılmış kopya) kullanabilirsiniz. En iyi kopya verisini elde etmek için varolan güçlü bir kümeden yararlanmak için DistCp ile tümleşik Veri Fabrikası'nı kullanın. Ayrıca, Veri Fabrikası'ndan esnek zamanlama ve birleşik bir izleme deneyimi nden de yararlanırsınız. Veri Fabrikası yapılandırmanıza bağlı olarak, kopyalama etkinliği otomatik olarak bir DistCp komutu oluşturuyor, verileri Hadoop kümenize gönderir ve ardından kopyalama durumunu izler. Şirket içi Hadoop kümesinden Azure'a veri geçirmek için Veri Fabrikası DistCp modunu öneririz.
- **Veri Fabrikası yerel tümleştirme çalışma zamanı modu**: DistCp tüm senaryolarda bir seçenek değildir. Örneğin, Bir Azure Sanal Ağlar ortamında, DIsCp aracı Azure Depolama sanal ağ bitiş noktasıyla Azure ExpressRoute özel eşlemi desteklemez. Buna ek olarak, bazı durumlarda, varolan Hadoop kümenizi veri geçirmek için bir motor olarak kullanmak istemezsiniz, böylece kümenize ağır yükler yüklemezsiniz ve bu da varolan ETL işlerinin performansını etkileyebilir. Bunun yerine, veri fabrikası tümleştirme çalışma zamanının yerel yeteneğini, şirket içi HDFS'den Azure'a verileri kopyalayan altyapı olarak kullanabilirsiniz.

Bu makalede, her iki yaklaşım hakkında aşağıdaki bilgiler verilmektedir:
> [!div class="checklist"]
> * Performans 
> * Esnekliği kopyalama
> * Ağ güvenliği
> * Üst düzey çözüm mimarisi 
> * Uygulama en iyi uygulamaları  

## <a name="performance"></a>Performans

Veri Fabrikası DistCp modunda, iş tükenir, DistCp aracını bağımsız olarak kullanıyormuşsunuz gibi aynıdır. Veri Fabrikası DistCp modu, mevcut Hadoop kümenizin kapasitesini en üst düzeye çıkarır. Büyük kümeler arası veya küme içi kopyalama için DistCp'yi kullanabilirsiniz. 

DistCp, dağıtımını, hata işlemesini ve kurtarmasını ve raporlamasını etkilemek için MapReduce'i kullanır. Görev eşleme için girilen dosyalar ve dizinler listesini genişletir. Her görev, kaynak listesinde belirtilen bir dosya bölümlerini kopyalar. Ortamınız için veri hareketi veri matını en üst düzeye çıkarmak için ağ bant genişliğinizi, depolama IOPS'nizi ve bant genişliğinizi tam olarak kullanmak için veri kanallarını oluşturmak için DistCp ile tümleştirilmiş Veri Fabrikası'nı kullanabilirsiniz.  

Veri Fabrikası yerel tümleştirme çalışma zamanı modu da farklı düzeylerde paralellik sağlar. Veri hareketi veri verisini en üst düzeye çıkarmak için ağ bant genişliğinizi, depolama IOPS'unuzu ve bant genişliğinizi tam olarak kullanmak için paralellik kullanabilirsiniz:

- Tek bir kopyalama etkinliği ölçeklenebilir işlem kaynaklarından yararlanabilir. Kendi kendine barındırılan tümleştirme çalışma süresiyle, makineyi el ile ölçeklendirebilir veya birden çok makineye[(en fazla dört düğüm)](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)ölçeklendirebilirsiniz. Tek bir kopyalama etkinliği, dosya kümesini tüm düğümler arasında bölümlere ayırır. 
- Tek bir kopyalama etkinliği birden çok iş parçacığı kullanarak veri deposundan okur ve yazar. 
- Veri Fabrikası denetim akışı paralel olarak birden çok kopyalama faaliyeti başlatabilir. Örneğin, Her biri [için](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)bir döngü kullanabilirsiniz. 

Daha fazla bilgi için [kopyalama etkinliği performans kılavuzuna](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)bakın.

## <a name="resilience"></a>Esnek -lik

Veri Fabrikası DistCp modunda, farklı esneklik düzeyleri için farklı DistCp komut satırı parametrelerini (Örneğin, `-i`hataları yoksay veya, `-update`kaynak dosya ve hedef dosyası boyutu farklı olduğunda veri yazmak) kullanabilirsiniz.

Veri Fabrikası yerel tümleştirme çalışma zamanı modunda, tek bir kopyalama etkinliği çalışmasında, Veri Fabrikası'nın yerleşik yeniden deneme mekanizması vardır. Veri depolarında veya altta yatan ağda belirli bir düzeyde geçici hataları işleyebilir. 

Şirket içi HDFS'den Blob depolamasına ve şirket içi HDFS'den Data Lake Store Gen2'ye ikili kopyalama yaparken, Veri Fabrikası büyük ölçüde kontrol noktalarını otomatik olarak gerçekleştirir. Bir kopya etkinliği çalışması başarısız olursa veya zaman dışarı, sonraki bir yeniden denemede (yeniden deneme sayısının > olduğundan emin olun), kopya başlangıçta başlamak yerine son hata noktasından devam eder.

## <a name="network-security"></a>Ağ güvenliği 

Varsayılan olarak, Veri Fabrikası verileri şirket içi HDFS'den Blob depolama ya da Azure Veri Gölü Depolama Gen2'ye HTTPS protokolü üzerinden şifreli bir bağlantı kullanarak aktarır. HTTPS aktarım sırasında veri şifrelemesağlar ve gizlice dinlemeyi ve ortadaki adam saldırılarını önler. 

Alternatif olarak, daha yüksek güvenlik için verilerin genel internet üzerinden aktarılmasını istemiyorsanız, ExpressRoute üzerinden özel bir izleme bağlantısı üzerinden veri aktarımı yapabilirsiniz. 

## <a name="solution-architecture"></a>Çözüm mimarisi

Bu resim, genel internet üzerinden veri geçiş görüntüler:

![Genel ağ üzerinden veri geçirmek için çözüm mimarisini gösteren diyagram](media/data-migration-guidance-hdfs-to-azure-storage/solution-architecture-public-network.png)

- Bu mimaride, veriler genel internet üzerinden HTTPS kullanılarak güvenli bir şekilde aktarılır. 
- Genel ağ ortamında Veri Fabrikası DistCp modunu kullanmanızı öneririz. En iyi kopya iş masını elde etmek için varolan güçlü bir kümeden yararlanabilirsiniz. Ayrıca Veri Fabrikası'ndan esnek zamanlama ve birleşik izleme deneyiminden de yararlanırsınız.
- Bu mimari için, DistCp komutunu Hadoop kümenize göndermek ve kopyalama durumunu izlemek için, şirket güvenlik duvarının arkasındaki Windows makinesine Veri Fabrikası'nın kendi barındırılan tümleştirme çalışma zamanını yüklemeniz gerekir. Makine verileri taşıyacak motor olmadığından (yalnızca kontrol amacıyla), makinenin kapasitesi veri hareketinin hacmini etkilemez.
- DistCp komutundan varolan parametreler desteklenir.

Bu resim, özel bir bağlantı üzerinden veri geçişgörüntüler: 

![Özel bir ağ üzerinden veri geçirmek için çözüm mimarisini gösteren diyagram](media/data-migration-guidance-hdfs-to-azure-storage/solution-architecture-private-network.png)

- Bu mimaride, veriler Azure ExpressRoute üzerinden özel bir izleme bağlantısı üzerinden geçirilir. Veriler hiçbir zaman genel internet üzerinden geçmeyecek.
- DistCp aracı, Azure Depolama sanal ağ bitiş noktasıyla ExpressRoute özel eşlemi desteklemez. Verileri geçirmek için tümleştirme çalışma süresi boyunca Veri Fabrikası'nın yerel yeteneğini kullanmanızı öneririz.
- Bu mimari için, Azure sanal ağınızdaki bir Windows VM'de Veri Fabrikası'nın kendi kendine barındırılan tümleştirme çalışma süresini yüklemeniz gerekir. Ağınızı ve depolama IOPS'unuzu veya bant genişliğinizi tam olarak kullanmak için VM'nizi el ile ölçeklendirebilir veya birden çok VM'ye ölçeklendirebilirsiniz.
- Her Azure VM için başlatılması önerilen yapılandırma (Veri Fabrikası'nın kendi kendine barındırılan tümleştirme çalışma süresi yüklü) 32 vCPU ve 128 GB belleğe sahip Standard_D32s_v3. Daha iyi performans için VM'yi büyütmeniz mi yoksa maliyeti azaltmak için VM'yi küçültmeniz gerekip gerekmediğini görmek için veri geçişi sırasında VM'nin CPU ve bellek kullanımını izleyebilirsiniz.
- Ayrıca, tek bir kendi kendine barındırılan tümleştirme çalışma süresiyle en fazla dört VM düğümü ilişkilendirerek ölçeklendirebilirsiniz. Kendi kendine barındırılan tümleştirme çalışma süresine karşı çalışan tek bir kopyalama işi, dosya kümesini otomatik olarak bölümlere ayırır ve dosyaları paralel kopyalamak için tüm VM düğümlerini kullanır. Yüksek kullanılabilirlik için, veri geçişi sırasında tek bir hata noktası senaryosundan kaçınmak için iki VM düğümüyle başlamanızı öneririz.
- Bu mimariyi kullandığınızda, ilk anlık görüntü veri geçişi ve delta veri geçişi kullanılabilir.

## <a name="implementation-best-practices"></a>Uygulama en iyi uygulamaları

Veri geçişinizi uygularken bu en iyi uygulamaları izlemenizi öneririz.

### <a name="authentication-and-credential-management"></a>Kimlik doğrulama ve kimlik bilgisi yönetimi 

- HDFS'ye kimlik doğrulamak için [Windows (Kerberos) veya Anonymous'u](https://docs.microsoft.com/azure/data-factory/connector-hdfs#linked-service-properties)kullanabilirsiniz. 
- Azure Blob depolama alanına bağlanmak için birden çok kimlik doğrulama türü desteklenir.  Azure kaynakları [için yönetilen kimlikleri](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#managed-identity)kullanmanızı şiddetle öneririz. Azure Active Directory'de (Azure AD) otomatik olarak yönetilen bir Veri Fabrikası kimliğinin üzerine inşa edilen yönetilen kimlikler, bağlı hizmet tanımında kimlik bilgileri sağlamadan ardışık hatları yapılandırmanıza olanak tanır. Alternatif olarak, bir [hizmet ilkesi,](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#service-principal-authentication) [paylaşılan erişim imzası](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication)veya depolama hesabı [anahtarı](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#account-key-authentication)kullanarak Blob depolama kimliğidoğrulayabilirsiniz. 
- Veri Gölü Depolama Gen2'ye bağlanmak için birden çok kimlik doğrulama türü de desteklenir.  Azure kaynakları [için yönetilen kimlikleri](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#managed-identity)kullanmanızı şiddetle öneririz, ancak bir [hizmet sorumlusu](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication) veya depolama [hesabı anahtarı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#account-key-authentication)da kullanabilirsiniz. 
- Azure kaynakları için yönetilen kimlikler kullanmıyorsanız, Veri Fabrikası bağlantılı hizmetlerini değiştirmeden anahtarları merkezi olarak yönetmeyi ve döndürmeyi kolaylaştırmak için [kimlik bilgilerini Azure Key Vault'ta saklamanızı](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) öneririz. Bu aynı zamanda [CI / CD için en iyi uygulamadır.](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd) 

### <a name="initial-snapshot-data-migration"></a>İlk anlık görüntü veri geçişi 

Veri Fabrikası DistCp modunda, DistCp komutunu göndermek ve ilk veri geçişi davranışını denetlemek için farklı parametrelerkullanmak için bir kopyalama etkinliği oluşturabilirsiniz. 

Veri Fabrikası yerel tümleştirme çalışma zamanı modunda, özellikle 10 TB'den fazla veri aktarırken veri bölümlemenizi öneririz. Verileri bölmek için HDFS'deki klasör adlarını kullanın. Ardından, her Veri Fabrikası kopyalama işi aynı anda bir klasör bölümü kopyalayabilir. Daha iyi iş elde etmek için aynı anda birden çok Veri Fabrikası kopyalama işi çalıştırabilirsiniz.

Kopyalama işlerinden herhangi biri ağ veya veri depolama geçici sorunları nedeniyle başarısız olursa, bu belirli bölümü HDFS'den yeniden yüklemek için başarısız kopyalama işini yeniden çalıştırabilirsiniz. Diğer bölümleri yükleyen diğer kopyalama işleri etkilenmez.

### <a name="delta-data-migration"></a>Delta veri geçişi 

Veri Fabrikası DistCp modunda, Delta veri geçişi için, `-update`kaynak dosya ve hedef dosyası boyutu farklı olduğunda, DistCp komut satırı parametrek, veri yazmak kullanabilirsiniz.

Veri Fabrikası yerel tümleştirme modunda, YENI veya değiştirilen dosyaları HDFS'den tanımlamanın en önemli yolu, zaman bölümlü bir adlandırma kuralı kullanmaktır. HDFS'deki verileriniz dosya veya klasör adındaki zaman dilimi bilgileriyle zaman bölümlere ayrılmıştığinde (örneğin, */yyyy/mm/dd/file.csv),* ardınız hangi dosya ve klasörlerin aşamalı olarak kopyalanabileceğini kolayca belirleyebilir.

Alternatif olarak, HDFS'deki verileriniz zaman bölümlenmiş değilse, Veri Fabrikası **LastModifiedDate** değerlerini kullanarak yeni veya değiştirilmiş dosyaları tanımlayabilir. Veri Fabrikası, HDFS'deki tüm dosyaları tarar ve yalnızca ayarlanmış bir değerden daha büyük son değiştirilmiş bir zaman damgası olan yeni ve güncelleştirilmiş dosyaları kopyalar. 

HDFS'de çok sayıda dosyanız varsa, filtre koşuluyla eşleşen kaç dosyaya bakılmaksızın ilk dosya taraması uzun sürebilir. Bu senaryoda, ilk anlık görüntü geçişi için kullandığınız aynı bölümü kullanarak önce verileri bölmenizi öneririz. Daha sonra, dosya tarama paralel olarak oluşabilir.

### <a name="estimate-price"></a>Tahmini fiyat 

HDFS'den Azure Blob depolamasına veri geçirmek için aşağıdaki ardışık alanı göz önünde bulundurun: 

![Fiyatlandırma ardışık hattını gösteren diyagram](media/data-migration-guidance-hdfs-to-azure-storage/pricing-pipeline.png)

Aşağıdaki bilgileri varsayalım: 

- Toplam veri hacmi 1 PB'dir.
- Veri Fabrikası yerel tümleştirme çalışma zamanı modunu kullanarak verileri geçirin.
- 1 PB 1.000 bölüme bölünür ve her kopya bir bölüm taşır.
- Her kopyalama etkinliği, dört makineyle ilişkili ve 500 MBp'lik iş elde eden, kendi kendine barındırılan bir tümleştirme çalışma süresiyle yapılandırılır.
- ForEach eşzamanlılık **4** olarak ayarlanır ve toplam iş toplamı 2 GBps'dir.
- Göçü tamamlamak toplamda 146 saat sürüyor.

Varsayımlarımıza göre tahmini fiyat şu şekildedir: 

![Fiyatlandırma hesaplamalarını gösteren tablo](media/data-migration-guidance-hdfs-to-azure-storage/pricing-table.png)

> [!NOTE]
> Bu varsayımsal bir fiyatlandırma örneğidir. Gerçek fiyatlandırmanız ortamınızdaki gerçek üretim başına bağlıdır.
> Azure Windows VM'nin (kendi kendine barındırılan tümleştirme çalışma süresi yüklü) fiyatı dahil değildir.

### <a name="additional-references"></a>Ek başvurular

- [HDFS konektörü](https://docs.microsoft.com/azure/data-factory/connector-hdfs)
- [Azure Blob depolama konektörü](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
- [Azure Data Lake Storage 2. Nesil bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
- [Etkinlik performans atonlama kılavuzunu kopyalama](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)
- [Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma ve yapılandırma](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)
- [Kendi kendine barındırılan entegrasyon çalışma zamanı yüksek kullanılabilirlik ve ölçeklenebilirlik](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [Veri taşırken güvenlikle ilgili dikkat edilmesi gerekenler](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)
- [Kimlik bilgilerini Azure Key Vault’ta depolama](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
- [Zaman bölümlenmiş dosya adına dayalı olarak dosyayı aşamalı olarak kopyalama](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-partitioned-file-name-copy-data-tool)
- [LastModifiedDate'e göre yeni ve değiştirilmiş dosyaları kopyalama](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-lastmodified-copy-data-tool)
- [Veri Fabrikası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Fabrikası'nı kullanarak dosyaları birden çok kapsayıcıdan kopyalama](solution-template-copy-files-multiple-containers.md)