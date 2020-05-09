---
title: Azure 'da bir sanal ağ silinemiyor | Microsoft Docs
description: Azure 'da bir sanal ağı silememe sorunu nasıl giderebileceğinizi öğrenin.
services: virtual-network
documentationcenter: na
author: chadmath
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: c2152b872c82c224c786e56db0318c9df994ac25
ms.sourcegitcommit: c8a0fbfa74ef7d1fd4d5b2f88521c5b619eb25f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82801595"
---
# <a name="troubleshooting-failed-to-delete-a-virtual-network-in-azure"></a>Sorun giderme: Azure 'da bir sanal ağ silinemedi
<p class="alert is-flex is-primary"><span class="has-padding-left-medium has-padding-top-extra-small"><a class="button is-primary" href="https://azurevirtualsupportagent.services.microsoft.com?content=a13a0908-28e0-f9f7-c6c7-301fcd187560" target='_blank'>Start</a></span><span class="has-padding-small"> <b>Otomatikleştirilmiş tanılamayı</b> çalıştırmak için sanal aracımızı kullanarak sorununuzu hızlı bir şekilde çözmeyi başlatın.</span> <span class="has-padding-small"> <a href="https://privacy.microsoft.com/privacystatement" target='_blank'> <sub>Privacy Statement</sub> Gizlilik <div align="right">bildirimi</div></a></span></p>
Microsoft Azure bir sanal ağı silmeye çalıştığınızda hata alabilirsiniz. Bu makalede, bu sorunu çözmenize yardımcı olacak sorun giderme adımları sunulmaktadır. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Sorun giderme rehberi 

1. Sanal [ağda bir sanal ağ geçidinin çalışıp çalışmadığını denetleyin](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).
2. [Sanal ağda bir uygulama ağ geçidinin çalışıp çalışmadığını denetleyin](#check-whether-an-application-gateway-is-running-in-the-virtual-network).
3. [Sanal ağda Azure Active Directory etki alanı hizmetinin etkin olup olmadığını denetleyin](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).
4. [Sanal ağın diğer kaynağa bağlanıp bağlanmadığını denetleyin](#check-whether-the-virtual-network-is-connected-to-other-resource).
5. Sanal [ağda bir sanal makinenin hala çalışıp çalışmadığını denetleyin](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).
6. [Sanal ağın geçiş sırasında takılı olup olmadığını denetleyin](#check-whether-the-virtual-network-is-stuck-in-migration).

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

### <a name="check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network"></a>Sanal ağda bir sanal ağ geçidinin çalışıp çalışmadığını denetleyin

Sanal ağı kaldırmak için öncelikle sanal ağ geçidini kaldırmanız gerekir.

Klasik sanal ağlar için Azure portal klasik sanal ağın **genel bakış** sayfasına gidin. **VPN bağlantıları** bölümünde Ağ Geçidi sanal ağda çalışıyorsa, ağ geçidinin IP adresini görürsünüz. 

![Ağ geçidinin çalışıp çalışmadığını denetleyin](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Sanal ağlar için, sanal ağın **genel bakış** sayfasına gidin. Sanal ağ geçidi için **bağlı cihazları** denetleyin.

![Bağlı cihazı denetleme](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Ağ geçidini kaldırabilmeniz için önce ağ geçidinde **bağlantı** nesnelerini kaldırın. 

### <a name="check-whether-an-application-gateway-is-running-in-the-virtual-network"></a>Sanal ağda bir uygulama ağ geçidinin çalışıp çalışmadığını denetleyin

Sanal ağın **genel bakış** sayfasına gidin. Uygulama ağ geçidi için **bağlı cihazları** denetleyin.

![Bağlı cihazı denetleme](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Bir uygulama ağ geçidi varsa, sanal ağı silebilmeniz için önce onu kaldırmanız gerekir.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network"></a>Sanal ağda Azure Active Directory etki alanı hizmetinin etkin olup olmadığını denetleyin

Active Directory Etki Alanı hizmeti etkin ve sanal ağa bağlıysa, bu sanal ağı silemezsiniz. 

![Bağlı cihazı denetleme](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

Hizmeti devre dışı bırakmak için [Azure Portal kullanarak Azure Active Directory Domain Services devre dışı bırak](../active-directory-domain-services/delete-aadds.md)' a bakın.

### <a name="check-whether-the-virtual-network-is-connected-to-other-resource"></a>Sanal ağın diğer kaynağa bağlanıp bağlanmadığını denetleyin

Devre bağlantılarını, bağlantıları ve sanal ağ eşlemelerini denetleyin. Bunlardan herhangi biri sanal ağ silmenin başarısız olmasına neden olabilir. 

Önerilen silme sırası aşağıdaki gibidir:

1. Ağ geçidi bağlantıları
2. Ağ geçitleri
3. 'Nin
4. Sanal ağ eşlemeleri
5. App Service Ortamı (Ao)

### <a name="check-whether-a-virtual-machine-is-still-running-in-the-virtual-network"></a>Sanal ağda bir sanal makinenin hala çalışıp çalışmadığını denetleyin

Sanal ağda sanal makine olmadığından emin olun.

### <a name="check-whether-the-virtual-network-is-stuck-in-migration"></a>Sanal ağın geçiş sırasında takılı olup olmadığını denetleyin

Sanal ağ bir geçiş durumunda takıldığında, silinemez. Geçişi durdurmak için aşağıdaki komutu çalıştırın ve ardından sanal ağı silin.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Sanal Ağ](virtual-networks-overview.md)
- [Azure Sanal Ağ hakkında sık sorulan sorular (SSS)](virtual-networks-faq.md)
