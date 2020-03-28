---
title: Öğretici - Ansible kullanarak Redis için Azure Önbelleğinde önbellekleri yapılandırın
description: Redis için Azure Önbelleği'ne güvenlik duvarı kuralı oluşturmak, ölçeklendirmek, yeniden başlatmak ve eklemek için Ansible'ı nasıl kullanacağınızı öğrenin
keywords: ansible, masmavi, devops, bash, oyun kitabı, önbellek, redis
ms.topic: tutorial
ms.date: 04/30/2019
ms.openlocfilehash: 2ef36ee9e3601d77bfa114b903f6a75b5874b158
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74156518"
---
# <a name="tutorial-configure-caches-in-azure-cache-for-redis-using-ansible"></a>Öğretici: Ansible kullanarak Redis için Azure Önbelleğinde önbellekleri yapılandırın

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[Redis için Azure Önbelleği,](/azure/azure-cache-for-redis/) verilere hızlı erişim sağlayarak duyarlı uygulamalar oluşturmanıza olanak tanıyan açık kaynak uyumlu bir hizmettir. 

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Bir önbellek oluşturma
> * Önbelleği ölçeklendir
> * Önbelleği yeniden başlatma
> * Önbelleğe güvenlik duvarı kuralı ekleme
> * Önbelleği silme

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-cache"></a>Bir önbellek oluşturma

Yeni bir kaynak grubunda Redis için bir Azure Önbelleği oluşturun.

```yml
  - name: Create resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"

  - name: Create Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      sku:
        name: basic
        size: C1 
```

Önbelleği sağlamak birkaç dakika sürebilir. Aşağıdaki kod, Ansible'a işlemin tamamlanmasını beklemesini söyler:

```yml
  - name: Wait for Redis provisioning to complete
    azure_rm_rediscache_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
    register: facts
    until: "{{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}"
    retries: 100
    delay: 60
```

Uzun sağlama işlemi sırasında, birkaç "hata" iletisi görüntülenir. Bu iletiler güvenle yoksayılabilir. Önemli mesaj son mesajdır. Aşağıdaki örnekte, son ("tamam") iletiye kadar birçok hata iletisi vardır.

```Output
FAILED - RETRYING: Get facts (100 retries left).
FAILED - RETRYING: Get facts (99 retries left).
FAILED - RETRYING: Get facts (98 retries left).
FAILED - RETRYING: Get facts (97 retries left).
FAILED - RETRYING: Get facts (96 retries left).
FAILED - RETRYING: Get facts (95 retries left).
FAILED - RETRYING: Get facts (94 retries left).
FAILED - RETRYING: Get facts (93 retries left).
FAILED - RETRYING: Get facts (92 retries left).
FAILED - RETRYING: Get facts (91 retries left).
ok: [localhost]
```

## <a name="scale-the-cache"></a>Önbelleği ölçeklendirin

Redis için Azure Önbelleği, uygulamanızın gereksinimlerine bağlı olarak farklı önbellek tekliflerine sahiptir. Bu önbellek seçenekleri, önbellek boyutu ve özellikleri seçiminde esneklik sağlar. Önbellek oluşturulduktan sonra uygulama gereksinimleriniz değişirse, önbelleği gerektiği gibi ölçeklendirebilirsiniz. Ölçeklendirme hakkında daha fazla bilgi için [Redis için Azure Önbelleğini niçin ölçeklendireceğini](/azure/azure-cache-for-redis/cache-how-to-scale)öğrenin.

Aşağıdaki örnek kod önbelleği **Standart**olarak ölçeklendirin:

```yml
- name: Scale up Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      sku:
        name: standard
        size: C1
```

Önbelleği ölçeklendirmek birkaç dakika sürebilir. Aşağıdaki kod, Ansible'a işlemin tamamlanmasını beklemesini söyler:

```yml
  - name: Wait for Redis scaling up to complete
    azure_rm_rediscache_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
    register: facts
    until: "{{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}"
    retries: 100
    delay: 60
```

Redis için Azure Önbelleği sağlama görevine benzer şekilde, aşağıdaki ileti gibi çıktı normaldir:

```Ouput
**FAILED - RETRYING: Get facts (100 retries left)** is normal.
```

## <a name="reboot-the-cache"></a>Önbelleği yeniden başlatın

Aşağıdaki kod önceki bölümlerde oluşturulan önbelleği yeniden başlatAbilir.

```yml
  - name: Reboot Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      reboot:
        reboot_type: all
```

### <a name="add-firewall-rule"></a>Güvenlik duvarı kuralı ekleme

Aşağıdaki kod önbelleğe bir güvenlik duvarı kuralı ekler:

```yml
  - name: Add Firewall rule
    azure_rm_rediscachefirewallrule:
      resource_group: "{{ resource_group }}"
      cache_name: "{{ redis_name }}"
      name: rule1
      start_ip_address: 168.1.1.1
      end_ip_address: 168.1.1.4
```

## <a name="delete-the-cache"></a>Önbelleği silme

Aşağıdaki kod önbelleği siler:

```yml
  - name: Delete Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      state: absent
```

## <a name="get-the-sample-playbook"></a>Örnek oyun kitabını alın

Tam örnek oyun kitabını almanın iki yolu vardır:
- [Oyun kitabını indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/rediscache.yml) ve `rediscache.yml`'ye kaydedin.
- Adlandırılmış `rediscache.yml` yeni bir dosya oluşturun ve aşağıdaki içeriği kopyalayın:

```yml
- name: Manage Azure Cache for Redis
  hosts: localhost
  connection: local
  vars:
    resource_group: "{{ resource_group_name }}"
    redis_name: "redis{{ resource_group_name }}"
    location: eastus2

  roles:
    - azure.azure_preview_modules

  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"

  - name: Create Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      sku:
        name: basic
        size: C1

  - name: Wait for Redis provisioning to complete
    azure_rm_rediscache_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
    register: facts
    until: "{{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}"
    retries: 100
    delay: 60

  - name: Scale up Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      sku:
        name: standard
        size: C1

  - name: Wait for Redis scaling up to complete
    azure_rm_rediscache_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
    register: facts
    until: "{{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}"
    retries: 100
    delay: 60

  - name: Reboot Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      reboot:
        reboot_type: all

  - name: Add Firewall rule
    azure_rm_rediscachefirewallrule:
      resource_group: "{{ resource_group }}"
      cache_name: "{{ redis_name }}"
      name: rule1
      start_ip_address: 168.1.1.1
      end_ip_address: 168.1.1.4

  - name: Delete Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      state: absent
```

## <a name="run-the-sample-playbook"></a>Örnek oyun kitabını çalıştırın

Bu bölümde, bu makalede gösterilen çeşitli özellikleri test etmek için oyun kitabını çalıştırın.

`vars` Bölümde, yer tutucuyu `{{ resource_group_name }}` kaynak grubunuzun adı ile değiştirin.

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook rediscache.yml
```

Çıktı aşağıdaki sonuçlara benzer görünür:

```Output
TASK [create resource group] 
Tuesday 12 March 2019  16:21:07 +0800 (0:00:00.054)       0:00:01.503 
ok: [localhost]

TASK [Create Azure Cache for Redis] 
Tuesday 12 March 2019  16:21:09 +0800 (0:00:01.950)       0:00:03.454 
 [WARNING]: Azure API profile latest does not define an entry for RedisManagementClient

changed: [localhost]

TASK [Dump host name] 
Tuesday 12 March 2019  16:21:49 +0800 (0:00:40.125)       0:00:43.580 
ok: [localhost] =>
  output['host_name']: redis0312.redis.cache.windows.net

TASK [Get facts] 
Tuesday 12 March 2019  16:21:49 +0800 (0:00:00.056)       0:00:43.636 
 [WARNING]: conditional statements should not include jinja2 templating delimiters such as {{ }} or {% %}. Found: {{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}

FAILED - RETRYING: Get facts (100 retries left).
FAILED - RETRYING: Get facts (99 retries left).
FAILED - RETRYING: Get facts (98 retries left).
FAILED - RETRYING: Get facts (97 retries left).
FAILED - RETRYING: Get facts (96 retries left).
FAILED - RETRYING: Get facts (95 retries left).
FAILED - RETRYING: Get facts (94 retries left).
FAILED - RETRYING: Get facts (93 retries left).
FAILED - RETRYING: Get facts (92 retries left).
FAILED - RETRYING: Get facts (91 retries left).
FAILED - RETRYING: Get facts (90 retries left).
ok: [localhost]

TASK [Scale up Azure Cache for Redis] 
Tuesday 12 March 2019  16:33:20 +0800 (0:11:31.296)       0:12:14.933 
changed: [localhost]

TASK [Get facts] 
Tuesday 12 March 2019  16:33:29 +0800 (0:00:09.164)       0:12:24.097 
 [WARNING]: conditional statements should not include jinja2 templating delimiters such as {{ }} or {% %}. Found: {{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}

FAILED - RETRYING: Get facts (100 retries left).
FAILED - RETRYING: Get facts (99 retries left).
FAILED - RETRYING: Get facts (98 retries left).
FAILED - RETRYING: Get facts (97 retries left).
FAILED - RETRYING: Get facts (96 retries left).
FAILED - RETRYING: Get facts (95 retries left).
FAILED - RETRYING: Get facts (94 retries left).
FAILED - RETRYING: Get facts (93 retries left).
FAILED - RETRYING: Get facts (92 retries left).
FAILED - RETRYING: Get facts (91 retries left).
ok: [localhost]

TASK [Reboot Azure Cache for Redis] 
Tuesday 12 March 2019  16:43:57 +0800 (0:10:27.740)       0:22:51.838 
ok: [localhost]

TASK [Add Firewall rule] 
Tuesday 12 March 2019  16:44:02 +0800 (0:00:05.432)       0:22:57.271 
changed: [localhost]

TASK [Delete Azure Cache for Redis] 
Tuesday 12 March 2019  16:44:08 +0800 (0:00:05.137)       0:23:02.409 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=10   changed=4    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

Tuesday 12 March 2019  16:44:14 +0800 (0:00:06.217)       0:23:08.626 

```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, bu makalede oluşturulan kaynakları silin. 

Aşağıdaki kodu aşağıdaki `cleanup.yml`gibi kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: "{{ resource_group_name }}"
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

`vars` Bölümde, yer tutucuyu `{{ resource_group_name }}` kaynak grubunuzun adı ile değiştirin.

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](https://docs.microsoft.com/azure/ansible/)