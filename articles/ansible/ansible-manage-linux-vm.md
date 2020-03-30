---
title: Quickstart - Ansible kullanarak Azure'daki Linux sanal makinelerini yönetin
description: Bu hızlı başlangıçta, Ansible'ı kullanarak Azure'da bir Linux sanal makinesini nasıl yönetişize edin
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: gwallace
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: d94858391951aaf9387394afeb5ad2ae373fa7b5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80239524"
---
# <a name="quickstart-manage-linux-virtual-machines-in-azure-using-ansible"></a>Hızlı başlatma: Ansible kullanarak Azure'daki Linux sanal makinelerini yönetin

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Bu makalede, bir Linux sanal makine başlatmak ve durdurmak için bir Ansible oyun kitabı kullanın. 

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-sub.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="stop-a-virtual-machine"></a>Sanal makineyi durdurma

Bu bölümde, Bir Azure sanal makinesini dağıtmak (durdurmak) için Ansible'ı kullanırsınız.

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.

1. [Açık Bulut Kabuk](/azure/cloud-shell/overview).

1. Adlı `azure-vm-stop.yml`bir dosya oluşturun ve düzenleyicide açın:

    ```bash
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

1. Yer `{{ vm_name }}` `{{ resource_group_name }}` tutucuları ve yer tutucuları değerlerinizle değiştirin.

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

1. Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1. Oyun kitabını çalıştırdıktan sonra, aşağıdaki sonuçlara benzer çıktı görürsünüz:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="start-a-virtual-machine"></a>Sanal makineyi başlatma

Bu bölümde, ayrılmış (durdurulmuş) bir Azure sanal makinesini başlatmak için Ansible'ı kullanırsınız.

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.

1. [Açık Bulut Kabuk](/azure/cloud-shell/overview).

1. Adlı `azure-vm-start.yml`bir dosya oluşturun ve düzenleyicide açın:

    ```bash
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

1. Yer `{{ vm_name }}` `{{ resource_group_name }}` tutucuları ve yer tutucuları değerlerinizle değiştirin.

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

1. Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1. Oyun kitabını çalıştırdıktan sonra, aşağıdaki sonuçlara benzer çıktı görürsünüz:

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
> [Öğretici: Ansible kullanarak Azure dinamik envanterlerini yönetme](./ansible-manage-azure-dynamic-inventories.md)