---
title: Windows VM'den Azure AD DS'ye katılmak için şablon kullanma | Microsoft Dokümanlar
description: Azure Etkin Dizin Etki Alanı Hizmetleri yönetilen yeni veya varolan bir Windows Server VM'ye katılmak için Azure Kaynak Yöneticisi şablonlarını nasıl kullanacağınızı öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 4eabfd8e-5509-4acd-86b5-1318147fddb5
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: 19e4033b462cda9511ee45420bd4c1a76cc36b92
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80518950"
---
# <a name="join-a-windows-server-virtual-machine-to-an-azure-active-directory-domain-services-managed-domain-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak Windows Server sanal makinesine Azure Active Directory Etki Alanı Hizmetleri yönetilen bir etki alanına katılma

Azure sanal makinelerinin (VM) dağıtımını ve yapılandırmasını otomatikleştirmek için Kaynak Yöneticisi şablonu kullanabilirsiniz. Bu şablonlar, her seferinde tutarlı dağıtımlar oluşturmanıza izin verir. Uzantılar, dağıtımın bir parçası olarak bir VM'i otomatik olarak yapılandırmak için şablonlara da eklenebilir. Kullanışlı bir uzantı, Azure Active Directory Domain Services (Azure AD DS) yönetilen etki alanlarında kullanılabilen bir etki alanına VM'lere katılır.

Bu makalede, Kaynak Yöneticisi şablonlarını kullanarak Bir Windows Server VM'yi Azure AD DS yönetilen etki alanına nasıl oluşturup katılacağınız gösterilmektedir. Ayrıca, varolan bir Windows Server VM'e Azure AD DS etki alanına nasıl katılacağınızı da öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, ilk öğretici [bir Azure Active Directory Etki Alanı Hizmetleri örneği oluşturur ve yapılandırır.][create-azure-ad-ds-instance]
* Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcı hesabı.

## <a name="azure-resource-manager-template-overview"></a>Azure Kaynak Yöneticisi şablonuna genel bakış

Kaynak Yöneticisi şablonları, Azure altyapınızı kod olarak tanımlamanıza izin verir. Gerekli kaynaklar, ağ bağlantıları veya VM yapılandırması şablonda tanımlanabilir. Bu şablonlar her seferinde tutarlı, tekrarlanabilir dağıtımlar oluşturur ve siz değişiklik yaptığınızda sürümlenebilir. Daha fazla bilgi için Azure [Kaynak Yöneticisi şablonlarına genel bakış][template-overview]bakın.

Her kaynak, JavaScript Nesne Gösterimi (JSON) kullanılarak bir şablonda tanımlanır. Aşağıdaki JSON örneği, Active Directory join uzantısını yüklemek için *Microsoft.Compute/virtualMachines/extensions* kaynak türünü kullanır. Dağıtım zamanında belirttiğiniz parametreler kullanılır. Uzantı dağıtıldığında, VM belirtilen Azure AD DS yönetilen etki alanına katılır.

```json
 {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "[concat(parameters('dnsLabelPrefix'),'/joindomain')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('dnsLabelPrefix'))]"
      ],
      "properties": {
        "publisher": "Microsoft.Compute",
        "type": "JsonADDomainExtension",
        "typeHandlerVersion": "1.3",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "Name": "[parameters('domainToJoin')]",
          "OUPath": "[parameters('ouPath')]",
          "User": "[concat(parameters('domainToJoin'), '\\', parameters('domainUsername'))]",
          "Restart": "true",
          "Options": "[parameters('domainJoinOptions')]"
        },
        "protectedSettings": {
          "Password": "[parameters('domainPassword')]"
        }
      }
    }
```

Bu VM uzantısı, aynı şablonda bir VM oluşturmasanız bile dağıtılabilir. Bu makaledeki örnekler aşağıdaki yaklaşımların her ikisini de gösterir:

* [Windows Server VM oluşturun ve yönetilen bir etki alanına katılın](#create-a-windows-server-vm-and-join-to-a-managed-domain)
* [Yönetilen bir etki alanına varolan bir Windows Server VM'ye katılma](#join-an-existing-windows-server-vm-to-a-managed-domain)

## <a name="create-a-windows-server-vm-and-join-to-a-managed-domain"></a>Windows Server VM oluşturun ve yönetilen bir etki alanına katılın

Windows Server VM'ye ihtiyacınız varsa, Kaynak Yöneticisi şablonu kullanarak bir tane oluşturabilir ve yapılandırabilirsiniz. VM dağıtıldığında, VM'ye Azure AD DS yönetilen etki alanına katılmak için bir uzantı yüklenir. Azure AD DS yönetilen bir etki alanına katılmak istediğiniz bir VM'niz varsa, [varolan bir Windows Server VM'ini yönetilen bir etki alanına katılmak için](#join-an-existing-windows-server-vm-to-a-managed-domain)atlayın.

Bir Windows Server VM oluşturmak ve ardından Azure AD DS yönetilen bir etki alanına katılmak için aşağıdaki adımları tamamlayın:

1. Hızlı başlangıç [şablonuna](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)göz atın. Azure'a **Dağıtma**seçeneğini seçin.
1. Özel **dağıtım** sayfasında, Bir Windows Server VM oluşturmak ve Azure AD DS yönetilen etki alanına katılmak için aşağıdaki bilgileri girin:

    | Ayar                   | Değer |
    |---------------------------|-------|
    | Abonelik              | Azure AD Etki Alanı Hizmetlerini etkinleştirdiğiniz aynı Azure aboneliğini seçin. |
    | Kaynak grubu            | VM'niz için kaynak grubunu seçin. |
    | Konum                  | VM'nizin konumunu seçin. |
    | Varolan VNET Adı        | VM'yi *myVnet*gibi bağlamak için varolan sanal ağın adı. |
    | Varolan Subnet Adı      | *İş Yükleri*gibi varolan sanal ağ alt ağının adı. |
    | DNS Etiket Öneki          | *Myvm*gibi VM için kullanılacak bir DNS adı girin. |
    | VM boyutu                   | *Standard_DS2_v2*gibi bir VM boyutu belirtin. |
    | Katılacak Etki Alanı            | Azure AD DS, *aaddscontoso.com*gibi etki alanı DNS adını yönetti. |
    | Etki Alanı Kullanıcı Adı           | VM'yi yönetilen etki alanına katılmak için kullanılması gereken Azure AD DS yönetilen `contosoadmin@aaddscontoso.com`etki alanındaki kullanıcı hesabı. Bu hesap, Azure AD DS yönetilen etki alanının bir parçası olmalıdır. |
    | Etki Alanı Şifresi           | Önceki ayarda belirtilen kullanıcı hesabının parolası. |
    | İsteğe bağlı OU Yolu          | VM eklemek için özel OU. Bu parametre için bir değer belirtmezseniz, VM varsayılan *AAD DC Bilgisayar* OU eklenir. |
    | VM Yönetici Kullanıcı Adı         | VM'de oluşturmak için yerel bir yönetici hesabı belirtin. |
    | VM Yönetici Şifresi         | VM için yerel yönetici parolası belirtin. Parola kaba kuvvet saldırılarına karşı korumak için güçlü bir yerel yönetici parolası oluşturun. |

1. Hüküm ve koşulları gözden geçirin, daha sonra **yukarıda belirtilen hüküm ve koşulları kabul ettiğim için**kutuyu işaretleyin. Hazır olduğunuzda, VM'yi oluşturmak ve Azure AD DS yönetilen etki alanına katılmak için **Satın Al'ı** seçin.

> [!WARNING]
> **Parolaları dikkatli bir şekilde işleyebilir.**
> Şablon parametre dosyası, Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcı hesabı için parola ister. Bu dosyaya değerleri el ile girmeyin ve dosya paylaşımlarında veya diğer paylaşılan konumlarda erişilebilir bırakmayın.

Dağıtımın başarıyla tamamlanması birkaç dakika sürer. Tamamlandığında, Windows VM oluşturulur ve Azure AD DS yönetilen etki alanına katılır. VM etki alanı hesapları kullanılarak yönetilebilir veya oturum açabilir.

## <a name="join-an-existing-windows-server-vm-to-a-managed-domain"></a>Yönetilen bir etki alanına varolan bir Windows Server VM'ye katılma

Azure AD DS yönetilen bir etki alanına katılmak istediğiniz varolan bir VM veya VM grubunuz varsa, VM uzantısını dağıtmak için kaynak yöneticisi şablonu kullanabilirsiniz.

Varolan bir Windows Server VM'ine Azure AD DS yönetilen bir etki alanına katılmak için aşağıdaki adımları tamamlayın:

1. Hızlı başlangıç [şablonuna](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)göz atın. Azure'a **Dağıtma**seçeneğini seçin.
1. Özel **dağıtım** sayfasında, VM'ye Azure AD DS yönetilen etki alanına katılmak için aşağıdaki bilgileri girin:

    | Ayar                   | Değer |
    |---------------------------|-------|
    | Abonelik              | Azure AD Etki Alanı Hizmetlerini etkinleştirdiğiniz aynı Azure aboneliğini seçin. |
    | Kaynak grubu            | Mevcut VM'niz ile kaynak grubunu seçin. |
    | Konum                  | Mevcut VM'nizin konumunu seçin. |
    | VM listesi                   | *MyVM1,myVM2*gibi Azure AD DS yönetilen etki alanına katılmak için varolan VM(ler)in virgülle ayrılmış listesini girin. |
    | Etki Alanına Katılma Kullanıcı Adı     | VM'yi yönetilen etki alanına katılmak için kullanılması gereken Azure AD DS yönetilen `contosoadmin@aaddscontoso.com`etki alanındaki kullanıcı hesabı. Bu hesap, Azure AD DS yönetilen etki alanının bir parçası olmalıdır. |
    | Etki Alanına Katılma Kullanıcı Şifresi | Önceki ayarda belirtilen kullanıcı hesabının parolası. |
    | İsteğe bağlı OU Yolu          | VM eklemek için özel OU. Bu parametre için bir değer belirtmezseniz, VM varsayılan *AAD DC Bilgisayar* OU eklenir. |

1. Hüküm ve koşulları gözden geçirin, daha sonra **yukarıda belirtilen hüküm ve koşulları kabul ettiğim için**kutuyu işaretleyin. Hazır olduğunuzda, Azure AD DS yönetilen etki alanına VM'ye katılmak için **Satın Al'ı** seçin.

> [!WARNING]
> **Parolaları dikkatli bir şekilde işleyebilir.**
> Şablon parametre dosyası, Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcı hesabı için parola ister. Bu dosyaya değerleri el ile girmeyin ve dosya paylaşımlarında veya diğer paylaşılan konumlarda erişilebilir bırakmayın.

Dağıtımın başarıyla tamamlanması birkaç dakika sürer. Tamamlandığında, belirtilen Windows VM'leri Azure AD DS yönetilen etki alanına katılır ve etki alanı hesapları kullanılarak yönetilebilir veya oturum açılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kaynakları şablonları kullanarak yapılandırmak ve dağıtmak için Azure portalını kullandınız. Azure [PowerShell][deploy-powershell] veya [Azure CLI'yi][deploy-cli]kullanarak Kaynak Yöneticisi şablonlarıyla da kaynak dağıtabilirsiniz.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[template-overview]: ../azure-resource-manager/templates/overview.md
[deploy-powershell]: ../azure-resource-manager/templates/deploy-powershell.md
[deploy-cli]: ../azure-resource-manager/templates/deploy-cli.md
