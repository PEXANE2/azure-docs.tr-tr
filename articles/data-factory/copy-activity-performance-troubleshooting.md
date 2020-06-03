---
title: Kopyalama etkinliği performansını sorun giderme
description: Azure Data Factory 'da etkinlik performansını kopyalama sorunlarını giderme hakkında bilgi edinin.
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
ms.openlocfilehash: 694f10b53d02d44d189cbe7cbe492f48ac3b5669
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84299800"
---
# <a name="troubleshoot-copy-activity-performance"></a>Kopyalama etkinliği performansını sorun giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory ' de kopyalama etkinliği performans sorununu giderme konusu özetlenmektedir. 

Bir kopyalama etkinliği çalıştırdıktan sonra, [kopyalama etkinliği izleme](copy-activity-monitoring.md) görünümünde sonuçları ve performans istatistiklerini toplayabilirsiniz. Bir örnek verilmiştir.

![Kopyalama etkinliği çalıştırma ayrıntılarını izle](./media/copy-activity-overview/monitor-copy-activity-run-details.png)

## <a name="performance-tuning-tips"></a>Performans ayarı ipuçları

Bazı senaryolarda, Data Factory bir kopyalama etkinliği çalıştırdığınızda Yukarıdaki örnekte gösterildiği gibi en üst kısımdaki **"performans ayarlama ipuçları"** nı görürsünüz. İpuçları, bu belirli bir kopya çalıştırması için ADF tarafından tanımlanan darboğazyı, kopyalama aktarım hızını nasıl arttıracaklarını gösteren önerilerden daha fazla bilgi sağlar. Yeniden konumlandırılan değişikliği yapmayı deneyin, sonra kopyayı yeniden çalıştırın.

Bir başvuru olarak şu anda performans ayarlama ipuçları aşağıdaki durumlar için öneriler sağlar:

| Kategori              | Performans ayarı ipuçları                                      |
| --------------------- | ------------------------------------------------------------ |
| Veri deposuna özgü   | **Azure Synpao Analytics 'e veri yükleme (eski ADıYLA SQL DW)**: kullanılmıyorsa PolyBase veya Copy deyimlerini kullanmayı önerin. |
| &nbsp;                | Verileri **Azure SQL veritabanı**'ndan kopyalama: DTU yüksek kullanım altındaysa, daha yüksek katmana yükseltmeyi önerin. |
| &nbsp;                | Veri kopyalama/hedef **Azure Cosmos DB**: ru yüksek kullanım altındayken, daha büyük ru 'ya yükseltmeyi önerin. |
|                       | **SAP tablosundan**veri kopyalama: büyük miktarda veriyi kopyalarken, paralel yüklemeyi etkinleştirmek ve en fazla bölüm numarasını ARTıRMAK için SAP bağlayıcısının bölüm seçeneğinden yararlanın. |
| &nbsp;                | **Amazon Redshift**'tan veri almak: KULLANıLMıYORSA, kaldırma kullanmayı önerin. |
| Veri depolama alanı azaltma | Kopyalama sırasında veri deposu tarafından bir dizi okuma/yazma işlemi kısıtlanıyor ise, veri deposu için izin verilen istek hızını denetlemeyi ve artırmayı önerin ya da eşzamanlı iş yükünü azaltın. |
| Tümleştirme çalışma zamanı  | Şirket içinde barındırılan bir **Integration Runtime (IR)** kullanıyorsanız ve kopyalama ETKINLIĞI, IR 'nin yürütülmesi için kullanılabilir kaynağa sahip olana kadar kuyrukta uzun süre bekliyorsa, IR 'nin ölçeğini genişletme/büyütme önerin. |
| &nbsp;                | En uygun olmayan bir bölgede bulunan bir **Azure Integration Runtime** kullanırsanız, yavaş okuma/yazma ile sonuçlanır, başka bir bölgede bir IR kullanmak için yapılandırmayı önerin. |
| Hataya dayanıklılık       | Hata toleransını yapılandırır ve uyumsuz satırları atlayarak performansı düşürür, kaynak ve havuz verilerinin uyumlu olmasını önerin. |
| Hazırlanmış kopya           | Hazırlanan kopya yapılandırıldıysa ancak kaynak havuzu çiftinde yararlı değilse, kaldırmayı önerin. |
| Sürdür                | Kopyalama etkinliği son hata noktasından devam ettirildiğinde, ancak özgün çalıştırmasından sonra DIU ayarını değiştirmeniz durumunda, yeni DIU ayarının etkili olmadığına göz atın. |

## <a name="understand-copy-activity-execution-details"></a>Kopyalama etkinliği yürütme ayrıntılarını anlama

Kopyalama etkinliği izleme görünümünün en altındaki yürütme ayrıntıları ve süreleri, kopyalama etkinliğinizin (Bu makalenin başındaki örnek), özellikle de kopyalama performansının giderilmesi için yararlı olan anahtar aşamalarını açıklar. Kopya çalışmalarınızın performans sorunu en uzun süreye sahip bir süredir. Her bir aşamanın tanımında aşağıdaki tabloya başvurun ve [Azure IR kopyalama etkinliğinin nasıl giderileceğini](#troubleshoot-copy-activity-on-azure-ir) ve bu tür bilgiyle şirket IÇINDE [barındırılan IR 'de kopyalama etkinliğinin nasıl giderileceğini](#troubleshoot-copy-activity-on-self-hosted-ir) öğrenin.

| Aşama           | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| Kuyruk           | Kopyalama etkinliği tümleştirme çalışma zamanı üzerinde çalışmaya başlanana kadar geçen süre. |
| Kopyalama öncesi betiği | IR ve kopyalama etkinliğinden itibaren kopyalama etkinliği arasındaki geçen süre, havuz veri deposunda kopyalama öncesi betiği yürütmeyi tamamlıyor. Veritabanı havuzları için kopyalama öncesi betiği yapılandırdığınızda, örneğin Azure SQL veritabanı 'na veri yazarken yeni verileri kopyalamaya başlamadan önce bu uygulamayı temizleyebilirsiniz. |
| Aktarma        | Önceki adımın sonu ile IR 'nin tüm verileri kaynaktan havuza aktarma arasındaki geçen süre. "Aktarım" altındaki alt adımlar paralel olarak çalışır.<br><br>- **İlk bayta kalan süre:** Önceki adımın sonu ile IR 'nin kaynak veri deposundan ilk baytı aldığı zaman arasında geçen süre. Dosya tabanlı olmayan kaynaklar için geçerlidir.<br>- **Listeleme kaynağı:** Kaynak dosyalarını veya veri bölümlerini listelemek için harcanan sürenin miktarı. İkincisi, veritabanı kaynakları için bölüm seçeneklerini yapılandırdığınızda, ör. Oracle/SAP HANA/Teradata/Netezza/vb gibi veritabanlarından veri kopyalama yaparken geçerlidir.<br/>-**Kaynaktan okunuyor:** Kaynak veri deposundan verileri almak için harcanan sürenin miktarı.<br/>- **Havuza yazma:** Havuz veri deposuna veri yazmak için harcanan sürenin miktarı. |

## <a name="troubleshoot-copy-activity-on-azure-ir"></a>Azure IR kopyalama etkinliğinin sorunlarını giderme

Senaryolarınız için performans testi planlamak ve yürütmek için [performans ayarlama adımlarını](copy-activity-performance.md#performance-tuning-steps) izleyin. 

Kopyalama etkinliği performansı beklentilerinizi karşılamıyorsa, Azure Integration Runtime üzerinde çalışan tek kopyalama etkinliğinin sorunlarını gidermek için, kopyalama izleme görünümünde gösterilen [performans ayarlama ipuçları](#performance-tuning-tips) ' nı görürseniz, öneriyi uygulayın ve yeniden deneyin. Aksi takdirde, [kopyalama etkinliği yürütme ayrıntılarını anlayın](#understand-copy-activity-execution-details), **en uzun** süreye sahip olan aşamayı denetleyin ve kopyalama performansını artırmak için aşağıdaki kılavuzu uygulayın:

- **"Kopyalama öncesi betiği" uzun süre yaşadı:** havuz veritabanında çalışan kopyalama öncesi betiğin tamamlanmasının uzun sürmesi anlamına gelir. Performansı geliştirmek için belirtilen kopyalama öncesi betik mantığını ayarlayın. Betiği geliştirmeye yönelik daha fazla yardıma ihtiyacınız varsa veritabanı ekibinize başvurun.

- **"Aktarım-ilk bayta kalan süre" uzun çalışma süresi yaşadı**: kaynak sorgunuzun herhangi bir veri döndürmesi uzun sürme anlamına gelir. Sorguyu veya sunucuyu denetleyin ve iyileştirin. Daha fazla yardıma ihtiyacınız varsa veri Mağazası ekibinize başvurun.

- **"Aktarım listeleme kaynağı" uzun çalışma süresi yaşadı**: kaynak dosyalarının listelenmesi veya kaynak veritabanının veri bölümlerinin yavaş olması anlamına gelir.

  - Dosya tabanlı kaynaktan veri kopyalarken, klasör yolu veya dosya adı (veya) üzerinde **joker karakter filtresi** kullanırsanız `wildcardFolderPath` `wildcardFileName` veya **dosya son değiştirme zamanı filtresi** ( `modifiedDatetimeStart` veya `modifiedDatetimeEnd` ) kullanıyorsanız, bu filtre kopyalama etkinliğinin belirtilen klasörün altındaki tüm dosyaları istemci tarafına listelemesine neden olur ve sonra filtreyi uygular. Bu tür dosya numaralandırması, özellikle de yalnızca küçük bir dosya kümesi filtre kuralını karşıladığında performans sorunlarına neden olabilir.

    - [Dosyaları, tarih saat bölümlenmiş dosya yoluna veya adına göre kopyalayıp kopyalayamayacağını](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md)denetleyin. Bu şekilde, kaynak tarafında liste üzerinde yük getirmez.

    - Bunun yerine, Amazon S3 ve Azure Blob için özel olarak "**önek**" olmak üzere veri deposunun yerel filtresini kullanıp kullanınızın olup olmadığını kontrol edin. Ön ek filtresi, veri deposu sunucu tarafı filtretir ve çok daha iyi performansa sahip olur.

    - Tek büyük veri kümesini birkaç küçük veri kümesine bölmek ve bu kopyalama işlerinin verilerin her bir tackles bölümünü aynı anda çalıştırmasına izin vermek için göz önünde bulundurun. Bunu arama/GetMetadata + ForEach + Copy ile yapabilirsiniz. [Birden çok kapsayıcıdan dosya kopyalama](solution-template-copy-files-multiple-containers.md) veya [Amazon S3 'ten verileri](solution-template-migration-s3-azure.md) genel örnek olarak ADLS 2. çözüm şablonlarına geçirme bölümüne bakın.

  - ADF 'nin kaynakta azaltma hatası olduğunu veya veri deponuzda yüksek kullanım durumunda olup olmadığını denetleyin. Bu durumda, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinizle iletişim kurmayı deneyin.

  - Azure IR, kaynak veri deposu bölgenize aynı veya yakın bir şekilde kullanın.

- **"Kaynaktan okuma aktarım" uzun çalışma süresi yaşadı**: 

  - Varsa bağlayıcıya özgü verileri yükleme en iyi uygulamasını benimseyin. Örneğin, [Amazon Redshift](connector-amazon-redshift.md)'tan veri kopyalarken, Redshift Unload kullanacak şekilde yapılandırın.

  - ADF 'nin kaynakta herhangi bir azaltma hatası olup olmadığını ve veri deponuzda yüksek kullanım altında olup olmadığını denetleyin. Bu durumda, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinizle iletişim kurmayı deneyin.

  - Kopyalama kaynak ve havuz örüntüsünün olup olmadığını denetleyin: 

    - Kopyalama düzeniniz 4 ' ten fazla veri tümleştirme birimini (DIUs) destekliyorsa, Ayrıntılar [bölümünde bu bölüme](copy-activity-performance-features.md#data-integration-units) bakın, genellikle daha iyi performans almak için daha fazla performans sağlayabilirsiniz. 

    - Aksi takdirde, tek büyük veri kümesini birkaç küçük veri kümesine bölmeyi düşünün ve bu kopyalama işlerinin verilerin her bir tackles bölümünü aynı anda çalıştırmasına izin verin. Bunu arama/GetMetadata + ForEach + Copy ile yapabilirsiniz. Bkz. [birden çok kapsayıcıdan dosya kopyalama](solution-template-copy-files-multiple-containers.md), [Amazon S3 ' dan ADLS 2.](solution-template-migration-s3-azure.md)veya [toplu kopyalama ile genel örnek olarak bir denetim tablosu](solution-template-bulk-copy-with-control-table.md) çözüm şablonlarıyla veri geçirme.

  - Azure IR, kaynak veri deposu bölgenize aynı veya yakın bir şekilde kullanın.

- **"Havuza aktarım yazma" uzun çalışma süresi yaşadı**:

  - Varsa bağlayıcıya özgü verileri yükleme en iyi uygulamasını benimseyin. Örneğin, verileri [Azure SYNAPSE Analytics](connector-azure-sql-data-warehouse.md) 'e (eskı ADıYLA SQL DW) kopyalarken PolyBase veya Copy deyimlerini kullanın. 

  - ADF 'nin havuzda herhangi bir azaltma hatası olduğunu veya veri deponuzda yüksek kullanım altında olup olmadığını denetleyin. Bu durumda, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinizle iletişim kurmayı deneyin.

  - Kopyalama kaynak ve havuz örüntüsünün olup olmadığını denetleyin: 

    - Kopyalama düzeniniz 4 ' ten fazla veri tümleştirme birimini (DIUs) destekliyorsa, Ayrıntılar [bölümünde bu bölüme](copy-activity-performance-features.md#data-integration-units) bakın, genellikle daha iyi performans almak için daha fazla performans sağlayabilirsiniz. 

    - Aksi takdirde, [paralel kopyaları](copy-activity-performance-features.md)kademeli olarak ayarlayabilir, çok fazla sayıda paralel kopya bile performansa zarar verebilir.

  - Azure IR, havuz veri deposu bölgenize aynı veya yakın bir şekilde kullanın.

## <a name="troubleshoot-copy-activity-on-self-hosted-ir"></a>Şirket içinde barındırılan IR 'de kopyalama etkinliğinin sorunlarını giderme

Senaryolarınız için performans testi planlamak ve yürütmek için [performans ayarlama adımlarını](copy-activity-performance.md#performance-tuning-steps) izleyin. 

Kopyalama performansı beklentilerinizi karşılamıyorsa, Azure Integration Runtime üzerinde çalışan tek kopyalama etkinliğinin sorunlarını gidermek için, kopyalama izleme görünümünde gösterilen [performans ayarlama ipuçları](#performance-tuning-tips) ' nı görürseniz, öneriyi uygulayın ve yeniden deneyin. Aksi takdirde, [kopyalama etkinliği yürütme ayrıntılarını anlayın](#understand-copy-activity-execution-details), **en uzun** süreye sahip olan aşamayı denetleyin ve kopyalama performansını artırmak için aşağıdaki kılavuzu uygulayın:

- **"Kuyruk" uzun süre yaşadı:** bu, şirket IÇINDE barındırılan IR 'nin yürütülmesi için kaynağa sahip olana kadar kopyalama etkinliğinin sırada beklediği anlamına gelir. IR kapasitesini ve kullanımını denetleyin ve iş yükünüze göre [ölçeği artırma veya genişletme](create-self-hosted-integration-runtime.md#high-availability-and-scalability) .

- **"Aktarım-ilk bayta kalan süre" uzun çalışma süresi yaşadı**: kaynak sorgunuzun herhangi bir veri döndürmesi uzun sürme anlamına gelir. Sorguyu veya sunucuyu denetleyin ve iyileştirin. Daha fazla yardıma ihtiyacınız varsa veri Mağazası ekibinize başvurun.

- **"Aktarım listeleme kaynağı" uzun çalışma süresi yaşadı**: kaynak dosyalarının listelenmesi veya kaynak veritabanının veri bölümlerinin yavaş olması anlamına gelir.

  - Şirket içinde barındırılan IR makinesinin, kaynak veri deposuna bağlanmak için düşük gecikme süresine sahip olup olmadığını denetleyin. Kaynağınız Azure 'da ise, şirket içinde barındırılan IR makinesinden Azure bölgesine gecikme süresini denetlemek için [Bu aracı](http://www.azurespeed.com/Azure/Latency) kullanabilirsiniz.

  - Dosya tabanlı kaynaktan veri kopyalarken, klasör yolu veya dosya adı (veya) üzerinde **joker karakter filtresi** kullanırsanız `wildcardFolderPath` `wildcardFileName` veya **dosya son değiştirme zamanı filtresi** ( `modifiedDatetimeStart` veya `modifiedDatetimeEnd` ) kullanıyorsanız, bu filtre kopyalama etkinliğinin belirtilen klasörün altındaki tüm dosyaları istemci tarafına listelemesine neden olur ve sonra filtreyi uygular. Bu tür dosya numaralandırması, özellikle de yalnızca küçük bir dosya kümesi filtre kuralını karşıladığında performans sorunlarına neden olabilir.

    - [Dosyaları, tarih saat bölümlenmiş dosya yoluna veya adına göre kopyalayıp kopyalayamayacağını](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md)denetleyin. Bu şekilde, kaynak tarafında liste üzerinde yük getirmez.

    - Bunun yerine, Amazon S3 ve Azure Blob için özel olarak "**önek**" olmak üzere veri deposunun yerel filtresini kullanıp kullanınızın olup olmadığını kontrol edin. Ön ek filtresi, veri deposu sunucu tarafı filtretir ve çok daha iyi performansa sahip olur.

    - Tek büyük veri kümesini birkaç küçük veri kümesine bölmek ve bu kopyalama işlerinin verilerin her bir tackles bölümünü aynı anda çalıştırmasına izin vermek için göz önünde bulundurun. Bunu arama/GetMetadata + ForEach + Copy ile yapabilirsiniz. [Birden çok kapsayıcıdan dosya kopyalama](solution-template-copy-files-multiple-containers.md) veya [Amazon S3 'ten verileri](solution-template-migration-s3-azure.md) genel örnek olarak ADLS 2. çözüm şablonlarına geçirme bölümüne bakın.

  - ADF 'nin kaynakta azaltma hatası olduğunu veya veri deponuzda yüksek kullanım durumunda olup olmadığını denetleyin. Bu durumda, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinizle iletişim kurmayı deneyin.

- **"Kaynaktan okuma aktarım" uzun çalışma süresi yaşadı**: 

  - Şirket içinde barındırılan IR makinesinin, kaynak veri deposuna bağlanmak için düşük gecikme süresine sahip olup olmadığını denetleyin. Kaynağınız Azure 'da ise, şirket içinde barındırılan IR makinesinden Azure bölgelerine gecikme süresini denetlemek için [Bu aracı](http://www.azurespeed.com/Azure/Latency) kullanabilirsiniz.

  - Şirket içinde barındırılan IR makinesinde verileri okumak ve verileri verimli bir şekilde aktarmak için yeterli gelen bant genişliğine sahip olup olmadığını denetleyin. Kaynak veri depolduğunuz Azure 'da olması, indirme hızını denetlemek için [Bu aracı](https://www.azurespeed.com/Azure/Download) kullanabilirsiniz.

  - Data Factory-> genel bakış sayfanıza Azure portal > şirket içinde barındırılan IR 'nin CPU ve bellek kullanımı eğilimini kontrol edin. CPU kullanımı yüksek veya kullanılabilir bellek düşükse, [ölçeği artırma/genişletme](create-self-hosted-integration-runtime.md#high-availability-and-scalability) için göz önünde bulundurun.

  - Varsa bağlayıcıya özgü verileri yükleme en iyi uygulamasını benimseyin. Örneğin:

    - [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP tablosu](connector-sap-table.md#sap-table-as-source)ve [SAP açık hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)'dan verileri kopyalarken, verileri paralel olarak kopyalamak için veri bölümü seçeneklerini etkinleştirin.

    - Bir [sunucudan verileri](connector-hdfs.md)kopyalarken, distcp kullanacak şekilde yapılandırın.

    - [Amazon Redshift](connector-amazon-redshift.md)'tan veri kopyalarken, Redshift Unload kullanacak şekilde yapılandırın.

  - ADF 'nin kaynakta herhangi bir azaltma hatası bildirme veya veri deponuzda yüksek kullanım altında olup olmadığını denetleyin. Bu durumda, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinizle iletişim kurmayı deneyin.

  - Kopyalama kaynak ve havuz örüntüsünün olup olmadığını denetleyin: 

    - Bölüm seçeneği etkinleştirilmiş veri depolarından verileri kopyalarsanız, [paralel kopyaları](copy-activity-performance-features.md)kademeli olarak ayarlamayı düşünün. çok fazla paralel kopya, performansı da etkileyebilir.

    - Aksi takdirde, tek büyük veri kümesini birkaç küçük veri kümesine bölmeyi düşünün ve bu kopyalama işlerinin verilerin her bir tackles bölümünü aynı anda çalıştırmasına izin verin. Bunu arama/GetMetadata + ForEach + Copy ile yapabilirsiniz. Bkz. [birden çok kapsayıcıdan dosya kopyalama](solution-template-copy-files-multiple-containers.md), [Amazon S3 ' dan ADLS 2.](solution-template-migration-s3-azure.md)veya [toplu kopyalama ile genel örnek olarak bir denetim tablosu](solution-template-bulk-copy-with-control-table.md) çözüm şablonlarıyla veri geçirme.

- **"Havuza aktarım yazma" uzun çalışma süresi yaşadı**:

  - Varsa bağlayıcıya özgü verileri yükleme en iyi uygulamasını benimseyin. Örneğin, verileri [Azure SYNAPSE Analytics](connector-azure-sql-data-warehouse.md) 'e (eskı ADıYLA SQL DW) kopyalarken PolyBase veya Copy deyimlerini kullanın. 

  - Şirket içinde barındırılan IR makinesinin, havuz veri deposuna bağlanma sırasında düşük gecikme süresine sahip olup olmadığını denetleyin. Havuzunuzu Azure 'da kullanıyorsanız, [Bu aracı](http://www.azurespeed.com/Azure/Latency) kullanarak şirket IÇINDE barındırılan IR makinesinden Azure bölgesine gecikme süresini denetleyebilir.

  - Şirket içinde barındırılan IR makinesinin verileri etkili bir şekilde aktarımına ve yazmaya yetecek kadar giden bant genişliğine sahip olup olmadığını denetleyin. Havuz veri depoluizin Azure 'da ise, yükleme hızını denetlemek için [Bu aracı](https://www.azurespeed.com/Azure/UploadLargeFile) kullanabilirsiniz.

  - Data Factory-> genel bakış sayfanıza Azure portal > şirket içinde barındırılan IR 'nin CPU ve bellek kullanımı eğilimi olup olmadığını denetleyin. CPU kullanımı yüksek veya kullanılabilir bellek düşükse, [ölçeği artırma/genişletme](create-self-hosted-integration-runtime.md#high-availability-and-scalability) için göz önünde bulundurun.

  - ADF 'nin havuzda herhangi bir azaltma hatası olduğunu veya veri deponuzda yüksek kullanım altında olup olmadığını denetleyin. Bu durumda, veri deposundaki iş yüklerinizi azaltın veya azaltma sınırını veya kullanılabilir kaynağı artırmak için veri deposu yöneticinizle iletişim kurmayı deneyin.

  - [Paralel kopyaları](copy-activity-performance-features.md)kademeli olarak ayarlamayı göz önünde bulundurun. çok fazla paralel kopya, hatta performansın zarar görmediğini unutmayın.

## <a name="other-references"></a>Diğer başvurular

Desteklenen bazı veri depoları için performans izleme ve ayarlama başvuruları aşağıda verilmiştir:

* Azure Blob depolama: BLOB depolama için [ölçeklenebilirlik ve performans hedefleri](../storage/blobs/scalability-targets.md) , [BLOB depolaması için performans ve ölçeklenebilirlik denetim listesi](../storage/blobs/storage-performance-checklist.md).
* Azure Tablo Depolama: tablo depolaması için tablo depolama ve performans [ve ölçeklenebilirlik denetim listesi](../storage/tables/storage-performance-checklist.md) [için ölçeklenebilirlik ve performans hedefleri](../storage/tables/scalability-targets.md) .
* Azure SQL veritabanı: [performansı izleyebilir](../sql-database/sql-database-single-database-monitor.md) ve veritabanı işlem BIRIMI (DTU) yüzdesini kontrol edebilirsiniz.
* Azure SQL veri ambarı: yeteneği, veri ambarı birimlerinde (DWU) ölçülür. Bkz. [Azure SQL veri ambarı 'nda işlem gücünü yönetme (genel bakış)](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Azure Cosmos DB performans düzeyleri](../cosmos-db/performance-levels.md).
* SQL Server: [performansı izleme ve ayarlama](https://msdn.microsoft.com/library/ms189081.aspx).
* Şirket içi dosya sunucusu: [dosya sunucuları Için performans ayarlama](https://msdn.microsoft.com/library/dn567661.aspx).

## <a name="next-steps"></a>Sonraki adımlar
Diğer kopyalama etkinliği makalelerine bakın:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği performans ve ölçeklenebilirlik Kılavuzu](copy-activity-performance.md)
- [Etkinlik performansını en iyi duruma getirme özelliklerini Kopyala](copy-activity-performance-features.md)
- [Data Lake veya veri ambarınızdan verileri Azure 'a geçirmek için Azure Data Factory kullanın](data-migration-guidance-overview.md)
- [Verileri Amazon S3'ten Azure Depolama'ya geçirme](data-migration-guidance-s3-azure-storage.md)
