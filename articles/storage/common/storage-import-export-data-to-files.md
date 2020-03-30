---
title: Verileri Azure Dosyalarına aktarmak için Azure İçe Aktarma/Verme kullanma | Microsoft Dokümanlar
description: Verileri Azure Dosyaları'na aktarmak için Azure portalında nasıl içe aktarma işleri oluşturabilirsiniz öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 04/08/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 201d0c0a545c5ba7ae1bb0b5e119f7acb1ae362f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268307"
---
# <a name="use-azure-importexport-service-to-import-data-to-azure-files"></a>Azure Dosyaları'na veri aktarmak için Azure İçeri/Dışarı Aktarma hizmetini kullanma

Bu makalede, Azure Dosyaları'na büyük miktarda veri güvenli bir şekilde içe aktarmak için Azure İçe Alma/Dışa Aktarma hizmetinin nasıl kullanılacağına ilişkin adım adım yönergeler verilmektedir. Veri almak için hizmet, verilerinizi içeren desteklenen disk sürücülerini bir Azure veri merkezine göndermenizi gerektirir.  

İçe Aktarma/Verme hizmeti, Azure Dosyalarının Azure Depolama'ya yalnızca içe aktarını destekler. Azure Dosyalarını dışa aktarma desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

Verileri Azure Dosyaları'na aktarmak için bir alma işi oluşturmadan önce, aşağıdaki ön koşullar listesini dikkatle inceleyin ve tamamlayın. Şunları yapmalısın:

- İçe Aktarma/Dışa Aktarma hizmetiyle kullanmak üzere etkin bir Azure aboneliğine sahip olun.
- En az bir Azure Depolama hesabınız var. [İçe Aktarma/Dışa Aktarma hizmeti için Desteklenen depolama hesapları nın ve depolama türlerinin listesine](storage-import-export-requirements.md)bakın. Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için [bkz.](storage-account-create.md)
- [Desteklenen türlerin](storage-import-export-requirements.md#supported-disks)disksayısı yeterli sayıda var.
- Desteklenen işletim sistemi [sürümünü](storage-import-export-requirements.md#supported-operating-systems)çalıştıran bir Windows sistemine sahip.
- [WAImportExport sürüm 2'yi](https://aka.ms/waiev2) Windows sisteminden indirin. Varsayılan klasöre `waimportexport`unzip . Örneğin, `C:\WaImportExport`.
- Bir FedEx/DHL hesabınız var. FedEx/DHL dışında bir operatör kullanmak istiyorsanız, 'den Azure `adbops@microsoft.com`Veri Kutusu İşlemleri ekibine başvurun.  
    - Hesap geçerli olmalı, bakiyeye sahip olmalı ve sevkiyat alabilmeli.
    - Dışa aktarma işi için bir izleme numarası oluşturun.
    - Her işin ayrı bir izleme numarası olmalıdır. Aynı izleme numarasına sahip birden çok iş desteklenmez.
    - Operatör hesabınız yoksa, şu na gidin:
        - [Bir FedEX hesabı oluşturun](https://www.fedex.com/en-us/create-account.html)veya
        - [Bir DHL hesabı oluşturun.](http://www.dhl-usa.com/en/express/shipping/open_account.html)



## <a name="step-1-prepare-the-drives"></a>Adım 1: Sürücüleri hazırlama

Bu adım bir günlük dosyası oluşturur. Günlük dosyası sürücü seri numarası, şifreleme anahtarı ve depolama hesabı ayrıntıları gibi temel bilgileri depolar.

Sürücüleri hazırlamak için aşağıdaki adımları gerçekleştirin.

1. Disk sürücülerimizi SATA konektörleri aracılığıyla Windows sistemine bağlayın.
2. Her sürücüde tek bir NTFS hacmi oluşturun. Ses birimine bir sürücü mektubu atayın. Montaj noktalarını kullanmayın.
3. Aracın bulunduğu kök klasöründeki *dataset.csv* dosyasını değiştirin. Bir dosya veya klasör veya her ikisini de almak isteyip istemediğiniz bağlı olarak, *dataset.csv* dosyasına aşağıdaki örneklere benzer girişler ekleyin.  

   - **Bir dosyayı almak için**: Aşağıdaki örnekte, kopyalanması gereken veriler F: sürücüde bulunur. Dosyanız *MyFile1.txt* *MyAzureFileshare1*köküne kopyalanır. *MyAzureFileshare1* yoksa, Azure Depolama hesabında oluşturulur. Klasör yapısı korunur.

       ```
           BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
           "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None

       ```
   - **Bir klasörü almak için**: *MyFolder2* altındaki tüm dosya ve klasörler özyinelemeli olarak fileshare'e kopyalanır. Klasör yapısı korunur.

       ```
           "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None

       ```
     Aynı dosyada, içe aktarılan klasörlere veya dosyalara karşılık gelen birden çok giriş yapılabilir.

       ```
           "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
           "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None

       ```
     [Dataset CSV dosyasını hazırlama](storage-import-export-tool-preparing-hard-drives-import.md)hakkında daha fazla bilgi edinin.


4. Aracın bulunduğu kök klasöründeki *driveset.csv* dosyasını değiştirin. *Driveset.csv* dosyasındaki girişleri aşağıdaki örneklere benzer şekilde ekleyin. Driveset dosyası, aracın hazırlanacak disklerin listesini doğru şekilde seçebilmeleri için disklerin ve ilgili sürücü harflerinin listesini kullanır.

    Bu örnek, iki diskin bağlı olduğunu ve temel NTFS hacimlerinin G:\ ve H:\ oluşturulur. G: zaten şifrelenmiş iken H:\şifrelenmez. Araç, H:\ sadece (ve G\)değil: .

   - **Şifrelenmemiş bir disk için**: Diskte BitLocker şifrelemesini etkinleştirmek için *Şifrele'yi* belirtin.

       ```
       DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
       H,Format,SilentMode,Encrypt,
       ```

   - **Zaten şifrelenmiş bir disk**için : *AlreadyEncrypted'ı* belirtin ve BitLocker tuşunu temin edin.

       ```
       DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
       G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
       ```

     Aynı dosyada birden çok sürücüye karşılık gelen birden çok giriş yapılabilir. [Driveset CSV dosyasını hazırlama](storage-import-export-tool-preparing-hard-drives-import.md)hakkında daha fazla bilgi edinin.

5. Verileri `PrepImport` disk sürücüsüne kopyalama ve hazırlama seçeneğini kullanın. Dizinleri ve/veya dosyaları yeni bir kopyalama oturumuyla kopyalamak için ilk kopyalama oturumu için aşağıdaki komutu çalıştırın:

       ```
       .\WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
       ```

   Bir alma örneği aşağıda gösterilmiştir.

       ```
       .\WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset.csv /DataSet:dataset.csv /logdir:C:\logs
       ```

6. Parametre ile `/j:` sağlanan adı içeren bir günlük dosyası, komut satırının her çalışması için oluşturulur. Hazırladığınız her sürücü, alma işini oluştururken yüklenmesi gereken bir günlük dosyasına sahiptir. Günlük dosyaları olmayan sürücüler işlenmez.

    > [!IMPORTANT]
    > - Disk hazırlamayı tamamladıktan sonra disk sürücüleri veya günlük dosyasındaki verileri değiştirmeyin.

Ek örnekler [için, günlük dosyaları için Örnekler'e](#samples-for-journal-files)gidin.

## <a name="step-2-create-an-import-job"></a>Adım 2: Bir alma işi oluşturma

Azure portalında bir alma işi oluşturmak için aşağıdaki adımları gerçekleştirin.
1. Oturum https://portal.azure.com/aç.
2. Tüm **hizmetlere gidin > Depolama > İthalat/Dışa Aktarma işleri.**

    ![İthalat/dışa aktarmaya git](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. **İçe Alma/Dışa Aktar İşi Oluştur'u**tıklatın.

    ![İçe/Dışa Aktar/Dışa Aktarma işini tıklatın](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. **Temel olarak:**

    - **Azure'a Aktar'ı**seçin.
    - Alma işi için açıklayıcı bir ad girin. İşlerinizi devam ederken ve tamamlandıktan sonra izlemek için bu adı kullanın.
        -  Bu ad yalnızca küçük harfler, sayılar, tireler ve alt çizerler içerebilir.
        -  Ad bir harfle başlamalı ve boşluk içermeyebilir.
    - Bir abonelik seçin.
    - Kaynak grubunu seçin.

        ![Alma işi oluşturma - Adım 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. **İş ayrıntıları:**

    - Önceki Adım 1 sırasında oluşturduğunuz günlük dosyalarını [yükleyin: Sürücüleri hazırlayın.](#step-1-prepare-the-drives)
    - Verilerin aktarılacAcağı depolama hesabını seçin.
    - Açılan konumu, seçilen depolama hesabının bölgesine bağlı olarak otomatik olarak doldurulur.

       ![Alma işi oluşturma - Adım 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. **Karşılığında nakliye bilgileri**:

    - Açılan listeden taşıyıcıyı seçin. FedEx/DHL dışında bir taşıyıcı kullanmak istiyorsanız, açılır yerden mevcut bir seçenek seçin. Kullanmayı planladığınız `adbops@microsoft.com` operatörle ilgili bilgilerle birlikte Azure Veri Kutusu İşlemleri ekibine başvurun.
    - Bu taşıyıcıyla oluşturduğunuz geçerli bir taşıyıcı hesap numarasını girin. Microsoft, alma işiniz tamamlandıktan sonra sürücüleri size geri sevk etmek için bu hesabı kullanır.
    - Tam ve geçerli bir iletişim adı, telefon, e-posta, sokak adresi, şehir, zip, eyalet/ il ve ülke/bölge sağlayın.

        > [!TIP]
        > Tek bir kullanıcı için e-posta adresi belirtmek yerine, grup e-postası sağlayın. Bu, bir yönetici ayrılsa bile bildirim almanızı sağlar.

       ![Alma işi oluşturma - Adım 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)


5. **Özetle**:

    - Diskleri Azure'a geri gönderme için Azure veri merkezi gönderi adresini sağlayın. İş adının ve tam adresinin sevkiyat etiketinde belirtildiğinden emin olun.
    - Alma iş oluşturmayı tamamlamak için **Tamam'ı** tıklatın.

        ![Alma işi oluşturma - Adım 4](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives-to-the-azure-datacenter"></a>Adım 3: Sürücüleri Azure veri merkezine gönderme

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-4-update-the-job-with-tracking-information"></a>Adım 4: İzleme bilgileriyle işi güncelleştirme

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-5-verify-data-upload-to-azure"></a>Adım 5: Azure'a veri yüklemesini doğrulayın

İşi tamamlayakadar izleyin. İş tamamlandıktan sonra verilerinizin Azure'a yüklendiğini doğrulayın. Şirket içi verileri yalnızca yüklemenin başarılı olduğunu doğruladıktan sonra silin.

## <a name="samples-for-journal-files"></a>Günlük dosyaları için örnekler

Daha **fazla sürücü eklemek**için yeni bir sürücü seti dosyası oluşturun ve komutu aşağıdaki gibi çalıştırın.

*InitialDriveset .csv* dosyasında belirtilenden farklı disk sürücülerine sonraki kopyalama oturumları için yeni bir driveset *.csv* dosyası belirtin ve parametreye `AdditionalDriveSet`değer olarak sağlayın. Aynı **günlük dosya** adını kullanın ve yeni bir **oturum kimliği**sağlayın. AdditionalDriveset CSV dosyasının biçimi InitialDriveSet biçimiyle aynıdır.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
    ```

Bir alma örneği aşağıda gösterilmiştir.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
    ```


Aynı sürücü kümesine ek veri eklemek için, ek dosyaları/dizini kopyalamak için sonraki kopyalama oturumları için PrepImport komutunu kullanın.

*InitialDriveset.csv* dosyasında belirtilen aynı sabit disk sürücülerine sonraki kopyalama oturumları **için, aynı günlük dosya** adını belirtin ve yeni bir **oturum kimliği**sağlayın; depolama hesabı anahtarını sağlamaya gerek yoktur.

    ```
    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
    ```

Bir alma örneği aşağıda gösterilmiştir.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [İş ve sürücü durumunu görüntüleme](storage-import-export-view-drive-status.md)
* [İthalat/İhracat gereksinimlerini gözden geçirin](storage-import-export-requirements.md)
