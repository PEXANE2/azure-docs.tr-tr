---
title: Önerilen performans kıyaslama testleri - Azure NetApp Files
description: Azure NetApp Dosyalarını kullanarak toplu performans ve ölçümler için kıyaslama testi önerileri hakkında bilgi edinin.
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: conceptual
ms.date: 08/07/2019
ms.openlocfilehash: 8f354152c23dd7ad0413f27585d724f8070ca003
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551531"
---
# <a name="performance-benchmark-test-recommendations-for-azure-netapp-files"></a>Azure NetApp Files için performans kıyaslamaları test önerileri

Bu makalede, Azure NetApp Dosyalarını kullanarak birim performansı ve ölçümler için kıyaslama testi önerileri verilmektedir.

## <a name="overview"></a>Genel Bakış

Azure NetApp Dosyaları biriminin performans özelliklerini anlamak için, çeşitli iş yüklerini simüle etmek için bir dizi kıyaslama çalıştırmak için açık kaynak takiolan [FIO'yu](https://github.com/axboe/fio) kullanabilirsiniz. FIO hem Linux hem de Windows tabanlı işletim sistemlerine yüklenebilir.  Hem IOPS hem de bir hacim için iş hacminin hızlı bir anlık görüntüsünü elde etmek için mükemmel bir araçtır.

### <a name="vm-instance-sizing"></a>VM örnek boyutlandırma

En iyi sonuçlar için, testleri gerçekleştirmek için uygun şekilde boyutlandırılmış bir sanal makine (VM) örneği kullandığınızdan emin olun. Aşağıdaki örneklerde Standard_D32s_v3 bir örnek kullanılır. VM örnek boyutları hakkında daha fazla bilgi için, Windows tabanlı VM'ler [için Azure'daki Windows sanal makinelerine](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) yönelik Boyutlar ve Linux tabanlı Sanal MM'ler [için Azure'daki Linux sanal makineleri için Boyutlar'a](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bakın.

### <a name="azure-netapp-files-volume-sizing"></a>Azure NetApp Dosyaları toplu boyutlandırma

Beklenen performans düzeyi için doğru hizmet düzeyini ve birim kota boyutunu seçtiğinizden emin olun. Daha fazla bilgi [için Azure NetApp Dosyaları için Hizmet düzeylerine](azure-netapp-files-service-levels.md) bakın.

### <a name="virtual-network-vnet-recommendations"></a>Sanal ağ (VNet) önerileri

Kıyaslama testini Azure NetApp Dosyaları ile aynı VNet'te gerçekleştirmelisiniz. Aşağıdaki örnekte öneri gösteriş:

![VNet önerileri](../media/azure-netapp-files/azure-netapp-files-benchmark-testing-vnet.png)

## <a name="installation-of-fio"></a>FIO kurulumu

FIO hem Linux hem de Windows için ikili formatta kullanılabilir. Seçtiğiniz platform için yüklemek için [FIO'daki](https://github.com/axboe/fio) İkili Paketler bölümünü takip edin.

## <a name="fio-examples-for-iops"></a>IOPS için FIO örnekleri 

Bu bölümdeki FIO örnekleri aşağıdaki kurulumu kullanır:
* VM örnek boyutu: D32s_v3
* Kapasite havuzu hizmet seviyesi ve boyutu: Premium / 50 TiB
* Birim kota boyutu: 48 TiB

Aşağıdaki örnekler FIO rasgele okur ve yazıyor gösterir.

### <a name="fio-8k-block-size-100-random-reads"></a>FIO: 8k blok boyutu % 100 rasgele okur

`fio --name=8krandomreads --rw=randread --direct=1 --ioengine=libaio --bs=8k --numjobs=4 --iodepth=128 --size=4G --runtime=600 --group_reporting`

### <a name="output-68k-read-iops-displayed"></a>Çıktı: 68k okuma IOPS görüntülendi

`Starting 4 processes`  
`Jobs: 4 (f=4): [r(4)][84.4%][r=537MiB/s,w=0KiB/s][r=68.8k,w=0 IOPS][eta 00m:05s]`

### <a name="fio-8k-block-size-100-random-writes"></a>FIO: 8k blok boyutu % 100 rasgele yazıyor

`fio --name=8krandomwrites --rw=randwrite --direct=1 --ioengine=libaio --bs=8k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-73k-write-iops-displayed"></a>Çıktı: 73k iops yazma görüntülenir

`Starting 4 processes`  
`Jobs: 4 (f=4): [w(4)][26.7%][r=0KiB/s,w=571MiB/s][r=0,w=73.0k IOPS][eta 00m:22s]`

## <a name="fio-examples-for-bandwidth"></a>Bant genişliği için FIO örnekleri

Bu bölümdeki örnekler FIO sıralı okuma ve yazma gösterir.

### <a name="fio-64k-block-size-100-sequential-reads"></a>FIO: 64k blok boyutu %100 sıralı okuma

`fio --name=64kseqreads --rw=read --direct=1 --ioengine=libaio --bs=64k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-118-gbits-throughput-displayed"></a>Çıkış: 11,8 Gbit/s verim görüntülendi

`Starting 4 processes`  
`Jobs: 4 (f=4): [R(4)][40.0%][r=1313MiB/s,w=0KiB/s][r=21.0k,w=0 IOPS][eta 00m:09s]`

### <a name="fio-64k-block-size-100-sequential-writes"></a>FIO: 64k blok boyutu %100 sıralı yazıyor

`fio --name=64kseqwrites --rw=write --direct=1 --ioengine=libaio --bs=64k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-122-gbits-throughput-displayed"></a>Çıkış: 12.2 Gbit/s verim görüntülenir

`Starting 4 processes`  
`Jobs: 4 (f=4): [W(4)][85.7%][r=0KiB/s,w=1356MiB/s][r=0,w=21.7k IOPS][eta 00m:02s]`

## <a name="volume-metrics"></a>Birim ölçümleri

Azure NetApp Files performans verilerini Azure Monitor sayaçları aracılığıyla edinilebilir. Sayaçlar Azure portalı ve REST API GET istekleri aracılığıyla kullanılabilir. 

Aşağıdaki bilgiler için geçmiş verileri görüntüleyebilirsiniz:
* Ortalama okuma gecikmesi 
* Ortalama yazma gecikmesi 
* IOPS'yi okuyun (ortalama)
* IOPS yaz (ortalama)
* Birim mantıksal boyutu (ortalama)
* Birim anlık görüntü boyutu (ortalama)

### <a name="using-azure-monitor"></a>Azure İzleyici’yi kullanma 

Azure NetApp Dosyaları sayaçlarına aşağıda gösterildiği gibi Ölçümler sayfasından birim başına olarak erişebilirsiniz:

![Azure Monitör ölçümleri](../media/azure-netapp-files/azure-netapp-files-benchmark-monitor-metrics.png)

Ayrıca Azure NetApp Dosyaları için Azure Monitor'da Ölçümler sayfasına giderek, NetApp için filtre uygulayarak ve birim sayacını belirterek bir pano oluşturabilirsiniz: 

![Azure İzleyici panosu](../media/azure-netapp-files/azure-netapp-files-benchmark-monitor-dashboard.png)

### <a name="azure-monitor-api-access"></a>Azure Monitör API erişimi

REST API çağrılarını kullanarak Azure NetApp Dosyaları sayaçlarına erişebilirsiniz. Azure Monitor ile desteklenen ölçümlere bakın: Kapasite havuzları ve hacimler için sayaçlar için [Microsoft.NetApp/netAppAccounts/capacityPools/Volumes.](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftnetappnetappaccountscapacitypoolsvolumes)

Aşağıdaki örnekte, mantıksal birim boyutunu görüntülemek için GET URL'si gösterilmektedir:

`#get ANF volume usage`  
`curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/ANFACCOUNTGOESHERE/capacityPools/ANFPOOLGOESHERE/Volumes/ANFVOLUMEGOESHERE/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=VolumeLogicalSize`


## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
- [Azure NetApp Files için performans kıyaslamaları](azure-netapp-files-performance-benchmarks.md)