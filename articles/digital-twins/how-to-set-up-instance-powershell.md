---
title: Örnek ve kimlik doğrulaması ayarlama (PowerShell)
titleSuffix: Azure Digital Twins
description: Azure PowerShell kullanarak Azure dijital TWINS hizmeti örneğini ayarlama bölümüne bakın
author: baanders
ms.author: baanders
ms.date: 12/16/2020
ms.topic: how-to
ms.service: digital-twins
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 65495da13bc6c9ed91c1ecbd587c16c949136d73
ms.sourcegitcommit: c4c554db636f829d7abe70e2c433d27281b35183
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040696"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-powershell"></a>Azure dijital TWINS örneği ve kimlik doğrulaması (PowerShell) ayarlama

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Bu makalede, örneği oluşturma ve kimlik doğrulamasını ayarlama dahil olmak üzere **Yeni bir Azure dijital TWINS örneği ayarlama** adımları ele alınmaktadır. Bu makaleyi tamamladıktan sonra, programlamaya başlamak için kullanabileceğiniz bir Azure dijital TWINS örneğine sahip olursunuz.

Bu makalenin bu sürümü, bir diğeri Azure PowerShell kullanarak bu adımları el ile, tek tek gider.

* Azure portal kullanarak bu adımları el ile almak için, bu makalenin Portal sürümüne bakın: [*nasıl yapılır: bir örnek ve kimlik doğrulaması (portal) ayarlama*](how-to-set-up-instance-portal.md).
* Bir dağıtım betiği örneği kullanarak otomatikleştirilmiş bir kurulum aracılığıyla çalıştırmak için, bu makalenin betikleştirilmiş sürümüne bakın: [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama (komut dosyalı)*](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-steps.md](../../includes/digital-twins-setup-steps.md)]
[!INCLUDE [digital-twins-setup-permissions.md](../../includes/digital-twins-setup-permissions.md)]

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

1. Azure PowerShell yerel olarak kullanmayı tercih ederseniz:
   1. [Az PowerShell modülünü yükler](/powershell/azure/install-az-ps).
   1. [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanın.
1. Azure Cloud Shell kullanmayı seçerseniz:
   1. Daha fazla bilgi için bkz. [Azure Cloud Shell Genel Bakış](../cloud-shell/overview.md) .
   1. Cloud Shell simge çubuğunda, Cloud Shell PowerShell sürümünü çalıştıracak şekilde ayarlandığından emin olun.

      :::image type="content" source="media/how-to-set-up-instance/cloud-shell/cloud-shell-powershell.png" alt-text="PowerShell sürümünün seçimini gösteren pencere Cloud Shell":::

1. Birden çok Azure aboneliğiniz varsa, kaynakların faturalandırılması gereken uygun aboneliği seçin. [Set-AzContext](/powershell/module/az.accounts/set-azcontext) cmdlet 'ini kullanarak belirli bir abonelik seçin.

   ```azurepowershell-interactive
   Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
   ```

1. Bu abonelikle Azure dijital TWINS 'i ilk kez kullanıyorsanız, **Microsoft. DigitalTwins** kaynak sağlayıcısını kaydetmeniz gerekir.

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace Microsoft.DigitalTwins
   ```

> [!IMPORTANT]
> **Az. DigitalTwins** PowerShell modülü önizlemedeyken, cmdlet 'ini kullanarak ayrı olarak yüklemelisiniz `Install-Module` . Bu PowerShell modülü genel kullanıma sunulduktan sonra, gelecekteki Az PowerShell modülü sürümlerinin bir parçası haline gelecek ve Azure Cloud Shell içinden varsayılan olarak sağlanacaktır.

```azurepowershell-interactive
Install-Module -Name Az.DigitalTwins
```

## <a name="create-the-azure-digital-twins-instance"></a>Azure dijital TWINS örneğini oluşturma

Bu bölümde, Azure PowerShell kullanarak **Azure dijital TWINS 'in yeni bir örneğini oluşturacaksınız** .
Şunları sağlamanız gerekir:

* Bir [Azure Kaynak grubu](../azure-resource-manager/management/overview.md). Zaten mevcut bir kaynak grubunuz yoksa, [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) cmdlet 'ini kullanarak bir tane oluşturabilirsiniz.

  ```azurepowershell-interactive
  New-AzResourceGroup -Name <name-for-your-resource-group> -Location <region>
  ```

* Dağıtım için bir bölge. Azure dijital TWINS 'i destekleyen bölgeleri görmek için [*bölgeye göre kullanılabilen Azure ürünlerini*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins)ziyaret edin.
* Örneğiniz için bir ad. Yeni örneğin adı, aboneliğinizin bölgesi içinde benzersiz olmalıdır. Aboneliğiniz zaten belirtilen adı kullanan bölgede başka bir Azure dijital TWINS örneğine sahipse, farklı bir ad seçmeniz istenir.

Örneği oluşturmak için aşağıdaki örnekteki değerleri kullanın:

```azurepowershell-interactive
New-AzDigitalTwinsInstance -ResourceGroupName <your-resource-group> -ResourceName <name-for-your-Azure-Digital-Twins-instance> -Location <region>
```

### <a name="verify-success-and-collect-important-values"></a>Başarıyı doğrulama ve önemli değerleri toplama

Örnek başarıyla oluşturulduysa, sonuç, oluşturduğunuz kaynakla ilgili bilgiler içeren aşağıdaki çıktıya benzer şekilde görünür:

```Output
Location   Name                                         Type
--------   ----                                         ----
<region>   <name-for-your-Azure-Digital-Twins-instance> Microsoft.DigitalTwins/digitalTwinsInstances
```

> [!TIP]
> Bu özellikleri, örneğinizin tüm özellikleriyle birlikte çalıştırarak ve kullanarak istediğiniz zaman görebilirsiniz `Get-AzDigitalTwinsInstance` `Select-Object -Property *` .

```azurepowershell-interactive
Get-AzDigitalTwinsInstance -ResourceGroupName <your-resource-group> -ResourceName <name-for-your-Azure-Digital-Twins-instance> |
  Select-Object -Property *
```

Azure dijital TWINS örneğinin **ana bilgisayar** adı, **adı** ve **ResourceGroup** değerini aklınızda edin. Bunlar, Azure dijital TWINS örneğiniz ile çalışmaya devam ederken kimlik doğrulamasını ve ilgili Azure kaynaklarını ayarlamak için ihtiyaç duyduğunuz önemli değerlerdir. Diğer kullanıcılar örnekle programlama yapacaktır, bu değerleri bunlarla paylaşmanız gerekir.

Artık hazır bir Azure dijital TWINS örneğiniz var. Daha sonra, uygun Azure Kullanıcı izinlerini yönetmek için size izin verirsiniz.

## <a name="set-up-user-access-permissions"></a>Kullanıcı erişim izinlerini ayarlama

[!INCLUDE [digital-twins-setup-role-assignment.md](../../includes/digital-twins-setup-role-assignment.md)]

[Get-AzAdUser](/powershell/module/az.resources/get-azaduser) cmdlet 'ini kullanarak role atanması gereken kullanıcının Azure AD hesabı için **ObjectID** 'yi belirleme.

```azurepowershell-interactive
Get-AzADUser -UserPrincipalName <Azure-AD-user-principal-name-of-user-to-assign>
```

Rolü atamak için aşağıdaki komutu kullanın. Azure aboneliğinde [yeterli izinlere](#prerequisites-permission-requirements) sahip bir kullanıcı tarafından çalıştırılması gerekir. Bu komut, kullanıcının rolün atanması gereken Azure AD hesabı için **ObjectID** 'yi geçmesi gerekir. Ayrıca, daha önce seçtiğiniz abonelik KIMLIĞINI, kaynak grubu adını ve Azure dijital TWINS örnek adını kullanmanız gerekir.

```azurepowershell-interactive
$Params = @{
  ObjectId = '<Azure-AD-user-object-id-of-user-to-assign>'
  RoleDefinitionName = 'Azure Digital Twins Data Owner'
  Scope = '/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DigitalTwins/digitalTwinsInstances/<name-for-your-Azure-Digital-Twins-instance>'
}
New-AzRoleAssignment @Params
```

Bu komutun sonucu oluşturulan rol ataması hakkında bilgi verilir.

### <a name="verify-success"></a>Başarıyı doğrula

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

Artık bir Azure dijital TWINS örneğiniz var ve bunu yönetmek için izinler atandı.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. kimlik doğrulama kodu ile bir istemci uygulamasını örneğinize bağlama:
* [*Nasıl yapılır: uygulama kimlik doğrulama kodunu yazma*](how-to-authenticate-client.md)
