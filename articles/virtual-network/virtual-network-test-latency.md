---
title: Azure sanal ağında Azure sanal ağ gecikmesi testi yapın | Microsoft Dokümanlar
description: Sanal ağdaki Azure sanal makineleri arasındaki ağ gecikmesini nasıl test edebilirsiniz öğrenin
services: virtual-network
documentationcenter: na
author: steveesp
manager: Marina Lipshteyn
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/29/2019
ms.author: steveesp
ms.openlocfilehash: 00efc2754948d53d4f80a6261dbd4041b358185b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74896369"
---
# <a name="test-vm-network-latency"></a>VM ağ gecikmesini sınama

En doğru sonuçları elde etmek için, görev için tasarlanmış bir araçla Azure sanal makine (VM) ağı gecikmesini ölçün. SockPerf (Linux için) ve latte.exe (Windows için) gibi herkese açık araçlar, uygulama gecikmesi gibi diğer gecikme türlerini hariç tartarken ağ gecikmesini yalıtabilir ve ölçebilir. Bu araçlar, uygulama performansını etkileyen ağ trafiği türüne (yani, İletim Denetim Protokolü [TCP] ve Kullanıcı Datagram Protokolü [UDP] trafiğine odaklanır. 

Ping gibi diğer yaygın bağlantı araçları gecikmeyi ölçebilir, ancak sonuçları gerçek iş yüklerinde kullanılan ağ trafiğini temsil etmeyebilir. Bunun nedeni, bu araçların çoğunun uygulama trafiğinden farklı şekilde değerlendirilebilen ve sonuçları TCP ve UDP kullanan iş yükleri için geçerli olmayan Internet Denetim İleti Protokolü'nü (ICMP) kullanmasıdır. 

Çoğu uygulama tarafından kullanılan protokollerin doğru ağ gecikme sınama sınaması için, SockPerf (Linux için) ve latte.exe (Windows için) en alakalı sonuçları üretir. Bu makalede, bu araçların her ikisini de kapsar.

## <a name="overview"></a>Genel Bakış

Biri gönderen, diğeri alıcı olarak olmak üzere iki VM kullanarak, iki yönlü bir iletişim kanalı oluşturursunuz. Bu yaklaşımla, paketleri her iki yönde de gönderip alabilir ve gidiş-dönüş süresini (RTT) ölçebilirsiniz.

Bu yaklaşımı, iki VM arasında ve hatta iki fiziksel bilgisayar arasında ağ gecikmesini ölçmek için kullanabilirsiniz. Gecikme sonu ölçümleri aşağıdaki senaryolar için yararlı olabilir:

- Dağıtılan VM'ler arasında ağ gecikmesi için bir ölçüt belirleyin.
- Aşağıdakilerde ilgili değişiklikler yapıldıktan sonra ağ gecikmesindeki değişikliklerin etkilerini karşılaştırın:
  - Yapılandırma değişiklikleri de dahil olmak üzere işletim sistemi (OS) veya ağ yığını yazılımı.
  - Kullanılabilirlik bölgesine veya yakınlık yerleşim grubuna (PPG) dağıtım gibi bir VM dağıtım yöntemi.
  - Hızlandırılmış Ağ veya boyut değişiklikleri gibi VM özellikleri.
  - Değişiklikleri yönlendirme veya filtreleme gibi sanal ağ.

### <a name="tools-for-testing"></a>Test araçları
Gecikme yi ölçmek için iki farklı araç seçeneğiniz vardır:

* Windows tabanlı sistemler için: [latte.exe (Windows)](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b)
* Linux tabanlı sistemler için: [SockPerf (Linux)](https://github.com/mellanox/sockperf)

Bu araçları kullanarak, yalnızca TCP veya UDP taşıma teslim sürelerinin ÖLÇÜlmesini ve ICMP (Ping) veya uygulamalar tarafından kullanılmayan ve performanslarını etkilemeyen diğer paket türlerinin ölçülmesini sağlamaya yardımcı olursunuz.

### <a name="tips-for-creating-an-optimal-vm-configuration"></a>En iyi VM yapılandırması oluşturmak için ipuçları

VM yapılandırmanızı oluştururken aşağıdaki önerileri aklınızda bulundurun:
- Windows veya Linux'un en son sürümünü kullanın.
- En iyi sonuçlar için Hızlandırılmış Ağ'ı etkinleştirin.
- [Azure yakınlık yerleşim grubu](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)içeren VM'leri dağıtın.
- Büyük VM'ler genellikle daha küçük VM'lerden daha iyi performans gösterir.

### <a name="tips-for-analysis"></a>Analiz için ipuçları

Test sonuçlarını analiz ederken, aşağıdaki önerileri göz önünde bulundurun:

- Dağıtım, yapılandırma ve optimizasyonlar tamamlanır tamamlanmaz bir taban çizgisi erken kurun.
- Her zaman yeni sonuçları bir taban çizgisiyle veya başka bir şekilde, bir testten diğerine, kontrollü değişikliklerle karşılaştırın.
- Değişiklikler gözlemlendiğinde veya planlandığında testleri yineleyin.


## <a name="test-vms-that-are-running-windows"></a>Windows çalıştıran test VM'leri

### <a name="get-latteexe-onto-the-vms"></a>Latte.exe'yi VM'lere geçirin

[Latte.exe en son sürümünü indirin.](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b)

latte.exe'yi *c:\tools*gibi ayrı bir klasöre koymayı düşünün.

### <a name="allow-latteexe-through-windows-defender-firewall"></a>Windows Defender Güvenlik Duvarı üzerinden latte.exe izin ver

*Alıcıda,* latte.exe trafiğinin gelmesine izin vermek için Windows Defender Güvenlik Duvarı'nda İzin Ver kuralı oluşturun. Belirli TCP bağlantı noktalarının gelene izin vermek yerine tüm latte.exe programına adıyla izin vermek en kolayıdır.

Aşağıdaki komutu çalıştırarak Windows Defender Güvenlik Duvarı üzerinden latte.exe izin verin:

```cmd
netsh advfirewall firewall add rule program=<path>\latte.exe name="Latte" protocol=any dir=in action=allow enable=yes profile=ANY
```

Örneğin, latte.exe'yi *c:\tools* klasörüne kopyaladıysanız, bu komut şu olur:

`netsh advfirewall firewall add rule program=c:\tools\latte.exe name="Latte" protocol=any dir=in action=allow enable=yes profile=ANY`

### <a name="run-latency-tests"></a>Gecikme testleri çalıştırın

* *Alıcıda,* latte.exe başlatın (PowerShell'den değil CMD penceresinden çalıştırın):

    ```cmd
    latte -a <Receiver IP address>:<port> -i <iterations>
    ```

    Yaklaşık 65.000 yineleme temsil sonuçları döndürmek için yeterince uzun.

    Kullanılabilir bağlantı noktası numarası gayet iyi.

    VM'nin IP adresi 10.0.0.4 ise, komut şuna benzer:

    `latte -a 10.0.0.4:5005 -i 65100`

* *Gönderende,* latte.exe başlatın (PowerShell'den değil CMD penceresinden çalıştırın):

    ```cmd
    latte -c -a <Receiver IP address>:<port> -i <iterations>
    ```

    Elde edilen komut alıcıüzerinde aynıdır, bunun *istemci*veya&nbsp; *gönderen*olduğunu belirtmek için *-c* eklenmesi dışında:

    `latte -c -a 10.0.0.4:5005 -i 65100`

Sonuçları bekleyin. VM'lerin ne kadar uzakta olduğuna bağlı olarak, testin tamamlanması birkaç dakika sürebilir. Daha uzun testler çalıştırmadan önce başarı için test etmek için daha az yineleme ile başlamayı düşünün.

## <a name="test-vms-that-are-running-linux"></a>Linux çalıştıran test VM'leri

Linux çalıştıran VM'leri test etmek için [SockPerf](https://github.com/mellanox/sockperf)kullanın.

### <a name="install-sockperf-on-the-vms"></a>VM'lere SockPerf'i yükleyin

Linux VM'lerde, hem *gönderen* hem de *alıcı,* VM'lerde SockPerf'i hazırlamak için aşağıdaki komutları çalıştırın. Büyük dağıtımlar için komutlar verilir.

#### <a name="for-red-hat-enterprise-linux-rhelcentos"></a>Red Hat Enterprise Linux (RHEL)/CentOS için

Aşağıdaki komutları çalıştırın:

```bash
#RHEL/CentOS - Install Git and other helpful tools
    sudo yum install gcc -y -q
    sudo yum install git -y -q
    sudo yum install gcc-c++ -y
    sudo yum install ncurses-devel -y
    sudo yum install -y automake
    sudo yum install -y autoconf
```

#### <a name="for-ubuntu"></a>Ubuntu için

Aşağıdaki komutları çalıştırın:

```bash
#Ubuntu - Install Git and other helpful tools
    sudo apt-get install build-essential -y
    sudo apt-get install git -y -q
    sudo apt-get install -y autotools-dev
    sudo apt-get install -y automake
    sudo apt-get install -y autoconf
```

#### <a name="for-all-distros"></a>Tüm dağıtımlar için

SockPerf'i aşağıdaki adımlara göre kopyalayın, derlayın ve kurun:

```bash
#Bash - all distros

#From bash command line (assumes Git is installed)
git clone https://github.com/mellanox/sockperf
cd sockperf/
./autogen.sh
./configure --prefix=

#make is slower, may take several minutes
make

#make install is fast
sudo make install
```

### <a name="run-sockperf-on-the-vms"></a>VMs üzerinde Çalıştır SockPerf

SockPerf yüklemesi tamamlandıktan sonra, VM'ler gecikme testlerini çalıştırmaya hazırdır. 

İlk olarak, *alıcı*üzerinde SockPerf başlatın.

Kullanılabilir bağlantı noktası numarası gayet iyi. Bu örnekte, bağlantı noktası 12345'i kullanıyoruz:

```bash
#Server/Receiver - assumes server's IP is 10.0.0.4:
sudo sockperf sr --tcp -i 10.0.0.4 -p 12345
```

Sunucu dinlediği için istemci, dinlediği bağlantı noktasındaki sunucuya paket göndermeye başlayabilir (bu durumda, 12345).

Yaklaşık 100 saniye, aşağıdaki örnekte gösterildiği gibi temsilci sonuçlarını döndürmek için yeterlidir:

```bash
#Client/Sender - assumes server's IP is 10.0.0.4:
sockperf ping-pong -i 10.0.0.4 --tcp -m 350 -t 101 -p 12345  --full-rtt
```

Sonuçları bekleyin. VM'lerin ne kadar birbirinden uzak olduğuna bağlı olarak, yineleme sayısı değişir. Daha uzun testler çalıştırmadan önce başarı yı test etmek için, yaklaşık 5 saniyelik daha kısa testlerle başlamayı düşünün.

Bu SockPerf örneği, ortalama bir paket için tipik olan 350 baytlık ileti boyutu kullanır. VM'lerinizde çalışan iş yükünü daha doğru bir şekilde temsil eden sonuçlar elde etmek için boyutu daha yüksek veya daha düşük olarak ayarlayabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
* Azure yakınlık yerleşim [grubuyla](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)gecikme yi artırın.
* Senaryonuz [için VM'ler için ağ landırmayı](../virtual-network/virtual-network-optimize-network-bandwidth.md) nasıl optimize edebilirsiniz öğrenin.
* Bant [genişliğinin sanal makinelere nasıl tahsis edildiğini](../virtual-network/virtual-machine-network-throughput.md)okuyun.
* Daha fazla bilgi için Azure [Sanal Ağ SSS'si'ne](../virtual-network/virtual-networks-faq.md)bakın.
