---
title: VM'ler için özel IP adreslerini yapılandırma (Klasik) - Azure portalı | Microsoft Dokümanlar
description: Azure portalını kullanarak sanal makineler için özel IP adreslerini (Klasik) nasıl yapılandırıştırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9de9fd2eb7a46719b841a885ac173f8d9c1271c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71059102"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Azure portalını kullanarak sanal bir makine (Klasik) için özel IP adreslerini yapılandırma

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. [Kaynak Yöneticisi dağıtım modelinde statik bir özel IP adresini](virtual-networks-static-private-ip-arm-pportal.md)de yönetebilirsiniz.

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

İzleyen örnek adımlar zaten oluşturulmuş basit bir ortam bekler. Adımları bu belgede görüntülenen gibi çalıştırmak istiyorsanız, önce [vnet oluşturmak](virtual-networks-create-vnet-classic-pportal.md)ta açıklanan test ortamını oluşturun.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>VM oluştururken statik özel IP adresi nasıl belirtilir?
*TestVNet* adlı bir VNet'in *FrontEnd* alt netinde *192.168.1.101*statik özel IP'si ile *DNS01* adında bir VM oluşturmak için aşağıdaki adımları tamamlayın:

1. Tarayıcıdan https://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınızla oturum açın.
2. **Yenİ** > **İşlem** > **Windows Server 2012 R2 Datacenter'ı**seçin , **dağıtım modeli** seç listesinin zaten **Klasik'i**gösterdiğini ve ardından **Oluştur'u**seçtiğinizi unutmayın.
   
    ![Azure portalında VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. **VM oluştur**altında oluşturulacak VM'nin adını (senaryoda*DNS01),* yerel yönetici hesabını ve parolayı girin.
   
    ![Azure portalında VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. **İsteğe Bağlı Yapılandırma** > **Ağı** > **Sanal Ağı'nı**seçin ve ardından **TestVNet'i**seçin. **TestVNet** kullanılamıyorsa, *ABD'nin Orta* Konumunu kullandığınızdan ve bu makalenin başında açıklanan test ortamını oluşturduğunuzdan emin olun.
   
    ![Azure portalında VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. **Ağ**altında, şu anda seçilen alt net *FrontEnd*olduğundan emin olun , sonra **IP adresleri**seçin , **IP adresi atama** altında **Statik**seçin , ve sonra ip **adresi** için *192.168.1.101* girin aşağıda görüldüğü gibi.
   
    ![Azure portalında VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. **IP adreslerinin**altında **Tamam'ı** seçin, **Ağ**altında **Tamam'ı** seçin ve ardından **İsteğe Bağlı config**altında **Tamam'ı** seçin.
7. **VM Oluştur'un**altında **Oluştur'u**seçin. Panonuzda görüntülenen aşağıdaki döşemeye dikkat edin:
   
    ![Azure portalında VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir VM için statik özel IP adresi bilgileri nasıl alınır?
Yukarıdaki adımlarla oluşturulan VM için statik özel IP adresi bilgilerini görüntülemek için aşağıdaki adımları uygulayın.

1. Azure portalından **BROWSE ALL** > **Virtual machines (klasik)** > **DNS01** > **Tüm ayarlar** > **IP adreslerini** seçin ve ip adresi atamasını ve IP adresini aşağıda görüldüğü gibi fark edin.
   
    ![Azure portalında VM oluşturma](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Statik özel IP adresi VM'den nasıl kaldırılır?

**IP adreslerialtında,** **IP adresi atamasının**sağında **dinamik** seçeneğini belirleyin , **Kaydet'i**seçin ve ardından aşağıdaki resimde gösterildiği gibi **Evet'i**seçin:
   
    ![Create VM in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Varolan bir VM'ye statik özel IP adresi ekleme

1. Daha önce gösterilen **IP adresleri** **altında, IP adresi atamasının**sağında **statik'i** seçin.
2. **IP adresi**için *192.168.1.101* yazın , **Kaydet'i**seçin ve ardından **Evet'i**seçin.

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içindeki IP adreslerini ayarlama

Gerekli olmadıkça, Bir VM işletim sistemi içinde Azure sanal makinesine atanan özel IP'yi statik olarak atamamanız önerilir. İşletim sistemi içindeki özel IP adresini el ile ayarlarsanız, bunun Azure VM'ye atanan özel IP adresiyle aynı adres olduğundan emin olun veya sanal makineye bağlantınızı kaybedebilirsiniz. Sanal makinenin işletim sistemi içinde bir Azure sanal makinesine atanan genel IP adresini asla el ile atamamalısınız.

## <a name="next-steps"></a>Sonraki adımlar
* Ayrılmış [genel IP](virtual-networks-reserved-public-ip.md) adresleri hakkında bilgi edinin.
* Örnek [düzeyindeki genel IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adresleri hakkında bilgi edinin.
* Ayrılmış [IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx)başvurun.

