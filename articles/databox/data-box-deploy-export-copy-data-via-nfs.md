---
title: NFS aracılığıyla Azure Data Box veri kopyalama öğreticisi | Microsoft Docs
description: Azure Data Box NFS aracılığıyla verileri nasıl kopyalayacağınızı öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 10/01/2020
ms.author: alkohli
ms.openlocfilehash: bd8e6d4175c57bd31c3fd83bf6f9669d2b65ffb2
ms.sourcegitcommit: 487a9f5272300d60df2622c3d13e794d54680f90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91660861"
---
# <a name="tutorial-copy-data-from-azure-data-box-via-nfs-preview"></a>Öğretici: NFS aracılığıyla Azure Data Box veri kopyalama (Önizleme)

Bu öğreticide, Data Box yerel Web kullanıcı arabiriminden NFS aracılığıyla şirket içi veri sunucusuna nasıl bağlanabileceği ve verilerin nasıl kopyalanacağı açıklanmaktadır. Data Box veriler Azure Storage hesabınızdan içe aktarılabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Data Box'a bağlanma
> * Data Box’tan veri kopyalama

[!INCLUDE [Data Box feature is in preview](../../includes/data-box-feature-is-preview-info.md)]

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Azure Data Box siparişi verdiniz.
    - İçeri aktarma siparişi için bkz. [Öğretici: Azure Data Box sipariş etme](data-box-deploy-ordered.md) bölümünü tamamladınız.
    - Dışarı aktarma siparişi için bkz. [Öğretici: Azure Data Box sipariş etme](data-box-deploy-export-ordered.md) bölümünü tamamladınız.
2. Data Box’ı teslim aldınız ve portaldaki sipariş durumu **Teslim Edildi** oldu.
3. Data Box’ınızdan verileri kopyalamak istediğiniz bir ana bilgisayarınız var. Ana bilgisayarınız:
   * [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
   * Yüksek hızlı bir ağa bağlı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa, 1 GbE veri bağlantısı kullanın ancak kopyalama hızınızın etkileneceğini göz önünde bulundurun.

## <a name="connect-to-data-box"></a>Data Box'a bağlanma

[!INCLUDE [data-box-shares](../../includes/data-box-shares.md)]

Linux ana bilgisayarı kullanıyorsanız aşağıdaki adımları gerçekleştirerek Data Box'ı NFS istemcilerine izin verecek şekilde yapılandırın. Data Box, tek seferde beş adet NFS istemcisi olarak bağlanabilir.

1. Paylaşıma erişebilen izin verilen istemcilerin IP adreslerini sağlayın:

    1.  Yerel Web Kullanıcı arabiriminde **Bağlan ve Kopyala** sayfasına gidin. **NFS ayarları** bölümünde **NFS istemci erişimi**'ne tıklayın. 

        ![NFS istemci erişimini aç](media/data-box-deploy-export-copy-data/nfs-client-access-1.png)

    1. NFS istemcisi eklemek için istemcinin IP adresini sağlayın ve **Ekle**' ye tıklayın. Data Box, tek seferde beş adet NFS istemcisi olarak bağlanabilir. Bitirdiğinizde, **Tamam**' a tıklayın.

         ![NFS istemcisi ekleme](media/data-box-deploy-export-copy-data/nfs-client-access-2.png)

2. Linux ana bilgisayarında NFS istemcisinin [desteklenen sürümünün](data-box-system-requirements.md) yüklü olduğundan emin olun. Linux dağıtımınıza uygun sürümü kullanın. 

3. NFS istemcisi yüklendikten sonra, Data Box cihazınızdaki NFS paylaşımını bağlamak için aşağıdaki komutu kullanın:

    `sudo mount <Data Box device IP>:/<NFS share on Data Box device> <Path to the folder on local Linux computer>`

    Aşağıdaki örnekte, Data Box paylaşımına NFS yoluyla nasıl bağlanılacağı gösterilir. Data Box cihaz IP'si `10.161.23.130` şeklindedir, `Mystoracct_Blob` ubuntuVM'ye bağlanmıştır ve bağlama noktası `/home/databoxubuntuhost/databox` ile belirtilmiştir.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`
    
    Mac istemcileri için aşağıdaki gibi ek bir seçenek eklemeniz gerekir: 
    
    `sudo mount -t nfs -o sec=sys,resvport 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`

    **Her zaman kopyalamayı düşündüğünüz dosyalar için paylaşımda bir klasör oluşturun ve ardından dosyaları bu klasöre kopyalayın**. Blok blobu ve sayfa blobu paylaşımları altında oluşturulan klasör, verilerin blob olarak karşıya yüklendiği kapsayıcıyı temsil eder. Dosyaları depolama hesabındaki *root* klasörüne doğrudan kopyalayamazsınız.

## <a name="copy-data-from-data-box"></a>Data Box’tan veri kopyalama

Data Box paylaşımlarına bağlandıktan sonra veri kopyalamaya başlayabilirsiniz.

[!INCLUDE [data-box-export-review-logs](../../includes/data-box-export-review-logs.md)]

 Artık veri kopyalamaya başlayabilirsiniz. Linux ana bilgisayar kullanıyorsanız Robocopy ile benzer bir kopyalama yardımcı programı kullanabilirsiniz. Linux için kullanabileceğiniz bazı alternatifler: [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) veya [Ultracopier](https://ultracopier.first-world.info/).  

`cp` komutu, dizin kopyalamak için en iyi seçeneklerden biridir. Kullanımı hakkında daha fazla bilgi için [cp man sayfalarına](http://man7.org/linux/man-pages/man1/cp.1.html) gidin.

Çok iş parçacığına sahip olan bir kopyalama işlemi için rsync seçeneğini kullanıyorsanız şu yönergeleri izleyin:

* Linux istemcinizde kullanılan dosya sistemine bağlı olarak **CIFS Utils** veya **NFS Utils** paketini yükleyebilirsiniz.

    `sudo apt-get install cifs-utils`

    `sudo apt-get install nfs-utils`

* **Rsync** ve **Parallel** (Linux dağıtılmış sürümüne bağlı olarak değişir) ' i yükler.

    `sudo apt-get install rsync`
   
    `sudo apt-get install parallel` 

* Bağlama noktası oluşturun.

    `sudo mkdir /mnt/databox`

* Birimi bağlayın.

    `sudo mount -t NFS4  //Databox IP Address/share_name /mnt/databox` 

* Klasör dizin yapısını kopyalayın.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databox`

* Dosyaları kopyalayın.

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databox/{}`

     Burada j paralelleştirme sayısını, X ise paralel kopya sayısını belirtir

     16 paralel kopyayla başlamanızı ve kullanılabilir kaynak durumuna göre iş parçacığı sayısını artırmanızı öneririz.

> [!IMPORTANT]
> Şu Linux dosya türleri desteklenmez: sembolik bağlantılar, karakter dosyaları, blok dosyaları, yuvalar ve kanallar. Bu dosya türleri **göndermeye hazırlama** adımı sırasında hatalara neden olur.

Kopyalama tamamlandıktan sonra **Pano**’ya giderek cihazınızdaki kullanılan alanı ve boş alanı doğrulayın.

Şimdi işleme devam edip Data Box’ınızı Microsoft’a gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Data Box'a bağlanma
> * Data Box’tan veri kopyalama

Data Box'ı Microsoft’a geri gönderme hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a gönderme](./data-box-deploy-export-picked-up.md)
