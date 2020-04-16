---
title: Verileri Amazon S3'ten Azure Depolama'ya geçirme
description: Verileri Amazon S3'ten Azure Depolama'ya geçirmek için Azure Veri Fabrikası'nı kullanın.
services: data-factory
ms.author: yexu
author: dearandyxu
ms.reviewer: ''
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 8/04/2019
ms.openlocfilehash: 3f40ad7346219b48a38ade38b2a75ddf71940875
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416409"
---
# <a name="use-azure-data-factory-to-migrate-data-from-amazon-s3-to-azure-storage"></a>Verileri Amazon S3'ten Azure Depolama'ya geçirmek için Azure Veri Fabrikası'nı kullanın 

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Veri Fabrikası, verileri Amazon S3'ten Azure Blob Depolama veya Azure Veri Gölü Depolama Gen2'ye ölçekte geçirmek için performans, sağlam ve uygun maliyetli bir mekanizma sağlar.  Bu makalede, veri mühendisleri ve geliştiriciler için aşağıdaki bilgiler sağlar: 

> [!div class="checklist"]
> * Performans 
> * Esnekliği kopyalama
> * Ağ güvenliği
> * Üst düzey çözüm mimarisi 
> * Uygulama en iyi uygulamaları  

## <a name="performance"></a>Performans

ADF, geliştiricilerin ortamınız için veri hareketi veri veri miniliğini en üst düzeye çıkarmak için ağ bant genişliğinizi, depolama IOPS'nizi ve bant genişliğinizi tam olarak kullanmalarını sağlayan farklı düzeylerde paralellik sağlayan sunucusuz bir mimari sunar. 

Müşteriler, Amazon S3'ten Azure Blob Depolama'ya 2 gbp ve daha yüksek sürekli bir iş ortası ile yüz milyonlarca dosyadan oluşan petabaytverileri başarıyla aktarmış. 

![performans](media/data-migration-guidance-s3-to-azure-storage/performance.png)

Yukarıdaki resimde, farklı paralellik düzeyleri ile büyük veri hareket hızlarına nasıl ulaşabileceğiniz gösterilebilir:
 
- Tek bir kopyalama etkinliği ölçeklenebilir bilgi işlem kaynaklarından yararlanabilir: Azure Tümleştirme Çalışma Zamanı'nı kullanırken, her kopyalama etkinliği için sunucusuz bir şekilde [256'ya kadar DIUs](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#data-integration-units) belirtebilirsiniz; kendi kendine barındırılan Tümleştirme Runtime kullanırken, makineyi el ile ölçeklendirebilir veya birden çok makineye[(en fazla 4 düğüm)](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)ölçeklendirebilirsiniz ve tek bir kopyalama etkinliği dosya kümesini tüm düğümler arasında bölümlere ayırır. 
- Tek bir kopyalama etkinliği birden çok iş parçacığı kullanarak veri deposundan okur ve yazar. 
- ADF denetim akışı, örneğin [Her döngü için](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)kullanarak, paralel olarak birden çok kopyalama etkinlikleri başlatabilirsiniz. 

## <a name="resilience"></a>Esnek -lik

Tek bir kopyalama etkinliği çalışması içinde, ADF'nin yerleşik yeniden deneme mekanizması vardır, böylece veri depolarında veya temel ağdaki belirli bir geçici hataları işleyebilir. 

S3'ten Blob'a ve S3'ten ADLS Gen2'ye ikili kopyalama yaparken, ADF otomatik olarak kontrol noktalama gerçekleştirir.  Bir kopya etkinliği çalıştırılamaması veya zaman ları dolduysa, sonraki yeniden denemede kopya, başlangıçtan başlamak yerine son hata noktasından devam eder. 

## <a name="network-security"></a>Ağ güvenliği 

Varsayılan olarak ADF, HTTPS protokolü üzerinden şifreli bağlantı kullanarak verileri Amazon S3'ten Azure Blob Depolama veya Azure Veri Gölü Depolama Gen2'ye aktarır.  HTTPS aktarım sırasında veri şifrelemesağlar ve gizlice dinlemeyi ve ortadaki adam saldırılarını önler. 

Alternatif olarak, verilerin genel Internet üzerinden aktarılmasını istemiyorsanız, AWS Direct Connect ve Azure Express Route arasındaki özel bir eşleme bağlantısı üzerinden veri aktarımı yaparak daha yüksek güvenlik elde edebilirsiniz.  Bunun nasıl sağlanabileceğine ilişkin aşağıdaki çözüm mimarisine bakın. 

## <a name="solution-architecture"></a>Çözüm mimarisi

Verileri genel Internet üzerinden geçirin:

![çözüm-mimari-genel ağ](media/data-migration-guidance-s3-to-azure-storage/solution-architecture-public-network.png)

- Bu mimaride, veriler genel Internet üzerinden HTTPS kullanılarak güvenli bir şekilde aktarılır. 
- Hem kaynak Amazon S3 hem de hedef Azure Blob Depolama veya Azure Veri Gölü Depolama Gen2, tüm ağ IP adreslerinden gelen trafiğe izin verecek şekilde yapılandırılmıştır.  Ağ erişimini belirli IP aralığıyla nasıl kısıtlayabileceğinizle ilgili aşağıdaki ikinci mimariye bakın. 
- Ortamınız için en iyi veri verisini elde etmek için ağınızı ve depolama bant genişliğinizi tam olarak kullanmak için sunucusuz bir şekilde beygir gücü miktarını kolayca ölçeklendirebilirsiniz. 
- Hem ilk anlık görüntü geçişi hem de delta veri geçişi bu mimari kullanılarak elde edilebilir. 

Verileri özel bağlantı üzerinden geçirin: 

![çözüm-mimari-özel ağ](media/data-migration-guidance-s3-to-azure-storage/solution-architecture-private-network.png)

- Bu mimaride, veri geçişi, AWS Direct Connect ve Azure Express Route arasındaki özel bir bakış bağlantısı üzerinden yapılır, böylece veriler genel Internet üzerinden asla geçiş yapmaz.  AWS VPC ve Azure Sanal ağının kullanımını gerektirir. 
- Bu mimariyi elde etmek için Azure sanal ağınızdaki bir Windows VM'de ADF'nin kendi kendine barındırılan tümleştirme çalışma süresini yüklemeniz gerekir.  Kendi barındırılan IR VM'lerinizi el ile ölçeklendirebilir veya ağınızı ve depolama IOPS/bant genişliğini tam olarak kullanmak için birden fazla VM'ye (en fazla 4 düğüm) ölçeklendirebilirsiniz. 
- HTTPS üzerinden veri aktarımı kabul edilebilir sa, ancak kaynak S3'e ağ erişimini belirli bir IP aralığına kilitlemek istiyorsanız, AWS VPC'yi kaldırarak ve özel bağlantıyı HTTPS ile değiştirerek bu mimarinin bir varyasyonunu benimseyebilirsiniz.  Azure Sanal'ı ve kendi kendine barındırılan IR'yi Azure VM'de tutmak isteyeceksiniz, böylece beyaz liste amacıyla statik bir genel olarak kullanılabilir BIR IP'ye sahip olabilirsiniz. 
- Hem ilk anlık görüntü veri geçişi hem de delta veri geçişi bu mimari kullanılarak elde edilebilir. 

## <a name="implementation-best-practices"></a>Uygulama en iyi uygulamaları 

### <a name="authentication-and-credential-management"></a>Kimlik doğrulama ve kimlik bilgisi yönetimi 

- Amazon S3 hesabına kimlik doğrulamak [için IAM hesabı için erişim anahtarını](https://docs.microsoft.com/azure/data-factory/connector-amazon-simple-storage-service#linked-service-properties)kullanmanız gerekir. 
- Azure Blob Depolama'ya bağlanmak için birden çok kimlik doğrulama türü desteklenir.  Azure [kaynakları için yönetilen kimliklerin](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#managed-identity) kullanımı son derece önerilir: Azure AD'de otomatik olarak yönetilen bir ADF tanımlamasının üzerine inşa edilmiştir, bağlantılı hizmet tanımında kimlik bilgileri sağlamadan ardışık hatları yapılandırmanıza olanak tanır.  Alternatif olarak, [Hizmet Sorumlusu,](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#service-principal-authentication) [paylaşılan erişim imzası](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication)veya depolama hesabı [anahtarını](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#account-key-authentication)kullanarak Azure Blob Depolama'ya kimlik doğrulayabilirsiniz. 
- Azure Veri Gölü Depolama Gen2'ye bağlanmak için birden çok kimlik doğrulama türü de desteklenir.  [Hizmet sorumlusu](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication) veya [depolama hesabı anahtarı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#account-key-authentication) da kullanılabilse de, Azure kaynakları için yönetilen [kimliklerin](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#managed-identity) kullanılması önerilir. 
- Azure kaynakları için yönetilen kimlikleri kullanmadığınızda, [kimlik bilgilerini Azure Anahtar Kasası'nda depolamanız,](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) ADF bağlantılı hizmetleri değiştirmeden tuşları merkezi olarak yönetmeyi ve döndürmeyi kolaylaştırmak için önerilir.  Bu aynı zamanda [CI / CD için en iyi uygulamalardan](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd)biridir. 

### <a name="initial-snapshot-data-migration"></a>İlk anlık görüntü veri geçişi 

Veri bölme, özellikle 100 TB'den fazla veri aktarırken önerilir.  Verileri bölmek için, Amazon S3'teki klasör ve dosyaları ada göre filtrelemek için 'önek' ayarını uygulayın ve ardından her ADF kopyalama işi aynı anda bir bölümü kopyalayabilir.  Daha iyi iş elde etmek için aynı anda birden çok ADF kopyalama işi çalıştırabilirsiniz. 

Kopyalama işlerinden herhangi biri ağ veya veri deposu geçici sorunu nedeniyle başarısız olursa, bu belirli bölümü AWS S3'ten yeniden yüklemek için başarısız kopyalama işini yeniden çalıştırabilirsiniz.  Diğer bölümleri yükleyen diğer tüm kopyalama işleri etkilenmez. 

### <a name="delta-data-migration"></a>Delta veri geçişi 

AWS S3'ten yeni veya değiştirilen dosyaları tanımlamanın en önemli yolu, zaman bölümlü adlandırma kuralını kullanmaktır – AWS S3'teki verileriniz dosya veya klasör adındaki zaman dilimi bilgileriyle (örneğin, /yyyy/mm/dd/file.csv) zaman bölümlere ayrılmıştığinde, ardınız hangi dosyaların/klasörlerin aşamalı olarak kopyalanabileceğini kolayca belirleyebilir. 

Alternatif olarak, AWS S3'teki verileriniz zaman bölümlenmiyorsa, ADF yeni veya değiştirilen dosyaları LastModifiedDate'lerine göre tanımlayabilir.   Çalışma şekli, ADF'nin AWS S3'teki tüm dosyaları tarayıp yalnızca son değiştirilen zaman damgası belirli bir değerden büyük olan yeni ve güncelleştirilmiş dosyayı kopyalamasIdır.  S3'te çok sayıda dosyanız varsa, filtre koşuluyla eşleşen kaç dosyaya bakılmaksızın ilk dosya taramanın uzun sürebileceğini unutmayın.  Bu durumda, dosya taramasıparalel olarak gerçekleşebilsin diye ilk anlık görüntü geçişi için aynı 'önek' ayarını kullanarak önce verileri bölmeniz önerilir.  

### <a name="for-scenarios-that-require-self-hosted-integration-runtime-on-azure-vm"></a>Azure VM'de kendi kendine barındırılan Tümleştirme çalışma süresi gerektiren senaryolar için 

İster verileri özel bağlantı üzerinden geçirin, ister Amazon S3 güvenlik duvarında belirli IP aralığına izin vermek istiyorsanız, Azure Windows VM'ye kendi kendine barındırılan Tümleştirme çalışma süresini yüklemeniz gerekir. 

- Her Azure VM için başlatılması gereken tavsiye yapılandırması 32 vCPU ve 128 GB bellekle Standard_D32s_v3.  Daha iyi performans için VM'yi daha fazla büyütmeniz veya maliyet tasarrufu sağlamak için VM'yi küçültmeniz gerekip gerekmeden gerek olup olmadığını görmek için veri geçişi sırasında IR VM'nin CPU'yu ve bellek kullanımını izlemeye devam edebilirsiniz. 
- Ayrıca, kendi kendine barındırılan tek bir IR ile 4 VM düğüme kadar ilişkilendirerek ölçeklendirebilirsiniz.  Kendi kendine barındırılan bir IR'ye karşı çalışan tek bir kopyalama işi, dosya kümesini otomatik olarak bölümlere ayırır ve dosyaları paralel olarak kopyalamak için tüm VM düğümlerinden yararlanır.  Yüksek kullanılabilirlik için, veri geçişi sırasında tek bir hata noktasını önlemek için 2 VM düğümüyle başlamanız önerilir. 

### <a name="rate-limiting"></a>Hız sınırlaması 

En iyi uygulama olarak, uygun bir bölüm boyutunu belirleyebilmeniz için temsili bir örnek veri kümesiyle bir performans POC'u gerçekleştirin. 

Varsayılan DIU ayarı ile tek bir bölüm ve tek bir kopyalama etkinliği ile başlayın.  Ağınızın veya veri depolarınızın IOPS/bant genişliği sınırına ulaşana kadar DIU ayarını kademeli olarak artırın veya tek bir kopyalama etkinliğinde izin verilen maksimum 256 DIU'ya ulaştınız. 

Ardından, ortamınızın sınırlarına ulaşana kadar eşzamanlı kopyalama etkinliklerinin sayısını kademeli olarak artırın. 

ADF kopyalama etkinliği tarafından bildirilen azaltma hatalarıyla karşılaştığınızda, ADF'deki eşzamanlılık veya DIU ayarını azaltın veya ağ ve veri depolarının bant genişliği/IOPS sınırlarını artırmayı düşünün.  

### <a name="estimating-price"></a>Tahmini Fiyat 

> [!NOTE]
> Bu varsayımsal bir fiyatlandırma örneğidir.  Gerçek fiyatlandırmanız ortamınızdaki gerçek üretim başına bağlıdır.

Verileri S3'ten Azure Blob Depolama'ya geçirmek için oluşturulmuş aşağıdaki ardışık yapıyı göz önünde bulundurun: 

![fiyatlandırma-boru hattı](media/data-migration-guidance-s3-to-azure-storage/pricing-pipeline.png)

Aşağıdakileri varsayalım: 

- Toplam veri hacmi 2 PB'dir 
- İlk çözüm mimarisini kullanarak verileri HTTPS üzerinden geçirme 
- 2 PB 1 K bölüme bölünür ve her kopya bir bölüm taşır 
- Her kopya DIU=256 ile yapılandırılır ve 1 GBps iş elde edilir 
- ForEach eşzamanlılık 2 olarak ayarlanır ve toplam iş toplamı 2 GBps'dir 
- Toplamda, göçü tamamlamak 292 saat sürer. 

Yukarıdaki varsayımlara göre tahmini fiyat aşağıda verilmiştir: 

![fiyatlandırma tablosu](media/data-migration-guidance-s3-to-azure-storage/pricing-table.png)

### <a name="additional-references"></a>Ek başvurular 
- [Amazon Basit Depolama Servisi konektörü](https://docs.microsoft.com/azure/data-factory/connector-amazon-simple-storage-service)
- [Azure Blob Depolama konektörü](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
- [Azure Data Lake Storage 2. Nesil bağlayıcısı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
- [Etkinlik performans atonlama kılavuzunu kopyalama](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)
- [Kendi barındırılan Tümleştirme Çalışma Süresi oluşturma ve yapılandırma](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)
- [Kendi kendine barındırılan entegrasyon çalışma süresi HA ve ölçeklenebilirlik](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [Veri taşırken güvenlikle ilgili dikkat edilmesi gerekenler](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)
- [Kimlik bilgilerini Azure Key Vault’ta depolama](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
- [Zaman bölümlenmiş dosya adına göre dosyayı artımlı olarak kopyalama](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-partitioned-file-name-copy-data-tool)
- [LastModifiedDate'e göre yeni ve değiştirilmiş dosyaları kopyalama](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-lastmodified-copy-data-tool)
- [ADF fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)

## <a name="template"></a>Şablon

Amazon S3'ten Azure Veri Gölü Depolama Gen2'ye yüz milyonlarca dosyadan oluşan petabaytlarca veriyi geçirmek için başlamak için [şablon](solution-template-migration-s3-azure.md) aşağıda verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Fabrikası ile birden çok kapsayıcıdaki dosyaları kopyalama](solution-template-copy-files-multiple-containers.md)
