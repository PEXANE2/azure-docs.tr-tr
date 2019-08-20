---
title: Windows Server VM 'sini Azure Active Directory Domain Services 'e ekleme | Microsoft Docs
description: Azure Resource Manager şablonları kullanarak bir Windows Server sanal makinesini yönetilen bir etki alanına katın.
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 4eabfd8e-5509-4acd-86b5-1318147fddb5
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: 599d474b7c45274c87878c622149a86bc93af318
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69612280"
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak Windows Server sanal makinesini yönetilen bir etki alanına katma
Bu makalede, Kaynak Yöneticisi şablonları kullanarak bir Windows Server sanal makinesini Azure AD Domain Services yönetilen bir etki alanına nasıl katılabilmeniz gösterilmektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için şunlar gerekir:
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure ad dizini** -şirket içi bir dizinle veya yalnızca bulut diziniyle eşitlenir.
3. Azure AD dizini için **Azure AD Domain Services** etkinleştirilmelidir. Bunu yapmadıysanız, [Başlarken kılavuzunda](tutorial-create-instance.md)özetlenen tüm görevleri izleyin.
4. Yönetilen etki alanının IP adreslerini, sanal ağ için DNS sunucuları olarak yapılandırdığınızdan emin olun. Daha fazla bilgi için bkz [. Azure sanal ağı IÇIN DNS ayarlarını güncelleştirme](tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network)
5. [Parolaları Azure AD Domain Services yönetilen etki alanı ile senkronize](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)etmek için gereken adımları uygulayın.


## <a name="install-and-configure-required-tools"></a>Gerekli araçları yükleyip yapılandırma
Bu belgede özetlenen adımları gerçekleştirmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:
* **Azure PowerShell**: [Yükleyip yapılandırın](https://azure.microsoft.com/documentation/articles/powershell-install-configure/)
* **Azure CLI**: [Yükleyip yapılandırın](https://azure.microsoft.com/documentation/articles/xplat-cli-install/)


## <a name="option-1-provision-a-new-windows-server-vm-and-join-it-to-a-managed-domain"></a>Seçenek 1: Yeni bir Windows Server VM 'si sağlayın ve yönetilen bir etki alanına ekleyin
**hızlı başlangıç şablonu adı**: [201-VM-etki alanına ekleme](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)

Bir Windows Server sanal makinesini dağıtmak ve yönetilen bir etki alanına katmak için aşağıdaki adımları gerçekleştirin:
1. [Hızlı başlangıç şablonuna](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)gidin.
2. **Azure’a dağıt**’a tıklayın.
3. **Özel dağıtım** sayfasında, sanal makineyi sağlamak için gerekli bilgileri sağlayın.
4. Sanal makinenin sağlanacağı **Azure aboneliğini** seçin. Azure AD Domain Services etkinleştirdiğiniz Azure aboneliğini seçin.
5. Var olan bir **kaynak grubunu** seçin veya yeni bir tane oluşturun.
6. Yeni sanal makinenin dağıtılacağı bir **konum** seçin.
7. **Mevcut VNET adında**, Azure AD Domain Services yönetilen etki alanınızı dağıttığınız sanal ağı belirtin.
8. **Mevcut alt ağ adı**' nda, bu sanal makineyi dağıtmak istediğiniz sanal ağ içindeki alt ağı belirtin. Sanal ağda ağ geçidi alt ağını seçmeyin. Ayrıca, yönetilen etki alanının dağıtıldığı ayrılmış alt ağı seçmeyin.
9. **DNS etiketi öneki**' nde, sağlanmakta olan sanal makine için ana bilgisayar adını belirtin. Örneğin, ' contoso-Win '.
10. Sanal makine için uygun **VM boyutunu** seçin.
11. **Katılacak etki alanı**' nda, yönetilen etkı alanının DNS etki alanı adını belirtin.
12. **Etki alanı Kullanıcı adı**' nda, yönetilen etkı alanında VM 'yi yönetilen etki alanına katmak için kullanılması gereken kullanıcı hesap adını belirtin.
13. **Etki alanı parolası**' nda, ' DomainUserName ' parametresi tarafından başvurulan etki alanı kullanıcı hesabının parolasını belirtin.
14. İsteğe bağlı: Bir özel OU için, sanal makinenin ekleneceği bir **OU yolu** belirtebilirsiniz. Bu parametre için bir değer belirtmezseniz, sanal makine yönetilen etki alanındaki varsayılan **AAD DC bilgisayarları** OU 'ya eklenir.
15. **VM Yöneticisi Kullanıcı adı** alanında, sanal makine için bir yerel yönetici hesabı adı belirtin.
16. **VM yönetici parolası** alanında, sanal makine için bir yerel yönetici parolası belirtin. Parola deneme yanılma saldırılarına karşı korunmak için sanal makine için güçlü bir yerel yönetici parolası sağlayın.
17. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**' a tıklayın.
18. Sanal makineyi sağlamak için **satın al** ' a tıklayın.

> [!WARNING]
> **Parolaları dikkatle işleyin.**
> Şablon parametre dosyası, etki alanı hesaplarının parolalarını ve sanal makine için yerel yönetici parolalarını içerir. Dosya paylaşımlarında veya diğer paylaşılan konumlarda bu dosyayı bir konumda bırakmadığınızdan emin olun. Sanal makinelerinizi dağıtmaya işiniz bittiğinde bu dosyayı atmayı öneririz.
>

Dağıtım başarıyla tamamlandıktan sonra, yeni sağlanan Windows sanal makineniz yönetilen etki alanına katılır.


## <a name="option-2-join-an-existing-windows-server-vm-to-a-managed-domain"></a>Seçenek 2: Mevcut bir Windows Server sanal makinesini yönetilen bir etki alanına katma
**hızlı başlangıç şablonu**: [201-VM-etki alanına ekleme-var](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)

Mevcut bir Windows Server sanal makinesini yönetilen bir etki alanına katmak için aşağıdaki adımları gerçekleştirin:
1. [Hızlı başlangıç şablonuna](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)gidin.
2. **Azure’a dağıt**’a tıklayın.
3. **Özel dağıtım** sayfasında, sanal makineyi sağlamak için gerekli bilgileri sağlayın.
4. Sanal makinenin sağlanacağı **Azure aboneliğini** seçin. Azure AD Domain Services etkinleştirdiğiniz Azure aboneliğini seçin.
5. Var olan bir **kaynak grubunu** seçin veya yeni bir tane oluşturun.
6. Yeni sanal makinenin dağıtılacağı bir **konum** seçin.
7. **VM listesi** alanında, yönetilen etki alanına katılacak var olan sanal makinelerin adlarını belirtin. Tek bir VM adını ayırmak için virgül kullanın. Örneğin, **contoso-Web, contoso-API**.
8. **Etki alanına katılması Kullanıcı adı**' nda, yönetilen etkı alanında VM 'yi yönetilen etki alanına katmak için kullanılması gereken kullanıcı hesap adını belirtin.
9. **Etki alanına katılması Kullanıcı parolası**' nda, ' DomainUserName ' parametresi tarafından başvurulan etki alanı kullanıcı hesabının parolasını belirtin.
10. **Etki alanı FQDN 'si**' nde, yönetilen etkı alanının DNS etki alanı adını belirtin.
11. İsteğe bağlı: Bir özel OU için, sanal makinenin ekleneceği bir **OU yolu** belirtebilirsiniz. Bu parametre için bir değer belirtmezseniz, sanal makine yönetilen etki alanındaki varsayılan **AAD DC bilgisayarları** OU 'ya eklenir.
12. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**' a tıklayın.
13. Sanal makineyi sağlamak için **satın al** ' a tıklayın.

> [!WARNING]
> **Parolaları dikkatle işleyin.**
> Şablon parametre dosyası, etki alanı hesaplarının parolalarını ve sanal makine için yerel yönetici parolalarını içerir. Dosya paylaşımlarında veya diğer paylaşılan konumlarda bu dosyayı bir konumda bırakmadığınızdan emin olun. Sanal makinelerinizi dağıtmaya işiniz bittiğinde bu dosyayı atmayı öneririz.
>

Dağıtım başarıyla tamamlandıktan sonra, belirtilen Windows sanal makineleri yönetilen etki alanına katılır.


## <a name="related-content"></a>İlgili İçerik
* [Azure PowerShell’e genel bakış](/powershell/azure/overview)
* [Azure hızlı başlangıç şablonu-etki alanına yeni bir VM 'ye katılarak](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)
* [Azure hızlı başlangıç şablonu-etki alanına mevcut VM 'Leri ekleme](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
