---
title: Öğretici - Ansible kullanarak Azure Paylaşılan Resim Galerisi'nden VM veya sanal makine ölçeği seti oluşturun
description: Paylaşılan Resim Galerisi'nde genelleştirilmiş bir görüntüye dayalı VM veya sanal makine ölçeği kümesi oluşturmak için Ansible'ı nasıl kullanacağınızı öğrenin.
keywords: ansible, azure, devops, bash, playbook, sanal makine, sanal makine ölçek seti, paylaşılan resim galerisi
ms.topic: tutorial
ms.date: 10/14/2019
ms.openlocfilehash: f784419736854095cc1bc5da14f3867ac3f7eb12
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74155829"
---
# <a name="tutorial-create-a-vm-or-virtual-machine-scale-set-from-the-azure-shared-image-gallery-using-ansible"></a>Öğretici: Ansible kullanarak Azure Paylaşılan Resim Galerisi'nden VM veya sanal makine ölçeği seti oluşturun

[!INCLUDE [ansible-29-note.md](../../includes/ansible-29-note.md)]

[Paylaşılan Resim Galerisi,](/azure/virtual-machines/windows/shared-image-galleries) özel olarak yönetilen görüntüleri kolayca yönetmenize, paylaşmanıza ve düzenlemenize olanak tanıyan bir hizmettir. Bu özellik, birçok görüntükorunduğu ve paylaşıldığı senaryolar için yararlıdır. Özel görüntüler abonelikler arasında ve Azure Etkin Dizin kiracıları arasında paylaşılabilir. Görüntüler, daha hızlı dağıtım ölçekleme için birden çok bölgeye çoğaltılabilir.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Genelleştirilmiş VM ve özel görüntü oluşturma
> * Paylaşılan Resim Galerisi Oluşturma
> * Paylaşılan bir resim ve resim sürümü oluşturma
> * Genelleştirilmiş görüntüyü kullanarak VM oluşturma
> * Genelleştirilmiş görüntüyü kullanarak sanal makine ölçeği kümesi oluşturma
> * Paylaşılan Resim Galeriniz, resminiz ve sürümünüz hakkında bilgi alın.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="get-the-sample-playbooks"></a>Örnek oyun kitaplarını alın

Örnek oyun kitaplarının tüm kümesini almanın iki yolu vardır:

- [SIG klasörünü indirin](https://github.com/Azure-Samples/ansible-playbooks/tree/master/SIG_generalized_image) ve yerel makinenize kaydedin.
- Her bölüm için yeni bir dosya oluşturun ve buradaki örnek oyun kitabını kopyalayın.

Dosya, `vars.yml` bu öğretici için tüm örnek oyun kitapları tarafından kullanılan değişkenleri içerir. Benzersiz adlar ve değerler sağlamak için dosyayı değiştirebilirsiniz.

İlk örnek oyun `00-prerequisites.yml` kitabı, bu öğreticiyi tamamlamak için gerekenleri oluşturur:
- Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir kapsayıcı olan kaynak grubu.
- Sanal ağ; alt ağ; VM için genel IP adresi ve ağ arabirim kartı.
- Genelleştirilmiş görüntü oluşturmak için kullanılan bir kaynak Sanal Makine.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
    - name: Create resource group if doesn't exist
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
    
    - name: Create virtual network
      azure_rm_virtualnetwork:
        resource_group: "{{ resource_group }}"
        name: "{{ virtual_network_name }}"
        address_prefixes: "10.0.0.0/16"

    - name: Add subnet
      azure_rm_subnet:
        resource_group: "{{ resource_group }}"
        name: "{{ subnet_name }}"
        address_prefix: "10.0.1.0/24"
        virtual_network: "{{ virtual_network_name }}"

    - name: Create public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}"
        allocation_method: Static
        name: "{{ ip_name }}"

    - name: Create virtual network inteface cards for VM A and B
      azure_rm_networkinterface:
        resource_group: "{{ resource_group }}"
        name: "{{ network_interface_name }}"
        virtual_network: "{{ virtual_network_name }}"
        subnet: "{{ subnet_name }}"

    - name: Create VM
      azure_rm_virtualmachine:
        resource_group: "{{ resource_group }}"
        name: "{{ source_vm_name }}"
        admin_username: testuser
        admin_password: "Password1234!"
        vm_size: Standard_B1ms
        network_interfaces: "{{ network_interface_name }}"
        image:
          offer: UbuntuServer
          publisher: Canonical
          sku: 16.04-LTS
          version: latest
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook 00-prerequisites.yml
```

Azure [portalında,](https://portal.azure.com)yeni sanal makineyi `vars.yml` ve oluşturduğunuz çeşitli kaynakları görmek için belirttiğiniz kaynak grubunu denetleyin.

## <a name="generalize-the-vm-and-create-a-custom-image"></a>VM'yi genelleştirin ve özel bir görüntü oluşturun

Sonraki oyun kitabı, `01a-create-generalized-image.yml`önceki adımda oluşturulan kaynak VM genelleştirir ve daha sonra buna dayalı özel bir görüntü oluşturur.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
    - name: Generalize VM
      azure_rm_virtualmachine:
        resource_group: "{{ resource_group }}"
        name: "{{ source_vm_name }}"
        generalized: yes

    - name: Create custom image
      azure_rm_image:
        resource_group: "{{ resource_group }}"
        name: "{{ image_name }}"
        source: "{{ source_vm_name }}"
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook 01a-create-generalized-image.yml
```

Kaynak grubunuzu kontrol `testimagea` edin ve görünenden emin olun.

## <a name="create-the-shared-image-gallery"></a>Paylaşılan Resim Galerisini Oluşturma

Resim galerisi, görüntüleri paylaşmak ve yönetmek için bir depodur. Örnek oyun kitabı `02-create-shared-image-gallery.yml` kodu, kaynak grubunuzda Paylaşılan Resim Galerisi oluşturur.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
    - name: Create shared image gallery
      azure_rm_gallery:
        resource_group: "{{ resource_group }}"
        name: "{{ shared_gallery_name }}"
        location: "{{ location }}"
        description: This is the gallery description.
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook 02-create-shared-image-gallery.yml
```

Şimdi kaynak grubunuzda `myGallery`yeni bir galeri görüyorsunuz.

## <a name="create-a-shared-image-and-image-version"></a>Paylaşılan bir resim ve resim sürümü oluşturma

Sonraki oyun kitabı, `03a-create-shared-image-generalized.yml` bir resim tanımı ve bir görüntü sürümü oluşturur.

Resim tanımları görüntü türünü (Windows veya Linux), sürüm notlarını ve minimum ve maksimum bellek gereksinimlerini içerir. Resim sürümü, görüntünün sürümüdür. Galeri, resim tanımı ve resim sürümü, görüntüleri mantıksal gruplar halinde düzenlemenize yardımcı olur. 

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
    - name: Create shared image
      azure_rm_galleryimage:
        resource_group: "{{ resource_group }}"
        gallery_name: "{{ shared_gallery_name }}"
        name: "{{ shared_image_name }}"
        location: "{{ location }}"
        os_type: linux
        os_state: generalized
        identifier:
          publisher: myPublisherName
          offer: myOfferName
          sku: mySkuName
        description: Image description
    
    - name: Create or update a simple gallery image version.
      azure_rm_galleryimageversion:
        resource_group: "{{ resource_group }}"
        gallery_name: "{{ shared_gallery_name }}"
        gallery_image_name: "{{ shared_image_name }}"
        name: "{{ shared_image_version }}"
        location: "{{ location }}"
        publishing_profile:
          end_of_life_date: "2020-10-01t00:00:00+00:00"
          exclude_from_latest: yes
          replica_count: 3
          storage_account_type: Standard_LRS
          target_regions:
            - name: West US
              regional_replica_count: 1
            - name: East US
              regional_replica_count: 2
              storage_account_type: Standard_ZRS
          managed_image:
            name: "{{ image_name }}"
            resource_group: "{{ resource_group }}"
      register: output

    - debug:
        var: output
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook 03a-create-shared-image-generalized.yml
```

Kaynak grubunuzun artık galeriniz için bir resim tanımı ve resim sürümü var.

## <a name="create-a-vm-based-on-the-generalized-image"></a>Genelleştirilmiş görüntüye dayalı bir VM oluşturma

Son olarak, önceki adımda oluşturduğunuz genelleştirilmiş görüntüye dayalı bir VM oluşturmak için çalıştırın. `04a-create-vm-using-generalized-image.yml`

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
  - name: Create VM using shared image
    azure_rm_virtualmachine:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}"
      vm_size: Standard_DS1_v2
      admin_username: adminUser
      admin_password: PassWord01
      managed_disk_type: Standard_LRS
      image:
        id: "/subscriptions/{{ lookup('env', 'AZURE_SUBSCRIPTION_ID') }}/resourceGroups/{{ resource_group }}/providers/Microsoft.Compute/galleries/{{ shared_gallery_name }}/images/{{ shared_image_name }}/versions/{{ shared_image_version }}"
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook 04a-create-vm-using-generalized-image.yml
```

## <a name="create-a-virtual-machine-scale-sets-based-on-the-generalized-image"></a>Genelleştirilmiş görüntüye dayalı sanal makine ölçeği kümeleri oluşturma

Ayrıca, genelleştirilmiş görüntüye dayalı sanal bir makine ölçeği kümesi de oluşturabilirsiniz. Bunu `05a-create-vmss-using-generalized-image.yml` yapmak için koş.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
  - name: Create a virtual machine scale set using a shared image
    azure_rm_virtualmachinescaleset:
      resource_group: "{{ resource_group }}"
      name: "{{ vmss_name }}"
      vm_size: Standard_DS1_v2
      admin_username: adminUser
      admin_password: PassWord01
      capacity: 2
      virtual_network_name: "{{ virtual_network_name }}"
      upgrade_policy: Manual
      subnet_name: "{{ subnet_name }}"
      managed_disk_type: Standard_LRS
      image:
        id: "/subscriptions/{{ lookup('env', 'AZURE_SUBSCRIPTION_ID') }}/resourceGroups/{{ resource_group }}/providers/Microsoft.Compute/galleries/{{ shared_gallery_name }}/images/{{ shared_image_name }}/versions/{{ shared_image_version }}"
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook 05a-create-vmss-using-generalized-image.yml
```

## <a name="get-information-about-the-gallery"></a>Galeri hakkında bilgi alın

Galeri, resim tanımı ve sürüm `06-get-info.yml`hakkında bilgi alarak.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
  - name: Get Shared Image Gallery information
    azure_rm_gallery_info:
      resource_group: "{{ resource_group }}"
      name: "{{ shared_gallery_name }}"
  - name: Get shared image information
    azure_rm_galleryimage_info:
      resource_group: "{{ resource_group }}"
      gallery_name: "{{ shared_gallery_name }}"
      name: "{{ shared_image_name }}"
  - name: Get Shared Image Gallery image version information
    azure_rm_galleryimageversion_info:
      resource_group: "{{ resource_group }}"
      gallery_name: "{{ shared_gallery_name }}"
      gallery_image_name: "{{ shared_image_name }}"
      name: "{{ shared_image_version }}"
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook 06-get-info.yml
```

## <a name="delete-the-shared-image"></a>Paylaşılan görüntüyü silme

Galeri kaynaklarını silmek için örnek `07-delete-gallery.yml`oyun kitabına bakın. Kaynakları ters sırada silin. Resim sürümünü silerek başlayın. Tüm resim sürümlerini sildikten sonra, resim tanımını silebilirsiniz. Tüm resim tanımlarını sildikten sonra galeriyi silebilirsiniz.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
  - name: Delete gallery image version.
    azure_rm_galleryimageversion:
      resource_group: "{{ resource_group }}"
      gallery_name: "{{ shared_gallery_name }}"
      gallery_image_name: "{{ shared_image_name }}"
      name: "{{ shared_image_version }}"
      state: absent

  - name: Delete gallery image
    azure_rm_galleryimage:
      resource_group: "{{ resource_group }}"
      gallery_name: "{{ shared_gallery_name }}"
      name: "{{ shared_image_name }}"
      state: absent

  - name: Delete a simple gallery.
    azure_rm_gallery:
      resource_group: "{{ resource_group }}"
      name: "{{ shared_gallery_name }}"
      state: absent
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook 07-delete-gallery.yml
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, bu makalede oluşturulan kaynakları silin. 

Bu bölümdeki örnek oyun kitabı kodu aşağıdakileri yapmak için kullanılır:

- Daha önce oluşturulan iki kaynak grubuna silme

Aşağıdaki playbook'u `cleanup.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: "{{ resource_group_name }}"
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        force_delete_nonempty: yes
        state: absent
```

Örnek oyun kitabıyla çalışırken göz önünde bulundurulması gereken bazı önemli notlar şunlardır:

- Yer `{{ resource_group_name }}` tutucuyu kaynak grubunuzun adı ile değiştirin.
- Belirtilen iki kaynak grubundaki tüm kaynaklar silinir.

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](/azure/ansible/)