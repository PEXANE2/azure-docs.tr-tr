---
title: Azure Veri Kutusu Kenarı ile paylaşımlara veri aktarma eğitimi | Microsoft Dokümanlar
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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79212955"
---
# <a name="tutorial-transfer-data-with-azure-data-box-edge"></a>Öğretici: Azure Veri Kutusu Kenarı ile veri aktarımı

Bu öğretici, Veri Kutusu Kenarı aygıtınızdaki paylaşımlara nasıl ekleyeceğiniz ve bunlara nasıl bağlanılabildiğini açıklar. Paylaşımları ekledikten sonra, Data Box Edge verileri Azure'a aktarabilir.

Bu yordamın tamamlanması 10 dakika kadar sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma

 
## <a name="prerequisites"></a>Ön koşullar

Veri Kutusu Edge'e paylaşım eklemeden önce şunları yapın:

- Azure [Veri Kutusu Kenarını Yükle'de](data-box-edge-deploy-install.md)açıklandığı gibi fiziksel aygıtınızı yükledin.

- Fiziksel aygıtı [Connect,set ve etkinleştirazure Veri Kutusu Kenarı'nda](data-box-edge-deploy-connect-setup-activate.md)açıklandığı şekilde etkinleştirdin.


## <a name="add-a-share"></a>Paylaşım ekleme

Bir paylaşım oluşturmak için aşağıdaki yordamı yapın:

1. Azure [portalında,](https://portal.azure.com/)Veri Kutusu Kenarı kaynağınızı seçin ve ardından **Genel Bakış'a**gidin. Cihazınız çevrimiçi olmalıdır.

   ![Cihaz çevrimiçi](./media/data-box-edge-deploy-add-shares/device-online-1.png)

2. Aygıt komut çubuğunda **+ Paylaşım ekle'yi** seçin.

   ![Paylaşım ekleme](./media/data-box-edge-deploy-add-shares/select-add-share-1.png)

3. Paylaşım **Ekle** bölmesinde aşağıdaki yordamı yapın:

    a. **Ad** kutusunda, paylaşımınız için benzersiz bir ad sağlayın.  
    Paylaşım adının yalnızca küçük harfleri, rakamları ve tireleri olabilir. 3 ila 63 karakter arasında olmalı ve bir harf veya bir sayı ile başlamalıdır. Tireler önce ve bir harf veya bir sayı takip edilmelidir.
    
    b. Paylaşım için **Tür** seçin.  
    Tür **SMB** veya **NFS**olabilir, SMB varsayılan olarak. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır.  
    SMB veya NFS hisselerini seçip seçmediğinize bağlı olarak, diğer seçenekler biraz değişir. 

    c. Paylaşımın bulunacağı bir depolama hesabı sağlayın.

      > [!IMPORTANT]
      > Kullandığınız Azure Depolama hesabının üzerinde bir Azure Yığını Kenarı veya Veri Kutusu Ağ Geçidi aygıtıyla kullanıyorsanız, üzerinde geçici çözümegeçirim ilkeleri olmadığından emin olun. Daha fazla bilgi [için, blob depolama için değişmezlik ilkelerini ayarla ve yönetin.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage)

    d. Depolama **hizmeti** açılır listesinde Blob, Sayfa **Blob**veya **Dosyaları** **Engelle'yi**seçin.  
    Seçtiğiniz hizmet türü, verilerin Azure'da hangi biçimi kullanmasını istediğinize bağlıdır. Bu örnekte, verileri Azure'da blok blobolarak depolamak istediğimizden, **Blob'u engelle'yi**seçin. **Sayfa Blob'u**seçerseniz, verilerinizin 512 bayt hizalanmış olduğundan emin olun. Örneğin VHDX her zaman 512 bayt hizalıdır.

    e. Yeni bir blob kapsayıcısı oluşturun veya açılır listeden varolan bir kapsayıcı kullanın. Bir blob kapsayıcı sıyrık oluşturuyorsanız, bir kapsayıcı adı sağlayın. Bir kapsayıcı zaten yoksa, depolama hesabında yeni oluşturulan paylaşım adı ile oluşturulur.

    f. Bir Kobİ payı veya NFS payı oluşturup oluşturmadığınıza bağlı olarak aşağıdaki adımlardan birini yapın:

    * **SMB payı**: **Tüm ayrıcalık yerel kullanıcı**altında, yeni **oluştur** veya **varolan kullan'ı**seçin. Yeni bir yerel kullanıcı oluşturursanız, bir kullanıcı adı ve parola girin ve ardından parolayı onaylayın. Bu eylem, yerel kullanıcıya izinatar. Hisse düzeyi izinlerinin değiştirilmesi şu anda desteklenmez.

        Bu paylaşım verileri için **yalnızca okuma işlemlerine izin ver** onay kutusunu seçerseniz, salt okunur kullanıcıları belirtebilirsiniz.

        ![SMB paylaşımı ekleme](./media/data-box-edge-deploy-add-shares/add-share-smb-1.png)

    * **NFS payı**: Paylaşıma erişebilen izin verilen istemcilerin IP adreslerini girin.

        ![NFS paylaşımı ekleme](./media/data-box-edge-deploy-add-shares/add-share-nfs-1.png)

4. Paylaşımı oluşturmak için **Oluştur'u** seçin.
    
    Paylaşım oluşturma nın devam ettiği size bildirilir. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, Yeni paylaşımı yansıtacak şekilde **Hisse** döşemesi güncellenir.
    

## <a name="connect-to-the-share"></a>Paylaşıma bağlanma

Artık son adımda oluşturduğunuz paylaşımlardan birine veya daha fazlasına bağlanabilirsiniz. SMB'niz veya NFS payınız olup olmadığına bağlı olarak adımlar değişebilir.

### <a name="connect-to-an-smb-share"></a>SMB paylaşımına bağlanma

Veri Kutusu Kenar ı aygıtınıza bağlı Windows Server istemcinizde, komutları girerek bir Kobİ paylaşımına bağlanın:


1. Komut penceresinde şunları yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

2. Sizden istendiğinde, paylaşım için parolayı girin.  
   Burada, bu komutun örnek çıkışı gösterilir.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192)
    (c) 2017 Microsoft Corporation. All rights reserved.
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60':
    The command completed successfully.
    
    C: \Users\DataBoxEdgeUser>
    ```   


3. Klavyenizde Windows + R'yi seçin.

4. **Çalıştır** penceresinde , `\\<device IP address>`' **OK**  
   Dosya Gezgini açılır. Artık oluşturduğunuz paylaşımları klasör olarak görüntüleyebilmelisiniz. Dosya Gezgini'nde, içeriği görüntülemek için bir paylaşıma (klasör) çift tıklayın.
 
    ![SMB paylaşımına bağlanma](./media/data-box-edge-deploy-add-shares/connect-to-share2.png)

    Bu paylaşımlara oluşturulduğu anda veriler yazılır ve cihaz verileri buluta gönderir.

### <a name="connect-to-an-nfs-share"></a>NFS paylaşımına bağlanma

Veri Kutusu Edge cihazınıza bağlı Linux istemcinizde aşağıdaki yordamı yapın:

1. İstemcinin NFSv4 istemcisinin yüklü olduğundan emin olun. NFS istemcisini yüklemek için aşağıdaki komutu kullanın:

   `sudo apt-get install nfs-common`

    Daha fazla bilgi için [NFSv4 istemcisini yükleme](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) konusuna gidin.

2. NFS istemcisi yüklendikten sonra, aşağıdaki komutu kullanarak Veri Kutusu Kenarı aygıtınızda oluşturduğunuz NFS payını monte edin:

   `sudo mount -t nfs -o sec=sys,resvport <device IP>:/<NFS shares on device> /home/username/<Folder on local Linux computer>`

    > [!IMPORTANT]
    > Hisse `sync` montajı yaparken opsiyon kullanımı, büyük dosyaların aktarım oranlarını artırır.
    > Paylaşıma binmeden önce, yerel bilgisayarınızda montaj noktaları olarak hareket edecek dizinlerin zaten oluşturulduğundan emin olun. Bu dizinler herhangi bir dosya veya alt klasör içermemelidir.

    Aşağıdaki örnekte, Data Box Edge cihazındaki bir paylaşıma NFS yoluyla nasıl bağlanılacağı gösterilir. Cihaz IP adresi: `10.10.10.60`. `mylinuxshare2` paylaşımı ubuntuVM öğesine bağlanmış. Paylaşımı bağlama noktası: `/home/databoxubuntuhost/edge`.

    `sudo mount -t nfs -o sec=sys,resvport 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> Aşağıdaki uyarılar bu sürüm için geçerlidir:
> - Paylaşımda bir dosya oluşturulduktan sonra, dosyanın yeniden adlandırılması desteklenmez. 
> - Bir dosyayı bir paylaşımdan silmek, depolama hesabındaki girişi silmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, aşağıdaki Data Box Edge konuları hakkında bilgi edinilmişsinizdir:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma

Data Box Edge'i kullanarak verilerinizi nasıl dönüştürerinizi öğrenmek için bir sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Data Box Edge ile veri dönüştürme](./data-box-edge-deploy-configure-compute.md)


