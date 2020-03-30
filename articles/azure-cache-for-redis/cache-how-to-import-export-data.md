---
title: Redis için Azure Önbelleğinde veri alma ve dışa aktarma
description: Redis örnekleri için premium Azure Önbelleğiile blob depolamaya veri alma ve verme konusunda nasıl bilgi edinin
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 07/31/2017
ms.author: yegu
ms.openlocfilehash: 29ad5ca6c9058b88a539c7a3bb8ace4d9a65083a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278096"
---
# <a name="import-and-export-data-in-azure-cache-for-redis"></a>Redis için Azure Önbelleğinde veri alma ve dışa aktarma
Alma/Dışa Aktarma, Redis için Verileri Azure Önbelleğine aktarmanıza veya Redis için Azure Önbelleğinden veri dışa aktarmanıza olanak tanıyan bir Azure Önbelleğidir ve bir Azure Depolama Hesabı'ndaki bir azure önbelleğinden (RDB) bir Azure Önbelleği anlık görüntüsünü içe aktarıp dışa aktarAbilirsiniz.

- **Dışa aktarma** - Redis RDB anlık görüntülerini bir Sayfa Blob'una dışa aktarabilirsiniz.
- **Alma** - Redis RDB anlık görüntülerini Bir Sayfa Blob'undan veya Blok Blob'undan almak için Azure Önbelleğinizi içe aktarabilirsiniz.

Alma/Dışa Aktarma, Redis örnekleri için farklı Azure Önbelleği arasında geçiş yapmanızı veya kullanmadan önce önbelleği verilerle doldurmanızı sağlar.

Bu makalede, Redis için Azure Önbelleği ile veri alma ve dışa aktarma kılavuzu ve sık sorulan soruların yanıtlarını sağlar.

> [!IMPORTANT]
> Alma/Dışa Aktarma yalnızca [premium katman](cache-premium-tier-intro.md) önbellekleri için kullanılabilir.
>
>

## <a name="import"></a>İçeri Aktarma
Alma, Linux, Windows veya Amazon Web Hizmetleri ve diğerleri gibi herhangi bir bulut sağlayıcısında çalışan Redis de dahil olmak üzere herhangi bir bulut veya ortamda çalışan herhangi bir Redis sunucusundan Redis uyumlu RDB dosyalarını getirmek için kullanılabilir. Veri alma, önceden doldurulmuş verilerle bir önbellek oluşturmanın kolay bir yoludur. Alma işlemi sırasında, Redis için Azure Önbelleği RDB dosyalarını Azure depolama alanından belleğe yükler ve anahtarları önbelleğe ekler.

> [!NOTE]
> Alma işlemine başlamadan önce Redis Veritabanı (RDB) dosyanızın veya dosyanızın Azure depolama alanında, aynı bölgede ve Redis örneğinde Azure Önbelleği ile abonelikte sayfaveya blok blob'larına yüklendiğinden emin olun. Daha fazla bilgi için Azure [Blob depolama alanına başlayın'](../storage/blobs/storage-dotnet-how-to-use-blobs.md)a bakın. RDB dosyanızı [Redis Dışa Aktarım için Azure Önbelleği](#export) özelliğini kullanarak dışa aktarıyorsanız, RDB dosyanız zaten bir sayfa blob'unda depolanır ve içe aktarılmaya hazırdır.
>
>

1. Bir veya daha fazla dışa aktarılan önbellek bloblarını almak için Azure portalındaki [önbelleğinize göz atın](cache-configure.md#configure-azure-cache-for-redis-settings) ve **Kaynak menüsünden**verileri içe **aktar'ı** tıklatın.

    ![Veri içeri aktarma](./media/cache-how-to-import-export-data/cache-import-data.png)
2. **Blob(lar) seçeneğini** tıklatın ve içe aktarılan verileri içeren depolama hesabını seçin.

    ![Depolama hesabı seçin](./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png)
3. İçe aktarılabilmek için verileri içeren kapsayıcıyı tıklatın.

    ![Kapsayıcı yı seçin](./media/cache-how-to-import-export-data/cache-import-choose-container.png)
4. Blob adının solundaki alanı tıklatarak almak için bir veya daha fazla blob seçin ve sonra **Seç'i**tıklatın.

    ![Blobs seçin](./media/cache-how-to-import-export-data/cache-import-choose-blobs.png)
5. Alma işlemini başlatmak için **İçe Aktar'ı** tıklatın.

   > [!IMPORTANT]
   > Önbelleğe alma işlemi sırasında önbellek istemcileri tarafından erişilemez ve önbellekteki varolan veriler silinir.
   >
   >

    ![İçeri Aktarma](./media/cache-how-to-import-export-data/cache-import-blobs.png)

    Azure portalındaki bildirimleri izleyerek veya [denetim günlüğündeki](../azure-resource-manager/management/view-activity-logs.md)olayları görüntüleyerek alma işleminin ilerlemesini izleyebilirsiniz.

    ![İthalat ilerlemesi](./media/cache-how-to-import-export-data/cache-import-data-import-complete.png)

## <a name="export"></a>Dışarı Aktarma
Dışa aktarma, Redis için Azure Önbelleğinde depolanan verileri Redis uyumlu RDB dosyasına(lar) dışa aktarmanızı sağlar. Bu özelliği, Redis örneği için bir Azure Önbelleğinden diğerine veya başka bir Redis sunucusuna taşımak için kullanabilirsiniz. Dışa aktarma işlemi sırasında, Redis sunucusu örneği için Azure Önbelleğini barındıran VM'de geçici bir dosya oluşturulur ve dosya belirlenen depolama hesabına yüklenir. Dışa aktarma işlemi başarı veya hata durumuyla tamamlandığında, geçici dosya silinir.

1. Önbelleğin geçerli içeriğini depolamaya aktarmak için Azure portalındaki [önbelleğinize göz atın](cache-configure.md#configure-azure-cache-for-redis-settings) ve **Kaynak menüsünden**verileri **dışa** aktar'ı tıklatın.

    ![Depolama kapsayıcısı seçin](./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png)
2. **Depolama Kapsayıcısı'nı seçin** ve istenen depolama hesabını seçin. Depolama hesabı önbelleğinizle aynı abonelikte ve bölgede olmalıdır.

   > [!IMPORTANT]
   > Hem klasik hem de Kaynak Yöneticisi depolama hesapları tarafından desteklenen, ancak şu anda Blob depolama hesapları tarafından desteklenmeyen sayfa lekeleri ile dışa aktarma çalışmaları. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../storage/common/storage-account-overview.md).
   >

    ![Depolama hesabı](./media/cache-how-to-import-export-data/cache-export-data-choose-account.png)
3. İstenilen blob kabını seçin ve **Seç'e**tıklayın. Yeni bir kapsayıcı kullanmak için önce **Kapsayıcı Ekle'yi** tıklatın ve sonra listeden seçin.

    ![Depolama kapsayıcısı seçin](./media/cache-how-to-import-export-data/cache-export-data-container.png)
4. Bir **Blob adı öneki** yazın ve dışa aktarma işlemini başlatmak için **Dışa** Aktar'ı tıklatın. Blob adı öneki, bu dışa aktarma işlemi tarafından oluşturulan dosyaların adlarını önek için kullanılır.

    ![Dışarı Aktarma](./media/cache-how-to-import-export-data/cache-export-data.png)

    Azure portalındaki bildirimleri izleyerek veya [denetim günlüğündeki](../azure-resource-manager/management/view-activity-logs.md)olayları görüntüleyerek dışa aktarma işleminin ilerlemesini izleyebilirsiniz.

    ![Dışa aktarma verileri tamamlandı](./media/cache-how-to-import-export-data/cache-export-data-export-complete.png)

    Önbellekler dışa aktarma işlemi sırasında kullanılabilir durumda kalır.

## <a name="importexport-faq"></a>İthalat/İhracat SSS
Bu bölümde, İçe Aktarma/Dışa Aktarma özelliği hakkında sık sorulan sorular yer almaktadır.

* [Hangi fiyatlandırma katmanları İçe/Dışa Aktar'ı kullanabilir?](#what-pricing-tiers-can-use-importexport)
* [Herhangi bir Redis sunucusundan veri aktarabilir miyim?](#can-i-import-data-from-any-redis-server)
* [Hangi RDB sürümlerini içe aktarabilirim?](#what-rdb-versions-can-i-import)
* [Önbelleğe alma/dışa aktarma işlemi sırasında kullanılabilir mi?](#is-my-cache-available-during-an-importexport-operation)
* [Redis kümesiile Alma/Dışa Aktarma'yı kullanabilir miyim?](#can-i-use-importexport-with-redis-cluster)
* [Özel veritabanları ayarı ile Dışa Aktarma/Dışa Aktarma nasıl çalışır?](#how-does-importexport-work-with-a-custom-databases-setting)
* [Redis'in kalıcılığından İthalat/İhracat'ın farkı nedir?](#how-is-importexport-different-from-redis-persistence)
* [PowerShell, CLI veya diğer yönetim istemcilerini kullanarak Alma/Dışa Aktarma'yı otomatikleştirebilir miyim?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [Alma/Dışa Aktarma işlemi sırasında bir zaman arahatası aldım. Bu ne anlama geliyor?](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [Verilerimi Azure Blob Depolama'ya dışa aktarırken bir hata oluştu. Ne oldu?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>Hangi fiyatlandırma katmanları İçe/Dışa Aktar'ı kullanabilir?
İthalat/İhracat yalnızca premium fiyatlandırma katmanında kullanılabilir.

### <a name="can-i-import-data-from-any-redis-server"></a>Herhangi bir Redis sunucusundan veri aktarabilir miyim?
Evet, Redis örnekleri için Azure Önbellek'ten dışa aktarılan verileri almanın yanı sıra, Linux, Windows veya Amazon Web Hizmetleri gibi bulut sağlayıcıları gibi herhangi bir bulut veya ortamda çalışan herhangi bir Redis sunucusundan RDB dosyalarını içe aktarabilirsiniz. Bunu yapmak için, RDB dosyasını istenilen Redis sunucusundan bir sayfaya yükleyin veya bir Azure Depolama Hesabı'nda blob'u engelleyin ve ardından Redis örneği için premium Azure Önbelleğinize aktarın. Örneğin, verileri üretim önbelleğinizden dışa aktarmak ve sınama veya geçiş için hazırlama ortamının bir parçası olarak kullanılan bir önbelleğe aktarmak isteyebilirsiniz.

> [!IMPORTANT]
> Sayfa blob'u kullanırken Redis için Azure Önbelleği dışındaki Redis sunucularından aktarılan verileri başarıyla almak için sayfa blob boyutunun 512 bayt sınırında hizalanması gerekir. Örnek kodun gerekli bayt dolgularını gerçekleştirmek için [Örnek sayfa blob yüklemesine](https://github.com/JimRoberts-MS/SamplePageBlobUpload)bakın.
>
>

### <a name="what-rdb-versions-can-i-import"></a>Hangi RDB sürümlerini içe aktarabilirim?

Redis için Azure Önbelleği, RDB sürümü 7 aracılığıyla RDB içe aktarmayı destekler.

### <a name="is-my-cache-available-during-an-importexport-operation"></a>Önbelleğe alma/dışa aktarma işlemi sırasında kullanılabilir mi?
* **Dışa Aktarma** - Önbellekler kullanılabilir durumda kalır ve bir dışa aktarma işlemi sırasında önbelleğinizi kullanmaya devam edebilirsiniz.
* **Alma** - Önbellekler, bir alma işlemi başladığında kullanılamaz hale gelir ve alma işlemi tamamlandığında kullanıma hazır hale gelir.

### <a name="can-i-use-importexport-with-redis-cluster"></a>Redis kümesiile Alma/Dışa Aktarma'yı kullanabilir miyim?
Evet ve kümelenmiş önbellek ile kümelenmiş olmayan önbellek arasında içe aktarabilirsiniz/dışa aktarabilirsiniz. Redis cluster [yalnızca veritabanı 0'ı desteklediğinden,](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)0 dışındaki veritabanlarındaki veriler içe aktarılmaz. Kümelenmiş önbellek verileri içe aktarıldığında, anahtarlar kümenin parçaları arasında yeniden dağıtılır.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Özel veritabanları ayarı ile Dışa Aktarma/Dışa Aktarma nasıl çalışır?
Bazı fiyatlandırma katmanlarının farklı [veritabanları sınırları](cache-configure.md#databases)vardır, bu nedenle önbellek oluşturma sırasında `databases` ayar için özel bir değer yapılandırdığınızda içe aktarırken bazı hususlar vardır.

* Dışa aktardığınız katmandan daha `databases` düşük bir sınıra sahip bir fiyatlandırma katmanına içe aktarırken:
  * Tüm fiyatlandırma katmanları için `databases`16 olan varsayılan sayıyı kullanıyorsanız, veri kaybolmaz.
  * Bu özel `databases` bir sayı kullanıyorsanız, içe aktardığınız katman için sınırlar içinde kalır, hiçbir veri kaybolmaz.
  * Dışa aktarılan verileriniz, yeni katmanın sınırlarını aşan bir veritabanında veri içeriyorsa, bu yüksek veritabanlarından gelen veriler alınmaz.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Redis'in kalıcılığından İthalat/İhracat'ın farkı nedir?
Redis için Azure Önbelleği kalıcılığı, Redis'te depolanan verileri Azure Depolama'ya kadar devam etmenizi sağlar. Kalıcılık yapılandırıldıktan sonra, Redis için Azure Önbelleği, Redis ikili biçiminde ki Azure Önbelleğinin anlık görüntüsünü yapılandırılabilir yedekleme frekansını temel alan diske olarak saklar. Hem birincil hem de yineleme önbelleğini devre dışı eden bir felaket oluşursa, önbellek verileri en son anlık görüntü kullanılarak otomatik olarak geri yüklenir. Daha fazla bilgi için [Redis için Premium Azure Önbelleği için veri kalıcılığını nasıl yapılandıracağınız](cache-how-to-premium-persistence.md)abakın.

Alma/Dışa Aktarma, Redis için Azure Önbelleğine veri getirmenize veya dışa aktarmanıza olanak tanır. Redis kalıcılığını kullanarak yedeklemeyi yapılandırmaz ve geri yüklemez.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>PowerShell, CLI veya diğer yönetim istemcilerini kullanarak Alma/Dışa Aktarma'yı otomatikleştirebilir miyim?
Evet, PowerShell yönergeleri [için bkz. Redis için bir Azure Önbelleği almak](cache-how-to-manage-redis-cache-powershell.md#to-import-an-azure-cache-for-redis) ve [Redis için bir Azure Önbelleği dışa aktarmak için](cache-how-to-manage-redis-cache-powershell.md#to-export-an-azure-cache-for-redis)bkz.

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Alma/Dışa Aktarma işlemi sırasında bir zaman arahatası aldım. Bu ne anlama geliyor?
İşlemi başlatmadan önce 15 dakikadan uzun süre **Veri Aktar** veya **Dışa** Aktar veri skalasında kalırsanız, aşağıdaki örneğe benzer bir hata iletisi ile hata alırsınız:

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Bunu çözmek için, 15 dakika geçmeden alma veya dışa aktarma işlemini başlatın.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Verilerimi Azure Blob Depolama'ya dışa aktarırken bir hata oluştu. Ne oldu?
Dışa aktarma yalnızca sayfa blobs olarak depolanan RDB dosyaları ile çalışır. Diğer blob türleri şu anda, sıcak ve serin katmanları ile Blob depolama hesapları da dahil olmak üzere desteklenmez. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../storage/common/storage-account-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliğini nasıl kullanacağınızı öğrenin.

* [Redis Premium katmanı için Azure Önbelleğine Giriş](cache-premium-tier-intro.md)
