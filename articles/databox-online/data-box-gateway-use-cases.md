---
title: Microsoft Azure Data Box Gateway kullanım örnekleri | Microsoft Docs
description: Azure 'a veri aktarmanıza imkan tanıyan bir Sanal Gereç depolama çözümü olan Azure Data Box Gateway için kullanım örneklerini açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/02/2019
ms.author: alkohli
ms.openlocfilehash: e72113313e27949819db567c550401b1f051473f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77022690"
---
# <a name="use-cases-for-azure-data-box-gateway"></a>Azure Data Box Gateway için kullanım örnekleri

Azure Data Box Gateway, şirket içinde bulunan ve görüntünüzü, medyayı ve diğer verilerinizi Azure 'a gönderen bir bulut depolama ağ geçidi aygıtıdır. Bu bulut depolama ağ geçidi, hiper yöneticiniz üzerinde sağlanan bir sanal makinedir. Bu sanal cihaza, daha sonra Azure 'a gönderdiği NFS ve SMB protokollerini kullanarak veri yazarsınız. Bu makalede, bu cihazı dağıtabileceğiniz senaryoların ayrıntılı bir açıklaması sağlanmaktadır.

Aşağıdaki senaryolar için Data Box Gateway kullanın:

- Büyük miktarlarda verileri sürekli alma.
- Güvenli ve etkili bir şekilde veri bulutu arşivi için.
- Data Box kullanılarak ilk toplu aktarım yapıldıktan sonra ağ üzerinden artımlı veri aktarımı için.

Bu senaryoların her biri, sonraki bölümlerde ayrıntılı olarak açıklanmıştır.


## <a name="continuous-data-ingestion"></a>Sürekli veri alımı

Data Box Gateway başlıca avantajlarından biri, veri boyutundan bağımsız olarak buluta kopyalamak için verileri cihaza sürekli olarak alma olanağıdır.

Veri ağ geçidi cihazına yazıldığı için cihaz, verileri Azure depolama 'ya yükler. Bu cihaz, belirli bir eşiğe ulaştığında meta verileri korurken, dosyaları yerel olarak kaldırarak depolamayı otomatik olarak yönetir. Meta verilerin yerel bir kopyasının tutulması, ağ geçidi cihazının yalnızca dosya güncelleştirilirken değişiklikleri karşıya yüklemesine olanak sağlar. Ağ geçidinize yüklenen veriler, [veri yükleme uyarıları](data-box-gateway-limits.md#data-upload-caveats)'ndaki yönergelere göre olmalıdır.

Cihaz verilerle doldurulduğunda, verilerin buluta yüklenme oranıyla eşleşecek şekilde giriş hızını azaltmayı (gerektiğinde) başlatır. Cihazdaki sürekli alımı izlemek için uyarıları kullanırsınız. Bu uyarılar, daraltma başladıktan sonra tetiklenir ve daraltma durdurulduktan sonra temizlenir.

## <a name="cloud-archival-of-data"></a>Veri bulutu Arşivi

Verilerinizi bulutta uzun süreli olarak sürdürmek istediğinizde Data Box Gateway kullanın. Depolamanın **Arşiv** katmanını uzun süreli saklama için kullanabilirsiniz.

Arşiv katmanı, nadiren erişilen verileri en az 180 gün boyunca depolamak için iyileştirilmiştir. **Arşiv** katmanı en düşük depolama maliyetlerini sunar, ancak en yüksek erişim maliyetlerine sahiptir. Daha fazla bilgi için [Arşiv erişim katmanı](/azure/storage/blobs/storage-blob-storage-tiers#archive-access-tier)' na gidin.

### <a name="move-data-to-archive-tier"></a>Verileri arşiv katmanına taşıma

Başlamadan önce, çalışan bir Data Box Gateway cihazınıza sahip olduğunuzdan emin olun. Eğitim bölümünde açıklanan adımları izleyin. [Azure Data Box Gateway dağıtmaya hazırlanın](data-box-gateway-deploy-prep.md) ve bir işlem cihazına sahip olana kadar sonraki öğreticiye ilerletmeden devam edin.

- Verileri [Data Box Gateway aracılığıyla aktarma](data-box-gateway-deploy-add-shares.md)bölümünde açıklandığı gibi olağan aktarım yordamı aracılığıyla Azure 'a veri yüklemek için Data Box Gateway cihazını kullanın.
- Veriler karşıya yüklendikten sonra arşiv katmanına taşımanız gerekir. Blob katmanını iki şekilde ayarlayabilirsiniz: Azure PowerShell betiği veya Azure Storage yaşam döngüsü yönetim ilkesi.  
    - Azure PowerShell kullanıyorsanız, verileri arşiv katmanına taşımak için aşağıdaki [adımları](/azure/databox/data-box-how-to-set-data-tier#use-azure-powershell-to-set-the-blob-tier) izleyin.
    - Azure yaşam döngüsü yönetimi kullanıyorsanız, verileri arşiv katmanına taşımak için aşağıdaki adımları izleyin.
        - Arşiv katmanını kullanmak için blob yaşam döngüsü yönetim hizmetinin önizlemesine [kaydolun](/azure/storage/common/storage-lifecycle-management-concepts) .
        - Alma [sırasında verileri arşivlemek](/azure/storage/blobs/storage-lifecycle-management-concepts#archive-data-after-ingest)için aşağıdaki ilkeyi kullanın.
- Blob 'lar arşiv olarak işaretlendikten sonra sık veya soğuk katmana taşınmadığı sürece Ağ Geçidi tarafından artık değiştirilemeyebilir. Dosya yerel depodadır, yerel kopyada yapılan değişiklikler (silme dahil) arşiv katmanına yüklenmeyecektir.
- Arşiv depolamadaki verileri okumak için, blob katmanı sık erişimli veya seyrek erişimli olarak değiştirilerek yeniden doldurulması gerekir. Ağ geçidinde [paylaşımın yenilenmesi](data-box-gateway-manage-shares.md#refresh-shares) blob 'u yeniden göstermez.

Daha fazla bilgi için [Azure Blob depolama yaşam döngüsünü yönetme](/azure/storage/common/storage-lifecycle-management-concepts)hakkında daha fazla bilgi edinin.

## <a name="initial-bulk-transfer-followed-by-incremental-transfer"></a>İlk toplu aktarım ve ardından artımlı aktarım

Büyük miktarda verileri toplu karşıya yüklemeyi ve ardından artımlı aktarımları yapmak istediğinizde Data Box ve Data Box Gateway birlikte kullanın. Bir çevrimdışı modda (ilk çekirdek) ve ağ üzerinden artımlı aktarımlar (devam eden akış) için Data Box Gateway toplu aktarım için Data Box kullanın.

### <a name="seed-the-data-with-data-box"></a>Data Box ile verileri tohum

Data Box verileri kopyalamak ve Azure depolama alanına yüklemek için bu adımları izleyin.

1. [Data Box sıralayın](/azure/databox/data-box-deploy-ordered).
2. [Data Box ayarlayın](/azure/databox/data-box-deploy-set-up).
3. [SMB aracılığıyla Data Box verileri kopyalayın](/azure/databox/data-box-deploy-copy-data).
4. [Data Box döndürün, verilerin Azure 'a yüklenmesini doğrulayın](/azure/databox/data-box-deploy-picked-up).
5. Verileri Azure 'a yükleme işlemi tamamlandıktan sonra tüm veriler Azure depolama kapsayıcıları 'nda olmalıdır. Data Box için depolama hesabında, tüm verilerin kopyalandığından emin olmak için blob (ve dosya) kapsayıcısına gidin. Bu adı daha sonra kullanacağınız için kapsayıcı adı ' nı bir yere getirin. Örneğin, aşağıdaki ekran görüntüsünde, `databox` kapsayıcı artımlı aktarım için kullanılacaktır.

    ![Data Box veri içeren kapsayıcı](media/data-box-gateway-use-cases/data-container1.png)

Bu toplu aktarım, ilk dengeli dağıtım aşamasını tamamlar.

### <a name="ongoing-feed-with-data-box-gateway"></a>Data Box Gateway ile devam eden akış

Data Box Gateway ile ilgili devam eden adımları izleyin.

1. Data Box Gateway üzerinde bir bulut paylaşma oluşturun. Bu paylaşımın tüm verileri Azure depolama hesabına otomatik olarak yükler. Data Box Gateway kaynağınızın **Paylaşımlar** bölümüne gidin ve **+ Paylaşım Ekle**' ye tıklayın.

    ![+ Paylaşma Ekle ' ye tıklayın.](media/data-box-gateway-use-cases/add-share1.png)

2. Bu paylaşımın, sağlanan verileri içeren kapsayıcınıza eşlendiğinden emin olun. **BLOB kapsayıcısı Seç**Için **Varolanı kullan** ' ı seçin ve Data Box verilerin aktarıldığı kapsayıcıya gidin.

    ![Ayarları paylaşma](media/data-box-gateway-use-cases/share-settings-select-existing-container1.png)

3. Paylaşma oluşturulduktan sonra, paylaşma 'yı yenileyin. Bu işlem şirket içi paylaşma işlemini Azure 'daki içerikle yeniler.

    ![Paylaşma Yenile](media/data-box-gateway-use-cases/refresh-share1.png)

    Paylaşma eşitlendiğinde, dosyalar istemcide değiştirilmişse, artımlı değişiklikleri karşıya yükler Data Box Gateway.

## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Gateway sistem gereksinimlerini](data-box-gateway-system-requirements.md) gözden geçirin.
- [Data Box Gateway sınırlarını](data-box-gateway-limits.md) anlayın.
- Azure portalında [Azure Data Box Gateway](data-box-gateway-deploy-prep.md)’i dağıtın.