---
title: Azure hızlı başlangıç-Azure Resource Manager şablonu kullanarak yönetilen bir HSM oluşturma
description: Kaynak Yöneticisi şablonu kullanarak Azure Azure Key Vault yönetilen bir HSM oluşturmayı gösteren hızlı başlangıç
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: quickstart
ms.custom: mvc
ms.date: 09/15/2020
ms.author: mbaldwin
ms.openlocfilehash: 4d1488d6dd2e5d08ae774ca88b7ab41b2020efe5
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91001055"
---
# <a name="quickstart-create-an-key-vault-managed-hsm-using-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonu kullanarak Key Vault yönetilen HSM oluşturma

Yönetilen HSM, **fıps 140-2 düzey 3** tarafından doğrulanan HSM 'ler kullanılarak bulut uygulamalarınız için şifreleme anahtarlarını korumanızı sağlayan, tam olarak yönetilen, yüksek oranda kullanılabilir, tek kiracılı ve standartlara uygun bir bulut hizmetidir.  

Bu hızlı başlangıç, yönetilen bir HSM oluşturmak için Kaynak Yöneticisi şablonu dağıtma işlemine odaklanır.  [Resource Manager şablonu](../../azure-resource-manager/templates/overview.md) projenizin altyapısını ve yapılandırmasını tanımlayan bir JavaScript Nesne Gösterimi (JSON) dosyasıdır. Şablon, dağıtmak istediğiniz öğeyi oluşturmaya yönelik programlama komutları dizisini yazmak zorunda kalmadan bu öğeyi belirtmenize imkan tanıyan bildirim temelli söz dizimini kullanır. Resource Manager şablonları geliştirme hakkında daha fazla bilgi edinmek istiyorsanız bkz. [Resource Manager belgeleri](../../azure-resource-manager/index.yml) ve [şablon başvurusu](/azure/templates/microsoft.keyvault/allversions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için aşağıdaki öğelere sahip olmanız gerekir:

- Bir Microsoft Azure aboneliği. Hesabınız yoksa, [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/pricing/free-trial).
- Azure CLı sürüm 2.12.0 veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz [. Azure CLI 'Yı yüklemek]( /cli/azure/install-azure-cli)


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

CLı kullanarak Azure 'da oturum açmak için şunu yazabilirsiniz:

```azurecli
az login
```

CLı aracılığıyla oturum açma seçenekleri hakkında daha fazla bilgi için bkz. [Azure CLI ile oturum açma](/cli/azure/authenticate-azure-cli?view=azure-cli-latest&preserve-view=true)

## <a name="create-a-manage-hsm"></a>Bir yönetim HSM oluşturma

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-managed-hsm-create/) alınmıştır.

Şablonda tanımlanan Azure kaynağı:

* **Microsoft. Keykasası/managedHSMs**: Azure Key Vault YÖNETILEN bir HSM oluşturun.

Daha fazla Azure Key Vault şablon örneği [burada](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Keyvault)bulunabilir.

Şablon, hesabınızla ilişkili nesne KIMLIĞINI gerektiriyor. Bunu bulmak için, e-posta adresinizi parametreye geçirerek Azure CLı [az ad User Show](/cli/azure/ad/user?view=azure-cli-latest&preserve-view=true#az_ad_user_show) komutunu kullanın `--id` . Yalnızca parametresiyle ilgili çıktıyı nesne KIMLIĞIYLE sınırlayabilirsiniz `--query` .

```azurecli-interactive
az ad user show --id <your-email-address> --query "objectId"
```

Kiracı KIMLIĞINIZ da gerekebilir. Bunu bulmak için, Azure CLı [az ad User Show](/cli/azure/account?view=azure-cli-latest&preserve-view=true#az_account_show) komutunu kullanın. Çıktıyı yalnızca parametresiyle birlikte kiracı KIMLIĞIYLE sınırlayabilirsiniz `--query` .

 ```azurecli-interactive
 az account show --query "tenantId"
 ```

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir anahtar kasası ve gizli dizi oluşturur.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-managed-hsm-create%2Fazuredeploy.json"><img src="../media/deploy-to-azure.svg" alt="deploy to azure"/></a>

2. Aşağıdaki değerleri seçin veya girin.

    Belirtilmediği takdirde, anahtar kasasını ve bir parolayı oluşturmak için varsayılan değeri kullanın.

    - **Abonelik**: bir Azure aboneliği seçin.
    - **Kaynak grubu**: **Yeni oluştur**' u seçin, kaynak grubu için benzersiz bir ad girin ve ardından **Tamam**' a tıklayın.
    - **Konum**: bir konum seçin. Örneğin, **Orta Güney ABD**.
    - **Managedhsmname**: yönetilen HSM 'niz için bir ad girin.
    - **SKU**: oluşturmak ISTEDIĞINIZ yönetilen HSM 'nin adını ve ailesini girin.  Bu hızlı başlangıçta, Aile için "Standard_B1" adını ve "B" yazın.
    - **KIRACı kimliği**: şablon IşLEVI kiracı kimliğinizi otomatik olarak alır; Varsayılan değeri değiştirmeyin.  Değer yoksa, [ön](#prerequisites)koşullarda ALDıĞıNıZ Kiracı kimliğini girin.
    * **ınitialadminobjectıds**: [ön](#prerequisites)koşullarda aldığınız nesne kimliğini girin.

3. **Satın al**'ı seçin. Anahtar Kasası başarıyla dağıtıldıktan sonra bir bildirim alırsınız:

Şablonu dağıtmak için Azure portalı kullanılır. Azure portal ek olarak, Azure PowerShell, Azure CLı ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../../azure-resource-manager/templates/deploy-powershell.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yönetilen bir HSM oluşturdunuz. Bu yönetilen HSM, etkinleştirilinceye kadar tamamen işlevsel olmayacaktır. HSM 'nizi nasıl etkinleştireceğinizi öğrenmek için bkz. [YÖNETILEN HSM 'Nizi etkinleştirme](quick-create-cli.md#activate-your-managed-hsm) .

- [YÖNETILEN HSM 'ye genel bakış](overview.md) konusunu okuyun
- [Yönetilen BIR HSM 'de anahtarları yönetme](key-management.md) hakkında bilgi edinin
- [YÖNETILEN HSM en iyi yöntemlerini](best-practices.md) gözden geçirme