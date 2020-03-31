---
title: Azure Blockchain Service konsorsiyum üyelerini yönetin - PowerShell
description: Azure PowerShell'i kullanarak Azure Blockchain Service konsorsiyum üyelerini nasıl yöneteceklerini öğrenin.
ms.date: 10/14/2019
ms.topic: article
ms.reviewer: zeyadr
ms.openlocfilehash: e819dd39481b58d446384a5e2253c548ce0c267c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77505987"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-using-powershell"></a>PowerShell'i kullanarak Azure Blockchain Hizmeti'ndeki konsorsiyum üyelerini yönetme

Azure Blockchain Hizmetiniz için blockchain konsorsiyum üyelerini yönetmek için PowerShell'i kullanabilirsiniz. Yönetici ayrıcalıklarına sahip üyeler blockchain konsorsiyumundaki tüm katılımcılar için rolleri davet edebilir, ekleyebilir, kaldırabilir ve değiştirebilir. Kullanıcı ayrıcalıklarına sahip üyeler blockchain konsorsiyumundaki tüm katılımcıları görüntüleyebilir ve üye ekran adlarını değiştirebilirler.

## <a name="prerequisites"></a>Ön koşullar

* [Azure portalını](create-member.md)kullanarak bir blockchain üyesi oluşturun.
* Konsorsiyumlar, üyeler ve düğümler hakkında daha fazla bilgi için [Azure Blockchain Service konsorsiyumuna](consortium.md)bakın.

## <a name="open-azure-cloud-shell"></a>Azure Cloud Shell’i açma

Azure Bulut Kabuğu, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Ayrıca [shell.azure.com/powershell'a](https://shell.azure.com/powershell)giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de açabilirsiniz. Kod **Copy** bloklarını kopyalamak için Kopyala'yı seçin, Bulut Kabuğu'na yapıştırın ve çalıştırmak için **Enter'u** seçin.

## <a name="install-the-powershell-module"></a>PowerShell modüllerini yükleyin

Microsoft.AzureBlockchainService.ConsortiumManagement.PS paketini PowerShell Galerisi'nden yükleyin.

```powershell-interactive
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="set-the-information-preference"></a>Bilgi tercihini ayarlama

Bilgi tercihi değişkenini ayarlayarak cmdlets'i çalıştırırken daha fazla bilgi alabilirsiniz. Varsayılan olarak, *$InformationPreference* *SilentlyContinue*olarak ayarlanır.

Cmdlets daha ayrıntılı bilgi için, Aşağıdaki gibi PowerShell tercihayarlayın:

```powershell-interactive
$InformationPreference = 'Continue'
```

## <a name="establish-a-web3-connection"></a>Web3 bağlantısı kurma

Konsorsiyum üyelerini yönetmek için Blockchain Service üye bitiş noktanıza bir Web3 bağlantısı kurun. Konsorsiyum yönetimi cmdlets aramak için genel değişkenler ayarlamak için bu komut dosyası kullanabilirsiniz.

```powershell-interactive
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

* \<Üye hesap\> parolasını,* üyeyi oluşturduğunuzda kullandığınız üye hesabı parolasıyla değiştirin.

Azure portalındaki diğer değerleri bulun:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Varsayılan Blockchain Hizmeti üye **genel bakış** sayfanıza gidin.

    ![Üyeye genel bakış](./media/manage-consortium-powershell/member-overview.png)

    * \<Üye hesabını\> * ve * \<RootContract adresini\> * portaldaki değerlerle değiştirin.

1. Bitiş noktası adresi için **Hareket düğümlerini**seçin ve sonra **varsayılan hareket düğümünü**seçin. Varsayılan düğüm blockchain üyesiyle aynı ada sahiptir.
1. **Bağlantı dizeleri'ni**seçin.

    ![Bağlantı dizeleri](./media/manage-consortium-powershell/connection-strings.png)

    * \<Uç nokta\> adresini* HTTPS **(Access tuşu 1)** veya **HTTPS (Erişim tuşu 2)** değeriyle değiştirin.

## <a name="manage-the-network-and-smart-contracts"></a>Ağı ve akıllı sözleşmeleri yönetme

Blockchain endpoint'in konsorsiyum yönetiminden sorumlu akıllı sözleşmelerine bağlantı kurmak için ağı ve akıllı sözleşme cmdletlerini kullanın.

### <a name="import-consortiummanagementcontracts"></a>İthalat-KonsorsiyumYönetim Sözleşmeleri

Konsorsiyum yönetiminin akıllı sözleşmelerine bağlanmak için bu cmdlet'i kullanın. Bu sözleşmeler, konsorsiyum içindeki üyeleri yönetmek ve uygulamak için kullanılır.

`Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| RootContractAddress | Konsorsiyum yönetimi akıllı sözleşmelerin kök sözleşme adresi | Evet |
| Web3İstesi | Yeni-Web3Connection'dan elde edilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>İthalat-Web3Account

Uzak düğüm yönetim hesabı nın bilgilerini tutmak için bir nesne oluşturmak için bu cmdlet'i kullanın.

`Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Yönetilen Hesap Adresi | Blockchain üye hesap adresi | Evet |
| Yönetilen Hesap Şifresi | Hesap adresi şifresi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>Yeni-Web3Bağlantı

İşlem düğümünün RPC bitiş noktasına bağlantı kurmak için bu cmdlet'i kullanın.

`New-Web3Connection [-RemoteRPCEndpoint <String>]`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Blockchain üye uç noktası adresi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="manage-the-consortium-members"></a>Konsorsiyum üyelerini yönetme

Konsorsiyum içindeki üyeleri yönetmek için konsorsiyum üye yönetim cmdletlerini kullanın. Kullanılabilir eylemler konsorsiyum rolünüze bağlıdır.

### <a name="get-blockchainmember"></a>BlockchainÜye Al

Konsorsiyumun üye bilgilerini veya liste üyelerini almak için bu cmdlet'i kullanın.

`Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Adı | Hakkında bilgi almak istediğiniz Blockchain Service üyesinin adı. Bir ad girildiğinde, üyenin ayrıntılarını döndürür. Bir ad atlandığında, tüm konsorsiyum üyelerinin listesini döndürür. | Hayır |
| Üyeler | İthalat-KonsorsiyumYönetim Sözleşmelerinden elde edilen üye nesnesi | Evet |
| Web3İstesi | Yeni-Web3Connection'dan elde edilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection değişkenini ayarlamak için [bir Web3 bağlantısı kurun.](#establish-a-web3-connection)

```powershell-interactive
$ContractConnection | Get-BlockchainMember -Name <Member Name>
```

#### <a name="example-output"></a>Örnek çıktı

```
Name           : myblockchainmember
CorrelationId  : 0
DisplayName    : myCompany
SubscriptionId : <Azure subscription ID>
AccountAddress : 0x85b911c9e103d6405573151258d668479e9ebeef
Role           : ADMIN
```

### <a name="remove-blockchainmember"></a>Remove-BlockchainMember

Blockchain üyesini kaldırmak için bu cmdlet'i kullanın.

`Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Adı | Kaldırılacak üye adı | Evet |
| Üyeler | İthalat-KonsorsiyumYönetim Sözleşmelerinden elde edilen üye nesnesi | Evet |
| Web3Hesap | Import-Web3Account'tan alınan Web3Account nesnesi | Evet |
| Web3İstesi | Yeni-Web3Connection'dan elde edilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection ve $MemberAccount değişkenlerini ayarlamak için [bir Web3 bağlantısı kurun.](#establish-a-web3-connection)

```powershell-interactive
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Bu cmdlet'i, ekran adı ve konsorsiyum rolü de dahil olmak üzere blockchain üye özniteliklerini ayarlamak için kullanın.

Konsorsiyum yöneticileri tüm üyeler için **DisplayName** ve **Role** ayarlayabilir. Kullanıcı rolüne sahip bir konsorsiyum üyesi yalnızca kendi üyesinin görüntü adını değiştirebilir.

```
Set-BlockchainMember -Name <String> [-DisplayName <String>] [-AccountAddress <String>] [-Role <String>]
 -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>
```

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Adı | Blockchain üyesinin adı | Evet |
| DisplayName | Yeni ekran adı | Hayır |
| Hesap Adresi | Hesap adresi | Hayır |
| Üyeler | İthalat-KonsorsiyumYönetim Sözleşmelerinden elde edilen üye nesnesi | Evet |
| Web3Hesap | Import-Web3Account'tan alınan Web3Account nesnesi | Evet |
| Web3İstesi |  Yeni-Web3Connection'dan elde edilen Web3Client nesnesi| Evet |

#### <a name="example"></a>Örnek

$ContractConnection ve $MemberAccount değişkenlerini ayarlamak için [bir Web3 bağlantısı kurun.](#establish-a-web3-connection)

```powershell-interactive
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="manage-the-consortium-members-invitations"></a>Konsorsiyum üyelerinin davetlerini yönetme

Konsorsiyum üyelerinin davetlerini yönetmek için konsorsiyum üye daveti yönetim cmdletlerini kullanın. Kullanılabilir eylemler konsorsiyum rolünüze bağlıdır.

### <a name="new-blockchainmemberinvitation"></a>Yeni BlockchainMemberInvitation

Konsorsiyuma yeni üyeler davet etmek için bu cmdlet kullanın.

```
New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>
```

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Davet edilebilen üyenin Azure abonelik kimliği | Evet |
| Rol | Konsorsiyum rolü. Değerler ADMIN veya USER olabilir. ADMIN konsorsiyum yöneticisi rolüdür. KULLANICI konsorsiyum üyesi rolüdür. | Evet |
| Üyeler | İthalat-KonsorsiyumYönetim Sözleşmelerinden elde edilen üye nesnesi | Evet |
| Web3Hesap | Import-Web3Account'tan alınan Web3Account nesnesi | Evet |
| Web3İstesi | Yeni-Web3Connection'dan elde edilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection ve $MemberAccount değişkenlerini ayarlamak için [bir Web3 bağlantısı kurun.](#establish-a-web3-connection)

```powershell-interactive
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>BlockchainÜye Davetiye Al

Konsorsiyum üyesinin davet durumunu almak veya listelemek için bu cmdlet'i kullanın.

`Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Davet edilebilen üyenin Azure abonelik kimliği. Abonelik kimliği sağlanırsa, abonelik kimliğinin davet ayrıntılarını döndürür. Abonelik kimliği atlanırsa, tüm üye davetlerinin listesini verir. | Hayır |
| Üyeler | İthalat-KonsorsiyumYönetim Sözleşmelerinden elde edilen üye nesnesi | Evet |
| Web3İstesi | Yeni-Web3Connection'dan elde edilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection değişkenini ayarlamak için [bir Web3 bağlantısı kurun.](#establish-a-web3-connection)

```powershell-interactive
$ContractConnection | Get-BlockchainMemberInvitation –SubscriptionId <Azure subscription ID>
```

#### <a name="example-output"></a>Örnek çıktı

```
SubscriptionId                       Role CorrelationId
--------------                       ---- -------------
<Azure subscription ID>              USER             2
```

### <a name="remove-blockchainmemberinvitation"></a>Remove-BlockchainMemberInvitation

Bir konsorsiyum üyesinin davetini iptal etmek için bu cmdlet'i kullanın.

```
Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>
```

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | İptal etmek için üyenin Azure abonelik kimliği | Evet |
| Üyeler | İthalat-KonsorsiyumYönetim Sözleşmelerinden elde edilen üye nesnesi | Evet |
| Web3Hesap | Import-Web3Account'tan alınan Web3Account nesnesi | Evet |
| Web3İstesi | Yeni-Web3Connection'dan elde edilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection ve $MemberAccount değişkenlerini ayarlamak için [bir Web3 bağlantısı kurun.](#establish-a-web3-connection)

```powershell-interactive
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-BlockchainMemberInvitation

**Varolan** bir davet için Rolü ayarlamak için bu cmdlet'i kullanın. Davetiyeleri yalnızca konsorsiyum yöneticileri değiştirebilir.

```
Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>
```

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Davet edilebilen üyenin Azure abonelik kimliği | Evet |
| Rol | Davet için yeni konsorsiyum rolü. Değerler **KULLANICI** veya **ADMIN**olabilir. | Evet |
| Üyeler |  İthalat-KonsorsiyumYönetim Sözleşmelerinden elde edilen üye nesnesi | Evet |
| Web3Hesap | Import-Web3Account'tan alınan Web3Account nesnesi | Evet |
| Web3İstesi | Yeni-Web3Connection'dan elde edilen Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

$ContractConnection ve $MemberAccount değişkenlerini ayarlamak için [bir Web3 bağlantısı kurun.](#establish-a-web3-connection)

```powershell-interactive
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Sonraki adımlar

Konsorsiyumlar, üyeler ve düğümler hakkında daha fazla bilgi için [Azure Blockchain Service konsorsiyumuna](consortium.md) bakın
