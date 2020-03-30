---
title: Şablon kullanarak Azure DS Etki Alanı Hizmetlerini etkinleştirme | Microsoft Dokümanlar
description: Azure Kaynak Yöneticisi şablonu kullanarak Azure Etkin Dizin Etki Alanı Hizmetlerini nasıl yapılandırabileceğinizi ve etkinleştirmeyi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: iainfou
ms.openlocfilehash: 2daadb539bc08df37f15c187866b735e45309288
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77612782"
---
# <a name="create-an-azure-active-directory-domain-services-managed-domain-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak Azure Etkin Dizin Etki Alanı Hizmetleri yönetilen bir etki alanı oluşturma

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS), Windows Server Active Directory ile tam uyumlu etki alanı birleştirme, grup ilkesi, LDAP, Kerberos/NTLM kimlik doğrulaması gibi yönetilen etki alanı hizmetleri sağlar. Etki alanı denetleyicilerini dağıtmadan, yönetmeden ve düzeltmeden bu etki alanı hizmetlerini tüketirsiniz. Azure AD DS, mevcut Azure AD kiracınızla tümleşir. Bu tümleştirme, kullanıcıların şirket kimlik bilgilerini kullanarak oturum açmalarına olanak tanır ve kaynaklara erişimi güvence altına almak için varolan grupları ve kullanıcı hesaplarını kullanabilirsiniz.

Bu makalede, Azure Kaynak Yöneticisi şablonu kullanarak Azure AD DS'yi nasıl etkinleştirdiğinizi gösterilmektedir. Destek kaynakları Azure PowerShell kullanılarak oluşturulur.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ihtiyacınız var:

* Azure PowerShell'i yükleyip yapılandırın.
    * Gerekirse, [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlanmak için](/powershell/azure/install-az-ps)yönergeleri izleyin.
    * [Connect-AzAccount][Connect-AzAccount] cmdlet'i kullanarak Azure aboneliğinizde oturum açtığınızdan emin olun.
* Azure AD PowerShell'i yükleyin ve yapılandırın.
    * Gerekirse, Azure AD [PowerShell modülünü yüklemek ve Azure AD'ye bağlanmak için](/powershell/azure/active-directory/install-adv2)yönergeleri izleyin.
    * [Connect-AzureAD][Connect-AzureAD] cmdlet'ini kullanarak Azure AD kiracınızda oturum açtığınızdan emin olun.
* Azure AD DS'yi etkinleştirmek için Azure AD kiracınızda *genel yönetici* ayrıcalıklarına ihtiyacınız vardır.
* Gerekli Azure AD DS kaynaklarını oluşturmak için Azure aboneliğinizde *Katılımcı* ayrıcalıklarına ihtiyacınız vardır.

## <a name="dns-naming-requirements"></a>DNS adlandırma gereksinimleri

Bir Azure AD DS örneği oluşturduğunuzda, bir DNS adı belirtirsiniz. Bu DNS adını seçerken bazı hususlar vardır:

* **Yerleşik etki alanı adı:** Varsayılan olarak, dizinin yerleşik etki alanı adı kullanılır *(.onmicrosoft.com* soneki). Yönetilen etki alanına Internet üzerinden güvenli LDAP erişimi sağlamak istiyorsanız, bu varsayılan etki alanıyla bağlantıyı güvence altına almak için dijital bir sertifika oluşturamazsınız. Microsoft *.onmicrosoft.com* etki alanının sahibidir, bu nedenle Sertifika Yetkilisi (CA) sertifika vermez.
* **Özel alan adları:** En yaygın yaklaşım, genellikle zaten sahip olduğunuz ve routable olan özel bir etki alanı adı belirtmektir. Bir routable, özel etki alanı kullandığınızda, trafik doğru uygulamalarınızı desteklemek için gerektiği gibi akabilir.
* **Routable olmayan etki alanı sonekleri:** Genellikle *contoso.local*gibi routable olmayan bir etki alanı adı soneki nden kaçınmanızı öneririz. *.yerel* sonek çözülebilir değildir ve DNS çözümüyle ilgili sorunlara neden olabilir.

> [!TIP]
> Özel bir etki alanı adı oluşturursanız, varolan DNS ad alanlarına dikkat edin. Varolan herhangi bir Azure veya şirket içi DNS ad alanından ayrı bir etki alanı adı kullanılması önerilir.
>
> Örneğin, *contoso.com*varolan bir DNS ad alanınız varsa, *aaddscontoso.com*özel etki alanı adı içeren bir Azure AD DS yönetilen etki alanı oluşturun. Güvenli LDAP kullanmanız gerekiyorsa, gerekli sertifikaları oluşturmak için bu özel alan adını kaydetmeniz ve sahip olmanız gerekir.
>
> Ortamınızdaki diğer hizmetler için bazı ek DNS kayıtları veya ortamınızdaki varolan DNS ad alanları arasında koşullu DNS iletmeleri oluşturmanız gerekebilir. Örneğin, kök DNS adını kullanarak bir siteyi barındıran bir web sunucusu çalıştırıyorsanız, ek DNS girişleri gerektiren ad çakışmaları olabilir.
>
> Bu öğreticilerde ve nasıl yapılır makalelerinde, *özel aaddscontoso.com* etki alanı kısa bir örnek olarak kullanılır. Tüm komutlarda kendi etki alanı adınızı belirtin.

Aşağıdaki DNS ad kısıtlamaları da geçerlidir:

* **Etki alanı öneki kısıtlamaları:** Önek 15 karakterden daha uzun olan yönetilen bir etki alanı oluşturamazsınız. Belirtilen etki alanı adınızın öneki *(aaddscontoso.com* etki alanı adındaki *aaddscontoso* gibi) 15 veya daha az karakter içermelidir.
* **Ağ adı çakışmaları:** Yönetilen etki alanınızın DNS etki alanı adı sanal ağda zaten bulunmamalıdır. Özellikle, bir ad çakışması neden olacak aşağıdaki senaryoları denetleyin:
    * Azure sanal ağında aynı DNS etki alanı adı olan bir Active Directory etki alanınız zaten varsa.
    * Yönetilen etki alanını etkinleştirmeyi planladığınız sanal ağın şirket içi ağınızla VPN bağlantısı varsa. Bu senaryoda, şirket içi ağınızda aynı DNS etki alanı adı olan bir etki alanınız olmadığından emin olun.
    * Azure sanal ağında bu ada sahip mevcut bir Azure bulut hizmetiniz varsa.

## <a name="create-required-azure-ad-resources"></a>Gerekli Azure AD kaynaklarını oluşturma

Azure AD DS bir hizmet sorumlusu ve bir Azure REKLAM grubu gerektirir. Bu kaynaklar, Azure AD DS yönetilen etki alanının verileri eşitlemasına ve yönetilen etki alanında hangi kullanıcıların yönetim izinlerine sahip olduğunu tanımlamasına izin verir.

İlk olarak, [Register-AzResourceProvider][Register-AzResourceProvider] cmdlet'ini kullanarak Azure AD Etki Alanı Hizmetleri kaynak sağlayıcısına kaydolun:

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.AAD
```

İletişim kurmak ve kimliğini doğrulamak için Azure AD DS için [Yeni AzureADServicePrincipal][New-AzureADServicePrincipal] cmdlet'ini kullanarak bir Azure AD hizmet ilkesi oluşturun. Özel bir uygulama kimliği *2565bd9d-da50-47d4-8b85-4c97f669dc36*kimliği ile *Etki Alanı Denetleyici Hizmetleri* olarak adlandırılır. Bu uygulama kimliğini değiştirmeyin.

```powershell
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
```

Şimdi [Yeni AzureADGroup][New-AzureADGroup] cmdlet'ini kullanarak *AAD DC Yöneticileri* adında bir Azure AD grubu oluşturun. Bu gruba eklenen kullanıcılara Azure AD DS yönetilen etki alanında yönetim görevlerini gerçekleştirme izni verilir.

```powershell
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"
```

Oluşturulan *AAD DC Yöneticileri* grubuyla, [Ekle-AzureADGroupMember][Add-AzureADGroupMember] cmdlet'ini kullanarak gruba bir kullanıcı ekleyin. *Önce AAD DC Yöneticileri* grup nesne kimliğini Get-AzureADGroup cmdlet'ini, ardından [Get-AzureADUser][Get-AzureADGroup] cmdlet'ini kullanarak istediğiniz kullanıcının nesne kimliğini alırsınız. [Get-AzureADUser][Get-AzureADUser]

Aşağıdaki örnekte, bir UPN ile hesabın kullanıcı `admin@aaddscontoso.onmicrosoft.com`nesne kimliği . Bu kullanıcı hesabını *AAD DC Yöneticileri* grubuna eklemek istediğiniz kullanıcının UPN'iyle değiştirin:

```powershell
# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq 'admin@aaddscontoso.onmicrosoft.com'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId
```

Son olarak, [Yeni-AzResourceGroup][New-AzResourceGroup] cmdlet kullanarak bir kaynak grubu oluşturun. Aşağıdaki örnekte, kaynak grubu *myResourceGroup* olarak adlandırılır ve *westus* bölgesinde oluşturulur. Kendi adınızı ve istediğiniz bölgeyi kullanın:

```powershell
New-AzResourceGroup `
  -Name "myResourceGroup" `
  -Location "WestUS"
```

Kullanılabilirlik Bölgelerini destekleyen bir bölge seçerseniz, Azure AD DS kaynakları ek fazlalık için bölgeler e göre dağıtılır. Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Esnekliği sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge vardır.

Azure AD DS'nin bölgelere dağıtılması için yapılandırabileceğiniz hiçbir şey yoktur. Azure platformu, kaynakların bölge dağıtımını otomatik olarak işler. Daha fazla bilgi ve bölgenin kullanılabilirliğini görmek için [Azure'daki Kullanılabilirlik Bölgeleri nelerdir?][availability-zones]

## <a name="resource-definition-for-azure-ad-ds"></a>Azure AD DS için kaynak tanımı

Kaynak Yöneticisi kaynak tanımının bir parçası olarak, aşağıdaki yapılandırma parametreleri gereklidir:

| Parametre               | Değer |
|-------------------------|---------|
| Etkialanıadı              | Önekleri ve çakışmaları adlandırma önceki noktaları dikkate alarak yönetilen etki alanı için DNS etki alanı adı. |
| filtrelenmişSync            | Azure AD DS, Azure AD'de kullanılabilen *tüm* kullanıcıları ve grupları veya yalnızca belirli grupların *kapsamlı* eşitlemesini eşitlemenize olanak tanır. Tüm kullanıcıları ve grupları eşitlemeyi seçerseniz, daha sonra yalnızca kapsamlı bir eşitleme gerçekleştirmeyi seçemezsiniz.<br /> Kapsamlı eşitleme hakkında daha fazla bilgi için [Azure AD Etki Alanı Hizmetleri kapsamlı eşitleme][scoped-sync]bilgisine bakın.|
| bildirimAyarlar    | Azure AD DS yönetilen etki alanında oluşturulan herhangi bir uyarı varsa, e-posta bildirimleri gönderilebilir. <br />Bu bildirimler için Azure kiracısının *genel yöneticileri* ve *AAD DC Yöneticileri* grubunun üyeleri *etkinleştirilebilir.*<br /> İstenirse, dikkat gerektiren uyarılar olduğunda bildirimler için ek alıcılar ekleyebilirsiniz.|
| domainConfigurationType | Varsayılan olarak, *Kullanıcı* ormanı olarak Azure AD DS yönetilen bir etki alanı oluşturulur. Bu tür orman, şirket içi AD DS ortamında oluşturulan kullanıcı hesapları da dahil olmak üzere Azure AD'deki tüm nesneleri eşitler. Bir kullanıcı ormanı oluşturmak için bir *etki alanı Yapılandırma* değeri belirtmeniz gerekmez.<br /> *Kaynak* ormanı yalnızca doğrudan Azure AD'de oluşturulan kullanıcıları ve grupları eşitler. Kaynak ormanları şu anda önizlemede. Kaynak ormanı oluşturmak için değeri *ResourceTrusting* olarak ayarlayın.<br />*Kaynak* ormanları hakkında daha fazla bilgi için, neden kullanabileceğiniz ve şirket içi AD DS etki alanlarında orman güvenlerinin nasıl oluşturulabileceği de dahil olmak üzere, [Azure AD DS kaynak ormanlarına genel bakış][resource-forests]alabilirsiniz.|

Aşağıdaki yoğunlaştırılmış parametreler tanımı, bu değerlerin nasıl beyan edildiğini gösterir. *Azure* AD'deki tüm kullanıcılar Azure AD DS yönetilen etki alanına senkronize edilmiş aaddscontoso.com adlı bir kullanıcı ormanı oluşturulur:

```json
"parameters": {
    "domainName": {
        "value": "aaddscontoso.com"
    },
    "filteredSync": {
        "value": "Disabled"
    },
    "notificationSettings": {
        "value": {
            "notifyGlobalAdmins": "Enabled",
            "notifyDcAdmins": "Enabled",
            "additionalRecipients": []
        }
    },
    [...]
}
```

Ardından, Azure AD DS yönetilen etki alanını tanımlamak ve oluşturmak için aşağıdaki yoğunlaştırılmış Kaynak Yöneticisi şablonu kaynak türü kullanılır. Bir Azure sanal ağı ve alt ağı zaten mevcut olmalı veya Kaynak Yöneticisi şablonunun bir parçası olarak oluşturulmalıdır. Azure AD DS yönetilen etki alanı bu alt ağa bağlıdır.

```json
"resources": [
    {
        "apiVersion": "2017-06-01",
        "type": "Microsoft.AAD/DomainServices",
        "name": "[parameters('domainName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
            "domainName": "[parameters('domainName')]",
            "subnetId": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', parameters('vnetName'), '/subnets/', parameters('subnetName'))]",
            "filteredSync": "[parameters('filteredSync')]",
            "notificationSettings": "[parameters('notificationSettings')]"
        }
    },
    [...]
]
```

Bu parametreler ve kaynak türü, aşağıdaki bölümde gösterildiği gibi yönetilen bir etki alanını dağıtmak için daha geniş bir Kaynak Yöneticisi şablonunun parçası olarak kullanılabilir.

## <a name="create-a-managed-domain-using-sample-template"></a>Örnek şablonu kullanarak yönetilen bir etki alanı oluşturma

Aşağıdaki tam Kaynak Yöneticisi örnek şablonu, Azure AD DS yönetilen etki alanı ve desteklenen sanal ağ, alt ağ ve ağ güvenlik grubu kuralları oluşturur. Yönetilen etki alanını güvenli hale getirmek ve trafiğin doğru akmasını sağlamak için ağ güvenlik grubu kuralları gereklidir. *tüm* kullanıcılar Azure AD'den senkronize aaddscontoso.com DNS adını içeren bir kullanıcı ormanı oluşturulur:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "apiVersion": {
            "value": "2017-06-01"
        },
        "domainConfigurationType": {
            "value": "FullySynced"
        },
        "domainName": {
            "value": "aaddscontoso.com"
        },
        "filteredSync": {
            "value": "Disabled"
        },
        "location": {
            "value": "westus"
        },
        "notificationSettings": {
            "value": {
                "notifyGlobalAdmins": "Enabled",
                "notifyDcAdmins": "Enabled",
                "additionalRecipients": []
            }
        },
        "subnetName": {
            "value": "aadds-subnet"
        },
        "vnetName": {
            "value": "aadds-vnet"
        },
        "vnetAddressPrefixes": {
            "value": [
                "10.1.0.0/24"
            ]
        },
        "subnetAddressPrefix": {
            "value": "10.1.0.0/24"
        },
        "nsgName": {
            "value": "aadds-nsg"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-06-01",
            "type": "Microsoft.AAD/DomainServices",
            "name": "[parameters('domainName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "domainName": "[parameters('domainName')]",
                "subnetId": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', parameters('vnetName'), '/subnets/', parameters('subnetName'))]",
                "filteredSync": "[parameters('filteredSync')]",
                "domainConfigurationType": "[parameters('domainConfigurationType')]",
                "notificationSettings": "[parameters('notificationSettings')]"
            }
        },
        {
            "type": "Microsoft.Network/NetworkSecurityGroups",
            "name": "[parameters('nsgName')]",
            "location": "[parameters('location')]",
            "properties": {
                "securityRules": [
                    {
                        "name": "AllowSyncWithAzureAD",
                        "properties": {
                            "access": "Allow",
                            "priority": 101,
                            "direction": "Inbound",
                            "protocol": "Tcp",
                            "sourceAddressPrefix": "AzureActiveDirectoryDomainServices",
                            "sourcePortRange": "*",
                            "destinationAddressPrefix": "*",
                            "destinationPortRange": "443"
                        }
                    },
                    {
                        "name": "AllowPSRemoting",
                        "properties": {
                            "access": "Allow",
                            "priority": 301,
                            "direction": "Inbound",
                            "protocol": "Tcp",
                            "sourceAddressPrefix": "AzureActiveDirectoryDomainServices",
                            "sourcePortRange": "*",
                            "destinationAddressPrefix": "*",
                            "destinationPortRange": "5986"
                        }
                    },
                    {
                        "name": "AllowRD",
                        "properties": {
                            "access": "Allow",
                            "priority": 201,
                            "direction": "Inbound",
                            "protocol": "Tcp",
                            "sourceAddressPrefix": "CorpNetSaw",
                            "sourcePortRange": "*",
                            "destinationAddressPrefix": "*",
                            "destinationPortRange": "3389"
                        }
                    }
                ]
            },
            "apiVersion": "2018-04-01"
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "apiVersion": "2018-04-01",
            "dependsOn": [
                "[concat('Microsoft.Network/NetworkSecurityGroups/', parameters('nsgName'))]"
            ],
            "properties": {
                "addressSpace": {
                    "addressPrefixes": "[parameters('vnetAddressPrefixes')]"
                },
                "subnets": [
                    {
                        "name": "[parameters('subnetName')]",
                        "properties": {
                            "addressPrefix": "[parameters('subnetAddressPrefix')]",
                            "networkSecurityGroup": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/NetworkSecurityGroups/', parameters('nsgName'))]"
                            }
                        }
                    }
                ]
            }
        }
    ],
    "outputs": {}
}
```

Bu şablon, [Azure portalı, Azure][portal-deploy] [PowerShell][powershell-deploy]veya CI/CD ardışık işlem yöntemi gibi tercih ettiğiniz dağıtım yöntemini kullanarak dağıtılabilir. Aşağıdaki örnekte [Yeni Kaynak GrubuDağıtım][New-AzResourceGroupDeployment] cmdlet kullanır. Kendi kaynak grubu adınızı ve şablon dosya adınızı belirtin:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myResourceGroup" -TemplateFile <path-to-template>
```

Kaynağı oluşturmak ve denetimi PowerShell istemine döndürmek birkaç dakika sürer. Azure AD DS yönetilen etki alanı arka planda sağlanmaya devam eder ve dağıtımı tamamlamak bir saat kadar sürebilir. Azure portalında, Azure AD DS yönetilen etki alanınız için **Genel Bakış** sayfası, dağıtım aşaması boyunca geçerli durumu gösterir.

Azure portalı Azure AD DS yönetilen etki alanının sağlanmasını tamamladığını gösterdiğinde, aşağıdaki görevlerin tamamlanması gerekir:

* Sanal makinelerin etki alanı birleştirme veya kimlik doğrulama için yönetilen etki alanını bulabilmesi için sanal ağ için DNS ayarlarını güncelleştirin.
    * DNS'yi yapılandırmak için portaldaki Azure AD DS yönetilen etki alanınızı seçin. Genel **Bakış** penceresinde, bu DNS ayarlarını otomatik olarak yapılandırmanız istenir.
* Son kullanıcıların kurumsal kimlik bilgilerini kullanarak yönetilen etki alanında oturum açabilmesi [için Azure AD Etki Alanı Hizmetlerine parola eşitlemesini etkinleştirin.](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS yönetilen etki alanını iş başında görmek için, [bir Windows VM'ye katılabilir,][windows-join] [güvenli LDAP'yi yapılandırabilir][tutorial-ldaps]ve [parola karma eşitlemeyi yapılandırabilirsiniz.][tutorial-phs]

<!-- INTERNAL LINKS -->
[windows-join]: join-windows-vm.md
[tutorial-ldaps]: tutorial-configure-ldaps.md
[tutorial-phs]: tutorial-configure-password-hash-sync.md
[availability-zones]: ../availability-zones/az-overview.md
[portal-deploy]: ../azure-resource-manager/templates/deploy-portal.md
[powershell-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[scoped-sync]: scoped-synchronization.md
[resource-forests]: concepts-resource-forest.md

<!-- EXTERNAL LINKS -->
[Connect-AzAccount]: /powershell/module/Az.Accounts/Connect-AzAccount
[Connect-AzureAD]: /powershell/module/AzureAD/Connect-AzureAD
[New-AzureADServicePrincipal]: /powershell/module/AzureAD/New-AzureADServicePrincipal
[New-AzureADGroup]: /powershell/module/AzureAD/New-AzureADGroup
[Add-AzureADGroupMember]: /powershell/module/AzureAD/Add-AzureADGroupMember
[Get-AzureADGroup]: /powershell/module/AzureAD/Get-AzureADGroup
[Get-AzureADUser]: /powershell/module/AzureAD/Get-AzureADUser
[Register-AzResourceProvider]: /powershell/module/Az.Resources/Register-AzResourceProvider
[New-AzResourceGroup]: /powershell/module/Az.Resources/New-AzResourceGroup
[Get-AzSubscription]: /powershell/module/Az.Accounts/Get-AzSubscription
[cloud-shell]: /azure/cloud-shell/cloud-shell-windows-users
[naming-prefix]: /windows-server/identity/ad-ds/plan/selecting-the-forest-root-domain
[New-AzResourceGroupDeployment]: /powershell/module/Az.Resources/New-AzResourceGroupDeployment
