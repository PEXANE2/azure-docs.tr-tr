---
title: "Öğretici: VHD'lerden yönetilen disklere kopyala"
titleSuffix: Azure Data Box
description: Şirket içi VM iş yüklerinden Azure Veri Kutunuza VHD'lerden gelen verileri nasıl kopyalayınızı öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: alkohli
ms.openlocfilehash: 965c768df9138d850c2ac9f88e3797dcc54fa3fc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79501865"
---
# <a name="tutorial-use-data-box-to-import-data-as-managed-disks-in-azure"></a>Öğretici: Azure'da yönetilen diskolarak veri almak için Veri Kutusu'ni kullanma

Bu öğretici, sizi şirket içi VHD'leri Azure'daki yönetilen disklere geçirmek için Azure Veri Kutusu'nun nasıl kullanılacağını açıklar. Şirket içi VM'lerden gelen VHD'ler sayfa blob'ları olarak Veri Kutusu'na kopyalanır ve yönetilen diskler olarak Azure'a yüklenir. Bu yönetilen diskler daha sonra Azure VM'lere eklenebilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Önkoşulları inceleyin
> * Data Box'a bağlanma
> * Data Box'a veri kopyalama

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. [Öğreticiyi tamamladınız: Azure Veri Kutusu'nu ayarlayın.](data-box-deploy-set-up.md)
2. Data Box’ı teslim aldınız ve portaldaki sipariş durumu **Teslim Edildi** oldu.
3. Yüksek hızlı bir ağa bağlısınız. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa, 1 GbE veri bağlantısı kullanın, ancak kopyalama hızları etkilenir.
4. İncelediniz:

    - Azure [nesne boyutu sınırlarında](data-box-limits.md#azure-object-size-limits)desteklenen yönetilen disk boyutları.
    - [Azure yönetilen disklere giriş.](/azure/virtual-machines/windows/managed-disks-overview) 

5. Veri Kutusu'nun verilerinizi Azure Depolama'ya aktardığını doğrulayana kadar kaynak verilerin bir kopyasını korumuştur.

## <a name="connect-to-data-box"></a>Data Box'a bağlanma

Belirtilen kaynak gruplarına bağlı olarak, Veri Kutusu ilişkili her kaynak grubu için bir paylaşım oluşturur. Örneğin, `mydbmdrg1` `mydbmdrg2` sipariş verirken oluşturulduysa, aşağıdaki paylaşımlar oluşturulur:

- `mydbmdrg1_MDisk`
- `mydbmdrg2_MDisk`

Her paylaşımda, depolama hesabınızdaki kapsayıcılara karşılık gelen aşağıdaki üç klasör oluşturulur.

- Premium SSD
- Standart HDD
- Standart SSD

Aşağıdaki tablo, Veri Kutunuzdaki paylaşımların UNC yollarını gösterir.
 
|        Bağlantı protokolü           |             Paya UNC yolu                                               |
|-------------------|--------------------------------------------------------------------------------|
| SMB |`\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<PremiumSSD>\file1.vhd`<br> `\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<StandardHDD>\file2.vhd`<br> `\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<StandardSSD>\file3.vhd` |  
| NFS |`//<DeviceIPAddress>/<ResourceGroup1_MDisk>/<PremiumSSD>/file1.vhd`<br> `//<DeviceIPAddress>/<ResourceGroupName_MDisk>/<StandardHDD>/file2.vhd`<br> `//<DeviceIPAddress>/<ResourceGroupName_MDisk>/<StandardSSD>/file3.vhd` |

Data Box paylaşımlarına bağlanmak için SMB veya NFS'i kullanıp kullanmadığınıza bağlı olarak, bağlanma adımları farklıdır.

> [!NOTE]
> REST üzerinden bağlanma bu özellik için desteklenmez.

### <a name="connect-to-data-box-via-smb"></a>SMB ile Veri Kutusuna Bağlan

Windows Server ana bilgisayarı kullanıyorsanız Data Box'a bağlanmak için aşağıdaki adımları izleyin.

1. İlk adım kimlik doğrulamasından geçmek ve oturum başlatmaktır. **Bağlan ve kopyala**'ya gidin. Kaynak grubunuzla ilişkili paylaşımların erişim kimlik bilgilerini almak için **kimlik bilgilerini al'ı** tıklatın. Azure portalındaki **Aygıt ayrıntılarından** erişim kimlik bilgilerini de alabilirsiniz.

    > [!NOTE]
    > Yönetilen diskler için tüm paylaşımların kimlik bilgileri aynıdır.

    ![Paylaşım kimlik bilgilerini alma 1](media/data-box-deploy-copy-data-from-vhds/get-share-credentials1.png)

2. Access paylaş ve veri iletişim kutusunu kopyalayın, paylaşım için **Kullanıcı Adını** ve **Parola'yı** kopyalayın. **Tamam**'a tıklayın.
    
    ![Paylaşım kimlik bilgilerini alma 1](media/data-box-deploy-copy-data-from-vhds/get-share-credentials2.png)

3. Ana bilgisayardan kaynağınızla ilişkili paylaşımlara (aşağıdaki örnekte*mydbmdrg1)* erişmek için bir komut penceresi açın. Komut istemine şunları yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Bu örnekteki UNC paylaşım yollarınız aşağıdaki gibidir:

    - `\\169.254.250.200\mydbmdrg1_MDisk`
    - `\\169.254.250.200\mydbmdrg2_MDisk`
    
4. İstendiğinde paylaşımın parolasını girin. Aşağıdaki örnekte yukarıdaki komutla paylaşıma bağlanma adımları gösterilmektedir.

    ```
    C:\>net use \\169.254.250.200\mydbmdrgl_MDisk /u:mdisk
    Enter the password for 'mdisk' to connect to '169.254.250.200':
    The command completed successfully.
    C: \>
    ```

4. Windows + R tuşlarına basın. **Çalıştır** penceresinde `\\<device IP address>\<ShareName>` değerini belirtin. Dosya Gezgini'ni açmak için **Tamam**’a tıklayın.
    
    ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-deploy-copy-data-from-vhds/connect-shares-file-explorer1.png)

    Şimdi her paylaşım da aşağıdaki önceden oluşturulmuş klasörleri görmeniz gerekir.
    
    ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-deploy-copy-data-from-vhds/connect-shares-file-explorer2.png)


### <a name="connect-to-data-box-via-nfs"></a>NFS ile Veri Kutusuna Bağlanma

Linux ana bilgisayarı kullanıyorsanız aşağıdaki adımları gerçekleştirerek Data Box'ı NFS istemcilerine izin verecek şekilde yapılandırın.

1. Paylaşıma erişmesine izin verilen istemcilerin IP adreslerini sağlayın. Yerel web arabiriminde **Bağlan ve kopyala** sayfasına gidin. **NFS ayarları** bölümünde **NFS istemci erişimi**'ne tıklayın.

    ![NFS istemci erişimini yapılandırma 1](media/data-box-deploy-copy-data-from-vhds/nfs-client-access1.png)

2. NFS istemcisinin IP adresini girin ve **Ekle**'ye tıklayın. Bu adımı tekrarlayarak birden fazla NFS istemcisi için erişim sağlayabilirsiniz. **Tamam**'a tıklayın.

    ![NFS istemci erişimini yapılandırma 2](media/data-box-deploy-copy-data-from-vhds/nfs-client-access2.png)

2. Linux ana bilgisayarında NFS istemcisinin [desteklenen sürümünün](data-box-system-requirements.md) yüklü olduğundan emin olun. Linux dağıtımınıza uygun sürümü kullanın.

3. NFS istemcisi yüklendikten sonra, Data Box cihazınızdaki NFS paylaşımını bağlamak için aşağıdaki komutu kullanın:

    `sudo mount <Data Box device IP>:/<NFS share on Data Box device> <Path to the folder on local Linux computer>`

    Aşağıdaki örnekte, Data Box paylaşımına NFS yoluyla nasıl bağlanılacağı gösterilir. Data Box cihaz IP'si `169.254.250.200` şeklindedir, `mydbmdrg1_MDisk` ubuntuVM'ye bağlanmıştır ve bağlama noktası `/home/databoxubuntuhost/databox` ile belirtilmiştir.

    `sudo mount -t nfs 169.254.250.200:/mydbmdrg1_MDisk /home/databoxubuntuhost/databox`


## <a name="copy-data-to-data-box"></a>Data Box'a veri kopyalama

Veri sunucusuna bağlandıktan sonra, bir sonraki adım verileri kopyalamaktır. VHD dosyası, evreleme depolama hesabına sayfa blob'u olarak kopyalanır. Sayfa blob sonra yönetilen bir diske dönüştürülür ve bir kaynak grubuna taşınır.

Veri kopyalamaya başlamadan önce aşağıdaki hususları gözden geçirin:

- VHD'leri her zaman önceden oluşturulmuş klasörlerden birine kopyalayın. VHD'leri bu klasörlerin dışında veya oluşturduğunuz bir klasörde kopyalarsanız, VHD'ler sayfa blob'ları olarak Azure Depolama hesabına yüklenir ve yönetilmeyen diskler olarak yüklenir.
- Yönetilen diskler oluşturmak için yalnızca sabit VHD'ler karşıya yüklenebilir. VHDX dosyaları veya dinamik ve farklılaştırıcı VHDD'ler desteklenmez.
- Önceden oluşturulmuş tüm klasörlerde kaynak grubunda belirli bir ada sahip yalnızca bir yönetilen diskiniz olabilir. Bu durum, önceden oluşturulan klasörlere yüklenen VHD'lerin benzersiz adlara sahip olması gerektiği anlamına gelir. Verilen adın bir kaynak grubunda zaten var olan bir yönetilen diskle eşleşmediğinden emin olun.
- [Azure nesne boyutu sınırlarında](data-box-limits.md#azure-object-size-limits)yönetilen disk sınırlarını gözden geçirin.

SMB veya NFS üzerinden bağlanıp bağlanmadığınıza bağlı olarak şunları kullanabilirsiniz:

- [SMB ile veri kopyalama](data-box-deploy-copy-data.md#copy-data-to-data-box)
- [NFS ile veri kopyalama](data-box-deploy-copy-data-via-nfs.md#copy-data-to-data-box)

Kopyalama işlerinin bitmesini bekleyin. Bir sonraki adıma geçmeden önce kopyalama işlerinin hiçbir hata olmadan tamamlanındığından emin olun.

![**Bağlan ve kopyala** sayfasında hata yok](media/data-box-deploy-copy-data-from-vhds/verify-no-errors-connect-and-copy.png)

Kopyalama işlemi sırasında hatalar varsa, günlükleri **Bağlan ve kopyala** sayfasından indirin.

- 512 bayt hizalı olmayan bir dosyayı kopyaladıysanız, dosya evreleme depolama hesabınıza sayfa blob olarak yüklenmez. Günlüklerde bir hata görürsünüz. Dosyayı kaldırın ve 512 bayt hizalanmış bir dosyayı kopyalayın.

- Uzun bir adla bir VHDX kopyaladıysanız (bu dosyalar desteklenmez), günlüklerde bir hata görürsünüz.

    ![**Bağlan ve kopyala** sayfasındaki günlüklerde hata](media/data-box-deploy-copy-data-from-vhds/errors-connect-and-copy.png)

    Bir sonraki adıma geçmeden önce hataları çözümleyin.

Veri bütünlüğünü sağlamak için sağlama toplamı veri kopyalama sırasında satır içinde hesaplanır. Kopyalama tamamlandıktan sonra cihazınızdaki kullanılan alanı ve boş alanı doğrulayın.
    
![Panoda boş ve kullanılan alanı doğrulama](media/data-box-deploy-copy-data-from-vhds/verify-used-space-dashboard.png)

Kopyalama işi bittikten **sonra, gemi hazırlamak**için gidebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Önkoşulları inceleyin
> * Data Box'a bağlanma
> * Data Box'a veri kopyalama


Data Box'ı Microsoft’a geri gönderme hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a gönderme](./data-box-deploy-picked-up.md)

