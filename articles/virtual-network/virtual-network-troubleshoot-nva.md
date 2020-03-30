---
title: Azure'da sorun giderme ağı sanal cihaz sorunları | Microsoft Dokümanlar
description: Azure'da ağ sanal cihaz sorunlarını nasıl gidereceklerini öğrenin.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/26/2018
ms.author: genli
ms.openlocfilehash: b998043bc7d896989590ac21db5f309a81cc02bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71056828"
---
# <a name="network-virtual-appliance-issues-in-azure"></a>Azure'da ağ sanal cihaz sorunları

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Microsoft Azure'da üçüncü taraf Network Virtual Appliance (NVA) kullanırken VM veya VPN bağlantısı sorunları ve hatalarıyla karşılaşabilirsiniz. Bu makale, NVA yapılandırmaları için temel Azure Platformu gereksinimlerini doğrulamanıza yardımcı olacak temel adımlar sağlar.

Üçüncü taraf NV'ler için teknik destek ve Azure platformuyla tümleştirmeleri NVA satıcısı tarafından sağlanır.

> [!NOTE]
> NVA içeren bir bağlantı veya yönlendirme sorununz varsa, doğrudan [NVA satıcısına başvurmalısınız.](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="checklist-for-troubleshooting-with-nva-vendor"></a>NVA satıcısıyla sorun giderme için denetim listesi

- NVA VM yazılımı için yazılım güncellemeleri
- Hizmet Hesabı kurulumu ve işlevselliği
- Trafiği NVA'ya yönlendiren sanal ağ alt ağlarında kullanıcı tanımlı rotalar (ÜDR)
- NVA'dan trafiği yönlendiren sanal ağ alt ağlarında UdR'lar
- NVA içindeki yönlendirme tabloları ve kuralları (örneğin, NIC1'den NIC2'ye kadar)
- Ağ trafiğini alma ve göndermeyi doğrulamak için NVA NIC'leri izleme
- Standart SKU ve Genel IP'ler kullanırken, trafiğin NVA'ya yönlendirilmesiiçin bir NSG ve açık bir kural oluşturulmalıdır.

## <a name="basic-troubleshooting-steps"></a>Temel sorun giderme adımları

- Temel yapılandırmayı kontrol edin
- NVA performansını kontrol edin
- Gelişmiş ağ sorun giderme

## <a name="check-the-minimum-configuration-requirements-for-nvas-on-azure"></a>Azure'daki NV'ler için minimum yapılandırma gereksinimlerini kontrol edin

Her NVA'nın Azure'da düzgün çalışması için temel yapılandırma gereksinimleri vardır. Aşağıdaki bölümde bu temel yapılandırmaları doğrulamak için adımlar sağlar. Daha fazla bilgi için [NVA satıcısına başvurun.](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)

**NVA'da IP iletme özelliğinin etkin olup olmadığını kontrol edin**

Azure portalı kullanma

1. [Azure portalında](https://portal.azure.com)NVA kaynağını bulun, Ağ'ı seçin ve ardından Ağ arabirimini seçin.
2. Ağ arabirimi sayfasında IP yapılandırmasını seçin.
3. IP iletmenin etkin olduğundan emin olun.

PowerShell kullanma

1. PowerShell'i açın ve ardından Azure hesabınızda oturum açın.
2. Aşağıdaki komutu çalıştırın (parantez içinde bulunan değerleri bilgilerinizle değiştirin):

   ```powershell
   Get-AzNetworkInterface -ResourceGroupName <ResourceGroupName> -Name <NicName>
   ```

3. **EnableIPForwarding** özelliğini kontrol edin.
4. IP iletme etkin değilse, etkinleştirmek için aşağıdaki komutları çalıştırın:

   ```powershell
   $nic2 = Get-AzNetworkInterface -ResourceGroupName <ResourceGroupName> -Name <NicName>
   $nic2.EnableIPForwarding = 1
   Set-AzNetworkInterface -NetworkInterface $nic2
   Execute: $nic2 #and check for an expected output:
   EnableIPForwarding   : True
   NetworkSecurityGroup : null
   ```

**Standart SKU Pubilc IP kullanırken NSG'yi kontrol edin** Standart SKU ve Genel IP'ler kullanırken, NVA trafiğine izin vermek için bir NSG ve açık bir kural oluşturulmalıdır.

**Trafiğin NVA'ya yönlendirilip yönlendirilemeyeceğini kontrol edin**

1. [Azure portalında](https://portal.azure.com), **Ağ İzleyicisini**aç, **Next Hop'u**seçin.
2. Trafiği NVA'ya yönlendirecek şekilde yapılandırılan bir VM ve bir sonraki atlamayı görüntülemek için bir hedef IP adresi belirtin. 
3. NVA **bir sonraki atlama**olarak listelenmiyorsa, Azure rota tablolarını denetleyin ve güncelleştirin.

**Trafiğin NVA'ya ulaşıp ulaşamayacağını kontrol edin**

1. [Azure portalında](https://portal.azure.com) **Ağ İzleyicisi'ni**açın ve **ardından IP Akışını Doğrula'yı**seçin. 
2. NVA'nın VM ve IP adresini belirtin ve ardından trafiğin herhangi bir Ağ güvenlik grupları (NSG) tarafından engellenip engellenmediğini kontrol edin.
3. Trafiği engelleyen bir NSG kuralı varsa, NSG'yi **etkili güvenlik** kurallarında bulun ve trafiğin geçmesine izin verecek şekilde güncelleştirin. Ardından IP Akışını Yeniden **Doğrula'yı** çalıştırın ve VM'den dahili veya harici IP adresinize TCP iletişimlerini test etmek için **Bağlantı sorun giderini** kullanın.

**NVA ve VM'lerin beklenen trafiği dinleyip dinlemediğini kontrol edin**

1. RDP veya SSH kullanarak NVA'ya bağlanın ve aşağıdaki komutu çalıştırın:

    Windows için:

        netstat -an

    Linux için:

        netstat -an | grep -i listen
2. Sonuçlarda listelenen NVA yazılımı tarafından kullanılan TCP bağlantı noktasını görmüyorsanız, bu bağlantı noktalarına ulaşan trafiği dinlemek ve bunlara yanıt vermek için Uygulamayı NVA ve VM'de yapılandırmanız gerekir. [Gerektiğinde yardım için NVA satıcısına başvurun.](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)

## <a name="check-nva-performance"></a>NVA Performansını Kontrol Edin

### <a name="validate-vm-cpu"></a>VM CPU'u doğrula

CPU kullanımı yüzde 100'e yaklaşırsa, ağ paketi düşüşlerini etkileyen sorunlarla karşılaşabilirsiniz. VM'niz, Azure portalında belirli bir zaman aralığı için ortalama CPU raporediyor. CPU ani sırasında, konuk VM'deki hangi işlemin yüksek CPU'ya neden olduğunu araştırın ve mümkünse azlayın. Ayrıca VM'yi daha büyük bir SKU boyutuna yeniden boyutlandırmanız veya sanal makine ölçeği kümesi için örnek sayısını artırmanız veya CPU kullanımında otomatik ölçeklendirmeye ayarlamanız da gerekebilir. Bu sorunlardan biri için, gerektiğinde [yardım için NVA satıcısına başvurun.](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)

### <a name="validate-vm-network-statistics"></a>VM Ağ istatistiklerini doğrulama

VM ağı ani artışlar kullanıyorsa veya yüksek kullanım sürelerini gösteriyorsa, daha yüksek üretim özellikleri elde etmek için VM'nin SKU boyutunu da artırmanız gerekebilir. Hızlandırılmış Ağ özelliğini etkinleştirerek VM'yi yeniden dağıtabilirsiniz. NVA'nın Hızlandırılmış Ağ özelliğini destekleyip desteklemediğini doğrulamak için gerektiğinde [yardım için NVA satıcısına başvurun.](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)

## <a name="advanced-network-administrator-troubleshooting"></a>Gelişmiş ağ yöneticisi sorun giderme

### <a name="capture-network-trace"></a>Ağ izleme yakalama
**[PsPing](https://docs.microsoft.com/sysinternals/downloads/psping)** veya **Nmap'i**çalıştırırken kaynak VM, NVA ve hedef VM'de eşzamanlı bir ağ izlemesini yakalayın ve izlemeyi durdurun.

1. Eşzamanlı ağ izlemesini yakalamak için aşağıdaki komutu çalıştırın:

   **Windows için**

   netsh trace start capture=yes tracefile=c:\server_IP.etl scenario=netconnection

   **Linux için**

   sudo tcpdump -s0 -i eth0 -X -w vmtrace.cap

2. Kaynak VM'den hedef VM'ye **PsPing** veya **Nmap** kullanın (örneğin: `PsPing 10.0.0.4:80` veya). `Nmap -p 80 10.0.0.4`
3. [Network Monitor](https://www.microsoft.com/download/details.aspx?id=4865) veya tcpdump kullanarak hedef VM'den ağ izlemesini açın. **PsPing** veya **Nmap'i** çalıştırdığınız Kaynak VM'nin `IPv4.address==10.0.0.4 (Windows netmon)` IP'si `tcpdump -nn -r vmtrace.cap src or dst host 10.0.0.4` için (Linux) bir görüntü filtresi uygulayın.

### <a name="analyze-traces"></a>İzleri analiz et

Arka uç VM izlemesine gelen paketleri görmüyorsanız, büyük olasılıkla bir NSG veya UDR müdahale veya NVA yönlendirme tabloları yanlıştır.

Gelen paketleri görüyorsanız ancak yanıt yoksa, VM uygulaması veya güvenlik duvarı sorununu çözmeniz gerekebilir. Bu sorunlardan biri için, [gerektiğinde yardım için NVA satıcısına başvurun.](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)