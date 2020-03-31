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
ms.openlocfilehash: d0f96fe758966a435f8fb8e448e75cbb18b85122
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604516"
---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde disk şifreleme uygulayın
Azure Disk Şifrelemesi kullanılarak şifrelenmemiş Windows veya Linux VM diskleriniz varsa Azure Güvenlik Merkezi disk şifrelemesi uygulamanızı önerir. Disk Şifrelemesi Windows ve Linux IaaS VM disklerinizi şifrelemenizi sağlar.  Şifreleme hem işletim sistemi hem de VM’nizin üzerindeki veri birimleri için önerilir.

Disk Şifreleme, Windows'un endüstri standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) özelliğini ve Linux'un [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) özelliğini kullanır. Bu özellikler, verilerinizin korunmasına ve korunmasına ve kuruluş güvenliği ve uyumluluk taahhütlerinizi karşılamaya yardımcı olmak için işletim sistemi ve veri şifrelemesi sağlar. Disk Şifreleme, Key Vault aboneliğinizdeki disk şifreleme anahtarlarını ve sırlarını kontrol etmenize ve yönetmenize yardımcı olmak için [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) ile entegre olurken, VM diskleri'ndeki tüm verilerin Azure [Depolama](https://azure.microsoft.com/documentation/services/storage/)alanınızda dinlenerek şifrelenmesini sağlar.

> [!NOTE]
> Azure Disk Şifrelemesi aşağıdaki Windows sunucu işletim sistemlerinde desteklenir - Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2. Disk şifreleme aşağıdaki Linux sunucu işletim sistemlerinde desteklenir - Ubuntu, CentOS, SUSE ve SUSE Linux Enterprise Server (SLES).
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulayın
1. **Öneriler** bıçağında **disk şifrelemeuygula'yı**seçin.
2. Disk **şifreleme uygula** bıçağında, Disk Şifreleme'nin önerildiği bir VM listesi görürsünüz.
3. Bu VM'lere şifreleme uygulamak için yönergeleri izleyin.

![][1]

Güvenlik Merkezi tarafından şifreleme ihtiyacı olarak tanımlanan Azure Sanal Makinelerini şifrelemek için aşağıdaki adımları öneririz:

* Azure PowerShell'i yükleyip yapılandırın. Bu, Azure Sanal Makineleri şifrelemek için gereken ön koşulları ayarlamak için gereken PowerShell komutlarını çalıştırmanızı sağlar.
* Azure Disk Şifreleme Önkoşulları Azure PowerShell komut dosyasını edinin ve çalıştırın.
* Sanal makinelerinizi şifreleyin.

[Azure PowerShell ile bir Windows IaaS VM'yi şifrelemek](../virtual-machines/windows/disk-encryption-powershell-quickstart.md) size şu adımları iletebilir. Bu konu, disk şifrelemesini yapılandırdığınız bir Windows istemci simakinesi kullandığınızı varsayar.

Azure Sanal Makineler için kullanılabilecek birçok yaklaşım vardır. Azure PowerShell veya Azure CLI konusunda zaten bilgiliyseniz alternatif yaklaşımlar kullanmayı tercih edebilirsiniz. Bu diğer yaklaşımlar hakkında bilgi edinmek için [Azure disk şifrelemesine](../security/fundamentals/encryption-overview.md)bakın.

## <a name="see-also"></a>Ayrıca bkz.
Bu belge, Güvenlik Merkezi önerisi "Disk şifrelemesi uygulayın" önerisini nasıl uygulayacağınızı gösterdi. Disk şifreleme hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Azure Key Vault ile şifreleme ve anahtar yönetimi](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video, 36 dk 39 sn) -- Verilerinizi korumaya ve korumaya yardımcı olmak için IaaS VM'ler ve Azure Key Vault için disk şifreleme yönetimini nasıl kullanacağınızı öğrenin.
* [Azure disk şifreleme (belge)](../security/fundamentals/encryption-overview.md) -- Windows ve Linux VM'leri için disk şifrelemeyi nasıl etkinleştirin öğrenin.

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkeleri belirleme](tutorial-security-policy.md) -- Güvenlik ilkelerini nasıl yapılandırıştırılamayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumu izleme](security-center-monitoring.md) -- Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını yönetme ve yanıtla](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını nasıl yönetip yanıtlaştak yapılacağını öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -- Önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) -- Azure güvenliği ve uyumluluğu yla ilgili blog gönderilerini bulun.

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
