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
ms.openlocfilehash: e5148ff9e92a2e550a3117356a4e77cbac8fc6f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67673555"
---
*Önbelleği Isınma*  
ReadOnly ana bilgisayar önbelleğe sahip disk, disk sınırından daha yüksek IOPS verebilir. Ana bilgisayar önbelleğinden bu maksimum okuma performansını elde etmek için önce bu diskin önbelleğini ısıtmanız gerekir. Bu, Okuma IOs'un, kıyaslama aracının Önbellek hacminde sürmesini, diske doğrudan değil, önbelleğe çarpmasını sağlar. Önbellek isabetleri, tek önbellek etkin diskten ek IOPS ile sonuçlanır.

> [!IMPORTANT]
> Karşılaştırma yı çalıştırmadan önce, VM her yeniden başlatılındığında önbelleği ısıtmanız gerekir.

## <a name="tools"></a>Araçlar

### <a name="iometer"></a>İyometre

[VM'deki Iometer aracını indirin.](https://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download)

#### <a name="test-file"></a>Test dosyası

Iometer, kıyaslama testini çalıştırdığınız birimde depolanan bir test dosyası kullanır. Disk IOPS ve Throughput ölçmek için bu test dosyasıüzerinde Okumave Yazma sürücüler. Iometer, sağlamadıysanız bu test dosyasini oluşturur. Önbellekler ve NoCacheWrites birimlerinde iobw.tst adlı 200 GB'lık bir test dosyası oluşturun.

#### <a name="access-specifications"></a>Erişim özellikleri

Belirtimler, istek IO boyutu, % okuma/yazma, % rasgele/sıralı Iometer'deki "Erişim Özellikleri" sekmesi kullanılarak yapılandırılır. Aşağıda açıklanan senaryoların her biri için bir erişim belirtimi oluşturun. RandomWrites 8K, RandomReads\_\_8K gibi uygun bir adla erişim özellikleri ve "Kaydet" oluşturun. Test senaryosunu çalıştırırken ilgili belirtimi seçin.

Maksimum Yazma IOPS senaryosu için erişim belirtimlerinin bir örneği aşağıda gösterilmiştir,  
    ![Maksimum yazma IOPS için erişim belirtimleri örneği](../articles/virtual-machines/linux/media/premium-storage-performance/image8.png)

#### <a name="maximum-iops-test-specifications"></a>Maksimum IOPS test özellikleri

Maksimum IOPs göstermek için, daha küçük istek boyutu kullanın. 8K istek boyutunu kullanın ve Rastgele Yazma ve Okumalar için belirtimler oluşturun.

| Erişim Belirtimi | İstek boyutu | Rastgele % | Okuma % |
| --- | --- | --- | --- |
| RandomWrites\_8K |8K |100 |0 |
| RandomReads\_8K |8K |100 |100 |

#### <a name="maximum-throughput-test-specifications"></a>Maksimum iş testi özellikleri

Maksimum Verimi göstermek için daha büyük istek boyutunu kullanın. 64 K istek boyutunu kullanın ve Rastgele Yazma ve Okumalar için belirtimler oluşturun.

| Erişim Belirtimi | İstek boyutu | Rastgele % | Okuma % |
| --- | --- | --- | --- |
| RandomWrites\_64K |64 K |100 |0 |
| RandomReads\_64K |64 K |100 |100 |

#### <a name="run-the-iometer-test"></a>Iometer testini çalıştırın

Önbelleği ısıtmak için aşağıdaki adımları gerçekleştirin

1. Aşağıda gösterilen değerlerle iki erişim belirtimi oluşturun,

   | Adı | İstek boyutu | Rastgele % | Okuma % |
   | --- | --- | --- | --- |
   | RandomWrites\_1MB |1 MB |100 |0 |
   | RandomReads\_1MB |1 MB |100 |100 |
1. Önbellek diskini aşağıdaki parametrelerle başlatmaiçin Iometer testini çalıştırın. Hedef birim ve 128 sıra derinliği için üç alt iş parçacığı kullanın. "Test Kurulumu" sekmesinde testin "Çalışma süresi" süresini 2 saatolarak ayarlayın.

   | Senaryo | Hedef Hacim | Adı | Süre |
   | --- | --- | --- | --- |
   | Önbellek Diskini Başlatma |ÖnbellekLer |RandomWrites\_1MB |2 saat |
1. Önbellek diskini aşağıdaki parametrelerle ısıtmak için Iometer testini çalıştırın. Hedef birim ve 128 sıra derinliği için üç alt iş parçacığı kullanın. "Test Kurulumu" sekmesinde testin "Çalışma süresi" süresini 2 saatolarak ayarlayın.

   | Senaryo | Hedef Hacim | Adı | Süre |
   | --- | --- | --- | --- |
   | Önbellek Diskini Isıt |ÖnbellekLer |RandomReads\_1MB |2 saat |

Önbellek diski ısındıktan sonra, aşağıda listelenen test senaryolarına devam edin. Iometer testini çalıştırmak **için, her** hedef birim için en az üç alt iş parçacığı kullanın. Her alt iş parçacığı için hedef birim seçin, sıra derinliğini ayarlayın ve ilgili test senaryosunu çalıştırmak için aşağıdaki tabloda gösterildiği gibi kaydedilen test özelliklerinden birini seçin. Tablo da bu testleri çalıştırırken IOPS ve ThroughPut için beklenen sonuçları gösterir. Tüm senaryolar için, 8 KB küçük bir IO boyutu ve 128 yüksek sıra derinliği kullanılır.

| Test Senaryosu | Hedef Hacim | Adı | Sonuç |
| --- | --- | --- | --- |
| En çok, IOPS'u Okuyun |ÖnbellekLer |RandomWrites\_8K |50.000 IOPS |
| En çok, IOPS Yaz |NoCacheWrites |RandomReads\_8K |64.000 IOPS |
| En çok, Kombine IOPS |ÖnbellekLer |RandomWrites\_8K |100.000 IOPS |
| NoCacheWrites |RandomReads\_8K | &nbsp; | &nbsp; |
| En çok, MB/sn oku |ÖnbellekLer |RandomWrites\_64K |524 MB/sn |
| En çok, MB/sn yazın |NoCacheWrites |RandomReads\_64K |524 MB/sn |
| Kombine MB/sn |ÖnbellekLer |RandomWrites\_64K |1000 MB/sn |
| NoCacheWrites |RandomReads\_64K | &nbsp; | &nbsp; |

Aşağıda, birleşik IOPS ve İş Sonu senaryoları için Iometer test sonuçlarının ekran görüntüleri verilmiştir.

#### <a name="combined-reads-and-writes-maximum-iops"></a>Kombine okuma ve maksimum IOPS yazıyor

![Kombine Okuma ve Maksimum IOPS Yazıyor](../articles/virtual-machines/linux/media/premium-storage-performance/image9.png)

#### <a name="combined-reads-and-writes-maximum-throughput"></a>Kombine okuma ve maksimum verim yazma

![Kombine Okuma ve Maksimum Verim Yazma](../articles/virtual-machines/linux/media/premium-storage-performance/image10.png)

### <a name="fio"></a>FIO

FIO, Linux VM'lerinde depolamayı kıyaslamak için popüler bir araçtır. Farklı IO boyutlarını, sıralı veya rasgele okuma ve yazmaları seçme esnekliğine sahiptir. Belirtilen G/Ç işlemlerini gerçekleştirmek için alt iş parçacıkları veya işlemler oluşturur. Her alt iş parçacığının iş dosyalarını kullanarak gerçekleştirmesi gereken G/Ç işlemlerinin türünü belirtebilirsiniz. Aşağıdaki örneklerde gösterilen senaryo başına bir iş dosyası oluşturduk. Bu iş dosyalarındaki özellikleri, Premium Depolama'da çalışan farklı iş yüklerini karşılaştırmak için değiştirebilirsiniz. Örneklerde, **Ubuntu**çalıştıran bir Standart DS 14 VM kullanıyoruz. Kıyaslama bölümünün başında açıklanan aynı kurulumu kullanın ve kıyaslama testlerini çalıştırmadan önce önbelleği ısıtın.

Başlamadan önce [FIO'ya indirin](https://github.com/axboe/fio) ve sanal makinenize yükleyin.

Ubuntu için aşağıdaki komutu çalıştırın,

```
apt-get install fio
```

Yazma işlemlerini sürmek için dört alt iş parçacığı ve disklerde Okuma işlemlerini yönlendirmek için dört alt iş parçacığı kullanırız. Yazma çalışanları, "Yok" olarak ayarlanmış önbellekli 10 diski olan "nocache" biriminde trafik sürüşü yapıyor. Read çalışanları, önbelleği "ReadOnly" olarak ayarlanmış bir diski olan "readcache" biriminde trafiği yönlendiriyor.

#### <a name="maximum-write-iops"></a>Maksimum yazma IOPS

Maksimum Yazma IOPS almak için aşağıdaki özellikleri ile iş dosyası oluşturun. Adını "fiowrite.ini" olarak adlandır.

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

Önceki bölümlerde tartışılan tasarım yönergelerine uygun olan aşağıdaki önemli şeyleri izleyin. Bu özellikler maksimum IOPS sürücü için gereklidir,  

* 256 yüksek sıra derinliği.  
* 8 KB küçük bir blok boyutu.  
* Rasgele yazma yapan birden çok iş parçacığı.

FIO testini 30 saniye başlatmak için aşağıdaki komutu çalıştırın,  

```
sudo fio --runtime 30 fiowrite.ini
```

Test çalışırken, VM ve Premium disklerin sunduğu IOPS yazma sayısını görebilirsiniz. Aşağıdaki örnekte gösterildiği gibi, DS14 VM maksimum yazma IOPS limitini 50.000 IOPS olarak teslim ediyor.  
    ![Yazma IOPS VM ve Premium disklerin sayısı teslim](../articles/virtual-machines/linux/media/premium-storage-performance/image11.png)

#### <a name="maximum-read-iops"></a>Maksimum okuma IOPS

Maksimum Okuma IOPS almak için aşağıdaki özellikleri içeren iş dosyasını oluşturun. Adını "fioread.ini" söyle.

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

Önceki bölümlerde tartışılan tasarım yönergelerine uygun olan aşağıdaki önemli şeyleri izleyin. Bu özellikler maksimum IOPS sürücü için gereklidir,

* 256 yüksek sıra derinliği.  
* 8 KB küçük bir blok boyutu.  
* Rasgele yazma yapan birden çok iş parçacığı.

FIO testini 30 saniye başlatmak için aşağıdaki komutu çalıştırın,

```
sudo fio --runtime 30 fioread.ini
```

Test çalışırken, VM ve Premium disklerin sunduğu okuma IOPS sayısını görebilirsiniz. Aşağıdaki örnekte gösterildiği gibi, DS14 VM 64.000'den fazla Read IOPS teslim ediyor. Bu, disk ve önbellek performansının bir birleşimidir.  
    ![](../articles/virtual-machines/linux/media/premium-storage-performance/image12.png)

#### <a name="maximum-read-and-write-iops"></a>Maksimum okuma ve yazma IOPS

En fazla birleştirilmiş Okuma ve Yazma IOPS'yi almak için aşağıdaki belirtimleri içeren iş dosyasını oluşturun. Adını "fioreadwrite.ini" olarak adlandır.

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

Önceki bölümlerde tartışılan tasarım yönergelerine uygun olan aşağıdaki önemli şeyleri izleyin. Bu özellikler maksimum IOPS sürücü için gereklidir,

* 128 yüksek sıra derinliği.  
* 4 KB küçük bir blok boyutu.  
* Rasgele okuma ve yazma gerçekleştiren birden çok iş parçacığı.

FIO testini 30 saniye başlatmak için aşağıdaki komutu çalıştırın,

```
sudo fio --runtime 30 fioreadwrite.ini
```

Test çalışırken, VM ve Premium disklerin sunduğu IOPS'yi birlikte okuma ve yazma sayısını görebilirsiniz. Aşağıdaki örnekte gösterildiği gibi, DS14 VM 100.000'den fazla kombine Okuma ve Yazma IOPS teslim ediyor. Bu, disk ve önbellek performansının bir birleşimidir.  
    ![Kombine okuma ve yazma IOPS](../articles/virtual-machines/linux/media/premium-storage-performance/image13.png)

#### <a name="maximum-combined-throughput"></a>Maksimum kombine iş verimi

En fazla birleştirilmiş Okuma ve Yazma İş Parçacığı elde etmek için, okuma ve yazma yapan birden çok iş parçacığı yla daha büyük bir blok boyutu ve büyük kuyruk derinliği kullanın. 64 KB blok boyutu ve 128 sıra derinliği kullanabilirsiniz.
