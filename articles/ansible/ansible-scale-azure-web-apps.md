---
title: Öğretici - Ansible kullanarak Azure Uygulama Hizmeti'ndeki uygulamaları ölçeklendirin
description: Azure Uygulama Hizmeti'nde bir uygulamayı nasıl ölçeklendireceklerini öğrenin
keywords: ansible, azure, devops, bash, playbook, Azure App Service, Web App, ölçek, Java
ms.topic: tutorial
ms.date: 04/30/2019
ms.openlocfilehash: 9eb50922361c817de8047dece4849a9b221677f0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74155921"
---
# <a name="tutorial-scale-apps-in-azure-app-service-using-ansible"></a>Öğretici: Ansible kullanarak Azure Uygulama Hizmeti'ndeki uygulamaları ölçeklendirin

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-app-service.md](../../includes/open-source-devops-intro-app-service.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Varolan bir Uygulama Hizmeti planının gerçeklerini öğrenin
> * Uygulama Hizmeti planını üç işçiyle Birlikte S2'ye ölçeklendirin

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
- **Azure Uygulama Hizmeti uygulaması** - Azure Uygulama Hizmeti uygulamanız yoksa, [Ansible'ı kullanarak Azure Uygulama Hizmeti'nde bir uygulamayı yapılandırın.](ansible-create-configure-azure-web-apps.md)

## <a name="scale-up-an-app"></a>Uygulamayı ölçeklendir

Ölçeklendirme için iki iş akışı vardır: *ölçeklendirin* ve *ölçeklendirin.*

**Ölçeklendirin:** Ölçeklendirmek, daha fazla kaynak elde etmek anlamına gelir. Bu kaynaklar CPU, bellek, disk alanı, VM'ler ve daha fazlasını içerir. Uygulamanın ait olduğu Uygulama Hizmeti planının fiyatlandırma katmanını değiştirerek bir uygulamayı ölçeklendirirsiniz. 
**Ölçeklendirin:** Ölçeklendirmek, uygulamanızı çalıştıran VM örneklerinin sayısını artırmak anlamına gelir. Uygulama Hizmeti planı fiyatlandırma katmanınıza bağlı olarak, 20 örneğe kadar ölçeklendirebilirsiniz. [Otomatik ölçeklendirme,](/azure/azure-monitor/platform/autoscale-get-started) örnek sayısını önceden tanımlanmış kurallara ve zamanlamalara göre otomatik olarak ölçeklendirmenize olanak tanır.

Bu bölümdeki oyun kitabı kodu aşağıdaki işlemi tanımlar:

* Varolan bir Uygulama Hizmeti planının gerçeklerini öğrenin
* Uygulama hizmet planını üç işçiyle S2'ye güncelleştirin

Aşağıdaki playbook'u `webapp_scaleup.yml` olarak kaydedin:

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_name: myAppServicePlan
    location: eastus

  tasks:
  - name: Get facts of existing App service plan
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku

  - name: Scale up the App service plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      is_linux: true
      sku: S2
      number_of_workers: 3
      
  - name: Get facts
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook webapp_scaleup.yml
```

Oyun kitabını çalıştırdıktan sonra, aşağıdaki sonuçlara benzer çıktı görürsünüz:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Get facts of existing App service plan] 
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

ok: [localhost]

TASK [debug] 
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 1,
        "family": "S",
        "name": "S1",
        "size": "S1",
        "tier": "Standard"
    }
}

TASK [Scale up the App service plan] 
changed: [localhost]

TASK [Get facts] 
ok: [localhost]

TASK [debug] 
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 3,
        "family": "S",
        "name": "S2",
        "size": "S2",
        "tier": "Standard"
    }
}

PLAY RECAP 
localhost                  : ok=6    changed=1    unreachable=0    failed=0 
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](/azure/ansible/)