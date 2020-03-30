---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/28/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 6740ea320f2d950386da12eb44726e2c826b60a4
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80386105"
---
## <a name="premium-ssd"></a>Premium SSD

Azure premium SSD'ler, giriş/çıkış (IO) yoğun iş yükleriyle sanal makineler (VM'ler) için yüksek performanslı ve düşük gecikmeli disk desteği sağlar. Birinci sınıf depolama disklerinin hızından ve performansından yararlanmak için, varolan VM diskleri Premium SSD'lere geçirebilirsiniz. Premium SSD'ler kritik üretim uygulamaları için uygundur. Premium SSD'ler yalnızca premium depolama alanına uyumlu VM serisi ile kullanılabilir.

Hangi boyutların premium depolama alanına uyumlu olduğu da dahil olmak üzere Windows için Azure'daki tek tek VM türleri ve boyutları hakkında daha fazla bilgi edinmek için [Windows VM boyutlarına](../articles/virtual-machines/windows/sizes.md)bakın. Hangi boyutların premium depolama yla uyumlu olduğu da dahil olmak üzere Linux için Azure'daki bireysel VM türleri ve boyutları hakkında daha fazla bilgi edinmek için [Linux VM boyutlarına](../articles/virtual-machines/linux/sizes.md)bakın. Bu makalelerden herhangi birinden, premium depolama uyumlu olup olmadığını belirlemek için her bir VM boyutu makalesini kontrol etmiş olmanız gerekir.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Standart depolamadan farklı olarak, birinci sınıf bir depolama diski sağlarken, bu diskin kapasitesini, IOPS'u ve üretim ini garanti esiniz. Örneğin, bir P50 disk oluşturursanız, Azure bu disk için 4.095 GB depolama kapasitesi, 7.500 IOPS ve 250 MB/s'lik iş ortası sağlar. Uygulamanız kapasite ve performansın tamamını veya bir kısmını kullanabilir. Premium SSD diskler, bir önceki tabloda açıklanan düşük tek basamaklı milisaniye gecikmeleri ve hedef IOPS ve %99,9 oranında açıklanan iş ortası sağlamak üzere tasarlanmıştır.

## <a name="bursting"></a>Patlama

P30'dan daha küçük premium SSD boyutları artık disk patlaması sunuyor ve disk başına IOPS'lerini 3.500'e ve bant genişliklerini 170 Mbps'ye kadar patlatabiliyor. Bursting otomatiktir ve bir kredi sistemine dayalı çalışır. Disk trafiği sağlanan performans hedefinin altında olduğunda, krediler otomatik olarak bir seri kovasında birikir ve trafik hedefin ötesinde, maksimum patlama sınırına kadar patladığında krediler otomatik olarak tüketilir. Maksimum seri patlama sınırı, tüketecek patlama kredilerine sahip olsanız bile disk IOPS & Bant genişliğinin tavanını tanımlar. Disk patlaması, IO desenlerinin öngörülemeyen değişikliklerine daha iyi tolerans sağlar. En iyi os disk önyükleme ve dikenli trafik ile uygulamalar için kaldıraç olabilir.    

Diskler patlama desteği varsayılan olarak geçerli disk boyutlarındaki yeni dağıtımlarda etkinleştirilir ve kullanıcı eylemi gerekmez. Geçerli boyutlardaki varolan diskler için, iki seçenekten biriyle patlamayı etkinleştirebilirsiniz: diski ayırın ve yeniden takın veya bağlı VM'yi durdurup yeniden başlatın. Tüm patlama uygulanabilir disk boyutları, disk 30 dakika lık maksimum patlama limitinde maksimum süreyi destekleyen sanal makineye bağlandığında tam bir seri çekim kredi kovasıyla başlar. Azure Diskleri'nde patlama nın nasıl çalıştığı hakkında daha fazla bilgi edinmek için [Premium SSD patlamasına](../articles/virtual-machines/linux/disk-bursting.md)bakın. 

### <a name="transactions"></a>İşlemler

Premium SSD'ler için, 256 KiB'den daha az veya 256 KiB'den daha az veya eşit olan her G/Ç işlemi tek bir G/Ç işlemi olarak kabul edilir. 256 KiB'den büyük i/ç işlemleri, 256 KiB boyutunda birden fazla G/Ç olarak kabul edilir.

## <a name="standard-ssd"></a>Standart SSD

Azure standart SSD'leri, daha düşük IOPS düzeylerinde tutarlı performans gerektiren iş yükleri için optimize edilmiş uygun maliyetli bir depolama seçeneğidir. Standart SSD, özellikle şirket içinde HDD çözümlerinizde çalışan iş yüklerinin değişkenliğiyle ilgili sorunlarla karşılaşırsanız, buluta geçmek isteyenler için iyi bir giriş düzeyi deneyimi sunar. Standart HDD'lerle karşılaştırıldığında, standart SSD'ler daha iyi kullanılabilirlik, tutarlılık, güvenilirlik ve gecikme süresi sunar. Standart SSD'ler Web sunucuları, düşük IOPS uygulama sunucuları, hafif çehreli kurumsal uygulamalar ve Geliştirme/Test iş yükleri için uygundur. Standart HDD'ler gibi, standart SSD'ler de tüm Azure VM'lerinde kullanılabilir.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

Standart SSD'ler, tek basamaklı milisaniye gecikmeleri ve IOPS'ler sağlamak ve bir önceki tabloda açıklanan limitlere kadar %99'luk bir süre elde etmek üzere tasarlanmıştır. Gerçek IOPS ve iş artışı bazen trafik modellerine bağlı olarak değişebilir. Standart SSD'ler, daha düşük gecikme sürelerine sahip HDD disklere göre daha tutarlı performans sağlar.

### <a name="transactions"></a>İşlemler

Standart SSD'ler için, 256 KiB'den daha az veya 256 KiB'den daha az veya eşit olan her G/Ç işlemi tek bir G/Ç işlemi olarak kabul edilir. 256 KiB'den büyük i/ç işlemleri, 256 KiB boyutunda birden fazla G/Ç olarak kabul edilir. Bu hareketlerin faturalandırma etkisi vardır.

## <a name="standard-hdd"></a>Standart HDD

Azure standart HDD'leri, gecikmeye duyarsız iş yükleri çalıştıran VM'ler için güvenilir, düşük maliyetli disk desteği sunar. Standart depolama ile veriler sabit disk sürücülerinde (HDD' ler) depolanır. Gecikme, IOPS ve Standart HDD disklerin İş Bilgililiği, SSD tabanlı disklere göre daha büyük farklılıklar gösterebilir. Standart HDD Diskler 10ms altında yazma gecikmeleri sunmak ve çoğu IO işlemleri için 20ms altında gecikmeler okumak için tasarlanmıştır, ancak gerçek performans IO boyutu ve iş yükü desenine bağlı olarak değişebilir. VM'lerle çalışırken, geliştirme/test senaryoları ve daha az kritik iş yükleri için standart HDD diskleri kullanabilirsiniz. Standart HDD'ler tüm Azure bölgelerinde kullanılabilir ve tüm Azure VM'lerinde kullanılabilir.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

### <a name="transactions"></a>İşlemler

Standart HDD'ler için, Her IO işlemi, G/Ç boyutundan bağımsız olarak tek bir işlem olarak kabul edilir. Bu hareketlerin faturalandırma etkisi vardır.

## <a name="billing"></a>Faturalandırma

Yönetilen diskler kullanırken aşağıdaki faturalandırma hususları geçerlidir:

- Disk türü
- yönetilen disk Boyutu
- Anlık Görüntüler
- Giden veri aktarımları
- Hareket sayısı

**Yönetilen disk boyutu**: yönetilen diskler verilen boyutta faturalandırılır. Azure, sağlanan boyutu (yuvarlanmış) en yakın sunulan disk boyutuyla eşler. Sunulan disk boyutlarının ayrıntıları için önceki tablolara bakın. Her disk, desteklenen bir disk boyutu teklifiyle eşlenir ve buna göre faturalandırılır. Örneğin, 200 GiB Standart SSD verdiyseniz, E15'in (256 GiB) disk boyutu teklifini eşler. Sağlanan herhangi bir diskiçin faturalandırma, Premium Depolama teklifinin aylık fiyatı kullanılarak saatlik olarak eşit olarak eşit olarak sınıflandırılır. Örneğin, bir E10 diski temin ettiyseniz ve 20 saat sonra sildiyseniz, 20 saate eşit olarak eşit olarak verilen E10 teklifi için faturalandırılırsınız. Bu, diske yazılan gerçek veri miktarından bağımsızdır.

**Anlık görüntüler**: Anlık görüntüler, kullanılan boyuta göre faturalandırılır. Örneğin, 64 GiB ve 10 GiB fiili kullanılan veri boyutu ile yönetilen bir disk anlık görüntü oluşturursanız, anlık görüntü yalnızca kullanılan 10 GiB veri boyutu için faturalandırılır.
