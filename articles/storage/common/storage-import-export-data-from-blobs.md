---
title: Azure Blobs'tan veri aktarmak için Azure İçe Aktarma/Verme kullanma | Microsoft Dokümanlar
description: Azure Blobs'tan veri aktarmak için Azure portalında dışa aktarma işleri oluşturmayı öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 03/12/2020
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: a5afa6439caa6b7c1572447e3b212f3357bf296a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282520"
---
# <a name="use-the-azure-importexport-service-to-export-data-from-azure-blob-storage"></a>Azure Blob depolamadan verileri dışarı aktarmak için Azure İçeri/Dışarı Aktarma hizmetini kullanma

Bu makalede, Azure Blob depolamadan büyük miktarda veriyi güvenli bir şekilde dışa aktarmak için Azure İçe Alma/Dışa Aktarma hizmetinin nasıl kullanılacağına ilişkin adım adım yönergeler verilmektedir. Hizmet, boş sürücüleri Azure veri merkezine göndermenizi gerektirir. Hizmet, depolama hesabınızdaki verileri sürücülere aktarır ve sürücüleri geri yönlendirir.

## <a name="prerequisites"></a>Ön koşullar

Verileri Azure Blob Depolama'dan aktarmak için bir dışa aktarma işi oluşturmadan önce, bu hizmet için aşağıdaki ön koşullar listesini dikkatle gözden geçirin ve tamamlayın.
Şunları yapmalısın:

- İçe Aktarma/Dışa Aktarma hizmeti için kullanılabilecek etkin bir Azure aboneliğine sahip olun.
- En az bir Azure Depolama hesabınız var. [İçe Aktarma/Dışa Aktarma hizmeti için Desteklenen depolama hesapları nın ve depolama türlerinin listesine](storage-import-export-requirements.md)bakın. Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için [bkz.](storage-account-create.md)
- [Desteklenen türlerin](storage-import-export-requirements.md#supported-disks)disksayısı yeterli sayıda var.
- Bir FedEx/DHL hesabınız var. FedEx/DHL dışında bir operatör kullanmak istiyorsanız, 'den Azure `adbops@microsoft.com`Veri Kutusu İşlemleri ekibine başvurun.
  - Hesap geçerli olmalı, bakiyeye sahip olmalı ve sevkiyat alabilmeli.
  - Dışa aktarma işi için bir izleme numarası oluşturun.
  - Her işin ayrı bir izleme numarası olmalıdır. Aynı izleme numarasına sahip birden çok iş desteklenmez.
  - Operatör hesabınız yoksa, şu na gidin:
    - [Bir FedEX hesabı oluşturun](https://www.fedex.com/en-us/create-account.html)veya
    - [Bir DHL hesabı oluşturun.](http://www.dhl-usa.com/en/express/shipping/open_account.html)

## <a name="step-1-create-an-export-job"></a>Adım 1: Bir dışa aktarma işi oluşturma

Azure portalında bir dışa aktarma işi oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Oturum https://portal.azure.com/aç.
2. Tüm **hizmetlere gidin > Depolama > İthalat/Dışa Aktarma işleri.**

    ![Alma/dışa aktarma işlerine gitme](./media/storage-import-export-data-from-blobs/export-from-blob1.png)

3. **İçe Alma/Dışa Aktar İşi Oluştur'u**tıklatın.

    ![İçe/Dışa Aktar/Dışa Aktarma işini tıklatın](./media/storage-import-export-data-from-blobs/export-from-blob2.png)

4. **Temel olarak:**

    - **Azure'dan Dışa Aktar'ı**seçin.
    - Dışa aktarma işi için açıklayıcı bir ad girin. İşlerinizin ilerlemesini izlemek için seçtiğiniz adı kullanın.
        - Ad yalnızca küçük harfler, sayılar, tireler ve alt çizerler içerebilir.
        - Ad bir harfle başlamalı ve boşluk içermeyebilir.
    - Bir abonelik seçin.
    - Bir kaynak grubu girin veya seçin.

        ![Temel Bilgiler](./media/storage-import-export-data-from-blobs/export-from-blob3.png)

5. **İş ayrıntıları:**

    - Dışa aktarılacak verilerin bulunduğu depolama hesabını seçin. Bulunduğunuz yere yakın bir depolama hesabı kullanın.
    - Açılan konumu, seçilen depolama hesabının bölgesine bağlı olarak otomatik olarak doldurulur.
    - Depolama hesabınızdan boş sürücünüze veya sürücülerinize aktarmak istediğiniz blob verilerini belirtin.
    - Depolama hesabındaki tüm blob verilerini **dışa** aktarmayı seçin.

         ![Tümlerini dışa aktar](./media/storage-import-export-data-from-blobs/export-from-blob4.png)

    - Hangi kapların ve lekelerin dışa aktarılamasını belirtebilirsiniz.
        - **Dışa aktarmak için bir blob belirtmek için**: **Eşit To** seçiciyi kullanın. Kapsayıcı adı ile başlayan blob göreli yolu belirtin. Kök kapsayıcısını belirtmek için *$root* kullanın.
        - **Önek ile başlayan tüm lekeleri belirtmek için**: Seçici **ile Başlar'ı** kullanın. Öne eğik çizgi '/' ile başlayan önek belirtin. Önek, kapsayıcı adının öneki, tam kapsayıcı adı veya blob adının önekinin ardından gelen tam kapsayıcı adı olabilir. Bu ekran görüntüsünde gösterildiği gibi, işleme sırasında hataları önlemek için geçerli biçiminde blob yolları sağlamanız gerekir. Daha fazla bilgi için geçerli [blob yolları örneklerine](#examples-of-valid-blob-paths)bakın.

           ![Seçili kapları ve lekeleri dışa aktarma](./media/storage-import-export-data-from-blobs/export-from-blob5.png)

    - Blob listesi dosyasından dışa aktarabilirsiniz.

        ![Blob listesi dosyasından dışa aktarma](./media/storage-import-export-data-from-blobs/export-from-blob6.png)  

   > [!NOTE]
   > Dışa aktarılacak blob veri kopyalama sırasında kullanılıyorsa, Azure İçe Alma/Dışa Aktarma hizmeti blob'un anlık görüntüsünü alır ve anlık görüntünün kopyalanır.

6. **Karşılığında nakliye bilgileri**:

    - Açılan listeden taşıyıcıyı seçin. FedEx/DHL dışında bir taşıyıcı kullanmak istiyorsanız, açılır yerden mevcut bir seçenek seçin. Kullanmayı planladığınız `adbops@microsoft.com` operatörle ilgili bilgilerle birlikte Azure Veri Kutusu İşlemleri ekibine başvurun.
    - Bu taşıyıcıyla oluşturduğunuz geçerli bir taşıyıcı hesap numarasını girin. Microsoft, dışa aktarma işiniz tamamlandıktan sonra sürücüleri size geri sevk etmek için bu hesabı kullanır.
    - Tam ve geçerli bir iletişim adı, telefon, e-posta, sokak adresi, şehir, zip, eyalet/ il ve ülke/bölge sağlayın.

        > [!TIP]
        > Tek bir kullanıcı için e-posta adresi belirtmek yerine, grup e-postası sağlayın. Bu, bir yönetici ayrılsa bile bildirim almanızı sağlar.

7. **Özetle**:

    - İşin ayrıntılarını gözden geçirin.
    - İş adını not alın ve diskleri Azure'a sevk etmek için Azure veri merkezi gönderi adresi sağladı.

        > [!NOTE]
        > Diskleri her zaman Azure portalında belirtilen veri merkezine gönderin. Diskler yanlış veri merkezine gönderilirse, iş işlenmez.

    - Dışa aktarma iş oluşturmayı tamamlamak için **Tamam'ı** tıklatın.

<!--## (Optional) Step 2: -->

## <a name="step-2-ship-the-drives"></a>Adım 2: Sürücüleri gönderme

İhtiyacınız olan sürücü sayısını bilmiyorsanız, sürücü [sayısını kontrol](#check-the-number-of-drives)edin. Sürücülerin sayısını biliyorsanız, sürücüleri sevk etmeye devam edin.

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-3-update-the-job-with-tracking-information"></a>Adım 3: İzleme bilgileriyle işi güncelleştirin

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-4-receive-the-disks"></a>Adım 4: Diskleri alma

Pano iş tamamlandığında, diskler size gönderilir ve gönderinin takip numarası portalda bulunur.

1. Dışa aktarılan verileri içeren sürücüleri aldıktan sonra, sürücülerin kilidini açmak için BitLocker anahtarlarını almanız gerekir. Azure portalındaki dışa aktarma işine gidin. **İçe/Aktar sekmesini** tıklatın.
2. Listeden dışa aktarma işinizi seçin ve tıklatın. **Şifreleme'ye** git ve anahtarları kopyala.

   ![Dışa aktarma işi için BitLocker anahtarlarını görüntüleme](./media/storage-import-export-service/export-job-bitlocker-keys-02.png)

3. Disklerin kilidini açmak için BitLocker tuşlarını kullanın.

İhracat tamamlandı.

## <a name="step-5-unlock-the-disks"></a>Adım 5: Disklerin kilidini açın

WAImportExport aracının 1.4.0.300 sürümünü kullanıyorsanız, sürücünün kilidini açmak için aşağıdaki komutu kullanın:

    `WAImportExport Unlock /externalKey:<BitLocker key (base 64 string) copied from journal (*.jrn*) file>`  

Aracın önceki sürümlerini kullanıyorsanız, sürücünün kilidini açmak için BitLocker iletişim kutusunu kullanın.

Şu anda, işi silebilir veya bırakabilirsiniz. İşler 90 gün sonra otomatik olarak silinir.

## <a name="check-the-number-of-drives"></a>Sürücülerin sayısını kontrol edin

Bu *isteğe bağlı* adım, dışa aktarma işi için gereken sürücü sayısını belirlemenize yardımcı olur. Desteklenen işletim sistemi sürümünü çalıştıran bir Windows sisteminde bu adımı [gerçekleştirin.](storage-import-export-requirements.md#supported-operating-systems)

1. [WAImportExport sürüm 1'i](https://www.microsoft.com/download/details.aspx?id=42659) Windows sisteminden indirin.
2. Varsayılan klasöre `waimportexportv1`unzip . Örneğin, `C:\WaImportExportV1`.
3. Yönetim ayrıcalıklarına sahip bir PowerShell veya komut satırı penceresi açın. Dizini gün önce gün önce gün önce gün önce pişmemiş klasöre değiştirmek için aşağıdaki komutu çalıştırın:

    `cd C:\WaImportExportV1`

4. Seçili lekeler için gereken disk sayısını denetlemek için aşağıdaki komutu çalıştırın:

    `WAImportExport.exe PreviewExport /sn:<Storage account name> /sk:<Storage account key> /ExportBlobListFile:<Path to XML blob list file> /DriveSize:<Size of drives used>`

    Parametreler aşağıdaki tabloda açıklanmıştır:

    |Komut satırı parametresi|Açıklama|  
    |--------------------------|-----------------|  
    |**/logdir:**|İsteğe bağlı. Günlük dizini. Verbose günlük dosyaları bu dizine yazılır. Belirtilmemişse, geçerli dizin günlük dizini olarak kullanılır.|  
    |**/sn:**|Gereklidir. Dışa aktarma işi için depolama hesabının adı.|  
    |**/sk:**|Yalnızca bir kapsayıcı SAS belirtilmemişse gereklidir. Dışa aktarma işinin depolama hesabının hesap anahtarı.|  
    |**/csas:**|Yalnızca bir depolama hesabı anahtarı belirtilmemişse gereklidir. İhracat işinde dışa aktarılacak lekeleri listelemek için konteyner SAS.|  
    |**/İhracatBlobListFile:**|Gereklidir. Dışa aktarılacak blob sevesleri veya blob yolu önekleri listesini içeren XML dosyasına giden yol. Alma/Dışa Aktar `BlobListBlobPath` hizmeti REST API'nin İş Icabı'nda öğede kullanılan dosya biçimi. [Put Job](/rest/api/storageimportexport/jobs)|  
    |**/DriveSize:**|Gereklidir. Bir dışa aktarma işi için kullanılacak sürücülerin boyutu, *örneğin,* 500 GB, 1,5 TB.|  

    [PreviewExport komutunun](#example-of-previewexport-command)bir örneğine bakın.

5. Dışa aktarma işi için sevk edilecek sürücülere okuma/yazma yapabileceğinizden kontrol edin.

### <a name="example-of-previewexport-command"></a>PreviewExport komutu örneği

Aşağıdaki örnek komutu `PreviewExport` gösterir:  

```powershell
    WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB
```  

Dışa aktarma blob listesi dosyası, burada gösterildiği gibi blob adları ve blob önekleri içerebilir:  

```xml
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

Azure İçe Alma/Dışa Aktarma Aracı, dışa aktarılacak tüm lekeleri listeler ve gerekli ek yükü dikkate alarak bunları belirtilen boyuttaki sürücülere nasıl paketleeceğini hesaplar, ardından lekeleri tutmak ve kullanım bilgilerini kullanmak için gereken sürücü sayısını tahmin eder.  

Aşağıda, bilgi günlüklerinin atlanan çıktıya bir örneği verilmiştir:  

```powershell
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  

Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB
```

## <a name="examples-of-valid-blob-paths"></a>Geçerli blob yolları örnekleri

Aşağıdaki tablo, geçerli blob yollarının örneklerini gösterir:

   | Seçici | Blob Yolu | Açıklama |
   | --- | --- | --- |
   | Ile Başlar |/ |Depolama hesabındaki tüm lekeleri dışa aktarma |
   | Ile Başlar |/$root/ |Kök kabındaki tüm lekeleri dışa aktarma |
   | Ile Başlar |/kitap |Önek **defteriyle** başlayan herhangi bir kaptaki tüm lekeleri dışa aktarma |
   | Ile Başlar |/müzik/ |Konteyner **müzikteki** tüm lekeleri dışa aktarma |
   | Ile Başlar |/müzik/aşk |Önek **aşk** ile başlayan konteyner **müzik** tüm lekeler ihrac |
   | Eşit |$root/logo.bmp |Kök konteyneriçinde blob **logo.bmp** dışa aktarma |
   | Eşit |videolar/hikaye.mp4 |Konteyner **videoları** damla **hikaye.mp4** İhracat |

## <a name="next-steps"></a>Sonraki adımlar

- [İş ve sürücü durumunu görüntüleme](storage-import-export-view-drive-status.md)
- [İthalat/İhracat gereksinimlerini gözden geçirin](storage-import-export-requirements.md)
