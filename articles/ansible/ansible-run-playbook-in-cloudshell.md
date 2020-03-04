---
title: Hızlı başlangıç-Azure Cloud Shell içinde Bash aracılığıyla Anerişilebilir PlayBook 'lar çalıştırma
description: Bu hızlı başlangıçta, Azure Cloud Shell ile Bash ile çeşitli görevleri nasıl gerçekleştirebileceğinizi öğrenin
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.date: 04/30/2019
ms.openlocfilehash: 2a938179cf2e07a61749042db32ef9e1c9d843ba
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78247884"
---
# <a name="quickstart-run-ansible-playbooks-via-bash-in-azure-cloud-shell"></a>Hızlı başlangıç: Azure Cloud Shell Bash aracılığıyla anerişilebilir PlayBook 'ları çalıştırma

Azure Cloud Shell, Azure kaynaklarını yönetmeye yönelik etkileşimli, tarayıcıda erişilebilen bir kabuktur. Cloud Shell, bir bash veya PowerShell komut satırı kullanmanıza olanak sağlar. Bu makalede, Anerişilebilir bir PlayBook 'u çalıştırmak için Azure Cloud Shell içinde Bash 'i kullanacaksınız.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Azure Cloud Shell yapılandırma** -Azure Cloud Shell yeni başladıysanız [Azure Cloud Shell bash için hızlı başlangıç](https://docs.microsoft.com/azure/cloud-shell/quickstart)' a bakın.
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Otomatik kimlik bilgisi yapılandırma

Cloud Shell oturum açıldığında, ek bir yapılandırma olmadan altyapıyı yönetmek için Azure ile kimlik doğrulaması yapılır. 

Birden çok abonelikle çalışırken, `AZURE_SUBSCRIPTION_ID` ortam değişkenini dışarı aktararak aboneliği erişilebilir kullanımları belirtin. 

Tüm Azure aboneliklerinizi listelemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az account list
```

Azure abonelik KIMLIĞINIZI kullanarak `AZURE_SUBSCRIPTION_ID` aşağıdaki gibi ayarlayın:

```console
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama
Başarılı yapılandırmayı doğrulamak için, bir Azure Kaynak grubu oluşturmak için anda kullanın.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Hızlı başlangıç: Azure 'da sanal makineyi anormal olarak yapılandırma](./ansible-create-vm.md)