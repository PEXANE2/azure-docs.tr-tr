---
title: Azure PowerShell komut dosyası örneği - Bir blob kapsayıcısının toplam fatura boyutunu hesaplayın | Microsoft Dokümanlar
description: Faturalandırma amacıyla Azure Blob depolama alanında bir kapsayıcının toplam boyutunu hesaplayın.
services: storage
author: fhryo-msft
ms.service: storage
ms.subservice: blobs
ms.devlang: powershell
ms.topic: sample
ms.date: 11/07/2017
ms.author: fryu
ms.openlocfilehash: 12b32256c91dfcf93ca55eeb348cc78613ba860e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80067098"
---
# <a name="calculate-the-total-billing-size-of-a-blob-container"></a>Blob kapsayıcısının toplam fatura boyutunu hesaplama

Bu komut dosyası, fatura maliyetlerini tahmin etmek amacıyla Azure Blob depolama alanında bir kapsayıcının boyutunu hesaplar. Komut dosyası, kapsayıcıdaki lekelerin boyutunu toplamlar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!NOTE]
> Bu PowerShell komut dosyası, faturalandırma amacıyla bir kapsayıcının boyutunu hesaplar. Kapsayıcı boyutunu başka amaçlarla hesaplıyorsanız, [bkz.](../scripts/storage-blobs-container-calculate-size-powershell.md)

## <a name="determine-the-size-of-the-blob-container"></a>Blob kabının boyutunu belirleme

Blob konteynerinin toplam boyutu, kabın boyutunu ve kabın altındaki tüm lekelerin boyutunu içerir.

Aşağıdaki bölümlerde, blob kapları ve lekeler için depolama kapasitesinin nasıl hesaplandığı açıklanmaktadır.Aşağıdaki bölümde, Len(X) dizedeki karakter sayısını ifade eder.

### <a name="blob-containers"></a>Blob kaplar

Aşağıdaki hesaplama, blob kapsayıcı başına tüketilen depolama miktarının nasıl tahmin edilebildiğini açıklar:

```
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
```

Aşağıdaki arıza:
* Her kapsayıcı için 48 bayt genel tepede Son Değiştirilen Süre, İzinler, Genel Ayarlar ve bazı sistem meta verileri içerir.

* Kapsayıcı adı Unicode olarak depolanır, bu nedenle karakter sayısını alın ve ikiyle çarpın.

* Depolanan blob kapsayıcı meta verilerinin her bloğu için, adın (ASCII) uzunluğunu ve dize değerinin uzunluğunu depolarız.

* İmzalı Tanımlayıcı başına 512 bayt, imzalı tanımlayıcı adı, başlangıç saati, son kullanma tarihi ve izinleri içerir.

### <a name="blobs"></a>Bloblar

Aşağıdaki hesaplamalar, blob başına tüketilen depolama miktarının nasıl tahmin edilebildiğini gösterir.

* Blok blob (taban blob veya anlık görüntü):

   ```
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
   SizeInBytes(data in unique committed data blocks stored) +
   SizeInBytes(data in uncommitted data blocks)
   ```

* Sayfa blob (taban blob veya anlık görüntü):

   ```
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   number of nonconsecutive page ranges with data * 12 bytes +
   SizeInBytes(data in unique pages stored)
   ```

Aşağıdaki arıza:

* Blob için 124 bayt havai, hangi içerir:
    - Son Değişiklik Zamanı
    - Boyut
    - Önbellek Denetimi
    - İçerik Türü
    - İçerik Dili
    - İçerik Kodlama
    - İçerik-MD5
    - İzinler
    - Anlık görüntü bilgileri
    - Kira
    - Bazı sistem meta verileri

* Blob adı Unicode olarak depolanır, bu nedenle karakter sayısını alın ve ikiyle çarpın.

* Depolanan her meta veri bloğu için, adın uzunluğunu (ASCII olarak depolanan) ve dize değerinin uzunluğunu ekleyin.

* Blok lekeleri için:
  * Blok listesi için 8 bayt.
  * Baytlarda blok kimliği boyutunun kat sayısı.
  * Tüm taahhüt edilen ve işlenmemiş bloklarda verilerin boyutu.

    >[!NOTE]
    >Anlık görüntüler kullanıldığında, bu boyut yalnızca bu temel veya anlık görüntü blob'u için benzersiz verileri içerir. İşlenmemiş bloklar bir hafta sonra kullanılmazsa, çöp toplanır. Bundan sonra, onlar fatura doğru sayılmaz.

* Sayfa lekeleri için:
  * Veri süreleri 12 bayt olan ardışık olmayan sayfa aralıklarının sayısı. Bu, **GetPageRanges** API'yi ararken gördüğünüz benzersiz sayfa aralıklarının sayısıdır.

  * Depolanan tüm sayfaların baytlarında bulunan verilerin boyutu.

    >[!NOTE]
    >Anlık görüntüler kullanıldığında, bu boyut yalnızca temel blob için benzersiz sayfaları veya sayılmayan anlık görüntü blob'u içerir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size-ex.ps1 "Calculate container size")]

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. Kapsayıcı boyutunun tahminini sağlayan basit bir komut dosyası için [Blob depolama kapsayıcısının toplam boyutunu hesaplayın.](../scripts/storage-blobs-container-calculate-size-powershell.md)

- Azure Depolama faturalandırması hakkında daha fazla bilgi için windows [azure depolama faturasını anlama](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/)'ya bakın.

- Azure PowerShell modülü hakkında daha fazla bilgi için [Azure PowerShell belgelerine](https://docs.microsoft.com/powershell/azure/overview)bakın.

- [Azure Depolama için PowerShell örneklerinde](../blobs/storage-samples-blobs-powershell.md)ek Depolama PowerShell komut dosyası örnekleri bulabilirsiniz.
