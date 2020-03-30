---
title: Azure Veri Kutusu Disk sınırları | Microsoft Dokümanlar
description: Microsoft Azure Veri Kutusu Diski için sistem sınırlarını ve önerilen boyutları açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 11/05/2019
ms.author: alkohli
ms.openlocfilehash: 1bb8300f1e54cf03563704cf00549ce9e09a3916
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260169"
---
# <a name="azure-data-box-disk-limits"></a>Azure Veri Kutusu Disk sınırları


Microsoft Azure Veri Kutusu Disk çözümünüzü dağıtırken ve çalıştırırken bu sınırları göz önünde bulundurun.

## <a name="data-box-service-limits"></a>Veri Kutusu hizmet limitleri

 - Veri Kutusu hizmeti, [Bölge kullanılabilirliği'nde](data-box-disk-overview.md#region-availability)listelenen Azure bölgelerinde kullanılabilir.
 - Veri Kutusu Diski ile tek bir depolama hesabı desteklenir.

## <a name="data-box-disk-performance"></a>Veri Kutusu Disk performansı

USB 3.0 bağlantısıyla yapılan testlerde disk performansının 430 MB/sn seviyesine çıkabildiği görülmüştür. Gerçek performans kullanılan dosya boyutuna göre değişiklik gösterecektir. Daha küçük dosyalarda performans daha düşük olabilir.

## <a name="azure-storage-limits"></a>Azure depolama sınırları

Bu bölümde, Veri Kutusu hizmetiiçin geçerli olduğu gibi Azure Depolama hizmetinin sınırları ve Azure Dosyaları, Azure blok blob'ları ve Azure sayfa lekeleri için gerekli adlandırma kuralları açıklanmaktadır. Depolama sınırlarını dikkatle gözden geçirin ve tüm önerileri uygulayın.

Azure depolama hizmeti sınırları ve paylaşımları, kapsayıcıları ve dosyaları adlandırmak için en iyi uygulamalar hakkında en son bilgiler için şu bilgilere gidin:

- [Kapsayıcıları adlandırma ve başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Paylaşımları adlandırma ve onlara başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blobs ve sayfa blob kuralları blok](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Azure Depolama hizmet sınırlarını aşan veya Azure Files/Blob adlandırma kurallarına uymayan dosyalar veya dizinler varsa, bu dosyalar veya dizinler Veri Kutusu hizmeti aracılığıyla Azure Depolama alanına alınmaz.

## <a name="data-upload-caveats"></a>Veri yükleme uyarıları

- Verileri doğrudan disklere kopyalamayın. Verileri önceden oluşturulmuş *BlockBlob,**PageBlob*ve *AzureFile* klasörlerine kopyalayın.
- *BlockBlob ve PageBlob'un* altındaki klasör bir kapsayıcıdır. *PageBlob* Örneğin, kapsayıcılar *BlockBlob/konteyner* ve *PageBlob/konteyner*olarak oluşturulur.
- Bulutta kopyalanan nesneyle aynı ada sahip varolan bir Azure nesneniz (örneğin blob gibi) varsa, Veri Kutusu Diski dosyayı buluttaki dosya(1) olarak yeniden adlandırır.
- *BlockBlob* ve *PageBlob* paylaşımlarına yazılan her dosya sırasıyla blok blob ve sayfa blob olarak yüklenir.
- *BlockBlob* ve *PageBlob* klasörleri altında oluşturulan boş dizin hiyerarşisi (dosya olmadan) yüklenmez.
- Azure'a veri yüklerken herhangi bir hata varsa, hedef depolama hesabında bir hata günlüğü oluşturulur. Bu hata günlüğüne giden yol, yükleme tamamlandığında portalda kullanılabilir ve düzeltici eylemde yer almak için günlüğü inceleyebilirsiniz. Yüklenen verileri doğrulamadan verileri kaynaktan silmeyin.
- Dosya meta verileri ve NTFS izinleri, veriler Azure Dosyaları'na yüklendiğinde korunmaz. Örneğin, veriler kopyalandığında dosyaların *Son değiştirilen* özniteliği tutulmaz.
- Yönetilen diskleri sırada belirttiyseniz, aşağıdaki ek hususları gözden geçirin:

    - Önceden oluşturulmuş tüm klasörler ve tüm Data Box Disk’ler arasında, bir kaynak grubunda belirli bir ada sahip yalnızca bir yönetilen diskiniz olabilir. Bu durum, önceden oluşturulan klasörlere yüklenen VHD'lerin benzersiz adlara sahip olması gerektiği anlamına gelir. Verilen adın bir kaynak grubunda zaten var olan bir yönetilen diskle eşleşmediğinden emin olun. VHD'ler aynı ada sahipse, o adı taşıyan yalnızca bir VHD yönetilen diske dönüştürülür. Diğer VHD'ler, hazırlama depolama hesabına sayfa blobları olarak yüklenir.
    - VHD'leri her zaman önceden oluşturulmuş klasörlerden birine kopyalayın. VHD'leri bu klasörlerin dışına veya oluşturduğunuz bir klasöre kopyalarsanız, VHD'ler Azure Depolama hesabına yönetilen diskler olarak değil sayfa blobları olarak yüklenir.
    - Yönetilen diskler oluşturmak için yalnızca sabit VHD'ler karşıya yüklenebilir. Dinamik VHD'ler, fark kayıt VHD'leri veya VHDX dosyaları desteklenmez.

## <a name="azure-storage-account-size-limits"></a>Azure depolama hesabı boyutu sınırları

Aşağıda, depolama hesabına kopyalanan verilerin boyutuna ilişkin sınırlar ve bu sınırlar ve bu veriler ve bu veriler ve bu sınırlar ve bu veriler ve bu veriler aşağıda veda edilebilgilidir. Yüklediğiniz verilerin bu sınırlara uydur olduğundan emin olun. Bu sınırlardaki en güncel bilgiler için Azure [blob depolama ölçeği hedeflerine](https://docs.microsoft.com/azure/storage/blobs/scalability-targets#scale-targets-for-blob-storage) ve [Azure Dosyaları ölçek hedeflerine](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account#scale-targets-for-standard-storage-accounts)gidin.

| Azure depolama hesabına kopyalanan verilerin boyutu                      | Varsayılan limit          |
|---------------------------------------------------------------------|------------------------|
| Blob ve sayfa blob blok                                            | Depolama hesabı başına 500 TB. <br> Bu, Data Box Disk de dahil olmak üzere tüm kaynaklardan gelen verileri içerir.|


## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

Azure nesnelerinin yazılabilir boyutları aşağıda verebilirsiniz. Yüklenen tüm dosyaların bu sınırlara uyduklarına emin olun.

| Azure nesne türü | Varsayılan limit                                             |
|-------------------|-----------------------------------------------------------|
| Blok Blob        | ~ 4.75 TiB                                                 |
| Sayfa Blob         | 8 TiB <br> (Sayfa Blob formatında yüklenen her dosya 512 bayt hizalanmış olmalıdır, aksi takdirde yükleme başarısız olur. <br> Hem VHD hem de VHDX 512 bayt hizalanır.) |
|Azure Dosyaları        | 1 TiB <br> En çok, pay büyüklüğü 5 TiB olduğunu     |
| Yönetilen diskler     |4 TiB <br> Boyut ve sınırlar hakkında daha fazla bilgi için bkz: <li>[Yönetilen diskler için ölçeklenebilirlik hedefleri](../virtual-machines/windows/disk-scalability-targets.md#managed-virtual-machine-disks)</li>|


## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Azure blok blob, sayfa blob ve dosya adlandırma kuralları

| Varlık                                       | Kurallar                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Blok blob ve sayfa blob için konteyner adları <br> Azure Dosyaları için dosya paylaşımı adları | 3 ila 63 karakter uzunluğunda geçerli bir DNS adı olmalıdır. <br>  Bir harf veya sayı ile başlamalıdır. <br> Yalnızca küçük harfler, sayılar ve tire (-) içerebilir. <br> Kısa çizgiden (-) hemen önce ve sonra bir harf veya rakam gelmelidir. <br> Adlarda kısa çizgiler art arda kullanılamaz. |
| Azure dosyaları için dizin ve dosya adları     |<li> Büyük/küçük harf koruma, büyük/küçük harf duyarsız ve uzunluğu 255 karakter geçmemelidir. </li><li> İleri eğik çizgi (/) ile sona eremez. </li><li>Sağlanırsa, otomatik olarak kaldırılır. </li><li> Aşağıdaki karakterlere izin verilmez:<code>" \\ / : \| < > * ?</code></li><li> Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. </li><li> Yasa dışı URL yolu karakterlerine izin verilmez. uE000 gibi \\kod noktaları geçerli Unicode karakterleri değildir. Kontrol karakterleri (0x00 - 0x1F, \\u0081, vb.) gibi bazı ASCII veya Unicode karakterlerine de izin verilmez. HTTP/1.1'deki Unicode dizelerini yöneten kurallar için bkz: RFC 2616, Bölüm 2.2: Temel Kurallar ve RFC 3987. </li><li> Aşağıdaki dosya adlarına izin verilmez: LPT1, LPT2, LPT3, LPT4, LPT7, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, dot karakter (.) ve iki nokta karakteri (.)</li>|
| Blok blobu ve sayfa blobu için blob adları      | Blob adları büyük/küçük harfe duyarlıdır ve karakterler herhangi bir düzende sıralanabilir. <br> Blob adı 1 ila 1024 karakter uzunluğunda olmalıdır. <br> Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. <br>Blob adını oluşturan yolun bölümleri 254 karakterden uzun olamaz. Yol bölümü, arka arkaya gelen sınırlayıcı karakterlerinin (örneğin eğik çizgi "/") arasında yer alan ve bir sanal dizinin adına karşılık gelen dizedir. |

## <a name="managed-disk-naming-conventions"></a>Yönetilen disk adlandırma kuralları

| Varlık | Kurallar                                             |
|-------------------|-----------------------------------------------------------|
| Yönetilen disk adları       | <li> Ad 1 ila 80 karakter uzunluğunda olmalıdır. </li><li> Ad bir harf veya sayı ile başlamalı, harf, sayı veya alt uçlu ile bitmelidir. </li><li> Ad yalnızca harfler, sayılar, alt çizerler, dönemler veya tireler içerebilir. </li><li>   Adı boşluk veya `/`olmamalıdır.                                              |

## <a name="next-steps"></a>Sonraki adımlar

- Veri Kutusu Disk sistem gereksinimlerini gözden [geçirin](data-box-disk-system-requirements.md)
