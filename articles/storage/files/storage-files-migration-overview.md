---
title: Azure dosya paylaşımlarına geçiş
description: Azure dosya paylaşımlarına geçişler hakkında bilgi edinin ve geçiş kılavuzunuzu bulun.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 3/18/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 903ce52120fce7c23c6a3754498b81fc6fc2430f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247325"
---
# <a name="migrate-to-azure-file-shares"></a>Azure dosya paylaşımlarına geçiş

Bu makalede, Azure dosya paylaşımlarına geçişin temel yönleri ele allanmıştır.

Geçiş temellerinin yanı sıra, bu makalede varolan, kişiselleştirilmiş geçiş kılavuzlarının bir listesi de yer almaktadır. Bu geçiş kılavuzları dosyalarınızı Azure dosya paylaşımlarına taşımanıza yardımcı olur ve verilerinizin bugün nerede bulunduğuna ve hangi dağıtım modeline (yalnızca bulut veya karma) taşınmayı planladığınıza göre düzenlenir.

## <a name="migration-basics"></a>Geçiş temelleri

Azure'da birden çok farklı bulut depolama türü vardır. Dosya ların Azure'a geçişinin temel bir yönü, verileriniz için hangi Azure depolama seçeneğinin doğru olduğunu belirlemektir.

Azure dosya paylaşımları genel amaçlı dosya verileri için harikadır. Gerçekten bir şirket içi SMB veya NFS payı için kullandığınız bir şey. [Azure Dosya Eşitleme](storage-sync-files-planning.md)ile, birkaç Azure dosya paylaşımının içeriğini şirket içinde birden fazla Windows Sunucusunda isteğe bağlı olarak önbelleğe alabilirsiniz.

Şu anda şirket içi bir sunucuda çalışan bir uygulamanız varsa, uygulamaya bağlı olarak dosyaları Azure dosya paylaşımlarında depolamak sizin için doğru olabilir. Uygulamayı Azure'da çalıştırmak için kaldırabilir ve Azure dosya paylaşımlarını paylaşılan depolama alanı olarak kullanabilirsiniz. Bu senaryo için [Azure Diskleri'ni](../../virtual-machines/windows/managed-disks-overview.md) de düşünebilirsiniz. Verilerine veya paylaşılan erişime SMB veya makine yerel erişimine bağlı olmayan bulut kaynaklı uygulamalar için Azure [lekeleri](../blobs/storage-blobs-overview.md)gibi nesne depolama genellikle en iyi seçimdir.

Herhangi bir geçişin anahtarı, dosyalarınızı geçerli depolama konumlarından Azure'a geçirirken geçerli tüm dosya doğrularını yakalamaktır. Doğru Azure depolama alanını seçmede bir yardım, Azure depolama seçeneği tarafından desteklenen ve senaryonuz tarafından ne kadar sadakat gerektirdiği de yönüdür. Genel amaçlı dosya verileri geleneksel olarak dosya meta verilerine bağlıdır. Uygulama verileri olmayabilir. Bir dosyanın iki temel bileşeni vardır:

- **Veri akışı**: Bir dosyanın veri akışı dosya içeriğini depolar.
- **Dosya meta verileri**: Dosya meta verilerinin birkaç alt bileşeni vardır:
   * Dosya öznitelikleri: Örneğin salt okunur.
   * Dosya izinleri: *NTFS izinleri* veya *dosya ve klasör ALA'ları*olarak anılacaktır.
   * Zaman damgaları: En önemlisi *oluşturma ve* *son değiştirilmiş* zaman damgaları.
   * Alternatif veri akışı: Daha büyük miktarlarda standart dışı özellikleri depolamak için alan.

Bu nedenle, bir geçişte dosya doğruluğu, geçerli tüm dosya bilgilerini kaynakta depolama, bunları geçiş aracıyla aktarma yeteneği ve bunları geçişin hedef depolama alanında depolama yeteneği olarak tanımlanabilir.

Geçişinizin mümkün olduğunca sorunsuz bir şekilde iletilmesini sağlamak [için, gereksinimleriniz için en iyi kopyalama aracını belirleyin](#migration-toolbox) ve bir depolama hedefini kaynağınızla eşleştirin.

Önceki bilgileri göz önünde bulundurarak, Azure'daki genel amaçlı dosyalar için hedef depolama alanının ne olduğu anlaşılır: [Azure dosya paylaşımları.](storage-files-introduction.md) Azure blobs nesne depolama ile karşılaştırıldığında, dosya meta verileri bir Azure dosya paylaşımında dosyalarda yerel olarak depolanabilir.

Azure dosya paylaşımları da dosya ve klasör hiyerarşisini korur. Ek olarak:
* NTFS izinleri, şirket içinde olduğu gibi dosya ve klasörlerde depolanabilir.
* AD kullanıcıları (veya Azure AD DS kullanıcıları) bir Azure dosya paylaşımına yerel olarak erişebilir. 
    Geçerli kimliklerini kullanırlar ve paylaşım izinlerinin yanı sıra dosya ve klasör Aç'larına göre erişim elde ederler. Kullanıcıların şirket içi dosya paylaşımına bağlanmasından farklı olmayan bir davranış.
*  Alternatif veri akışı, şu anda bir Dosya da bir Azure dosya paylaşımında depolanamayan dosya doğruluğunun birincil yönüdür.
   Azure Dosya Eşitlemi söz konusu olduğunda şirket içinde korunur.

* [Azure dosya paylaşımları için AD kimlik doğrulaması hakkında daha fazla bilgi edinin](storage-files-identity-auth-active-directory-enable.md)
* [Azure dosya paylaşımları için Azure Active Directory Domain Services (AAD DS) kimlik doğrulaması hakkında daha fazla bilgi edinin](storage-files-identity-auth-active-directory-domain-service-enable.md)

## <a name="migration-guides"></a>Geçiş kılavuzları

Aşağıdaki tabloda ayrıntılı geçiş kılavuzları listelendir.

Şu şekilde gezinin:
1. Dosyalarınızın şu anda depolanan kaynak sistem için satırı bulun.
2. Bir veya daha fazla Azure dosya paylaşımının içeriğini şirket içinde önbelleğe almak için Azure Dosya Eşitlemeyi'ni kullandığınız karma bir dağıtımı mı hedeflediğinizi veya Azure dosya paylaşımlarını doğrudan bulutta kullanmak istiyorsanız karar verin. Kararınızı yansıtan hedef sütunu seçin.
3. Kaynak ve hedefkesiştiği noktada, bir tablo hücresi kullanılabilir geçiş senaryolarını listeler. Ayrıntılı geçiş kılavuzuna doğrudan bağlantı vermek için bunlardan birini seçin.

Bağlantısı olmayan bir senaryoda henüz yayımlanmış bir geçiş kılavuzu yok. Güncelleştirmeler için ara sıra bu tabloyu kontrol edin. Yeni kılavuzlar kullanılabilir olduğunda yayınlanacaktır.

| Kaynak | Hedef: </br>Karma dağıtım | Hedef: </br>Yalnızca bulut dağıtımı |
|:---|:--|:--|
| | Araç kombinasyonu:| Araç kombinasyonu: |
| Windows Server 2012 R2 ve daha yeni | <ul><li>[Azure Dosya Eşitleme](storage-sync-files-deployment-guide.md)</li><li>[Azure Dosya Eşitle + DataBox](storage-sync-offline-data-transfer.md)</li><li>Depolama Geçiş Hizmeti + Azure Dosya Eşitleme</li></ul> | <ul><li>Azure Dosya Eşitleme</li><li>Azure Dosya Eşitle + DataBox</li><li>Depolama Geçiş Hizmeti + Azure Dosya Eşitleme</li><li>Robocopy</li></ul> |
| Windows Server 2012 ve üzeri | <ul><li>Azure Dosya Eşitle + DataBox</li><li>Depolama Geçiş Hizmeti + Azure Dosya Eşitleme</li></ul> | <ul><li>Depolama Geçiş Hizmeti + Azure Dosya Eşitleme</li><li>Robocopy</li></ul> |
| Ağ Ekli Depolama (NAS) | <ul><li>[Azure Dosya Eşitle + Robocopy](storage-files-migration-nas-hybrid.md)</li></ul> | <ul><li>Robocopy</li></ul> |
| Linux / Samba | <ul><li>[Robocopy + Azure Dosya Eşitleme](storage-files-migration-linux-hybrid.md)</li></ul> | <ul><li>Robocopy</li></ul> |
| StorBasit 8100 / 8600 | <ul><li>[Azure Dosya Senkronizasyonu + 8020 Sanal Cihaz](storage-files-migration-storsimple-8000.md)</li></ul> | |
| StorBasit 1200 | <ul><li>[Azure Dosya Eşitleme](storage-files-migration-storsimple-1200.md)</li></ul> | |
| | | |

## <a name="migration-toolbox"></a>Geçiş araç kutusu

### <a name="file-copy-tools"></a>Dosya kopyalama araçları

Birkaç Microsoft ve Microsoft olmayan dosya kopyalama araçları kullanılabilir. Geçiş senaryonuz için doğru dosya kopyalama aracını seçmek için üç temel soruyu göz önünde bulundurmanız gerekir:

* Kopyalama aracı, belirli bir dosya kopyası için kaynağı ve hedef konumu destekliyor mu? 
    * Kaynak ve hedef depolama konumlarına ağ yolunuzu ve/veya kullanılabilir protokollerinizi (örneğin REST/SMB/NFS) destekliyor mu?
* Kopyalama aracı kaynak/hedef konumu tarafından desteklenen gerekli dosya doğrusu korur mu? Bazı durumlarda, hedef depolama kaynağınızla aynı sadakati desteklemez. Hedef depolamanın gereksinimleriniz için yeterli olduğuna zaten karar verdiniz, bu nedenle kopyalama aracının yalnızca hedeflerin dosya doğruluğu yetenekleriyle eşleşmesi gerekir.
* Kopyalama aracı, geçiş stratejime uygun hale getiren özelliklere sahip mi? 
    * Örneğin, kapalı kalma sürenizi en aza indirmenize olanak tanıyan seçenekler olup olmadığını göz önünde bulundurun. Sormak için iyi bir soru: Ben aktif olarak erişilen konum tarafından, aynı aynı birden çok kez bu kopyayı çalıştırabilirsiniz? Bu nedenle, kapalı kalma süresini önemli ölçüde azaltabilirsiniz. Bunu, tam bir kopyayı garanti etmek için yalnızca kaynağın değiştirmeyi durdurması durumunda başlatabileceğiniz bir durumla karşılaştırın.

Aşağıdaki tablo, Microsoft araçlarını ve Azure dosya paylaşımlarına uygunluğunu sınıflandırıştır:

| Önerilen | Araç | Azure dosya paylaşımlarını destekler | Dosya doğrulığını korur |
| :-: | :-- | :---- | :---- |
|![Evet, tavsiye edilir.](media/storage-files-migration-overview/circle-green-checkmark.png)| Robocopy | Destekleniyor. Azure dosya paylaşımları ağ sürücüleri olarak monte edilebilir. | Tam sadakat* |
|![Evet, tavsiye edilir.](media/storage-files-migration-overview/circle-green-checkmark.png)| Azure Dosya Eşitleme | Azure dosya paylaşımlarına yerel olarak entegre edilmiştir. | Tam sadakat* |
|![Evet, tavsiye edilir.](media/storage-files-migration-overview/circle-green-checkmark.png)| Depolama Geçiş Hizmeti (SMS) | Dolaylı olarak desteklenir. Azure dosya paylaşımları, SMS hedef sunucusundaki ağ sürücüleri olarak monte edilebilir. | Tam sadakat* |
|![Tam olarak tavsiye edilmez.](media/storage-files-migration-overview/triangle-yellow-exclamation.png)| Azure Veri Kutusu | Destekleniyor. | Meta verileri kopyalamaz. [Azure Dosya Eşitlemi ile birlikte kullanılabilir.](storage-sync-offline-data-transfer.md) |
|![Önerilmez.](media/storage-files-migration-overview/circle-red-x.png)| AzCopy | Destekleniyor. | Meta verileri kopyalamaz. |
|![Önerilmez.](media/storage-files-migration-overview/circle-red-x.png)| Azure Storage Gezgini | Destekleniyor. | Meta verileri kopyalamaz. |
|![Önerilmez.](media/storage-files-migration-overview/circle-red-x.png)| Azure Data Factory | Destekleniyor. | Meta verileri kopyalamaz. |
|||||

*\*Tam doğruluk: Azure dosya paylaşımı özelliklerini karşılar veya aşar.*

### <a name="migration-helper-tools"></a>Geçiş yardımcı araçları

Bu bölümde, geçişlerin planlanıp yürütülmesine yardımcı olan araçlar listeleneb.)

* **RoboCopy, Microsoft Corporation'dan**

    Geçişleri dosyalamak için en uygun kopyalama araçlarından biri, Microsoft Windows'un bir parçası olarak gelir. Bu araçtaki birçok seçenek nedeniyle, ana [RoboCopy belgeleri](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) yararlı bir kaynaktır.

* **TreeSize, JAM Software GmbH'den**

    Azure Dosya Eşitleme, öncelikle öğe sayısı (dosya ve klasörler) ve toplam TiB tutarıyla daha az ölçeklendirilir. Araç, Windows Server birimlerinizdeki dosya ve klasör sayısını belirlemek için kullanılabilir. Ayrıca, [Azure Dosya Eşitleme dağıtımından](storage-sync-files-deployment-guide.md) önce bir perspektif oluşturmak için de kullanılabilir - ancak bulut katmanlama devreye girdiğinde ve yalnızca öğe sayısını değil, sunucu önbelleğinizin en çok hangi dizinler kullanıldığını da görmek istersiniz.
    Bu araç (test edilmiş sürüm 4.4.1) bulut katmanlı dosyalarla uyumludur. Normal çalışması sırasında katmanlı dosyaların geri çağrılması neden olmaz.


## <a name="next-steps"></a>Sonraki adımlar

1. Çabaladığınız Azure dosya paylaşımlarının (yalnızca bulut veya karma) dağıtımı için bir plan oluşturun.
2. Azure dosya paylaşımlarının kaynağınızla ve dağıtımınızla eşleşen ayrıntılı kılavuzu bulmak için kullanılabilir geçiş kılavuzları listesini gözden geçirin.

Bu makalede bahsedilen Azure Dosyaları teknolojileri hakkında daha fazla bilgi vardır:

* [Azure dosya paylaşımına genel bakış](storage-files-introduction.md)
* [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
* [Azure Dosya Eşitleme: Bulut katmanlama](storage-sync-cloud-tiering.md)
