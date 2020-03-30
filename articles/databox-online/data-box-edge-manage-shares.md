---
title: Azure Veri Kutusu Kenarı paylaşım yönetimi | Microsoft Dokümanlar
description: Azure Veri Kutusu Kenarı'ndaki paylaşımları yönetmek için Azure portalını nasıl kullanacağınızı açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: b25409c63806e203bd841b0373543b7cc2b96d9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79212933"
---
# <a name="use-the-azure-portal-to-manage-shares-on-your-azure-data-box-edge"></a>Azure Veri Kutusu Kenarınızdaki paylaşımları yönetmek için Azure portalını kullanın

Bu makalede, Azure Veri Kutusu Kenarı'nızdaki paylaşımların nasıl yönetilen bir şekilde yönetilen bir şekilde açıklanmaktadır. You can manage the Azure Data Box Edge via the Azure portal or via the local web UI. Paylaşımlarla ilişkili depolama hesabı için depolama anahtarı eklemek, silmek, yenilemek veya depolama anahtarını eşitlemek için Azure portalını kullanın.

## <a name="about-shares"></a>Paylaşımlar hakkında

Verileri Azure'a aktarmak için Azure Veri Kutusu Kenarınız'da paylaşımlar oluşturmanız gerekir. Veri Kutusu Kenarı aygıtına eklediğiniz paylaşımlar, verileri buluta iten yerel paylaşımlar veya paylaşımlar olabilir.

 - **Yerel paylaşımlar**: Verilerin aygıtta yerel olarak işlenmesini istediğinizde bu paylaşımları kullanın.
 - **Paylaşımlar**: Cihaz verilerinin buluttaki depolama hesabınıza otomatik olarak itilmesini istediğinizde bu paylaşımları kullanın. Depolama alanını **Yenile** ve **Eşitle** tuşları gibi tüm bulut işlevleri paylaşımlar için geçerlidir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşımı silme
> * Paylaşımları yenileme
> * Depolama anahtarını eşitleme


## <a name="add-a-share"></a>Paylaşım ekleme

Paylaşım oluşturmak için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Azure portalında, Veri Kutusu Kenarı kaynağınıza gidin ve ardından **Paylaşımlar > Ağ Geçidi'ne**gidin. Komut çubuğunda **+ Paylaşım ekle'yi** seçin.

    ![Paylaş ekle'yi seçin](media/data-box-edge-manage-shares/add-share-1.png)

2. **Paylaşım Ekle**'de, paylaşım ayarlarını belirtin. Paylaşımınız için benzersiz bir ad sağlayın.
    
    Paylaşım adları yalnızca rakam, küçük harf ve kısa çizgiler içerebilir. Paylaşım adı 3 ile 63 karakter arası uzunlukta olmalı ve bir harf veya rakamla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır.

3. Paylaşım için **Tür** seçin. Tür **SMB** veya **NFS**olabilir, SMB varsayılan olarak. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır. SMB paylaşımları mı yoksa NFS paylaşımları mı seçtiğinize bağlı olarak, gösterilen seçenekler biraz farklı olur.

4. Paylaşımın duracağı **Depolama hesabını** sağlamanız gerekir. Henüz kapsayıcı yoksa, depolama hesabında paylaşım adıyla bir kapsayıcı oluşturulur. Kapsayıcı zaten varsa, bu var olan kapsayıcı kullanılır.

5. Açılan listeden, blok blob, sayfa blob veya dosyalardan **Depolama hizmetini** seçin. Seçilen hizmetin türü, verilerin Azure'da hangi biçimde tutulmasını istediğinize bağlıdır. Örneğin, bu durumda, verilerin Azure'da blok blobolarak yer bilmesini isteriz, bu nedenle **Blob'u engelle'yi**seçeriz. **Sayfa Blob'u**seçerken, verilerinizin 512 bayt hizalandığından emin olmalısınız. Her zaman 512 bayt hizalanmış VHDD'ler veya VHDX için **Sayfa blob'u** kullanın.

   > [!IMPORTANT]
   > Kullandığınız Azure Depolama hesabının üzerinde bir Azure Yığını Kenarı veya Veri Kutusu Ağ Geçidi aygıtıyla kullanıyorsanız, üzerinde geçici çözümegeçirim ilkeleri olmadığından emin olun. Daha fazla bilgi [için, blob depolama için değişmezlik ilkelerini ayarla ve yönetin.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage)

6. Bu adım SMB paylaşımı mı yoksa NFS paylaşımı mı oluşturduğunuza bağlıdır.
    - **Bir Kobİ payı oluşturuyorsanız** - **Tüm ayrıcalık yerel kullanıcı** alanında, yeni **oluştur** veya **varolan ı kullan'dan**birini seçin. Yeni bir yerel kullanıcı oluşturuyorsanız, **kullanıcı adı,** **parolayı**girin ve ardından parolayı onaylayın. Bu, yerel kullanıcıya izinleri atar. Burada izinleri atadıktan sonra, Dosya Gezgini'ni kullanarak bu izinlerde değişiklik yapabilirsiniz.

        ![SMB paylaşımı ekleme](media/data-box-edge-manage-shares/add-smb-share.png)

        Bu paylaşımın verileri için Yalnızca okuma işlemlerine izin ver'i işaretlerseniz, salt okuma kullanıcıları belirtebilirsiniz.
    - **Bir NFS payı oluşturuyorsanız** - Paylaşıma erişebilen **izin verilen istemcilerin IP adreslerini** sağlamanız gerekir.

        ![NFS paylaşımı ekleme](media/data-box-edge-manage-shares/add-nfs-share.png)

7. Edge bilgi işlem modüllerinden paylaşımlara kolayca erişmek için yerel montaj noktasını kullanın. Paylaşım oluşturulduktan sonra otomatik olarak monte edilebilmek için **Edge bilgi işlemle paylaş'ı kullanın'ı** seçin. Bu seçenek seçildiğinde, Edge modülü yerel montaj noktasıyla birlikte bilgi işlem de kullanabilir.

8. Paylaşımı oluşturmak için **Oluştur**'a tıklayın. Paylaşım oluşturma işleminin devam ettiği size bildirilir. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, **Paylaşımlar** dikey penceresi yeni paylaşımı yansıtacak şekilde güncelleştirilir.

## <a name="add-a-local-share"></a>Yerel paylaşım ekleme

1. Azure portalında, Veri Kutusu Kenarı kaynağınıza gidin ve ardından **Paylaşımlar > Ağ Geçidi'ne**gidin. Komut çubuğunda **+ Paylaşım ekle'yi** seçin.

    ![Paylaş ekle'yi seçin](media/data-box-edge-manage-shares/add-local-share-1.png)

2. **Paylaşım Ekle**'de, paylaşım ayarlarını belirtin. Paylaşımınız için benzersiz bir ad sağlayın.
    
    Paylaşım adları yalnızca rakam, küçük harf ve kısa çizgiler içerebilir. Paylaşım adı 3 ile 63 karakter arası uzunlukta olmalı ve bir harf veya rakamla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır.

3. Paylaşım için **Tür** seçin. Tür **SMB** veya **NFS**olabilir, SMB varsayılan olarak. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır. SMB paylaşımları mı yoksa NFS paylaşımları mı seçtiğinize bağlı olarak, gösterilen seçenekler biraz farklı olur.

4. Edge bilgi işlem modüllerinden paylaşımlara kolayca erişmek için yerel montaj noktasını kullanın. Edge modülünün yerel montaj noktasıyla bilgi işlem noktasını kullanabilmesi için **Edge bilgi işlemle paylaşımı** nı kullan'ı seçin.

5. **Kenar yerel paylaşımları olarak Yapılandır'ı**seçin. Yerel paylaşımdaki veriler aygıtta yerel olarak kalır. Bu verileri yerel olarak işleyebilirsiniz.

6. Tüm **ayrıcalık yerel kullanıcı** alanında, **yeni oluştur** veya **varolan 'ı kullan'dan**birini seçin.

7. **Oluştur'u**seçin. 

    ![Yerel paylaşım oluşturma](media/data-box-edge-manage-shares/add-local-share-2.png)

    Paylaşım oluşturmanın devam ettiğine dair bir bildirim görürsünüz. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, **Paylaşımlar** dikey penceresi yeni paylaşımı yansıtacak şekilde güncelleştirilir.

    ![Güncellemeleri görüntüle Hisse bıçağı](media/data-box-edge-manage-shares/add-local-share-3.png)
    
    Bu paylaşım için Edge bilgi işlem modülleri için yerel montaj noktasını görüntülemek için paylaşımı seçin.

    ![Yerel paylaşım ayrıntılarını görüntüleme](media/data-box-edge-manage-shares/add-local-share-4.png)

## <a name="mount-a-share"></a>Bir paylaşım ala

Veri Kutusu Kenarı aygıtınızda hesaplamayı yapılandırmadan önce bir paylaşım oluşturduysanız, paylaşımı yüklemeniz gerekir. Pay almak için aşağıdaki adımları atın.


1. Azure portalında, Veri Kutusu Kenarı kaynağınıza gidin ve ardından **Paylaşımlar > Ağ Geçidi'ne**gidin. Paylaşımlar listesinden, monte etmek istediğiniz paylaşımı seçin. **Bilgi işlem için kullanılan** sütun, seçili paylaşım için Devre Dışı **bırakılmış** olarak durum gösterir.

    ![Paylaşımı seçme](media/data-box-edge-manage-shares/select-share-mount.png)

2. **Mount'u**seçin.

    ![Montaj'ı seçin](media/data-box-edge-manage-shares/select-mount.png)

3. Onay istendiğinde **Evet'i**seçin. Bu pay monte edecek.

    ![Montajı onaylayın](media/data-box-edge-manage-shares/confirm-mount.png)

4. Hisse monte olduktan sonra, hisse listesine gidin. Bilgi işlem için **Kullanılan** sütunun, paylaşım durumunu **Etkin**olarak gösterdiğini görürsünüz.

    ![Montajlı paylaş](media/data-box-edge-manage-shares/share-mounted.png)

5. Paylaşım için yerel montaj noktasını görüntülemek için paylaşımı yeniden seçin. Kenar bilgi işlem modülü, paylaşım için bu yerel montaj noktasını kullanır.

    ![Paylaşım için yerel montaj noktası](media/data-box-edge-manage-shares/share-mountpoint.png)

## <a name="unmount-a-share"></a>Bir paylaşımı sökme

Bir paylaşımın montajını sökmek için Azure portalında aşağıdaki adımları yapın.

1. Azure portalında, Veri Kutusu Kenarı kaynağınıza gidin ve ardından **Paylaşımlar > Ağ Geçidi'ne**gidin.

    ![Paylaşımı seçme](media/data-box-edge-manage-shares/select-share-unmount.png)

2. Paylaşımlar listesinden, sökmek istediğiniz paylaşımı seçin. Sökmeyaptığınız paylaşımın herhangi bir modül tarafından kullanılmadığından emin olmak istersiniz. Paylaşım bir modül tarafından kullanılıyorsa, ilgili modülle ilgili sorunları görürsünüz. **Unmount'u**seçin.

    ![Dağı'nı seçin](media/data-box-edge-manage-shares/select-unmount.png)

3. Onay istendiğinde **Evet'i**seçin. Bu, paylaşımın üstesinden gelecektir.

    ![Unmount'u onaylayın](media/data-box-edge-manage-shares/confirm-unmount.png)

4. Hisse ler söküldükten sonra hisse listesine gidin. Bilgi işlem sütunu **için kullanılanların** paylaşım durumunu **Devre Dışı olarak**gösterdiğini görürsünüz.

    ![Montajsız paylaş](media/data-box-edge-manage-shares/share-unmounted.png)

## <a name="delete-a-share"></a>Paylaşımı silme

Paylaşımı silmek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Paylaşım listesinde silmek istediğiniz paylaşımı seçin ve üzerine tıklayın.

    ![Paylaşımı seçme](media/data-box-edge-manage-shares/delete-share-1.png)

2. **Sil'i**tıklatın.

    ![Sil'e tıklayın](media/data-box-edge-manage-shares/delete-share-2.png)

3. Onayınız istendiğinde **Evet**’e tıklayın.

    ![Silmeyi onayla](media/data-box-edge-manage-shares/delete-share-3.png)

Silme işlemini yansıtacak paylaşım güncelleştirmeleri listesi.


## <a name="refresh-shares"></a>Paylaşımları yenileme

Yenileme özelliği, bir paylaşımın içeriğini yenilemenize olanak tanır. Bir paylaşımı yenilediğinizde bloblar ve dosyalar dahil olmak üzere son yenileme işleminden sonra buluta eklenmiş olan tüm Azure nesnelerini bulmak için bir arama başlatılır. Bu ek dosyalar daha sonra aygıttaki paylaşımın içeriğini yenilemek için karşıdan yüklenir.

> [!IMPORTANT]
> - Yerel hisseleri yenileyemezsiniz.
> - İzinler ve erişim denetim listeleri (ALA)'lar bir yenileme işlemi boyunca korunmaz. 

Paylaşımı yenilemek için Azure portalda aşağıdaki adımları gerçekleştirin.

1.   Azure portalda **Paylaşımlar** sayfasına gidin. Yenilemek istediğiniz paylaşımı seçin ve üzerine tıklayın.

    ![Paylaşımı seçme](media/data-box-edge-manage-shares/refresh-share-1.png)

2.   **Yenile**'ye tıklayın. 

    ![Yenile'ye tıklayın](media/data-box-edge-manage-shares/refresh-share-2.png)
 
3.   Onayınız istendiğinde **Evet**’e tıklayın. Şirket içi paylaşımın içeriğinin yenilenmesi için bir iş başlatılır.

    ![Yenilemeyi onaylama](media/data-box-edge-manage-shares/refresh-share-3.png)
 
4.   Yenileme işlemi devam ederken bağlam menüsündeki Yenile seçeneği gri renkte gösterilir. Yenileme işinin durumunu görüntülemek için iş bildirimine tıklayın.

5.   Yenileme süresi Azure kapsayıcısındaki ve cihazdaki dosya sayısına göre değişir. Yenileme işlemi başarıyla tamamlandıktan sonra paylaşım zaman damgası güncelleştirilir. Yenilemede kısmi hatalar olsa da işlem başarılı kabul edilir ve zaman damgası güncelleştirilir. Yenileme hatası günlükleri de güncelleştirilir.

    ![Güncelleştirilmiş zaman damgası](media/data-box-edge-manage-shares/refresh-share-4.png)
 
Hata varsa bir uyarı görüntülenir. Uyarıda sorunun nedeni ve düzeltme adımları yer alır. Uyarıda ayrıca güncelleştirme veya silme işleminin başarısız olduğu dosyalar da dahil olmak üzere hatanın ayrıntılı bir özetinin yer aldığı bir dosyaya bağlantı da verilir.


## <a name="sync-storage-keys"></a>Depolama anahtarlarını eşitleme

Depolama anahtarlarınız değiştirildiyse eşitlemeniz gerekir. Eşitleme, cihazın depolama hesabınızın en son anahtarlarını almasına yardımcı olur.

Depolama erişim anahtarınızı eşitlemek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Kaynağınızın **Genel bakış** sayfasına gidin. Paylaşım listesinden eşitlemek istediğiniz depolama hesabıyla ilişkilendirilmiş olan bir paylaşımı seçin ve üzerine tıklayın.

    ![İlgili depolama hesabıyla paylaş'ı seçme](media/data-box-edge-manage-shares/sync-storage-key-1.png)

2. **Depolama anahtarını eşitle**'ye tıklayın. Onayınız istendiğinde **Evet**’e tıklayın.

     ![Depolama anahtarını senkronize edin](media/data-box-edge-manage-shares/sync-storage-key-2.png)

3. Eşitleme tamamlandıktan sonra iletişim kutusunu kapatın.

>[!NOTE]
> Bu işlemi her depolama hesabı için yalnızca bir kez yapmanız gerekir. Tüm paylaşımlar aynı depolama hesabına aitse bu eylemi tüm paylaşımlar için tekrarlamanız gerekmez.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla kullanıcıları yönetme](data-box-edge-manage-users.md) hakkında bilgi edinin.
