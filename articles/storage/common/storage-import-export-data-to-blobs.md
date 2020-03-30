---
title: Verileri Azure Blobs'a aktarmak için Azure İçe Aktarma/Verme kullanma | Microsoft Dokümanlar
description: Azure Blobs'a veri aktarmak için Azure portalında nasıl içe alma ve dışa aktarma işleri oluşturup aktarılamayı öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 03/12/2020
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 570c663861361a19190f6fb5d608b6aa029a0885
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282503"
---
# <a name="use-the-azure-importexport-service-to-import-data-to-azure-blob-storage"></a>Verileri Azure Blob Depolamasına aktarmak için Azure İçe Aktarma/Verme hizmetini kullanın

Bu makalede, Azure Blob depolama alanına büyük miktarda veri güvenli bir şekilde almak için Azure İçe Alma/Dışa Aktarma hizmetinin nasıl kullanılacağına ilişkin adım adım yönergeler verilmektedir. Azure Blobs'a veri aktarmak için hizmet, verilerinizi içeren şifreli disk sürücülerini bir Azure veri merkezine göndermenizi gerektirir.  

## <a name="prerequisites"></a>Ön koşullar

Verileri Azure Blob Depolama'ya aktarmak için bir alma işi oluşturmadan önce, bu hizmet için aşağıdaki ön koşullar listesini dikkatle gözden geçirin ve tamamlayın.
Şunları yapmalısın:

* İçe Aktarma/Dışa Aktarma hizmeti için kullanılabilecek etkin bir Azure aboneliğine sahip olun.
* Depolama kapsayıcısıyla birlikte en az bir Azure Depolama hesabınız var. [İçe Aktarma/Dışa Aktarma hizmeti için Desteklenen depolama hesapları nın ve depolama türlerinin listesine](storage-import-export-requirements.md)bakın.
  * Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için [bkz.](storage-account-create.md)
  * Depolama kapsayıcısı hakkında bilgi için [bir depolama kapsayıcısı oluştur'a](../blobs/storage-quickstart-blobs-portal.md#create-a-container)gidin.
* [Desteklenen türlerin](storage-import-export-requirements.md#supported-disks)disksayısı yeterli sayıda var.
* Desteklenen işletim sistemi [sürümünü](storage-import-export-requirements.md#supported-operating-systems)çalıştıran bir Windows sistemine sahip.
* Windows sisteminde BitLocker'ı etkinleştirin. [Bkz. BitLocker'ı etkinleştirme.](https://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/)
* Windows sisteminde [en son WAImportExport sürüm 1 indirin.](https://www.microsoft.com/download/details.aspx?id=42659) Aracın en son sürümü, BitLocker anahtarı için harici bir koruyucuya ve güncelleştirilmiş kilit açma modu özelliğine izin vermek için güvenlik güncelleştirmelerine sahiptir.

  * Varsayılan klasöre `waimportexportv1`unzip . Örneğin, `C:\WaImportExportV1`.
* Bir FedEx/DHL hesabınız var. FedEx/DHL dışında bir operatör kullanmak istiyorsanız, 'den Azure `adbops@microsoft.com`Veri Kutusu İşlemleri ekibine başvurun.  
  * Hesap geçerli olmalı, bakiyeye sahip olmalı ve sevkiyat alabilmeli.
  * Dışa aktarma işi için bir izleme numarası oluşturun.
  * Her işin ayrı bir izleme numarası olmalıdır. Aynı izleme numarasına sahip birden çok iş desteklenmez.
  * Operatör hesabınız yoksa, şu na gidin:
    * [Bir FedEX hesabı oluşturun](https://www.fedex.com/en-us/create-account.html)veya
    * [Bir DHL hesabı oluşturun.](http://www.dhl-usa.com/en/express/shipping/open_account.html)

## <a name="step-1-prepare-the-drives"></a>Adım 1: Sürücüleri hazırlama

Bu adım bir günlük dosyası oluşturur. Günlük dosyası sürücü seri numarası, şifreleme anahtarı ve depolama hesabı ayrıntıları gibi temel bilgileri depolar.

Sürücüleri hazırlamak için aşağıdaki adımları gerçekleştirin.

1. Disk sürücülerinizi SATA konektörleri aracılığıyla Windows sistemine bağlayın.
2. Her sürücüde tek bir NTFS hacmi oluşturun. Ses birimine bir sürücü mektubu atayın. Montaj noktalarını kullanmayın.
3. NTFS biriminde BitLocker şifrelemesini etkinleştirin. Windows Server sistemi kullanıyorsanız, [Windows Server 2012 R2'de BitLocker'ı etkinleştirme](https://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/)yönergelerini kullanın.
4. Verileri şifreli hacime kopyalayın. Sürükle ve bırak veya Robocopy veya bu tür kopyalama aracı kullanın. Bir günlük (*.jrn*) dosyası aracı çalıştırdığınız klasörde oluşturulur.

   Sürücü kilitliyse ve sürücünün kilidini açmanız gerekiyorsa, kilidini açma adımları kullanım durumunuza bağlı olarak farklı olabilir.

   * Önceden şifrelenmiş bir sürücüye veri eklediyseniz (WAImportExport aracı şifreleme için kullanılmadı), sürücünün kilidini açmak için açılır penceredeki BitLocker anahtarını (belirttiğiniz sayısal parola) kullanın.

   * WAImportExport aracı tarafından şifrelenmiş bir sürücüye veri eklediyseniz, sürücünün kilidini açmak için aşağıdaki komutu kullanın:

        `WAImportExport Unlock /externalKey:<BitLocker key (base 64 string) copied from journal (*.jrn*) file>`

5. Yönetim ayrıcalıklarına sahip bir PowerShell veya komut satırı penceresi açın. Dizini gün önce gün önce gün önce gün önce pişmemiş klasöre değiştirmek için aşağıdaki komutu çalıştırın:

    `cd C:\WaImportExportV1`
6. Sürücünün BitLocker tuşunu almak için aşağıdaki komutu çalıştırın:

    `manage-bde -protectors -get <DriveLetter>:`
7. Diski hazırlamak için aşağıdaki komutu çalıştırın. **Veri boyutuna bağlı olarak, bu işlem birkaç saat ile günler arasında sürebilir.**

    ```powershell
    ./WAImportExport.exe PrepImport /j:<journal file name> /id:session#<session number> /t:<Drive letter> /bk:<BitLocker key> /srcdir:<Drive letter>:\ /dstdir:<Container name>/ /blobtype:<BlockBlob or PageBlob> /skipwrite
    ```

    Aracı çalıştırdığınız klasörde bir günlük dosyası oluşturulur. Diğer iki dosya da oluşturulur - bir *.xml* dosyası (aracı çalıştırdığınız klasör) ve bir *drive-manifest.xml* dosyası (verilerin bulunduğu klasör).

    Kullanılan parametreler aşağıdaki tabloda açıklanmıştır:

    |Seçenek  |Açıklama  |
    |---------|---------|
    |/j:     |.jrn uzantılı günlük dosyasının adı. Sürücü başına bir günlük dosyası oluşturulur. Disk seri numarasını günlük dosya adı olarak kullanmanızı öneririz.         |
    |/id:     |Oturum kimliği. Komutun her örneği için benzersiz bir oturum numarası kullanın.      |
    |/t:     |Sevk edilecek diskin sürücü harfi. Örneğin, sürücü `D`.         |
    |/bk:     |Sürücü için BitLocker tuşu. Çıktısından sayısal şifresi`manage-bde -protectors -get D:`      |
    |/srcdir:     |Sevk edilecek diskin sürücü harfi `:\`takip. Örneğin, `D:\`.         |
    |/dstdir:     |Azure Depolama'daki hedef kapsayıcının adı.         |
    |/blobtype:     |Bu seçenek, verileri almak istediğiniz blobs türünü belirtir. Blok lekeler için, bu `BlockBlob` ve sayfa lekeleri için, öyle. `PageBlob`         |
    |/skipwrite:     |Kopyalanacak yeni veri olmadığını ve diskteki varolan verilerin hazırlanabilmek için gerekli olduğunu belirten seçenek.          |
    |/enablecontentmd5:     |Etkinleştirildiğinde seçenek, MD5'in hesaplandığından ve `Content-md5` her blob'da özellik olarak ayarlandığından emin olun. Bu seçeneği yalnızca veriler Azure'a yüklendikten sonra `Content-md5` alanı kullanmak istiyorsanız kullanın. <br> Bu seçenek veri bütünlüğü denetimini etkilemez (varsayılan olarak oluşur). Ayar, buluta veri yüklemek için geçen süreyi artırır.          |
8. Sevk edilmesi gereken her disk için önceki adımı yineleyin. Komut satırının her çalışması için sağlanan ada sahip bir günlük dosyası oluşturulur.

    > [!IMPORTANT]
    > * Günlük dosyasıyla birlikte, `<Journal file name>_DriveInfo_<Drive serial ID>.xml` aracın bulunduğu klasörde de bir dosya oluşturulur. .xml dosyası, günlük dosyası çok büyükse, iş oluştururken günlük dosyasının yerine kullanılır.

## <a name="step-2-create-an-import-job"></a>Adım 2: Bir alma işi oluşturma

Azure portalında bir alma işi oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Oturum https://portal.azure.com/aç.
2. Tüm **hizmetlere gidin > Depolama > İthalat/Dışa Aktarma işleri.**

    ![Alma/dışa aktarma işlerine gitme](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. **İçe Alma/Dışa Aktar İşi Oluştur'u**tıklatın.

    ![İçe Aktarma/Dışa Aktarma işi oluştur'u tıklatın](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. **Temel olarak:**

   * **Azure'a Aktar'ı**seçin.
   * Alma işi için açıklayıcı bir ad girin. İşlerinizin ilerlemesini izlemek için adı kullanın.
       * Ad yalnızca küçük harfler, sayılar ve tireler içerebilir.
       * Ad bir harfle başlamalı ve boşluk içermeyebilir.
   * Bir abonelik seçin.
   * Bir kaynak grubu girin veya seçin.  

     ![Alma işi oluşturma - Adım 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

5. **İş ayrıntıları:**

   * Sürücü hazırlama adımı sırasında elde ettiğiniz sürücü günlüğü dosyalarını yükleyin. Kullanılmışsa, `waimportexport.exe version1` hazırladığınız her sürücü için bir dosya yükleyin. Günlük dosya boyutu 2 MB'ı aşarsa, `<Journal file name>_DriveInfo_<Drive serial ID>.xml` günlük dosyasıyla birlikte oluşturulan da kullanabilirsiniz.
   * Verilerin bulunduğu hedef depolama hesabını seçin.
   * Açılan konumu, seçilen depolama hesabının bölgesine bağlı olarak otomatik olarak doldurulur.

   ![Alma işi oluşturma - Adım 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

6. **Karşılığında nakliye bilgileri**:

   * Açılan listeden taşıyıcıyı seçin. FedEx/DHL dışında bir taşıyıcı kullanmak istiyorsanız, açılır yerden mevcut bir seçenek seçin. Kullanmayı planladığınız `adbops@microsoft.com` operatörle ilgili bilgilerle birlikte Azure Veri Kutusu İşlemleri ekibine başvurun.
   * Bu taşıyıcıyla oluşturduğunuz geçerli bir taşıyıcı hesap numarasını girin. Microsoft, alma işiniz tamamlandıktan sonra sürücüleri size geri sevk etmek için bu hesabı kullanır. Hesap numaranız yoksa, bir [FedEx](https://www.fedex.com/us/oadr/) veya [DHL](https://www.dhl.com/) taşıyıcı hesabı oluşturun.
   * Tam ve geçerli bir iletişim adı, telefon, e-posta, sokak adresi, şehir, zip, eyalet/ il ve ülke/bölge sağlayın.

       > [!TIP]
       > Tek bir kullanıcı için e-posta adresi belirtmek yerine, grup e-postası sağlayın. Bu, bir yönetici ayrılsa bile bildirim almanızı sağlar.

     ![Alma işi oluşturma - Adım 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)

7. **Özetle**:

   * Özette sağlanan iş bilgilerini gözden geçirin. Diskleri Azure'a geri sevk etmek için iş adını ve Azure veri merkezi gönderi adresini not alın. Bu bilgiler daha sonra sevkiyat etiketinde kullanılır.
   * Alma işini oluşturmak için **Tamam'ı** tıklatın.

     ![Alma işi oluşturma - Adım 4](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-optional-configure-customer-managed-key"></a>Adım 3 (İsteğe bağlı): Müşteri yönetilen anahtarını yapılandırma

Sürücüler için BitLocker tuşlarınızı korumak için Microsoft yönetilen anahtarını kullanmak istiyorsanız bu adımı atlayın ve bir sonraki adıma geçin. BitLocker anahtarını korumak için kendi anahtarınızı yapılandırmak [için, Azure portalında Azure İçe Alma/Dışa Aktarma için Azure Anahtar Kasası ile müşteri tarafından yönetilen tuşları yapılandırma yönergelerini](storage-import-export-encryption-key-portal.md) izleyin

## <a name="step-4-ship-the-drives"></a>Adım 4: Sürücüleri gönderme

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-5-update-the-job-with-tracking-information"></a>Adım 5: İzleme bilgileriyle işi güncelleştirin

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-6-verify-data-upload-to-azure"></a>Adım 6: Azure'a veri yüklemesini doğrulayın

İşi tamamlayakadar izleyin. İş tamamlandıktan sonra verilerinizin Azure'a yüklendiğini doğrulayın. Şirket içi verileri yalnızca yüklemenin başarılı olduğunu doğruladıktan sonra silin.

## <a name="next-steps"></a>Sonraki adımlar

* [İş ve sürücü durumunu görüntüleme](storage-import-export-view-drive-status.md)
* [İthalat/İhracat gereksinimlerini gözden geçirin](storage-import-export-requirements.md)
