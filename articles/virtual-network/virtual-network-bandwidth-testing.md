---
title: Azure VM ağı aktarım hızını test etme
titlesuffix: Azure Virtual Network
description: Azure sanal makine ağ aktarım hızını test etme hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: steveesp
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: 80e8a5e5de1da2098d895e09b36fb209050743a0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60743096"
---
# <a name="bandwidththroughput-testing-ntttcp"></a>Bant genişliği/Işleme testi (NTTTCP)

Azure 'da ağ aktarım hızı performansını sınarken, sınama için ağı hedefleyen bir araç kullanmak ve performansı etkileyebilecek diğer kaynakların kullanımını en aza indirir. NTTTCP önerilir.

Aracı aynı boyuttaki iki Azure VM 'ye kopyalayın. Bir VM, gönderen ve diğeri alıcı olarak çalışır.

#### <a name="deploying-vms-for-testing"></a>Test için VM dağıtma
Bu testin amaçları doğrultusunda, iç IP 'lerini kullanabilmeniz ve yük dengeleyicileri testten hariç tutmak için iki VM 'nin aynı bulut hizmetinde veya aynı Kullanılabilirlik kümesinde olması gerekir. VIP ile test kurulabilir, ancak bu tür bir test bu belgenin kapsamı dışındadır.

ALıCıNıN IP adresini bir yere getirin. "A. b. c. r" IP 'si arayalım

VM 'deki çekirdek sayısını unutmayın. Bu "\#NUM\_çekirdekleri" arayalım

Sender VM ve alıcı VM 'de 300 saniye (veya 5 dakika) boyunca NTTTCP testini çalıştırın.

İpucu: Bu testi ilk kez ayarlarken daha çabuk geri bildirim almak için daha kısa bir test süresi deneyebilirsiniz. Araç beklenen şekilde çalışmaya başladıktan sonra en doğru sonuçlar için test süresini 300 saniyeye uzatın.

> [!NOTE]
> Gönderen **ve** alıcı **aynı** test süresi parametresini (-t) belirtmelidir.

Tek bir TCP akışını 10 saniye boyunca test etmek için:

Alıcı parametreleri: NTttcp-r-t 10-P 1

Gönderen parametreleri: NTttcp-s 10.27.33.7-t 10-n 1-P 1

> [!NOTE]
> Yukarıdaki örnek yalnızca yapılandırmanızı onaylamak için kullanılmalıdır. Bu belgenin ilerleyen kısımlarında, test için geçerli örnekler ele alınmıştır.

## <a name="testing-vms-running-windows"></a>WINDOWS çalıştıran VM 'Leri test etme:

#### <a name="get-ntttcp-onto-the-vms"></a>VM 'lere NTTTCP 'yi alın.

En son sürümü indirin:<https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769>

Ya da, taşındıysanız arama yapın <https://www.bing.com/search?q=ntttcp+download> \< :--ilk isabet edilmelidir

NTTTCP 'yi c:\\araçları gibi ayrı bir klasöre yerleştirmeyi düşünün

#### <a name="allow-ntttcp-through-the-windows-firewall"></a>Windows Güvenlik Duvarı üzerinden NTTTCP 'ye izin ver
ALıCıDA, Windows Güvenlik Duvarı 'nda NTTTCP trafiğinin gelmesini sağlamak için bir Izin verme kuralı oluşturun. Gelen belirli TCP bağlantı noktalarına izin vermek yerine NTTTCP programının tamamına ada göre izin vermek en kolay yoldur.

Windows Güvenlik Duvarı üzerinden şu şekilde NTttcp 'ye izin ver:

netsh advfirewall firewall add rule program =\<Path\>\\NTttcp. exe name = "NTttcp" protokol = any dir = in Action = Allow Enable = Yes profile = any

Örneğin, NTttcp. exe ' yi "c:\\Tools" klasörüne kopyaladıysanız şu komut olacaktır: 

netsh advfirewall firewall add rule program = c:\\Araçlar\\NTttcp. exe name = "NTttcp" protokol = any dir = in Action = Allow Enable = Yes profile = any

#### <a name="running-ntttcp-tests"></a>NTTTCP testleri çalıştırılıyor

ALıCıDA NTTTCP 'yi başlatın (PowerShell 'den değil,**cmd 'den çalıştırın**):

NTttcp-r – m [2\*\#NUM\_çekirdekler]\*,, a. b. c. r-t 300

VM 'de dört çekirdek ve IP adresi 10.0.0.4 varsa, şöyle görünür:

NTttcp-r – m 8,\*, 10.0.0.4-t 300


GÖNDERENDEN NTTTCP 'yi başlatın (PowerShell 'den değil,**cmd 'den çalıştırın**):

NTttcp-s – a 8,\*, 10.0.0.4-t 300 

Sonuçları bekle.


## <a name="testing-vms-running-linux"></a>LINUX çalıştıran VM 'Leri test etme:

Linux için nttcp kullanın. Buradan kullanılabilir<https://github.com/Microsoft/ntttcp-for-linux>

Linux VM 'lerinde (Gönderen ve alıcı), VM 'lerinize NTttcp-for-Linux hazırlamak için şu komutları çalıştırın:

CentOS-git 'i yükler:
``` bash
  yum install gcc -y  
  yum install git -y
```
Ubuntu-git 'i yükler:
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
Her ikisine de oluştur ve yüklensin:
``` bash
 git clone https://github.com/Microsoft/ntttcp-for-linux
 cd ntttcp-for-linux/src
 make && make install
```

Windows örneğinde olduğu gibi, Linux alıcının IP 10.0.0.4 olduğunu varsaytık

ALıCıDA NTTTCP-for-Linux 'ı başlatın:

``` bash
ntttcp -r -t 300
```

GÖNDERENIN üzerinde şunu çalıştırın:

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
Zaman parametresi verilmezse test uzunluğu varsayılan olarak 60 saniyedir

## <a name="testing-between-vms-running-windows-and-linux"></a>Windows ve LINUX çalıştıran VM 'Ler arasında test:

Bu senaryolarda, testin çalışabilmesi için hiçbir eşitleme modunu etkinleştirmemiz gerekir. Bu, Linux için **-N bayrağı** ve Windows için **NS bayrağı** kullanılarak yapılır.

#### <a name="from-linux-to-windows"></a>Linux 'tan Windows 'a:

Alıcı \<Windows>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

Sender \<Linux>:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-to-linux"></a>Windows 'dan Linux 'a:

Alıcı \<Linux>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

Gönderen \<Windows>:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```
## <a name="testing-cloud-service-instances"></a>Bulut hizmeti örnekleri sınanıyor:
ServiceDefinition. csdef ' ye aşağıdaki bölümü eklemeniz gerekir
```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" />
</Endpoints> 
```

## <a name="next-steps"></a>Sonraki adımlar
* Sonuçlara bağlı olarak, senaryolarınız için [ağ verimlilik makinelerini iyileştirmek](virtual-network-optimize-network-bandwidth.md) için yer olabilir.
* [Bant genişliğinin sanal makinelere nasıl ayrıldığı](virtual-machine-network-throughput.md) hakkında bilgi edinin
* [Azure sanal ağ hakkında sık sorulan sorular (SSS)](virtual-networks-faq.md) hakkında daha fazla bilgi edinin
