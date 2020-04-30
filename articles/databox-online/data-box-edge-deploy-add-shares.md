---
title: Azure Data Box Edge ile paylaşımlara veri aktarma öğreticisi | Microsoft Docs
description: Data Box Edge cihazında paylaşımları eklemeyi ve bunlara bağlanmayı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to add and connect to shares on Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 3b1988656e2c15515e121df3ee71e31ce7edd750
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79212955"
---
# <a name="tutorial-transfer-data-with-azure-data-box-edge"></a>Öğretici: Azure Data Box Edge veri aktarma

Bu öğretici, Data Box Edge cihazınızda paylaşımların nasıl ekleneceğini ve bağlanılacağını açıklamaktadır. Paylaşımlar eklendikten sonra Data Box Edge Azure 'a veri aktarabilir.

Bu yordamın tamamlanması 10 dakika kadar sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma

 
## <a name="prerequisites"></a>Ön koşullar

Data Box Edge paylaşımlar eklemeden önce şunları yaptığınızdan emin olun:

- Fiziksel cihazınızı [yükleme Azure Data Box Edge](data-box-edge-deploy-install.md)bölümünde açıklandığı gibi yüklediniz.

- Fiziksel cihazı, [bağlanma, ayarlama ve etkinleştirme Azure Data Box Edge](data-box-edge-deploy-connect-setup-activate.md)açıklandığı şekilde etkinleştirdiniz.


## <a name="add-a-share"></a>Paylaşım ekleme

Bir paylaşma oluşturmak için aşağıdaki yordamı uygulayın:

1. [Azure portal](https://portal.azure.com/)Data Box Edge kaynağınızı seçip **genel bakış**' a gidin. Cihazınızın çevrimiçi olması gerekir.

   ![Cihaz çevrimiçi](./media/data-box-edge-deploy-add-shares/device-online-1.png)

2. Cihaz komut çubuğunda **+ paylaşma Ekle** ' yi seçin.

   ![Paylaşım ekleme](./media/data-box-edge-deploy-add-shares/select-add-share-1.png)

3. **Paylaşma Ekle** bölmesinde aşağıdaki yordamı uygulayın:

    a. **Ad** kutusunda, paylaşımınız için benzersiz bir ad sağlayın.  
    Paylaşma adı yalnızca küçük harf, sayı ve kısa çizgi içerebilir. 3 ila 63 karakter arasında olmalıdır ve bir harf veya rakam ile başlamalıdır. Kısa çizgilerden önce ve ardından bir harf ya da rakam gelmelidir.
    
    b. Paylaşım için **Tür** seçin.  
    Bu tür SMB veya **NFS**, SMB 'nin varsayılan olarak **olması olabilir.** SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır.  
    SMB veya NFS paylaşımlarını seçmenize bağlı olarak, seçeneklerin geri kalanı biraz farklılık gösterir. 

    c. Paylaşımın bulunacağı bir depolama hesabı sağlayın.

      > [!IMPORTANT]
      > Kullandığınız Azure depolama hesabının, bir Azure Stack Edge veya Data Box Gateway cihazından yararlanarak, bu sunucuda ayarlanmış bir şekilde kullanılabilirlik ilkesi olmadığından emin olun. Daha fazla bilgi için bkz. [BLOB depolama için dengesde kullanılabilirlik Ilkelerini ayarlama ve yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage).

    d. **Depolama hizmeti** açılan listesinde, **Blok Blobu**, **Sayfa Blobu**veya **dosyalar**' ı seçin.  
    Seçtiğiniz hizmetin türü, verilerin Azure 'da kullanmasını istediğiniz biçime bağlıdır. Bu örnekte, verileri Azure 'da blok Blobları olarak depolamak istiyoruz, **Blok Blobu**seçtik. **Sayfa Blobu**' nı seçerseniz, verilerinizin 512 bayt hizalı olduğundan emin olun. Örneğin VHDX her zaman 512 bayt hizalıdır.

    e. Yeni bir blob kapsayıcısı oluşturun veya açılan listeden mevcut bir tane kullanın. Blob kapsayıcısı oluşturuyorsanız bir kapsayıcı adı sağlayın. Zaten bir kapsayıcı yoksa, depolama hesabında yeni oluşturulan paylaşma adıyla oluşturulur.

    f. Bir SMB veya NFS paylaşımının oluşturulup oluşturulmayacağını bağlı olarak, aşağıdaki adımlardan birini yapın:

    * **SMB paylaşma**: **tüm ayrıcalık yerel kullanıcısı**' nın altında, **Yeni oluştur** ' u seçin veya **var olanı kullanın**. Yeni bir yerel kullanıcı oluşturursanız, bir Kullanıcı adı ve parola girin ve parolayı onaylayın. Bu eylem, yerel kullanıcıya izinler atar. Paylaşma düzeyi izinlerinin değiştirilmesi Şu anda desteklenmiyor.

        Bu paylaşma verileri için **yalnızca okuma Işlemlerine Izin ver** onay kutusunu seçerseniz, salt okunurdur kullanıcıları belirtebilirsiniz.

        ![SMB paylaşımı ekleme](./media/data-box-edge-deploy-add-shares/add-share-smb-1.png)

    * **NFS paylaşma**: paylaşıma erişebilen izin VERILEN istemcilerin IP adreslerini girin.

        ![NFS paylaşımı ekleme](./media/data-box-edge-deploy-add-shares/add-share-nfs-1.png)

4. Paylaşma oluşturmak için **Oluştur** ' u seçin.
    
    Paylaşma oluşturma işleminin devam ettiğini size bildirilir. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, **Paylaşımlar** kutucuğunu yeni paylaşımı yansıtacak şekilde güncelleştirir.
    

## <a name="connect-to-the-share"></a>Paylaşıma bağlanma

Artık son adımda oluşturduğunuz bir veya daha fazla paylaşıma bağlanabilirsiniz. SMB veya NFS paylaşımınız olmasına bağlı olarak, adımlar farklılık gösterebilir.

### <a name="connect-to-an-smb-share"></a>SMB paylaşımına bağlanma

Data Box Edge cihazınıza bağlı Windows Server istemcisinde şu komutları girerek bir SMB paylaşımıyla bağlantı edin:


1. Bir komut penceresinde, şunu yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

2. İstendiğinde, paylaşımın parolasını girin.  
   Burada, bu komutun örnek çıkışı gösterilir.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192)
    (c) 2017 Microsoft Corporation. All rights reserved.
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60':
    The command completed successfully.
    
    C: \Users\DataBoxEdgeUser>
    ```   


3. Klavyenizde Windows + R ' yi seçin.

4. **Çalıştır** penceresinde, `\\<device IP address>`öğesini belirtin ve ardından **Tamam**' ı seçin.  
   Dosya Gezgini açılır. Artık, klasör olarak oluşturduğunuz paylaşımları görüntüleyebilmelisiniz. Dosya Gezgini 'nde, içeriği görüntülemek için bir paylaşıma (klasör) çift tıklayın.
 
    ![SMB paylaşımına bağlanma](./media/data-box-edge-deploy-add-shares/connect-to-share2.png)

    Bu paylaşımlara oluşturulduğu anda veriler yazılır ve cihaz verileri buluta gönderir.

### <a name="connect-to-an-nfs-share"></a>NFS paylaşımına bağlanma

Data Box Edge cihazınıza bağlı Linux istemciniz üzerinde aşağıdaki yordamı uygulayın:

1. İstemcide NFSv4 istemcisinin yüklü olduğundan emin olun. NFS istemcisini yüklemek için aşağıdaki komutu kullanın:

   `sudo apt-get install nfs-common`

    Daha fazla bilgi için [NFSv4 istemcisini yükleme](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) konusuna gidin.

2. NFS istemcisi yüklendikten sonra, aşağıdaki komutu kullanarak Data Box Edge cihazınızda oluşturduğunuz NFS payını bağlayın:

   `sudo mount -t nfs -o sec=sys,resvport <device IP>:/<NFS shares on device> /home/username/<Folder on local Linux computer>`

    > [!IMPORTANT]
    > `sync` Paylaşım paylaşımları, büyük dosyaların aktarım hızlarını geliştirir.
    > Paylaşma 'yı bağlamadan önce, yerel bilgisayarınızda bağlama noktası olarak görev yapacak dizinlerin zaten oluşturulduğundan emin olun. Bu dizinler herhangi bir dosya veya alt klasör içermemelidir.

    Aşağıdaki örnekte, Data Box Edge cihazındaki bir paylaşıma NFS yoluyla nasıl bağlanılacağı gösterilir. Cihaz IP adresi: `10.10.10.60`. `mylinuxshare2` paylaşımı ubuntuVM öğesine bağlanmış. Paylaşımı bağlama noktası: `/home/databoxubuntuhost/edge`.

    `sudo mount -t nfs -o sec=sys,resvport 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> Bu sürüm için aşağıdaki uyarılar geçerlidir:
> - Paylaşımda bir dosya oluşturulduktan sonra dosyanın yeniden adlandırılması desteklenmez. 
> - Bir paylaşımdan bir dosyanın silinmesi, depolama hesabındaki girişi silmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Edge konular hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma

Data Box Edge kullanarak verilerinizi nasıl dönüştürebileceğinizi öğrenmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Data Box Edge ile veri dönüştürme](./data-box-edge-deploy-configure-compute.md)


