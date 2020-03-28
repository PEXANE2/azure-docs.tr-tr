---
title: Öğretici - Ansible kullanarak Azure'da otomatik ölçek sanal makine ölçek kümeleri
description: Azure'da otomatik ölçekle sanal makine ölçeği kümelerini ölçeklendirmek için Ansible'ı nasıl kullanacağınızı öğrenin
keywords: ansible, azure, devops, bash, oyun kitabı, ölçek, otomatik ölçek, sanal makine, sanal makine ölçek seti, vmss
ms.topic: tutorial
ms.date: 04/30/2019
ms.openlocfilehash: fb8d2a4bfca32be4575ca8f11018e5cab17cd9a2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74156823"
---
# <a name="tutorial-autoscale-virtual-machine-scale-sets-in-azure-using-ansible"></a>Öğretici: Ansible kullanarak Azure'da otomatik ölçek sanal makine ölçek kümeleri

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-vmss.md](../../includes/open-source-devops-intro-vmss.md)]

VM örneklerinin sayısını otomatik olarak ayarlama özelliğine [otomatik ölçek](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview)denir. Otomatik ölçeklendirmenin yararı, uygulamanızın performansını izlemek ve optimize etmek için yönetim ek yüküazaltıyor olmasıdır. Otomatik ölçek, talebe yanıt olarak veya tanımlı bir zamanlamada yapılandırılabilir. Ansible'ı kullanarak, pozitif bir müşteri deneyimi için kabul edilebilir performansı tanımlayan otomatik ölçeklendirme kurallarını belirtebilirsiniz.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Otomatik ölçeklendirme profilini tanımlama
> * Yinelenen zamanlamaya dayalı otomatik ölçeklendirme
> * Uygulama performansına göre otomatik ölçeklendirme
> * Otomatik ölçek ayarları bilgilerini alma 
> * Otomatik ölçek ayarını devre dışı

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)] 
[!INCLUDE [ansible-prereqs-vm-scale-set.md](../../includes/ansible-prereqs-vm-scale-set.md)]

## <a name="autoscale-based-on-a-schedule"></a>Zamanlamaya dayalı otomatik ölçeklendirme

Bir ölçek kümesinde otomatik ölçeklendirmeyi etkinleştirmek için ilk olarak bir otomatik ölçeklendirme profili tanımlamanız gerekir. Bu profil varsayılan, en düşük ve en yüksek ölçek kümesi kapasitesini tanımlar. Bu sınırlar, sürekli olarak VM örnekleri oluşturmayarak maliyeti denetlemenize ve ölçeklendirme olayında kalan en az sayıda örnekle kabul edilebilir performansı dengelemenize izin verebiliyor. 

Ansible, ölçek kümelerinizi belirli bir tarihe veya yinelenen zamanlamaya göre ölçeklendirmenize olanak tanır.

Bu bölümdeki oyun kitabı kodu, her Pazartesi saat 10:00'da VM örneklerinin sayısını üçe yükseltir.

Aşağıdaki playbook'u `vmss-auto-scale.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myScaleSet
    name: autoscalesetting
  tasks: 
    - name: Create autoscaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         target:
           namespace: "Microsoft.Compute"
           types: "virtualMachineScaleSets"
           name: "{{ vmss_name }}"
         enabled: true
         profiles:
         - count: '3'
           min_count: '3'
           max_count: '3'
           name: Auto created scale condition
           recurrence_timezone: Pacific Standard Time
           recurrence_frequency: Week
           recurrence_days:
              - Monday
           recurrence_mins:
              - '0'
           recurrence_hours:
              - '10'
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook vmss-auto-scale.yml
```

## <a name="autoscale-based-on-performance-data"></a>Performans verilerine dayalı otomatik ölçeklendirme

Uygulama talebiniz artarsa, ölçek kümelerinizdeki VM örneklerindeki yük artar. Bu kısa süreli bir talep olmayıp tutarlı şekilde yük artıyorsa, ölçek kümesindeki sanal makine örneği sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu sanal makine örnekleri oluşturulduğunda ve uygulamalarınız dağıtıldığında ölçek kümesi, yük dengeleyici aracılığıyla bunlara trafiği dağıtmaya başlar. Ansible, CPU kullanımı, disk kullanımı ve uygulama yükleme süresi gibi hangi ölçümlerin izlendiğini denetlemenize olanak tanır. Performans metrik eşiklerine, yinelenen bir zamanlamaya veya belirli bir tarihe göre ölçeklendirilebilir ve ölçeklendirebilirsiniz. 

Bu bölümdeki oyun kitabı kodu, her Pazartesi saat 18:00'de önceki 10 dakikanın CPU iş yükünü denetler. 

CPU yüzdesi ölçümlerine göre, oyun kitabı aşağıdaki eylemlerden birini yapar:

- VM örneklerinin sayısını dörte çıkarır
- VM örneklerinin sayısındaki ölçekler bir

Aşağıdaki playbook'u `vmss-auto-scale-metrics.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myScaleSet
    name: autoscalesetting
  tasks:
  - name: Get facts of the resource group
    azure_rm_resourcegroup_facts:
      name: "{{ resource_group }}"
    register: rg

  - name: Get scale set resource uri
    set_fact:
      vmss_id: "{{ rg.ansible_facts.azure_resourcegroups[0].id }}/providers/Microsoft.Compute/virtualMachineScaleSets/{{ vmss_name }}"
    
  - name: Create autoscaling
    azure_rm_autoscale:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      target: "{{ vmss_id }}"
      enabled: true
      profiles:
      - count: '1'
        max_count: '1'
        min_count: '1'
        name: 'This scale condition is executed when none of the other scale condition(s) match'
        recurrence_days:
          - Monday
        recurrence_frequency: Week
        recurrence_hours:
          - 18
        recurrence_mins:
          - 0
        recurrence_timezone: Pacific Standard Time
      - count: '1'
        min_count: '1'
        max_count: '4'
        name: Auto created scale condition
        recurrence_days:
          - Monday
        recurrence_frequency: Week
        recurrence_hours:
          - 18
        recurrence_mins:
          - 0
        recurrence_timezone: Pacific Standard Time
        rules:
          - cooldown: 5
            direction: Increase
            metric_name: Percentage CPU
            metric_resource_uri: "{{ vmss_id }}"
            operator: GreaterThan
            statistic: Average
            threshold: 70
            time_aggregation: Average
            time_grain: 1
            time_window: 10
            type: ChangeCount
            value: '1'
          - cooldown: 5
            direction: Decrease
            metric_name: Percentage CPU
            metric_resource_uri: "{{ vmss_id }}"
            operator: LessThan
            statistic: Average
            threshold: 30
            time_aggregation: Average
            time_grain: 1
            time_window: 10
            type: ChangeCount
            value: '1'
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook vmss-auto-scale-metrics.yml
```

## <a name="get-autoscale-settings-information"></a>Otomatik ölçek ayarları bilgileri alın 

Bu bölümdeki oyun kitabı `azure_rm_autoscale_facts` kodu, otomatik ölçek ayarınayrıntılarını almak için modülü kullanır.

Aşağıdaki playbook'u `vmss-auto-scale-get-settings.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Retrieve autoscale settings information
      azure_rm_autoscale_facts:
        resource_group: "{{ resource_group }}"
        name: "{{ name }}"
      register: autoscale_query
    
    - debug:
        var: autoscale_query.autoscales[0]
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook vmss-auto-scale-get-settings.yml
```

## <a name="disable-autoscale-settings"></a>Otomatik ölçek ayarlarını devre dışı

Otomatik ölçeklendirme ayarlarını devre dışı betmenin iki yolu vardır. Bir yolu `enabled` için anahtarı `true` değiştirmektir `false`. İkinci yol ayarı silmektir.

Bu bölümdeki oyun kitabı kodu otomatik ölçek ayarını siler. 

Aşağıdaki playbook'u `vmss-auto-scale-delete-setting.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Delete autoscaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         state: absent
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
vmss-auto-scale-delete-setting.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Öğretici: Ansible kullanarak Azure sanal makine ölçek kümelerinin özel görüntüsünü güncelleştirin](./ansible-vmss-update-image.md)