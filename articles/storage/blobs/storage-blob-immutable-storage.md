---
title: Sabit BLOB depolama-Azure depolama
description: Azure depolama, kullanıcıların belirli bir Aralık için silinebilir olmayan ve değiştirilemeyen bir durumda veri depolamasını sağlayan blob (nesne) depolama için solucan (bir kez yaz, çok oku) desteğini sunar.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: tamram
ms.reviewer: hux
ms.subservice: blobs
ms.openlocfilehash: 92bfa4f13467763fd88b9ae993554aef69355d75
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555224"
---
# <a name="store-business-critical-blob-data-with-immutable-storage"></a>Sabit depolamayla iş açısından kritik blob verilerini depolayın

Azure Blob depolama için sabit depolama, kullanıcıların iş açısından kritik veri nesnelerini bir solucan içinde depolamasına olanak sağlar (bir kez yaz, çok oku) durumu. Bu durum, verileri silinebilir olmayan ve Kullanıcı tarafından belirtilen bir Aralık için değiştirilemez hale getirir. Saklama aralığı süresince, Bloblar oluşturulup okunabilir, ancak değiştirilemez veya silinemez. Tüm Azure bölgelerindeki genel amaçlı v2 ve BLOB depolama hesapları için sabit depolama kullanılabilir.

Yasal tutmaları ayarlama ve temizleme veya Azure portal, PowerShell veya Azure CLı kullanarak zaman tabanlı bir bekletme ilkesi oluşturma hakkında bilgi için bkz. [BLOB depolama için imlebilirlik Ilkelerini ayarlama ve yönetme](storage-blob-immutability-policies-manage.md).

## <a name="about-immutable-blob-storage"></a>Sabit BLOB depolama hakkında

Sabit depolama, özellikle de aracı dağıtıcı kuruluşların verileri güvenli bir şekilde depolamasını&mdash;&mdash;sağlık kuruluşu, finans kurumları ve ilgili sektörlere yardımcı olur. Sabit depolama Ayrıca, değişiklik veya silme açısından kritik verileri korumak için herhangi bir senaryoda yararlanılabilir olabilir.

Tipik kullanım alanları şunlardır:

- **Mevzuat uyumluluğu**: Azure Blob depolama için sabit depolama, KURULUŞLARıN, sn. a-4 (f), CFTC 1.31 (d), finra ve diğer yönetmelikleri ele masına yardımcı olur. Cohasset ile teknik bir Teknik İnceleme ayrıntıları, bu mevzuat gereksinimlerinin ne kadar sabit olduğunu [Microsoft hizmet güveni portalı](https://aka.ms/AzureWormStorage)aracılığıyla indirileceğine ilişkin ayrıntıları ayrıntılarıyla ilişkilendirir. [Azure Güven Merkezi](https://www.microsoft.com/trustcenter/compliance/compliance-overview) , uyumluluk sertifikalarımızla ilgili ayrıntılı bilgiler içerir.

- **Güvenli belge saklama**: Azure Blob depolama için sabit depolama, verilerin hesap yönetici ayrıcalıklarına sahip kullanıcılar dahil olmak üzere herhangi bir kullanıcı tarafından değiştirilmesini veya silinmesini sağlar.

- **Yasal tutma**: Azure Blob depolama için sabit depolama, kullanıcıların, bekletme kaldırılana kadar istenen süre için, BT veya iş kullanımı açısından kritik öneme sahip gizli bilgileri, istenen süreye göre depolamanıza olanak sağlar. Bu özellik yalnızca yasal kullanım durumları ile sınırlı değildir, ancak olay tetikleyicilerine veya kurumsal ilkeye göre verilerin korunması gerektiğinde olay tabanlı bir saklama veya kurumsal kilit olarak da düşünülebilir.

Sabit depolama aşağıdaki özellikleri destekler:

- **[Zamana dayalı bekletme ilkesi desteği](#time-based-retention-policies)** : kullanıcılar, belirli bir Aralık için verileri depolamak üzere ilkeler ayarlayabilir. Zaman tabanlı bir bekletme ilkesi ayarlandığında, Bloblar oluşturulabilir ve okunabilir, ancak değiştirilemez veya silinemez. Saklama süresi dolduktan sonra Bloblar silinebilir ancak üzerine yazılmaz.

- **[Yasal tutma ilkesi desteği](#legal-holds)** : bekletme aralığı bilinmiyorsa, kullanıcılar yasal tutmaları, yasal saklama temizlenene kadar sabit verileri depolayacak şekilde ayarlayabilir.  Yasal bir saklama ilkesi ayarlandığında, Bloblar oluşturulabilir ve okunabilir, ancak değiştirilemez veya silinemez. Her yasal saklama, bir tanımlayıcı dize olarak kullanılan Kullanıcı tanımlı bir alfasayısal etiketle (örneğin, bir durum KIMLIĞI, olay adı vb.) ilişkilendirilir. 

- **Tüm blob katmanları Için destek**: solucan Ilkeleri Azure Blob depolama katmanından bağımsızdır ve tüm katmanlara uygulanır: sık erişimli, seyrek erişimli ve arşiv. Kullanıcılar iş yükleri için en uygun maliyetli katmana veri geçirebilir, bu da veri dengeszliği sağlar.

- **Kapsayıcı düzeyinde yapılandırma**: kullanıcılar, zaman tabanlı bekletme ilkelerini ve geçerli saklama etiketlerini kapsayıcı düzeyinde yapılandırabilir. Kullanıcılar, basit kapsayıcı düzeyi ayarları kullanarak zaman tabanlı bekletme ilkeleri oluşturup kilitleyebilir, bekletme aralıklarını genişletebilir, yasal tutmaları ayarlayabilir ve temizleyebilir ve daha fazlasını yapabilir. Bu ilkeler, kapsayıcıdaki tüm Bloblar, hem mevcut hem de yeni için geçerlidir.

- **Denetim günlüğü desteği**: her kapsayıcı bir ilke Denetim günlüğü içerir. Kilitleme zaman tabanlı bekletme ilkeleri için yedi adede kadar zamana dayalı saklama komutu gösterir ve Kullanıcı KIMLIĞI, komut türü, zaman damgaları ve bekletme aralığını içerir. Yasal tutar için, günlük Kullanıcı KIMLIĞI, komut türü, zaman damgaları ve yasal saklama etiketlerini içerir. Bu günlük, SEC 17A-4 (f) mevzuat yönergelerine uygun olarak ilkenin kullanım ömrü boyunca tutulur. [Azure etkinlik günlüğü](../../azure-monitor/platform/activity-logs-overview.md) tüm denetim düzlemi etkinliklerinin daha kapsamlı bir günlüğünü gösterir; [Azure tanılama günlüklerini](../../azure-monitor/platform/resource-logs-overview.md) etkinleştirme, veri düzlemi işlemlerini korur ve gösterir. Yasal bir şekilde veya başka amaçlar için gerekli olabileceğinden, bu günlükleri kalıcı olarak depolamak kullanıcının sorumluluğundadır.

## <a name="how-it-works"></a>Nasıl çalışır

Azure Blob depolama için sabit depolama, iki tür SOLUCANı veya sabit ilkeyi destekler: zamana dayalı bekletme ve yasal tutar. Bir kapsayıcıya zaman tabanlı bir bekletme ilkesi veya yasal saklama alanı uygulandığında, mevcut tüm Bloblar 30 saniyeden az bir şekilde sabit bir solucan durumuna geçer. Bu kapsayıcıya yüklenen tüm yeni Bloblar da sabit duruma geçer. Tüm Bloblar sabit duruma taşındıktan sonra, değişmez ilke onaylanır ve sabit kapsayıcıda var olan ve yeni nesneler için tüm üzerine yazma veya silme işlemlerine izin verilmez.

Kapsayıcıda veya depolama hesabında sabit bir ilkeyle korunan blob varsa kapsayıcı ve depolama hesabı silinmesine izin verilmez. Kilitli zaman tabanlı bekletme ilkesine veya yasal tutmaya sahip en az bir blob varsa kapsayıcı silme işlemi başarısız olur. Geçerli bir bekletme aralığına sahip en az bir solucan kapsayıcısı veya etkin saklama aralığı olan bir blob varsa, depolama hesabı silme işlemi başarısız olur.

### <a name="time-based-retention-policies"></a>Zamana dayalı saklama ilkeleri

> [!IMPORTANT]
> Bir zaman tabanlı bekletme ilkesi, blob 'un SEC 17A-4 (f) ve diğer yasal *uyumluluk için uyumlu* bir sabit (yazma ve silme korumalı) durumunda olması gerekir. İlkeyi, genellikle 24 saatten daha az sürede kilitlemenizi öneririz. Uygulanan bir zaman tabanlı bekletme ilkesinin ilk durumu, kilidi kapatmadan önce özelliği test etmeniz ve ilkede değişiklikler *yapmanız sağlanır.* *Kilidi açık* duruma karşı koruma sağlarken, kısa vadeli Özellik denemeleri dışında herhangi bir amaçla *kilitsiz* durum kullanılması önerilmez. 

Bir kapsayıcıya zaman tabanlı bir bekletme ilkesi uygulandığında, kapsayıcıdaki tüm Bloblar, *etkin* saklama dönemi süresince sabit durumda kalır. Mevcut bloblar için geçerli olan saklama süresi, blob oluşturma zamanı ile kullanıcı tarafından belirtilen saklama aralığı arasındaki farka eşittir.

Yeni bloblar için geçerli olan saklama süresi, kullanıcı tarafından belirtilen saklama aralığına eşittir. Kullanıcılar bekletme aralığını genişletebildiğinden, sabit depolama, etkin saklama süresini hesaplamak için Kullanıcı tarafından belirtilen bekletme aralığının en son değerini kullanır.

Örneğin, bir kullanıcının beş yıl bekletme aralığı ile zaman tabanlı bir bekletme ilkesi oluşturduğunu varsayalım. Bu kapsayıcıda bulunan mevcut bir blob, _testblob1_, bir yıl önce oluşturuldu. _Testblob1_ için geçerli saklama süresi dört yıldır. Yeni blob, _testblob2_, kapsayıcıya yüklendiğinde, yeni blob için geçerli saklama süresi beş yıldır.

Kilitlemeli bir süre tabanlı bekletme ilkesi yalnızca özellik testi için önerilir ve sn, SEC 17A-4 (f) ve diğer mevzuata uyumluluğuyla uyumlu olması için kilitlenmelidir. Zaman tabanlı bekletme ilkesi kilitlendiğinde, ilke kaldırılamaz ve etkin saklama süresine en fazla beş artışa izin verilir. Zamana dayalı saklama ilkelerini ayarlama ve kilitleme hakkında daha fazla bilgi için bkz. [BLOB depolama için imlebilirlik Ilkelerini ayarlama ve yönetme](storage-blob-immutability-policies-manage.md).

Aşağıdaki sınırlar bekletme ilkeleri için geçerlidir:

- Depolama hesabı için, kilitli zamana dayalı sabit ilkelerle maksimum kapsayıcı sayısı 1.000 ' dir.
- En düşük saklama aralığı bir gündür. En fazla 146.000 gün (400 yıl).
- Bir kapsayıcı için, kilitli zaman tabanlı sabit ilkeler için bir bekletme aralığını uzatmak üzere en fazla düzenleme sayısı 5 ' tir.
- Bir kapsayıcı için, kilitli bir ilke için en fazla yedi zaman tabanlı bekletme ilkesi denetim günlüğü tutulur.

### <a name="legal-holds"></a>Yasal tutma

Yasal bir saklama ayarladığınızda, yasal saklama temizlenene kadar tüm mevcut ve yeni Bloblar sabit durumda kalır. Yasal tutmaları ayarlama ve temizleme hakkında daha fazla bilgi için bkz. [BLOB depolama için imlebilirlik Ilkelerini ayarlama ve yönetme](storage-blob-immutability-policies-manage.md).

Bir kapsayıcı aynı anda hem yasal ayrı tutmaya hem de zaman tabanlı bir bekletme ilkesine sahip olabilir. Geçerli saklama süresi sona erse bile, bu kapsayıcıdaki tüm Bloblar, tüm yasal tutmalar temizlenene kadar sabit durumda kalır. Buna karşılık, bir blob, yasal saklama süresi sona erene kadar sabit bir durumda kalır, ancak tüm yasal tutmalar temizlenmiş olsa da geçerlidir.

Aşağıdaki tabloda, farklı sabit senaryolar için devre dışı bırakılmış BLOB depolama işlemlerinin türleri gösterilmektedir. Daha fazla bilgi için bkz. [Azure Blob hizmeti REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) belgeleri.

|Senaryo  |Blob durumu  |Blob işlemlerine izin verilmiyor  |
|---------|---------|---------|
|Blobdaki geçerli saklama süresi dolmadı ve/veya yasal tutma ayarlandı     |Sabit: hem silme hem de yazma korumalı         | Blob<sup>1</sup>koyma, yerleştirme blok<sup>1</sup>, yerleştirme listesi<sup>1</sup>, kapsayıcıyı silme, blobu silme, blob meta verilerini ayarlama, yerleştirme sayfası, blob özelliklerini ayarlama, anlık görüntü blobu, artımlı kopyalama blobu, ekleme bloğu         |
|Blob üzerindeki geçerli saklama süresi doldu     |Yalnızca yazma korumalı (silme işlemlerine izin verilir)         |Blob<sup>1</sup>, put bloğu<sup>1</sup>, yerleştirme, blok listesi<sup>1</sup>, blob meta verilerini ayarlama, yerleştirme sayfası, blob özelliklerini ayarlama, anlık görüntü blobu, artımlı kopyalama blobu, ekleme bloğu         |
|Tüm yasal tutar temizlendi ve kapsayıcıda zaman tabanlı bekletme ilkesi ayarlanmadı     |Değiştirilebilir         |Hiçbiri         |
|Bir solucan ilkesi oluşturulmaz (zamana dayalı saklama veya yasal saklama)     |Değiştirilebilir         |Hiçbiri         |

<sup>1</sup> uygulama bu işlemlerin bir kez yeni blob oluşturmasına izin verir. Sabit bir kapsayıcıda var olan bir blob yolundaki tüm sonraki üzerine yazma işlemlerine izin verilmez.

Geçerli tutmalar için aşağıdaki sınırlar geçerlidir:

- Bir depolama hesabı için, geçerli bir saklama ayarı olan en fazla kapsayıcı sayısı 1.000 ' dir.
- Bir kapsayıcı için, en fazla yasal saklama etiketi sayısı 10 ' dur.
- Geçerli bir saklama etiketinin minimum uzunluğu üç alfasayısal karakterdir. Maksimum uzunluk 23 alfasayısal karakterdir.
- Bir kapsayıcı için, ilke süresince en fazla 10 yasal saklama ilkesi denetim günlüğü tutulur.

## <a name="pricing"></a>Fiyatlandırma

Bu özelliği kullanmak için ek ücret alınmaz. Değişmez veriler, değişebilir verilerle aynı şekilde fiyatlandırılır. Azure Blob depolama hakkındaki fiyatlandırma ayrıntıları için bkz. [Azure Depolama fiyatlandırması sayfası](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="faq"></a>SSS

**SOLUCAN uyumluluğuna ilişkin belgeler verebilir misiniz?**

Evet. Microsoft, uyumluluğu belgelemek için kayıt yönetimi ve bilgi İdaresi, Cohasset Associates ' de uzmanlaşmış, sabit blob depolamayı ve buna özgü gereksinimlere uyum sağlayan önde gelen bağımsız bir değerlendirme firması Finans hizmetleri sektör. Cohasset, bir solucan durumunda zaman tabanlı Blobları sürdürmek için kullanıldığında, sabit BLOB depolama alanının, CFTC Rule 1.31 (c)-(d), FINRA kuralı 4511 ve SEC Rule 17A-4 ' nin ilgili depolama gereksinimlerini karşılar. Microsoft, bu kural kümesini hedefleyen, mali kurumlara yönelik kayıtlar için küresel olarak en iyi şekilde koruma kılavuzunu temsil etmektedir. Cohasset raporu, [Microsoft hizmet Güven Merkezi](https://aka.ms/AzureWormStorage)' nde kullanılabilir. Microsoft 'tan solucan uyumluluğuna ilişkin bir kanıtlama mektubu istemek için lütfen Azure desteğine başvurun.

**Özellik yalnızca blob 'ları engellemek veya sayfa ve ekleme Blobları da için geçerlidir mi?**

Sabit depolama, kapsayıcı düzeyinde ayarlandığı için herhangi bir blob türüyle kullanılabilir, ancak genellikle blok bloblarını depolayan kapsayıcılar için solucan kullanmanızı öneririz. Blok Bloblarından farklı olarak, tüm yeni sayfa Blobları ve ekleme Blobları bir solucan kapsayıcısının dışında oluşturulmalıdır ve sonra içine kopyalanmalıdır. Bu Blobları bir solucan kapsayıcısına kopyaladıktan sonra, bir ekleme blobuna *başka ekleme* veya sayfa blobuna yapılan değişikliklere izin verilmez. VM diskini kilitleyecek şekilde VHD 'leri (sayfa Blobları) depolayan bir kapsayıcıda bir solucan ilkesi ayarlamak önerilmez.

**Bu özelliği kullanmak için yeni bir depolama hesabı oluşturmem gerekir mi?**

Hayır, mevcut veya yeni oluşturulan genel amaçlı v2 veya blob depolama hesaplarıyla sabit depolama kullanabilirsiniz. Bu özellik, GPv2 ve BLOB depolama hesaplarında blok Blobları ile kullanım için tasarlanmıştır. Genel amaçlı v1 depolama hesapları desteklenmez, ancak genel amaçlı v2 'ye kolayca yükseltilebilir. Mevcut bir genel amaçlı v1 depolama hesabını yükseltme hakkında daha fazla bilgi için bkz. [Depolama hesabını yükseltme](../common/storage-account-upgrade.md).

**Yalnızca yasal bir saklama ve zaman tabanlı bekletme ilkesi uygulayabilir miyim?**

Evet, bir kapsayıcıda aynı anda hem yasal bir saklama hem de zaman tabanlı bir bekletme ilkesi bulunabilir. Geçerli saklama süresi sona erse bile, bu kapsayıcıdaki tüm Bloblar, tüm yasal tutmalar temizlenene kadar sabit durumda kalır. Buna karşılık, bir blob, yasal saklama süresi sona erene kadar sabit bir durumda kalır, ancak tüm yasal tutmalar temizlenmiş olsa da geçerlidir.

**Yasal saklama ilkeleri yalnızca yasal bir şekilde geçerlidir veya başka kullanım senaryolarında bulunabilir mi?**

Hayır, yasal tutma yalnızca zamana bağlı olmayan bir bekletme ilkesi için kullanılan genel bir terimdir. Yalnızca, ile ilgili bir yaklaşım için kullanılması gerekmez. Yasal saklama ilkeleri, bekletme döneminin bilinmediği önemli kurumsal solucan verilerini korumak için üzerine yazma ve silmeleri devre dışı bırakmak için yararlıdır. Görev açısından kritik solucan iş yüklerinizi korumak için bir kuruluş ilkesi olarak kullanabilir veya özel bir olay tetikleyicisi zaman tabanlı bir bekletme ilkesi kullanımını gerektirdiğinden bunu bir hazırlama ilkesi olarak kullanabilirsiniz. 

**_Kilitli_ bir zaman tabanlı bekletme ilkesi veya yasal tutmayı kaldırabilir miyim?**

Yalnızca kilitlenmemiş süre tabanlı bekletme ilkeleri bir kapsayıcıdan kaldırılabilir. Zaman tabanlı bekletme ilkesi kilitlendikten sonra kaldırılamaz; Yalnızca etkili Bekletme dönemi uzantılarına izin verilir. Yasal saklama etiketleri silinebilir. Tüm yasal Etiketler silindiğinde, yasal saklama kaldırılır.

**Zaman tabanlı saklama veya yasal tutma ilkesi ile *kilitlenmiş* bir kapsayıcıyı silmeye çalışırsam ne olur?**

Kilitli zaman tabanlı bekletme ilkesine veya yasal tutmaya sahip en az bir blob varsa, kapsayıcıyı silme işlemi başarısız olur. Kapsayıcıyı silme işlemi yalnızca etkin saklama aralığı olan bir blob yoksa ve geçerli tutar yoksa başarılı olur. Kapsayıcıyı silebilmek için Blobları silmeniz gerekir.

**Zaman tabanlı saklama veya yasal tutma ilkesi ile *kilitlenmiş* bir WORM kapsayıcısı bulunan depolama hesabını silmeye çalışırsam ne olur?**

Yasal tutma ilkesine veya zaman tabanlı saklama aralığına sahip en az bir WORM kapsayıcısı olması halinde depolama hesabı silme işlemi başarısız olur. Depolama hesabını silebilmeniz için önce tüm solucan kapsayıcılarını silmeniz gerekir. Kapsayıcı silme hakkında daha fazla bilgi için önceki soruya bakın.

**Blob sabit durumda olduğunda farklı blob katmanları (sık erişimli, seyrek erişimli, soğuk depolama) arasında veri aktarımı gerçekleştirebilir miyim?**

Evet, verileri uyumlu sabit durumda tutarken blob katmanlarında verileri taşımak için blob katmanını ayarla komutunu kullanabilirsiniz. Sabit depolama, sık erişimli, seyrek erişimli ve arşiv blob katmanlarında desteklenir.

**Ödemeyi yapamadıysam ve saklama süresi dolmadıysa ne olur?**

Ödeme dışı durumda, normal veri saklama ilkeleri Microsoft ile sözleşmenizin hüküm ve koşullarından dolayı geçerli olur.

**Özelliği deneyebilmek için deneme veya yetkisiz kullanım süresi sunuyor musunuz?**

Evet. Zaman tabanlı bir bekletme ilkesi ilk oluşturulduğunda, *kilidi açılmış* durumda olur. Bu durumda, bekletme aralığında (artış veya azaltma, hatta ilkeyi silme gibi) istediğiniz değişikliği yapabilirsiniz. İlke kilitlendikten sonra, saklama aralığı sona erene kadar kilitli kalır. Bu kilitli ilke, saklama aralığına silme ve değiştirme yapılmasını engeller. *Kilidi açık* durumdan yalnızca deneme amacıyla kullanmanızı ve ilkeyi 24 saatlik bir süre içinde kilitlemeyi önemle öneririz. Bu uygulamalar, SEC 17A-4 (f) ve diğer yönetmeliklere uyum sağlamanıza yardımcı olur.

**Sabit blob ilkeleriyle birlikte geçici silme kullanabilir miyim?**

Evet. [Azure Blob depolama Için geçici silme](storage-blob-soft-delete.md) , yasal bir saklama veya zaman tabanlı bekletme ilkesinden bağımsız olarak bir depolama hesabındaki tüm kapsayıcılar için geçerlidir. Herhangi bir sabit solucan İlkesi uygulanmadan ve onaylanmadan önce ek koruma için geçici silme özelliğinin etkinleştirilmesini öneririz.

**Özellik nerede kullanılabilir?**

Sabit depolama, Azure genel, Çin ve kamu bölgelerinde kullanılabilir. Bölgenizde sabit depolama alanı yoksa, lütfen desteğe ve e-posta azurestoragefeedback@microsoft.combaşvurun.

## <a name="next-steps"></a>Sonraki adımlar

[BLOB depolama için dengesde kullanılabilirlik ilkelerini ayarlama ve yönetme](storage-blob-immutability-policies-manage.md)