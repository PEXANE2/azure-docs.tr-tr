---
title: VM 'Ler için özel IP adresleri Yapılandırma-Azure portal
description: Azure portal kullanarak sanal makineler için özel IP adreslerini yapılandırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: kumud
ms.openlocfilehash: b1019b15463a03282c5d1bd8f0a878433d7f488e
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78199572"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal"></a>Azure portal kullanarak bir sanal makine için özel IP adresleri yapılandırma

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Aşağıdaki örnek adımlarda önceden oluşturulmuş basit bir ortam beklenir. Adımları bu belgede görüntülendikleri gibi çalıştırmak istiyorsanız, önce [sanal ağ oluşturma](quick-create-portal.md)bölümünde açıklanan test ortamını oluşturun.

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Statik özel IP adreslerini test etmek için VM oluşturma
Azure portal kullanarak Kaynak Yöneticisi dağıtım modunda bir VM oluşturma sırasında statik bir özel IP adresi ayarlayamazsınız. Önce VM 'yi oluşturmanız ve ardından özel IP 'sini statik olacak şekilde ayarlamanız gerekir.

*Testvnet*adlı VNET 'In *ön uç* alt AĞıNDA *DNS01* adlı bir VM oluşturmak için aşağıdaki adımları izleyin:

1. Tarayıcıdan https://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınızla oturum açın.
2. **Windows Server 2012 R2 Datacenter** > **Işlem** > **kaynak oluştur** ' a tıklayın, **bir dağıtım modeli Seç** listesinin zaten **Kaynak Yöneticisi**gösterdiğini ve ardından **Oluştur**' a tıklayarak aşağıdaki şekilde görüldüğü gibi.
   
    ![Azure portal VM oluşturma](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. **Temel bilgiler** bölmesinde, aşağıdaki şekilde görüldüğü gibi, oluşturulacak sanal makinenin adını (senaryoda*DNS01* ), yerel yönetici hesabını ve parolayı girin.
   
    ![Temel bilgiler bölmesi](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. Seçilen **konumun** *Orta ABD*olduğundan emin olun, ardından **kaynak grubu**altında **Varolanı Seç** ' e tıklayın, ardından **kaynak** grubu ' na ve ardından *testrg*' ye tıklayın ve ardından **Tamam**' a tıklayın.
   
    ![Temel bilgiler bölmesi](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. **Boyut seçin** bölmesinde, **a1 standart**' ı seçin ve ardından **Seç**' e tıklayın.
   
    ![Boyut bölmesi seçin](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. **Ayarlar** bölmesinde, özelliklerin aşağıdaki değerlerle ayarlandığından emin olun ve ardından **Tamam**' a tıklayın.
   
    -**depolama hesabı**: *vnetstorage*
   
   * **Ağ**: *testvnet*
   * **Alt ağ**: *ön uç*
     
     ![Boyut bölmesi seçin](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. **Özet** bölmesinde, **Tamam**' a tıklayın. Panonuzda aşağıdaki kutucuğun görüntülendiğini fark edin.
   
    ![Azure portal VM oluşturma](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

[Bir WINDOWS VM 'ye birden çok IP adresi atarken](virtual-network-multiple-ip-addresses-portal.md), GEREKMEDIKÇE, sanal makinenin işletim sistemi içinde Azure sanal makinesine atanan özel IP 'yi statik olarak atamanız önerilir. İşletim sistemi içinde özel IP adresini el ile ayarlarsanız, Azure [ağ arabirimine](virtual-network-network-interface-addresses.md#change-ip-address-settings)ATANMıŞ özel IP adresi ile aynı adres olduğundan emin olun veya sanal makineyle olan bağlantıyı kaybedebilirsiniz. [Özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları hakkında daha fazla bilgi edinin. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir sanal makine için statik özel IP adresi bilgilerini alma
Yukarıdaki adımlarla oluşturulan VM 'nin statik özel IP adresi bilgilerini görüntülemek için aşağıdaki adımları yürütün.

1. Azure portal, tüm > **sanal makinelere** **gözatıp** **' > ** **Tüm ayarlar** > **ağ arabirimlerini** > ve ardından listelenen tek ağ arabirimine tıklayın.
   
    ![VM kutucuğu dağıtılıyor](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. **Ağ arabirimi** bölmesinde, **Tüm ayarlar** > **IP adresleri** ' ne tıklayın ve **atama** ve **IP adresi** değerlerine dikkat edin.
   
    ![VM kutucuğu dağıtılıyor](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Var olan bir VM 'ye statik bir özel IP adresi ekleme
Yukarıdaki adımlar kullanılarak oluşturulan VM 'ye statik bir özel IP adresi eklemek için aşağıdaki adımları izleyin:

1. Yukarıda gösterilen **IP adresleri** bölmesinden **atama**altında **statik** ' e tıklayın.
2. **IP adresi**için *192.168.1.101* yazın ve ardından **Kaydet**' e tıklayın.
   
    ![Azure portal VM oluşturma](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> **Kaydet**'e tıkladıktan sonra, atamanın hala **dinamik**olarak ayarlandığını fark edersiniz. Bu, yazdığınız IP adresinin zaten kullanımda olduğunu gösterir. Farklı bir IP adresi deneyin.
> 
> 

[Bir WINDOWS VM 'ye birden çok IP adresi atarken](virtual-network-multiple-ip-addresses-portal.md), GEREKMEDIKÇE, sanal makinenin işletim sistemi içinde Azure sanal makinesine atanan özel IP 'yi statik olarak atamanız önerilir. İşletim sistemi içinde özel IP adresini el ile ayarlarsanız, Azure [ağ arabirimine](virtual-network-network-interface-addresses.md#change-ip-address-settings)ATANMıŞ özel IP adresi ile aynı adres olduğundan emin olun veya sanal makineyle olan bağlantıyı kaybedebilirsiniz. [Özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları hakkında daha fazla bilgi edinin. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>VM'den özel statik IP adresi kaldırma
Yukarıda oluşturulan VM 'den statik özel IP adresini kaldırmak için aşağıdaki adımları izleyin:

Yukarıda gösterilen **IP adresleri** bölmesinden **atama**altında **dinamik** ' e ve ardından **Kaydet**' e tıklayın.

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içinde IP adreslerini ayarlayın

[Bir WINDOWS VM 'ye birden çok IP adresi atarken](virtual-network-multiple-ip-addresses-portal.md), GEREKMEDIKÇE, sanal makinenin işletim sistemi içinde Azure sanal makinesine atanan özel IP 'yi statik olarak atamanız önerilir. İşletim sistemi içinde özel IP adresini el ile ayarlarsanız, Azure [ağ arabirimine](virtual-network-network-interface-addresses.md#change-ip-address-settings)ATANMıŞ özel IP adresi ile aynı adres olduğundan emin olun veya sanal makineyle olan bağlantıyı kaybedebilirsiniz. [Özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları hakkında daha fazla bilgi edinin. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[IP adresi ayarlarını](virtual-network-network-interface-addresses.md)yönetme hakkında bilgi edinin.

