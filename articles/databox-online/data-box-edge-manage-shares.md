---
title: Azure Data Box Edge Share Management | Microsoft Docs
description: Azure Data Box Edge paylaşımlarını yönetmek için Azure portal nasıl kullanılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: b25409c63806e203bd841b0373543b7cc2b96d9d
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79212933"
---
# <a name="use-the-azure-portal-to-manage-shares-on-your-azure-data-box-edge"></a>Azure Data Box Edge paylaşımlarını yönetmek için Azure portal kullanın

Bu makalede Azure Data Box Edge paylaşımların nasıl yönetileceği açıklanır. Azure Data Box Edge Azure portal veya yerel Web Kullanıcı arabirimi aracılığıyla yönetebilirsiniz. Paylaşımlar ile ilişkili depolama hesabı için, paylaşımları eklemek, silmek, yenilemek veya eşitleme depolama anahtarını kullanmak için Azure portal kullanın.

## <a name="about-shares"></a>Paylaşımlar hakkında

Azure 'a veri aktarmak için Azure Data Box Edge paylaşımlar oluşturmanız gerekir. Data Box Edge cihaza eklediğiniz paylaşımlar yerel paylaşımlar veya buluta veri ileten paylaşımlar olabilir.

 - **Yerel paylaşımlar**: verilerin cihazda yerel olarak işlenmesini istiyorsanız bu paylaşımları kullanın.
 - **Paylaşımlar**: cihaz verilerinin buluttaki depolama hesabınıza otomatik olarak iletilmesini istediğinizde bu paylaşımları kullanın. **Yenileme** ve **eşitleme depolama anahtarları** gibi tüm bulut işlevleri paylaşımlar için geçerlidir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşımı silme
> * Paylaşımları yenileme
> * Depolama anahtarını eşitleme


## <a name="add-a-share"></a>Paylaşım ekleme

Paylaşım oluşturmak için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Azure portal, Data Box Edge kaynağınız ' ne gidin ve **ağ geçidi > paylaşımları**' na gidin. Komut çubuğunda **+ paylaşma Ekle** ' yi seçin.

    ![Paylaşma Ekle seçeneğini belirleyin](media/data-box-edge-manage-shares/add-share-1.png)

2. **Paylaşım Ekle**'de, paylaşım ayarlarını belirtin. Paylaşımınız için benzersiz bir ad sağlayın.
    
    Paylaşım adları yalnızca rakam, küçük harf ve kısa çizgiler içerebilir. Paylaşım adı 3 ile 63 karakter arası uzunlukta olmalı ve bir harf veya rakamla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır.

3. Paylaşım için **Tür** seçin. Tür **SMB** veya **NFS** olabilir; varsayılan tür SMB'dir. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır. SMB paylaşımları mı yoksa NFS paylaşımları mı seçtiğinize bağlı olarak, gösterilen seçenekler biraz farklı olur.

4. Paylaşımın duracağı **Depolama hesabını** sağlamanız gerekir. Henüz kapsayıcı yoksa, depolama hesabında paylaşım adıyla bir kapsayıcı oluşturulur. Kapsayıcı zaten varsa, bu var olan kapsayıcı kullanılır.

5. Açılan listeden, Blok Blobu, Sayfa Blobu veya dosyalardan **Depolama hizmetini** seçin. Seçilen hizmetin türü, verilerin Azure'da hangi biçimde tutulmasını istediğinize bağlıdır. Örneğin, bu örnekte, verilerin Azure 'da blok Blobları olarak bulunmasını istiyoruz, bu nedenle **Blok Blobu**seçiyoruz. **Sayfa Blobu**seçilirse, verilerinizin 512 bayt hizalı olduğundan emin olmanız gerekir. Her zaman 512 bayt hizalı VHD veya VHDX için **Sayfa Blobu** kullanın.

   > [!IMPORTANT]
   > Kullandığınız Azure depolama hesabının, bir Azure Stack Edge veya Data Box Gateway cihazından yararlanarak, bu sunucuda ayarlanmış bir şekilde kullanılabilirlik ilkesi olmadığından emin olun. Daha fazla bilgi için bkz. [BLOB depolama için dengesde kullanılabilirlik Ilkelerini ayarlama ve yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage).

6. Bu adım SMB paylaşımı mı yoksa NFS paylaşımı mı oluşturduğunuza bağlıdır.
    - **SMB paylaşımı oluşturuyorsanız** - **Tüm ayrıcalıklara sahip yerel kullanıcı** alanında **Yeni oluştur**'u veya **Var olanı kullan**'ı seçin. Yeni bir yerel kullanıcı oluşturuluyorsa, **kullanıcı adı** ve **parola** sağlayın, sonra da parolayı onaylayın. Bu, yerel kullanıcıya izinleri atar. Burada izinleri atadıktan sonra, Dosya Gezgini'ni kullanarak bu izinlerde değişiklik yapabilirsiniz.

        ![SMB paylaşımı ekleme](media/data-box-edge-manage-shares/add-smb-share.png)

        Bu paylaşımın verileri için Yalnızca okuma işlemlerine izin ver'i işaretlerseniz, salt okuma kullanıcıları belirtebilirsiniz.
    - **NFS paylaşımı oluşturuluyorsa** - Paylaşıma erişmesine **izin verilen istemcilerin IP adreslerini** sağlamanız gerekir.

        ![NFS paylaşımı ekleme](media/data-box-edge-manage-shares/add-nfs-share.png)

7. Sınır işlem modüllerindeki paylaşımlara kolayca erişmek için yerel bağlama noktasını kullanın. Paylaşımın oluşturulduktan sonra otomatik olarak bağlanması için **kenar COMPUTE ile paylaşma kullan** seçeneğini belirleyin. Bu seçenek belirlendiğinde, Edge modülü aynı zamanda yerel bağlama noktasıyla işlem kullanabilir.

8. Paylaşımı oluşturmak için **Oluştur**'a tıklayın. Paylaşım oluşturma işleminin devam ettiği size bildirilir. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, **Paylaşımlar** dikey penceresi yeni paylaşımı yansıtacak şekilde güncelleştirilir.

## <a name="add-a-local-share"></a>Yerel bir paylaşma ekleyin

1. Azure portal, Data Box Edge kaynağınız ' ne gidin ve **ağ geçidi > paylaşımları**' na gidin. Komut çubuğunda **+ paylaşma Ekle** ' yi seçin.

    ![Paylaşma Ekle seçeneğini belirleyin](media/data-box-edge-manage-shares/add-local-share-1.png)

2. **Paylaşım Ekle**'de, paylaşım ayarlarını belirtin. Paylaşımınız için benzersiz bir ad sağlayın.
    
    Paylaşım adları yalnızca rakam, küçük harf ve kısa çizgiler içerebilir. Paylaşım adı 3 ile 63 karakter arası uzunlukta olmalı ve bir harf veya rakamla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır.

3. Paylaşım için **Tür** seçin. Tür **SMB** veya **NFS** olabilir; varsayılan tür SMB'dir. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır. SMB paylaşımları mı yoksa NFS paylaşımları mı seçtiğinize bağlı olarak, gösterilen seçenekler biraz farklı olur.

4. Sınır işlem modüllerindeki paylaşımlara kolayca erişmek için yerel bağlama noktasını kullanın. Sınır modülünün yerel bağlama noktasıyla işlem kullanabilmesi için **Edge COMPUTE ile paylaşma kullan** ' ı seçin.

5. **Sınır yerel paylaşımları olarak Yapılandır**' ı seçin. Yerel paylaşımlardaki veriler cihazda yerel olarak kalır. Bu verileri yerel olarak işleyebilirsiniz.

6. **Tüm ayrıcalık Yerel Kullanıcı** alanında **Yeni oluştur** ' u seçin veya **var olanı kullanın**.

7. **Oluştur**’u seçin. 

    ![Yerel paylaşma oluştur](media/data-box-edge-manage-shares/add-local-share-2.png)

    Paylaşma oluşturmanın devam ettiğini belirten bir bildirim görürsünüz. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, **Paylaşımlar** dikey penceresi yeni paylaşımı yansıtacak şekilde güncelleştirilir.

    ![Güncelleştirme paylaşımları dikey penceresini görüntüle](media/data-box-edge-manage-shares/add-local-share-3.png)
    
    Bu paylaşımın uç işlem modülleri için yerel bağlama noktasını görüntülemek üzere paylaşıma seçin.

    ![Yerel paylaşma ayrıntılarını görüntüle](media/data-box-edge-manage-shares/add-local-share-4.png)

## <a name="mount-a-share"></a>Bir paylaşma bağlama

Data Box Edge cihazınızda işlem yapılandırmadan önce bir paylaşma oluşturduysanız, bu paylaşıma bağlamanız gerekir. Bir paylaşma bağlamak için aşağıdaki adımları uygulayın.


1. Azure portal, Data Box Edge kaynağınız ' ne gidin ve **ağ geçidi > paylaşımları**' na gidin. Paylaşımlar listesinden bağlamak istediğiniz paylaşımı seçin. **İşlem Için kullanılan** sütunu, seçili paylaşımın durumunu **devre dışı** olarak gösterir.

    ![Paylaşımı seçme](media/data-box-edge-manage-shares/select-share-mount.png)

2. **Bağla**' yı seçin.

    ![Bağlama seçin](media/data-box-edge-manage-shares/select-mount.png)

3. Onay istendiğinde **Evet**' i seçin. Bu, paylaşımdan bağlantı oluşturacak.

    ![Bağlama Onayla](media/data-box-edge-manage-shares/confirm-mount.png)

4. Paylaşım bağlandıktan sonra paylaşımlar listesine gidin. **İşlem Için kullanılan** sütununda, paylaşımın durumunu **etkin**olarak gösteren bir görürsünüz.

    ![Bağlı paylaşıma](media/data-box-edge-manage-shares/share-mounted.png)

5. Paylaşıma ait yerel bağlama noktasını görüntülemek için yeniden paylaşma ' yı seçin. Edge işlem modülü, bu yerel bağlama noktasını paylaşıma kullanır.

    ![Paylaşımın yerel bağlama noktası](media/data-box-edge-manage-shares/share-mountpoint.png)

## <a name="unmount-a-share"></a>Bir paylaşımın bağlantısını kaldırma

Bir paylaşımın bağlantısını kesmek için Azure portal aşağıdaki adımları uygulayın.

1. Azure portal, Data Box Edge kaynağınız ' ne gidin ve **ağ geçidi > paylaşımları**' na gidin.

    ![Paylaşımı seçme](media/data-box-edge-manage-shares/select-share-unmount.png)

2. Paylaşımlar listesinden, çıkarmak istediğiniz paylaşımı seçin. Çıkardık paylaşımın herhangi bir modül tarafından kullanılmadığından emin olmak istiyorsunuz. Paylaşma bir modül tarafından kullanılıyorsa, ilgili modülle ilgili sorunları görürsünüz. Çıkar ' **ı seçin.**

    ![Çıkarma seçin](media/data-box-edge-manage-shares/select-unmount.png)

3. Onay istendiğinde **Evet**' i seçin. Bu, paylaşımın bağlantısını çıkaracaktır.

    ![Çıkarma bağlantısını onayla](media/data-box-edge-manage-shares/confirm-unmount.png)

4. Paylaşım kaldırıldıktan sonra paylaşımlar listesine gidin. **İşlem sütunu Için kullanılan** ' ın, paylaşma durumunu **devre dışı**olarak gösterdiğini görürsünüz.

    ![Kaldırılan paylaşma](media/data-box-edge-manage-shares/share-unmounted.png)

## <a name="delete-a-share"></a>Paylaşımı silme

Paylaşımı silmek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Paylaşım listesinde silmek istediğiniz paylaşımı seçin ve üzerine tıklayın.

    ![Paylaşımı seçme](media/data-box-edge-manage-shares/delete-share-1.png)

2. **Sil**'e tıklayın.

    ![Sil'e tıklayın](media/data-box-edge-manage-shares/delete-share-2.png)

3. Onayınız istendiğinde **Evet**’e tıklayın.

    ![Silmeyi onayla](media/data-box-edge-manage-shares/delete-share-3.png)

Paylaşımların listesi, silme işlemini yansıtacak şekilde güncelleştirilir.


## <a name="refresh-shares"></a>Paylaşımları yenileme

Yenile özelliği, bir paylaşımın içeriğini yenilekullanmanıza olanak sağlar. Bir paylaşımı yenilediğinizde bloblar ve dosyalar dahil olmak üzere son yenileme işleminden sonra buluta eklenmiş olan tüm Azure nesnelerini bulmak için bir arama başlatılır. Bu ek dosyalar daha sonra cihazdaki paylaşımın içeriğini yenilemek için indirilir.

> [!IMPORTANT]
> - Yerel paylaşımları yenileyemezsiniz.
> - İzinler ve erişim denetim listeleri (ACL 'Ler) bir yenileme işlemi boyunca korunmaz. 

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

    ![İlgili depolama hesabı ile paylaşma seçin](media/data-box-edge-manage-shares/sync-storage-key-1.png)

2. **Depolama anahtarını eşitle**'ye tıklayın. Onayınız istendiğinde **Evet**’e tıklayın.

     ![Eşitleme depolama anahtarını seçin](media/data-box-edge-manage-shares/sync-storage-key-2.png)

3. Eşitleme tamamlandıktan sonra iletişim kutusunu kapatın.

>[!NOTE]
> Bu işlemi her depolama hesabı için yalnızca bir kez yapmanız gerekir. Tüm paylaşımlar aynı depolama hesabına aitse bu eylemi tüm paylaşımlar için tekrarlamanız gerekmez.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla kullanıcıları yönetme](data-box-edge-manage-users.md) hakkında bilgi edinin.
