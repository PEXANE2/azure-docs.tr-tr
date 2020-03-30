---
title: Azure için verileri Avere vFXT'ye taşıma
description: Azure için Avere vFXT ile kullanılmak üzere yeni bir depolama birimine veri ekleme
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: rohogue
ms.openlocfilehash: c2a38b20fff789faf370e3161a92a31ed5f04c57
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153727"
---
# <a name="moving-data-to-the-vfxt-cluster---parallel-data-ingest"></a>VFXT kümesine veri taşıma - Paralel veri alma

Yeni bir vFXT kümesi oluşturduktan sonra, ilk göreviniz verileri Azure'daki yeni bir depolama birimine taşımak olabilir. Ancak, verileri taşıma her zamanki yönteminiz bir istemciden basit bir kopyalama komutu veriyorsa, büyük olasılıkla yavaş bir kopyalama performansı görürsünüz. Tek iş parçacığı kopyalama, verileri Avere vFXT kümesinin arka uç depolamasına kopyalamak için iyi bir seçenek değildir.

Azure kümesi için Avere vFXT ölçeklenebilir çok istemcili bir önbellek olduğundan, verileri kopyalamanın en hızlı ve en verimli yolu birden çok istemciyle dir. Bu teknik, dosya ve nesnelerin yutulması paralelleştirir.

![Çok istemcili, çok iş parçacığı veri hareketini gösteren diyagram: Sol üstte, şirket içi donanım depolama için bir simgenin birden çok ok gelir. Oklar dört istemci makinesini işaret ediyor. Her istemci makineden üç ok Avere vFXT doğru işaret. Avere vFXT'den birden fazla ok Blob depolama alanını işaret eder.](media/avere-vfxt-parallel-ingest.png)

Verileri ``cp`` ``copy`` bir depolama sisteminden diğerine aktarmak için yaygın olarak kullanılan komutlar, aynı anda yalnızca bir dosyayı kopyalayan tek iş parçacığı işlemleridir. Bu, dosya sunucusunun aynı anda yalnızca bir dosya yutturdösü anlamına gelir ve bu da kümenin kaynaklarının boşa harcadığı anlamına gelir.

Bu makalede, verileri Avere vFXT kümesine taşımak için çok istemcili, çok iş parçacığı dosya kopyalama sistemi oluşturma stratejileri açıklanmaktadır. Birden çok istemci ve basit kopyalama komutları kullanarak verimli veri kopyalama için kullanılabilecek dosya aktarım kavramlarını ve karar noktalarını açıklar.

Ayrıca yardımcı olabilecek bazı yardımcı programları açıklar. Yardımcı ``msrsync`` program, bir veri kümesini kovalara bölme ve komutları kullanma ``rsync`` işlemini kısmen otomatikleştirmek için kullanılabilir. Komut ``parallelcp`` dosyası, kaynak dizinini okuyan ve kopyalama komutlarını otomatik olarak veren başka bir yardımcı programdır. Ayrıca, ``rsync`` araç hala veri tutarlılığı sağlayan daha hızlı bir kopyasını sağlamak için iki aşamada kullanılabilir.

Bir bölüme atlamak için bağlantıyı tıklatın:

* [El ile kopyalama örneği](#manual-copy-example) - Kopyalama komutlarını kullanarak ayrıntılı bir açıklama
* [İki fazlı rsync örneği](#use-a-two-phase-rsync-process)
* [Kısmen otomatik (msrsync) örneği](#use-the-msrsync-utility)
* [Paralel kopya örneği](#use-the-parallel-copy-script)

## <a name="data-ingestor-vm-template"></a>Veri yutma VM şablonu

Bu makalede belirtilen paralel veri alma araçlarıyla otomatik olarak bir VM oluşturmak için GitHub'da bir Kaynak Yöneticisi şablonu kullanılabilir.

![blob depolama, donanım depolama ve Azure dosya kaynaklarından her biri birden çok ok gösteren diyagram. Oklar bir "veri yutuveya vm" işaret ve oradan, birden fazla ok Avere vFXT işaret](media/avere-vfxt-ingestor-vm.png)

Veri yutma VM yeni oluşturulan VM Avere vFXT küme bağlar ve kümeden bootstrap komut indirir bir öğretici parçasıdır. Ayrıntılar için [Bootstrap'ı veri alma vm'ini](https://github.com/Azure/Avere/blob/master/docs/data_ingestor.md) okuyun.

## <a name="strategic-planning"></a>Stratejik planlama

Verileri paralel olarak kopyalamak için bir strateji tasarlarken, dosya boyutu, dosya sayısı ve dizin derinliğindeki dengeleri anlamanız gerekir.

* Dosyalar küçük olduğunda, ilgi ölçüsü saniyedeki dosyalardır.
* Dosyalar büyükolduğunda (10MiBi veya daha büyük), ilgi ölçüsü saniyede bayttır.

Her kopyalama işleminde, kopyalama komutunun uzunluğunun zamanlaması ve dosya boyutunun ve dosya sayısının çarpanabinat edilmesiyle ölçülebilen bir işlem hızı ve dosya aktarılabilen bir hız vardır. Oranların nasıl ölçüleceğini açıklamak bu belgenin kapsamı dışındadır, ancak küçük veya büyük dosyalarla mı uğraşacağınız önemlidir.

## <a name="manual-copy-example"></a>Manuel kopyalama örneği

Önceden tanımlanmış dosya veya yol kümelerine karşı arka planda aynı anda birden fazla kopya komutu çalıştırarak istemciüzerinde çok iş parçacığı kopyasını el ile oluşturabilirsiniz.

Linux/UNIX ``cp`` komutu, ``-p`` sahipliği ve mtime meta verilerini korumak için bağımsız değişkeni içerir. Aşağıdaki komutlara bu bağımsız değişkenin eklenmesi isteğe bağlıdır. (Bağımsız değişken ekleme, istemciden gönderilen dosya sistemi çağrılarının sayısını meta veri değişikliği için hedef dosya sistemine artırır.)

Bu basit örnek iki dosyayı paralel olarak kopyalar:

```bash
cp /mnt/source/file1 /mnt/destination1/ & cp /mnt/source/file2 /mnt/destination1/ &
```

Bu komutu verdikten `jobs` sonra, komut iki iş parçacığının çalıştığını gösterir.

### <a name="predictable-filename-structure"></a>Öngörülebilir dosya adı yapısı

Dosya adlarınız öngörülebilirse, paralel kopyalama iş parçacıkları oluşturmak için ifadeleri kullanabilirsiniz.

Örneğin, dizininiz sırayla `0001` numaralandırılan 1000 dosya `1000`içeriyorsa, her biri 100 dosyayı kopyalayan on paralel iş parçacığı oluşturmak için aşağıdaki ifadeleri kullanabilirsiniz:

```bash
cp /mnt/source/file0* /mnt/destination1/ & \
cp /mnt/source/file1* /mnt/destination1/ & \
cp /mnt/source/file2* /mnt/destination1/ & \
cp /mnt/source/file3* /mnt/destination1/ & \
cp /mnt/source/file4* /mnt/destination1/ & \
cp /mnt/source/file5* /mnt/destination1/ & \
cp /mnt/source/file6* /mnt/destination1/ & \
cp /mnt/source/file7* /mnt/destination1/ & \
cp /mnt/source/file8* /mnt/destination1/ & \
cp /mnt/source/file9* /mnt/destination1/
```

### <a name="unknown-filename-structure"></a>Bilinmeyen dosya adı yapısı

Dosya adlandırma yapınız öngörülebilir değilse, dosyaları dizin adlarıyla gruplandırmayapabilirsiniz.

Bu örnek, arka plan görevleri ``cp`` olarak çalıştırılan komutlara göndermek için tüm dizinleri toplar:

```bash
/root
|-/dir1
| |-/dir1a
| |-/dir1b
| |-/dir1c
   |-/dir1c1
|-/dir1d
```

Dosyalar toplandıktan sonra, alt dizinleri ve tüm içeriğini özyinelemeli olarak kopyalamak için paralel kopyalama komutları çalıştırabilirsiniz:

```bash
cp /mnt/source/* /mnt/destination/
mkdir -p /mnt/destination/dir1 && cp /mnt/source/dir1/* mnt/destination/dir1/ &
cp -R /mnt/source/dir1/dir1a /mnt/destination/dir1/ &
cp -R /mnt/source/dir1/dir1b /mnt/destination/dir1/ &
cp -R /mnt/source/dir1/dir1c /mnt/destination/dir1/ & # this command copies dir1c1 via recursion
cp -R /mnt/source/dir1/dir1d /mnt/destination/dir1/ &
```

### <a name="when-to-add-mount-points"></a>Montaj noktaları ne zaman eklenir?

Tek bir hedef filesystem montaj noktasına doğru giden yeterli paralel iş parçacığı na sahip olduktan sonra, daha fazla iş parçacığı eklemenin daha fazla iş parçacığı vermediği bir nokta olacaktır. (İşlem, veri türünüze bağlı olarak dosya/saniye veya bayt/saniye olarak ölçülecektir.) Veya daha kötüsü, aşırı iş parçacığı bazen bir iş çıkarma bozulmasına neden olabilir.

Bu durumda, aynı uzak dosya sistemi montaj yolunu kullanarak diğer vFXT küme IP adreslerine istemci tarafı montaj noktaları ekleyebilirsiniz:

```bash
10.1.0.100:/nfs on /mnt/sourcetype nfs (rw,vers=3,proto=tcp,addr=10.1.0.100)
10.1.1.101:/nfs on /mnt/destination1type nfs (rw,vers=3,proto=tcp,addr=10.1.1.101)
10.1.1.102:/nfs on /mnt/destination2type nfs (rw,vers=3,proto=tcp,addr=10.1.1.102)
10.1.1.103:/nfs on /mnt/destination3type nfs (rw,vers=3,proto=tcp,addr=10.1.1.103)
```

İstemci tarafı montaj noktaları eklemek, ek `/mnt/destination[1-3]` montaj noktalarına ek kopyalama komutları atarak daha fazla paralellik elde etmenizi sağlar.

Örneğin, dosyalarınız çok büyükse, farklı hedef yolları kullanmak için kopyalama komutlarını tanımlayabilir ve kopyayı gerçekleştiren istemciden paralel olarak daha fazla komut gönderebilirsiniz.

```bash
cp /mnt/source/file0* /mnt/destination1/ & \
cp /mnt/source/file1* /mnt/destination2/ & \
cp /mnt/source/file2* /mnt/destination3/ & \
cp /mnt/source/file3* /mnt/destination1/ & \
cp /mnt/source/file4* /mnt/destination2/ & \
cp /mnt/source/file5* /mnt/destination3/ & \
cp /mnt/source/file6* /mnt/destination1/ & \
cp /mnt/source/file7* /mnt/destination2/ & \
cp /mnt/source/file8* /mnt/destination3/ & \
```

Yukarıdaki örnekte, üç hedef montaj noktası da istemci dosya kopyalama işlemleri tarafından hedeflenmektedir.

### <a name="when-to-add-clients"></a>İstemci ne zaman eklenir?

Son olarak, istemcinin yeteneklerine ulaştığınızda, daha fazla kopya iş parçacığı veya ek montaj noktaları eklemek ek dosya/sn veya bayt/sn artışı oluşturmaz. Bu durumda, kendi dosya kopyalama işlemleri kümelerini çalıştıracak aynı montaj noktaları kümesine sahip başka bir istemci dağıtabilirsiniz.

Örnek:

```bash
Client1: cp -R /mnt/source/dir1/dir1a /mnt/destination/dir1/ &
Client1: cp -R /mnt/source/dir2/dir2a /mnt/destination/dir2/ &
Client1: cp -R /mnt/source/dir3/dir3a /mnt/destination/dir3/ &

Client2: cp -R /mnt/source/dir1/dir1b /mnt/destination/dir1/ &
Client2: cp -R /mnt/source/dir2/dir2b /mnt/destination/dir2/ &
Client2: cp -R /mnt/source/dir3/dir3b /mnt/destination/dir3/ &

Client3: cp -R /mnt/source/dir1/dir1c /mnt/destination/dir1/ &
Client3: cp -R /mnt/source/dir2/dir2c /mnt/destination/dir2/ &
Client3: cp -R /mnt/source/dir3/dir3c /mnt/destination/dir3/ &

Client4: cp -R /mnt/source/dir1/dir1d /mnt/destination/dir1/ &
Client4: cp -R /mnt/source/dir2/dir2d /mnt/destination/dir2/ &
Client4: cp -R /mnt/source/dir3/dir3d /mnt/destination/dir3/ &
```

### <a name="create-file-manifests"></a>Dosya bildirimleri oluşturma

Yukarıdaki yaklaşımları (hedef başına birden çok kopya iş parçacığı, istemci başına birden çok hedef, ağ tarafından erişilebilen kaynak dosya sistemi başına birden çok istemci) anladıktan sonra şu öneriyi göz önünde bulundurun: Dosya bildirimleri oluşturun ve bunları kopyayla kullanın birden çok istemci arasında komutları.

Bu senaryo, dosyaların ``find`` veya dizinlerin bildirimlerini oluşturmak için UNIX komutunu kullanır:

```bash
user@build:/mnt/source > find . -mindepth 4 -maxdepth 4 -type d
./atj5b55c53be6-01/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-01/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-01/support/trace/rolling
./atj5b55c53be6-03/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-03/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-03/support/trace/rolling
./atj5b55c53be6-02/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-02/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-02/support/trace/rolling
```

Bu sonucu bir dosyaya yönlendirin:`find . -mindepth 4 -maxdepth 4 -type d > /tmp/foo`

Ardından dosyaları saymak ve alt dizinlerin boyutlarını belirlemek için BASH komutlarını kullanarak manifestoyu yineleyebilirsiniz:

```bash
ben@xlcycl1:/sps/internal/atj5b5ab44b7f > for i in $(cat /tmp/foo); do echo " `find ${i} |wc -l` `du -sh ${i}`"; done
244    3.5M    ./atj5b5ab44b7f-02/support/gsi/2018-07-18T00:07:03EDT
9      172K    ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-18T05:01:00UTC
124    5.8M    ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-19T01:01:01UTC
152    15M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T01:01:00UTC
131    13M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T21:59:41UTC_partial
789    6.2M    ./atj5b5ab44b7f-02/support/gsi/2018-07-20T21:59:41UTC
134    12M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T22:22:55UTC_vfxt_catchup
7      16K     ./atj5b5ab44b7f-02/support/pcap/2018-07-18T17:12:19UTC
8      83K     ./atj5b5ab44b7f-02/support/pcap/2018-07-18T17:17:17UTC
575    7.7M    ./atj5b5ab44b7f-02/support/cores/armada_main.2000.1531980253.gsi
33     4.4G    ./atj5b5ab44b7f-02/support/trace/rolling
281    6.6M    ./atj5b5ab44b7f-01/support/gsi/2018-07-18T00:07:03EDT
15     182K    ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-18T05:01:00UTC
244    17M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-19T01:01:01UTC
299    31M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T01:01:00UTC
256    29M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T21:59:41UTC_partial
889    7.7M    ./atj5b5ab44b7f-01/support/gsi/2018-07-20T21:59:41UTC
262    29M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T22:22:55UTC_vfxt_catchup
11     248K    ./atj5b5ab44b7f-01/support/pcap/2018-07-18T17:12:19UTC
11     88K     ./atj5b5ab44b7f-01/support/pcap/2018-07-18T17:17:17UTC
645    11M     ./atj5b5ab44b7f-01/support/cores/armada_main.2019.1531980253.gsi
33     4.0G    ./atj5b5ab44b7f-01/support/trace/rolling
244    2.1M    ./atj5b5ab44b7f-03/support/gsi/2018-07-18T00:07:03EDT
9      158K    ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-18T05:01:00UTC
124    5.3M    ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-19T01:01:01UTC
152    15M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T01:01:00UTC
131    12M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T21:59:41UTC_partial
789    8.4M    ./atj5b5ab44b7f-03/support/gsi/2018-07-20T21:59:41UTC
134    14M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T22:25:58UTC_vfxt_catchup
7      159K    ./atj5b5ab44b7f-03/support/pcap/2018-07-18T17:12:19UTC
7      157K    ./atj5b5ab44b7f-03/support/pcap/2018-07-18T17:17:17UTC
576    12M     ./atj5b5ab44b7f-03/support/cores/armada_main.2013.1531980253.gsi
33     2.8G    ./atj5b5ab44b7f-03/support/trace/rolling
```

Son olarak, istemcilere gerçek dosya kopyalama komutları zanaat gerekir.

Dört istemciniz varsa, şu komutu kullanın:

```bash
for i in 1 2 3 4 ; do sed -n ${i}~4p /tmp/foo > /tmp/client${i}; done
```

Beş istemciniz varsa, şuna benzer bir şey kullanın:

```bash
for i in 1 2 3 4 5; do sed -n ${i}~5p /tmp/foo > /tmp/client${i}; done
```

Ve altı için .... Gerektiği gibi tahmin edin.

```bash
for i in 1 2 3 4 5 6; do sed -n ${i}~6p /tmp/foo > /tmp/client${i}; done
```

Komuttan çıktının bir parçası olarak elde edilen düzey dört dizinlerine yol adlarına sahip *N* istemcilerinizin her biri için bir tane olan N sonuçlanan dosyaları alırsınız. *N* `find`

Kopyalama komutunu oluşturmak için her dosyayı kullanın:

```bash
for i in 1 2 3 4 5 6; do for j in $(cat /tmp/client${i}); do echo "cp -p -R /mnt/source/${j} /mnt/destination/${j}" >> /tmp/client${i}_copy_commands ; done; done
```

Yukarıdaki n *dosyaları,* her satır başına bir kopya komutu ile, istemci üzerinde bir BASH komut dosyası olarak çalıştırılabilir verecektir.

Amaç, birden çok istemcide paralel olarak istemci başına aynı anda bu komut birden çok komut iş parçacığı çalıştırmaktır.

## <a name="use-a-two-phase-rsync-process"></a>İki fazlı rsync işlemi kullanma

Standart ``rsync`` yardımcı program, veri bütünlüğünü garanti etmek için çok sayıda dosya oluşturma ve yeniden adlandırma işlemleri oluşturduğundan, Azure için Avere vFXT üzerinden bulut depolamayı doldurmak için iyi çalışmaz. Ancak, dosya bütünlüğünü denetleyen ikinci bir çalıştırmayla bunu izlerseniz, daha dikkatli kopyalama yordamını atlamak için ``--inplace`` seçeneği ``rsync`` güvenle kullanabilirsiniz.

Standart ``rsync`` bir kopyalama işlemi geçici bir dosya oluşturur ve verilerle doldurur. Veri aktarımı başarıyla tamamlanırsa, geçici dosya özgün dosya adı ile yeniden adlandırılır. Bu yöntem, kopya sırasında dosyalara erişilmiş olsa bile tutarlılığı garanti eder. Ancak bu yöntem, önbellek te dosya hareketini yavaşlatan daha fazla yazma işlemi oluşturur.

Seçenek, ``--inplace`` yeni dosyayı doğrudan son konumuna yazar. Dosyaların aktarım sırasında tutarlı olacağı garanti edilmez, ancak daha sonra kullanılmak üzere bir depolama sistemi kullanıyorsanız bu önemli değildir.

İkinci ``rsync`` işlem, ilk işlemde tutarlılık denetimi görevi görededir. Dosyalar zaten kopyalandığından, ikinci aşama, hedefteki dosyaların kaynaktaki dosyalarla eşleştirdiğinden emin olmak için hızlı bir tarayındır. Herhangi bir dosya eşleşmiyorsa, bunlar kopyalanır.

Her iki aşamayı tek bir komutla birlikte düzenleyebilirsiniz:

```bash
rsync -azh --inplace <source> <destination> && rsync -azh <source> <destination>
```

Bu yöntem, iç dizin yöneticisinin işleyebilir dosya sayısına kadar veri kümeleri için basit ve zaman etkili bir yöntemdir. (Bu genellikle 3 düğümlü küme için 200 milyon dosya, altı düğümlü küme için 500 milyon dosya vb.)

## <a name="use-the-msrsync-utility"></a>msrsync yardımcı programını kullanma

Araç ``msrsync`` ayrıca, verileri Avere kümesi için bir arka uç çekirdek filer'ına taşımak için de kullanılabilir. Bu araç, birden çok paralel ``rsync`` işlem çalıştırarak bant genişliği kullanımını en iyi duruma getirmek için tasarlanmıştır. GitHub'dan <https://github.com/jbd/msrsync>edinilebilir.

``msrsync``kaynak dizini ayrı "kovalara" ayırır ve ``rsync`` her kovada tek tek işlemleri çalıştırır.

Dört çekirdekli VM kullanılarak yapılan ön testler, 64 proses kullanırken en iyi verimi gösterdi. İşlem ``msrsync`` sayısını ``-p`` 64 olarak ayarlamak için seçeneği kullanın.

Bağımsız değişkeni ``--inplace`` ``msrsync`` komutlarla da kullanabilirsiniz. Bu seçeneği kullanıyorsanız, veri bütünlüğünü sağlamak için ikinci bir komut çalıştırmayı (yukarıda açıklanan [rsync'de](#use-a-two-phase-rsync-process)olduğu gibi) çalıştırmayı düşünün.

``msrsync``yalnızca yerel birimlere ve yerel birimlerden yazabilirsiniz. Kümenin sanal ağında yerel bağlar olarak kaynak ve hedef erişilebilir olmalıdır.

Bir ``msrsync`` Azure bulut hacmini Bir Avere kümesiyle doldurmak için aşağıdaki yönergeleri izleyin:

1. Yükleme ``msrsync`` ve önkoşulları (rsync ve Python 2.6 veya sonrası)
1. Kopyalanacak toplam dosya ve dizin sayısını belirleyin.

   Örneğin, Avere yardımcı ``prime.py`` programını ```prime.py --directory /path/to/some/directory``` bağımsız değişkenlerle kullanın <https://github.com/Azure/Avere/blob/master/src/clientapps/dataingestor/prime.py>(url indirerek kullanılabilir).

   Kullanmıyorsanız, ``prime.py``GNU ``find`` aracıyla madde sayısını aşağıdaki gibi hesaplayabilirsiniz:

   ```bash
   find <path> -type f |wc -l         # (counts files)
   find <path> -type d |wc -l         # (counts directories)
   find <path> |wc -l                 # (counts both)
   ```

1. İşlem başına madde sayısını belirlemek için madde sayısını 64'e bölün. Komutu ``-f`` çalıştırdığınızda kovaların boyutunu ayarlamak için seçeneği ile bu numarayı kullanın.

1. Dosyaları ``msrsync`` kopyalamak için komutu verme:

   ```bash
   msrsync -P --stats -p 64 -f <ITEMS_DIV_64> --rsync "-ahv" <SOURCE_PATH> <DESTINATION_PATH>
   ```

   Kullanıyorsanız, ``--inplace``verilerin doğru kopyalandığından denetleme seçeneği olmadan ikinci bir yürütme ekleyin:

   ```bash
   msrsync -P --stats -p 64 -f <ITEMS_DIV_64> --rsync "-ahv --inplace" <SOURCE_PATH> <DESTINATION_PATH> && msrsync -P --stats -p 64 -f <ITEMS_DIV_64> --rsync "-ahv" <SOURCE_PATH> <DESTINATION_PATH>
   ```

   Örneğin, bu komut 64 işlemdeki 11.000 dosyayı /test/kaynak deposundan /mnt/vfxt/depoya taşımak üzere tasarlanmıştır:

   ``msrsync -P --stats -p 64 -f 170 --rsync "-ahv --inplace" /test/source-repository/ /mnt/vfxt/repository && msrsync -P --stats -p 64 -f 170 --rsync "-ahv --inplace" /test/source-repository/ /mnt/vfxt/repository``

## <a name="use-the-parallel-copy-script"></a>Paralel kopya komut dosyasını kullanma

Komut ``parallelcp`` dosyası, vFXT kümenizin arka uç depolamasına veri taşımak için de yararlı olabilir.

Aşağıdaki komut dosyası yürütülebilir `parallelcp`ekler. (Bu komut dosyası Ubuntu için tasarlanmıştır; başka ``parallel`` bir dağıtım kullanıyorsanız, ayrı olarak yüklemeniz gerekir.)

```bash
sudo touch /usr/bin/parallelcp && sudo chmod 755 /usr/bin/parallelcp && sudo sh -c "/bin/cat >/usr/bin/parallelcp" <<EOM
#!/bin/bash

display_usage() {
    echo -e "\nUsage: \$0 SOURCE_DIR DEST_DIR\n"
}

if [  \$# -le 1 ] ; then
    display_usage
    exit 1
fi

if [[ ( \$# == "--help") ||  \$# == "-h" ]] ; then
    display_usage
    exit 0
fi

SOURCE_DIR="\$1"
DEST_DIR="\$2"

if [ ! -d "\$SOURCE_DIR" ] ; then
    echo "Source directory \$SOURCE_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -d "\$DEST_DIR" ] && ! mkdir -p \$DEST_DIR ; then
    echo "Destination directory \$DEST_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -w "\$DEST_DIR" ] ; then
    echo "Destination directory \$DEST_DIR is not writeable, or is not a directory"
    display_usage
    exit 3
fi

if ! which parallel > /dev/null ; then
    sudo apt-get update && sudo apt install -y parallel
fi

DIRJOBS=225
JOBS=225
find \$SOURCE_DIR -mindepth 1 -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$DIRJOBS -0 "mkdir -p \$DEST_DIR/{}"
find \$SOURCE_DIR -mindepth 1 ! -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$JOBS -0 "cp -P \$SOURCE_DIR/{} \$DEST_DIR/{}"
EOM
```

### <a name="parallel-copy-example"></a>Paralel kopya örneği

Bu örnek, Avere kümesinden kaynak dosyaları kullanarak derlemek ``glibc`` için paralel kopya komut dosyasını kullanır.
<!-- xxx what is stored where? what is 'the avere cluster mount point'? xxx -->

Kaynak dosyalar Avere küme montaj noktasında, nesne dosyaları ise yerel sabit diskte depolanır.

Bu komut dosyası yukarıda paralel kopyalama komut dosyası kullanır. ``-j`` Seçenek ile ``parallelcp`` ve ``make`` paralelleştirme kazanmak için kullanılır.

```bash
sudo apt-get update
sudo apt install -y gcc bison gcc binutils make parallel
cd
wget https://mirrors.kernel.org/gnu/libc/glibc-2.27.tar.bz2
tar jxf glibc-2.27.tar.bz2
ln -s /nfs/node1 avere
time parallelcp glibc-2.27 avere/glibc-2.27
cd
mkdir obj
mkdir usr
cd obj
/home/azureuser/avere/glibc-2.27/configure --prefix=/home/azureuser/usr
time make -j
```
