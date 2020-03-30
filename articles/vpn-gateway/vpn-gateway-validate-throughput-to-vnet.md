---
title: VPN masını Microsoft Azure Sanal Ağı'na doğrulama
description: Bu belgenin amacı, bir kullanıcının şirket içi kaynaklarından azure sanal makinesine ağ girdisini doğrulamasına yardımcı olmaktır.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: troubleshooting
ms.date: 05/29/2019
ms.author: radwiv
ms.reviewer: chadmat;genli
ms.openlocfilehash: a88e339e82484c2ec1cd2276f6218fa718b990f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75860495"
---
# <a name="how-to-validate-vpn-throughput-to-a-virtual-network"></a>Sanal ağa yönelik VPN aktarım hızını doğrulama

VPN ağ geçidi bağlantısı, Azure içindeki Sanal Ağınız ile şirket içi BT altyapınız arasında güvenli, tesisler arası bağlantı kurmanıza olanak tanır.

Bu makalede, şirket içi kaynaklardan bir Azure sanal makinesine (VM) ağ iş parçacığının nasıl doğrulanır.

> [!NOTE]
> Bu makalede, tanılamak ve ortak sorunları gidermek için tasarlanmıştır. Aşağıdaki bilgileri kullanarak sorunu çözemiyorsanız, [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)

## <a name="overview"></a>Genel Bakış

VPN ağ geçidi bağlantısı aşağıdaki bileşenleri içerir:

* Şirket içi VPN aygıtı [(Doğrulanmış VPN aygıtlarının](vpn-gateway-about-vpn-devices.md#devicetable)listesini görüntüleyin.)
* Genel internet
* Azure VPN ağ geçidi
* Azure VM

Aşağıdaki diyagram, şirket içi bir ağın VPN aracılığıyla bir Azure sanal ağına mantıksal bağlantısını gösterir.

![VPN kullanarak Müşteri Ağının MSFT Ağına Mantıksal Bağlantısı](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-the-maximum-expected-ingressegress"></a>Beklenen maksimum girişi/çıkış

1. Uygulamanızın temel üretim gereksinimlerini belirleyin.
1. Azure VPN ağ geçidi giriş çıkış limitlerinizi belirleyin. Yardım için VPN [Ağ Geçidi Hakkında'nın](vpn-gateway-about-vpngateways.md#gwsku)"Ağ Geçidi SK'leri" bölümüne bakın.
1. VM boyutunuz için [Azure VM üretim kılavuzunu](../virtual-machines/virtual-machines-windows-sizes.md) belirleyin.
1. Internet Servis Sağlayıcınızı (ISS) bant genişliğini belirleyin.
1. VM, VPN Ağ Geçidi veya ISS'nin en az bant genişliğini alarak beklenen iş parçacığınızı hesaplayın; megabit///) ile ölçülen saniye başına sekize bölünür (8).

Hesaplanan iş kaynağınız uygulamanızın temel iş gereksinimini karşılamazsa, darboğaz olarak tanımladığınız kaynağın bant genişliğini artırmanız gerekir. Azure VPN Ağ Geçidi'ni yeniden boyutlandırmak için [bkz.](vpn-gateway-about-vpn-gateway-settings.md#gwsku) Sanal bir makineyi yeniden boyutlandırmak için [vm yeniden boyutlandırma'ya](../virtual-machines/virtual-machines-windows-resize-vm.md)bakın. Beklenen Internet bant genişliğini yaşamıyorsanız, ISS'nize de başvurabilirsiniz.

> [!NOTE]
> VPN Ağ Geçidi girişi, tüm Siteden Siteye\VNET-to-VNET veya Site'ye bağlantı bağlantılarının toplamıdır.

## <a name="validate-network-throughput-by-using-performance-tools"></a>Performans araçlarını kullanarak ağ işletini doğrulayın

Test sırasında VPN tünel imişdosu doğru sonuçlar vermediğiiçin, bu doğrulama yoğun olmayan saatlerde yapılmalıdır.

Bu test için kullandığımız araç, hem Windows hem de Linux üzerinde çalışan ve hem istemci hem de sunucu modlarına sahip olan iPerf'tir. Windows VM'ler için 3Gbps ile sınırlıdır.

Bu araç diske herhangi bir okuma/yazma işlemi gerçekleştirmez. Yalnızca bir uçtan diğerine kendi kendine oluşturulan TCP trafiği üretir. İstemci ve sunucu düğümleri arasında kullanılabilen bant genişliğini ölçen denemeyi temel alan istatistikler oluşturur. İki düğüm arasında sınama sırasında, bir düğüm sunucu gibi davranır ve diğer düğüm istemci gibi davranır. Bu test tamamlandıktan sonra, her iki düğümde de yükleme ve indirme iş kısmını test etmek için düğümlerin rollerini tersine çevirmenizi öneririz.

### <a name="download-iperf"></a>iPerf İndir

Indirin [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip). Ayrıntılar için [iPerf belgelerine](https://iperf.fr/iperf-doc.php)bakın.

 > [!NOTE]
 > Bu makalede tartışılan üçüncü taraf ürünleri Microsoft'tan bağımsız şirketler tarafından üretilir. Microsoft bu ürünlerin performansı hakkında zımni ya da başka türlü hiçbir garanti vermez.

### <a name="run-iperf-iperf3exe"></a>çalıştırın iPerf (iperf3.exe)

1. Trafiğe izin veren bir NSG/ACL kuralını etkinleştirin (Azure VM'de genel IP adresi testi için).

1. Her iki düğümde de bağlantı noktası 5001 için bir güvenlik duvarı özel durumu etkinleştirin.

   **Windows:** Yönetici olarak aşağıdaki komutu çalıştırın:

   ```CMD
   netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
   ```

   Sınama tamamlandığında kuralı kaldırmak için şu komutu çalıştırın:

   ```CMD
   netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
   ```

   **Azure Linux:** Azure Linux görüntülerinin izin verilen güvenlik duvarları vardır. Bağlantı noktasında dinleme uygulaması varsa, trafiğin geçmesine izin verilir. Güvenli özel görüntülerin açık bir şekilde açılması gerektirebilir. Ortak Linux OS katmanı `iptables`güvenlik `ufw`duvarları `firewalld`, veya .

1. Sunucu düğümünde, iperf3.exe'nin ayıklandığı dizine değiştirin. Ardından iPerf'i sunucu modunda çalıştırın ve aşağıdaki komutlar olarak bağlantı noktası 5001'de dinleyecek şekilde ayarlayın:

   ```CMD
   cd c:\iperf-3.1.2-win65

   iperf3.exe -s -p 5001
   ```

   > [!Note]
   > Bağlantı noktası 5001, ortamınızdaki belirli güvenlik duvarı kısıtlamalarını hesaba katmak için özelleştirilebilir.

1. İstemci düğümünde, iperf aracının çıkarıldığı dizine değiştirin ve ardından aşağıdaki komutu çalıştırın:

   ```CMD
   iperf3.exe -c <IP of the iperf Server> -t 30 -p 5001 -P 32
   ```

   İstemci, 5001 portundaki otuz saniyelik trafiği sunucuya yönlendiriyor. '-P' bayrağı, sunucu düğümüne 32 eşzamanlı bağlantı yaptığımızı gösterir.

   Aşağıdaki ekranda bu örnekten çıktı gösterilmektedir:

   ![Çıktı](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

1. (İsteğE bağlı) Test sonuçlarını korumak için şu komutu çalıştırın:

   ```CMD
   iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
   ```

1. Önceki adımları tamamladıktan sonra, sunucu düğümü artık istemci düğümü olacak ve tersi olacak şekilde, rolleri ters ile aynı adımları yürütmek.

> [!Note]
> Tek araç Iperf değil. [NTTTCP test etmek için alternatif bir çözümdür.](https://docs.microsoft.com/azure/virtual-network/virtual-network-bandwidth-testing)

## <a name="test-vms-running-windows"></a>Windows çalıştıran Test VM'leri

### <a name="load-latteexe-onto-the-vms"></a>Latte.exe'yi VM'lere yükleyin

[Latte.exe'nin](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b) en son sürümünü indirin

Latte.exe'yi ayrı bir klasöre koymayı düşünün, örneğin`c:\tools`

### <a name="allow-latteexe-through-the-windows-firewall"></a>Latte.exe'nin Windows güvenlik duvarından geçmesine izin ver

Alıcıda, Latte.exe trafiğinin gelmesine izin vermek için Windows Güvenlik Duvarı'nda İzin Ver kuralı oluşturun. Belirli TCP bağlantı noktalarının gelene izin vermek yerine tüm Latte.exe programına adıyla izin vermek en kolayıdır.

### <a name="allow-latteexe-through-the-windows-firewall-like-this"></a>Latte.exe'nin Windows Güvenlik Duvarı'ndan böyle geçmesine izin ver

`netsh advfirewall firewall add rule program=<PATH>\latte.exe name="Latte" protocol=any dir=in action=allow enable=yes profile=ANY`

Örneğin, latte.exe'yi "c:\tools" klasörüne kopyaladıysanız, bu komut

`netsh advfirewall firewall add rule program=c:\tools\latte.exe name="Latte" protocol=any dir=in action=allow enable=yes profile=ANY`

### <a name="run-latency-tests"></a>Gecikme testleri çalıştırın

BAŞLAT LATTE.exe RECEIVER (CMD değil, PowerShell çalıştırın):

`latte -a <Receiver IP address>:<port> -i <iterations>`

Yaklaşık 65k yinelemeler temsilsonuçları döndürmek için yeterince uzundur.

Kullanılabilir bağlantı noktası numarası gayet iyi.

VM'nin IP adresi 10.0.0.4 ise, bu

`latte -c -a 10.0.0.4:5005 -i 65100`

LATTE.exe'yi SENDER'da başlatın (PowerShell'den değil CMD'den çalıştırın)

`latte -c -a <Receiver IP address>:<port> -i <iterations>`

Elde edilen komut, "istemci" veya gönderen olduğunu belirtmek için "-c" eklenmesi dışında alıcı üzerinde aynıdır

`latte -c -a 10.0.0.4:5005 -i 65100`

Sonuçları bekleyin. VM'lerin ne kadar uzakta olduğuna bağlı olarak, tamamlanması birkaç dakika sürebilir. Daha uzun testler çalıştırmadan önce başarı için test etmek için daha az yineleme ile başlamayı düşünün.

## <a name="test-vms-running-linux"></a>Linux çalıştıran Test VM'leri

VM'leri test etmek için [SockPerf'i](https://github.com/mellanox/sockperf) kullanın.

### <a name="install-sockperf-on-the-vms"></a>VM'lere SockPerf'i yükleyin

Linux VM'lerinde (hem SENDER hem de RECEIVER), VM'lerinizde SockPerf'i hazırlamak için bu komutları çalıştırın:

#### <a name="centos--rhel---install-git-and-other-helpful-tools"></a>CentOS / RHEL - GIT ve diğer yararlı araçları yükleyin

`sudo yum install gcc -y -q`
`sudo yum install git -y -q`
`sudo yum install gcc-c++ -y`
`sudo yum install ncurses-devel -y`
`sudo yum install -y automake`

#### <a name="ubuntu---install-git-and-other-helpful-tools"></a>Ubuntu - GIT ve diğer yararlı araçları yükleyin

`sudo apt-get install build-essential -y`
`sudo apt-get install git -y -q`
`sudo apt-get install -y autotools-dev`
`sudo apt-get install -y automake`

#### <a name="bash---all"></a>Bash - tüm

Bash komut satırından (git yüklü varsayar)

`git clone https://github.com/mellanox/sockperf`
`cd sockperf/`
`./autogen.sh`
`./configure --prefix=`

Yapmak daha yavaş, birkaç dakika sürebilir

`make`

Yükleme hızlı olun

`sudo make install`

### <a name="run-sockperf-on-the-vms"></a>VMs üzerinde Çalıştır SockPerf

#### <a name="sample-commands-after-installation-serverreceiver---assumes-servers-ip-is-10004"></a>Yüklemeden sonra örnek komutlar. Sunucu/Alıcı - sunucunun IP'si 10.0.0.4 olduğunu varsayar

`sudo sockperf sr --tcp -i 10.0.0.4 -p 12345 --full-rtt`

#### <a name="client---assumes-servers-ip-is-10004"></a>İstemci - sunucunun IP 10.0.0.4 olduğunu varsayar

`sockperf ping-pong -i 10.0.0.4 --tcp -m 1400 -t 101 -p 12345  --full-rtt`

> [!Note]
> VM ve Gateway arasındaki iş testi sırasında ara atlama (örneğin Sanal Cihaz) olmadığından emin olun.
> Yukarıdaki iPERF/NTTTCP testlerinden gelen kötü sonuçlar (genel iş kaynağı açısından) varsa, sorunun olası kök nedenlerinin arkasındaki temel faktörleri anlamak için lütfen aşağıdaki makaleye bakın:https://docs.microsoft.com/azure/virtual-network/virtual-network-tcpip-performance-tuning

Özellikle, bu testler sırasında istemci ve sunucudan paralel olarak toplanan paket yakalama izlerinin (Wireshark/Network Monitor) analizi kötü performansın değerlendirilmesinde yardımcı olacaktır. Bu izler paket kaybı, yüksek gecikme, MTU boyutu içerebilir. parçalanma, TCP 0 Penceresi, Sıra Dışı parçaları ve benzeri.

## <a name="address-slow-file-copy-issues"></a>Yavaş dosya kopyalama sorunlarını giderme

Önceki adımlarla (iPERF/NTTTCP/etc.) değerlendirilen genel iş kaynağı iyi olsa bile, Windows Explorer kullanırken veya bir RDP oturumuboyunca sürükleyip düşerken yavaş dosya yla başa çıkma deneyimi yaşayabilirsiniz. Bu sorun normalde aşağıdaki faktörlerden biri veya her ikisi nedeniyle:

* Windows Gezgini ve RDP gibi dosya kopyalama uygulamaları, dosyaları kopyalarken birden çok iş parçacığı kullanmaz. Daha iyi performans için, 16 veya 32 iş parçacığı kullanarak dosyaları kopyalamak için [Richcopy](https://technet.microsoft.com/magazine/2009.04.utilityspotlight.aspx) gibi çok iş parçacığı dosya kopyalama uygulamasını kullanın. Richcopy'de dosya kopyalama için iş parçacığı numarasını değiştirmek için **Eylem** > **Kopyalama seçenekleri** > **Dosya kopyasını**tıklatın.

   ![Yavaş dosya kopyalama sorunları](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)<br>

   > [!Note]
   > Tüm uygulama aynı çalışmaz ve tüm uygulama/işlem tüm iş parçacıklarını kullanmaz. Testi çalıştırıyorsanız, bazı iş parçacıklarının boş olduğunu görebilirsiniz ve doğru iş parçacığı sonuçları sağlamaz.
   > Uygulama dosya aktarım performansınızı kontrol etmek için, uygulamanın veya dosya aktarımının en uygun iş parçacığı nın elde ini bulmak için iş parçacığının # unu art arda artırarak veya azaltarak çoklu iş parçacığı kullanın.

* Yetersiz VM disk okuma/yazma hızı. Daha fazla bilgi için Azure [Depolama Sorun Giderme'ye](../storage/common/storage-e2e-troubleshooting.md)bakın.

## <a name="on-premises-device-external-facing-interface"></a>Şirket içi aygıt dış alet etek arabirimi

Azure'un Yerel Ağ Ağ Ağ Geçidi'nde VPN üzerinden erişmesini istediğiniz şirket içi aralıkların alt ağlarından bahsetmiştik. Aynı anda, Azure'daki VNET adres alanını şirket içi aygıta tanımlayın.

* **Rota Tabanlı Ağ Geçidi**: Rota tabanlı VPN'ler için ilke veya trafik seçici herhangi biri için herhangi bir (veya joker kart) olarak yapılandırılır.

* **İlke Tabanlı Ağ Geçidi**: İlke tabanlı VPN'ler, şirket içi ağınız ile Azure VNet arasındaki adres önekleri birleşimlerini temel alan Paketleri IPsec tünellerinde şifreler ve yönlendirir. İlke (veya Trafik Seçici) çoğunlukla VPN yapılandırmasında bir erişim listesi olarak tanımlanır.

* **UsePolicyBasedTrafficSelector** bağlantıları: ("UsePolicyBasedTrafficSelectors" bir bağlantı üzerinde $True için şirket içinde ilke tabanlı VPN güvenlik duvarına bağlanmak için Azure VPN ağ geçidini yapılandıracaktır. PolicyBasedTrafficSelectors'u etkinleştirirseniz, VPN cihazınızın, Azure sanal ağ önekleri yerine şirket içi ağınızın (yerel ağ ağ geçidi) önekleri nizdeki tüm kombinasyonları ile tanımlanan eşleşen trafik seçicilerine sahip olduğundan emin olmanız gerekir herhangi bir-to-any.

Uygun olmayan yapılandırma, tünel içinde sık sık kopmalara, paket düşüşlerine, hatalı iş çıkışına ve gecikmeye neden olabilir.

## <a name="check-latency"></a>Gecikme yi kontrol edin

Aşağıdaki araçları kullanarak gecikmeyi kontrol edebilirsiniz:

* WinMTR
* TCPTraceroute
* `ping`ve `psping` (Bu araçlar RTT iyi bir tahmin sağlayabilir, ancak her durumda kullanılamaz.)

![Gecikme Yi kontrol et](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)

MS Network omurgasına girmeden önce herhangi bir atlamada yüksek bir gecikme gecikmesi artışı fark ederseniz, Internet Servis Sağlayıcınızla daha fazla araştırma yapmak isteyebilirsiniz.

"msn.net" içindeki atlamalardan büyük, olağandışı bir gecikme ani sıçraması fark edilirse, daha fazla araştırma için lütfen MS desteğine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi veya yardım için aşağıdaki bağlantıya göz atın:

* [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
