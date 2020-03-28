---
title: 'Öğretici: Cihazınıza kopyalamak için veri kopyalama hizmetini kullanın'
titleSuffix: Azure Data Box
description: Bu eğitimde, veri kopyalama hizmeti aracılığıyla verileri Azure Veri Kutusu aygıtınıza kopyalamayı öğrenirsiniz
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: alkohli
ms.openlocfilehash: ef0d79cae11a382bcca0ddb61e1d4a04b5db41e9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79501867"
---
# <a name="tutorial-use-the-data-copy-service-to-copy-data-into-azure-data-box-preview"></a>Öğretici: Verileri Azure Veri Kutusu'na kopyalamak için veri kopyalama hizmetini kullanın (önizleme)

Bu öğretici, bir ara ana bilgisayar olmadan veri kopyalama hizmetini kullanarak veri nasıl yutulmak açıklanır. Veri kopyalama hizmeti Microsoft Azure Veri Kutusu'nda yerel olarak çalışır, ağ bağlantılı depolama (NAS) cihazınıza Kobİ üzerinden bağlanır ve verileri Veri Kutusu'na kopyalar.

Veri kopyalama hizmetini kullanın:

- Ara ana bilgisayarların kullanılamadığı NAS ortamlarında.
- Veri alımı ve yüklenmesi haftalar süren küçük dosyalarla. Veri kopyalama hizmeti, küçük dosyaların alım ve yükleme süresini önemli ölçüde artırır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Data Box'a veri kopyalama

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Bu öğreticiyi tamamladınız: [Azure Veri Kutusu'nı ayarlayın.](data-box-deploy-set-up.md)
2. Veri Kutusu aygıtınızı aldınız ve portaldaki sipariş durumu **teslim**edildi.
3. Veri kopyalama için bağlanacağınız kaynak NAS aygıtının kimlik bilgilerine sahipsiniz.
4. Yüksek hızlı bir ağa bağlısınız. En az bir adet 10 Gigabit Ethernet (GbE) bağlantınız olduğunu şiddetle tavsiye ediyoruz. 10 GbE bağlantı yoksa, 1 GbE veri bağlantısı kullanabilirsiniz, ancak kopyalama hızları etkilenir.

## <a name="copy-data-to-data-box"></a>Data Box'a veri kopyalama

NAS aygıtına bağlandıktan sonra, bir sonraki adım verilerinizi kopyalamaktır. Verileri kopyalamaya başlamadan önce aşağıdaki konuları gözden geçirin:

* Verileri kopyalarken, veri boyutunun Azure depolama ve Veri Kutusu [sınırlarında](data-box-limits.md)açıklanan boyut sınırlarına uyduklarından emin olun.

* Data Box tarafından yüklenen veriler, Veri Kutusu dışındaki diğer uygulamalar tarafından aynı anda yüklenirse, yükleme işi hataları ve veri bozulmasına neden olabilir.

* Veri kopyalama hizmeti okurken veriler değiştiriliyorsa, verilerin hataları veya bozulması nı görebilirsiniz.

* Veri Kutusu'nun verilerinizi Azure Depolama'ya aktardığını doğrulayana kadar kaynak verilerin bir kopyasını koruduğunuzdan emin olun.

Veri kopyalama hizmetini kullanarak verileri kopyalamak için bir iş oluşturmanız gerekir:

1. Veri Kutusu aygıtınızın yerel web ui'sinde Kopya**verilerini** **yönet'e** > gidin.
2. Veri **Kopyala** sayfasında **Oluştur'u**seçin.

    !["Verileri kopyala" sayfasında Oluştur'u seçin](media/data-box-deploy-copy-data-via-copy-service/click-create.png)

3. **Yapıliş ve başlat** iletişim kutusunda aşağıdaki alanları doldurun:
    
    |Alan                          |Değer    |
    |-------------------------------|---------|
    |**İş adı**                       |İş için 230 karakterden az benzersiz bir ad. Bu karakterler iş adına izin verilmez: \< \>, \| \?, \* \\, \: \/, , , ,\\\.         |
    |**Kaynak konumu**                |SMB yolunu veri kaynağına formatta `\\<ServerIPAddress>\<ShareName>` sağlayın: veya `\\<ServerName>\<ShareName>`.        |
    |**Username**                       |Veri kaynağına erişmek için biçimde `\\<DomainName><UserName>` kullanıcı adı. Yerel bir yönetici bağlanıyorsa, açık güvenlik izinlerine ihtiyaç duyarlar. Klasöre sağ tıklayın, **Özellikler'i** seçin ve ardından **Güvenlik'i**seçin. Bu, **Güvenlik** sekmesinde yerel yöneticiyi eklemelidir.       |
    |**Parola**                       |Veri kaynağına erişmek için parola.           |
    |**Hedef depolama hesabı**    |Listeden veri yüklemek için hedef depolama hesabını seçin.         |
    |**Hedef türü**       |Listeden hedef depolama türünü seçin: **Blob'** u, **Sayfa Blob'** u veya **Azure Dosyalarını**engelle .        |
    |**Hedef kapsayıcı/paylaş**    |Hedef depolama hesabınıza veri yüklemek istediğiniz kapsayıcının adını veya paylaşınızı girin. Ad bir paylaşım adı veya kapsayıcı adı olabilir. Örneğin `myshare` veya `mycontainer` kullanın. Ayrıca, adı biçime `sharename\directory_name` veya `containername\virtual_directory_name`.        |
    |**Dosyaları eşleşen deseni kopyalama**    | Dosya adı eşleştirme desenini aşağıdaki iki şekilde girebilirsiniz:<ul><li>**Joker karakter ifadelerini kullanın:** Yalnızca `*` `?` ve joker karakter ifadelerinde desteklenir. Örneğin, ifade `*.vhd` uzantısı olan tüm `.vhd` dosyalarla eşleşir. Benzer `*.dl?` şekilde, tüm dosyaları `.dl` uzantıyla eşleştirin ya da `.dl`' ile başlayan , gibi. `.dll` Aynı şekilde, `*foo` adları `foo`.<br>Doğrudan alana joker karakter ifadesini girebilirsiniz. Varsayılan olarak, alana girdiğiniz değer joker karakter ifadesi olarak kabul edilir.</li><li>**Normal ifadeler kullanın:** POSIX tabanlı düzenli ifadeler desteklenir. Örneğin, normal ifade `.*\.vhd` `.vhd` uzantısı olan tüm dosyaları eşleşir. Normal ifadeler için `<pattern>` doğrudan . `regex(<pattern>)` Normal ifadeler hakkında daha fazla bilgi için [Normal ifade diline](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)gidin - hızlı bir başvuru .</li><ul>|
    |**Dosya optimizasyonu**              |Bu özellik etkinleştirildiğinde, 1 MB'dan küçük dosyalar yutma sırasında paketlenir. Bu ambalaj, küçük dosyalar için veri kopyasını hızlandırıyor. Ayrıca, dosya sayısı dizin sayısını aştığında önemli miktarda zaman kazandırır.        |
 
4. **Başlat**'ı seçin. Girişler doğrulanır ve doğrulama başarılı olursa, iş başlar. İşin başlaması birkaç dakika sürebilir.

    !["İşi yapılandır ve başlat" iletişim kutusundan bir iş başlatma](media/data-box-deploy-copy-data-via-copy-service/configure-and-start.png)

5. Belirtilen ayarları içeren bir iş oluşturulur. Bir işi duraklatabilir, devam ettirebilir, iptal edebilir veya yeniden başlatabilirsiniz. İş adının yanındaki onay kutusunu seçin ve ardından uygun düğmeyi seçin.

    !["Verileri kopyalama" sayfasında bir işi yönetme](media/data-box-deploy-copy-data-via-copy-service/select-job.png)
    
    - Yoğun saatlerde NAS aygıtının kaynaklarını etkiliyorsa işi duraklatabilirsiniz:

        !["Verileri kopyalama" sayfasındaki bir işi duraklatma](media/data-box-deploy-copy-data-via-copy-service/pause-job.png)

        Yoğun olmayan saatlerde işe daha sonra devam edebilirsiniz:

        !["Verileri kopyalama" sayfasında ki bir işe devam edin](media/data-box-deploy-copy-data-via-copy-service/resume-job.png)

    - Bir işi istediğiniz zaman iptal edebilirsiniz:

        !["Verileri kopyalama" sayfasındaki bir işi iptal etme](media/data-box-deploy-copy-data-via-copy-service/cancel-job.png)
        
        Bir işi iptal ettiğinizde, bir onay gereklidir:

        ![İş iptalini onaylama](media/data-box-deploy-copy-data-via-copy-service/confirm-cancel-job.png)

        Bir işi iptal etmeye karar verirseniz, zaten kopyalanmış veriler silinmez. Veri Kutusu aygıtınıza kopyaladığınız tüm verileri silmek için aygıtı sıfırlayın.

        ![Aygıtı sıfırlama](media/data-box-deploy-copy-data-via-copy-service/reset-device.png)

        >[!NOTE]
        > Bir işi iptal ederseniz veya duraklatsanız, büyük dosyalar yalnızca kısmen kopyalanabilir. Kısmen kopyalanan bu dosyalar aynı durumda Azure'a yüklenir. Bir işi iptal ettiğinizde veya duraklattığınızda, dosyalarınızın düzgün kopyalandığından emin olun. Dosyaları doğrulamak için Kobİ paylaşımlarına bakın veya BOM dosyasını indirin.

    - Ağ hatası gibi geçici bir hata nedeniyle başarısız olduysa işi yeniden başlatabilirsiniz. Ancak, **başarılı** veya **hatalarla tamamlanmış**gibi bir terminal durumuna ulaşmışsa, bir işi yeniden başlatamazsınız. İş hataları, dosya adlandırma veya dosya boyutu sorunları neden olabilir. Bu hatalar günlüğe kaydedilir, ancak iş tamamlandıktan sonra yeniden başlatılamaz.

        ![Başarısız bir işi yeniden başlatma](media/data-box-deploy-copy-data-via-copy-service/restart-failed-job.png)

        Bir hatayla karşılaşırsanız ve işi yeniden başlatamıyorsanız, hata günlüklerini indirin ve günlük dosyalarındaki hataya bakın. Sorunu düzelttinden sonra, dosyaları kopyalamak için yeni bir iş oluşturun. [Ayrıca SMB üzerinden dosyaları](data-box-deploy-copy-data.md)kopyalayabilirsiniz.
    
    - Bu sürümde, bir işi silemezsiniz.
    
    - Sınırsız iş oluşturabilirsiniz, ancak herhangi bir anda paralel olarak en fazla 10 iş çalıştırabilirsiniz.
    - **Dosya optimizasyonu** açıksa, kopya performansını artırmak için küçük dosyalar yutulur da paketlenir. Bu gibi durumlarda, paketlenmiş bir dosya görürsünüz (dosya adı olarak bir GUID'si olacaktır). Bu dosyayı silmeyin. Yükleme sırasında ambalajı açılacaktır.

6. İş devam ederken, Veri **Kopyala** sayfasında:

    - **Durum** sütununda, kopyalama işinin durumunu görüntüleyebilirsiniz. Durum şu olabilir:
        - **Çalışıyor**
        - **Başarısız**
        - **Başarılı oldu**
        - **Duraklatılıyor**
        - **Duraklatıldı**
        - **İptal Ediliyor**
        - **İptal edildi**
        - **Hatalarla tamamlandı**
    - **Dosyalar** sütununda, kopyalanan dosyaların sayısını ve toplam boyutunu görebilirsiniz.
    - **İşlenen** sütunda, işlenen dosyaların sayısını ve toplam boyutunu görebilirsiniz.
    - İş **ayrıntıları** sütununda, iş ayrıntılarını görmek için **Görünüm'i** seçin.
    - **# Hatalar** sütununda gösterildiği gibi kopyalama işlemi sırasında herhangi bir hata oluşursa, **Hata günlüğü** sütununa gidin ve sorun giderme için hata günlüklerini indirin.

Kopyalama işinin bitmesini bekleyin. Bazı hatalar yalnızca **Bağlan ve kopyala** sayfasında günlüğe kaydedildiği için, bir sonraki adıma geçmeden önce kopyalama işinin hiçbir hata olmadan tamamlandığına emin olun.

!["Bağlan ve kopyala" sayfasında hata yok](media/data-box-deploy-copy-data-via-copy-service/verify-no-errors-on-connect-and-copy.png)

Veri bütünlüğünü sağlamak için, veriler kopyalanırken bir checksum satır içinde hesaplanır. Kopya tamamlandıktan sonra, cihazınızda kullanılan alanı ve boş alanı doğrulamak için **Görünüm panosunu** seçin.
    
![Panoda boş ve kullanılan alanı doğrulama](media/data-box-deploy-copy-data-via-copy-service/verify-used-space-dashboard.png)

Kopyalama işi bittikten **sonra, gönderiye hazırla'yı**seçebilirsiniz.

>[!NOTE]
> Kopyalama işleri devam ederken **gönderiye hazırlanın** çalıştırılama.

## <a name="next-steps"></a>Sonraki adımlar

Veri Kutusu aygıtınızı Microsoft'a nasıl geri göndertin ilerler öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Veri Kutusu cihazınızı Microsoft'a gönderme](./data-box-deploy-picked-up.md)

