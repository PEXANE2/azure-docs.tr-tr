---
title: Azure blok zinciri hizmeti Consortium üyelerini yönetme-PowerShell
description: Azure PowerShell kullanarak Azure blok zinciri hizmeti Consortium üyelerini yönetmeyi öğrenin.
ms.date: 10/14/2019
ms.topic: article
ms.reviewer: zeyadr
ms.openlocfilehash: 4eb35838394d3324f460b5e83cfbcb47b8114221
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74455510"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-using-powershell"></a>PowerShell kullanarak Azure blok zinciri hizmeti 'nde konsorsiyum üyelerini yönetme

PowerShell 'i kullanarak Azure blok zinciri hizmetinize yönelik blok zinciri Konsorsiyumu üyelerini yönetebilirsiniz. Yönetici ayrıcalıklarına sahip Üyeler blok zinciri Konsorsiyumu içindeki tüm katılımcılar için rolleri davet edebilir, ekleyebilir, kaldırabilir ve değiştirebilir. Kullanıcı ayrıcalıklarına sahip Üyeler blok zinciri Konsorsiyumu içindeki tüm katılımcıları görüntüleyebilir ve üye görünen adını değiştirebilir.

## <a name="prerequisites"></a>Önkoşullar

* [Azure Portal](create-member.md)kullanarak bir blok zinciri üyesi oluşturun.
* Yarışmada, Üyeler ve düğümler hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmeti Consortium](consortium.md).

## <a name="open-azure-cloud-shell"></a>Azure Cloud Shell’i açma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz, ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Ayrıca, [Shell.Azure.com/PowerShell](https://shell.azure.com/powershell)' a giderek ayrı bir tarayıcı sekmesinde Cloud Shell açabilirsiniz. Kod bloklarını kopyalamak için **Kopyala** ' yı seçin, Cloud Shell yapıştırın ve çalıştırmak için **ENTER** ' u seçin.

## <a name="install-the-powershell-module"></a>PowerShell modülünü yükler

PowerShell Galerisi Microsoft.AzureBlockchainService.ConsortiumManagement.PS paketini yükler.

```powershell-interactive
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="set-the-information-preference"></a>Bilgi tercihini ayarla

Bilgi tercih değişkenini ayarlayarak cmdlet 'leri yürütürken daha fazla bilgi edinebilirsiniz. Varsayılan olarak, *$InformationPreference* *SilentlyContinue*olarak ayarlanır.

Cmdlet 'lerden daha ayrıntılı bilgi için, PowerShell 'de tercihi aşağıdaki gibi ayarlayın:

```powershell-interactive
$InformationPreference = 'Continue'
```

## <a name="establish-a-web3-connection"></a>Web3 bağlantısı oluşturma

Consortium üyelerini yönetmek için, blok zinciri hizmeti üye uç noktanıza bir Web3 bağlantısı kurun. Bu betiği, konsorsiyum yönetimi cmdlet 'lerini çağırmak için genel değişkenleri ayarlamak üzere kullanabilirsiniz.

```powershell-interactive
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

*\<üye hesabı parolasını\>* , üyeyi oluştururken kullandığınız üye hesap parolasıyla değiştirin.

Azure portal diğer değerleri bulun:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Varsayılan blok zinciri hizmeti üyesine **genel bakış** sayfasına gidin.

    ![Üyeye genel bakış](./media/manage-consortium-powershell/member-overview.png)

    *\<üye hesabı\>* ve *\<rootcontract adresi\>* portaldan alınan değerlerle değiştirin.

1. Uç nokta adresi için **işlem düğümleri**' ni seçin ve ardından **varsayılan işlem düğümünü**seçin. Varsayılan düğüm, blok zinciri üyesiyle aynı ada sahiptir.
1. **Bağlantı dizelerini**seçin.

    ![Bağlantı dizeleri](./media/manage-consortium-powershell/connection-strings.png)

    *\<uç nokta adresi\>* **https (erişim anahtarı 1)** veya **https (erişim anahtarı 2)** değeri ile değiştirin.

## <a name="manage-the-network-and-smart-contracts"></a>Ağ ve akıllı sözleşmeleri yönetme

Blok zinciri uç noktasının, konsorsiyumun yönetiminden sorumlu akıllı sözleşmeleri ile bağlantı kurmak için ağ ve akıllı anlaşma cmdlet 'lerini kullanın.

### <a name="import-consortiummanagementcontracts"></a>İçeri aktarma-Consortiummanagementsözleşmeleri

Bu cmdlet 'i, konsorsiyum yönetiminin akıllı sözleşmelerine bağlamak için kullanın. Bu sözleşmeler, konsorsiyumun içindeki üyeleri yönetmek ve zorlamak için kullanılır.

`Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| RootContractAddress | Konsorsiyum yönetimi akıllı sözleşmelerinin kök sözleşme adresi | Evet |
| Web3Client | New-Web3Connection ' den edinilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>Import-Web3Account

Bu cmdlet 'i, uzak düğümün yönetim hesabının bilgilerini tutacak bir nesne oluşturmak için kullanın.

`Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| ManagedAccountAddress | Blok zinciri üyesi hesap adresi | Evet |
| ManagedAccountPassword | Hesap adresi parolası | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>New-Web3Connection

İşlem düğümünün RPC uç noktasıyla bağlantı kurmak için bu cmdlet 'i kullanın.

`New-Web3Connection [-RemoteRPCEndpoint <String>]`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Blok zinciri üye uç nokta adresi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="manage-the-consortium-members"></a>Konsorsiyum üyelerini yönetme

Konsorsiyumun içindeki üyeleri yönetmek için konsorsiyum üye yönetimi cmdlet 'lerini kullanın. Kullanılabilir eylemler, konsorsiyumun rolüne bağlıdır.

### <a name="get-blockchainmember"></a>Get-BlockchainMember

Bu cmdlet 'i kullanarak, konsorsiyumun üye ayrıntılarını veya listesini alın.

`Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Name | Ayrıntılarını almak istediğiniz blok zinciri hizmeti üyesinin adı. Bir ad girildiğinde, üyenin ayrıntılarını döndürür. Bir ad atlandığında, tüm konsorsiyum üyelerinin bir listesini döndürür. | Hayır |
| Üyeleri | Import-ConsortiumManagementContracts öğesinden edinilen Üyeler nesnesi | Evet |
| Web3Client | New-Web3Connection ' den edinilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection değişkenini ayarlamak için [bir Web3 bağlantısı kurun](#establish-a-web3-connection) .

```powershell-interactive
$ContractConnection | Get-BlockchainMember -Name <Member Name>
```

#### <a name="example-output"></a>Örnek çıkış

```
Name           : myblockchainmember
CorrelationId  : 0
DisplayName    : myCompany
SubscriptionId : <Azure subscription ID>
AccountAddress : 0x85b911c9e103d6405573151258d668479e9ebeef
Role           : ADMIN
```

### <a name="remove-blockchainmember"></a>Remove-BlockchainMember

Bir blok zinciri üyesini kaldırmak için bu cmdlet 'i kullanın.

`Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Name | Kaldırılacak üye adı | Evet |
| Üyeleri | Import-ConsortiumManagementContracts öğesinden edinilen Üyeler nesnesi | Evet |
| Web3Account | Import-Web3Account öğesinden elde edilen Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection ' den edinilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection ve $MemberAccount değişkenlerini ayarlamak için [bir Web3 bağlantısı kurun](#establish-a-web3-connection) .

```powershell-interactive
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Bu cmdlet 'i, görünen ad ve konsorsiyum rolü de dahil olmak üzere blok zinciri üye özniteliklerini ayarlamak için kullanın.

Konsorsiyum yöneticileri, tüm Üyeler için **DisplayName** ve **role** ayarlayabilir. Kullanıcı rolüne sahip bir konsorsiyum üyesi yalnızca kendi üyesinin görünen adını değiştirebilir.

```
Set-BlockchainMember -Name <String> [-DisplayName <String>] [-AccountAddress <String>] [-Role <String>]
 -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>
```

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Name | Blok zinciri üyesinin adı | Evet |
| displayName | Yeni görünen ad | Hayır |
| AccountAddress | Hesap adresi | Hayır |
| Üyeleri | Import-ConsortiumManagementContracts öğesinden edinilen Üyeler nesnesi | Evet |
| Web3Account | Import-Web3Account öğesinden elde edilen Web3Account nesnesi | Evet |
| Web3Client |  New-Web3Connection ' den edinilen Web3Client nesnesi| Evet |

#### <a name="example"></a>Örnek

$ContractConnection ve $MemberAccount değişkenlerini ayarlamak için [bir Web3 bağlantısı kurun](#establish-a-web3-connection) .

```powershell-interactive
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="manage-the-consortium-members-invitations"></a>Konsorsiyumun üyelerinin davetlerini yönetin

Konsorsiyum üyelerinin davetlerini yönetmek için konsorsiyum üye davetiyesi yönetimi cmdlet 'lerini kullanın. Kullanılabilir eylemler, konsorsiyumun rolüne bağlıdır.

### <a name="new-blockchainmemberinvitation"></a>New-Blockchainmemberdavetiyesi

Yeni üyeleri konsorsiya davet etmek için bu cmdlet 'i kullanın.

```
New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>
```

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Davet edilecek üyenin Azure abonelik KIMLIĞI | Evet |
| Rol | Konsorsiyum rolü. Değerler yönetıcı veya Kullanıcı olabilir. YÖNETICI, konsorsiyum Yöneticisi rolüdür. Kullanıcı, konsorsiyum üyesi rolüdür. | Evet |
| Üyeleri | Import-ConsortiumManagementContracts öğesinden edinilen Üyeler nesnesi | Evet |
| Web3Account | Import-Web3Account öğesinden elde edilen Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection ' den edinilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection ve $MemberAccount değişkenlerini ayarlamak için [bir Web3 bağlantısı kurun](#establish-a-web3-connection) .

```powershell-interactive
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>Get-Blockchainmemberdavetiyesi

Bu cmdlet 'i bir konsorsiyum üyesinin davetiye durumunu almak veya listelemek için kullanın.

`Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Davet edilecek üyenin Azure abonelik KIMLIĞI. Abonelik KIMLIĞI sağlanmışsa, abonelik KIMLIĞININ davet ayrıntılarını döndürür. Abonelik KIMLIĞI atlanırsa, tüm üye davetlerinin bir listesini döndürür. | Hayır |
| Üyeleri | Import-ConsortiumManagementContracts öğesinden edinilen Üyeler nesnesi | Evet |
| Web3Client | New-Web3Connection ' den edinilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection değişkenini ayarlamak için [bir Web3 bağlantısı kurun](#establish-a-web3-connection) .

```powershell-interactive
$ContractConnection | Get-BlockchainMemberInvitation – SubscriptionId <Azure subscription ID>
```

#### <a name="example-output"></a>Örnek çıkış

```
SubscriptionId                       Role CorrelationId
--------------                       ---- -------------
<Azure subscription ID>              USER             2
```

### <a name="remove-blockchainmemberinvitation"></a>Remove-Blockchainmemberdavetiyesi

Bir konsorsiyum üyesinin davetini iptal etmek için bu cmdlet 'i kullanın.

```
Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>
```

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | İptal edilecek üyenin Azure abonelik KIMLIĞI | Evet |
| Üyeleri | Import-ConsortiumManagementContracts öğesinden edinilen Üyeler nesnesi | Evet |
| Web3Account | Import-Web3Account öğesinden elde edilen Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection ' den edinilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection ve $MemberAccount değişkenlerini ayarlamak için [bir Web3 bağlantısı kurun](#establish-a-web3-connection) .

```powershell-interactive
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-Blockchainmemberdavetiyesi

Mevcut bir davetin **rolünü** ayarlamak için bu cmdlet 'i kullanın. Yalnızca konsorsiyum yöneticileri davetleri değiştirebilir.

```
Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>
```

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Davet edilecek üyenin Azure abonelik KIMLIĞI | Evet |
| Rol | Davet için yeni konsorsiyum rolü. Değerler **Kullanıcı** veya **yönetici**olabilir. | Evet |
| Üyeleri |  Import-ConsortiumManagementContracts öğesinden edinilen Üyeler nesnesi | Evet |
| Web3Account | Import-Web3Account öğesinden elde edilen Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection ' den edinilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection ve $MemberAccount değişkenlerini ayarlamak için [bir Web3 bağlantısı kurun](#establish-a-web3-connection) .

```powershell-interactive
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Sonraki adımlar

Yarışmada, Üyeler ve düğümler hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmeti Consortium](consortium.md)
