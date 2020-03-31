---
title: Quickstart - Azure Cloud Shell'de Bash üzerinden Ansible oyun kitaplarını çalıştırın
description: Bu hızlı başlangıçta, Azure Bulut Uyp'ta Bash ile çeşitli Ansible görevleri nasıl gerçekleştiracağınızı öğrenin
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.date: 04/30/2019
ms.openlocfilehash: 2a938179cf2e07a61749042db32ef9e1c9d843ba
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78247884"
---
# <a name="quickstart-run-ansible-playbooks-via-bash-in-azure-cloud-shell"></a>Hızlı Başlangıç: Azure Cloud Shell'de Bash üzerinden Ansible oyun kitaplarını çalıştırın

Azure Cloud Shell, Azure kaynaklarını yönetmek için kullanabileceğiniz etkileşimli ve tarayıcı ile erişilebilen bir kabuktur. Cloud Shell, Bash veya PowerShell komut satırlarını kullanmanızı sağlar. Bu makalede, Bir Ansible oyun kitabı çalıştırmak için Azure Bulut Kabuğu içindeki Bash'i kullanıyorsunuz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Azure Bulut Kabuğunu Yapılandırın** - Azure Bulut Shell'de yeniyseniz, [Azure Bulut BulutU'da Bash için Hızlı Başlangıç'a](https://docs.microsoft.com/azure/cloud-shell/quickstart)bakın.
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Otomatik kimlik bilgisi yapılandırma

Bulut Kabuğu'nda oturum açtığınızda, Ansible, ek yapılandırma olmadan altyapıyı yönetmek için Azure ile kimlik doğrulaması sağlar. 

Birden çok abonelikle çalışırken, `AZURE_SUBSCRIPTION_ID` ortam değişkenini dışa aktararak Ansible'ın kullandığı abonelikleri belirtin. 

Azure aboneliklerinizin tümünün listesini yapmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az account list
```

Azure abonelik kimliğinizi kullanarak `AZURE_SUBSCRIPTION_ID` aşağıdaki leri ayarlayın:

```console
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama
Başarılı yapılandırmayı doğrulamak için, Bir Azure kaynak grubu oluşturmak için Ansible'ı kullanın.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Hızlı başlatma: Ansible kullanarak Azure'da sanal makineyi yapılandırın](./ansible-create-vm.md)