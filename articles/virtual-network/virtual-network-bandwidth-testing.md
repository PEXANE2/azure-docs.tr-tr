---
title: Azure VM ağ iş buzunu test etme
titlesuffix: Azure Virtual Network
description: Azure sanal makine ağı iş birliğini nasıl test edebilirsiniz öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60743096"
---
# <a name="bandwidththroughput-testing-ntttcp"></a>Bant genişliği/İş-İş- İş Testi (NTTTCP)

Azure'da ağ iş hacmi performansını test ederken, en iyi sonucu test etmek için ağı hedefleyen ve performansı etkileyebilecek diğer kaynakların kullanımını en aza indiren bir araç kullanmak gerekir. NTTTCP önerilir.

Aracı aynı boyuttaki iki Azure VM'sine kopyalayın. Bir VM SENDER, diğeri ALıCı olarak işlev görür.

#### <a name="deploying-vms-for-testing"></a>Test için VM'leri dağıtma
Bu testin amaçları için, iki VM'nin aynı Bulut Hizmetinde veya aynı Kullanılabilirlik Kümesi'nde olması gerekir, böylece dahili IP'lerini kullanabilir ve Yük Dengeleyicilerini testten çıkarabiliriz. VIP ile test etmek mümkündür, ancak bu tür testler bu belgenin kapsamı dışındadır.

ALICI'nın IP adresini not alın. Buna IP'ye "a.b.c.r" diyelim.

VM'deki çekirdek sayısını not edin. Buna "num\#\_cores" diyelim.

Gönderen VM ve alıcı VM'de NTTTCP testini 300 saniye (veya 5 dakika) çalıştırın.

İpucu: Bu testi ilk kez ayarlarken, geri bildirimi daha erken almak için daha kısa bir test süresi deneyebilirsiniz. Araç beklendiği gibi çalıştığında, en doğru sonuçlar için test süresini 300 saniyeye kadar uzatın.

> [!NOTE]
> Gönderen **ve** alıcı aynı test süresi **parametresini** (-t) belirtmelidir.

Tek bir TCP akışını 10 saniye boyunca test etmek için:

Alıcı parametreleri: ntttcp -r -t 10 -P 1

Gönderen parametreleri: ntttcp -s10.27.33.7 -t 10 -n 1 -P 1

> [!NOTE]
> Önceki örnek yalnızca yapılandırmanızı onaylamak için kullanılmalıdır. Geçerli sınama örnekleri daha sonra bu belgede ele alınmıştır.

## <a name="testing-vms-running-windows"></a>WINDOWS çalıştıran VM'leri test etme:

#### <a name="get-ntttcp-onto-the-vms"></a>NTTTCP'yi VM'lere geçirin.

En son sürümü indirin:<https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769>

Ya da taşınırsa <https://www.bing.com/search?q=ntttcp+download> \< onu arayın: -- ilk vurulmalı

NTTTCP'yi c:\\tools gibi ayrı bir klasöre koymayı düşünün

#### <a name="allow-ntttcp-through-the-windows-firewall"></a>Windows güvenlik duvarından NTTTCP'ye izin verme
ALıCı'da, NTTTCP trafiğinin gelmesine izin vermek için Windows Güvenlik Duvarı'nda İzin Ver kuralı oluşturun. Belirli TCP bağlantı noktalarının gelene izin vermek yerine, tüm NTTTCP programına adıyla izin vermek en kolayıdır.

NTTTCP'nin Windows Güvenlik Duvarı'ndan şu şekilde geçmesine izin verin:

netsh advfirewall güvenlik duvarı\<kural\>\\programı eklemek = PATH ntttcp.exe adı="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY

Örneğin, ntttcp.exe'yi "c:\\tools" klasörüne kopyaladıysanız, bu komut olacaktır: 

netsh advfirewall güvenlik duvarı kural\\programı=c: araçlar\\nttcp.exe adı="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY

#### <a name="running-ntttcp-tests"></a>NTTTCP testlerini çalıştırma

Alıcıda NTTTCP'yi başlatın **(POWERShell'den değil CMD'den çalıştırın):**

ntttcp -r –m\*\#[2\_num\*çekirdek], a.b.c.r -t 300

VM dört çekirdek ve 10.0.0.4 bir IP adresi varsa, bu gibi görünür:

ntttcp -r –m\*8, 10.0.0.4 -t 300


Gönderen 'de NTTTCP'yi başlatın **(PowerShell'den değil CMD'den çalıştırın):**

ntttcp -s –m\*8, 10.0.0.4 -t 300 

Sonuçları bekleyin.


## <a name="testing-vms-running-linux"></a>LINUX çalıştıran VM'leri test etme:

nttcp-for-linux kullanın. Bu kullanılabilir<https://github.com/Microsoft/ntttcp-for-linux>

Linux VM'lerinde (hem SENDER hem de RECEIVER), VM'lerinizde nttcp-for-linux hazırlamak için bu komutları çalıştırın:

CentOS - Yükle yükleyin:
``` bash
  yum install gcc -y  
  yum install git -y
```
Ubuntu - Git yükleyin:
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
Her ikisinde de Yap ve Yükle:
``` bash
 git clone https://github.com/Microsoft/ntttcp-for-linux
 cd ntttcp-for-linux/src
 make && make install
```

Windows örneğinde olduğu gibi, Linux RECEIVER IP'sinin 10.0.0.4 olduğunu varsayıyoruz.

Alıcı da NTTTCP-for-Linux'u başlatın:

``` bash
ntttcp -r -t 300
```

Ve GÖNDEREN'de, çalıştırın:

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
Zaman parametresi verilmezse test uzunluğu varsayılan olarak 60 saniyeye

## <a name="testing-between-vms-running-windows-and-linux"></a>Windows ve LINUX çalıştıran VM'ler arasında test:

Bu senaryolarda, testin çalıştırabilmesi için eşitlemesiz modunu etkinleştirmeliyiz. Bu, Linux için **-N ve** Windows için **-ns bayrağı** kullanılarak yapılır.

#### <a name="from-linux-to-windows"></a>Linux'tan Windows'a:

Alıcı \<Windows>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

Gönderen \<Linux> :

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-to-linux"></a>Windows'dan Linux'a:

Alıcı \<Linux>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

Gönderen \<Windows>:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```
## <a name="testing-cloud-service-instances"></a>Bulut Hizmeti Örneklerini Test Etme:
ServiceDefinition.csdef'inize aşağıdaki bölümü eklemeniz gerekir
```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" />
</Endpoints> 
```

## <a name="next-steps"></a>Sonraki adımlar
* Sonuçlara bağlı olarak, senaryonuz için [ağ iş yeri makinelerini optimize](virtual-network-optimize-network-bandwidth.md) etmek için yer olabilir.
* [Bant genişliğinin sanal makinelere](virtual-machine-network-throughput.md) nasıl tahsis edildiğini okuyun
* Azure Sanal Ağı ile daha fazla bilgi edinin [sık sorulan sorular (SSS)](virtual-networks-faq.md)
