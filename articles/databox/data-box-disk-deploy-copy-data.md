---
title: Azure Data Box Disk verileri kopyalama öğreticisi | Microsoft Docs
description: Azure Data Box Disk'inizi nasıl veri kopyalayacağınızı öğrenmek için bu öğreticiyi kullanın
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.localizationpriority: high
ms.date: 07/23/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 5b9054f0be9b3098d853164e1ccf52df451f8165
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70012962"
---
::: zone target="docs"

# <a name="tutorial-copy-data-to-azure-data-box-disk-and-verify"></a>Öğretici: Azure Data Box Disk verileri kopyalama ve doğrulama

Bu öğreticide ana bilgisayarınızdan veri kopyalama ve veri bütünlüğünü doğrulamak için sağlama toplamı oluşturma adımları anlatılmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Verileri Data Box Disk'e kopyalama
> * Verileri doğrulama

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:
- [Öğreticiyi tamamladınız: Azure Data Box Disk](data-box-disk-deploy-set-up.md)yükleyip yapılandırın.
- Disklerinizin kilitleri açılır ve diskler bir istemci bilgisayara bağlanır.
- Disklere veri kopyalamak için kullanılan istemci bilgisayar [Desteklenen işletim sistemi](data-box-disk-system-requirements.md##supported-operating-systems-for-clients) çalıştırmalıdır.
- Verileriniz için hedeflenen depolama türünün [Desteklenen depolama türleri](data-box-disk-system-requirements.md#supported-storage-types-for-upload) ile eşleştiğinden emin olun.
- [Azure nesne boyut sınırları 'Ndaki yönetilen disk sınırlarını](data-box-disk-limits.md#azure-object-size-limits)gözden geçirin.


## <a name="copy-data-to-disks"></a>Disklere veri kopyalama

Verileri disklere kopyalamanız için aşağıdaki konuları gözden geçirin:

- Verilerin uygun dosya biçimine karşılık gelen klasörlere kopyalandığından emin olmak sizin sorumluluğunuzdur. Örneğin blok blobu verilerinin blok blobu klasörlerine kopyalanması gerekir. Veri biçimi uygun klasörle (depolama türü) eşleşmiyorsa veriler Azure'a yüklenemez.
- Veri kopyalama sırasında veri boyutunun [Azure depolama ve Data Box Disk sınırları](data-box-disk-limits.md) içinde belirtilen boyut sınırlarına uygun olduğundan emin olun.
- Data Box Disk tarafından yüklenen verilerin Data Box Disk haricinde başka bir uygulama tarafından da yüklenmesi durumunda yükleme işinde hata oluşabilir ve veri bozulması yaşanabilir.

   > [!IMPORTANT]
   >  Sipariş oluşturma sırasında depolama hedeflerinden biri olarak yönetilen diskler belirttiyseniz, aşağıdaki bölüm geçerlidir.

- Yalnızca bir kaynak grubunda verilen ada sahip tek bir yönetilen diske, önceden oluşan tüm klasörlerde ve tüm Data Box Disk erişebilirsiniz. Bu, önceden düzenlenen klasörlere yüklenen VHD 'Lerin benzersiz adlara sahip olması gerektiğini gösterir. Verilen adın bir kaynak grubunda zaten var olan bir yönetilen diskle eşleşmediğinden emin olun. VHD 'ler aynı ada sahip ise, yalnızca bir VHD bu adı taşıyan yönetilen diske dönüştürülür. Diğer VHD 'ler, hazırlama depolama hesabına sayfa Blobları olarak yüklenir.
- VHD 'leri her zaman önceden düzenlenen klasörlerden birine kopyalayın. VHD 'leri bu klasörlerin dışına veya oluşturduğunuz bir klasöre kopyalarsanız, VHD 'ler, yönetilen diskler değil, Azure depolama hesabına sayfa Blobları olarak yüklenir.
- Yönetilen diskler oluşturmak için yalnızca sabit VHD 'ler karşıya yüklenebilir. Dinamik VHD 'ler, fark kayıt vhd 'leri veya VHDX dosyaları desteklenmez.


Bilgisayarınızla Data Box Disk arasında bağlantı kurmak ve veri kopyalamak için aşağıdaki adımları gerçekleştirin.

1. Kilidi açılan sürücünün içeriğini görüntüleyin. Sürücüdeki önceden düzenlenen klasörlerin ve alt klasörlerin listesi, Data Box Disk sırası yerleştirilirken seçilen seçeneklere bağlı olarak değişir.

    |Seçili depolama hedefi  |Depolama hesabı türü|Hazırlama depolama hesabı türü |Klasörler ve alt klasörler  |
    |---------|---------|---------|------------------|
    |Depolama hesabı     |GPv1 veya GPv2                 | NA | Blok Blobu <br> PageBlob <br> AzureFile        |
    |Depolama hesabı     |BLOB depolama hesabı         | NA | Blok Blobu        |
    |Yönetilen diskler     |NA | GPv1 veya GPv2         | ManagedDisk<ul> <li>PremiumSSD</li><li>StandardSSD</li><li>StandardHDD</li></ul>        |
    |Depolama hesabı <br> Yönetilen diskler     |GPv1 veya GPv2 | GPv1 veya GPv2         |Blok Blobu <br> PageBlob <br> AzureFile <br> ManagedDisk<ul> <li> PremiumSSD </li><li>StandardSSD</li><li>StandardHDD</li></ul>         |
    |Depolama hesabı <br> Yönetilen diskler    |BLOB depolama hesabı | GPv1 veya GPv2         |Blok Blobu <br> ManagedDisk<ul> <li>PremiumSSD</li><li>StandardSSD</li><li>StandardHDD</li></ul>         |

    Bir GPv2 depolama hesabının belirtildiği bir sıranın örnek ekran görüntüsü aşağıda gösterilmiştir:

    ![Disk sürücüsünün içeriği](media/data-box-disk-deploy-copy-data/data-box-disk-content.png)
 
2. Blok Blobları olarak içeri aktarılması gereken verileri *blockblob* klasörüne kopyalayın. Benzer şekilde, VHD/VHDX gibi verileri *Pageblob* klasörüne ve içindeki verileri de *AzureFile* klasörüne kopyalayın.

    BlockBlob ve PageBlob klasörlerinin altındaki her klasör için Azure depolama hesabında bir kapsayıcı oluşturulur. BlockBlob ve PageBlob klasörlerinin altındaki tüm dosyalar Azure Depolama hesabındaki varsayılan `$root` kapsayıcısına kopyalanır. `$root` kapsayıcısındaki tüm dosyalar her zaman blok blobu olarak yüklenir.

   Dosyaları *AzureFile* klasörü içindeki bir klasöre kopyalayın. *AzureFile* klasöründeki bir alt klasör bir FileShare oluşturur. Doğrudan *AzureFile* klasörüne kopyalanan dosyalar başarısız olur ve blok Blobları olarak karşıya yüklenir.

    Kök dizinde dosya ve klasörler varsa veri kopyalama işlemine başlamadan önce bunları farklı bir klasöre taşımanız gerekir.

    > [!IMPORTANT]
    > Tüm kapsayıcılar, Bloblar ve dosya adları [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uymalıdır. Bu kurallara uyulmaması halinde veriler Azure'a yüklenemez.

3. Dosyaları kopyalarken, dosyaların blok Blobları için ~ 4,7 TiB, sayfa Blobları için yaklaşık 8 TiB ve Azure dosyaları için ~ 1 TiB 'yi aşmadığından emin olun. 
4. Verileri kopyalamak için Veri Gezgini'nde sürükle ve bırak komutlarını kullanabilirsiniz. Verilerinizi kopyalamak için Robocopy gibi SMB uyumlu herhangi bir dosya kopyalama aracını da kullanabilirsiniz. Aşağıdaki Robocopy komutunu kullanarak birden fazla kopyalama işlemini başlatabilirsiniz:

    `Robocopy <source> <destination>  * /MT:64 /E /R:1 /W:1 /NFL /NDL /FFT /Log:c:\RobocopyLog.txt` 
    
    Komut parametreleri ve seçenekleri aşağıda belirtilmiştir:
    
    |Parametreler/Seçenekler  |Açıklama |
    |--------------------|------------|
    |Source            | Kaynak dizin yolunu belirtir.        |
    |Hedef       | Hedef dizin yolunu belirtir.        |
    |/E                  | Boş dizinler dahil olmak üzere alt dizinleri kopyalar. |
    |/MT[:N]             | N iş parçacığına sahip çoklu iş parçacıklı kopyalama işlemleri oluşturur ve burada N, 1 ile 128 arasında bir tam sayıdır. <br>N için varsayılan değer 8 olarak belirlenmiştir.        |
    |SAĞ \<N >             | Başarısız kopyalama işlemleri için yeniden deneme sayısını belirtir. N için varsayılan değer 1.000.000 (bir milyon yeniden deneme) olarak belirlenmiştir.        |
    |ANLATIMI \<N >             | Yeniden deneme işlemleri arasındaki bekleme süresini saniye cinsinden belirtir. N için varsayılan değer 30 (30 saniyelik bekleme süresi) olarak belirlenmiştir.        |
    |/NFL                | Dosya adlarının günlüğü alınmayacağını belirtir.        |
    |/NDL                | Dizin adlarının günlüğü alınmayacağını belirtir.        |
    |/FFT                | FAT dosya sürelerini (iki saniyelik duyarlık) kullanır.        |
    |/Log:\<günlük dosyası >     | Durum çıkışını günlük dosyasına yazar (var olan günlük dosyasının üzerine yazar).         |

    Her birinde birden fazla işin çalıştığı birden fazla disk kullanılabilir.

6. İş devam ederken kopyalama durumunu kontrol edin. Aşağıdaki örnekte dosyaları Data Box Disk'e kopyalamak için kullanılan Robocopy komutunun çıkışı gösterilmektedir.

    ```
    C:\Users>robocopy
        -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:53 PM
           Simple Usage :: ROBOCOPY source destination /MIR
    
                 source :: Source Directory (drive:\path or \\server\share\path).
            destination :: Destination Dir  (drive:\path or \\server\share\path).
                   /MIR :: Mirror a complete directory tree.
    
        For more usage information run ROBOCOPY /?    
    
    ****  /MIR can DELETE files as well as copy them !
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:64 /E /R:1 /W:1 /FFT 
    -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:58 PM
       Source : C:\Git\azure-docs-pr\contributor-guide\
         Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
      Options : *.* /DCOPY:DA /COPY:DAT /MT:8 /R:1000000 /W:30
    
    ------------------------------------------------------------------------------
    
    100%        New File                 206        C:\Git\azure-docs-pr\contributor-guide\article-metadata.md
    100%        New File                 209        C:\Git\azure-docs-pr\contributor-guide\content-channel-guidance.md
    100%        New File                 732        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-index.md
    100%        New File                 199        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pr-criteria.md
                New File                 178        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-co100%  .md
                New File                 250        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-et100%  e.md
    100%        New File                 174        C:\Git\azure-docs-pr\contributor-guide\create-images-markdown.md
    100%        New File                 197        C:\Git\azure-docs-pr\contributor-guide\create-links-markdown.md
    100%        New File                 184        C:\Git\azure-docs-pr\contributor-guide\create-tables-markdown.md
    100%        New File                 208        C:\Git\azure-docs-pr\contributor-guide\custom-markdown-extensions.md
    100%        New File                 210        C:\Git\azure-docs-pr\contributor-guide\file-names-and-locations.md
    100%        New File                 234        C:\Git\azure-docs-pr\contributor-guide\git-commands-for-master.md
    100%        New File                 186        C:\Git\azure-docs-pr\contributor-guide\release-branches.md
    100%        New File                 240        C:\Git\azure-docs-pr\contributor-guide\retire-or-rename-an-article.md
    100%        New File                 215        C:\Git\azure-docs-pr\contributor-guide\style-and-voice.md
    100%        New File                 212        C:\Git\azure-docs-pr\contributor-guide\syntax-highlighting-markdown.md
    100%        New File                 207        C:\Git\azure-docs-pr\contributor-guide\tools-and-setup.md
    ------------------------------------------------------------------------------
    
                   Total    Copied   Skipped  Mismatch    FAILED    Extras
        Dirs :         1         1         1         0         0         0
       Files :        17        17         0         0         0         0
       Bytes :     3.9 k     3.9 k         0         0         0         0
       Times :   0:00:05   0:00:00                       0:00:00   0:00:00
        
       Speed :                5620 Bytes/sec.
       Speed :               0.321 MegaBytes/min.
       Ended : Thursday, March 8, 2018 2:34:59 PM
        
    C:\Users>
    ```
 
    Performansı iyileştirmek için veri kopyalama sırasında aşağıdaki Robocopy parametrelerini kullanın.

    |    Platform    |    Çoğunlukla küçük dosyalar, 512 KB altı                           |    Çoğunlukla orta büyüklükteki dosyalar, 512 KB-1 MB arası                      |    Çoğunlukla büyük dosyalar, 1 MB üzeri                             |   
    |----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|
    |    Data Box Disk        |    4 Robocopy oturumu * <br> Oturum başına 16 iş parçacığı    |    2 Robocopy oturumu * <br> Oturum başına 16 iş parçacığı    |    2 Robocopy oturumu * <br> Oturum başına 16 iş parçacığı    |
    
    **Her Robocopy oturumunda en fazla 7.000 dizin ve 150.000.000 dosyası olabilir.*
    
    >[!NOTE]
    > Yukarıda önerilen parametreler, Inhouse testinde kullanılan ortama dayalıdır.
    
    Robocopy komutu hakkında daha fazla bilgi için bkz. [Robocopy ve birkaç örnek](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx).

6. Kopyalanan dosyaları görüntülemek ve doğrulamak için hedef klasörü açın. Kopyalama işlemi sırasında hatayla karşılaşırsanız sorun giderme için günlük dosyalarını indirin. Günlük dosyaları Robocopy komutunda belirtilen şekilde bulunur.
 
### <a name="split-and-copy-data-to-disks"></a>Verileri bölme ve disklere kopyalama

Birden fazla disk kullanıyorsanız ve bölünerek tüm disklere kopyalanması gereken büyük bir veri kümesine sahipseniz bu isteğe bağlı yordamı kullanabilirsiniz. Data Box Split Copy aracı Windows bilgisayarlarda veri bölme ve kopyalama işlemlerini gerçekleştirmenize yardımcı olur.

>[!IMPORTANT]
> Data Box bölünmüş kopyalama aracı verilerinizi de doğrular. Verileri kopyalamak için Data Box bölünmüş kopyalama aracı kullanırsanız, [doğrulama adımını](#validate-data)atlayabilirsiniz.
> Bölünmüş kopyalama aracı, yönetilen disklerle desteklenmez.

1. Data Box Split Copy aracını Windows bilgisayarınıza indirip yerel bir klasöre ayıkladığınızdan emin olun. Bu araç Windows için Data Box Disk araç takımıyla birlikte indirilmiştir.
2. Dosya Gezgini'ni açın. Veri kaynağı sürücüsünü ve Data Box Disk'e atanmış olan sürücü harflerini not edin. 

     ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-1.png)
 
3. Kopyalanacak veri kaynağını belirleyin. Örneğin, bu durumda:
    - Aşağıdaki blok blobu tanımlanmıştır.

         ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-2.png)    

    - Aşağıdaki sayfa blobu tanımlanmıştır.

         ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-3.png)
 
4. Yazılımı ayıkladığınız klasöre gidin. `SampleConfig.json` Dosyayı bu klasörde bulun. Bu değiştirip kaydedebileceğiniz bir salt okunur dosyadır.

   ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-4.png)
 
5. `SampleConfig.json` Dosyayı değiştirin.
 
   - İş adı girin. Bunu yaptığınızda Data Box Disk'te bir klasör oluşturulur ve aynı ad Azure depolama hesabında bu disklerle ilişkilendirilmiş olan kapsayıcı için de kullanılır. İş adının Azure kapsayıcı adlandırma kurallarına uygun olması gerekir. 
   - İçindeki yol biçimini notunun kaynak yolunu sağlayın `SampleConfigFile.json`. 
   - Hedef disklere karşılık gelen sürücü harflerini girin. Veriler kaynak yoldan alınarak birden fazla diske kopyalanır.
   - Günlük dosyaları için bir yol belirtin. Varsayılan olarak, bulunduğu geçerli dizine `.exe` gönderilir.

     ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-5.png)

6. Dosya biçimini doğrulamak için adresine gidin `JSONlint`. Dosyayı farklı `ConfigFile.json`kaydedin. 

     ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-6.png)
 
7. Bir komut istemi penceresi açın. 

8. Öğesini çalıştırın `DataBoxDiskSplitCopy.exe`. Type

    `DataBoxDiskSplitCopy.exe PrepImport /config:<Your-config-file-name.json>`

     ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-7.png)
 
9. Betiği sürdürmek için Enter tuşuna basın.

    ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-8.png)
  
10. Veri kümesi bölünüp kopyalandıktan sonra kopyalama oturumuna ait Split Copy aracı özeti görüntülenir. Örnek çıktı aşağıda gösterilmiştir.

    ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-9.png)
 
11. Verilerin hedef disklere bölündüğünü doğrulayın. 
 
    ![Bölünmüş kopya verileri](media/data-box-disk-deploy-copy-data/split-copy-10.png)
    ![bölünmüş kopya verileri](media/data-box-disk-deploy-copy-data/split-copy-11.png)
     
    `n:` Sürücünün içeriğini daha fazla inceleyebilirsiniz, Blok Blobu ve Sayfa Blobu biçim verilerine karşılık gelen iki alt klasör oluşturulduğunu görürsünüz.
    
     ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-12.png)

12. Kopyalama oturumu başarısız olursa kurtarıp sürdürmek için şu komutu kullanın:

    `DataBoxDiskSplitCopy.exe PrepImport /config:<configFile.json> /ResumeSession`

Bölünmüş kopyalama aracını kullanarak hatalar görürseniz, [bölünmüş kopyalama aracı hatalarıyla ilgili sorunları giderme](data-box-disk-troubleshoot-data-copy.md)bölümüne gidin.

Veri kopyalama işlemi tamamlandıktan sonra verilerinizi doğrulamaya devam edebilirsiniz. Bölünmüş kopyalama aracını kullandıysanız, doğrulamayı atlayın (bölünmüş kopyalama aracı da doğrular) ve sonraki öğreticiye ilerleyin.


## <a name="validate-data"></a>Verileri doğrulama

Verileri kopyalamak için bölünmüş kopyalama aracını kullanmıyorsanız, verilerinizi doğrulamanız gerekir. Verileri doğrulamak için aşağıdaki adımları uygulayın.

1. Sağlama toplamı doğrulaması için sürücünüzün *DataBoxDiskImport* klasöründe `DataBoxDiskValidation.cmd` komutunu çalıştırın.
    
    ![Data Box Diski doğrulama aracı çıktısı](media/data-box-disk-deploy-copy-data/data-box-disk-validation-tool-output.png)

2. Uygun olanı seçin. **Her zaman 2. seçeneği işaretleyerek dosyaları doğrulamanızı ve sağlama toplamları oluşturmanızı öneririz**. Bu adım verilerinizin boyutuna bağlı olarak uzun sürebilir. Betik tamamlandığında komut penceresinden çıkın. Doğrulama ve sağlama toplamı alma sırasında herhangi bir hata olursa size bildirilir ve hata günlüklerine bir bağlantı sunulur.

    ![Sağlama toplamı çıktısı](media/data-box-disk-deploy-copy-data/data-box-disk-checksum-output.png)

    > [!TIP]
    > - İki çalıştırma arasında aracı sıfırlayın.
    > - Küçük dosyalar (~ KBs) içeren büyük veri kümesiyle uğraşıyorsanız 1 seçeneğini kullanın. Bu seçenek, yalnızca dosyaları doğrular, sağlama toplamı oluşturma çok uzun zaman alabilir ve performans çok yavaş olabilir.

3. Birden çok disk kullanıyorsanız, komutu her disk için çalıştırın.

Doğrulama sırasında hata görürseniz bkz. [doğrulama hatalarını giderme](data-box-disk-troubleshoot.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box Disk konularını öğrendiniz:

> [!div class="checklist"]
> * Verileri Data Box Disk'e kopyalama
> * Veri bütünlüğünü doğrulama

Data Box Disk'i iade etme ve Azure'a yüklenen verileri doğrulama adımları hakkında bilgi edinmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a geri gönderme](./data-box-disk-deploy-picked-up.md)

::: zone-end

::: zone target="chromeless"

## <a name="copy-data-to-disks"></a>Disklere veri kopyalama

Data Box Disk bağlanıp bilgisayarınızdan veri kopyalamak için aşağıdaki adımları uygulayın.

1. Kilidi açılan sürücünün içeriğini görüntüleyin. Sürücüdeki önceden düzenlenen klasörlerin ve alt klasörlerin listesi, Data Box Disk sırası yerleştirilirken seçilen seçeneklere bağlı olarak değişir.
2. Verileri uygun veri biçimine karşılık gelen klasörlere kopyalayın. Örneğin, yapılandırılmamış verileri *Blok Blobu* klasörü, VHD veya vhdx verileri Için *pageblob* klasörüne ve dosyaları *AzureFile*klasörüne kopyalayın. Veri biçimi uygun klasörle (depolama türü) eşleşmiyorsa, daha sonraki bir adımda Azure 'a yüklenen veriler başarısız olur.

    - BlockBlob ve PageBlob klasörlerinin altındaki her klasör için Azure depolama hesabında bir kapsayıcı oluşturulur. *Blockblob* ve *pageblob* klasörlerinin altındaki tüm dosyalar, Azure depolama hesabı altında $root varsayılan bir kapsayıcıya kopyalanır. 
    - $Root kapsayıcısındaki tüm dosyalar her zaman blok blob 'ları olarak yüklenir.
    - Dosyaları *AzureFile* klasörü içindeki bir klasöre kopyalayın. *AzureFile* klasöründeki bir alt klasör bir FileShare oluşturur. Doğrudan *AzureFile* klasörüne kopyalanan dosyalar başarısız olur ve blok Blobları olarak karşıya yüklenir.
    - Kök dizinde dosya ve klasörler varsa veri kopyalama işlemine başlamadan önce bunları farklı bir klasöre taşımanız gerekir.
    - Siparişinizin depolama hedeflerinden biri olarak yönetilen diskleri varsa, bkz. [yönetilen diskler](data-box-disk-limits.md#managed-disk-naming-conventions)için adlandırma kuralları.

    > [!IMPORTANT]
    > Tüm kapsayıcılar, Bloblar ve dosya [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions) ve [Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uymalıdır. Bu kurallara veya sınırlara uyulmazsa, verileri Azure 'a yükleme başarısız olur.

3. Dosya Gezgini ile sürükleyip bırakmayı veya verilerinizi kopyalamak için Robocopy gibi herhangi bir SMB uyumlu dosya kopyalama aracını kullanın. Birden çok kopyalama işi aşağıdaki komut kullanılarak başlatılabilir:

    ```
    Robocopy <source> <destination>  * /MT:64 /E /R:1 /W:1 /NFL /NDL /FFT /Log:c:\RobocopyLog.txt
    ```
4. Kopyalanan dosyaları görüntülemek ve doğrulamak için hedef klasörü açın. Kopyalama işlemi sırasında hatayla karşılaşırsanız sorun giderme için günlük dosyalarını indirin. Günlük dosyaları Robocopy komutunda belirtilen şekilde bulunur.

Birden çok disk kullandığınızda ve tüm disklerde bölünmesi ve kopyalanması gereken büyük bir veri kümesine sahip olduğunuzda, [bölme ve kopyalama](data-box-disk-deploy-copy-data.md#split-and-copy-data-to-disks) için isteğe bağlı prosedürü kullanın.

## <a name="validate-data"></a>Verileri doğrulama

Verilerinizi doğrulamak için aşağıdaki adımları uygulayın.

1. Sağlama toplamı doğrulaması için sürücünüzün *DataBoxDiskImport* klasöründe `DataBoxDiskValidation.cmd` komutunu çalıştırın.
2. Dosyalarınızı doğrulamak ve sağlama toplamı oluşturmak için 2 seçeneğini kullanın. Bu adım verilerinizin boyutuna bağlı olarak uzun sürebilir. Doğrulama ve sağlama toplamı alma sırasında herhangi bir hata olursa size bildirilir ve hata günlüklerine bir bağlantı sunulur.

    Doğrulama sırasında hata görürseniz bkz. [doğrulama hatalarını giderme](data-box-disk-troubleshoot.md).

::: zone-end
