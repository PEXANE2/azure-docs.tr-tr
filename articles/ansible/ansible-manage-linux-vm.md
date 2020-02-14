---
title: Hızlı başlangıç-Azure 'da Linux sanal makinelerini anormal olarak yönetme
description: Bu hızlı başlangıçta, Azure 'da bir Linux sanal makinesini kullanarak nasıl yönetebileceğinizi öğrenin
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: gwallace
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 90113373535c835e5cddf707bcb520789e596aec
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77202406"
---
# <a name="quickstart-manage-linux-virtual-machines-in-azure-using-ansible"></a>Hızlı başlangıç: Azure 'da Linux sanal makinelerini kullanarak yönetme

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Bu makalede, bir Linux sanal makinesini başlatmak ve durdurmak için anormal bir PlayBook kullanırsınız. 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-sub.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="stop-a-virtual-machine"></a>Bir sanal makineyi durdurma

Bu bölümde, bir Azure sanal makinesini serbest bırakmak (durdurmak) için kullanılması mümkün değildir.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)’ında oturum açın.

1. [Cloud Shell](/azure/cloud-shell/overview)'i açın.

1. `azure-vm-stop.yml`adlı bir dosya oluşturun ve düzenleyicide açın:

    ```azurecli-interactive
    code azure-vm-stop.yml
    ```

1. Aşağıdaki örnek kodu düzenleyiciye yapıştırın:

    ```yaml
    - name: Stop Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Stop virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
            allocated: no
    ```

1. `{{ resource_group_name }}` ve `{{ vm_name }}` yer tutucuları değerlerinizle değiştirin.

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

1. `ansible-playbook` komutunu kullanarak PlayBook 'u çalıştırın:

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1. PlayBook çalıştırıldıktan sonra aşağıdaki sonuçlara benzer bir çıktı görürsünüz:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="start-a-virtual-machine"></a>Bir sanal makineyi başlatma

Bu bölümde, serbest bırakılmış (durdurulmuş) bir Azure sanal makinesini başlatmak için kullanabilirsiniz.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)’ında oturum açın.

1. [Cloud Shell](/azure/cloud-shell/overview)'i açın.

1. `azure-vm-start.yml`adlı bir dosya oluşturun ve düzenleyicide açın:

    ```azurecli-interactive
    code azure-vm-start.yml
    ```

1. Aşağıdaki örnek kodu düzenleyiciye yapıştırın:

    ```yaml
    - name: Start Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Start virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
    ```

1. `{{ resource_group_name }}` ve `{{ vm_name }}` yer tutucuları değerlerinizle değiştirin.

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

1. `ansible-playbook` komutunu kullanarak PlayBook 'u çalıştırın:

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1. PlayBook çalıştırıldıktan sonra aşağıdaki sonuçlara benzer bir çıktı görürsünüz:

    ```bash
    PLAY [Start Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Öğretici: anormal kullanarak Azure dinamik envanterleri yönetme](./ansible-manage-azure-dynamic-inventories.md)