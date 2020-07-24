---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/11/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: da5811abec889bcc47d08878a0950df7f0983663
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87010843"
---
*Önbelleği hazırlama*  
ReadOnly konak önbelleğe alma ile disk, disk sınırından daha yüksek ıOPS verebilir. Ana bilgisayar önbelleğinden bu en büyük okuma performansını almak için öncelikle bu diskin önbelleğini ısınma yapmanız gerekir. Bu, sınama aracının, benchereads birimini önbelleğe almasını sağlar ve aslında diski doğrudan değil, önbelleğe alır. Önbellek İsabetleri, tek önbellek etkin diskten ek ıOPS ile sonuçlanır.

> [!IMPORTANT]
> Sanal makine her yeniden başlatıldığında, benchişaretlemesini çalıştırmadan önce önbelleği yeniden ısınma yapmanız gerekir.

## <a name="tools"></a>Araçlar

### <a name="iometer"></a>İometer

SANAL makinede [Iometer aracını indirin](https://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) .

#### <a name="test-file"></a>Test dosyası

İometer, sınama testi çalıştırdığınız birimde depolanan bir test dosyası kullanır. BT sürücüleri, disk ıOPS ve aktarım hızını ölçmek için bu test dosyasını okur ve yazar. Iometer, bir tane sağlamadıysanız bu test dosyasını oluşturur. CacheReads ve Nocacheyazmaları birimlerinde ıobw. tst adlı bir 200 GB test dosyası oluşturun.

#### <a name="access-specifications"></a>Erişim belirtimleri

Özellikler, istek GÇ boyutu,% okuma/yazma,% rastgele/sıralı, Iometer içindeki "erişim belirtimleri" sekmesi kullanılarak yapılandırılır. Aşağıda açıklanan senaryoların her biri için bir erişim belirtimi oluşturun. Erişim belirtimlerini oluşturun ve – Rasgeleyazmaları \_ 8k, rastgele okuma 8k gibi uygun bir adla "Kaydet" i oluşturun \_ . Test senaryosunu çalıştırırken ilgili belirtimi seçin.

En büyük yazma ıOPS senaryosuna yönelik erişim belirtimlerine bir örnek aşağıda verilmiştir.  
    ![Maksimum yazma ıOPS için erişim belirtimleri örneği](../articles/virtual-machines/linux/media/premium-storage-performance/image8.png)

#### <a name="maximum-iops-test-specifications"></a>Maksimum ıOPS test belirtimleri

Maksimum IOPS 'yi göstermek için, daha küçük bir istek boyutu kullanın. 8K istek boyutunu kullanın ve rastgele yazma ve okuma için belirtim oluşturun.

| Erişim belirtimi | İstek boyutu | Fi | Okuyamaz |
| --- | --- | --- | --- |
| Rastgele yazma \_ 8k |8K |100 |0 |
| Rastgele okuma \_ 8k |8K |100 |100 |

#### <a name="maximum-throughput-test-specifications"></a>En yüksek işleme testi belirtimleri

En yüksek aktarım hızını göstermek için daha büyük istek boyutunu kullanın. 64 K istek boyutunu kullanın ve rastgele yazma ve okuma için belirtim oluşturun.

| Erişim belirtimi | İstek boyutu | Fi | Okuyamaz |
| --- | --- | --- | --- |
| Rastgele yazar \_ 64K |64 K |100 |0 |
| Rastgele okuma \_ 64K |64 K |100 |100 |

#### <a name="run-the-iometer-test"></a>Iometer testini çalıştırma

Önbelleği ısınma için aşağıdaki adımları gerçekleştirin

1. Aşağıda gösterilen değerlerle iki erişim belirtimleri oluşturun,

   | Name | İstek boyutu | Fi | Okuyamaz |
   | --- | --- | --- | --- |
   | Rastgele yazar \_ 1 MB |1 MB |100 |0 |
   | Rastgele okunan \_ 1MB |1 MB |100 |100 |
1. Önbellek diskini başlatmak için aşağıdaki parametrelerle birlikte Iometer testini çalıştırın. Hedef birim ve 128 sıra derinliği için üç çalışan iş parçacığı kullanın. "Test kurulumu" sekmesinde testin "çalışma süresi" süresini 2 saat olarak ayarlayın.

   | Senaryo | Hedef birim | Name | Süre |
   | --- | --- | --- | --- |
   | Önbellek diskini Başlat |CacheReads |Rastgele yazar \_ 1 MB |2 saat |
1. Önbellek diskini aşağıdaki parametrelerle birlikte hazırlama için Iometer testini çalıştırın. Hedef birim ve 128 sıra derinliği için üç çalışan iş parçacığı kullanın. "Test kurulumu" sekmesinde testin "çalışma süresi" süresini 2 saat olarak ayarlayın.

   | Senaryo | Hedef birim | Name | Süre |
   | --- | --- | --- | --- |
   | Önbellek diskini ısınma |CacheReads |Rastgele okunan \_ 1MB |2 saat |

Önbellek diski çarpdıktan sonra, aşağıda listelenen test senaryolarına devam edin. Iometer testini çalıştırmak için, **her** bir hedef birim için en az üç çalışan iş parçacığı kullanın. Her çalışan iş parçacığı için, hedef birimi seçin, sıra derinliğini ayarlayın ve ilgili test senaryosunu çalıştırmak için aşağıdaki tabloda gösterildiği gibi kaydedilmiş test belirtimlerinden birini seçin. Tabloda Ayrıca bu testler çalıştırılırken ıOPS ve aktarım hızı için beklenen sonuçlar gösterilmektedir. Tüm senaryolarda, 8 KB küçük GÇ boyutu ve 128 yüksek bir sıra derinliği kullanılır.

| Test Senaryosu | Hedef birim | Name | Sonuç |
| --- | --- | --- | --- |
| En çok, IOPS 'yi oku |CacheReads |Rastgele yazma \_ 8k |50.000 ıOPS |
| En çok, IOPS yaz |Nocacheyazmaları |Rastgele okuma \_ 8k |64.000 ıOPS |
| En çok, Birleşik ıOPS |CacheReads |Rastgele yazma \_ 8k |100.000 ıOPS |
| Nocacheyazmaları |Rastgele okuma \_ 8k | &nbsp; | &nbsp; |
| En çok, Okunan MB/sn |CacheReads |Rastgele yazar \_ 64K |524 MB/sn |
| En çok, Yazma MB/sn |Nocacheyazmaları |Rastgele okuma \_ 64K |524 MB/sn |
| Birleşik MB/sn |CacheReads |Rastgele yazar \_ 64K |1000 MB/sn |
| Nocacheyazmaları |Rastgele okuma \_ 64K | &nbsp; | &nbsp; |

Aşağıda Birleşik ıOPS ve aktarım hızı senaryoları için Iometer test sonuçlarının ekran görüntüleri verilmiştir.

#### <a name="combined-reads-and-writes-maximum-iops"></a>Birleşik okuma ve yazma maksimum ıOPS

![Birleşik okuma ve yazma maksimum ıOPS](../articles/virtual-machines/linux/media/premium-storage-performance/image9.png)

#### <a name="combined-reads-and-writes-maximum-throughput"></a>Birleşik okuma ve yazma en fazla aktarım hızı

![Birleşik okuma ve yazma en fazla aktarım hızı](../articles/virtual-machines/linux/media/premium-storage-performance/image10.png)

### <a name="fio"></a>FIO

FIO, Linux VM 'lerinde kıyaslama depolaması için popüler bir araçtır. Farklı GÇ boyutları, sıralı veya rastgele okuma ve yazma esnekliği vardır. Belirtilen g/ç işlemlerini gerçekleştirmek için çalışan iş parçacıklarını veya işlemlerini işler. Her çalışan iş parçacığının iş dosyalarını kullanarak gerçekleştirmesi gereken g/ç işlemlerinin türünü belirtebilirsiniz. Aşağıdaki örneklerde gösterildiği her senaryo için bir iş dosyası oluşturduk. Bu iş dosyalarındaki belirtimleri, Premium depolamada çalışan farklı iş yükleriyle Kıyaslanılacak şekilde değiştirebilirsiniz. Örneklerde, **Ubuntu**çalıştıran standart bir DS 14 VM kullanılmaktadır. Benchişaretleme bölümünün başlangıcında açıklanan kurulum 'u kullanın ve sınama testlerini çalıştırmadan önce önbelleğin düzeyini ayarlayın.

Başlamadan önce, [FIO 'u indirin](https://github.com/axboe/fio) ve sanal makinenize yükleyin.

Ubuntu için aşağıdaki komutu çalıştırın,

```
apt-get install fio
```

Disklerde okuma işlemlerini yapmak için yazma işlemlerini ve dört çalışan iş parçacığını yönlendiren dört çalışan iş parçacığı kullanıyoruz. Yazma çalışanları "none" olarak ayarlanmış 10 disk olan "NoCache" biriminde trafiği yönlendirmiştir. Okuma çalışanları, "ReadOnly" olarak ayarlanmış bir disk olan "readcache" biriminde trafiği yönlendirmiştir.

#### <a name="maximum-write-iops"></a>Maksimum yazma ıOPS 'si

En yüksek yazma ıOPS 'sini almak için aşağıdaki belirtimlerle iş dosyasını oluşturun. "fiowrite.ini" olarak adlandırın.

```ini
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Önceki bölümlerde ele alınan tasarım yönergeleriyle birlikte aşağıdaki önemli noktalara dikkat edin. Bu belirtimler, maksimum ıOPS 'yi sürücü için gereklidir,  

* 256 yüksek sıra derinliği.  
* 8 KB 'lık küçük bir blok boyutu.  
* Rastgele yazma işlemleri gerçekleştiren birden çok iş parçacığı.

FIO testini 30 saniye boyunca çalıştırmak için aşağıdaki komutu çalıştırın.  

```
sudo fio --runtime 30 fiowrite.ini
```

Test çalışırken, VM ve Premium disklerin teslim aldığı yazma ıOPS sayısını görebilirsiniz. Aşağıdaki örnekte gösterildiği gibi, DS14 VM, 50.000 ıOPS 'nin en büyük yazma ıOPS sınırını dağıtmakta.  
    ![Yazma ıOPS VM 'si ve Premium diskler teslim edilir.](../articles/virtual-machines/linux/media/premium-storage-performance/image11.png)

#### <a name="maximum-read-iops"></a>Maksimum okuma ıOPS 'si

Maksimum okuma ıOPS 'sini almak için aşağıdaki belirtimlerle iş dosyasını oluşturun. "fioread.ini" olarak adlandırın.

```ini
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Önceki bölümlerde ele alınan tasarım yönergeleriyle birlikte aşağıdaki önemli noktalara dikkat edin. Bu belirtimler, maksimum ıOPS 'yi sürücü için gereklidir,

* 256 yüksek sıra derinliği.  
* 8 KB 'lık küçük bir blok boyutu.  
* Rastgele yazma işlemleri gerçekleştiren birden çok iş parçacığı.

FIO testini 30 saniye boyunca çalıştırmak için aşağıdaki komutu çalıştırın.

```
sudo fio --runtime 30 fioread.ini
```

Test çalışırken, VM ve Premium disklerin teslim aldığı okuma ıOPS sayısını görebilirsiniz. Aşağıdaki örnekte gösterildiği gibi, DS14 sanal makinesi 64.000 'den fazla okuma ıOPS 'yi teslim ediyor. Bu, diskin ve önbellek performansının bir birleşimidir.  
    ![Yazma ıOPS sanal makinesi ve Premium disklerinin sayısını teslim eden ekran görüntüsü.](../articles/virtual-machines/linux/media/premium-storage-performance/image12.png)

#### <a name="maximum-read-and-write-iops"></a>Maksimum okuma ve yazma ıOPS 'si

En yüksek Birleşik okuma ve yazma ıOPS 'yi almak için iş dosyasını aşağıdaki belirtimlerle oluşturun. "fioreadwrite.ini" olarak adlandırın.

```ini
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Önceki bölümlerde ele alınan tasarım yönergeleriyle birlikte aşağıdaki önemli noktalara dikkat edin. Bu belirtimler, maksimum ıOPS 'yi sürücü için gereklidir,

* 128 yüksek sıra derinliği.  
* 4 KB 'lık küçük bir blok boyutu.  
* Rastgele okuma ve yazma işlemleri gerçekleştiren birden çok iş parçacığı.

FIO testini 30 saniye boyunca çalıştırmak için aşağıdaki komutu çalıştırın.

```
sudo fio --runtime 30 fioreadwrite.ini
```

Test çalışırken, sanal makine ve Premium disklerin teslim edilen birleştirilmiş okuma ve yazma ıOPS sayısını görebilirsiniz. Aşağıdaki örnekte gösterildiği gibi, DS14 sanal makinesi 100.000 ' den fazla birleştirilmiş okuma ve yazma ıOPS 'yi teslim ediyor. Bu, diskin ve önbellek performansının bir birleşimidir.  
    ![Birleşik okuma ve yazma ıOPS](../articles/virtual-machines/linux/media/premium-storage-performance/image13.png)

#### <a name="maximum-combined-throughput"></a>En yüksek Birleşik verimlilik

En yüksek Birleşik okuma ve yazma verimini almak için, okuma ve yazma işlemleri yapan birden fazla iş parçacığı ile daha büyük bir blok boyutu ve büyük sıra derinliği kullanın. 64 KB 'lik bir blok boyutu ve 128 sıra derinliğini kullanabilirsiniz.
