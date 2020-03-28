---
title: Öğretici - Ansible kullanarak Azure Kubernetes Hizmetinde (AKS) kubenet ağ larını yapılandırın
description: Azure Kubernetes Hizmeti (AKS) kümesinde kubenet ağlarını yapılandırmak için Ansible'ı nasıl kullanacağınızı öğrenin
keywords: ansible, masmavi, devops, bash, cloudshell, playbook, aks, konteyner, aks, kubernetes
ms.topic: tutorial
ms.date: 10/23/2019
ms.openlocfilehash: bfb19371ad651439c087cebd03023d48852ee2df
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74156885"
---
# <a name="tutorial-configure-kubenet-networking-in-azure-kubernetes-service-aks-using-ansible"></a>Öğretici: Ansible kullanarak Azure Kubernetes Hizmetinde (AKS) kubenet ağ larını yapılandırın

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

AKS kullanarak, aşağıdaki ağ modellerini kullanarak bir küme dağıtabilirsiniz:

- [Kubenet ağ](/azure/aks/configure-kubenet) - Aks kümesi dağıtıldıkça ağ kaynakları genellikle oluşturulur ve yapılandırılır.
- [Azure Kapsayıcı Ağ Arabirimi (CNI) ağ -](/azure/aks/configure-azure-cni) AKS kümesi varolan sanal ağ kaynaklarına ve yapılandırmalarına bağlıdır.

AKS'deki uygulamalarınız için ağ oluşturma hakkında daha fazla bilgi [için, AKS'deki uygulamalar için Ağ kavramlarına](/azure/aks/concepts-network)bakın.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * AKS kümesi oluşturma
> * Azure kubenet ağlarını yapılandırma

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma

Bu bölümdeki oyun kitabı kodu aşağıdaki Azure kaynaklarını oluşturur:

- Sanal ağ
- Sanal ağ içinde alt ağ

Aşağıdaki playbook'u `vnet.yml` olarak kaydedin:

```yml
- name: Create vnet
  azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      address_prefixes_cidr:
          - 10.0.0.0/8

- name: Create subnet
  azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      address_prefix_cidr: 10.240.0.0/16
      virtual_network_name: "{{ name }}"
  register: subnet
```

## <a name="create-an-aks-cluster-in-the-virtual-network"></a>Sanal ağda AKS kümesi oluşturma

Bu bölümdeki oyun kitabı kodu, sanal ağ içinde bir AKS kümesi oluşturur. 

Aşağıdaki playbook'u `aks.yml` olarak kaydedin:

```yml
- name: List supported kubernetes version from Azure
  azure_rm_aks_version:
      location: "{{ location }}"
  register: versions

- name: Create AKS cluster with vnet
  azure_rm_aks:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      dns_prefix: "{{ name }}"
      kubernetes_version: "{{ versions.azure_aks_versions[-1] }}"
      agent_pool_profiles:
        - count: 3
          name: nodepool1
          vm_size: Standard_D2_v2
          vnet_subnet_id: "{{ vnet_subnet_id }}"
      linux_profile:
          admin_username: azureuser
          ssh_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
      service_principal:
          client_id: "{{ lookup('ini', 'client_id section=default file=~/.azure/credentials') }}"
          client_secret: "{{ lookup('ini', 'secret section=default file=~/.azure/credentials') }}"
      network_profile:
          network_plugin: kubenet
          pod_cidr: 192.168.0.0/16
          docker_bridge_cidr: 172.17.0.1/16
          dns_service_ip: 10.0.0.10
          service_cidr: 10.0.0.0/16
  register: aks
```

Örnek oyun kitabıyla çalışırken göz önünde bulundurulması gereken bazı önemli notlar şunlardır:

- Desteklenen `azure_rm_aks_version` sürümü bulmak için modülü kullanın.
- Önceki `vnet_subnet_id` bölümde oluşturulan alt ağdır.
- Kubenet `network_profile` ağ eklentisinin özelliklerini tanımlar.
- Bu, `service_cidr` AKS kümesindeki dahili hizmetleri bir IP adresine atamak için kullanılır. Bu IP adres aralığı, ağınızın başka bir yerinde kullanılmayan bir adres alanı olmalıdır. 
- Adres, `dns_service_ip` hizmet IP adres aralığınızın ".10" adresi olmalıdır.
- Ağ `pod_cidr` ortamınızın başka yerlerinde kullanılmayan büyük bir adres alanı olmalıdır. Adres aralığı, ölçeklendirmeyi beklediğiniz düğüm sayısını karşılayacak kadar büyük olmalıdır. Küme dağıtıldıktan sonra bu adres aralığını değiştiremezsiniz.
- Pod IP adresi aralığı kümedeki her düğüme bir /24 adres alanı atamak için kullanılır. Aşağıdaki örnekte, `pod_cidr` 192.168.0.0/16 ilk düğümü 192.168.0.0/24, ikinci düğüm 192.168.1.0/24 ve üçüncü düğüm 192.168.2.0/24 atar.
- Küme ölçeklendikçe veya yükseltilirken, Azure her yeni düğüme bir pod IP adresi aralığı atamaya devam eder.
- Oyun kitabı `ssh_key` `~/.ssh/id_rsa.pub`yükler . Bunu değiştirirseniz, "ssh-rsa" (tırnak işaretleri olmadan) ile başlayan tek satırlı biçimi kullanın.
- Ve `client_id` `client_secret` `~/.azure/credentials`değerleri, varsayılan kimlik bilgisi dosyası olan birinden yüklenir. Bu değerleri hizmet anabilim ülkenize ayarlayabilir veya bu değerleri ortam değişkenlerinden yükleyebilirsiniz:

    ```yml
    client_id: "{{ lookup('env', 'AZURE_CLIENT_ID') }}"
    client_secret: "{{ lookup('env', 'AZURE_SECRET') }}"
    ```

## <a name="associate-the-network-resources"></a>Ağ kaynaklarını ilişkilendirme

Bir AKS kümesi oluşturduğunuzda, bir ağ güvenlik grubu ve rota tablosu oluşturulur. Bu kaynaklar AKS tarafından yönetilir ve hizmetleri oluşturduğunuzda ve açığa çıkardığınızda güncelleştirilir. Ağ güvenlik grubunu ve rota tablosunu aşağıdaki gibi sanal ağ alt ağınızla ilişkilendirin. 

Aşağıdaki oyun kitabını `associate.yml`' olarak kaydedin.

```yml
- name: Get route table
  azure_rm_routetable_facts:
      resource_group: "{{ node_resource_group }}"
  register: routetable

- name: Get network security group
  azure_rm_securitygroup_facts:
      resource_group: "{{ node_resource_group }}"
  register: nsg

- name: Parse subnet id
  set_fact:
      subnet_name: "{{ vnet_subnet_id | regex_search(subnet_regex, '\\1') }}"
      subnet_rg: "{{ vnet_subnet_id | regex_search(rg_regex, '\\1') }}"
      subnet_vn: "{{ vnet_subnet_id | regex_search(vn_regex, '\\1') }}"
  vars:
      subnet_regex: '/subnets/(.+)'
      rg_regex: '/resourceGroups/(.+?)/'
      vn_regex: '/virtualNetworks/(.+?)/'

- name: Associate network resources with the node subnet
  azure_rm_subnet:
      name: "{{ subnet_name[0] }}"
      resource_group: "{{  subnet_rg[0] }}"
      virtual_network_name: "{{ subnet_vn[0] }}"
      security_group: "{{ nsg.ansible_facts.azure_securitygroups[0].id }}"
      route_table: "{{ routetable.route_tables[0].id }}"
```

Örnek oyun kitabıyla çalışırken göz önünde bulundurulması gereken bazı önemli notlar şunlardır:

- AKS `node_resource_group` düğümlerinin oluşturulduğu kaynak grubu adıdır.
- Önceki `vnet_subnet_id` bölümde oluşturulan alt ağdır.


## <a name="run-the-sample-playbook"></a>Örnek oyun kitabını çalıştırın

Bu bölümde, bu makalede oluşturan görevleri çağıran tam örnek oyun kitabı listelenir. 

Aşağıdaki playbook'u `aks-kubenet.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: aksansibletest
      name: aksansibletest
      location: eastus
  tasks:
     - name: Ensure resource group exist
       azure_rm_resourcegroup:
           name: "{{ resource_group }}"
           location: "{{ location }}"

     - name: Create vnet
       include_tasks: vnet.yml

     - name: Create AKS
       vars:
           vnet_subnet_id: "{{ subnet.state.id }}"
       include_tasks: aks.yml

     - name: Associate network resources with the node subnet
       vars:
           vnet_subnet_id: "{{ subnet.state.id }}"
           node_resource_group: "{{ aks.node_resource_group }}"
       include_tasks: associate.yml

     - name: Get details of the AKS
       azure_rm_aks_facts:
           name: "{{ name }}"
           resource_group: "{{ resource_group }}"
           show_kubeconfig: user
       register: output

     - name: Show AKS cluster detail
       debug:
           var: output.aks[0]
```

`vars` Bölümde aşağıdaki değişiklikleri yapın:

- `resource_group` Anahtar için, değeri `aksansibletest` kaynak grup adınız için değiştirin.
- `name` Anahtar `aksansibletest` için, değeri AKS adınız olarak değiştirin.
- `Location` Anahtar için, değeri `eastus` kaynak grubu konumunuza değiştirin.

Komutu kullanarak oyun `ansible-playbook` kitabının tamamını çalıştırın:

```bash
ansible-playbook aks-kubenet.yml
```

Oyun kitabının çalıştırılması, aşağıdaki çıktıya benzer sonuçlar gösterir:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Ensure resource group exist] 
ok: [localhost]

TASK [Create vnet] 
included: /home/devops/aks-kubenet/vnet.yml for localhost

TASK [Create vnet] 
ok: [localhost]

TASK [Create subnet] 
ok: [localhost]

TASK [Create AKS] 
included: /home/devops/aks-kubenet/aks.yml for localhost

TASK [List supported kubernetes version from Azure] 
 [WARNING]: Azure API profile latest does not define an entry for
ContainerServiceClient

ok: [localhost]

TASK [Create AKS cluster with vnet] 
changed: [localhost]

TASK [Associate network resources with the node subnet] 
included: /home/devops/aks-kubenet/associate.yml for localhost

TASK [Get route table] 
ok: [localhost]

TASK [Get network security group] 
ok: [localhost]

TASK [Parse subnet id] 
ok: [localhost]

TASK [Associate network resources with the node subnet] 
changed: [localhost]

TASK [Get details of the AKS] 
ok: [localhost]

TASK [Show AKS cluster detail] 
ok: [localhost] => {
    "output.aks[0]": {
        "id": /subscriptions/BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB/resourcegroups/aksansibletest/providers/Microsoft.ContainerService/managedClusters/aksansibletest",
        "kube_config": "apiVersion: ...",
        "location": "eastus",
        "name": "aksansibletest",
        "properties": {
            "agentPoolProfiles": [
                {
                    "count": 3,
                    "maxPods": 110,
                    "name": "nodepool1",
                    "osDiskSizeGB": 100,
                    "osType": "Linux",
                    "storageProfile": "ManagedDisks",
                    "vmSize": "Standard_D2_v2",
                    "vnetSubnetID": "/subscriptions/BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB/resourceGroups/aksansibletest/providers/Microsoft.Network/virtualNetworks/aksansibletest/subnets/aksansibletest"
                }
            ],
            "dnsPrefix": "aksansibletest",
            "enableRBAC": false,
            "fqdn": "aksansibletest-cda2b56c.hcp.eastus.azmk8s.io",
            "kubernetesVersion": "1.12.6",
            "linuxProfile": {
                "adminUsername": "azureuser",
                "ssh": {
                    "publicKeys": [
                        {
                            "keyData": "ssh-rsa ..."
                        }
                    ]
                }
            },
            "networkProfile": {
                "dnsServiceIP": "10.0.0.10",
                "dockerBridgeCidr": "172.17.0.1/16",
                "networkPlugin": "kubenet",
                "podCidr": "192.168.0.0/16",
                "serviceCidr": "10.0.0.0/16"
            },
            "nodeResourceGroup": "MC_aksansibletest_pcaksansibletest_eastus",
            "provisioningState": "Succeeded",
            "servicePrincipalProfile": {
                "clientId": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA"
            }
        },
        "type": "Microsoft.ContainerService/ManagedClusters"
    }
}

PLAY RECAP 
localhost                  : ok=15   changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, bu makalede oluşturulan kaynakları silin. 

Aşağıdaki kodu aşağıdaki `cleanup.yml`gibi kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: aksansibletest
  tasks:
      - name: Clean up resource group
        azure_rm_resourcegroup:
            name: "{{ resource_group }}"
            state: absent
            force: yes
```

`vars` Bölümde, yer tutucuyu `{{ resource_group_name }}` kaynak grubunuzun adı ile değiştirin.

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici - Ansible kullanarak AKS'de Azure Kapsayıcı Ağ Arabirimi (CNI) ağlarını yapılandırın](./ansible-aks-configure-cni-networking.md)