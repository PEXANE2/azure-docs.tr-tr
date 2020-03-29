---
title: Azure DevTest Labs'da paylaşılan IP adreslerini anlama | Microsoft Dokümanlar
description: Azure DevTest Laboratuvarlarının, laboratuvar VM'lerinize erişmek için gereken genel IP adreslerini en aza indirmek için paylaşılan IP adreslerini nasıl kullandığını öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2019
ms.author: spelluru
ms.openlocfilehash: f7c9feedddab1aea031cb3a8879e868aae04df00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65236878"
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Azure DevTest Labs'da paylaşılan IP adreslerini anlama

Azure DevTest Labs, laboratuvar VM'lerinize erişmek için gereken genel IP adresi sayısını en aza indirmek için laboratuvar VM'lerinin aynı genel IP adresini paylaşmasına olanak tanır.  Bu makalede, paylaşılan IP'lerin nasıl çalıştığı ve ilgili yapılandırma seçenekleri açıklanmaktadır.

## <a name="shared-ip-setting"></a>Paylaşılan IP ayarı

Bir laboratuvar oluşturduğunuzda, sanal bir ağın alt ağında oluşturulur.  Varsayılan olarak, bu alt **ağ, Paylaşılan Genel IP** kümesini *Evet*olarak etkinleştirme ile oluşturulur.  Bu yapılandırma, tüm alt ağ için bir genel IP adresi oluşturur.  Sanal ağları ve alt ağları yapılandırma hakkında daha fazla bilgi için Azure [DevTest Labs'da sanal ağı yapılandırma'ya](devtest-lab-configure-vnet.md)bakın.

![Yeni laboratuvar alt ağı](media/devtest-lab-shared-ip/lab-subnet.png)

Varolan laboratuvarlar için, **Sanal Ağlar > Yapılandırma ve ilkeleri**seçerek bu seçeneği etkinleştirebilirsiniz. Ardından, listeden bir sanal ağ seçin ve seçili bir alt ağ için **PAYLAŞıLAN GENEL IP'Yi ETKINLEŞTIR'i** seçin. Laboratuvar VM'leri arasında herkese açık bir IP adresini paylaşmak istemiyorsanız, bu seçeneği herhangi bir laboratuvarda da devre dışı kullanabilirsiniz.

Bu laboratuvarda oluşturulan tüm VM'ler paylaşılan bir IP kullanmak için varsayılan dır.  VM oluşturulurken, bu ayar **IP adresi yapılandırması**altında **Gelişmiş ayarlar** sayfasında görülebilir.

![Yeni VM](media/devtest-lab-shared-ip/new-vm.png)

- **Paylaşılan:** **Paylaşılan** olarak oluşturulan tüm VM'ler tek bir kaynak grubuna (RG) yerleştirilir. Söz ve RG'deki tüm VM'ler bu IP adresini kullanır.
- **Genel:** Oluşturduğunuz her VM'nin kendi IP adresi vardır ve kendi kaynak grubunda oluşturulur.
- **Özel:** Oluşturduğunuz her VM özel bir IP adresi kullanır. Uzak Masaüstü ile bu VM'ye doğrudan internetten bağlanamazsınız.

Paylaşılan IP etkinli bir VM alt ağa eklendiğinde, DevTest Labs otomatik olarak bir yük dengeleyicisine VM ekler ve ortak IP adresine bir TCP bağlantı noktası numarası atar ve VM'deki RDP bağlantı noktasına iletir.  

## <a name="using-the-shared-ip"></a>Paylaşılan IP'yi kullanma

- **Linux kullanıcıları:** IP adresi veya tam nitelikli etki alanı adı kullanarak VM SSH, bir üst üste, ardından bağlantı noktası tarafından takip. Örneğin, aşağıdaki resimde, VM'ye bağlanmak için RDP `mydevtestlab597975021002.eastus.cloudapp.azure.com:50661`adresi .

  ![VM örneği](media/devtest-lab-shared-ip/vm-info.png)

- **Windows kullanıcıları:** Önceden yapılandırılmış bir RDP dosyasını indirmek ve VM'ye erişmek için Azure portalındaki **Bağlan** düğmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure DevTest Labs'da laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md)
* [Azure DevTest Labs'da sanal ağı yapılandırma](devtest-lab-configure-vnet.md)





