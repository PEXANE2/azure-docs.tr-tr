---
title: Azure Güvenlik Merkezi'nde disk şifreleme uygulayın | Microsoft Dokümanlar
description: Bu belge, Azure Güvenlik Merkezi önerisini nasıl uygulayacağınızı gösterir **Disk şifrelemesi uygulayın.**
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: e0677ecc63573d04e384a0104ee0e4e28b4d5e3b
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80473278"
---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde disk şifreleme uygulayın

Azure Güvenlik Merkezi, hem Windows'ta hem de Linux VM'de şifrelenmemiş disklerde Azure Disk Şifrelemesi kullanmanızı önerir. Disk Şifrelemesi Windows ve Linux IaaS VM disklerinizi şifrelemenizi sağlar.  Şifreleme hem işletim sistemi hem de VM’nizin üzerindeki veri birimleri için önerilir.

Disk Şifreleme, Windows'un endüstri standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) özelliğini ve Linux'un [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) özelliğini kullanır. Bu özellikler, verilerinizin korunmasına ve korunmasına ve kuruluş güvenliği ve uyumluluk taahhütlerinizi karşılamaya yardımcı olmak için işletim sistemi ve veri şifrelemesi sağlar. Disk Şifreleme, Key Vault aboneliğinizdeki disk şifreleme anahtarlarını ve sırlarını kontrol etmenize ve yönetmenize yardımcı olmak için [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) ile entegre olurken, VM diskleri'ndeki tüm verilerin Azure [Depolama](https://azure.microsoft.com/documentation/services/storage/)alanınızda dinlenerek şifrelenmesini sağlar.

Windows ve Linux'un desteklenen sürümleri listesi için Azure Disk Şifreleme belgelerinde [Desteklenen VM'ler ve işletim sistemleri](../virtual-machines/windows/disk-encryption-overview.md#supported-vms-and-operating-systems) bölümüne bakın.

## <a name="implement-the-recommendation"></a>Öneriyi uygulayın
1. **Öneriler** sayfasında, sanal makinelerde disk **şifrelemesi yapılmalıdır.**
2. Sağlıksız **kaynaklardan,** Disk Şifreleme'nin önerildiği bir VM seçin.
3. Bu VM'lere şifreleme uygulamak için yönergeleri izleyin.

![Disk şifrelemesi uygulama](./media/security-center-apply-disk-encryption/apply-disk-encryption.png)

Güvenlik Merkezi tarafından şifreleme ihtiyacı olarak tanımlanan Azure Sanal Makinelerini şifrelemek için aşağıdaki adımları öneririz:

* Azure PowerShell'i yükleyip yapılandırın. Bu, Azure Sanal Makineleri şifrelemek için gereken ön koşulları ayarlamak için gereken PowerShell komutlarını çalıştırmanızı sağlar.
* Azure Disk Şifreleme Önkoşulları Azure PowerShell komut dosyasını edinin ve çalıştırın.
* Sanal makinelerinizi şifreleyin.

[Azure PowerShell ile bir Windows IaaS VM'i şifreleyin](../virtual-machines/windows/disk-encryption-powershell-quickstart.md) - Bu adımlar arasında size yol gösterir ve disk şifrelemesini yapılandırabileceğiniz bir Windows istemci makinesi kullandığınızı varsayar.

Azure Sanal Makineler için kullanılabilecek birçok yaklaşım vardır. Azure PowerShell veya Azure CLI konusunda zaten bilgiliyseniz, alternatif yaklaşımlar kullanmayı tercih edebilirsiniz. Bu diğer yaklaşımlar hakkında bilgi edinmek için [Azure disk şifrelemesine](../security/fundamentals/encryption-overview.md)bakın.

## <a name="see-also"></a>Ayrıca bkz.
Bu belge, Güvenlik Merkezi önerisi "Disk şifrelemesi uygulayın" önerisini nasıl uygulayacağınızı gösterdi. Disk şifreleme hakkında daha fazla bilgi edinmek için bkz:

* [Azure Key Vault ile şifreleme ve anahtar yönetimi](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video, 36 dk 39 sn)--Verilerinizi korumaya ve korumaya yardımcı olmak için IaaS VM'ler ve Azure Key Vault için disk şifreleme yönetimini nasıl kullanacağınızı öğrenin.
* [Azure disk şifrelemesi](../security/fundamentals/encryption-overview.md) (belge)--Windows ve Linux VM'leri için disk şifrelemeyi nasıl etkinleştirin öğrenin.