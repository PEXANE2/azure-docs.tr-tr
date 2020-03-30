---
title: Değişmez blob depolama - Azure Depolama
description: Azure Depolama, blob (nesne) depolama için kullanıcıların verileri belirli bir aralıkta silinemez, değiştirilemez bir durumda depolamasını sağlayan WORM (Write Once, Read Many) desteği sunar.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: tamram
ms.reviewer: hux
ms.subservice: blobs
ms.openlocfilehash: a980c7bd068a463956191eece43ec1be233e7890
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367627"
---
# <a name="store-business-critical-blob-data-with-immutable-storage"></a>İş açısından kritik blob verilerini değişmez depolama yla depolama

Azure Blob depolama için değişmez depolama, kullanıcıların işle ilgili kritik veri nesnelerini WORM (Bir Kez Yaz, Çok Oku) durumunda depolamasına olanak tanır. Bu durum, verileri kullanıcı tarafından belirtilen bir aralık için eritilemez ve değiştirilemez hale getirir. Bekletme aralığı süresince, lekeler oluşturulabilir ve okunabilir, ancak değiştirilemez veya silinemez. Değişmez depolama, tüm Azure bölgelerinde genel amaçlı v1, genel amaçlı v2, BlobStorage ve BlockBlobStorage hesapları için kullanılabilir.

Azure portalı, PowerShell veya Azure CLI'yi kullanarak yasal dayanakları nasıl ayarlayıp temizleyeceğimiz veya bunları oluşturabilirsiniz hakkında bilgi için [Blob depolama için uygun olmayan ilkeleri ayarlayın ve yönetin.](storage-blob-immutability-policies-manage.md)

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="about-immutable-blob-storage"></a>Değişmez Blob depolama hakkında

Değişmez depolama, sağlık kuruluşu, finans kurumları ve&mdash;ilgili sektörlerin&mdash;özellikle broker-bayi kuruluşlarının verileri güvenli bir şekilde depolamasına yardımcı olur. Değişmez depolama da değişiklik veya silme karşı kritik verileri korumak için herhangi bir senaryoda kaldıraçlı olabilir.

Tipik kullanım alanları şunlardır:

- **Mevzuata uygunluk**: Azure Blob depolama için değişmez depolama, kuruluşların SEC 17a-4(f), CFTC 1.31(d), FINRA ve diğer düzenlemeleri ele almalarına yardımcı olur. Cohasset Associates tarafından hazırlanan teknik bir teknik inceleme, Değişmez depolamanın bu düzenleyici gereksinimleri nasıl karşılar, [Microsoft Service Trust Portal](https://aka.ms/AzureWormStorage)üzerinden indirilebilir olduğunu ayrıntılarıyla anlatır. [Azure Güven Merkezi,](https://www.microsoft.com/trustcenter/compliance/compliance-overview) uyumluluk sertifikalarımız hakkında ayrıntılı bilgiler içerir.

- **Güvenli belge saklama**: Azure Blob depolama için değişmez depolama, hesap yönetim ayrıcalıklarına sahip kullanıcılar da dahil olmak üzere verilerin hiçbir kullanıcı tarafından değiştirilememesini veya silinememesini sağlar.

- **Yasal bekleme**: Azure Blob depolama için değişmez depolama, kullanıcıların dava veya iş kullanımı açısından kritik öneme sahip hassas bilgileri, bekleme kaldırılana kadar istenilen süre boyunca kurcalamaya dayanıklı bir durumda depolamalarına olanak tanır. Bu özellik yalnızca yasal kullanım durumlarla sınırlı değildir, ancak olay tetikleyicilerine veya şirket ilkesine dayalı verileri koruma gereksiniminin gerekli olduğu olay tabanlı bir tutma veya kuruluş kilidi olarak da düşünülebilir.

Değişmez depolama aşağıdaki özellikleri destekler:

- **[Zamana dayalı bekletme ilkesi desteği](#time-based-retention-policies)**: Kullanıcılar verileri belirli bir aralıkta depolamak için ilkeler ayarlayabilir. Zaman tabanlı bekletme ilkesi ayarlandığında, blobs oluşturulabilir ve okunabilir, ancak değiştirilemez veya silinmez. Bekletme süresi dolduktan sonra, lekeler silinebilir, ancak üzerine yazılamaz.

- **[Yasal tutma ilkesi desteği](#legal-holds)**: Bekletme aralığı bilinmiyorsa, kullanıcılar yasal tutma temizlenene kadar değişmez verileri depolamak için yasal tutmalar ayarlayabilir.  Yasal tutma ilkesi ayarlandığında, blobs oluşturulabilir ve okunabilir, ancak değiştirilemez veya silinmez. Her yasal tutma, tanımlayıcı dizesi olarak kullanılan kullanıcı tanımlı alfasayısal etiketle (servis talebi kimliği, olay adı, vb.) ilişkilidir. 

- **Tüm blob katmanları için destek**: WORM ilkeleri Azure Blob depolama katmanından bağımsızdır ve tüm katmanlar için geçerlidir: sıcak, serin ve arşiv. Kullanıcılar verileri iş yüklerine göre en uygun maliyetli katmanda depolarken verilerin de sabit tutulmasını sağlayabilir.

- **Kapsayıcı düzeyinde yapılandırma**: Kullanıcılar zaman tabanlı bekletme ilkelerini ve yasal bekleme etiketlerini kapsayıcı düzeyinde yapılandırabilir. Kullanıcılar basit kapsayıcı düzeyi ayarlarını kullanarak zamana bağlı saklama ilkelerini oluşturup kilitleme, saklama aralıklarını uzatma ve yasal tutma ayarlayıp silme gibi daha birçok işlem yapabilir. Bu ilkeler kapsayıcıdaki mevcut ve yeni tüm bloblara uygulanır.

- **Denetim günlüğü desteği**: Her kapsayıcı bir ilke denetim günlüğü içerir. Kilitli zaman tabanlı bekletme ilkeleri için yedi saate kadar bekletme komutunu gösterir ve kullanıcı kimliğini, komut türünü, zaman damgalarını ve bekletme aralığını içerir. Yasal tutma olaylarında günlük girişinde kullanıcı kimliği, komut türü, zaman damgaları ve yasal tutma etiketleri yer alır. Bu günlük, SEC 17a-4(f) düzenleyici yönergelerine uygun olarak, politikanın kullanım ömrü boyunca saklanır. [Azure Etkinlik Günlüğü,](../../azure-monitor/platform/platform-logs-overview.md) tüm kontrol düzlemi etkinliklerinin daha kapsamlı bir günlüğünü gösterir; [azure tanı günlüklerini](../../azure-monitor/platform/platform-logs-overview.md) etkinleştirirken veri düzlemi işlemlerini korur ve gösterir. Düzenlemeler veya diğer amaçlar doğrultusunda ihtiyaç duyulabilecek günlüklerin düzenli olarak depolanması kullanıcının sorumluluğundadır.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure Blob depolama için sabit depolama özelliği, iki WORM veya sabit ilke türünü destekler: zamana bağlı saklama ve yasal tutma. Bir kapsayıcıya zaman tabanlı saklama ilkesi veya yasal bekleme uygulandığında, varolan tüm lekeler 30 saniyeden kısa bir sürede değişmez bir WORM durumuna geçer. Bu ilke korumalı kapsayıcıya yüklenen tüm yeni lekeler de değişmez bir duruma taşınır. Tüm lekeler değişmez bir duruma geldiklerinde, değişmez ilke onaylanır ve değişmez kapsayıcıdaki herhangi bir işlem üzerinde veya silinmesine izin verilmez.

Konteyner ve depolama hesabı silme işlemi, yasal bir bekleme veya kilitli zaman tabanlı bir ilke tarafından korunan bir kapsayıcıda herhangi bir blob varsa da izin verilmez. Yasal bir bekleme ilkesi blob, konteyner ve depolama hesabı silme karşı koruyacaktır. Hem kilitsiz hem de kilitli zaman tabanlı ilkeler, belirtilen süre boyunca blob silmeye karşı koruma sağlayacaktır. Hem kilitsiz hem de kilitli zaman tabanlı ilkeler, yalnızca kapsayıcının içinde en az bir blob varsa, kapsayıcı silme işlemine karşı koruma sağlayacaktır. Yalnızca *kilitli* zaman tabanlı ilkeile bir kapsayıcı depolama hesabı silmelerine karşı korur; Kilitsiz zaman tabanlı ilkelere sahip kapsayıcılar depolama hesabı silme koruması veya uyumluluk sağlamaz.

Zaman tabanlı bekletme ilkelerini ayarlama ve kilitleme hakkında daha fazla bilgi için [Blob depolama için uygun olmayan ilkeleri ayarla ve yönetin.](storage-blob-immutability-policies-manage.md)

## <a name="time-based-retention-policies"></a>Zamana dayalı bekletme ilkeleri

> [!IMPORTANT]
> Blobun SEC 17a-4(f) ve diğer mevzuata uygunluk için uyumlu değişmez (korumalı yazma ve silme) durumunda olması için zamantabanlı bekletme ilkesi *kilitlenmelidir.* Politikayı genellikle 24 saatten kısa bir süre içinde kilitlemenizi öneririz. Uygulanan zaman tabanlı bekletme ilkesinin ilk durumu *açılır*ve bu özellik özelliği ni sınamanıza ve kilitlemeden önce ilkede değişiklik yapmanıza olanak sağlar. *Kilitsiz* durum kullanılabilirlik koruması sağlarken, *kilitlenme* durumu kısa süreli özellik denemeleri dışında herhangi bir amaçla kullanmanızı önermiyoruz. 

Bir kapsayıcıya zaman tabanlı bekletme ilkesi uygulandığında, kapsayıcıdaki tüm lekeler *etkili* bekletme süresi boyunca değişmez durumda kalır. Blobs için etkili bekletme süresi, blob'un **oluşturma süresi** ile kullanıcı tarafından belirtilen bekletme aralığı arasındaki farka eşittir. Kullanıcılar saklama süresini uzatabildiğinden, sabit depolama geçerli olan saklama süresinin hesaplanmasında kullanıcı tarafından belirtilen en son saklama aralığı değeri kullanılır.

Örneğin, bir kullanıcının beş yıllık bekletme aralığıyla zaman tabanlı bir bekletme ilkesi oluşturduğunu varsayalım. Bu kapta mevcut bir leke, _testblob1_, bir yıl önce oluşturuldu; bu nedenle, _testblob1_ için etkili saklama süresi dört yıldır. Yeni bir blob, _testblob2_, konteyner yüklendiğinde, _testblob2_ için etkili saklama süresi oluşturulduktan sonra beş yıldır.

Yalnızca özellik testi için kilitsiz zaman tabanlı bir saklama ilkesi önerilir ve SEC 17a-4(f) ve diğer mevzuata uygunlukla uyumlu olması için bir ilke kilitlenmelidir. Zaman ayarı tabanlı bekletme ilkesi kilitlendikten sonra, ilke kaldırılamaz ve etkin saklama süresine en fazla beş artışa izin verilir.

Aşağıdaki sınırlar bekletme ilkeleri için geçerlidir:

- Bir depolama hesabı için, kilitli zaman tabanlı değişmez ilkelere sahip kapsayıcıların maksimum sayısı 10.000'dir.
- Minimum bekletme aralığı 1 gündür. Maksimum 146.000 gün (400 yıl) olduğunu.
- Bir kapsayıcı için, kilitli zaman tabanlı değişmez ilkeler için bekletme aralığını uzatmak için en fazla olan düzenler 5'tir.
- Bir kapsayıcı için, kilitli bir ilke için en fazla yedi zaman tabanlı bekletme ilkesi denetim günlüğü tutulur.

### <a name="allow-protected-append-blobs-writes"></a>Korumalı ek blobs yazmaizin

Ek blobs veri blokları oluşur ve denetim ve günlük senaryoları tarafından gerekli veri ek işlemleri için optimize edilmiştir. Tasarım olarak, ek lekeler sadece blob sonuna yeni blokların eklenmesine izin verir. Ne olursa olsun immutability, değişiklik veya bir ek blob mevcut blokların silinmesi temelde izin verilmez. Ek blobs hakkında daha fazla bilgi edinmek için, [Append Blobs hakkında](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs)bakın.

Yalnızca zaman tabanlı bekletme `allowProtectedAppendWrites` ilkeleri, bir ek blob'a yeni bloklar yazmaya olanak tanıyan bir ayar alırken, bir yandan da karşılama koruması ve uyumluluğu korur. Etkinleştirilirse, doğrudan ilke korumalı kapsayıcıda bir ek blob oluşturmanıza ve *AppendBlock* API'sını kullanarak varolan ek bloblarının sonuna yeni veri blokları eklemeye devam edebilirsiniz. Yalnızca yeni bloklar eklenebilir ve varolan bloklar değiştirilemez veya silinemez. Etkin bekletme süresi geçene kadar ekteki blobun silinmesini engelleyerek zaman tutma uygulanabilirliği koruması hala geçerlidir. Bu ayarı etkinleştirmek blok lekeleri veya sayfa lekeleri değişmezlik davranışını etkilemez.

Bu ayar zaman tabanlı bekletme ilkesinin bir parçası olduğundan, ek blobs *etkili* bekletme süresi boyunca değişmez durumda kalmaya devam eder. Yeni veriler ekteki blob'un ilk oluşturmanın ötesine eklenebilir olduğundan, bekletme süresinin nasıl belirlendiği konusunda küçük bir fark vardır. Etkili bekletme, ek blob'un **son değişiklik süresi** ile kullanıcı tarafından belirtilen bekletme aralığı arasındaki farktır. Benzer şekilde bekletme aralığı genişletildiğinde, değişmez depolama etkili bekletme süresini hesaplamak için kullanıcı tarafından belirtilen bekletme aralığının en son değerini kullanır.

Örneğin, bir kullanıcının etkin leştirilmiş ve 90 `allowProtectedAppendWrites` günlük bekletme aralığına sahip zaman tabanlı bir bekletme ilkesi oluşturduğunu varsayalım. Bir ek blob, _logblob1_, bugün konteyner oluşturulur, yeni günlükleri önümüzdeki 10 gün boyunca ek blob eklenmeye devam; bu yüzden, _logblob1_ için etkili saklama süresi bugünden itibaren 100 gündür (son ekinin zamanı + 90 gün).

Kilitsiz zaman tabanlı bekletme `allowProtectedAppendWrites` ilkeleri, ayarının herhangi bir zamanda etkinleştirilmesine ve devre dışı tutulmasına olanak sağlar. Zaman tabanlı bekletme ilkesi kilitlendikten sonra `allowProtectedAppendWrites` ayar değiştirilemez.

Yasal tutma politikaları `allowProtectedAppendWrites` etkinleştiremez ve herhangi bir yasal tutma 'allowProtectedAppendWrites' özelliğini geçersiz kılar. `allowProtectedAppendWrites` Etkinleştirilmiş zaman tabanlı bir saklama ilkesine yasal bir bekleme uygulanırsa, yasal bekleme kaldırılana kadar *AppendBlock* API başarısız olur.

## <a name="legal-holds"></a>Yasal tutma

Yasal dayanaklar, yasal soruşturma amaçları veya genel koruma politikaları için kullanılabilecek geçici tutarlardır. Her yasal tutma ilkesinin bir veya daha fazla etiketle ilişkilendirilmesi gerekir. Etiketler, tutmanın amacını kategorilere ayırmak ve açıklamak için servis talebi kimliği veya olay gibi adlandırılmış bir tanımlayıcı olarak kullanılır.

Bir kapsayıcı nın hem yasal tutma hem de zamantabanlı saklama ilkesi aynı anda olabilir. Geçerli saklama süresi sona erse dahi tüm yasal tutma durumları kaldırılana kadar kapsayıcı içindeki tüm bloblar sabit durumda kalır. Buna karşın tüm yasal tutma ilkeleri silinse dahi geçerli olan saklama süresi boyunca blob sabit durumda kalır.

Aşağıdaki sınırlar yasal tutarlar için geçerlidir:

- Bir depolama hesabı için, yasal bekleme ayarı olan maksimum kapsayıcı sayısı 10.000'dir.
- Bir kapsayıcı için, yasal bekleme etiketleri maksimum sayısı 10'dur.
- Yasal bekleme etiketinin minimum uzunluğu üç alfasayısal karakterdir. Maksimum uzunluk 23 alfasayısal karakterdir.
- Bir kapsayıcı için, ilke süresince en fazla 10 yasal tutma ilkesi denetim günlüğü tutulur.

## <a name="scenarios"></a>Senaryolar
Aşağıdaki tablo, farklı değişmez senaryolar için devre dışı bırakılan Blob depolama işlemleri türlerini gösterir. Daha fazla bilgi için [Azure Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) belgelerine bakın.

|Senaryo  |Blob durumu  |Blob işlemleri reddedildi  |Konteyner ve hesap koruması
|---------|---------|---------|---------|
|Blobdaki geçerli saklama süresi dolmadı ve/veya yasal tutma ayarlandı     |Sabit: hem silme hem de yazma korumalı         | Blob<sup>1,</sup>Put Block<sup>1,</sup>Put Block List<sup>1</sup>, Sil Kapsayıcı, Blob Sil, Blob Metadata Set, Put Page, Set Blob Özellikleri, Snapshot Blob, Artımlı Kopya Blob, Ek Blok<sup>2</sup>         |Konteyner silme reddedildi; Depolama Hesabı silme reddedildi         |
|Blob üzerinde etkili saklama süresi doldu ve hiçbir yasal tutma ayarlanır    |Yalnızca yazma korumalı (silme işlemlerine izin verilir)         |Blob<sup>1</sup>koy , Blok<sup>1</sup>koymak , Blok Listesi<sup>1</sup>koyun , Set Blob Metadata, Put Page, Set Blob Özellikleri, Snapshot Blob, Artımlı Kopya Blob, Ek Blok<sup>2</sup>         |Korunan konteyner içinde en az 1 blob varsa konteyner silme reddedilir; Depolama Hesabı silme yalnızca *kilitli* zaman tabanlı ilkeler için reddedildi         |
|WORM ilkesi uygulanmaz (zamantabanlı saklama ve yasal bekleme etiketi yok)     |Değiştirilebilir         |None         |None         |

<sup>1</sup> Blob hizmeti, bu işlemlerin bir kez yeni bir blob oluşturmasına olanak tanır. Değişmez bir kapsayıcıda varolan bir blob yolunda sonraki tüm overwrite işlemlerine izin verilmez.

<sup>2</sup> Append Block'a yalnızca `allowProtectedAppendWrites` özellik etkinken zaman ayarı ilkesi için izin verilir. Daha fazla bilgi [için, Korumalı Ek Blobs Yazmaları](#allow-protected-append-blobs-writes) Bölümüne bakın.

## <a name="pricing"></a>Fiyatlandırma

Bu özelliği kullanmak için ek ücret alınmaz. Değişmez veriler, mutable verilerle aynı şekilde fiyatlandırılır. Azure Blob depolama alanıyla ilgili fiyatlandırma ayrıntıları için [Azure Depolama fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/blobs/)bakın.

## <a name="faq"></a>SSS

**WORM uyumluluğu ile ilgili belgeleri sağlayabilir misiniz?**

Evet. Microsoft, uygunluğu belgelemek için, kayıt yönetimi ve bilgi yönetimi konusunda uzmanlaşmış lider bir bağımsız değerlendirme firması olan Cohasset Associates'i, değişmez Blob depolamasını ve finansal hizmetler sektöründe. Cohasset, worm durumunda zaman tabanlı Blobs tutmak için kullanıldığında değişmez Blob depolama, CFTC Kural 1.31(c)-(d), FINRA Kural 4511 ve SEC Kural 17a-4 ilgili depolama gereksinimlerini karşılar doğrulanır. Microsoft, finansal kurumlar için kayıtların saklanması için küresel olarak en açıklayıcı kılavuzu temsil ettiği için bu kurallar kümesini hedeflemidir. Cohasset raporu [Microsoft Service Trust Center'da](https://aka.ms/AzureWormStorage)kullanılabilir. WORM immutability uyumluluğu ile ilgili olarak Microsoft'tan bir attestation mektubu istemek için lütfen Azure desteğine başvurun.

**Bu özellik yalnızca blobs ve append blobs engellemek için mi, ya da sayfa lekeleri için de geçerli mi?**

Değişmez depolama kapsayıcı düzeyinde ayarlanır gibi herhangi bir blob türü ile kullanılabilir, ancak özellikle blok lekeleri ve ek lekeleri depolar kapsayıcılar için WORM kullanmanızı öneririz. Bir kapsayıcıdaki varolan lekeler yeni ayarlanmış bir WORM ilkesi yle korunur. Ancak worm kapsayıcısının dışında yeni sayfa lekeleri oluşturulması ve sonra kopyaedilmesi gerekir. WORM kapsayıcısına kopyalandıktan sonra, sayfa lekesinde başka değişiklik yapılmasına izin verilmez. Herhangi bir etkin sanal makine için VHD'leri (sayfa lekeleri) depolayan bir kapsayıcıda WORM ilkesinin ayarlanması, VM diskini kilitleyeceği için önerilmez. Zamana dayalı ilkeleri kilitlemeden önce belgeleri ayrıntılı olarak gözden geçirmenizi ve senaryolarınızı test etmenizi öneririz.

**Bu özelliği kullanmak için yeni bir depolama hesabı oluşturmam gerekiyor mu?**

Hayır, mevcut veya yeni oluşturulan genel amaçlı v1, genel amaçlı v2, BlobStorage veya BlockBlobStorage hesapları ile değişmez depolama kullanabilirsiniz. Genel amaçlı v1 depolama hesapları desteklenir, ancak daha fazla özellikten yararlanabileceğiniz genel amaçlı v2'ye yükseltmenizi öneririz. Varolan genel amaçlı bir v1 depolama hesabını yükseltme hakkında bilgi için [bkz.](../common/storage-account-upgrade.md)

**Hem yasal tutma hem de zamana dayalı saklama politikası uygulayabilir miyim?**

Evet, bir kapsayıcı nın hem yasal tutma hem de zamana dayalı saklama ilkesi aynı anda olabilir; ancak, 'allowProtectedAppendWrites' ayarı yasal bekleme temizlenene kadar geçerli olmayacaktır. Geçerli saklama süresi sona erse dahi tüm yasal tutma durumları kaldırılana kadar kapsayıcı içindeki tüm bloblar sabit durumda kalır. Buna karşın tüm yasal tutma ilkeleri silinse dahi geçerli olan saklama süresi boyunca blob sabit durumda kalır. 

**Yasal tutma politikaları sadece yasal işlemler için mi dir, yoksa başka kullanım senaryoları mı var?**

Hayır, Legal Hold sadece zamana dayalı olmayan bir saklama ilkesi için kullanılan genel terimdir. Sadece dava ile ilgili işlemler için kullanılması gerekmez. Yasal Tutma ilkeleri, bekletme süresinin bilinmemesi gereken önemli kuruluş WORM verilerini korumak için üzerine yazmayı devre dışı bırakmak ve silmek için yararlıdır. Görevinizi kritik WORM iş yüklerini korumak için bir kuruluş ilkesi olarak kullanabilirsiniz veya özel bir olay tetikleyicizaman tabanlı bekletme ilkesi kullanımını gerektirmeden önce bir evreleme ilkesi olarak kullanabilirsiniz. 

**_Kilitli_ zaman tabanlı saklama politikasını veya yasal beklemeyi kaldırabilir miyim?**

Yalnızca kilitsiz zaman tabanlı bekletme ilkeleri bir kapsayıcıdan kaldırılabilir. Zaman tabanlı bekletme ilkesi kilitlendikten sonra kaldırılamaz; yalnızca etkili saklama süresi uzantılarına izin verilir. Yasal bekleme etiketleri silinebilir. Tüm yasal etiketler silindiğinde, yasal tutma kaldırılır.

**Zamana dayalı saklama ilkesi veya yasal tutma ile bir kapsayıcı silmeye çalışırsam ne olur?**

Kilitlenmiş veya kilitlenmiş zaman tabanlı saklama ilkesine sahip konteynerin içinde en az bir blob varsa veya kapsayıcıyasal bir ambarı varsa, Sil Kapsayıcısı işlemi başarısız olur. Sil Kapsayıcısı işlemi yalnızca kapsayıcının içinde blob yoksa ve yasal tutar lar yoksa başarılı olur. 

**Zamana dayalı saklama ilkesi veya yasal tutma olan bir kapsayıcıyla bir depolama hesabını silmeye çalışırsam ne olur?**

Yasal tutma kümesine sahip en az bir kapsayıcı veya **kilitli** zaman tabanlı bir ilke varsa depolama hesabı silme işlemi başarısız olur. Kilitsiz zaman tabanlı bir ilke içeren bir kapsayıcı, depolama hesabı silmeişlemine karşı koruma sağlamaz. Depolama hesabını silmeden önce tüm yasal saklamaları kaldırmalı ve **tüm kilitli** kapsayıcıları silmeniz gerekir. Kapsayıcı silme hakkında bilgi için önceki soruya bakın. [Azure Kaynak Yöneticisi kilitleri](../../azure-resource-manager/management/lock-resources.md)ile depolama hesabınız için daha fazla silme koruması da uygulayabilirsiniz.

**Blob değişmez durumdayken verileri farklı blob katmanları (sıcak, serin, arşiv) arasında taşıyabilir miyim?**

Evet, verileri uyumlu değişmez durumda tutarken verileri blob katmanları arasında taşımak için Blob Tier Ayarını Ayarla komutunu kullanabilirsiniz. Değişmez depolama sıcak, serin ve arşiv blob katmanlarında desteklenir.

**Ödemeyi yapamadıysam ve saklama süresi dolmadıysa ne olur?**

Ödeme olmaması durumunda, Normal veri saklama ilkeleri, Microsoft ile olan sözleşmenizin hüküm ve koşullarında öngörüldüğü şekilde geçerli olacaktır. Genel bilgi için [Microsoft'ta Veri yönetimi](https://www.microsoft.com/en-us/trust-center/privacy/data-management)ne bürünme bilgi. 

**Özelliği deneyebilmek için deneme veya yetkisiz kullanım süresi sunuyor musunuz?**

Evet. Zaman tabanlı bekletme ilkesi ilk oluşturulduğunda, *kilitsiz* durumdadır. Bu durumda, bekletme aralığında artırma veya azaltma gibi istediğiniz değişikliği yapabilir ve hatta ilkeyi silebilirsiniz. İlke kilitlendikten sonra bekletme aralığı sona erene kadar kilitli kalır. Bu kilitli ilke silme ve bekletme aralığında değişiklik önler. *Kilitsiz* durumu yalnızca deneme amaçlı kullanmanızı ve poliçeyi 24 saatlik bir süre içinde kilitlemenizi önemle tavsiye ediyoruz. Bu uygulamalar SEC 17a-4(f) ve diğer düzenlemelere uymanıza yardımcı olur.

**Değişmez blob ilkelerinin yanında yumuşak silme kullanabilir miyim?**

Evet, uyumluluk gereksinimleriniz yumuşak silmenin etkinleştirilmesine izin veriyorsa. [Azure Blob depolama alanı için yumuşak silme,](storage-blob-soft-delete.md) yasal tutma veya zamana dayalı saklama ilkesine bakılmaksızın bir depolama hesabındaki tüm kapsayıcılar için geçerlidir. Değişmez WORM ilkeleri uygulanmadan ve onaylanmadan önce ek koruma için yumuşak silmeyi etkinleştirmenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

- [Blob depolama için uygun olmayan ilkeler belirleyin ve yönetin](storage-blob-immutability-policies-manage.md)
- [Yaşam döngüsü yönetimiyle blob verilerini otomatik olarak katmanlamak ve silmek için kurallar ayarlama](storage-lifecycle-management-concepts.md)
- [Azure Depolama blobları için geçici silme](../blobs/storage-blob-soft-delete.md)
- [Azure Kaynak Yöneticisi kilitleriyle abonelikleri, kaynak gruplarını ve kaynakları koruyun.](../../azure-resource-manager/management/lock-resources.md)
