---
title: Azure Data Box Gateway ile verileri aktarma | Microsoft Docs
description: Data Box Gateway cihazında paylaşımları eklemeyi ve bunlara bağlanmayı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 03/08/2019
ms.author: alkohli
ms.openlocfilehash: 32466cc0a1ab9b86fc2fb8eb791c232ae13f1c01
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79213566"
---
# <a name="tutorial-transfer-data-with-azure-data-box-gateway"></a>Öğretici: Azure Veri Kutusu Ağ Geçidi ile veri aktarımı


## <a name="introduction"></a>Giriş

Bu makalede, Veri Kutusu Ağ Geçidinizdeki paylaşımlara nasıl ekleyeceğiniz ve bunlara bağlanılabilmek açıklanmaktadır. Paylaşımları ekledikten sonra, Veri Kutusu Ağ Geçidi aygıtı verileri Azure'a aktarabilir.

Bu yordamın tamamlanması 10 dakika kadar sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Paylaşım ekleme
> * Paylaşıma bağlanma

## <a name="prerequisites"></a>Ön koşullar

Data Box Gateway cihazınıza paylaşım eklemeden önce aşağıdakilerden emin olun:

- Bir sanal aygıt saysanız ve [hyper-v'deki Veri Kutusu Ağ Geçidi'nde](data-box-gateway-deploy-provision-hyperv.md) ayrıntılı olarak belirtildiği gibi ona bağlandınız veya [VMware'de Bir Veri Kutusu Ağ Geçidi'ni Sağlama.'](data-box-gateway-deploy-provision-vmware.md)

- Connect'te açıklanan sanal aygıtı [etkinleştirdiniz ve Azure Veri Kutusu Ağ Geçidinizi etkinleştirdin.](data-box-gateway-deploy-connect-setup-activate.md)

- Cihazınız paylaşım oluşturmak ve veri aktarmak için hazır durumda.

## <a name="add-a-share"></a>Paylaşım ekleme

Bir paylaşım oluşturmak için aşağıdaki yordamı yapın:

1. Azure [portalında,](https://portal.azure.com/)Veri Kutusu Ağ Geçidi kaynağınızı seçin ve ardından **Genel Bakış'a**gidin. Cihazınız çevrimiçi olmalıdır. Aygıt komut çubuğunda **+ Paylaşım ekle'yi** seçin.
   
   ![Paylaşım ekleme](./media/data-box-gateway-deploy-add-shares/click-add-share.png)

4. **Share**Ekle'de, aşağıdaki yordamı yapın:

    1. Paylaşımınız için benzersiz bir ad sağlayın. Paylaşım adlarında yalnızca küçük harfler, sayılar ve tireler olabilir. Paylaşım adı 3 ile 63 karakter uzunluğunda olmalı ve bir harf veya sayı ile başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır.
    
    2. Paylaşım için **Tür** seçin. Tür SMB veya NFS olabilir; varsayılan tür SMB'dir. SMB Windows istemcilerinin standardıdır ve NFS de Linux istemcilerinde kullanılır. SMB paylaşımları mı yoksa NFS paylaşımları mı seçtiğinize bağlı olarak, gösterilen seçenekler biraz farklı olur.

    3. Paylaşımın bulunacağı bir depolama hesabı sağlayın. Bir kapsayıcı zaten yoksa, depolama hesabında yeni oluşturulan paylaşım adı ile oluşturulur. Kapsayıcı zaten varsa, bu kapsayıcı kullanılır.
       > [!IMPORTANT]
       > Kullandığınız Azure Depolama hesabının üzerinde bir Azure Yığını Kenarı veya Veri Kutusu Ağ Geçidi aygıtıyla kullanıyorsanız, üzerinde geçici çözümegeçirim ilkeleri olmadığından emin olun. Daha fazla bilgi [için, blob depolama için değişmezlik ilkelerini ayarla ve yönetin.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage)
    
    4. Blok blobundan, sayfa blobundan veya dosyadan **Depolama hizmeti**'ni seçin. Seçilen hizmetin türü, verilerin Azure'da hangi biçimde tutulmasını istediğinize bağlıdır. Örneğin, buradaki örnekte biz verilerin Azure'da blob blokları olarak tutulmasını istediğimiz için Blok Blobunu seçtik. Sayfa Blobunu seçerseniz, verilerinizi 512 bayt hizalı olduğundan emin olmalısınız. Örneğin VHDX her zaman 512 bayt hizalıdır.
   
    5. Bu adım SMB paylaşımı mı yoksa NFS paylaşımı mı oluşturduğunuza bağlıdır.
     
    - **SMB payı** - **Tüm ayrıcalık yerel kullanıcı**altında, yeni **oluştur** veya **varolan kullan'ı**seçin. Yeni bir yerel kullanıcı oluşturursanız, bir **kullanıcı adı** ve **parola**girin ve ardından **parolayı onaylayın.** Bu eylem, izinleri yerel kullanıcıya atar. Hisse düzeyi izinlerinin değiştirilmesi şu anda desteklenmez.
    
        ![SMB paylaşımı ekleme](./media/data-box-gateway-deploy-add-shares/add-share-smb-1.png)
        
        Bu paylaşım verileri için yalnızca okuma işlemleri onay kutusunu **seçin,** salt okunur kullanıcıları belirtebilirsiniz.
        
    - **NFS payı** - Paylaşıma erişebilen izin verilen istemcilerin IP adreslerini girin.

        ![NFS paylaşımı ekleme](./media/data-box-gateway-deploy-add-shares/add-share-nfs-1.png)
   
9. Paylaşımı oluşturmak için **Oluştur'u** seçin.
    
    Paylaşım oluşturma nın devam ettiği size bildirilir. Paylaşım belirtilen ayarlarla oluşturulduktan sonra, Yeni paylaşımı yansıtacak şekilde **Hisse** döşemesi güncellenir.
    
    ![Güncelleştirilmiş Hisse döşemesi](./media/data-box-gateway-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Paylaşıma bağlanma

Artık son adımda oluşturduğunuz paylaşımlardan birine veya daha fazlasına bağlanabilirsiniz. SMB'niz veya NFS payınız olup olmadığına bağlı olarak adımlar değişebilir.

### <a name="connect-to-an-smb-share"></a>SMB paylaşımına bağlanma

Veri Kutusu Ağ Geçidinize bağlı Windows Server istemcinizde, komutları girerek bir Kobİ paylaşımına bağlanın:


1. Komut penceresinde şunları yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    İstendiğinde paylaşımın parolasını girin. Burada, bu komutun örnek çıkışı gösterilir.

    ```powershell
    Microsoft Windows [Version 18.8.16299.192) 
    (c) 2017 microsoft Corporation. All rights reserved . 
    
    C: \Users\GatewayUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60'  
    The command completed successfully. 
    
    C: \Users\GatewayUser>
    ```   


2. Klavyenizde Windows + R'yi seçin. 
3. **Çalıştır** penceresinde, belirtin `\\<device IP address>` ve sonra **Tamam'ı**seçin. Dosya Gezgini açılır. Artık oluşturduğunuz paylaşımları klasör olarak görüntüleyebilmelisiniz. Dosya Gezgini'nde, içeriği görüntülemek için bir paylaşıma (klasör) çift tıklayın.
 
    ![SMB paylaşımına bağlanma](./media/data-box-gateway-deploy-add-shares/connect-to-share2.png)-->

    Bu paylaşımlara oluşturulduğu anda veriler yazılır ve cihaz verileri buluta gönderir.

### <a name="connect-to-an-nfs-share"></a>NFS paylaşımına bağlanma

Veri Kutusu Edge cihazınıza bağlı Linux istemcinizde aşağıdaki yordamı yapın:

1. İstemcinin NFSv4 istemcisinin yüklü olduğundan emin olun. NFS istemcisini yüklemek için aşağıdaki komutu kullanın:

   `sudo apt-get install nfs-common`

    Daha fazla bilgi için [NFSv4 istemcisini yükleme](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) konusuna gidin.

2. NFS istemcisi yüklendikten sonra, Data Box Gateway cihazınızda oluşturduğunuz NFS paylaşımını bağlamak için aşağıdaki komutu kullanın:

   `sudo mount -t nfs -o sec=sys,resvport <device IP>:/<NFS shares on device> /home/username/<Folder on local Linux computer>`

    Bağlamaları ayarlamadan önce, yerel bilgisayarınızda bağlama noktası işlevi görecek dizinlerin zaten oluşturulmuş olduğundan ve hiçbir dosya veya alt klasör içermediğinden emin olun.

    Aşağıdaki örnekte, Gateway cihazındaki bir paylaşıma NFS yoluyla nasıl bağlanılacağı gösterilir. Sanal cihaz IP'si `10.10.10.60`dır, `mylinuxshare2` ubuntuVM'ye bağlanmıştır ve bağlama noktası `/home/databoxubuntuhost/gateway`dir.

    `sudo mount -t nfs -o sec=sys,resvport 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/gateway`

> [!NOTE] 
> Aşağıdaki uyarılar bu sürüm için geçerlidir:
> - Paylaşımlarda bir dosya oluşturulduktan sonra, dosyanın yeniden adlandırılması desteklenmez.
> - Paylaşımdan dosya silindiğinde, depolama hesabındaki girdi silinmez.
> - Verileri `rsync` kopyalamak için `rsync -a` kullanıyorsanız, seçenek desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Gateway konularını öğrendiniz:

> [!div class="checklist"]
> * Paylaşım ekleme
> * Paylaşıma bağlanma


Data Box Gateway'inizi yönetmeyi öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Data Box Gateway'i yönetmek için yerel web kullanıcı arabirimini kullanma](https://aka.ms/dbg-docs)


