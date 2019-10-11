---
title: Öğretici-Azure 'da Azure Kubernetes hizmeti (AKS) kümelerini kullanarak yapılandırma
description: Azure 'da bir Azure Kubernetes hizmet kümesi oluşturmak ve yönetmek için nasıl kullanılacağını öğrenin
keywords: anyalabilen, Azure, DevOps, Bash, cloudshell, PlayBook, aks, Container, aks, Kubernetes
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 9b70a9c364768322a3eae6ef5b92c87b6839c540
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72242080"
---
# <a name="tutorial-configure-azure-kubernetes-service-aks-clusters-in-azure-using-ansible"></a>Öğretici: Azure 'da Azure Kubernetes hizmeti (AKS) kümelerini anormal kullanarak yapılandırma

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

AKS, Kullanıcı kimlik doğrulaması için [Azure Active Directory (ad)](/azure/active-directory/) kullanacak şekilde yapılandırılabilir. Yapılandırıldıktan sonra, AKS kümesinde oturum açmak için Azure AD kimlik doğrulama belirtecinizi kullanırsınız. RBAC, kullanıcının kimliğini veya dizin grubu üyeliğini temel alabilir.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * AKS kümesi oluşturma
> * AKS kümesi yapılandırma

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-managed-aks-cluster"></a>Yönetilen AKS kümesi oluşturma

Örnek PlayBook, kaynak grubu içinde bir kaynak grubu ve bir AKS kümesi oluşturur.

Aşağıdaki PlayBook 'u @no__t olarak kaydet-0:

```yml
- name: Create Azure Kubernetes Service
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
  - name: Create a managed Azure Container Services (AKS) cluster
    azure_rm_aks:
      name: "{{ aks_name }}"
      location: "{{ location }}"
      resource_group: "{{ resource_group }}"
      dns_prefix: "{{ aks_name }}"
      linux_profile:
        admin_username: "{{ username }}"
        ssh_key: "{{ ssh_key }}"
      service_principal:
        client_id: "{{ client_id }}"
        client_secret: "{{ client_secret }}"
      agent_pool_profiles:
        - name: default
          count: 2
          vm_size: Standard_D2_v2
      tags:
        Environment: Production
```

PlayBook 'u çalıştırmadan önce aşağıdaki notlara bakın:

- @No__t-0 ' daki ilk bölüm, `eastus` konumunda `myResourceGroup` adlı bir kaynak grubu tanımlar.
- @No__t-0 ' daki ikinci bölüm, `myResourceGroup` kaynak grubu içinde `myAKSCluster` adlı bir AKS kümesini tanımlar.
- @No__t-0 yer tutucusu için, "SSH-RSA" (tırnak işaretleri olmadan) ile başlayan RSA ortak anahtarınızı tek satırlık biçimde girin.

@No__t-0 komutunu kullanarak PlayBook 'u çalıştırın:

```bash
ansible-playbook azure_create_aks.yml
```

PlayBook 'un çalıştırılması aşağıdaki çıktıya benzer sonuçları gösterir:

```Output
PLAY [Create AKS] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create resource group] 
changed: [localhost]

TASK [Create an Azure Container Services (AKS) cluster] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=3    changed=2    unreachable=0    failed=0
```

## <a name="scale-aks-nodes"></a>AKS düğümlerini ölçeklendirin

Önceki bölümde yer aldığı örnek PlayBook iki düğümü tanımlar. @No__t-1 bloğundaki `count` değerini değiştirerek düğüm sayısını ayarlayabilirsiniz.

Aşağıdaki PlayBook 'u @no__t olarak kaydet-0:

```yml
- name: Scale AKS cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Scaling an existed AKS cluster
    azure_rm_aks:
        name: "{{ aks_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        dns_prefix: "{{ aks_name }}"
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 3
            vm_size: Standard_D2_v2
```

PlayBook 'u çalıştırmadan önce aşağıdaki notlara bakın:

- @No__t-0 yer tutucusu için, "SSH-RSA" (tırnak işaretleri olmadan) ile başlayan RSA ortak anahtarınızı tek satırlık biçimde girin.

@No__t-0 komutunu kullanarak PlayBook 'u çalıştırın:

```bash
ansible-playbook azure_configure_aks.yml
```

PlayBook 'un çalıştırılması aşağıdaki çıktıya benzer sonuçları gösterir:

```Output
PLAY [Scale AKS cluster] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Scaling an existed AKS cluster] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

## <a name="delete-a-managed-aks-cluster"></a>Yönetilen bir AKS kümesini silme

Örnek PlayBook bir AKS kümesini siler.

Aşağıdaki PlayBook 'u @no__t olarak kaydet-0:


```yml
- name: Delete a managed Azure Container Services (AKS) cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    aks_name: myAKSCluster
  tasks:
  - name:
    azure_rm_aks:
      name: "{{ aks_name }}"
      resource_group: "{{ resource_group }}"
      state: absent
  ```

@No__t-0 komutunu kullanarak PlayBook 'u çalıştırın:

```bash
ansible-playbook azure_delete_aks.yml
```

PlayBook 'un çalıştırılması aşağıdaki çıktıya benzer sonuçları gösterir:

```Output
PLAY [Delete a managed Azure Container Services (AKS) cluster] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [azure_rm_aks] 

PLAY RECAP 
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure Kubernetes Service 'te (AKS) uygulama ölçeklendirme](/azure/aks/tutorial-kubernetes-scale)