---
title: Azure Storage ile AzCopy 'i yapılandırma, iyileştirme ve sorunlarını giderme | Microsoft Docs
description: AzCopy 'i yapılandırın, iyileştirin ve sorun giderin.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: 3843eb2e906e3fb8d390e509e17117b7849ac220
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244709"
---
# <a name="configure-optimize-and-troubleshoot-azcopy"></a>AzCopy 'i yapılandırma, iyileştirme ve sorun giderme

AzCopy, bir depolama hesabına blob veya dosya kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, gelişmiş yapılandırma görevlerini gerçekleştirmenize yardımcı olur ve AzCopy kullandığınızda oluşabilecek sorunları gidermenize yardımcı olur.

> [!NOTE]
> AzCopy kullanmaya başlamanıza yardımcı olacak içerik arıyorsanız, aşağıdaki makalelerden birine bakın:
> - [AzCopy ile çalışmaya başlama](storage-use-azcopy-v10.md)
> - [AzCopy ve BLOB Storage ile veri aktarma](storage-use-azcopy-blobs.md)
> - [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)
> - [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)

## <a name="configure-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma

AzCopy için proxy ayarlarını yapılandırmak üzere `https_proxy` ortam değişkenini ayarlayın. AzCopy 'i Windows üzerinde çalıştırırsanız AzCopy proxy ayarlarını otomatik olarak algılar, bu nedenle bu ayarı Windows 'ta kullanmanız gerekmez. Bu ayarı Windows 'ta kullanmayı seçerseniz, otomatik algılamayı geçersiz kılar.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | Komut isteminde şunu kullanın: `set https_proxy=<proxy IP>:<proxy port>`<br> PowerShell kullanımda: `$env:https_proxy="<proxy IP>:<proxy port>"`|
| **'Un** | `export https_proxy=<proxy IP>:<proxy port>` |
| **MacOS** | `export https_proxy=<proxy IP>:<proxy port>` |

Şu anda AzCopy, NTLM veya Kerberos ile kimlik doğrulaması gerektiren proxy 'leri desteklemiyor.

## <a name="optimize-throughput"></a>Aktarım hızını iyileştirme

Verimlilik veri hızına bir tavan koymak için `cap-mbps` bayrağını kullanabilirsiniz. Örneğin, aşağıdaki komut, saniye başına `10` megabit (MB) olarak büyük/küçük harf işleme sağlar.

```azcopy
azcopy cap-mbps 10
```

Küçük dosyalar aktarılırken üretilen iş azalabilir. @No__t-0 ortam değişkenini ayarlayarak aktarım hızını artırabilirsiniz. Bu değişken, gerçekleşebileceğini eşzamanlı isteklerin sayısını belirtir.  Bilgisayarınızda 5 ' ten az CPU varsa, bu değişkenin değeri `32` olarak ayarlanır. Aksi takdirde, varsayılan değer 16 ' ya eşittir CPU sayısıyla çarpılır. Bu değişkenin en büyük varsayılan değeri `300` ' dır, ancak bu değeri el ile veya daha düşük bir şekilde ayarlayabilirsiniz.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **'Un** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **MacOS** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

Bu değişkenin geçerli değerini denetlemek için `azcopy env` kullanın.  Değer boşsa `AZCOPY_CONCURRENCY_VALUE` değişkeni varsayılan değer olan `300` ' e ayarlanır.

## <a name="change-the-location-of-the-log-files"></a>Günlük dosyalarının konumunu değiştirme

Varsayılan olarak, günlük dosyaları Windows üzerinde `%USERPROFILE%\.azcopy` dizininde veya Mac ve Linux 'ta `$HOME\\.azcopy` dizininde bulunur. Bu komutları kullanarak gerekirse, bu konumu değiştirebilirsiniz.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_LOG_LOCATION=<value>` |
| **'Un** | `export AZCOPY_LOG_LOCATION=<value>` |
| **MacOS** | `export AZCOPY_LOG_LOCATION=<value>` |

Bu değişkenin geçerli değerini denetlemek için `azcopy env` kullanın. Değer boşsa, günlükler varsayılan konuma yazılır.

## <a name="change-the-default-log-level"></a>Varsayılan günlük düzeyini değiştirme

Varsayılan olarak, AzCopy günlük düzeyi `INFO` olarak ayarlanır. Disk alanını kaydetmek için günlük ayrıntı düzeyini azaltmak isterseniz, ``--log-level`` seçeneğini kullanarak bu ayarın üzerine yazın. 

Kullanılabilir günlük düzeyleri şunlardır: `DEBUG`, `INFO`, `WARNING`, `ERROR`, `PANIC` ve `FATAL`.

## <a name="troubleshoot-issues"></a>Sorunları giderme

AzCopy her iş için günlük ve plan dosyaları oluşturur. Olası sorunları araştırmak ve sorunlarını gidermek için günlükleri kullanabilirsiniz. 

Günlüklerde hata durumu (`UPLOADFAILED`, `COPYFAILED` ve `DOWNLOADFAILED`), tam yol ve hatanın nedeni yer alacak.

Varsayılan olarak, günlük ve plan dosyaları Windows üzerinde `%USERPROFILE\\.azcopy` dizininde veya Mac ve Linux üzerindeki `$HOME\\.azcopy` dizininde bulunur.

> [!IMPORTANT]
> Microsoft Desteği bir istek gönderirken (veya herhangi bir üçüncü taraf ile ilgili sorunu gidermeye çalıştığınızda), yürütmek istediğiniz komutun Redaksiyonu yapılmış sürümünü paylaşabilirsiniz. Bu, SAS 'nin yanlışlıkla herhangi bir gövdele paylaşılmamasını sağlar. Redaksiyonu yapılmış sürümü günlük dosyasının başlangıcında bulabilirsiniz.

### <a name="review-the-logs-for-errors"></a>Günlükleri hatalara karşı gözden geçirin

Aşağıdaki komut, `04dc9ca9-158f-7945-5933-564021086c79` günlüğünden `UPLOADFAILED` durumundaki tüm hataları alacak:

**Windows (PowerShell)**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**'Un**

```
grep UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

### <a name="view-and-resume-jobs"></a>İşleri görüntüleme ve sürdürme

Her aktarım işlemi bir AzCopy işi oluşturur. İşlerin geçmişini görüntülemek için aşağıdaki komutu kullanın:

```
azcopy jobs list
```

İş istatistiklerini görüntülemek için aşağıdaki komutu kullanın:

```
azcopy jobs show <job-id>
```

Aktarımları duruma göre filtrelemek için aşağıdaki komutu kullanın:

```
azcopy jobs show <job-id> --with-status=Failed
```

Başarısız/iptal edildi bir işi yeniden başlatmak için aşağıdaki komutu kullanın. Bu komut, güvenlik nedenleriyle kalıcı olmadığından, kimliğini SAS belirteciyle birlikte kullanır:

```
azcopy jobs resume <job-id> --source-sas="<sas-token>"
azcopy jobs resume <job-id> --destination-sas="<sas-token>"
```

Bir işi sürdürürseniz AzCopy iş planı dosyasına bakar. Plan dosyası, iş ilk oluşturulduğunda işleme için tanımlanan tüm dosyaları listeler. Bir işi sürdürürseniz AzCopy, önceden aktarılmayan plan dosyasında listelenen tüm dosyaları aktarmaya çalışır.
