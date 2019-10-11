---
title: Öğretici-Azure Service Bus konuları kullanarak yapılandırma
description: Azure Service Bus konu başlığı oluşturmak için nasıl kullanılacağını öğrenin
keywords: anerişilebilir, Azure, DevOps, Bash, PlayBook, Service Bus, konular, abonelikler
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 952779db582f9437f10608bf86b0b80560ded2c0
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72241227"
---
# <a name="tutorial-configure-topics-in-azure-service-bus-using-ansible"></a>Öğretici: Azure Service Bus konuları yapılandırma ve kullanma

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-servicebus.md](../../includes/open-source-devops-intro-servicebus.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Konu Oluştur
> * Abonelik oluşturma
> * SAS ilkesi oluşturma
> * Ad alanı bilgilerini al
> * Konu ve abonelik bilgilerini alma
> * SAS ilkesini iptal etme

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-the-service-bus-topic"></a>Service Bus konusunu oluşturma

Örnek PlayBook kodu aşağıdaki kaynakları oluşturur:
- Azure Kaynak grubu
- Kaynak grubu içinde Service Bus ad alanı
- Ad alanı ile Service Bus konu başlığı

Aşağıdaki PlayBook 'u @no__t olarak kaydet-0:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      location: eastus
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Ensure resource group exist
      azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          location: "{{ location }}"
    - name: Create a namespace
      azure_rm_servicebus:
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
    - name: Create a topic
      azure_rm_servicebustopic:
          name: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: topic
    - debug:
          var: topic
```

@No__t-0 komutunu kullanarak PlayBook 'u çalıştırın:

```bash
ansible-playbook servicebus_topic.yml
```

## <a name="create-the-subscription"></a>Abonelik oluşturma

Örnek PlayBook kodu bir Service Bus konusunun altında aboneliği oluşturur. Azure Service Bus konular birden çok aboneliğe sahip olabilir. Bir konuya abone olmak, konuya gönderilen her iletinin bir kopyasını alabilir. Abonelikler, bir şekilde oluşturulan, ancak isteğe bağlı olarak kullanım süresini belirleyen varlıklardır.

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      location: eastus
      namespace: servicebustestns
      topic: servicebustesttopic
      subscription: servicebustestsubs
  tasks:
    - name: Create a subscription
      azure_rm_servicebustopicsubscription:
          name: "{{ subscription }}"
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: subs
    - debug:
          var: subs
```

Aşağıdaki PlayBook 'u @no__t olarak kaydet-0:

@No__t-0 komutunu kullanarak PlayBook 'u çalıştırın:

```bash
ansible-playbook servicebus_subscription.yml
```

## <a name="create-the-sas-policy"></a>SAS ilkesi oluşturma

[Paylaşılan erişim imzası (SAS)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) , belirteçleri kullanan, talep tabanlı bir yetkilendirme mekanizmasıdır. 

Örnek PlayBook kodu, farklı ayrıcalıklara sahip bir Service Bus kuyruğu için iki SAS ilkesi oluşturur.

Aşağıdaki PlayBook 'u @no__t olarak kaydet-0:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Create a policy with send and listen privilege
      azure_rm_servicebussaspolicy:
          name: "{{ topic }}-{{ item }}"
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          rights: "{{ item }}"
      with_items:
        - send
        - listen
      register: policy
    - debug:
          var: policy
```

@No__t-0 komutunu kullanarak PlayBook 'u çalıştırın:

```bash
ansible-playbook servicebus_topic_policy.yml
```

## <a name="retrieve-namespace-information"></a>Ad alanı bilgilerini al

Örnek PlayBook kodu, ad alanı bilgilerini sorgular.

Aşağıdaki PlayBook 'u @no__t olarak kaydet-0:

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

PlayBook 'u çalıştırmadan önce aşağıdaki notlara bakın:
- @No__t-0 değeri, belirtilen ad alanı altında SAS ilkelerinin gösterilip gösterilmeyeceğini gösterir. Varsayılan olarak, ek ağ yükünün önüne geçmek için değer `False` ' dır.

@No__t-0 komutunu kullanarak PlayBook 'u çalıştırın:

```bash
ansible-playbook servicebus_namespace_info.yml
```

## <a name="retrieve-topic-and-subscription-information"></a>Konu ve abonelik bilgilerini alma

Örnek PlayBook kodu aşağıdaki bilgiler için sorgular:
- Service Bus konu bilgileri
- Konunun abonelik ayrıntıları listesi
 
Aşağıdaki PlayBook 'u @no__t olarak kaydet-0:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Get a topic's information
      azure_rm_servicebus_facts:
          type: topic
          name: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          show_sas_policies: yes
      register: topic_fact
    - name: "List subscriptions under topic {{ topic }}"
      azure_rm_servicebus_facts:
          type: subscription
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: subs_fact
    - debug:
          var: "{{ item }}"
      with_items:
        - topic_fact.servicebuses[0]
        - subs_fact.servicebuses
```

PlayBook 'u çalıştırmadan önce aşağıdaki notlara bakın:
- @No__t-0 değeri, belirtilen sırada SAS ilkelerinin gösterilip gösterilmeyeceğini gösterir. Bu değer, ek ağ yükünü önlemek için varsayılan olarak `False` olarak ayarlanır.

@No__t-0 komutunu kullanarak PlayBook 'u çalıştırın:

```bash
ansible-playbook servicebus_list.yml
```

## <a name="revoke-the-queue-sas-policy"></a>Kuyruk SAS ilkesini iptal etme

Örnek PlayBook kodu bir sıra SAS ilkesini siler.

Aşağıdaki PlayBook 'u @no__t olarak kaydet-0:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Delete a policy
      azure_rm_servicebussaspolicy:
          name: "{{ topic }}-policy"
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          state: absent
```

@No__t-0 komutunu kullanarak PlayBook 'u çalıştırın:

```bash
ansible-playbook servicebus_topic_policy_delete.yml
```

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Artık gerekli değilse, bu makalede oluşturulan kaynakları silin. 

Aşağıdaki kodu @no__t olarak kaydet-0:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      topic: servicebustesttopic
      subscription: servicebustestsubs
  tasks:
    - name: Delete subscription
      azure_rm_servicebustopicsubscription:
          name: "{{ subscription }}"
          topic: "{{ topic }}"
          resource_group: "{{ resource_group }}"
          namespace: "{{ namespace }}"
          state: absent
    - name: Delete topic
      azure_rm_servicebustopic:
          name: "{{ topic }}"
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

@No__t-0 komutunu kullanarak PlayBook 'u çalıştırın:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Azure üzerinde anormal](/azure/ansible/)