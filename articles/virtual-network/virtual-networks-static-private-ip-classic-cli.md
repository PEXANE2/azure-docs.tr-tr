---
title: VM 'Ler için özel IP adresleri yapılandırma (klasik)-CLı
titlesuffix: Azure Virtual Network
description: Klasik Azure komut satırı arabirimi (CLI) kullanarak sanal makineler (Klasik) için özel IP adresleri yapılandırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: 9deaf4b1d80ad4e55e7c971998e8b1f5ea562257
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74196576"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-classic-cli"></a>Klasik Azure CLI kullanarak sanal makine (Klasik) için özel IP adreslerini yapılandırın

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [, Kaynak Yöneticisi dağıtım modelinde statik bir özel IP adresi yönetebilirsiniz](virtual-networks-static-private-ip-arm-cli.md).

Önceden oluşturulmuş basit bir ortam izleyen beklediğiniz Klasik Azure CLI komutları örneği. Komutları bu belgede görüntülendikleri gibi çalıştırmak istiyorsanız, önce [VNET oluşturma](virtual-networks-create-vnet-classic-cli.md)bölümünde açıklanan test ortamını oluşturun.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Bir VM oluşturulurken özel bir statik IP adresi belirtme
Yukarıdaki senaryoya bağlı olarak *TestService* adlı yeni bir bulut hizmetinde *DNS01* adlı yeni bir sanal makine oluşturmak için aşağıdaki adımları izleyin:

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](/cli/azure/install-cli-version-1.0); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Bulut hizmetini oluşturmak için **Azure hizmeti oluşturma** komutunu çalıştırın.
   
        azure service create TestService --location uscentral
   
    Beklenen çıktı:
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. VM oluşturmak için **Azure VM oluşturma** komutunu çalıştırın. Statik özel IP adresi değeri dikkat edin. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    Beklenen çıktı:
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * **-l (veya --location)** . VM'nin oluşturulacağı azure bölgesi. Bizim senaryomuz için bu *centralus*’tur.
   * **-n (veya--VM-adı)** . Oluşturulacak VM adı.
   * **-w (veya--sanal-ağ-adı)** . VM'nin oluşturulacağı Vnet'in adı. 
   * **-S (veya--statik-IP)** . Statik özel IP adresi VM için.
   * **TestService**. VM'nin oluşturulacağı bulut hizmeti adı.
   * **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v 14.2**. VM oluşturmak için kullanılan görüntü.
   * **AdminUser**. Windows VM için Yönetici'yi tıklatın.
   * <strong>AdminP@ssw0rd</strong>. Windows VM yerel yönetici parolası.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir sanal makine için statik özel IP adresi bilgilerini alma
Yukarıdaki betikle oluşturulan VM 'nin statik özel IP adresi bilgilerini görüntülemek için aşağıdaki Azure CLı komutunu çalıştırın ve *ağ Staticıp*değerini gözlemleyin:

    azure vm static-ip show DNS01

Beklenen çıktı:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>VM'den özel statik IP adresi kaldırma
Statik özel IP adresini kaldırmak için yukarıdaki komut VM'ye aşağıdaki Azure CLI komutunu çalıştırın eklendi:

    azure vm static-ip remove DNS01

Beklenen çıktı:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Mevcut bir VM'ye statik özel IP ekleme
Yukarıdaki betiği kullanarak oluşturulan VM 'ye statik bir özel IP adresi eklemek için aşağıdaki komutu çalıştırın:

    azure vm static-ip set DNS01 192.168.1.101

Beklenen çıktı:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içinde IP adreslerini ayarlayın

Statik olarak bir sanal makinenin işletim sistemi içinde Azure sanal makinesine atanmış özel IP sürece atamayın, önerilen gerekli. İşletim sistemi içinde özel IP adresini el ile ayarlamanız durumunda Azure VM'sine atanan özel IP adresiyle aynı adresi olduğundan emin olun veya sanal makineye bağlantı kaybedebilir. Sanal makinenin işletim sistemi içindeki bir Azure sanal makinesine atanan genel IP adresini el ile atamayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Ayrılmış genel IP](virtual-networks-reserved-public-ip.md) adresleri hakkında bilgi edinin.
* [Örnek düzeyi genel IP (ıLPıP)](virtual-networks-instance-level-public-ip.md) adresleri hakkında bilgi edinin.
* [Ayrılmış IP REST API 'lerine](https://msdn.microsoft.com/library/azure/dn722420.aspx)danışın.