---
title: Öğretici - Ansible kullanarak Azure Hizmet Veri Tos'undaki kuyrukları yapılandırma
description: Azure Hizmet Veri Yolu kuyruğu oluşturmak için Ansible'ı nasıl kullanacağınızı öğrenin
keywords: ansible, azure, devops, bash, oyun kitabı, servis otobüsü, kuyruk
ms.topic: tutorial
ms.date: 04/30/2019
ms.openlocfilehash: 8ba4c2296d903c4f35aa36eb92dfbc3b56ec4b18
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76713227"
---
# <a name="tutorial-configure-queues-in-azure-service-bus-using-ansible"></a>Öğretici: Ansible kullanarak Azure Hizmet Veri Tos'undaki kuyrukları yapılandırma

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-servicebus.md](../../includes/open-source-devops-intro-servicebus.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Bir kuyruk oluşturma
> * Bir SAS plicy oluşturma
> * Ad alanı bilgilerini alma
> * Sıra bilgilerini alma
> * Sıra SAS ilkesini iptal et

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-the-service-bus-queue"></a>Servis Veri Servisi kuyruğunu oluşturma

Örnek oyun kitabı kodu aşağıdaki kaynakları oluşturur:
- Azure kaynak grubu
- Kaynak grubu içindeki Servis Veri Servisi ad alanı
- Ad alanı ile Servis Veri Servisi sırası

Aşağıdaki playbook'u `servicebus_queue.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      location: eastus
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Ensure resource group exist
      azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          location: "{{ location }}"
    - name: Create a namespace
      azure_rm_servicebus:
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
    - name: Create a queue
      azure_rm_servicebusqueue:
          name: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: queue
    - debug:
          var: queue
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook servicebus_queue.yml
```

## <a name="create-the-sas-policy"></a>SAS ilkesini oluşturma

[Paylaşılan Erişim İmzası (SAS),](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) belirteçleri kullanan taleplere dayalı bir yetkilendirme mekanizmasıdır. 

Örnek oyun kitabı kodu, farklı ayrıcalıklara sahip bir Servis Veri Servisi veri günü kuyruğu için iki SAS ilkeleri oluşturur.

Aşağıdaki playbook'u `servicebus_queue_policy.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Create a policy with send and listen privilege
      azure_rm_servicebussaspolicy:
          name: "{{ queue }}-policy"
          queue: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          rights: listen_send
      register: policy
    - debug:
          var: policy
```

Oyun kitabını çalıştırmadan önce aşağıdaki notalara bakın:
- Değer, `rights` kullanıcının kuyrukla olan ayrıcalığını temsil eder. Aşağıdaki değerlerden birini `manage`belirtin: , , `listen` `send`, veya `listen_send`.

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook servicebus_queue_policy.yml
```

## <a name="retrieve-namespace-information"></a>Ad alanı bilgilerini alma

Örnek oyun kitabı kodu ad alanı bilgilerini sorgular.

Aşağıdaki playbook'u `servicebus_namespace_info.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
  tasks:
    - name: Get a namespace's information
      azure_rm_servicebus_facts:
          type: namespace
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          show_sas_policies: yes
      register: ns
    - debug:
          var: ns
```

Oyun kitabını çalıştırmadan önce aşağıdaki notalara bakın:
- Değer, `show_sas_policies` SAS ilkelerinin belirtilen ad alanı altında gösterip göstermeyeceğini gösterir. Varsayılan olarak, değer `False` ek ağ ek yükü önlemek içindir.

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook servicebus_namespace_info.yml
```

## <a name="retrieve-queue-information"></a>Sıra bilgilerini alma

Örnek playbook kodu sıra bilgilerini sorgular. 

Aşağıdaki playbook'u `servicebus_queue_info.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Get a queue's information
      azure_rm_servicebus_facts:
          type: queue
          name: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          show_sas_policies: yes
      register: queue
    - debug:
          var: queue
```

Oyun kitabını çalıştırmadan önce aşağıdaki notalara bakın:
- Değer, `show_sas_policies` belirtilen sıranın altındaki SAS ilkelerinin gösterip gösterilmeyip gösterilemeyeceğini gösterir. Varsayılan olarak, bu değer `False` ek ağ ek yükü önlemek için ayarlanır.

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook servicebus_queue_info.yml
```

## <a name="revoke-the-queue-sas-policy"></a>Sıra SAS ilkesini iptal et

Örnek playbook kodu bir sıra SAS ilkesini siler.

Aşağıdaki playbook'u `servicebus_queue_policy_delete.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Create a policy with send and listen privilege
      azure_rm_servicebussaspolicy:
          name: "{{ queue }}-policy"
          queue: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          state: absent
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook servicebus_queue_policy_delete.yml
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, bu makalede oluşturulan kaynakları silin. 

Aşağıdaki kodu aşağıdaki `cleanup.yml`gibi kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Delete queue
      azure_rm_servicebusqueue:
          name: "{{ queue }}"
          resource_group: "{{ resource_group }}"
          namespace: "{{ namespace }}"
          state: absent
    - name: Delete namespace
      azure_rm_servicebus:
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          state: absent
    - name: Delete resource group
      azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          state: absent
          force_delete_nonempty: yes
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Öğretici: Ansible kullanarak Azure Hizmet Veri Tos'undaki bir konuyu yapılandırma](ansible-service-bus-topic-configure.md)