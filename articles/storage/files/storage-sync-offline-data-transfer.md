---
title: Azure Veri Kutusu ile verileri Azure Dosya Eşitleme'sine geçirin
description: Toplu verileri Azure Dosya Eşitlemi ile uyumlu bir şekilde geçirin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: d0331419de89775062f1309c5d854cd7325c68e4
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656758"
---
# <a name="migrate-bulk-data-to-azure-file-sync-with-azure-databox"></a>Azure DataBox ile verileri Azure Dosya Eşitleme’ye toplu olarak geçirme
Toplu verileri Azure Dosya Eşitleme'sine iki şekilde geçirebilirsiniz:

* **Azure Dosya Eşitlemeyi'ni kullanarak dosyalarınızı yükleyin.** Bu en basit yöntemdir. Dosyalarınızı yerel olarak Windows Server 2012 R2 veya sonraki lerine taşıyın ve Azure Dosya Eşitleme aracısını yükleyin. Eşitlemeyi ayarladıktan sonra dosyalarınız sunucudan yüklenir. (Müşterilerimiz şu anda her iki günde bir ortalama 1 TiB yükleme hızı yaşarlar.) Sunucunuzun veri merkeziniz için bant genişliğinin çok fazla kullanılmadığından emin olmak için bir [bant genişliği azaltma zamanlaması](storage-sync-files-server-registration.md#ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter)ayarlamak isteyebilirsiniz.
* **Dosyalarınızı çevrimdışınakil edin.** Yeterli bant genişliğiniz yoksa, makul bir süre içinde DosyalarıNızı Azure'a yükleyemeyebilirsiniz. Sorun, tüm dosya kümesinin ilk eşitlemidir. Bu zorluğuaşmak için [Azure Veri Kutusu ailesi](https://azure.microsoft.com/services/storage/databox)gibi çevrimdışı toplu geçiş araçlarını kullanın. 

Bu makalede, Azure Dosya Eşitlemi ile uyumlu bir şekilde dosyaların çevrimdışı nasıl geçirilir açıklanmaktadır. Dosya çakışmalarını önlemek ve eşitlemeyi etkinleştirdikten sonra dosya ve klasör erişim denetim listelerinizi (ALA'lar) ve zaman damgalarını korumak için bu yönergeleri izleyin.

## <a name="migration-tools"></a>Geçiş araçları
Bu makalede açıkladığımız işlem yalnızca Veri Kutusu için değil, diğer çevrimdışı geçiş araçları için de çalışır. Ayrıca AzCopy, Robocopy veya iş ortağı araçları ve internet üzerinden düz çalışma hizmetleri gibi araçlar için çalışır. Ancak ilk yükleme zorluğunun üstesinden gelmek için, bu araçları Azure Dosya Eşitlemi ile uyumlu bir şekilde kullanmak için bu makaledeki adımları izleyin.

Bazı durumlarda, Azure Dosya Eşitlemeyi'ni benimsemeden önce bir Windows Server'dan başka bir Windows Server'a geçmeniz gerekir. [Depolama Geçiş Hizmeti](https://aka.ms/storagemigrationservice) (SMS) bu konusunda yardımcı olabilir. İster Azure Dosya Eşitlemesi (Windows Server 2012R2 ve üzeri) tarafından desteklenen bir Server OS sürümüne geçiş yapmanız, ister Azure Dosya Eşitlemesi için yeni bir sistem satın aldığınız için geçiş yapmanız gereksin, SMS'in geçişinizi sorunsuz bir şekilde yapmanıza yardımcı olacak çok sayıda özellik ve avantajvardır.

## <a name="benefits-of-using-a-tool-to-transfer-data-offline"></a>Verileri çevrimdışı aktarmak için bir araç kullanmanın yararları
Çevrimdışı geçiş için Veri Kutusu gibi bir aktarım aracı kullanmanın başlıca avantajları şunlardır:

- Tüm dosyalarınızı ağa yüklemeniz gerekmez. Büyük ad alanları için bu araç önemli ağ bant genişliği ve zaman kaydedebilirsiniz.
- Hangi aktarım aracını kullanırsanız kullanın (Veri Kutusu, Azure İçe Alma/Dışa Aktarma hizmeti vb.) kullandığınızda, canlı sunucunuz yalnızca verileri Azure'a taşıdıktan sonra değişen dosyaları yükler.
- Azure Dosya Eşitlemesi, çevrimdışı toplu geçiş aracı ALA'lar taşımıyor olsa bile dosyanızı ve klasör Aç'larınızı eşitler.
- Veri Kutusu ve Azure Dosya Eşitleme hiçbir kapalı kalma süresi gerektirir. Verileri Azure'a aktarmak için Veri Kutusu'nu kullandığınızda, ağ bant genişliğini verimli bir şekilde kullanır ve dosya doğrulmasını korursunuz. Ayrıca, verileri Azure'a taşıdıktan sonra değişen dosyaları yükleyerek ad alanınızı güncel tutarsınız.

## <a name="prerequisites-for-the-offline-data-transfer"></a>Çevrimdışı veri aktarımı için ön koşullar
Çevrimdışı veri aktarımınızı tamamlamadan önce geçiş yaptığınız sunucuda eşitlemeyi etkinleştirmemelisiniz. Başlamadan önce göz önünde bulundurulması gereken diğer şeyler şunlardır:

- Toplu geçişiniz için Veri Kutusu'nu kullanmayı planlıyorsanız: [Veri Kutusu için dağıtım ön koşullarını](../../databox/data-box-deploy-ordered.md#prerequisites)gözden geçirin.
- Son Azure Dosya Eşitleme topolojinizi planlayın: [Azure Dosya Eşitleme dağıtımı için planlayın](storage-sync-files-planning.md)
- Eşitlemek istediğiniz dosya paylaşımlarını tutacak Azure depolama hesabı(lar)'ı seçin. Toplu geçişinizin aynı Depolama Hesabı'ndaki (ler) geçici evreleme hisselerine gerçekleştiğinden emin olun. Toplu geçiş yalnızca aynı depolama hesabında bulunan bir son ve bir evreleme paylaşımı kullanılarak etkinleştirilebilir.
- Toplu geçiş yalnızca sunucu konumuyla yeni bir eşitleme ilişkisi oluşturduğunuzda kullanılabilir. Varolan eşitleme ilişkisi olan toplu geçişi etkinleştiremezsiniz.


## <a name="process-for-offline-data-transfer"></a>Çevrimdışı veri aktarımı için işlem
Azure Dosya Eşitlemeyi'ni Azure Veri Kutusu gibi toplu geçiş araçlarıyla uyumlu bir şekilde nasıl ayarlayabilirsiniz:

![Azure Dosya Eşitlemeyi'nin nasıl ayarlandığını gösteren diyagram](media/storage-sync-files-offline-data-transfer/data-box-integration-1-600.png)

| Adım | Ayrıntı |
|---|---------------------------------------------------------------------------------------|
| ![1. Adım](media/storage-sync-files-offline-data-transfer/bullet_1.png) | [Veri Kutunuzu Sipariş Edin.](../../databox/data-box-deploy-ordered.md) Veri Kutusu ailesi ihtiyaçlarınızı karşılamak için [çeşitli ürünler](https://azure.microsoft.com/services/storage/databox/data) sunar. Veri Kutunuzu aldığınızda, verilerinizi Veri Kutusu'ndaki bu UNC yoluna [kopyalamak için belgelerini](../../databox/data-box-deploy-copy-data.md#copy-data-to-data-box) takip edin: * \\<DeviceIPAddres\>\<\>\<StorageAccountName_AzFile ShareName\>*. Burada, *ShareName* evreleme payının adıdır. Veri Kutusunu Azure'a geri gönderin. |
| ![2. Adım](media/storage-sync-files-offline-data-transfer/bullet_2.png) | Dosyalarınız geçici evreleme paylaşımları olarak seçtiğiniz Azure dosyası paylaşımlarında görünene kadar bekleyin. *Bu paylaşımlara eşitleme yi etkinleştirmeyin.* |
| ![3. Adım](media/storage-sync-files-offline-data-transfer/bullet_3.png) | <ul><li>Veri Kutusu'nun sizin için oluşturduğu her dosya paylaşımı için yeni bir boş paylaşım oluşturun. Bu yeni paylaşım, Veri Kutusu paylaşımıyla aynı depolama hesabında olmalıdır. [Yeni bir Azure dosya paylaşımı nasıl oluşturulur?](storage-how-to-create-file-share.md)</li><li>Depolama Eşitleme Hizmeti'nde [eşitleme grubu oluşturun.](storage-sync-files-deployment-guide.md#create-a-sync-group-and-a-cloud-endpoint) Boş payı bulut bitiş noktası olarak başvurun. Her Veri Kutusu dosya paylaşımı için bu adımı yineleyin. [Azure Dosya Eşitlemeyi'ni ayarlama.](storage-sync-files-deployment-guide.md)</li></ul> |
| ![4. Adım](media/storage-sync-files-offline-data-transfer/bullet_4.png) | [Canlı sunucu dizininizi sunucu bitiş noktası olarak ekleyin.](storage-sync-files-deployment-guide.md#create-a-server-endpoint) Bu süreçte, dosyaları Azure'a taşıdığınızı belirtin ve hazırlama paylaşımlarına başvurun. Gerektiğinde bulut katmanlamayı etkinleştirebilir veya devre dışı kullanabilirsiniz. Canlı sunucunuzda bir sunucu bitiş noktası oluştururken, evreleme paylaşımına başvurun. Sunucu **uç noktası** ekle bıçağında, **Çevrimdışı Veri Aktarımı**altında **Etkin'i**seçin ve ardından bulut bitiş noktasıyla aynı depolama hesabında olması gereken hazırlama payını seçin. Burada, kullanılabilir paylaşımlar listesi depolama hesabı ve zaten eşitlenmemiş paylaşımlar tarafından filtrelenir. Bu tabloyu izleyen ekran görüntüsü, Azure portalındaki sunucu bitiş noktası oluşturma sırasında DataBox paylaşımına nasıl başvurulsüreceğini gösterir. |
| ![5. Adım](media/storage-sync-files-offline-data-transfer/bullet_5.png) | Önceki adıma sunucu bitiş noktasını ekledikten sonra, veriler otomatik olarak doğru kaynaktan akmaya başlar. Paylaşım bölümü [eşitleme,](#syncing-the-share) veri akışı DataBox paylaşımından veya Windows Server'dan ne zaman akar |
| |

![Yeni bir sunucu bitiş noktası oluştururken çevrimdışı veri aktarımının nasıl etkinleştirilen gösteren Azure portalı kullanıcı arabiriminin ekran görüntüsü](media/storage-sync-files-offline-data-transfer/data-box-integration-2-600.png)

## <a name="syncing-the-share"></a>Paylaşımı eşitleme
Sunucu bitiş noktanızı oluşturduktan sonra eşitleme başlar. Eşitleme işlemi, sunucudaki her dosyanın Veri Kutusu'nun dosyaları yatırdığı hazırlama paylaşımında da bulunup bulunmadığını belirler. Dosya orada varsa, eşitleme işlemi dosyayı sunucudan yüklemek yerine hazırlama paylaşımından kopyalar. Dosya hazırlama paylaşımında yoksa veya yerel sunucuda daha yeni bir sürüm varsa, eşitleme işlemi dosyayı yerel sunucudan yükler.

Paylaşımı eşitlerken, eşitleme, yerel sunucudaki dosya türevlerindeki eksik dosya özniteliklerini, izinleri veya zaman damgalarını birleştirerek Bunları DataBox paylaşımındaki dosya benzerleriyle birleştirir. Bu, her dosya ve klasörün Azure dosya paylaşımında olası tüm dosya doğruluğuyla birlikte varmasını sağlar.

> [!IMPORTANT]
> Toplu geçiş modunu yalnızca sunucu bitiş noktası oluştururken etkinleştirebilirsiniz. Bir sunucu bitiş noktası oluşturduktan sonra, zaten eşitlenmiş bir sunucudan toplu olarak geçirilen verileri ad alanına entegre edemezsiniz.

## <a name="acls-and-timestamps-on-files-and-folders"></a>Dosya ve klasörlerdeki ALA'lar ve zaman damgaları
Azure Dosya Eşitlemesi, kullandığınız toplu geçiş aracı başlangıçta ALA'ları taşımamış olsa bile, dosya ve klasör ALA'larının canlı sunucudan eşitlenmesini sağlar. Bu nedenle, evreleme paylaşımı nın dosya ve klasörler için aLAK içermesine gerek yoktur. Yeni bir sunucu bitiş noktası oluştururken çevrimdışı veri aktarma özelliğini etkinleştirdiğinizde, tüm dosya AçL'ları sunucuda eşitlenir. Yeni oluşturulan ve değiştirilen zaman damgaları da eşitlenir.

## <a name="shape-of-the-namespace"></a>Ad alanının şekli
Eşitlemeyi etkinleştirdiğinizde, sunucunun içeriği ad alanının şeklini belirler. Veri Kutusu anlık görüntüsü ve geçiş bittikten sonra dosyalar yerel sunucudan silinirse, bu dosyalar canlı ya da eşitleme ad alanına geçmez. Sahneleme de kalıyorlar ama kopyalanmıyorlar. Eşitleme, ad alanını canlı sunucuya göre tuttuğundan bu gereklidir. Veri Kutusu *anlık görüntüsü,* verimli dosya kopyalama için yalnızca bir hazırlama alanıdır. Bu canlı isim alanının şekli için bir otorite değil.

## <a name="cleaning-up-after-bulk-migration"></a>Toplu geçişten sonra temizleme 
Sunucu ad alanının ilk eşitlemini tamamlanın, Veri Kutusu toplu olarak geçirilen dosyalar hazırlama dosya paylaşımını kullanır. Azure portalındaki **Server Endpoint Properties** belinde, **Çevrimdışı Veri Aktarımı** bölümünde, durum **Devam Ediyor'dan** **Tamamlanana**değişir. 

![Çevrimdışı veri aktarımı için durum ve devre dışı bırakdenetimlerinin bulunduğu Server Endpoint Properties blade'in ekran görüntüsü](media/storage-sync-files-offline-data-transfer/data-box-integration-3-444.png)

Artık maliyetlerden tasarruf etmek için evreleme payını temizleyebilirsiniz:

1. Sunucu **Bitiş Noktası Özellikleri** bısatırında, durum **tamamlandığında**çevrimdışı **veri aktarımını devre dışı bırak'ı**seçin.
2. Maliyetleri kaydetmek için evreleme payını silmeyi düşünün. Evreleme paylaşımı büyük olasılıkla dosya ve klasör ALA'ları içermez, bu nedenle yararlı olması olası değildir. Yedekleme noktası zamanında amaçlar için, [eşitleme Azure dosya paylaşımının](storage-snapshots-files.md)gerçek bir anlık görüntüsünü oluşturun. Azure [Yedekleme'yi]( ../../backup/backup-afs.md) bir zamanlamada anlık görüntü alacak şekilde ayarlayabilirsiniz.

Çevrimdışı veri aktarım modunu yalnızca durum **tamamlandığında** veya bir yanlış yapılandırma nedeniyle iptal etmek istediğinizde devre dışı bırak. Dağıtım sırasında modu devre dışı ederseniz, evreleme paylaşımınız hala kullanılabilir olsa bile dosyalar sunucudan yüklenmeye başlar.

> [!IMPORTANT]
> Çevrimdışı veri aktarım modunu devre dışı bırakttıktan sonra, toplu geçişten evreleme payı hala kullanılabilir olsa bile, bunu yeniden etkinleştiremezsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Dosya Eşitleme dağıtımı için plan](storage-sync-files-planning.md)
- [Azure Dosya Eşitleme’yi dağıtma](storage-sync-files-deployment-guide.md)
