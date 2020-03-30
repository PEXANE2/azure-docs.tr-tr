---
title: Sorun giderme kopyalama etkinliği performansı
description: Azure Veri Fabrikası'nda kopyalama etkinliği performansını nasıl giderebilirsiniz hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/11/2020
ms.openlocfilehash: 963b86852a7df557ad7179e444e7c3a2692f57d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79531461"
---
# <a name="troubleshoot-copy-activity-performance"></a>Sorun giderme kopyalama etkinliği performansı

Bu makalede, Azure Veri Fabrikası'nda kopya etkinliği performansı sorunu nasıl giderilir özetlenir. 

Bir kopyalama etkinliği çalıştırdıktan sonra, [kopyalama etkinliği izleme](copy-activity-monitoring.md) görünümünde çalıştırma sonucu ve performans istatistiklerini toplayabilirsiniz. Bir örnek verilmiştir.

![Kopyalama etkinliği çalıştırma ayrıntılarını izleme](./media/copy-activity-overview/monitor-copy-activity-run-details.png)

## <a name="performance-tuning-tips"></a>Performans ayarı ipuçları

Bazı senaryolarda, Veri Fabrikası'nda bir kopyalama etkinliği çalıştırdığınızda, yukarıdaki örnekte gösterildiği gibi üstte **"Performans alamı ipuçları"** görürsünüz. İpuçları, bu belirli kopya çalışması için ADF tarafından tanımlanan darboğaz ve kopyalama iş masını artırma konusunda önerilerde bulunmak. Yeniden komut edilen değişikliği yapmayı deneyin ve kopyayı yeniden çalıştırın.

Başvuru olarak, şu anda performans tuning ipuçları aşağıdaki durumlar için öneriler sağlar:

| Kategori              | Performans ayarı ipuçları                                      |
| --------------------- | ------------------------------------------------------------ |
| Veri deposuna özgü   | **Azure Synpase Analytics'e (eski adıyla SQL DW)** veri yükleme : kullanılmadığı takdirde PolyBase veya COPY deyimini kullanmanızı öneririz. |
| &nbsp;                | Verilerin Azure SQL **Veritabanı'ndan**kopyalanması : DTU yüksek kullanım altındayken, daha yüksek katmana yükseltmeyi önerir. |
| &nbsp;                | Verileri **Azure Cosmos DB'den**kopyalama : RU yüksek kullanım altındayken, daha büyük RU'ya yükseltmeyi önerir. |
| &nbsp;                | **Amazon Redshift'ten**veri alma : kullanılmadığı takdirde UNLOAD kullanmanızı öneririz. |
| Veri deposu azaltma | Kopyalama sırasında veri deposu tarafından bir dizi okuma/yazma işlemi kısıtlanırsa, veri deposu için izin verilen istek oranını denetlemeyi ve artırmayı veya eşzamanlı iş yükünü azaltmayı önerin. |
| Tümleştirme çalışma zamanı  | Kendi kendine **barındırılan Tümleştirme Çalışma Süresi (IR)** kullanıyorsanız ve kopyalama etkinliği, IR'nin yürütülecek kullanılabilir kaynağı olana kadar kuyrukta uzun süre bekliyorsa, IR'nizi ölçekleme/yükseltmeyi öneririz. |
| &nbsp;                | Yavaş okuma/yazma yla sonuçlanan en uygun olmayan bölgede olmayan bir **Azure Tümleştirme Çalışma Zamanı** kullanıyorsanız, başka bir bölgede IR kullanmak üzere yapılandırmanızı öneririz. |
| Hataya dayanıklılık       | Hata toleransı yapılandırın ve uyumsuz satırları atlayarak yavaş performans sağlarsanız, kaynak ve lavabo verilerinin uyumlu olmasını öneririz. |
| Aşamalı kopya           | Aşamalı kopya yapılandırıldıysa ancak kaynak lavabo çiftiniz için yararlı değilse, kopyayı kaldırmanızı öneririz. |
| Sürdür                | Kopyalama etkinliği son hata noktasından itibaren devam ettirildiğinde ancak özgün çalıştırmadan sonra DIU ayarını değiştirdiğinizde, yeni DIU ayarının etkili olmadığını unutmayın. |

## <a name="understand-copy-activity-execution-details"></a>Kopyalama etkinliği yürütme ayrıntılarını anlama

Kopyalama etkinliği izleme görünümünün altındaki yürütme ayrıntıları ve süreleri, kopyalama etkinliğinizin geçtiği temel aşamaları açıklar (bu makalenin başındaki örneğe bakın), bu da özellikle kopya performansını gidermede yararlıdır. Kopya çalıştırmanızın darboğazı en uzun süreye sahip olandır. Her aşamanın tanımındaki aşağıdaki tabloya bakın ve [Azure IR'deki kopyalama etkinliğini](#troubleshoot-copy-activity-on-azure-ir) nasıl gidereceklerini öğrenin ve bu tür bilgilerle Kendi [barındırılan IR'de Sorun Giderme kopyalama etkinliği](#troubleshoot-copy-activity-on-self-hosted-ir) öğrenin.

| Aşama           | Açıklama                                                  |
| --------------- | ------------------------------------------------------------ |
| Kuyruk           | Kopyalama etkinliği gerçekten tümleştirme çalışma zamanında başlayana kadar geçen süre. |
| Komut dosyasının ön kopyası | IR'de başlayan kopyalama etkinliği ile lavabo veri deposunda ön kopyalama komut dosyasının yürütülmesini tamamlama kopyalama etkinliği arasında geçen süre. Veritabanı lavaboları için ön kopyalama komut dosyasını yapılandırırken uygulayın ( örneğin Azure SQL Veritabanı'na veri yazarken yeni verileri kopyalamadan önce temizleyin. |
| Aktarma        | Önceki adımın sonu ile KAYNAKtan batmaya tüm verileri aktaran IR arasındaki geçen süre. "Aktarım" altındaki alt adımlar paralel olarak çalışır.<br><br>- **Zaman ilk bayt için:** Önceki adımın sonu ile IR'nin kaynak veri deposundan ilk baytı aldığı saat arasında geçen süre. Dosya tabanlı olmayan kaynaklar için geçerlidir.<br>- **Listeleme kaynağı:** Kaynak dosyaları veya veri bölümlerini derecelendirmek için harcanan süre. İkincisi, veritabanı kaynakları için bölüm seçeneklerini yapılandırdığınızda geçerlidir, örneğin Oracle/SAP HANA/Teradata/Netezza/vb. veritabanlarından kopya verileri kopyalarken.<br/>-**Kaynaktan okuma:** Kaynak veri deposundan veri almak için harcanan süre.<br/>- **Lavaboya yazma:** Veri deposunu batırmak için veri yazmak için harcanan süre. |

## <a name="troubleshoot-copy-activity-on-azure-ir"></a>Azure IR'de sorun giderme kopyalama etkinliği

Senaryonuz için performans testini planlamak ve gerçekleştirmek için [Performans tuning adımlarını](copy-activity-performance.md#performance-tuning-steps) izleyin. 

Kopyalama etkinliği performansı beklentinizi karşılamazsa, Azure Tümleştirme Çalıştırzamanında çalışan tek kopya etkinliğini gidermek için, kopya izleme görünümünde gösterilen [performans ayar ipuçlarını](#performance-tuning-tips) görürseniz, öneriyi uygulayın ve yeniden deneyin. Aksi takdirde, [kopyalama etkinliği yürütme ayrıntılarını anlayın,](#understand-copy-activity-execution-details)hangi aşamanın en **uzun** süreye sahip olduğunu kontrol edin ve kopyalama performansını artırmak için aşağıdaki kılavuzu uygulayın:

- **"Ön kopyalama komut dosyası" uzun süre deneyimli:** bu lavabo veritabanında çalışan ön kopyalama komut dosyası bitirmek için uzun sürer anlamına gelir. Performansı artırmak için belirtilen ön kopya komut dosyası mantığını ayarlayın. Komut dosyasını geliştirmek için daha fazla yardıma ihtiyacınız varsa, veritabanı ekibinize başvurun.

- **"Aktarım - İlk bayt**için zaman" deneyimli uzun çalışma süresi : bu kaynak sorguherhangi bir veri döndürmek için uzun sürer anlamına gelir. Sorguyu veya sunucuyu denetleyin ve optimize edin. Daha fazla yardıma ihtiyacınız varsa, veri depolama ekibinize başvurun.

- **"Transfer - Listeleme kaynak" deneyimli uzun çalışma süresi**: bu kaynak dosyaları veya kaynak veritabanı veri bölümleri nin sayısalya yavaş anlamına gelir.

  - Dosya tabanlı kaynaktan veri kopyalarken, **wildcard filter** klasör yolunda veya dosya adı`wildcardFolderPath` (veya), `wildcardFileName`üzerinde joker karakter filtresi`modifiedDatetimeEnd`kullanıyorsanız veya **dosyayı son değiştirilen zaman filtresini** kullanıyorsanız (veya),`modifiedDatetimeStart` bu filtrenin belirtilen klasörün altındaki tüm dosyaları istemci tarafına listeleyerek kopyalama etkinliğine neden olacağını unutmayın ve ardından filtreyi uygulayın. Bu tür dosya numaralandırması, özellikle yalnızca küçük dosya kümesi filtre kuralıyla karşılaştığında darboğaza dönüşebilir.

    - [Tarih bölümlenmiş dosya yoluna veya adına göre dosyaları kopyalayıp](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md)kopyalayamayacağınızı denetleyin. Bu şekilde kaynak tarafında listeleme yük getirmez.

    - Bunun yerine veri deposunun yerel filtresini, Özellikle Amazon S3 ve Azure Blob için **"önek**" kullanıp kullanamayacağınızı kontrol edin. Önek filtresi bir veri deposu sunucusu tarafı filtresidir ve çok daha iyi performansa sahip olur.

    - Tek büyük veri kümesini birkaç küçük veri kümesine bölmeyi ve bu kopyalama işlerinin eş zamanlı olarak çalışmasına izin vermeyi düşünün. Bunu Lookup/GetMetadata + ForEach + Copy ile yapabilirsiniz. Dosyaları [birden çok kapsayıcıdan kopyalayın](solution-template-copy-files-multiple-containers.md) veya [verileri Amazon S3'ten ADLS Gen2](solution-template-migration-s3-azure.md) çözüm şablonlarına genel örnek olarak geçirin.

  - ADF'nin kaynakta herhangi bir azaltma hatası raporedip bildirmeyin veya veri deponuzun yüksek kullanım durumu altında olup olmadığını kontrol edin. Bu nedenle, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinize başvurmayı deneyin.

  - Azure IR'yi kaynak veri deposu bölgenizin aynısında veya yakınında kullanın.

- **"Transfer - kaynaktan okuma" deneyimli uzun çalışma süresi:** 

  - Varsa bağlayıcıya özel veri yükleme en iyi uygulama benimseyin. Örneğin, [Amazon Redshift'ten](connector-amazon-redshift.md)veri kopyalarken Redshift UNLOAD'ı kullanacak şekilde yapılandırın.

  - ADF'nin kaynakta herhangi bir azaltma hatası raporedip bildirmeyin veya veri deponuzun yüksek kullanım altında olup olmadığını kontrol edin. Bu nedenle, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinize başvurmayı deneyin.

  - Kopya kaynağınızı ve lavabo deseninizi kontrol edin: 

    - Kopyalama deseniniz 4 Veri Tümleştirme Birimi'nden (DIUS) daha büyük görünüyorsa - ayrıntılarla [ilgili bu bölüme](copy-activity-performance.md#data-integration-units) bakın, genellikle daha iyi performans elde etmek için DIUs'ları artırmayı deneyebilirsiniz. 

    - Aksi takdirde, tek büyük veri kümesini birkaç küçük veri kümesine bölmeyi düşünün ve bu kopyalama işlerinin eş zamanlı olarak çalışmasına izin verin, her biri verilerin bir kısmını ele alır. Bunu Lookup/GetMetadata + ForEach + Copy ile yapabilirsiniz. Birden [çok kapsayıcıdan dosyaları kopyalamaya](solution-template-copy-files-multiple-containers.md)bakın, [verileri Amazon S3'ten ADLS Gen2'ye geçirin](solution-template-migration-s3-azure.md)veya genel örnek olarak bir denetim tablosu çözümü şablonları [içeren toplu kopya.](solution-template-bulk-copy-with-control-table.md)

  - Azure IR'yi kaynak veri deposu bölgenizin aynısında veya yakınında kullanın.

- **"Transfer - lavaboya yazma" deneyimli uzun çalışma süresi:**

  - Varsa bağlayıcıya özel veri yükleme en iyi uygulama benimseyin. Örneğin, verileri [Azure Synapse Analytics'e](connector-azure-sql-data-warehouse.md) (eski adıyla SQL DW) kopyalarken, PolyBase veya COPY deyimini kullanın. 

  - ADF'nin lavaboda herhangi bir azaltma hatası rapor edip etmeyin veya veri deponuzun yüksek kullanım altında olup olmadığını kontrol edin. Bu nedenle, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinize başvurmayı deneyin.

  - Kopya kaynağınızı ve lavabo deseninizi kontrol edin: 

    - Kopyalama deseniniz 4 Veri Tümleştirme Birimi'nden (DIUS) daha büyük görünüyorsa - ayrıntılarla [ilgili bu bölüme](copy-activity-performance.md#data-integration-units) bakın, genellikle daha iyi performans elde etmek için DIUs'ları artırmayı deneyebilirsiniz. 

    - Aksi takdirde, yavaş yavaş [paralel kopyaları](copy-activity-performance-features.md)ayarlayın , çok fazla paralel kopyabile performans zarar verebilir unutmayın.

  - Azure IR'yi lavabo veri deposu bölgenizin aynısında veya yakınında kullanın.

## <a name="troubleshoot-copy-activity-on-self-hosted-ir"></a>Kendi barındırılan IR'de sorun giderme kopyalama etkinliği

Senaryonuz için performans testini planlamak ve gerçekleştirmek için [Performans tuning adımlarını](copy-activity-performance.md#performance-tuning-steps) izleyin. 

Kopyalama performansı beklentinizi karşılamadığında, Azure Tümleştirme Çalıştır zamanında çalışan tek kopya etkinliğini gidermek için, kopya izleme görünümünde gösterilen [performans ayar ipuçlarını](#performance-tuning-tips) görürseniz, öneriyi uygulayın ve yeniden deneyin. Aksi takdirde, [kopyalama etkinliği yürütme ayrıntılarını anlayın,](#understand-copy-activity-execution-details)hangi aşamanın en **uzun** süreye sahip olduğunu kontrol edin ve kopyalama performansını artırmak için aşağıdaki kılavuzu uygulayın:

- **"Kuyruk" uzun süreli deneyimli:** kopyalama etkinliği, Kendi barındırılan IR'niz yürütülecek kaynağa sahip olana kadar kuyrukta uzun süre beklediği anlamına gelir. IR kapasitesini ve kullanımını kontrol edin ve iş yükünüze göre [ölçeklendirin veya ölçeklendirin.](create-self-hosted-integration-runtime.md#high-availability-and-scalability)

- **"Aktarım - İlk bayt**için zaman" deneyimli uzun çalışma süresi : bu kaynak sorguherhangi bir veri döndürmek için uzun sürer anlamına gelir. Sorguyu veya sunucuyu denetleyin ve optimize edin. Daha fazla yardıma ihtiyacınız varsa, veri depolama ekibinize başvurun.

- **"Transfer - Listeleme kaynak" deneyimli uzun çalışma süresi**: bu kaynak dosyaları veya kaynak veritabanı veri bölümleri nin sayısalya yavaş anlamına gelir.

  - Kendi kendine barındırılan IR makinesinin kaynak veri deposuna bağlanan düşük gecikme gecikmesi olup olmadığını denetleyin. Kaynağınız Azure'daysa, kendi kendine barındırılan IR makinesinden Azure bölgesine gecikme gecikmesini denetlemek için [bu aracı](http://www.azurespeed.com/Azure/Latency) kullanabilirsiniz, ne kadar iyi olursa o kadar iyi.

  - Dosya tabanlı kaynaktan veri kopyalarken, **wildcard filter** klasör yolunda veya dosya adı`wildcardFolderPath` (veya), `wildcardFileName`üzerinde joker karakter filtresi`modifiedDatetimeEnd`kullanıyorsanız veya **dosyayı son değiştirilen zaman filtresini** kullanıyorsanız (veya),`modifiedDatetimeStart` bu filtrenin belirtilen klasörün altındaki tüm dosyaları istemci tarafına listeleyerek kopyalama etkinliğine neden olacağını unutmayın ve ardından filtreyi uygulayın. Bu tür dosya numaralandırması, özellikle yalnızca küçük dosya kümesi filtre kuralıyla karşılaştığında darboğaza dönüşebilir.

    - [Tarih bölümlenmiş dosya yoluna veya adına göre dosyaları kopyalayıp](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md)kopyalayamayacağınızı denetleyin. Bu şekilde kaynak tarafında listeleme yük getirmez.

    - Bunun yerine veri deposunun yerel filtresini, Özellikle Amazon S3 ve Azure Blob için **"önek**" kullanıp kullanamayacağınızı kontrol edin. Önek filtresi bir veri deposu sunucusu tarafı filtresidir ve çok daha iyi performansa sahip olur.

    - Tek büyük veri kümesini birkaç küçük veri kümesine bölmeyi ve bu kopyalama işlerinin eş zamanlı olarak çalışmasına izin vermeyi düşünün. Bunu Lookup/GetMetadata + ForEach + Copy ile yapabilirsiniz. Dosyaları [birden çok kapsayıcıdan kopyalayın](solution-template-copy-files-multiple-containers.md) veya [verileri Amazon S3'ten ADLS Gen2](solution-template-migration-s3-azure.md) çözüm şablonlarına genel örnek olarak geçirin.

  - ADF'nin kaynakta herhangi bir azaltma hatası raporedip bildirmeyin veya veri deponuzun yüksek kullanım durumu altında olup olmadığını kontrol edin. Bu nedenle, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinize başvurmayı deneyin.

- **"Transfer - kaynaktan okuma" deneyimli uzun çalışma süresi:** 

  - Kendi kendine barındırılan IR makinesinin kaynak veri deposuna bağlanan düşük gecikme gecikmesi olup olmadığını denetleyin. Kaynağınız Azure'daysa, kendi kendine barındırılan IR makinesinden Azure bölgelerine gecikme gecikmesini denetlemek için [bu aracı](http://www.azurespeed.com/Azure/Latency) kullanabilirsiniz, ne kadar iyi olursa o kadar iyi.

  - Self barındırılan IR makinesinin verileri verimli bir şekilde okumak ve aktarmak için yeterli gelen bant genişliğine sahip olup olmadığını kontrol edin. Kaynak veri mağazanız Azure'daysa, indirme hızını denetlemek için [bu aracı](https://www.azurespeed.com/Azure/Download) kullanabilirsiniz.

  - Veri fabrikanıza > genel bakış sayfanıza > Azure portalında Kendi kendine barındırılan IR'nin CPU ve bellek kullanım eğilimini kontrol edin. CPU kullanımı yüksek veya kullanılabilir bellek düşükse [IR'yi ölçeklendirmeyi/çıkarmayı](create-self-hosted-integration-runtime.md#high-availability-and-scalability) düşünün.

  - Varsa bağlayıcıya özel veri yükleme en iyi uygulama benimseyin. Örnek:

    - [Oracle,](connector-oracle.md#oracle-as-source) [Netezza,](connector-netezza.md#netezza-as-source) [Teradata,](connector-teradata.md#teradata-as-source)SAP [HANA,](connector-sap-hana.md#sap-hana-as-source) [SAP Table](connector-sap-table.md#sap-table-as-source)ve [SAP Open Hub'dan](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)veri kopyalarken, verileri paralel olarak kopyalamak için veri bölme seçenekleri etkinleştirin.

    - [HDFS'den](connector-hdfs.md)veri kopyalarken, DistCp'yi kullanacak şekilde yapılandırın.

    - [Amazon Redshift'ten](connector-amazon-redshift.md)veri kopyalarken Redshift UNLOAD'ı kullanacak şekilde yapılandırın.

  - ADF'nin kaynakta herhangi bir azaltma hatası rapor edip etmeyin veya veri deponuzun yüksek kullanım altında olup olmadığını kontrol edin. Bu nedenle, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinize başvurmayı deneyin.

  - Kopya kaynağınızı ve lavabo deseninizi kontrol edin: 

    - Bölüm seçeneği etkin veri depolarından verileri kopyalarsanız, [paralel kopyaları](copy-activity-performance-features.md)kademeli olarak ayarlamayı düşünün, çok fazla paralel kopyanın performansa bile zarar verebileceğini unutmayın.

    - Aksi takdirde, tek büyük veri kümesini birkaç küçük veri kümesine bölmeyi düşünün ve bu kopyalama işlerinin eş zamanlı olarak çalışmasına izin verin, her biri verilerin bir kısmını ele alır. Bunu Lookup/GetMetadata + ForEach + Copy ile yapabilirsiniz. Birden [çok kapsayıcıdan dosyaları kopyalamaya](solution-template-copy-files-multiple-containers.md)bakın, [verileri Amazon S3'ten ADLS Gen2'ye geçirin](solution-template-migration-s3-azure.md)veya genel örnek olarak bir denetim tablosu çözümü şablonları [içeren toplu kopya.](solution-template-bulk-copy-with-control-table.md)

- **"Transfer - lavaboya yazma" deneyimli uzun çalışma süresi:**

  - Varsa bağlayıcıya özel veri yükleme en iyi uygulama benimseyin. Örneğin, verileri [Azure Synapse Analytics'e](connector-azure-sql-data-warehouse.md) (eski adıyla SQL DW) kopyalarken, PolyBase veya COPY deyimini kullanın. 

  - Kendi kendine barındırılan IR makinesinin lavabo veri deposuna bağlanan düşük gecikme gecikmesi olup olmadığını kontrol edin. Lavabonuz Azure'daysa, kendi kendine barındırılan IR makinesinden Azure bölgesine gecikme gecikmesini denetlemek için [bu aracı](http://www.azurespeed.com/Azure/Latency) kullanabilirsiniz, ne kadar iyi olursa o kadar iyi.

  - Self barındırılan IR makinesinin verileri verimli bir şekilde aktarmak ve yazmak için yeterli giden bant genişliğine sahip olup olmadığını kontrol edin. Lavabo veri mağazanız Azure'daysa, yükleme hızını denetlemek için [bu aracı](https://www.azurespeed.com/Azure/UploadLargeFile) kullanabilirsiniz.

  - Azure portalında Kendi kendine barındırılan IR'nin CPU ve bellek kullanım eğiliminin veri fabrikanıza > genel > kontrol edin. CPU kullanımı yüksek veya kullanılabilir bellek düşükse [IR'yi ölçeklendirmeyi/çıkarmayı](create-self-hosted-integration-runtime.md#high-availability-and-scalability) düşünün.

  - ADF'nin lavaboda herhangi bir azaltma hatası rapor edip etmeyin veya veri deponuzun yüksek kullanım altında olup olmadığını kontrol edin. Bu nedenle, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinize başvurmayı deneyin.

  - Yavaş yavaş paralel [kopyaları](copy-activity-performance-features.md)ayarlamak için düşünün , çok fazla paralel kopyaları bile performans zarar verebilir unutmayın.

## <a name="other-references"></a>Diğer referanslar

Desteklenen veri depolarından bazıları için performans izleme ve tuning başvuruları aşağıda veda edilmiştir:

* Azure Blob depolama: Blob depolama ve [Performans ve Blob depolama için ölçeklenebilirlik kontrol listesi](../storage/blobs/storage-performance-checklist.md)için [ölçeklenebilirlik ve performans hedefleri.](../storage/blobs/scalability-targets.md)
* Azure Tablo depolama: Tablo depolama ve Performans ve [Tablo depolama için ölçeklenebilirlik denetim listesi için](../storage/tables/storage-performance-checklist.md) [ölçeklenebilirlik ve performans hedefleri.](../storage/tables/scalability-targets.md)
* Azure SQL Veritabanı: [Performansı izleyebilir](../sql-database/sql-database-single-database-monitor.md) ve Veritabanı İşlem Birimi (DTU) yüzdesini kontrol edebilirsiniz.
* Azure SQL Veri Ambarı: Yeteneği Veri Ambarı Birimleri (DWUs) cinsinden ölçülür. Bkz. [Azure SQL Veri Ambarı'nda bilgi işlem gücünü yönet (Genel Bakış)](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Azure Cosmos DB'de performans düzeyleri.](../cosmos-db/performance-levels.md)
* Şirket içi SQL Server: [Performans için izleyin ve ayarlayın.](https://msdn.microsoft.com/library/ms189081.aspx)
* Şirket içi dosya sunucusu: [Dosya sunucuları için performans ayarı.](https://msdn.microsoft.com/library/dn567661.aspx)

## <a name="next-steps"></a>Sonraki adımlar
Diğer kopyalama etkinliği makalelerini görün:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Etkinlik performansı ve ölçeklenebilirlik kılavuzunu kopyalama](copy-activity-performance.md)
- [Etkinlik performans optimizasyonu özelliklerini kopyalama](copy-activity-performance-features.md)
- [Verilerinizi veri gölünüzden veya veri ambarınızdan Azure'a geçirmek için Azure Veri Fabrikası'nı kullanın](data-migration-guidance-overview.md)
- [Verileri Amazon S3'ten Azure Depolama'ya geçirme](data-migration-guidance-s3-azure-storage.md)
