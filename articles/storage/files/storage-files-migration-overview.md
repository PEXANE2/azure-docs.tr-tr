---
title: Azure dosya paylaşımlarına geçirme
description: Azure dosya paylaşımlarına geçişler hakkında bilgi edinin ve geçiş kılavuzunuzu bulun.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 3/18/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: d6141d48d67dd44c348961c6e09acf4e2531a61e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685983"
---
# <a name="migrate-to-azure-file-shares"></a>Azure dosya paylaşımlarına geçirme

Bu makalede, Azure dosya paylaşımlarına geçişin temel yönleri ele allanmıştır.

Bu makalede, geçiş temelleri ve geçiş kılavuzları tablosu içerir. Bu kılavuzlar, dosyalarınızı Azure dosya paylaşımlarına taşımanıza yardımcı olur. Kılavuzlar, verilerinizin nerede olduğuna ve hangi dağıtım modeline (yalnızca bulut veya karma) taşındığınıza bağlı olarak düzenlenir.

## <a name="migration-basics"></a>Geçiş temelleri

Azure'da birden çok kullanılabilir bulut depolama türü vardır. Azure'a dosya geçişlerinin temel bir yönü, verileriniz için hangi Azure depolama seçeneğinin doğru olduğunu belirlemektir.

[Azure dosya paylaşımları](storage-files-introduction.md) genel amaçlı dosya verileri için uygundur. Bu veriler, şirket içi SMB veya NFS paylaşımını kullandığınız her şeyi içerir. [Azure Dosya Eşitlemi](storage-sync-files-planning.md)ile, Windows Server'ı şirket içinde çalıştıran sunucularda birkaç Azure dosya paylaşımının içeriğini önbelleğe alabilirsiniz.

Şu anda şirket içi bir sunucuda çalışan bir uygulama için, dosyaları Azure dosya paylaşımında depolamak iyi bir seçim olabilir. Uygulamayı Azure'a taşıyabilir ve Azure dosya paylaşımlarını paylaşılan depolama alanı olarak kullanabilirsiniz. Bu senaryo için [Azure Diskleri'ni](../../virtual-machines/windows/managed-disks-overview.md) de düşünebilirsiniz.

Bazı bulut uygulamaları Kobİ'ye veya makine-yerel veri erişimine veya paylaşılan erişime bağlı değildir. Bu uygulamalar için [Azure lekeleri](../blobs/storage-blobs-overview.md) gibi nesne depolama genellikle en iyi seçimdir.

Herhangi bir geçişin anahtarı, dosyalarınızı geçerli depolama konumlarından Azure'a taşınırken geçerli tüm dosya doğrularını yakalamaktır. Azure depolama seçeneğinin ne kadar sadakati desteklediği ve senaryonuzun ne kadar gerektirdiği de doğru Azure depolama alanını seçmenize yardımcı olur. Genel amaçlı dosya verileri geleneksel olarak dosya meta verilerine bağlıdır. Uygulama verileri olmayabilir.

Bir dosyanın iki temel bileşeni şunlardır:

- **Veri akışı**: Bir dosyanın veri akışı dosya içeriğini depolar.
- **Dosya meta verileri**: Dosya meta verileri şu alt bileşenlere sahiptir:
   * Salt okunur gibi dosya öznitelikleri
   * *NTFS izinleri* veya *dosya ve klasör Aç'ları* olarak adlandırılabilen dosya izinleri
   * Zaman damgaları, en önemlisi oluşturma ve son değiştirilmiş zaman damgaları
   * Daha büyük miktarda standart dışı özellikleri depolamak için bir alan olan alternatif bir veri akışı

Bir geçişteki dosya doğruluğu aşağıdaki leri yapma özelliği olarak tanımlanabilir:

- Geçerli tüm dosya bilgilerini kaynakta saklayın.
- Geçiş aracıyla dosyaları aktarın.
- Dosyaları geçişin hedef deposunda depolayın.

Geçişinizin sorunsuz bir şekilde iletilmesini sağlamak [için, gereksinimleriniz için en iyi kopyalama aracını belirleyin](#migration-toolbox) ve bir depolama hedefini kaynağınızla eşleştirin.

Önceki bilgileri göz önünde bulundurarak, Azure'daki genel amaçlı dosyalar için hedef depolama alanının [Azure dosya paylaşımları](storage-files-introduction.md)olduğunu görebilirsiniz.

Azure blobs nesne depolama aksine, Bir Azure dosya paylaşımı yerel dosya meta verileri saklayabilir. Azure dosya paylaşımları dosya ve klasör hiyerarşisini, özniteliklerini ve izinleri de korur. NTFS izinleri şirket içinde oldukları için dosya ve klasörlerde depolanabilir.

Şirket içi etki alanı denetleyicisi olan Active Directory kullanıcısı, azure dosya paylaşımına yerel olarak erişebilir. Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) kullanıcısı da olabilir. Her biri, paylaşım izinlerine ve dosya ve klasör ALA'larına dayalı olarak erişim elde etmek için geçerli kimliklerini kullanır. Bu davranış, şirket içi dosya paylaşımına bağlanan bir kullanıcıya benzer.

Alternatif veri akışı, şu anda bir Dosya'da Azure dosya paylaşımında depolanamamakta olan dosya doğruluğunun birincil yönüdür. Azure Dosya Eşitlemi kullanıldığında şirket içinde korunur.

Azure dosya paylaşımları için [Azure AD kimlik doğrulaması](storage-files-identity-auth-active-directory-enable.md) ve Azure AD [DS kimlik doğrulaması](storage-files-identity-auth-active-directory-domain-service-enable.md) hakkında daha fazla bilgi edinin.

## <a name="migration-guides"></a>Geçiş kılavuzları

Aşağıdaki tabloda ayrıntılı geçiş kılavuzları listelendir.

Tablo nasıl kullanılır:

1. Dosyalarınızın şu anda depolanan kaynak sistem için satırı bulun.

1. Bu hedeflerden birini seçin:

   - Azure dosya paylaşımlarının içeriğini şirket içinde önbelleğe almak için Azure Dosya Eşitlemeyi'ni kullanarak karma dağıtım
   - Azure dosyası bulutta paylaşımları

   Seçtiğiniz hedef sütunu seçin.

1. Kaynak ve hedefkesiştiği noktada, bir tablo hücresi kullanılabilir geçiş senaryolarını listeler. Ayrıntılı geçiş kılavuzuna doğrudan bağlantı vermek için birini seçin.

Bağlantısı olmayan bir senaryoda henüz yayımlanmış bir geçiş kılavuzu yok. Güncelleştirmeler için ara sıra bu tabloyu kontrol edin. Yeni kılavuzlar kullanılabilir olduklarında yayınlanacaktır.

| Kaynak | Hedef: </br>Karma dağıtım | Hedef: </br>Yalnızca bulut dağıtımı |
|:---|:--|:--|
| | Araç kombinasyonu:| Araç kombinasyonu: |
| Windows Server 2012 R2 ve sonrası | <ul><li>[Azure Dosya Eşitleme](storage-sync-files-deployment-guide.md)</li><li>[Azure Dosya Eşitleme ve Azure Veri Kutusu](storage-sync-offline-data-transfer.md)</li><li>Azure Dosya Eşitleme ve Depolama Geçiş Hizmeti</li></ul> | <ul><li>Azure Dosya Eşitleme</li><li>Azure Dosya Eşitleme ve Veri Kutusu</li><li>Azure Dosya Eşitleme ve Depolama Geçiş Hizmeti</li><li>Robocopy</li></ul> |
| Windows Server 2012 ve önceki | <ul><li>Azure Dosya Eşitleme ve Veri Kutusu</li><li>Azure Dosya Eşitleme ve Depolama Geçiş Hizmeti</li></ul> | <ul><li>Azure Dosya Eşitleme ve Depolama Geçiş Hizmeti</li><li>Robocopy</li></ul> |
| Ağa bağlı depolama (NAS) | <ul><li>[Azure Dosya Eşitleme ve RoboCopy](storage-files-migration-nas-hybrid.md)</li></ul> | <ul><li>Robocopy</li></ul> |
| Linux veya Samba | <ul><li>[Azure Dosya Eşitleme ve RoboCopy](storage-files-migration-linux-hybrid.md)</li></ul> | <ul><li>Robocopy</li></ul> |
| Microsoft Azure StorBasit Bulut Cihazı 8100 veya StorBasit Bulut Cihazı 8600 | <ul><li>[Azure Dosya Eşitleme ve StorBasit Bulut Cihazı 8020](storage-files-migration-storsimple-8000.md)</li></ul> | |
| StorSimple Bulut Cihazı 1200 | <ul><li>[Azure Dosya Eşitleme](storage-files-migration-storsimple-1200.md)</li></ul> | |
| | | |

## <a name="migration-toolbox"></a>Geçiş araç kutusu

### <a name="file-copy-tools"></a>Dosya kopyalama araçları

Microsoft ve diğer kişiler tarafından kullanılabilen birkaç dosya kopyalama aracı vardır. Geçiş senaryonuz için doğru aracı seçmek için şu temel soruları göz önünde bulundurmanız gerekir:

* Araç, dosya kopyanızın kaynak ve hedef konumlarını destekliyor mu?

* Araç, kaynak ve hedef depolama yerleri arasında ağ yolunuzu veya kullanılabilir protokollerinizi (REST, SMB veya NFS gibi) destekliyor mu?

* Araç, kaynak ve hedef konumlarınız tarafından desteklenen gerekli dosya doğrulatuğu korunuyor mu?

    Bazı durumlarda, hedef depolama alanınız kaynağınızla aynı sadakati desteklemez. Hedef depolama gereksinimleriniz için yeterliyse, araç yalnızca hedefin dosya doğruluğu özellikleriyle eşleşmelidir.

* Aracın geçiş stratejinize uymasını sağlayan özellikler var mı?

    Örneğin, aracın kapalı kalma sürenizi en aza indirmenize izin verip vermeyi göz önünde bulundurun.
    
    Bir araç bir hedefi yansıtma seçeneğini desteklediğinde, kaynak erişilebilir kalırken genellikle aynı kaynak ve hedef üzerinde birden çok kez çalıştırabilirsiniz.

    Aracı ilk çalıştırdığınızda, verilerin büyük bir kısmını kopyalar. Bu ilk çalıştırma bir süre sürebilir. Genellikle iş süreçleriniz için veri kaynağını çevrimdışı na almak istediğinizden daha uzun sürer.

    Bir kaynağı bir hedefe yansıtarak **(robocopy /MIR'de**olduğu gibi), aracı aynı kaynak ve hedefüzerinde tekrar çalıştırabilirsiniz. Önceki çalıştırmadan sonra oluşan yalnızca kaynak değişikliklerini taşıması gerektiğinden, çalıştırma çok daha hızlıdır. Bir kopyalama aracını bu şekilde yeniden çalıştırmak, kapalı kalma süresini önemli ölçüde azaltabilir.

Aşağıdaki tablo, Microsoft araçlarını ve Azure dosya paylaşımlarına uygunluğunu sınıflandırıştır:

| Önerilen | Araç | Azure dosya paylaşımları için destek | Dosya doğruluğunun korunması |
| :-: | :-- | :---- | :---- |
|![Evet, tavsiye edilen](media/storage-files-migration-overview/circle-green-checkmark.png)| Robocopy | Destekleniyor. Azure dosya paylaşımları ağ sürücüleri olarak monte edilebilir. | Tam sadakat.* |
|![Evet, tavsiye edilen](media/storage-files-migration-overview/circle-green-checkmark.png)| Azure Dosya Eşitleme | Azure dosya paylaşımlarına yerel olarak entegre edilmiştir. | Tam sadakat.* |
|![Evet, tavsiye edilen](media/storage-files-migration-overview/circle-green-checkmark.png)| Depolama Geçiş Hizmeti | Dolaylı olarak desteklenir. Azure dosya paylaşımları, SMS hedef sunucularında ağ sürücüleri olarak monte edilebilir. | Tam sadakat.* |
|![Tam olarak tavsiye edilmez](media/storage-files-migration-overview/triangle-yellow-exclamation.png)| Data Box | Destekleniyor. | Meta verileri kopyalamaz. [Veri Kutusu Azure Dosya Eşitlemi ile kullanılabilir.](storage-sync-offline-data-transfer.md) |
|![Önerilmez](media/storage-files-migration-overview/circle-red-x.png)| AzCopy | Destekleniyor. | Meta verileri kopyalamaz. |
|![Önerilmez](media/storage-files-migration-overview/circle-red-x.png)| Azure Depolama Gezgini | Destekleniyor. | Meta verileri kopyalamaz. |
|![Önerilmez](media/storage-files-migration-overview/circle-red-x.png)| Azure Data Factory | Destekleniyor. | Meta verileri kopyalamaz. |
|||||

*\*Tam doğruluk: Azure dosya paylaşımı özelliklerini karşılar veya aşar.*

### <a name="migration-helper-tools"></a>Geçiş yardımcı araçları

Bu bölümde, geçişleri planlamanıza ve çalıştırmanıza yardımcı olan araçlar açıklanmaktadır.

#### <a name="robocopy-from-microsoft-corporation"></a>Microsoft Corporation'dan RoboCopy

RoboCopy dosya geçişleri için en uygun araçlardan biridir. Windows'un bir parçası olarak gelir. Ana [RoboCopy belgeleri,](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) bu aracın birçok seçeneği için yararlı bir kaynaktır.

#### <a name="treesize-from-jam-software-gmbh"></a>JAM Software GmbH'den TreeSize

Azure Dosya Eşitleme, toplam depolama miktarıyla değil, öncelikle öğe (dosya ve klasör) sayısıyla ölçeklendirilir. TreeSize aracı, Windows Server birimlerinizdeki öğe sayısını belirlemenize olanak tanır.

Aracı, [Azure Dosya Eşitleme dağıtımından](storage-sync-files-deployment-guide.md)önce bir perspektif oluşturmak için kullanabilirsiniz. Dağıtımdan sonra bulut katmanlama devreye girdiğinde de kullanabilirsiniz. Bu senaryoda, öğe sayısını ve sunucu önbelleğinizi en çok hangi dizinlerin kullandığını görürsünüz.

Aracın test edilmiş sürümü sürüm 4.4.1'dir. Bulut katmanlı dosyalarla uyumludur. Araç, normal çalışması sırasında katmanlı dosyaların geri çağrılması na neden olmaz.

## <a name="next-steps"></a>Sonraki adımlar

1. Azure dosya paylaşımlarının (yalnızca bulut veya karma) dağıtılmasını istediğiniz bir plan oluşturun.
1. Azure dosya paylaşımlarının kaynağınızla ve dağıtımınızla eşleşen ayrıntılı kılavuzu bulmak için kullanılabilir geçiş kılavuzları listesini gözden geçirin.

Bu makalede bahsedilen Azure Dosyaları teknolojileri hakkında daha fazla bilgi aşağıda verebilmektedir:

* [Azure dosya paylaşımına genel bakış](storage-files-introduction.md)
* [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
* [Azure Dosya Eşitleme: Bulut katmanlama](storage-sync-cloud-tiering.md)
