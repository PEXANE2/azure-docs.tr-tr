---
title: Azure Data Box Disk sorunlarını giderme | Microsoft Docs
description: Azure Data Box Disk dağıtırken oluşabilecek doğrulama sorunlarını gidermek için günlükleri nasıl kullanacağınızı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: troubleshooting
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: 1c1b38c4021660b9f59098f8442d16bfd0ecc582
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87925551"
---
# <a name="use-logs-to-troubleshoot-validation-issues-in-azure-data-box-disk"></a>Azure Data Box Disk doğrulama sorunlarını gidermek için günlükleri kullanma

Bu makale, Microsoft Azure Data Box Disk için geçerlidir. Makalesinde, bu çözümü dağıtırken görebileceğiniz doğrulama sorunlarını gidermek için günlüklerin nasıl kullanılacağı açıklanır.

## <a name="validation-tool-log-files"></a>Doğrulama aracı günlük dosyaları

[Doğrulama aracını](data-box-disk-deploy-copy-data.md#validate-data)kullanarak disklerdeki verileri doğruladığınızda, tüm hataları günlüğe kaydetmek için bir *error.xml* oluşturulur. Günlük dosyası `Drive:\DataBoxDiskImport\logs` sürücünüzün klasöründe bulunur. Doğrulama çalıştırdığınızda hata günlüğüne bir bağlantı sağlanır.

<!--![Validation tool with link to error log](media/data-box-disk-troubleshoot/validation-tool-link-error-log.png)-->

Doğrulama için birden çok oturum çalıştırırsanız, oturum başına bir hata günlüğü oluşturulur.

- Aşağıda, klasöre yüklenen veriler 512 bayt hizalı olmadığında oluşan hata günlüğü örneği verilmiştir `PageBlob` . PageBlob 'a yüklenen tüm veriler 512 bayt hizalı olmalıdır; örneğin, bir VHD veya VHDX. Bu dosyadaki hatalar `<Errors>` içindeki ve uyarılardır `<Warnings>` .

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

- Kapsayıcı adı geçerli olmadığında bir hata günlüğü örneği aşağıda verilmiştir. `BlockBlob`, `PageBlob` , Veya diskteki klasörler altında oluşturduğunuz klasör, `AzureFile` Azure depolama hesabınızda bir kapsayıcı haline gelir. Kapsayıcının adı [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uymalıdır.

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

*error.xml* bulunan hatalar, ilgili önerilen eylemlerle birlikte aşağıdaki tabloda özetlenmiştir.

| Hata kodu| Description                       | Önerilen Eylemler               |
|------------|--------------------------|-----------------------------------|
| `None` | Veriler başarıyla doğrulanmadı. | İşlem yapmanız gerekmez. |
| `InvalidXmlCharsInPath` |Dosya yolu geçersiz karakterler içerdiğinden bildirim dosyası oluşturulamadı. | Devam etmek için bu karakterleri kaldırın.  |
| `OpenFileForReadFailed`| Dosya işlenemedi. Bunun nedeni, bir erişim sorunu veya dosya sistemi Bozulması olabilir.|Bir hata nedeniyle dosya okunamadı. Hata ayrıntıları özel durumdur. |
| `Not512Aligned` | Bu dosya PageBlob klasörü için geçerli bir biçimde değil.| Yalnızca 512 bayt hizalı verileri `PageBlob` klasöre yükle. Dosyayı PageBlob klasöründen kaldırın veya BlockBlob klasörüne taşıyın. Doğrulamayı yeniden deneyin.|
| `InvalidBlobPath` | Dosya yolu, Azure Blob adlandırma kurallarına göre buluttaki geçerli bir blob yoluna eşlenmiyor.|Dosya yolunu yeniden adlandırmak için Azure adlandırma yönergeleri ' ni izleyin. |
| `EnumerationError` | Doğrulama için dosya numaralandırılamadı. |Bu hatanın birden çok nedeni olabilir. En olası nedeni dosyaya erişimidir. |
| `ShareSizeExceeded` | Bu dosya, Azure dosya paylaşımının boyutunun 5 TB 'lık Azure sınırını aşmasına neden oldu.|Paylaşımdaki verilerin boyutunu [Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uygun olacak şekilde küçültün. Doğrulamayı yeniden deneyin. |
| `AzureFileSizeExceeded` | Dosya boyutu Azure dosya boyutu sınırlarını aşıyor.| Dosyanın veya verilerin boyutunu [Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uygun olacak şekilde küçültün. Doğrulamayı yeniden deneyin.|
| `BlockBlobSizeExceeded` | Dosya boyutu, Azure Blok Blobu boyut sınırlarını aşıyor. | Dosyanın veya verilerin boyutunu [Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uygun olacak şekilde küçültün. Doğrulamayı yeniden deneyin. |
| `ManagedDiskSizeExceeded` | Dosya boyutu, Azure tarafından yönetilen disk boyutu sınırlarını aşıyor. | Dosyanın veya verilerin boyutunu [Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uygun olacak şekilde küçültün. Doğrulamayı yeniden deneyin. |
| `PageBlobSizeExceeded` | Dosya boyutu, Azure tarafından yönetilen disk boyutu sınırlarını aşıyor. | Dosyanın veya verilerin boyutunu [Azure nesne boyutu sınırlarına](data-box-disk-limits.md#azure-object-size-limits)uygun olacak şekilde küçültün. Doğrulamayı yeniden deneyin. |
| `InvalidShareContainerFormat`  |Dizin adları, kapsayıcılar veya paylaşımlar için Azure adlandırma kurallarına uymuyor.         |Diskteki önceden var olan klasörler altında oluşturulan ilk klasör, depolama hesabınızda bir kapsayıcı haline gelir. Bu paylaşımın veya kapsayıcı adı Azure adlandırma kurallarına uymuyor. Dosyayı [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uygun olacak şekilde yeniden adlandırın. Doğrulamayı yeniden deneyin.   |
| `InvalidBlobNameFormat` | Dosya yolu, Azure Blob adlandırma kurallarına göre buluttaki geçerli bir blob yoluna eşlenmiyor.|Dosyayı [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uygun olacak şekilde yeniden adlandırın. Doğrulamayı yeniden deneyin. |
| `InvalidFileNameFormat` | Dosya yolu, Azure dosya adlandırma kurallarına göre buluttaki geçerli bir dosya yoluna eşlenmiyor. |Dosyayı [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uygun olacak şekilde yeniden adlandırın. Doğrulamayı yeniden deneyin. |
| `InvalidDiskNameFormat` | Dosya yolu, Azure yönetilen disk adlandırma kurallarına göre buluttaki geçerli bir disk adıyla eşlenmiyor. |Dosyayı [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uygun olacak şekilde yeniden adlandırın. Doğrulamayı yeniden deneyin.       |
| `NotPartOfFileShare` | Dosyalar için karşıya yükleme yolu geçerli değil. Dosyaları Azure dosyalarındaki bir klasöre yükleyin.   | Hatalı dosyaları kaldırın ve bu dosyaları önceden düzenlenen bir klasöre yükleyin. Doğrulamayı yeniden deneyin. |
| `NonVhdFileNotSupportedForManagedDisk` | VHD olmayan bir dosya yönetilen disk olarak karşıya yüklenemez. |Bu dosyalar desteklenmediğinden, VHD olmayan dosyaları `ManagedDisk` klasörden kaldırın veya bu dosyaları bir `PageBlob` klasöre taşıyın. Doğrulamayı yeniden deneyin. |


## <a name="next-steps"></a>Sonraki adımlar

- [Karşıya veri yükleme hatalarını](data-box-disk-troubleshoot-upload.md)giderme.
