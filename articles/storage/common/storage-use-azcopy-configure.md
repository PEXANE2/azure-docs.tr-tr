---
title: Azure Storage ile AzCopy 'i yapılandırma, iyileştirme ve sorunlarını giderme | Microsoft Docs
description: AzCopy 'i yapılandırın, iyileştirin ve sorun giderin.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 07/25/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 3773f9a8464dc94436d6d2503b173d4674033ab1
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565045"
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

AzCopy ara sunucu ayarlarını yapılandırmak için `https_proxy` ortam değişkenini ayarlayın. AzCopy 'i Windows üzerinde çalıştırırsanız AzCopy proxy ayarlarını otomatik olarak algılar, bu nedenle bu ayarı Windows 'ta kullanmanız gerekmez. Bu ayarı Windows 'ta kullanmayı seçerseniz, otomatik algılamayı geçersiz kılar.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | Komut isteminde şunu kullanın:`set https_proxy=<proxy IP>:<proxy port>`<br> PowerShell kullanımı:`$env:https_proxy="<proxy IP>:<proxy port>"`|
| **Linux** | `export https_proxy=<proxy IP>:<proxy port>` |
| **MacOS** | `export https_proxy=<proxy IP>:<proxy port>` |

Şu anda AzCopy, NTLM veya Kerberos ile kimlik doğrulaması gerektiren proxy 'leri desteklemiyor.

## <a name="optimize-throughput"></a>Aktarım hızını iyileştirme

Aktarım hızı veri hızına `cap-mbps` bir tavan koymak için bayrağını kullanabilirsiniz. Örneğin, aşağıdaki komut, aktarım hızını saniyede megabit `10` (MB) ile sınırlar.

```azcopy
azcopy cap-mbps 10
```

Küçük dosyalar aktarılırken üretilen iş azalabilir. `AZCOPY_CONCURRENCY_VALUE` Ortam değişkenini ayarlayarak aktarım hızını artırabilirsiniz. Bu değişken, gerçekleşebileceğini eşzamanlı isteklerin sayısını belirtir.  Bilgisayarınızda 5 ' ten az CPU varsa, bu değişkenin değeri olarak `32`ayarlanır. Aksi takdirde, varsayılan değer 16 ' ya eşittir CPU sayısıyla çarpılır. Bu değişkenin `300`en büyük varsayılan değeri, ancak bu değeri el ile veya daha düşük bir şekilde ayarlayabilirsiniz.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Linux** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **MacOS** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

Bu değişkenin geçerli değerini denetlemek içinöğesinikullanın.`azcopy env`  Değer boşsa, `AZCOPY_CONCURRENCY_VALUE` değişkeni varsayılan `300`değerine ayarlanır.

## <a name="change-the-location-of-the-log-files"></a>Günlük dosyalarının konumunu değiştirme

Varsayılan olarak, günlük dosyaları Windows veya `%USERPROFILE\\.azcopy` `$HOME\\.azcopy` Mac ve Linux dizinindeki dizinde bulunur. Bu komutları kullanarak gerekirse, bu konumu değiştirebilirsiniz.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_LOG_LOCATION=<value>` |
| **Linux** | `export AZCOPY_LOG_LOCATION=<value>` |
| **MacOS** | `export AZCOPY_LOG_LOCATION=<value>` |

Bu değişkenin geçerli değerini denetlemek içinöğesinikullanın.`azcopy env` Değer boşsa, günlükler varsayılan konuma yazılır.

## <a name="change-the-default-log-level"></a>Varsayılan günlük düzeyini değiştirme

Varsayılan olarak, AzCopy günlük düzeyi olarak `INFO`ayarlanır. Disk alanından tasarruf etmek için günlük ayrıntı düzeyini azaltmak isterseniz, ``--log-level`` seçeneğini kullanarak bu ayarın üzerine yazın. 

Kullanılabilir günlük düzeyleri şunlardır: `DEBUG`, `INFO`, `WARNING` `ERROR`,, `PANIC`ve. `FATAL`

## <a name="troubleshoot-issues"></a>Sorunları giderme

AzCopy her iş için günlük ve plan dosyaları oluşturur. Olası sorunları araştırmak ve sorunlarını gidermek için günlükleri kullanabilirsiniz. 

Günlükler hatanın durumunu (`UPLOADFAILED`, `COPYFAILED`, ve `DOWNLOADFAILED`), tam yolu ve hatanın nedenini içerecektir.

Varsayılan olarak, günlük ve plan dosyaları, Mac ve Linux üzerindeki `%USERPROFILE\\.azcopy` Windows veya `$HOME\\.azcopy` dizindeki dizinde bulunur.

> [!IMPORTANT]
> Microsoft Desteği bir istek gönderirken (veya herhangi bir üçüncü taraf ile ilgili sorunu gidermeye çalıştığınızda), yürütmek istediğiniz komutun Redaksiyonu yapılmış sürümünü paylaşabilirsiniz. Bu, SAS 'nin yanlışlıkla herhangi bir gövdele paylaşılmamasını sağlar. Redaksiyonu yapılmış sürümü günlük dosyasının başlangıcında bulabilirsiniz.

### <a name="review-the-logs-for-errors"></a>Günlükleri hatalara karşı gözden geçirin

Aşağıdaki komut, `UPLOADFAILED` `04dc9ca9-158f-7945-5933-564021086c79` günlükteki durum ile tüm hataları alacak:

**Windows (PowerShell)**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**Linux**

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