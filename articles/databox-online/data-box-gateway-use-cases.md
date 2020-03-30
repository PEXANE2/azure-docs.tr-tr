---
title: Microsoft Azure Veri Kutusu Ağ Geçidi kullanım örnekleri | Microsoft Dokümanlar
description: Verileri Azure'a aktarmanızı sağlayan bir sanal cihaz depolama çözümü olan Azure Veri Kutusu Ağ Geçidi'nin kullanım durumlarını açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/02/2019
ms.author: alkohli
ms.openlocfilehash: e72113313e27949819db567c550401b1f051473f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022690"
---
# <a name="use-cases-for-azure-data-box-gateway"></a>Azure Veri Kutusu Ağ Geçidi için kılıfları kullanma

Azure Veri Kutusu Ağ Geçidi, binanızda bulunan ve resminizi, medyanızı ve diğer verilerinizi Azure'a gönderen bir bulut depolama ağ geçidi aygıtıdır. Bu bulut depolama ağ geçidi, hipervizörünüzde bulunan sanal bir makinedir. Verileri bu sanal aygıta NFS ve Kobİ protokollerini kullanarak yazarsınız ve bu cihaz azure'a gönderir. Bu makalede, bu aygıtı dağıtabileceğiniz senaryoların ayrıntılı bir açıklamasını sağlar.

Aşağıdaki senaryolar için Veri Kutusu Ağ Geçidi'ni kullanın:

- Sürekli büyük miktarda veri yutmak için.
- Verilerin güvenli ve verimli bir şekilde bulut arşivlemesine neden olun.
- İlk toplu aktarım Veri Kutusu kullanılarak yapıldıktan sonra ağ üzerinden artımlı veri aktarımiçin.

Bu senaryoların her biri sonraki bölümlerde ayrıntılı olarak açıklanmıştır.


## <a name="continuous-data-ingestion"></a>Sürekli veri alımı

Veri Kutusu Ağ Geçidi'nin en önemli avantajlarından biri, veri boyutundan bağımsız olarak buluta kopyalamak için aygıta sürekli veri yutabilmesidir.

Veriler ağ geçidi aygıtına yazıldığından, aygıt verileri Azure Depolama'ya yükler. Aygıt, belirli bir eşiğe ulaştığında meta verileri korurken dosyaları yerel olarak kaldırarak depolama alanını otomatik olarak yönetir. Meta verilerin yerel bir kopyasını tutmak, ağ geçidi aygıtının değişiklikleri yalnızca dosya güncelleştirildiğinde yüklemesine olanak tanır. Ağ geçidi aygıtınıza yüklenen veriler, [Veri yükleme ihtarlarında](data-box-gateway-limits.md#data-upload-caveats)belirtilen yönergelere uygun olmalıdır.

Aygıt verilerle dolduğunda, verilerin buluta yüklenme hızıyla eşleşecek şekilde giriş hızını (gerektiği gibi) daraltmaya başlar. Cihazdaki sürekli alımını izlemek için uyarılar kullanırsınız. Bu uyarılar, azaltma başladıktan sonra yükseltilir ve azaltma durdurulduktan sonra temizlenir.

## <a name="cloud-archival-of-data"></a>Verilerin bulut arşivlemi

Verilerinizi bulutta uzun süre saklamak istediğinizde Veri Kutusu Ağ Geçidi'ni kullanın. Uzun süreli bekletme için **Arşiv** depolama katmanını kullanabilirsiniz.

Arşiv katmanı, nadiren erişilen verileri en az 180 gün boyunca depolamak için optimize edilmiştir. **Arşiv** katmanı en düşük depolama maliyetlerini sunar, ancak en yüksek erişim maliyetlerine sahiptir. Daha fazla bilgi için [Arşiv erişim katmanına](/azure/storage/blobs/storage-blob-storage-tiers#archive-access-tier)gidin.

### <a name="move-data-to-archive-tier"></a>Verileri Arşiv katmanına taşıma

Başlamadan önce çalışan bir Veri Kutusu Ağ Geçidi aygıtınız olduğundan emin olun. Öğretici'de ayrıntılı olarak belirtilen adımları [izleyin: Azure Veri Kutusu Ağ Geçidi'ni dağıtmaya hazırlanın](data-box-gateway-deploy-prep.md) ve operasyonel bir aygıtınız olana kadar bir sonraki öğreticiye ilerlemeye devam edin.

- Veri Kutusu Ağ Geçidi üzerinden Veri Aktarımı'nda açıklandığı gibi, her zamanki aktarım yordamı yla Azure'a veri yüklemek için [Veri Kutusu Ağ Geçidi aygıtını](data-box-gateway-deploy-add-shares.md)kullanın.
- Veriler yüklendikten sonra Arşiv katmanına taşımanız gerekir. Blob katmanını iki şekilde ayarlayabilirsiniz: Azure PowerShell komut dosyası veya Azure Depolama Yaşam Döngüsü Yönetimi ilkesi.  
    - Azure PowerShell kullanıyorsanız, verileri Arşiv katmanına taşımak için aşağıdaki [adımları](/azure/databox/data-box-how-to-set-data-tier#use-azure-powershell-to-set-the-blob-tier) izleyin.
    - Azure Yaşam Döngüsü Yönetimi'ni kullanıyorsanız, verileri Arşiv katmanına taşımak için aşağıdaki adımları izleyin.
        - Arşiv katmanını kullanmak için Blob Yaşam Döngüsü yönetim hizmetinin önizlemesine [kaydolun.](/azure/storage/common/storage-lifecycle-management-concepts)
        - Verileri [yutma üzerine arşivlemek](/azure/storage/blobs/storage-lifecycle-management-concepts#archive-data-after-ingest)için aşağıdaki ilkeyi kullanın.
- Lekeler Arşiv olarak işaretlendikten sonra, sıcak veya soğuk katmana taşınmadıkları sürece ağ geçidi tarafından değiştirilemezler. Dosya yerel depolama alanındaysa, yerel kopyada yapılan değişiklikler (silmeler dahil) arşiv katmanına yüklenmez.
- Arşiv depolamasındaki verileri okumak için, blob katmanını sıcak veya serin olarak değiştirerek yeniden sulandırılmalıdır. Ağ [geçidindeki payı yenilemek](data-box-gateway-manage-shares.md#refresh-shares) lekeyi yeniden sulendirmez.

Daha fazla bilgi için [Azure Blob Depolama Yaşam Döngüsünü](/azure/storage/common/storage-lifecycle-management-concepts)Nasıl Yönetebilirsiniz hakkında daha fazla bilgi edinin.

## <a name="initial-bulk-transfer-followed-by-incremental-transfer"></a>İlk toplu aktarım ve ardından artımlı aktarım

Büyük miktarda veriyi ve ardından artımlı aktarımları toplu olarak yüklemek istediğinizde Veri Kutusu ve Veri Kutusu Ağ Geçidi'ni birlikte kullanın. Ağ üzerinden artımlı aktarımlar (devam eden akış) için çevrimdışı modda (ilk tohum) toplu aktarım için Veri Kutusu'nu ve Veri Kutusu Ağ Geçidi'ni kullanın.

### <a name="seed-the-data-with-data-box"></a>Veri Kutusu ile verileri tohumla

Verileri Veri Kutusu'na kopyalamak ve Azure Depolama'ya yüklemek için aşağıdaki adımları izleyin.

1. [Veri Kutunuzu Sipariş Edin.](/azure/databox/data-box-deploy-ordered)
2. [Veri Kutunuzu ayarlayın.](/azure/databox/data-box-deploy-set-up)
3. [Verileri SMB üzerinden Veri Kutusu'na kopyalayın.](/azure/databox/data-box-deploy-copy-data)
4. [Veri Kutusunu döndürün, veri yüklemesini Azure'a doğrulayın.](/azure/databox/data-box-deploy-picked-up)
5. Azure'a veri yüklemesi tamamlandıktan sonra, tüm veriler Azure depolama kapsayıcılarında olmalıdır. Veri Kutusu'nun depolama hesabında, tüm verilerin kopyalandığından emin olmak için Blob (ve Dosya) kapsayıcısına gidin. Bu adı daha sonra kullanacağınız için kapsayıcı adını not edin. Örneğin, aşağıdaki ekran görüntüsünde, `databox` kapsayıcı artımlı aktarım için kullanılır.

    ![Veri Kutusu'nda veri içeren kapsayıcı](media/data-box-gateway-use-cases/data-container1.png)

Bu toplu aktarım ilk tohumlama aşamasını tamamlar.

### <a name="ongoing-feed-with-data-box-gateway"></a>Veri Kutusu Ağ Geçidi ile devam eden akış

Data Box Ağ Geçidi tarafından devam eden alım için aşağıdaki adımları izleyin.

1. Veri Kutusu Ağ Geçidi'nde bulut paylaşımı oluşturun. Bu paylaşım, tüm verileri otomatik olarak Azure Depolama hesabına yükler. Veri Kutusu Ağ Geçidi kaynağınızdaki **Paylaşımlar'a** gidin ve **+ Paylaş ekle'yi**tıklatın.

    ![+Ekle paylaşım'ı tıklatın](media/data-box-gateway-use-cases/add-share1.png)

2. Bu paylaşım, tohumlu verileri içeren kapsayıcıyla eşlerinizi olduğundan emin olun. **Blob kapsayıcısını seç**seçeneğini belirleyin ve **Varolan'ı kullanın** ve Veri Kutusu'ndaki verilerin aktarıldığı kapsayıcıya göz atın.

    ![Ayarları paylaşın](media/data-box-gateway-use-cases/share-settings-select-existing-container1.png)

3. Paylaşım oluşturulduktan sonra, payı yenileyin. Bu işlem, Azure'daki içerikle şirket içi paylaşımı yeniler.

    ![Paylaşımı yenile](media/data-box-gateway-use-cases/refresh-share1.png)

    Paylaşım eşitlendiğinde, dosyalar istemcide değiştirildiğinde Veri Kutusu Ağ Geçidi artımlı değişiklikleri yükler.

## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Gateway sistem gereksinimlerini](data-box-gateway-system-requirements.md) gözden geçirin.
- [Data Box Gateway sınırlarını](data-box-gateway-limits.md) anlayın.
- Azure portalında [Azure Data Box Gateway](data-box-gateway-deploy-prep.md)’i dağıtın.