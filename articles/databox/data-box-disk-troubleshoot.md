---
title: Azure Data Box Disk sorunlarını giderme | Microsoft Docs
description: Azure Data Box Disk sorunlarının nasıl giderileceği anlatılmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: f8116ec0836623adf803991017950ddc7f960923
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67805719"
---
# <a name="use-logs-to-troubleshoot-validation-issues-in-azure-data-box-disk"></a>Azure Veri Kutusu Diski'ndeki doğrulama sorunlarını gidermek için günlükleri kullanma

Bu makale, Microsoft Azure Veri Kutusu Diski için geçerlidir. Makalede, bu çözümü dağıttığınızda görebileceğiniz doğrulama sorunlarını gidermek için günlüklerin nasıl kullanılacağı açıklanmaktadır.

## <a name="validation-tool-log-files"></a>Doğrulama aracı günlük dosyaları

[Doğrulama aracını](data-box-disk-deploy-copy-data.md#validate-data)kullanarak disklerde verileri doğruladiğinizde, hataları günlüğe kaydetmek için bir *error.xml* oluşturulur. Günlük dosyası sürücünüzün `Drive:\DataBoxDiskImport\logs` klasöründe bulunur. Doğrulamayı çalıştırdığınızda hata günlüğüne bir bağlantı sağlanır.

<!--![Validation tool with link to error log](media/data-box-disk-troubleshoot/validation-tool-link-error-log.png)-->

Doğrulama için birden çok oturum çalıştırırsanız, oturum başına bir hata günlüğü oluşturulur.

- Burada `PageBlob` klasöre yüklenen veriler 512 bayt hizalanmış değil hata günlüğü bir örnektir. PageBlob'a yüklenen tüm veriler, örneğin VHD veya VHDX gibi 512 bayt hizalanmış olmalıdır. Bu dosyadaki `<Errors>` hatalar ve 'deki `<Warnings>`uyarılar.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <ErrorLog Version="2018-10-01">
            <SessionId>session#1</SessionId>
            <ItemType>PageBlob</ItemType>
            <SourceDirectory>D:\Dataset\TestDirectory</SourceDirectory>
            <Errors>
                <Error Code="Not512Aligned">
                    <Description>The file is not 512 bytes aligned.</Description>
                    <List>
                        <File Path="\Practice\myScript.ps1" />
                    </List>
                    <Count>1</Count>
                </Error>
            </Errors>
            <Warnings />
        </ErrorLog>
    ```

- Burada, kapsayıcı adı geçerli olmadığında hata günlüğünün bir örneği verem. Diskte oluşturduğunuz `BlockBlob` `PageBlob`klasör veya `AzureFile` klasörler Azure Depolama hesabınızda bir kapsayıcı haline gelir. Kapsayıcının adı [Azure adlandırma kurallarını](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)izlemeli.

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <ErrorLog Version="2018-10-01">
          <SessionId>bbsession</SessionId>
          <ItemType>BlockBlob</ItemType>
          <SourceDirectory>E:\BlockBlob</SourceDirectory>
          <Errors>
            <Error Code="InvalidShareContainerFormat">
              <List>
                <Container Name="Azu-reFile" />
                <Container Name="bbcont ainer1" />
              </List>
              <Count>2</Count>
            </Error>
          </Errors>
          <Warnings />
    </ErrorLog>
    ```

## <a name="validation-tool-errors"></a>Doğrulama aracı hataları

*Hata.xml'de* yer alan hatalar ile ilgili önerilen eylemler aşağıdaki tabloda özetlenmiştir.

| Hata kodu| Açıklama                       | Önerilen eylemler               |
|------------|--------------------------|-----------------------------------|
| `None` | Verileri başarıyla doğruladı. | İşlem yapmanız gerekmez. |
| `InvalidXmlCharsInPath` |Dosya yolu geçerli olmayan karakterler ekibe sahip olduğundan bir bildirim dosyası oluşturulamadı. | Devam etmek için bu karakterleri kaldırın.  |
| `OpenFileForReadFailed`| Dosyayı işleyemedi. Bunun nedeni bir erişim sorunu veya dosya sistemi bozulması olabilir.|Bir hata nedeniyle dosyayı okuyamadı. Hata ayrıntıları özel durum bulunmaktadır. |
| `Not512Aligned` | Bu dosya PageBlob klasörü için geçerli bir biçimde değildir.| Yalnızca klasöre `PageBlob` hizalanmış 512 baytlık verileri yükleyin. Dosyayı PageBlob klasöründen çıkarın veya BlockBlob klasörüne taşıyın. Doğrulamayı yeniden deneyin.|
| `InvalidBlobPath` | Dosya yolu, Azure Blob adlandırma kurallarına göre bulutta geçerli bir blob yoluna eş değildir.|Dosya yolunu yeniden adlandırmak için Azure adlandırma yönergelerini izleyin. |
| `EnumerationError` | Doğrulama için dosyayı sayısaldı. |Bu hatanın birden çok nedeni olabilir. En olası neden dosyaya erişimdir. |
| `ShareSizeExceeded` | Bu dosya, Azure dosya paylaşım boyutunun 5 TB'lik Azure sınırını aşmasını neden oldu.|[Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uyacak şekilde paylaşımdaki verilerin boyutunu küçültün. Doğrulamayı yeniden deneyin. |
| `AzureFileSizeExceeded` | Dosya boyutu Azure Dosya boyutu sınırlarını aşıyor.| Dosyanın veya verilerin boyutunu [Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uyacak şekilde küçültün. Doğrulamayı yeniden deneyin.|
| `BlockBlobSizeExceeded` | Dosya boyutu Azure Blok Blob boyut sınırlarını aşıyor. | Dosyanın veya verilerin boyutunu [Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uyacak şekilde küçültün. Doğrulamayı yeniden deneyin. |
| `ManagedDiskSizeExceeded` | Dosya boyutu Azure Yönetilen Disk boyutu sınırlarını aşıyor. | Dosyanın veya verilerin boyutunu [Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uyacak şekilde küçültün. Doğrulamayı yeniden deneyin. |
| `PageBlobSizeExceeded` | Dosya boyutu Azure Yönetilen Disk boyutu sınırlarını aşıyor. | Dosyanın veya verilerin boyutunu [Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uyacak şekilde küçültün. Doğrulamayı yeniden deneyin. |
| `InvalidShareContainerFormat`  |Dizin adları, kapsayıcılar veya paylaşımlar için Azure adlandırma kurallarına uymaz.         |Diskte önceden varolan klasörlerin altında oluşturulan ilk klasör, depolama hesabınızda bir kapsayıcı olur. Bu paylaşım veya kapsayıcı adı Azure adlandırma kurallarına uymuyor. Dosyayı [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uyacak şekilde yeniden adlandırın. Doğrulamayı yeniden deneyin.   |
| `InvalidBlobNameFormat` | Dosya yolu, Azure Blob adlandırma kurallarına göre bulutta geçerli bir blob yoluna eş değildir.|Dosyayı [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uyacak şekilde yeniden adlandırın. Doğrulamayı yeniden deneyin. |
| `InvalidFileNameFormat` | Dosya yolu, Azure Dosya adlandırma kurallarına göre bulutta geçerli bir dosya yoluna eş değildir. |Dosyayı [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uyacak şekilde yeniden adlandırın. Doğrulamayı yeniden deneyin. |
| `InvalidDiskNameFormat` | Dosya yolu, Azure Yönetilen Disk adlandırma kurallarına göre bulutta geçerli bir disk adıile eşleşemez. |Dosyayı [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uyacak şekilde yeniden adlandırın. Doğrulamayı yeniden deneyin.       |
| `NotPartOfFileShare` | Dosyalar için yükleme yolu geçerli değildir. Dosyaları Azure Dosyaları'ndaki bir klasöre yükleyin.   | Dosyaları yanlışlıkla kaldırın ve bu dosyaları önceden oluşturulmuş bir klasöre yükleyin. Doğrulamayı yeniden deneyin. |
| `NonVhdFileNotSupportedForManagedDisk` | VHD olmayan bir dosya yönetilen disk olarak yüklenemez. |VHD olmayan dosyaları desteklenmedikçe klasörden `ManagedDisk` kaldırın veya bu `PageBlob` dosyaları bir klasöre taşıyın. Doğrulamayı yeniden deneyin. |


## <a name="next-steps"></a>Sonraki adımlar

- Sorun giderme [veri yükleme hataları](data-box-disk-troubleshoot-upload.md).
