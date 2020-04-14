---
title: Azure Depolama ile AzCopy'i yapılandırın, optimize edin ve sorun giderin | Microsoft Dokümanlar
description: AzCopy'yi yapılandırın, optimize edin ve sorun giderin.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: 87a335f44a31436de735395adbee9035493cbbd2
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81263429"
---
# <a name="configure-optimize-and-troubleshoot-azcopy"></a>AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme

AzCopy, bir depolama hesabına veya bir depolama hesabından blobveya dosya kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, gelişmiş yapılandırma görevleri gerçekleştirmenize yardımcı olur ve AzCopy'i kullandığınızda ortaya çıkabilecek sorunları gidermenize yardımcı olur.

> [!NOTE]
> AzCopy'ye başlamanıza yardımcı olacak içerik arıyorsanız, aşağıdaki makalelerden herhangi birini görün:
> - [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
> - [AzCopy ve blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
> - [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
> - [AzCopy ve Amazon S3 kovaları ile veri aktarımı](storage-use-azcopy-s3.md)

## <a name="configure-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma

AzCopy için proxy ayarlarını yapılandırmak `https_proxy` için ortam değişkenini ayarlayın. AzCopy'yi Windows'da çalıştırıyorsanız, AzCopy proxy ayarlarını otomatik olarak algılar, böylece bu ayarı Windows'ta kullanmak zorunda kalmamanız gerekmez. Bu ayarı Windows'da kullanmayı seçerseniz, otomatik algılamayı geçersiz kılar.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | Komut istemi kullanımında:`set https_proxy=<proxy IP>:<proxy port>`<br> PowerShell kullanımında:`$env:https_proxy="<proxy IP>:<proxy port>"`|
| **Linux** | `export https_proxy=<proxy IP>:<proxy port>` |
| **Macos** | `export https_proxy=<proxy IP>:<proxy port>` |

Şu anda, AzCopy NTLM veya Kerberos ile kimlik doğrulaması gerektiren yakınlıkları desteklemez.

## <a name="optimize-performance"></a>Performansı iyileştirme

Performansı kıyaslayabilir ve ardından performans ve kaynak tüketimi arasında en iyi dengeyi bulmak için komutları ve ortam değişkenlerini kullanabilirsiniz.

Bu bölüm, bu optimizasyon görevlerini gerçekleştirmenize yardımcı olur:

> [!div class="checklist"]
> * Kıyaslama testlerini çalıştırma
> * Elde iş akışını optimize edin
> * Bellek kullanımını optimize edin 
> * Dosya eşitlemesi optimize edin

### <a name="run-benchmark-tests"></a>Kıyaslama testlerini çalıştırma

Genel performans istatistiklerini görüntülemek ve kimlik performans darboğazlarını görüntülemek için belirli blob kapsayıcılarında bir performans ölçüt testi çalıştırabilirsiniz. 

Performans ölçüt testi çalıştırmak için aşağıdaki komutu kullanın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy bench 'https://<storage-account-name>.blob.core.windows.net/<container-name>'` |
| **Örnek** | `azcopy bench 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D'` |

> [!TIP]
> Bu örnek, yol bağımsız değişkenlerini tek tırnak ('') ile kapsar. Windows Komut Uyruşu (cmd.exe) hariç tüm komut kabuklarında tek tırnak işareti kullanın. Windows Komut Uyruşu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak ('') yerine çift tırnak ("") içeren bir şekilde içine çekin.

Bu komut, test verilerini belirli bir hedefe yükleyerek bir performans ölçütü çalıştırUr. Test verileri bellekte oluşturulur, hedefe yüklenir ve test tamamlandıktan sonra hedeften silinir. İsteğe bağlı komut parametrelerini kullanarak kaç dosya oluşturacağınızı ve hangi boyutta olmasını istediğinizi belirtebilirsiniz.

Ayrıntılı referans dokümanları için [azcopy bankına](storage-ref-azcopy-bench.md)bakın.

Bu komut için ayrıntılı yardım `azcopy bench -h` kılavuzunu görüntülemek için ENTER tuşuna yazın ve basın.

### <a name="optimize-throughput"></a>Elde iş akışını optimize edin

Elde etme `cap-mbps` veri hızına bir tavan yerleştirmek için komutlarınızdaki bayrağı kullanabilirsiniz. Örneğin, aşağıdaki komut bir işi devam ettirer `10` ve saniyede megabit (MB) ile büyük iş artışı elde eder. 

```azcopy
azcopy jobs resume <job-id> --cap-mbps 10
```

Küçük dosyaları aktarırken iş aktarımı azalabilir. Ortam değişkenini ayarlayarak iş `AZCOPY_CONCURRENCY_VALUE` ortamını artırabilirsiniz. Bu değişken, oluşabilecek eşzamanlı istek sayısını belirtir.  

Bilgisayarınızda 5 CPU'dan az varsa, bu değişkenin değeri `32`. Aksi takdirde, varsayılan değer 16'ya eşittir ve CPU sayısıyla çarpılır. Bu değişkenin en büyük `3000`varsayılan değeri, ancak bu değeri el ile daha yüksek veya daha düşük ayarlayabilirsiniz. 

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Linux** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Macos** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

Bu `azcopy env` değişkenin geçerli değerini denetlemek için kullanın. Değer boşsa, herhangi bir AzCopy günlük dosyasının başına bakarak hangi değerin kullanıldığını okuyabilirsiniz. Seçili değer ve seçilme nedeni burada bildirilir.

Bu değişkeni ayarlamadan önce, bir kıyaslama testi çalıştırmanızı öneririz. Kıyaslama test işlemi önerilen eşzamanlılık değerini bildirir. Alternatif olarak, ağ koşullarınız ve yükleriniz farklıysa, bu değişkeni belirli bir sayı yerine sözcük `AUTO` için ayarlayın. Bu, AzCopy'nin her zaman kıyaslama testlerinde kullandığı otomatik ayar işlemini çalıştırmasını sağlayacaktır.

### <a name="optimize-memory-use"></a>Bellek kullanımını optimize edin

Dosya `AZCOPY_BUFFER_GB` indirirken ve yüklerken AzCopy'nin kullanmasını istediğiniz sistem belleğinizin maksimum miktarını belirtmek için ortam değişkenini ayarlayın.
Bu değeri gigabayt (GB) olarak ifade edin.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_BUFFER_GB=<value>` |
| **Linux** | `export AZCOPY_BUFFER_GB=<value>` |
| **Macos** | `export AZCOPY_BUFFER_GB=<value>` |

### <a name="optimize-file-synchronization"></a>Dosya eşitlemesi optimize edin

[Eşitleme](storage-ref-azcopy-sync.md) komutu hedefteki tüm dosyaları tanımlar ve eşitleme işlemini başlatmadan önce dosya adlarını ve son değiştirilen zaman damgalarını karşılaştırır. Çok sayıda dosyanız varsa, bu ön işlemeyi ortadan kaldırarak performansı artırabilirsiniz. 

Bunu başarmak için, bunun yerine [azcopy](storage-ref-azcopy-copy.md) copy `--overwrite` copy `ifSourceNewer`komutunu kullanın ve bayrağı . AzCopy, herhangi bir ön tarama ve karşılaştırma yapmadan kopyalanan dosyaları karşılaştıracaktır. Bu, karşılaştırılacak çok sayıda dosyanın olduğu durumlarda bir performans kenarı sağlar.

[Azcopy copy copy](storage-ref-azcopy-copy.md) komutu dosyaları hedeften silmez, bu nedenle kaynakta artık yokken hedefteki dosyaları silmek istiyorsanız, `--delete-destination` [azcopy eşitleme](storage-ref-azcopy-sync.md) komutunu kullanarak bayrak kümesini bir değere `true` veya `prompt`. 

## <a name="troubleshoot-issues"></a>Sorunları giderme

AzCopy her iş için günlük ve plan dosyaları oluşturur. Günlükleri, olası sorunları araştırmak ve sorun gidermek için kullanabilirsiniz. 

Günlükler hata durumunu (,`UPLOADFAILED` `COPYFAILED`ve `DOWNLOADFAILED`, tam yol ve hata nın nedenini içerir.

Varsayılan olarak, günlük ve plan dosyaları `%USERPROFILE%\.azcopy` Windows dizininde `$HOME$\.azcopy` veya Mac ve Linux'taki dizinde bulunur, ancak isterseniz bu konumu değiştirebilirsiniz.

İlgili hata, dosyada görünen ilk hata olması gerekmez. Ağ hataları, zaman zaman alakart ve Sunucu Meşgul hataları gibi hatalar için AzCopy 20 kata kadar yeniden dener ve genellikle yeniden deneme işlemi başarılı olur.  Gördüğünüz ilk hata, başarıyla yeniden denenmiş zararsız bir şey olabilir.  Bu nedenle, dosyadaki ilk hataya bakmak yerine, yakın `UPLOADFAILED` `COPYFAILED`olan `DOWNLOADFAILED`hataları arayın, veya . 

> [!IMPORTANT]
> Microsoft Destek'e bir istek gönderirken (veya herhangi bir üçüncü tarafı ilgilendiren sorunu giderirken), yürütmek istediğiniz komutun redacted sürümünü paylaşın. Bu, SAS'ın yanlışlıkla kimseyle paylaşılmaması için dir. Yeniden düzenlenmiş sürümü günlük dosyasının başında bulabilirsiniz.

### <a name="review-the-logs-for-errors"></a>Hatalar için günlükleri gözden geçirme

Aşağıdaki komut `UPLOADFAILED` `04dc9ca9-158f-7945-5933-564021086c79` günlük ten durum ile tüm hataları alırsınız:

**Windows (PowerShell)**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**Linux**

```
grep UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

### <a name="view-and-resume-jobs"></a>İşleri görüntüleme ve devam ettirme

Her aktarım işlemi bir AzCopy iş oluşturur. İşlerin geçmişini görüntülemek için aşağıdaki komutu kullanın:

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

Başarısız/iptal edilmiş bir işe devam etmek için aşağıdaki komutu kullanın. Bu komut, güvenlik nedenleriyle kalıcı olmadığından, tanımlayıcısını SAS belirteciyle birlikte kullanır:

```
azcopy jobs resume <job-id> --source-sas="<sas-token>"
azcopy jobs resume <job-id> --destination-sas="<sas-token>"
```

> [!TIP]
> SAS belirteci gibi yol bağımsız değişkenlerini tek tırnak işaretleriyle ('') belirtin. Windows Komut Uyruşu (cmd.exe) hariç tüm komut kabuklarında tek tırnak işareti kullanın. Windows Komut Uyruşu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak ('') yerine çift tırnak ("") içeren bir şekilde içine çekin.

Bir işe devam ettiğinizde, AzCopy iş planı dosyasına bakar. Plan dosyası, iş ilk oluşturulduğunda işlenmek için tanımlanan tüm dosyaları listeler. Bir işe devam ettiğinizde, AzCopy, plan dosyasında listelenen ve aktarılmadı olan tüm dosyaları aktarmayı dener.

## <a name="change-the-location-of-the-plan-and-log-files"></a>Planın konumunu değiştirme ve dosyaları günlüğe kaydetme

Varsayılan olarak, plan ve günlük dosyaları `%USERPROFILE%\.azcopy` Windows'daki dizinde `$HOME$\.azcopy` veya Mac ve Linux dizininde bulunur. Bu konumu değiştirebilirsiniz.

### <a name="change-the-location-of-plan-files"></a>Plan dosyalarının konumunu değiştirme

Bu komutlardan herhangi birini kullanın.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_JOB_PLAN_LOCATION=<value>` |
| **Linux** | `export AZCOPY_JOB_PLAN_LOCATION=<value>` |
| **Macos** | `export AZCOPY_JOB_PLAN_LOCATION=<value>` |

Bu `azcopy env` değişkenin geçerli değerini denetlemek için kullanın. Değer boşsa, plan dosyaları varsayılan konuma yazılır.

### <a name="change-the-location-of-log-files"></a>Günlük dosyalarının konumunu değiştirme

Bu komutlardan herhangi birini kullanın.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AZCOPY_LOG_LOCATION=<value>` |
| **Linux** | `export AZCOPY_LOG_LOCATION=<value>` |
| **Macos** | `export AZCOPY_LOG_LOCATION=<value>` |

Bu `azcopy env` değişkenin geçerli değerini denetlemek için kullanın. Değer boşsa, günlükler varsayılan konuma yazılır.

## <a name="change-the-default-log-level"></a>Varsayılan günlük düzeyini değiştirme

Varsayılan olarak, AzCopy günlük `INFO`düzeyi . Disk alanı kaydetmek için günlük verme süresini azaltmak istiyorsanız, ``--log-level`` seçeneği kullanarak bu ayarın üzerine yazın. 

Mevcut günlük seviyeleri `NONE` `DEBUG`şunlardır: `PANIC`, `FATAL`, `INFO` `WARNING`, `ERROR`, , ve .

## <a name="remove-plan-and-log-files"></a>Plan ve günlük dosyalarını kaldırma

Disk alanı kaydetmek için tüm planı ve günlük dosyalarını yerel `azcopy jobs clean` makinenizden kaldırmak istiyorsanız, komutu kullanın.

Planı kaldırmak ve yalnızca bir iş ile `azcopy jobs rm <job-id>`ilişkili dosyaları günlüğe kaydetmek için. Bu `<job-id>` örnekteki yer tutucuyu işin iş kimliğiyle değiştirin.


