---
title: Paylaşılan erişim imzaları (SAS) ile verilere sınırlı erişim izni verme
titleSuffix: Azure Storage
description: Blobs, kuyruklar, tablolar ve dosyalar da dahil olmak üzere Azure Depolama kaynaklarına erişimi devretmek için paylaşılan erişim imzalarını (SAS) kullanma hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 7a5967f52a187fe289c6fb1ca72af2d5fd17f010
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255242"
---
# <a name="grant-limited-access-to-azure-storage-resources-using-shared-access-signatures-sas"></a>Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni verme

Paylaşılan erişim imzası (SAS), verilerinizin güvenliğinden ödün vermeden depolama hesabınızdaki kaynaklara güvenli temsilci erişimi sağlar. SAS ile, istemcinin verilerinize nasıl erişebileceği üzerinde ayrıntılı denetime sahipsiniz. İstemcinin hangi kaynaklara erişebileceğini, bu kaynaklarda hangi izinlere sahip olduğunu ve diğer parametrelerin yanı sıra SAS'ın ne kadar süreyle geçerli olduğunu denetleyebilirsiniz.

## <a name="types-of-shared-access-signatures"></a>Paylaşılan erişim imzası türleri

Azure Depolama üç tür paylaşılan erişim imzaını destekler:

- **Kullanıcı delegasyonu SAS.** Bir kullanıcı delegasyonu SAS, Azure Etkin Dizin (Azure AD) kimlik bilgileri ve SAS için belirtilen izinlerle güvence altına alınmıştır. Bir kullanıcı delegasyonu SAS yalnızca Blob depolama için geçerlidir.

    Kullanıcı delegasyonu SAS hakkında daha fazla bilgi için [bkz.](/rest/api/storageservices/create-user-delegation-sas)

- **Hizmet SAS.** Bir hizmet SAS depolama hesabı anahtarı ile güvenlidir. Bir hizmet SAS, Azure Depolama hizmetlerinden yalnızca birinde bir kaynağa erişir: Blob depolama, Sıra depolama, Tablo depolama veya Azure Dosyaları. 

    Hizmet SAS hakkında daha fazla bilgi için [bkz.](/rest/api/storageservices/create-service-sas)

- **Hesap SAS.** Bir hesap SAS depolama hesabı anahtarı ile güvenlidir. Hesap SAS ise bir veya daha fazla depolama hizmetindeki kaynaklara erişim atar. Bir hizmet veya kullanıcı delegasyonu SAS aracılığıyla kullanılabilen tüm işlemler de bir hesap SAS üzerinden kullanılabilir. Ayrıca, SAS hesabıyla, **Hizmet Özelliklerini Al** ve Hizmet **İstatistikleri Al** işlemleri gibi hizmet düzeyinde geçerli olan işlemlere erişimi devredebilirsiniz. Bununla birlikte hizmet SAS ile izin verilmeyen blob kapsayıcılar, tablolar kuyruklar ve dosya paylaşımları üzerinde okuma, yazma ve silme işlemleri için yetkilendirme yapabilirsiniz. 

    Hesap SAS hakkında daha fazla bilgi için, [bir hesap SAS (REST API) oluşturun.](/rest/api/storageservices/create-account-sas)

> [!NOTE]
> Microsoft, hesap anahtarını kullanmak yerine, daha kolay tehlikeye girebilecek bir güvenlik uygulaması olarak mümkün olduğunda Azure AD kimlik bilgilerini kullanmanızı önerir. Uygulama tasarımınız Blob depolama alanına erişmek için paylaşılan erişim imzaları gerektiriyorsa, üstün güvenlik için mümkün olduğunda bir kullanıcı delegasyonu SAS oluşturmak için Azure AD kimlik bilgilerini kullanın.

Paylaşılan erişim imzası iki formdan birini alabilir:

- **Ad hoc SAS:** Geçici bir SAS oluşturduğunuzda, Başlangıç saati, son kullanma süresi ve SAS izinlerinin tümü SAS URI'de belirtilir (veya başlangıç zamanı atlanırsa zımni olarak). SAS her türlü özel bir SAS olabilir.
- **SAS'yi depolanmış erişim ilkesiyle hizmet eki:** Depolanan erişim ilkesi, blob kapsayıcısı, tablo, sıra veya dosya paylaşımı olabilecek bir kaynak kapsayıcısı üzerinde tanımlanır. Depolanan erişim ilkesi, bir veya daha fazla hizmet paylaşılan erişim imzasının kısıtlamalarını yönetmek için kullanılabilir. Bir hizmet SAS'ı depolanmış bir erişim ilkesiyle ilişkilendirdiğinizde,&mdash;SAS kısıtlamaları başlangıç saati,&mdash;son kullanma tarihi ve depolanan erişim ilkesi için tanımlanan izinleri devralır.

> [!NOTE]
> Bir kullanıcı delegasyonu SAS veya bir hesap SAS geçici bir SAS olmalıdır. Saklanan erişim ilkeleri, kullanıcı delegasyonu SAS veya hesap SAS için desteklenmez.

## <a name="how-a-shared-access-signature-works"></a>Paylaşılan erişim imzası nasıl çalışır?

Paylaşılan erişim imzası, bir veya daha fazla depolama kaynağına işaret eden ve özel bir sorgu parametre kümesi içeren bir belirteç içeren imzalı bir URI'dir. Belirteç, kaynaklara istemci tarafından nasıl erişilebileceğini gösterir. Sorgu parametrelerinden biri olan imza, SAS parametrelerinden oluşturulur ve SAS oluşturmak için kullanılan anahtarla imzalanır. Bu imza, Azure Depolama tarafından depolama kaynağına erişimi yetkilendirmek için kullanılır.

### <a name="sas-signature"></a>SAS imzası

SAS'yi iki şekilde imzalayabilirsiniz:

- Azure Etkin Dizin (Azure AD) kimlik bilgileri kullanılarak oluşturulan *bir kullanıcı delegasyonu anahtarıyla.* Bir kullanıcı delegasyonu SAS kullanıcı delegasyonu anahtarı ile imzalanır.

    Kullanıcı delegasyonu anahtarını almak ve SAS'ı oluşturmak için, Bir Azure REKLAM güvenlik ilkesine **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini içeren rol tabanlı erişim denetimi (RBAC) rolü atanması gerekir. Kullanıcı delegasyonu anahtarını almak için izinlere sahip RBAC rolleri hakkında ayrıntılı bilgi için [bkz.](/rest/api/storageservices/create-user-delegation-sas)

- Depolama hesabı anahtarıyla. Hem hizmet SAS hem de bir hesap SAS depolama hesabı anahtarı ile imzalanır. Hesap anahtarıyla imzalanmış bir SAS oluşturmak için, bir uygulamanın hesap anahtarına erişimi olmalıdır.

### <a name="sas-token"></a>SAS belirteci

SAS belirteci, örneğin Azure Depolama istemci kitaplıklarından birini kullanarak istemci tarafında oluşturduğunuz bir dizedir. SAS belirteci Azure Depolama tarafından hiçbir şekilde izlenmez. İstemci tarafında sınırsız sayıda SAS belirteci oluşturabilirsiniz. Bir SAS oluşturduktan sonra, depolama hesabınızdaki kaynaklara erişim gerektiren istemci uygulamalarına dağıtabilirsiniz.

İstemci uygulaması, isteğin bir parçası olarak Azure Depolama'ya Bir SAS URI sağladığında, hizmet isteği yetkilendirmek için geçerli olup olmadığını doğrulamak için SAS parametrelerini ve imzasını denetler. Hizmet imzanın geçerli olduğunu doğrularsa, istek yetkilidir. Aksi takdirde, istek hata kodu 403 (Yasak) ile reddedilir.

Burada kaynak URI ve SAS belirteci gösteren bir hizmet SAS URI bir örnek:

![Bir hizmet SAS URI bileşenleri](./media/storage-sas-overview/sas-storage-uri.png)

## <a name="when-to-use-a-shared-access-signature"></a>Paylaşılan erişim imzasıne ne zaman kullanılır?

Depolama hesabınızdaki kaynaklara güvenli erişim sağlamak istediğinizde, bu kaynaklara izinleri olmayan herhangi bir istemciye sas kullanın.

SAS'Nin yararlı olduğu yaygın bir senaryo, kullanıcıların kendi verilerini depolama hesabınıza okuyup yazdıkları bir hizmettir. Depolama hesabının kullanıcı verilerini depoladığı bir senaryoda iki tipik tasarım deseni vardır:

1. İstemciler, kimlik doğrulamasını yapan ön uç ara sunucu hizmeti üzerinden verileri karşıya yükleyip indirirler. Bu ön uç proxy hizmeti, iş kurallarının doğrulanmasına izin verme avantajına sahiptir, ancak büyük miktarda veri veya yüksek hacimli hareketler için, talebi eşleşecek şekilde ölçeklendirebilecek bir hizmet oluşturmak pahalı veya zor olabilir.

   ![Senaryo diyagramı: Ön uç proxy hizmeti](./media/storage-sas-overview/sas-storage-fe-proxy-service.png)

1. Basit bir hizmet gerektiğinde istemcinin kimliğini doğrular ve ardından bir SAS oluşturur. İstemci uygulaması SAS'ı aldıktan sonra, depolama hesabı kaynaklarına Doğrudan SAS tarafından tanımlanan izinlerle ve SAS tarafından izin verilen aralıkta erişebilir. SAS tüm verileri ön uç ara sunucu hizmetiyle yönlendirme gereksinimini azaltır.

   ![Senaryo diyagramı: SAS sağlayıcı hizmeti](./media/storage-sas-overview/sas-storage-provider-service.png)

Birçok gerçek dünya hizmetleri bu iki yaklaşımın bir melezk kullanabilirsiniz. Örneğin, bazı veriler ön uç proxy'si aracılığıyla işlenebilir ve doğrulanabilirken, diğer veriler SAS kullanılarak doğrudan kaydedilebilir ve/veya okunabilir.

Ayrıca, belirli senaryolarda bir kopyalama işleminde kaynak nesneye erişimi yetkilendirmek için bir SAS gereklidir:

- Bir blob'u farklı bir depolama hesabında bulunan başka bir blob'a kopyaladiğinizde, kaynak blob'a erişimi yetkilendirmek için bir SAS kullanmanız gerekir. Hedef blob'a erişimi yetkilendirmek için isteğe bağlı olarak bir SAS kullanabilirsiniz.
- Bir dosyayı farklı bir depolama hesabında bulunan başka bir dosyaya kopyaladiğinizde, kaynak dosyaya erişimi yetkilendirmek için bir SAS kullanmanız gerekir. Hedef dosyaya erişimi yetkilendirmek için isteğe bağlı olarak bir SAS kullanabilirsiniz.
- Bir blob'u bir dosyaya veya bir dosyayı blob'a kopyaladığınızı, kaynak ve hedef nesneler aynı depolama hesabı içinde bulunsa bile kaynak nesneye erişimi yetkilendirmek için Bir SAS kullanmanız gerekir.

## <a name="best-practices-when-using-sas"></a>SAS kullanırken en iyi uygulamalar

Uygulamalarınızda paylaşılan erişim imzalarını kullandığınızda, iki olası riskin farkında olmanız gerekir:

- Bir SAS sızdırılırsa, bu belgeyi elde eden herkes tarafından kullanılabilir ve bu da depolama hesabınızı tehlikeye atabilir.
- İstemci uygulamasına sağlanan bir SAS'ın süresi doluyorsa ve uygulama hizmetinizden yeni bir SAS alamıyorsa, uygulamanın işlevselliği engellenebilir.

Paylaşılan erişim imzalarını kullanmak için aşağıdaki öneriler, bu riskleri azaltmaya yardımcı olabilir:

- SAS oluşturmak veya dağıtmak için **her zaman HTTPS'yi kullanın.** Bir SAS HTTP üzerinden geçirilir ve ele geçirilirse, ortadaki adam saldırısını gerçekleştiren bir saldırgan SAS'ı okuyabilir ve daha sonra bunu amaçlanan kullanıcının sahip olabileceği şekilde kullanabilir, hassas verileri tehlikeye atabilir veya kötü amaçlı kullanıcı tarafından veri bozulmasına izin verebilir.
- **Mümkün olduğunda bir kullanıcı delegasyonu SAS kullanın.** Bir kullanıcı delegasyonu SAS bir hizmet SAS veya bir hesap SAS üstün güvenlik sağlar. Bir kullanıcı delegasyonu SAS, hesap anahtarınızı kodunuzla depolamanız gerekmeden Azure AD kimlik bilgileriyle güvenli hale gelir.
- **Bir SAS için bir iptal planı hazırlayın.** Bir SAS tehlikeye girerse yanıt vermeye hazır olduğundan emin olun.
- **Bir hizmet SAS için depolanmış bir erişim ilkesi tanımlayın.** Depolanan erişim ilkeleri, depolama hesabı anahtarlarını yeniden oluşturmak zorunda kalmadan bir hizmet SAS için izinleri iptal etme seçeneği sunar. Gelecekte (veya sonsuz) bu çok uzak üzerinde sona erme ayarlayın ve daha ileriye taşımak için düzenli olarak güncelleştirilmiş olduğundan emin olun.
- **Özel bir SAS hizmeti SAS veya hesap SAS'ta yakın süreli son kullanma sürelerini kullanın.** Bu şekilde, bir SAS tehlikeye bile, sadece kısa bir süre için geçerlidir. Depolanmış bir erişim ilkesine başvurulamıyorsanız, bu uygulama özellikle önemlidir. Yakın vade sonu süreleri, yükleme için kullanılabilir süreyi sınırlayarak bir blob'a yazılabilir veri miktarını da sınırlar.
- **İstemciler gerekirse SAS'ı otomatik olarak yenilemelerini sağlar.** İstemciler, SAS'ı sağlayan hizmet in kullanılamıyorsa yeniden denemeler için zaman tanımak için SAS'yi son kullanma tarihinden önce yenilemelidir. SAS'nizin, son kullanma süresi içinde tamamlanması beklenen az sayıda ki acil ve kısa süreli işlemler için kullanılması gerekiyorsa, SAS'ın yenilenmesi beklenmez. Ancak, sas üzerinden rutin olarak istekte bulunuyor istemciniz varsa, son kullanma süresi nin dolması olasılığı devreye girer. Önemli olan, SAS'ın kısa ömürlü (daha önce de belirtildiği gibi) olması ihtiyacını, müşterinin yeterince erken yenileme talebinde bulunma ihtiyacıyla dengelemektir (Başarılı yenilemeden önce sona eren SAS nedeniyle aksamayı önlemek için).
- **SAS başlangıç saatine dikkat edin.** Bir SAS'ın başlangıç saatini **şimdi**ayarlarsanız, saat çarpıklığı (farklı makinelere göre geçerli zaman farklılıkları) nedeniyle, ilk birkaç dakika boyunca aralıklı olarak hatalar gözlemlenebilir. Genel olarak, başlangıç saatini geçmişte en az 15 dakika olarak ayarlayın. Ya da, her durumda hemen geçerli hale getirecek, hiç ayarlamayın. Aynı genellikle son kullanma süresi için de geçerlidir-herhangi bir istek üzerine her iki yönde de 15 dakikaya kadar saat eğriltme gözlemleyebilir unutmayın. 2012-02-12'den önce REST sürümünü kullanan istemciler için, depolanan bir erişim ilkesine başvurmayan bir SAS için maksimum süre 1 saattir ve bundan daha uzun vadeli belirtilergösteren ilkeler başarısız olur.
- **SAS tarih saati biçimine dikkat edin.** Bir SAS için başlangıç saatini ve/veya son kullanma tarihini ayarlarsanız, bazı yardımcı programlar için (örneğin komut satırı yardımcı programı AzCopy için) tarih formatının '+%Y-%m-%dT%H:%M:%SZ' olması gerekir, özellikle SAS belirteci kullanılarak çalışması için saniyeler de dahil olmak üzere.  
- **Erişilecek kaynağa karşı açık olun.** Güvenlik en iyi uygulama, bir kullanıcıya gereken en az ayrıcalıkları sağlamaktır. Bir kullanıcının yalnızca tek bir varlığa okuma erişimine ihtiyacı varsa, bu tek bir varlığa okuma erişimi hakkı tanıyın ve tüm varlıklara erişimi okuma/yazma/silmeyin. Bu, SAS'ın bir saldırganın elinde daha az güce sahip olması nedeniyle sas tehlikeye girerse hasarı azaltmaya da yardımcı olur.
- **Hesabınızın SAS aracılığıyla da dahil olmak üzere herhangi bir kullanım için faturalandırılacağını anlayın.** Bir blob'a yazma erişimi sağlarsanız, bir kullanıcı 200 GB'lık bir blob yüklemeyi seçebilir. Onlara da okuma erişimi verdiyseniz, 10 kez indirmeyi seçebilirler ve sizin için çıkış ücreti olarak 2 TB'ye maruz kalabilirler. Yine, kötü amaçlı kullanıcıların olası eylemlerini azaltmaya yardımcı olmak için sınırlı izinler sağlayın. Bu tehdidi azaltmak için kısa ömürlü SAS kullanın (ancak son anda saat çarpık dikkatli olun).
- **SAS kullanılarak yazılan verileri doğrulayın.** Bir istemci uygulaması depolama hesabınıza veri yazdığında, bu verilerle ilgili sorunlar olabileceğini unutmayın. Uygulamanız, verilerin kullanıma hazır hale gelmeden önce doğrulanmasını veya yetkilendirilmesini gerektiriyorsa, bu doğrulamayı veriler yazıldıktan ve uygulamanız tarafından kullanılmadan önce gerçekleştirmeniz gerekir. Bu uygulama, SAS'ı düzgün bir şekilde satın alan bir kullanıcı veya sızdırılan bir SAS'tan yararlanan bir kullanıcı tarafından hesabınıza yazılan bozuk veya kötü amaçlı verilere karşı da koruma sağlar.
- **SAS'ı ne zaman kullanmayacağımı bilin.** Bazen depolama hesabınıza yönelik belirli bir operasyonla ilişkili riskler, SAS kullanmanın avantajlarından daha ağır basar. Bu tür işlemler için, iş kuralı doğrulama, kimlik doğrulaması ve denetim yaptıktan sonra depolama hesabınıza yazan orta katman bir hizmet oluşturun. Ayrıca, bazen erişimi başka şekillerde yönetmek daha kolaydır. Örneğin, bir kapsayıcıdaki tüm lekeleri herkese açık olarak okunabilir hale getirmek istiyorsanız, her istemciye erişim için Bir SAS sağlamak yerine kapsayıcıyı Genel hale getirebilirsiniz.
- **Uygulamanızı izlemek için Azure Monitörü ve Azure Depolama günlüklerini kullanın.** Azure Monitörü ve depolama analitiği günlüğe kaydetmeyi, SAS sağlayıcı hizmetinizdeki bir kesinti den kaynaklanan yetkilendirme hatalarında veya depolanmış bir erişim ilkesinin yanlışlıkla kaldırılmasından kaynaklanan ani artışları gözlemlemek için kullanabilirsiniz. Daha fazla bilgi için Azure Monitörü ve [Azure Depolama Analizi günlüğe kaydetmede](storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) [Azure Depolama ölçümlerine](storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) bakın.

## <a name="get-started-with-sas"></a>SAS ile başlayın

Paylaşılan erişim imzalarıyla başlamak için her SAS türü için aşağıdaki makalelere bakın.

### <a name="user-delegation-sas"></a>Kullanıcı delegasyonu SAS

- [PowerShell ile bir konteyner veya blob için bir kullanıcı delegasyonu SAS oluşturun](../blobs/storage-blob-user-delegation-sas-create-powershell.md)
- [Azure CLI ile bir kapsayıcı veya blob için bir kullanıcı delegasyonu SAS oluşturun](../blobs/storage-blob-user-delegation-sas-create-cli.md)
- [.NET ile bir kapsayıcı veya blob için bir kullanıcı delegasyonu SAS oluşturma](../blobs/storage-blob-user-delegation-sas-create-dotnet.md)

### <a name="service-sas"></a>Hizmet SAS

- [.NET ile bir kapsayıcı veya blob için bir hizmet SAS oluşturun](../blobs/storage-blob-service-sas-create-dotnet.md)

### <a name="account-sas"></a>Hesap SAS

- [.NET ile hesap SAS oluşturma](storage-account-sas-create-dotnet.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Paylaşılan erişim imzasıyla temsilci erişimi (REST API)](/rest/api/storageservices/delegate-access-with-shared-access-signature)
- [Bir kullanıcı delegasyonu SAS (REST API) oluşturma](/rest/api/storageservices/create-user-delegation-sas)
- [Hizmet SAS (REST API) oluşturma](/rest/api/storageservices/create-service-sas)
- [Hesap SAS (REST API) oluşturma](/rest/api/storageservices/create-account-sas)
