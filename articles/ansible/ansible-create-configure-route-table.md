---
title: Öğretici - Ansible kullanarak Azure rota tablolarını yapılandırın
description: Ansible'ı kullanarak Azure rota tablolarını nasıl oluşturup yöneteceknizi ve sileyin öğrenin. Ayrıca rotaoluşturmayı ve silmeyi de öğrenin.
keywords: ansible, azure, devops, bash, oyun kitabı, ağ, rota, rota tablosu
ms.topic: tutorial
ms.date: 04/30/2019
ms.openlocfilehash: 1f08aebe7e9dcc1c5687f50ac91c7cb8cc8a62eb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75659806"
---
# <a name="tutorial-configure-azure-route-tables-using-ansible"></a>Öğretici: Ansible kullanarak Azure rota tablolarını yapılandırın

[!INCLUDE [ansible-27-note.md](../../includes/ansible-28-note.md)]

Azure, Azure alt ağları, sanal ağlar ve şirket içi ağlar arasındaki trafiği otomatik olarak yönlendirir. Ortamınızın yönlendirmesi üzerinde daha fazla denetime ihtiyacınız varsa, bir [rota tablosu](/azure/virtual-network/virtual-networks-udr-overview)oluşturabilirsiniz. 

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> Rota tablosu oluşturma Sanal ağ ve alt ağ ile bir rota tablosunu alt ağdan ilişkilendirin Rota tablosunu bir alt ağdan oluştur ve yolları sil Bir rota tablosunu silme

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Bu bölümdeki oyun kitabı kodu bir rota tablosu oluşturur. Rota tablosu sınırları hakkında bilgi için [Azure sınırlarına](/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-resource-manager-virtual-networking-limits)bakın. 

Aşağıdaki playbook'u `route_table_create.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create a route table
      azure_rm_routetable:
        name: "{{ route_table_name }}"
        resource_group: "{{ resource_group }}"
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook route_table_create.yml
```

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bu bölümdeki oyun kitabı kodu:

* Sanal ağ oluşturur
* Sanal ağ içinde bir alt ağ oluşturur
* Bir rota tablosunu alt ağa ilişkilendirir

Rota tabloları sanal ağlarla ilişkili değildir. Bunun yerine, rota tabloları sanal bir ağın alt ağıyla ilişkilidir.

Sanal ağ ve rota tablosu aynı Azure konumu nda ve aboneliğinde bir arada bulunmalıdır.

Alt ağlar ve rota tablolarının bir-çok ilişkisi vardır. Bir alt ağ ilişkili rota tablosu veya bir rota tablosu olmadan tanımlanabilir. Rota tabloları hiçbir, bir veya çok alt ağile ilişkilendirilebilir. 

Alt ağdan gelen trafik aşağıdakilere göre yönlendirilir:

- rota tabloları içinde tanımlanan rotalar
- [varsayılan rotalar](/azure/virtual-network/virtual-networks-udr-overview#default)
- şirket içi bir ağdan yayılan rotalar

Sanal ağ bir Azure sanal ağ ağ geçidine bağlı olmalıdır. Ağ geçidi, VPN ağ geçidi olan BGP kullanıyorsa ExpressRoute veya VPN olabilir.

Aşağıdaki playbook'u `route_table_associate.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    subnet_name: mySubnet
    virtual_network_name: myVirtualNetwork 
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create virtual network
      azure_rm_virtualnetwork:
        name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
        - 10.1.0.0/16
        - 172.100.0.0/16
        dns_servers:
        - 127.0.0.1
        - 127.0.0.3
    - name: Create a subnet with route table
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: "10.1.0.0/24"
        route_table: "{ route_table_name }"
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook route_table_associate.yml
```

## <a name="dissociate-a-route-table-from-a-subnet"></a>Bir rota tablosunu alt ağdan ayırma

Bu bölümdeki oyun kitabı kodu bir alt ağdan bir rota tablosunu ayırıyor.

Bir rota tablosunu alt ağdan ayrıştırırken, alt `route_table` ağ `None`için '' için 

Aşağıdaki playbook'u `route_table_dissociate.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    subnet_name: mySubnet
    virtual_network_name: myVirtualNetwork 
    resource_group: myResourceGroup
  tasks:
    - name: Dissociate a route table
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: "10.1.0.0/24"
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook route_table_dissociate.yml
```

## <a name="create-a-route"></a>Yönlendirme oluşturma

Bu bölümdeki oyun kitabı kodu, bir rota tablosu içindeki bir rotadır. 

Aşağıdaki playbook'u `route_create.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    route_name: myRoute
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create route
      azure_rm_route:
        name: "{{ route_name }}"
        resource_group: "{{ resource_group }}"
        next_hop_type: virtual_network_gateway
        address_prefix: "10.1.0.0/16"
        route_table_name: "{{ route_table_name }}"
```

Oyun kitabını çalıştırmadan önce aşağıdaki notalara bakın:

* `virtual_network_gateway`olarak `next_hop_type`tanımlanır. Azure'un yolları nasıl seçtiği hakkında daha fazla bilgi için [Yönlendirme'ye genel bakış](/azure/virtual-network/virtual-networks-udr-overview)bilgisine bakın.
* `address_prefix`olarak `10.1.0.0/16`tanımlanır. Önek rota tablosu içinde çoğaltılamaz.

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook route_create.yml
```

## <a name="delete-a-route"></a>Rotayı silme

Bu bölümdeki oyun kitabı kodu rota tablosundan bir rotayı siler.

Aşağıdaki playbook'u `route_delete.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    route_name: myRoute
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Remove route
      azure_rm_route:
        name: "{{ route_name }}"
        resource_group: "{{ resource_group }}"
        route_table_name: "{{ route_table_name }}"
        state: absent
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook route_delete.yml
```

## <a name="get-route-table-information"></a>Rota tablosu bilgilerini alma

Bu bölümdeki oyun kitabı kodu, rota `azure_rm_routetable_facts` tablosu bilgilerini almak için Ansible modüllerini kullanır.

Aşağıdaki playbook'u `route_table_facts.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks: 
    - name: Get route table information
      azure_rm_routetable_facts:
         resource_group: "{{ resource_group }}"
         name: "{{ route_table_name }}"
      register: query
    
    - debug:
         var: query.route_tables[0]
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook route_table_facts.yml
```

## <a name="delete-a-route-table"></a>Rota tablosunu silme

Bu bölümdeki oyun kitabı kodu bir rota tablosu.

Bir rota tablosu silindiğinde, tüm yolları da silinir.

Bir alt ağla ilişkiliyse, rota tablosu silinemez. Rota tablosunu silmeye çalışmadan önce [rota tablosunu herhangi bir alt ağdan](#dissociate-a-route-table-from-a-subnet) ayırın. 

Aşağıdaki playbook'u `route_table_delete.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create a route table
      azure_rm_routetable:
        name: "{{ route_table_name }}"
        resource_group: "{{ resource_group }}"
        state: absent
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook route_table_delete.yml
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](/azure/ansible/)