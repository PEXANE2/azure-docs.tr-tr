---
title: Günlükleri kullanarak veri yüklemelerini giderme
titleSuffix: Azure Data Box Disk
description: Azure Veri Kutusu Diski'ne veri yüklerken görülen günlüklerin nasıl kullanılacağını ve sorunları gidermeyi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/17/2019
ms.author: alkohli
ms.openlocfilehash: 7c14988706ef193ef5da868c55f6c4f55e7d98f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260143"
---
# <a name="understand-logs-to-troubleshoot-data-upload-issues-in-azure-data-box-disk"></a>Azure Veri Kutusu Diski'ndeki veri yükleme sorunlarını gidermek için günlükleri anlama

Bu makale, Microsoft Azure Veri Kutusu Diski için geçerlidir ve Azure'a veri yüklerken gördüğünüz sorunları açıklar.

## <a name="about-upload-logs"></a>Yükleme günlükleri hakkında

Veriler veri merkezinde Azure'a yüklendiğinde `_error.xml` ve `_verbose.xml` her depolama hesabı için dosyalar oluşturulduğunda. Bu günlükler, veri yüklemek için kullanılan aynı depolama hesabına yüklenir. 

Her iki günlük de aynı biçimdedir ve verileri diskten Azure Depolama hesabına kopyalarken oluşan olayların XML açıklamalarını içerir.

Ayrıntılı günlük, her blob veya dosya için kopyalama işleminin durumu hakkında tam bilgi içerirken, hata günlüğü yalnızca yükleme sırasında hatalarla karşılaşan blobs veya dosyalar için bilgiler içerir.

Hata günlüğü, verbose günlüğüyle aynı yapıya sahiptir, ancak başarılı işlemleri filtreler.

## <a name="download-logs"></a>İndirme günlükleri

Yükleme günlüklerini bulmak için aşağıdaki adımları izleyin.

1. Verileri Azure'a yüklerken herhangi bir hata varsa, portal tanılama günlüklerinin bulunduğu klasöre giden bir yol görüntüler.

    ![Portaldaki günlüklere bağlantı](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs.png)

2. **Waies**gidin .

    ![hata ve ayrıntılı günlükleri](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs-1.png)

Her durumda, hata günlükleri ve ayrıntılı günlükleri bakın. Her günlük seçin ve yerel bir kopyasını indirin.

## <a name="sample-upload-logs"></a>Örnek yükleme günlükleri

Bir örnek `_verbose.xml` aşağıda gösterilmiştir. Bu durumda, sipariş hiçbir hata olmadan başarıyla tamamlandı.

```xml

<?xml version="1.0" encoding="utf-8"?>
<DriveLog Version="2018-10-01">
  <DriveId>184020D95632</DriveId>
  <Blob Status="Completed">
    <BlobPath>$root/botetapageblob.vhd</BlobPath>
    <FilePath>\PageBlob\botetapageblob.vhd</FilePath>
    <Length>1073742336</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <PageRangeList>
      <PageRange Offset="0" Length="4194304" Status="Completed" />
      <PageRange Offset="4194304" Length="4194304" Status="Completed" />
      <PageRange Offset="8388608" Length="4194304" Status="Completed" />
      --------CUT-------------------------------------------------------
      <PageRange Offset="1061158912" Length="4194304" Status="Completed" />
      <PageRange Offset="1065353216" Length="4194304" Status="Completed" />
      <PageRange Offset="1069547520" Length="4194304" Status="Completed" />
      <PageRange Offset="1073741824" Length="512" Status="Completed" />
    </PageRangeList>
  </Blob>
  <Blob Status="Completed">
    <BlobPath>$root/botetablockblob.txt</BlobPath>
    <FilePath>\BlockBlob\botetablockblob.txt</FilePath>
    <Length>19</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <BlockList>
      <Block Offset="0" Length="19" Status="Completed" />
    </BlockList>
  </Blob>
  <File Status="Completed">
    <FileStoragePath>botetaazurefilesfolder/botetaazurefiles.txt</FileStoragePath>
    <FilePath>\AzureFile\botetaazurefilesfolder\botetaazurefiles.txt</FilePath>
    <Length>20</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <FileRangeList>
      <FileRange Offset="0" Length="20" Status="Completed" />
    </FileRangeList>
  </File>
  <Status>Completed</Status>
</DriveLog>
```

Aynı sipariş için, aşağıda `_error.xml` bir örnek gösterilmiştir.

```xml

<?xml version="1.0" encoding="utf-8"?>
<DriveLog Version="2018-10-01">
  <DriveId>184020D95632</DriveId>
  <Summary>
    <ValidationErrors>
      <None Count="3" />
    </ValidationErrors>
    <CopyErrors>
      <None Count="3" Description="No errors encountered" />
    </CopyErrors>
  </Summary>
  <Status>Completed</Status>
</DriveLog>
```

Siparişin `_error.xml` hatalarla tamamlandığı aşağıda bir örnek gösterilmiştir. 

Bu durumda hata dosyası `Summary` bir bölüm ve tüm dosya düzeyi hataları içeren başka bir bölüm vardır. 

Içerir `Summary` `ValidationErrors` ve `CopyErrors`. Bu durumda, 8 dosya veya klasör Azure'a yüklendi ve doğrulama hatası yoktu. Veriler Azure Depolama hesabına kopyalandığında, 5 dosya veya klasör başarıyla yüklenir. Geri kalan 3 dosya veya klasör, Azure kapsayıcı adlandırma kurallarına göre yeniden adlandırıldı ve ardından Azure'a başarıyla yüklendi.

Dosya düzeyi durumu, `BlobStatus` lekeleri yüklemek için yapılan eylemleri açıklayan bir durum. Bu durumda, verilerin kopyalandığı klasörler kapsayıcılar için Azure adlandırma kurallarıyla uyumlu olmadığından üç kapsayıcı yeniden adlandırılır. Bu kapsayıcılara yüklenen lekeler için, yeni kapsayıcı adı, Azure'daki blob yolu, özgün geçersiz dosya yolu ve blob boyutu dahildir.
    
```xml
 <?xml version="1.0" encoding="utf-8"?>
    <DriveLog Version="2018-10-01">
      <DriveId>18041C582D7E</DriveId>
      <Summary>
     <!--Summary for validation and data copy to Azure -->
        <ValidationErrors>
          <None Count="8" />
        </ValidationErrors>
        <CopyErrors>
          <Completed Count="5" Description="No errors encountered" />
          <ContainerRenamed Count="3" Description="Renamed the container as the original container name does not follow Azure conventions." />
        </CopyErrors>
      </Summary>
    <!--List of renamed containers with the new names, new file path in Azure, original invalid file path, and size -->
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/private.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\private.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/resource.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\resource.vhd</OriginalFilePath>
        <SizeInBytes>71528448</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/role.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\role.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Status>CompletedWithErrors</Status>
    </DriveLog>
```

## <a name="data-upload-errors"></a>Veri yükleme hataları

Verileri Azure'a yüklerken oluşturulan hatalar aşağıdaki tabloda özetlenmiştir.

| Hata kodu | Açıklama                   |
|-------------|------------------------------|
|`None` |  Başarıyla tamamlandı.           |
|`Renamed` | Blob'un adını başarıyla aldı.   |
|`CompletedWithErrors` | Yükleme hataları ile tamamlandı. Hatalı dosyaların ayrıntıları günlük dosyasına dahildir.  |
|`Corrupted`|Veri alımı sırasında hesaplanan CRC, yükleme sırasında hesaplanan CRC ile eşleşmez.  |  
|`StorageRequestFailed` | Azure depolama isteği başarısız oldu.   |     
|`LeasePresent` | Bu öğe kiralanır ve başka bir kullanıcı tarafından kullanılmaktadır. |
|`StorageRequestForbidden` |Kimlik doğrulama sorunları nedeniyle yüklenemedi. |
|`ManagedDiskCreationTerminalFailure` | Yönetilen diskler olarak yükleyemedi. Dosyalar, evreleme depolama hesabında sayfa lekeleri olarak kullanılabilir. Sayfa lekelerini yönetilen disklere el ile dönüştürebilirsiniz.  |
|`DiskConversionNotStartedTierInfoMissing` | VHD dosyası önceden oluşturulmuş katman klasörlerinin dışında kopyalandığından, yönetilen bir disk oluşturulmadı. Dosya, sipariş oluşturma sırasında belirtildiği gibi hazırlama depolama hesabına sayfa blob olarak yüklenir. El ile yönetilen bir diske dönüştürebilirsiniz.|
|`InvalidWorkitem` | Azure adlandırmasına uygun olmadığı ve kuralları sınırlandırdığı için verileri yükleyemedi.|
|`InvalidPageBlobUploadAsBlockBlob` | Önek `databoxdisk-invalid-pb-`ile bir kapta blok lekeler olarak yüklenir.|
|`InvalidAzureFileUploadAsBlockBlob` | Önek `databoxdisk-invalid-af`ile bir kapta blok lekeler olarak yüklenir -.|
|`InvalidManagedDiskUploadAsBlockBlob` | Önek `databoxdisk-invalid-md`ile bir kapta blok lekeler olarak yüklenir -.|
|`InvalidManagedDiskUploadAsPageBlob` |Önek `databoxdisk-invalid-md-`ile bir kapta sayfa lekeleri olarak yüklenir. |
|`MovedToOverflowShare` |Özgün paylaşım boyutu maksimum Azure boyutu sınırını aştığında dosyaları yeni bir paylaşıma yükledi. Yeni dosya paylaşım adı ile suffixed `-2`orijinal adı vardır.   |
|`MovedToDefaultAzureShare` |Varsayılan bir paylaşıma herhangi bir klasörün parçası olmayan yüklenen dosyalar. Paylaşım adı ile `databox-`başlar. |
|`ContainerRenamed` |Bu dosyaların kapsayıcısı Azure adlandırma kurallarına uymadı ve yeniden adlandırıldı. Yeni ad ile `databox-` başlar ve orijinal adın SHA1 karma ile suffixed |
|`ShareRenamed` |Bu dosyaların paylaşımı Azure adlandırma kurallarına uymadı ve yeniden adlandırıldı. Yeni ad ile `databox-` başlar ve orijinal adın SHA1 karma ile suffixed olduğunu. |
|`BlobRenamed` |Bu dosyalar Azure adlandırma kurallarına uymadı ve yeniden adlandırıldı. Yeni `BlobPath` ad için alanı kontrol edin. |
|`FileRenamed` |Bu dosyalar Azure adlandırma kurallarına uymadı ve yeniden adlandırıldı. Yeni `FileStoragePath` ad için alanı kontrol edin. |
|`DiskRenamed` |Bu dosyalar Azure adlandırma kurallarına uymadı ve yeniden adlandırıldı. Yeni `BlobPath` ad için alanı kontrol edin. |


## <a name="next-steps"></a>Sonraki adımlar

- [Veri Kutusu Diski sorunları için destek bileti açın.](data-box-disk-contact-microsoft-support.md)
