---
title: NFS üzerinden Azure Veri Kutusu Heavy'ye veri kopyalama eğitimi| Microsoft Dokümanlar
description: NFS aracılığıyla Azure Veri Kutusu Heavy'nize verileri nasıl kopyalayınız öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 4361cee3d07408c3abb5031d2ab18c15c92c5e0a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238989"
---
# <a name="tutorial-copy-data-to-azure-data-box-heavy-via-nfs"></a>Öğretici: Verileri NFS ile Azure Veri Kutusu Heavy'ye kopyalama

Bu öğretici, yerel web ui'sini kullanarak ana bilgisayarınızdaki verilere nasıl bağlanıp kopyalanın, Azure Veri Kutusu Ağır'ınıza nasıl bağlanılabildiğini ve kopyalayabildiğini açıklar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Ön koşullar
> * Data Box Heavy’ye bağlanma
> * Data Box Heavy’ye veri kopyalama

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. [Öğreticiyi tamamladınız: Azure Veri Kutusu Ağır'ı ayarlayın.](data-box-heavy-deploy-set-up.md)
2. Veri Kutunuz Ağır'ı aldınız ve portaldaki sipariş durumu **teslim**edildi.
3. Data Box Heavy üzerinden kopyalamak istediğiniz verileri içeren bir ana bilgisayarınız var. Konak bilgisayarınızda:
    - [Desteklenen bir işletim sistemi](data-box-heavy-system-requirements.md) çalıştırılmalıdır.
    - Yüksek hızlı bir ağa bağlı olmalıdır. En yüksek kopyalama hızları için iki adet 40 GbE bağlantı (düğüm başına bir tane) birbirine paralel olarak kullanılabilir. 40 GbE bağlantınız yoksa, en az iki tane (düğüm başına bir tane) 10 GbE bağlantınızın olması önerilir. 

## <a name="connect-to-data-box-heavy"></a>Data Box Heavy’ye bağlanma

Seçilen depolama hesabına bağlı olarak, Data Box Heavy şunların tamamını veya bir bölümünü oluşturabilir:
- İlişkili her depolama hesabına GPv1 ve GPv2 için üç paylaşım.
- Premium depolama için bir paylaşım.
- Blob depolama hesabı için bir paylaşım.

Bu paylaşımlar, cihazın her iki düğümünde de oluşturulur.

Blok blobu ve sayfa blobu paylaşımları altında:
- Birinci düzey varlıklar kapsayıcılardır.
- İkinci düzey varlıklar bloblardır.

Azure Dosyalar paylaşımlarının altında:
- Birinci düzey varlıklar paylaşımlardır.
- İkinci düzey varlıklar dosyalardır.

Aşağıdaki tabloda, Data Box Heavy üzerindeki paylaşımların UNC yolu ve verilerin karşıya yüklendiği Azure Depolama yolu URL'si gösterilmektedir. Son Azure depolama yolu URL'si, UNC paylaşım yolundan türetilebilir.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Azure Blok blobları | <li>Paylaşımların UNC yolu: `//<DeviceIPAddress>/<StorageAccountName_BlockBlob>/<ContainerName>/files/a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure Sayfa blobları  | <li>Paylaşımların UNC yolu: `//<DeviceIPAddres>/<StorageAccountName_PageBlob>/<ContainerName>/files/a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Dosyaları       |<li>Paylaşımların UNC yolu: `//<DeviceIPAddres>/<StorageAccountName_AzFile>/<ShareName>/files/a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |

Bir Linux ana bilgisayar kullanıyorsanız, aygıtınızı NFS istemcilerine erişime izin verecek şekilde yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. Paylaşıma erişmesine izin verilen istemcilerin IP adreslerini sağlayın. Yerel web arabiriminde **Bağlan ve kopyala** sayfasına gidin. **NFS ayarları** bölümünde **NFS istemci erişimi**'ne tıklayın. 

    ![NFS istemci erişimini yapılandırma 1](media/data-box-deploy-copy-data/nfs-client-access.png)

2. NFS istemcisinin IP adresini girin ve **Ekle**'ye tıklayın. Bu adımı tekrarlayarak birden fazla NFS istemcisi için erişim sağlayabilirsiniz. **Tamam**'a tıklayın.

    ![NFS istemci erişimini yapılandırma 2](media/data-box-deploy-copy-data/nfs-client-access2.png)

2. Linux ana bilgisayarında NFS istemcisinin [desteklenen sürümünün](data-box-system-requirements.md) yüklü olduğundan emin olun. Linux dağıtımınıza uygun sürümü kullanın. 

3. NFS istemcisi yüklendikten sonra, Data Box cihazınızdaki NFS paylaşımını bağlamak için aşağıdaki komutu kullanın:

    `sudo mount <Data Box Heavy device IP>:/<NFS share on Data Box Heavy device> <Path to the folder on local Linux computer>`

    Aşağıdaki örnek, NFS üzerinden Veri Kutusu Ağır paylaşımına nasıl bağlanılabildiğini gösterir. Veri Kutusu Ağır `10.161.23.130`IP, `Mystoracct_Blob` payı ubuntuVM üzerine monte edilir, montaj noktası olmak `/home/databoxheavyubuntuhost/databoxheavy`.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxheavyubuntuhost/databoxheavy`
    
    Mac istemcileri için aşağıdaki gibi ek bir seçenek eklemeniz gerekir: 
    
    `sudo mount -t nfs -o sec=sys,resvport 10.161.23.130:/Mystoracct_Blob /home/databoxheavyubuntuhost/databoxheavy`

    **Her zaman paylaşım altında kopyalamak istediğiniz dosyalar için bir klasör oluşturun ve sonra dosyaları bu klasöre kopyalayın.** Blok blobu ve sayfa blobu paylaşımları altında oluşturulan klasör, verilerin blob olarak karşıya yüklendiği kapsayıcıyı temsil eder. Dosyaları depolama hesabındaki *root* klasörüne doğrudan kopyalayamazsınız.

## <a name="copy-data-to-data-box-heavy"></a>Data Box Heavy’ye veri kopyalama

Data Box Heavy paylaşımlarına bağlandıktan sonra, bir sonraki adım verileri kopyalamaktır. Verileri kopyalamaya başlamadan önce aşağıdaki konuları gözden geçirin:

- Verilerin uygun dosya biçimine karşılık gelen paylaşımlara kopyalandığından emin olun. Örneğin blok blobu verilerinin blok blobu paylaşımına kopyalanması gerekir. Sayfa damlalarına VHD'leri kopyalayın. Veri biçimi uygun paylaşım türüyle eşleşmiyorsa verilerin Azure'a yüklenmesi başarısız olur.
-  Verileri kopyalarken, veri boyutunun Azure depolama ve Veri [Kutusu Ağır sınırlarında](data-box-heavy-limits.md)açıklanan boyut sınırlarına uyduklarından emin olun. 
- Data Box Heavy tarafından yüklenen verilerin Data Box Heavy haricindeki başka bir uygulama tarafından da yüklenmesi durumunda yükleme işinde hata oluşabilir ve veri bozulması yaşanabilir.
- Aynı anda hem SMB hem de NFS kullanmamanızı veya aynı verileri Azure'daki aynı uç hedefe kopyalamamanızı öneririz. Bu gibi durumlarda nihai sonucu kestirmek mümkün olmayabilir.
- **Her zaman paylaşım altında kopyalamak istediğiniz dosyalar için bir klasör oluşturun ve sonra dosyaları bu klasöre kopyalayın.** Blok blobu ve sayfa blobu paylaşımları altında oluşturulan klasör, verilerin blob olarak karşıya yüklendiği kapsayıcıyı temsil eder. Dosyaları depolama hesabındaki *root* klasörüne doğrudan kopyalayamazsınız.
- Data Box Heavy'de NFS paylaşımından NFS'ye büyük/küçük harf duyarlı dizin ve dosya adları alıyorsanız: 
    - Dava adına korunur.
    - Dosyalar büyük/küçük harf duyarsız.
    
    Örneğin, kopyalama `SampleFile.txt` ve `Samplefile.Txt`, durumda aygıta kopyalandığında adında korunur, ancak bu aynı dosya olarak kabul edilir gibi ikinci dosya ilkinin üzerine yazacaktır.


Linux ana bilgisayar kullanıyorsanız Robocopy ile benzer bir kopyalama yardımcı programı kullanabilirsiniz. Linux için kullanabileceğiniz bazı alternatifler: [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) veya [Ultracopier](https://ultracopier.first-world.info/).  

`cp` komutu, dizin kopyalamak için en iyi seçeneklerden biridir. Kullanımı hakkında daha fazla bilgi için [cp man sayfalarına](http://man7.org/linux/man-pages/man1/cp.1.html) gidin.

Çok iş parçacığına sahip olan bir kopyalama işlemi için rsync seçeneğini kullanıyorsanız şu yönergeleri izleyin:

 - Linux istemcinizde kullanılan dosya sistemine bağlı olarak **CIFS Utils** veya **NFS Utils** paketini yükleyebilirsiniz.

    `sudo apt-get install cifs-utils`

    `sudo apt-get install nfs-utils`

 -  **Rsync** ve **Parallel** uygulamalarını yükleyin (Linux dağıtımınıza göre değişir).

    `sudo apt-get install rsync`
   
    `sudo apt-get install parallel` 

 - Bağlama noktası oluşturun.

    `sudo mkdir /mnt/databoxheavy`

 - Birimi bağlayın.

    `sudo mount -t NFS4  //Databox-heavy-IP-Address/share_name /mnt/databoxheavy` 

 - Klasör dizin yapısını kopyalayın.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databoxheavy`

 - Dosyaları kopyalayın. 

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databoxheavy/{}`

     Burada j paralelleştirme sayısını, X ise paralel kopya sayısını belirtir

     16 paralel kopyayla başlamanızı ve kullanılabilir kaynak durumuna göre iş parçacığı sayısını artırmanızı öneririz.

> [!IMPORTANT]
> Aşağıdaki Linux dosya türleri desteklenmez: sembolik bağlantılar, karakter dosyaları, blok dosyaları, soketler ve borular. Bu dosya **türleri, gemiye hazırla** adımı sırasında hatalara neden olur.

Kopyalanan dosyaları görüntülemek ve doğrulamak için hedef klasörü açın. Kopyalama işlemi sırasında hatayla karşılaşırsanız sorun giderme için hata dosyalarını indirin. Daha fazla bilgi için bkz. [Data Box Heavy’ye veri kopyalama sırasında hata günlüklerini görüntüleme](data-box-logs.md#view-error-log-during-data-copy). Veri kopyalama sırasında karşılaşılan hataların ayrıntılı bir listesi için bkz. [Data Box Heavy sorunlarını giderme](data-box-troubleshoot.md).

Veri bütünlüğünü sağlamak için sağlama toplamı veri kopyalama sırasında satır içinde hesaplanır. Kopyalama tamamlandıktan sonra cihazınızdaki kullanılan alanı ve boş alanı doğrulayın.
    
   ![Panoda boş ve kullanılan alanı doğrulama](media/data-box-deploy-copy-data/verify-used-space-dashboard.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Data Box Heavy ile ilgili aşağıdaki bilgileri öğrendiniz:

> [!div class="checklist"]
> * Ön koşullar
> * Data Box Heavy’ye bağlanma
> * Data Box Heavy’ye veri kopyalama


Data Box'ı Microsoft’a geri gönderme hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box Heavy’yi Microsoft'a gönderme](./data-box-heavy-deploy-picked-up.md)

