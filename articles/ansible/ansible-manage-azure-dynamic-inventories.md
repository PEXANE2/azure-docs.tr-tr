---
title: Öğretici - Ansible kullanarak Azure kaynaklarınızın dinamik envanterlerini yapılandırın
description: Azure dinamik envanterlerinizi yönetmek için Ansible'ı nasıl kullanacağınızı öğrenin
keywords: ansible, azure, devops, bash, cloudshell, dinamik envanter
ms.topic: tutorial
ms.date: 10/23/2019
ms.openlocfilehash: cd225dcf8a0c307d49e985817b71c491559edb14
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78247851"
---
# <a name="tutorial-configure-dynamic-inventories-of-your-azure-resources-using-ansible"></a>Öğretici: Ansible kullanarak Azure kaynaklarınızın dinamik envanterlerini yapılandırın

Ansible, çeşitli kaynaklardan (Azure gibi bulut kaynakları dahil) envanter bilgilerini dinamik bir *envantere*çekmek için kullanılabilir. 

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * İki test sanal makinesini yapılandırın. 
> * Sanal makinelerden birini etiketle
> * Nginx'i etiketli sanal makinelere yükleyin
> * Yapılandırılan Azure kaynaklarını içeren dinamik bir envanter imal edin

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-the-test-vms"></a>Test VM'lerini oluşturma

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.

1. [Açık Bulut Kabuk](https://docs.microsoft.com/azure/cloud-shell/overview).

1. Bu öğretici için sanal makineleri tutmak için bir Azure kaynak grubu oluşturun.

    > [!IMPORTANT]  
    > Bu adımda oluşturduğunuz Azure kaynak grubunun tamamen küçük harfli bir adı olmalıdır. Aksi takdirde, dinamik envanterin üretimi başarısız olur.

    ```azurecli-interactive
    az group create --resource-group ansible-inventory-test-rg --location eastus
    ```

1. Aşağıdaki tekniklerden birini kullanarak Azure'da iki Linux sanal makinesi oluşturun:

    - **Ansible playbook** - Makale, [Ansible ile Azure'da temel bir sanal makine oluşturun,](./ansible-create-vm.md) Ansible oyun kitabından sanal bir makinenin nasıl oluşturulabildiğini göstermektedir. Sanal makinelerden birini veya her ikisini tanımlamak için bir oyun kitabı kullanıyorsanız, parola yerine SSH bağlantısının kullanıldığından emin olun.

    - **Azure CLI** - İki sanal makine oluşturmak için Bulut Kabuğu'ndaki aşağıdaki komutların her birini sorun:

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm1 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm2 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

## <a name="tag-a-vm"></a>Bir VM’yi etiketleme

Azure kaynaklarınızı kullanıcı tanımlı kategorilere göre [düzenlemek için etiketleri kullanabilirsiniz.](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#azure-cli) 

### <a name="using-ansible-version--28"></a>2.8< Ansible sürümünü kullanma
Anahtar ile sanal makine `ansible-inventory-test-vm1` etiketlemek için aşağıdaki `nginx`az [kaynak etiketi](/cli/azure/resource?view=azure-cli-latest.md#az-resource-tag) komutunu girin:

```azurecli-interactive
az resource tag --tags nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```

### <a name="using-ansible-version--28"></a>Ansible sürümünü >= 2.8 kullanma
Anahtar ile sanal makine `ansible-inventory-test-vm1` etiketlemek için aşağıdaki `Ansible=nginx`az [kaynak etiketi](/cli/azure/resource?view=azure-cli-latest.md#az-resource-tag) komutunu girin:

```azurecli-interactive
az resource tag --tags Ansible=nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```

## <a name="generate-a-dynamic-inventory"></a>Dinamik bir envanter oluşturma

Sanal makinelerinizi tanımladıktan (ve etiketlendikten sonra), dinamik envanteroluşturma nın zamanı gelmiştir.

### <a name="using-ansible-version--28"></a>2.8< Ansible sürümünü kullanma

Ansible, Azure kaynaklarınızın dinamik bir envanterini oluşturan [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) adlı bir Python komut dosyası sağlar. İki test Azure sanal `azure_rm.py` makinenize bağlanmak için komut dosyasını kullanarak aşağıdaki adımlar size yol verebiliyor:

1. Komut dosyasını `wget` almak için `azure_rm.py` GNU komutunu kullanın:

    ```python
    wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
    ```

1. Komut `chmod` dosyasına erişim izinlerini değiştirmek `azure_rm.py` için komutu kullanın. Aşağıdaki komut, `+x` belirtilen dosyanın yürütülmesine (çalıştırınlanın)`azure_rm.py`izin vermek için parametreyi kullanır ( ):

    ```python
    chmod +x azure_rm.py
    ```

1. Kaynak grubunuza bağlanmak için [ansible komutunu](https://docs.ansible.com/ansible/2.4/ansible.html) kullanın: 

    ```python
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
    ```

1. Bağlandıktan sonra, aşağıdaki çıktıya benzer sonuçlar görürsünüz:

    ```output
    ansible-inventory-test-vm1 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ansible-inventory-test-vm2 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ```

### <a name="ansible-version--28"></a>Ansi versiyon >= 2.8

Ansible 2.8 ile başlayarak, Ansible bir [Azure dinamik envanter eklentisi](https://github.com/ansible/ansible/blob/devel/lib/ansible/plugins/inventory/azure_rm.py)sağlar. Aşağıdaki adımlar eklentiyi kullanarak size yol:

1. Stok eklentisi bir yapılandırma dosyası gerektirir. Yapılandırma dosyası sona `azure_rm` ermeli ve bir `yml` `yaml`uzantısı olmalı ya da . Bu öğretici örnek için, aşağıdaki `myazure_rm.yml`oyun kitabını aşağıdaki gibi kaydedin:

    ```yml
        plugin: azure_rm
        include_vm_resource_groups:
        - ansible-inventory-test-rg
        auth_source: auto
    
        keyed_groups:
        - prefix: tag
          key: tags
    ```

1. Kaynak grubundaki Ping VM'leri için aşağıdaki komutu çalıştırın:

    ```bash
    ansible all -m ping -i ./myazure_rm.yml
    ```

1. Önceki komutu çalıştırırken aşağıdaki hatayı alabilirsiniz:

    ```output
    Failed to connect to the host via ssh: Host key verification failed.
    ```
    
    "Ana bilgisayar denetimi" hatasını alırsanız, Ansible yapılandırma dosyasına aşağıdaki satırı ekleyin. Ansible yapılandırma dosyası veya `/etc/ansible/ansible.cfg` . `~/.ansible.cfg`

    ```bash
    host_key_checking = False
    ```

1. Oyun kitabını çalıştırdığınızda, aşağıdaki çıktıya benzer sonuçlar görürsünüz:
  
    ```output
    ansible-inventory-test-vm1_0324 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ansible-inventory-test-vm2_8971 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ```

## <a name="enable-the-vm-tag"></a>VM etiketini etkinleştirme

### <a name="if-youre-using-ansible--28"></a>Ansible < 2.8 kullanıyorsanız,

- Bir etiket ayarladıktan sonra, bu etiketi "etkinleştirmeniz" gerekir. Bir etiketi etkinleştirmenin bir yolu, etiketi komut `AZURE_TAGS` aracılığıyla `export` bir ortam değişkenine dışa aktarmaktır:

    ```console
    export AZURE_TAGS=nginx
    ```
    
- Şu komutu çalıştırın:

    ```bash
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping
    ```
    
    Artık yalnızca bir sanal makine görüyorsunuz (etiketi ortam değişkenine `AZURE_TAGS` aktarılan değerle eşleşen bir makine):

    ```output
       ansible-inventory-test-vm1 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ```

### <a name="if-youre-using-ansible---28"></a>Ansible >kullanıyorsanız = 2.8

- Aşağıdaki çıktıyı almak için komutu `ansible-inventory -i myazure_rm.yml --graph` çalıştırın:

    ```output
        @all:
          |--@tag_Ansible_nginx:
          |  |--ansible-inventory-test-vm1_9e2f
          |--@ungrouped:
          |  |--ansible-inventory-test-vm2_7ba9
    ```

- Nginx VM bağlantısını test etmek için aşağıdaki komutu da çalıştırabilirsiniz:
  
    ```bash
    ansible -i ./myazure_rm.yml -m ping tag_Ansible_nginx
    ```


## <a name="set-up-nginx-on-the-tagged-vm"></a>Nginx'i etiketli VM'de ayarlama

Etiketlerin amacı, sanal makinelerinizin alt gruplarıyla hızlı ve kolay bir şekilde çalışabilme yeteneğini etkinleştirmektir. Örneğin, Nginx'i yalnızca `nginx`bir etiket atadığınız sanal makinelere yüklemek istediğinizi varsayalım. Aşağıdaki adımlar, bunu gerçekleştirmenin ne kadar kolay olduğunu göstermektedir:

1. Adlı `nginx.yml`bir dosya oluşturma:

   ```console
   code nginx.yml
   ```

1. Aşağıdaki örnek kodu düzenleyiciye yapıştırın:

    ```yml
        ---
        - name: Install and start Nginx on an Azure virtual machine
          hosts: all
          become: yes
          tasks:
          - name: install nginx
            apt: pkg=nginx state=present
            notify:
            - start nginx
    
          handlers:
            - name: start nginx
              service: name=nginx state=started
    ```

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

1. Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

   - Ansible < 2.8:

     ```bash
     ansible-playbook -i azure_rm.py nginx.yml
     ```

   - Ansible >= 2.8:

     ```bash
     ansible-playbook  -i ./myazure_rm.yml  nginx.yml --limit=tag_Ansible_nginx
     ```

1. Oyun kitabını çalıştırdıktan sonra, aşağıdaki sonuçlara benzer çıktı görürsünüz:

    ```output
    PLAY [Install and start Nginx on an Azure virtual machine] 

    TASK [Gathering Facts] 
    ok: [ansible-inventory-test-vm1]

    TASK [install nginx] 
    changed: [ansible-inventory-test-vm1]

    RUNNING HANDLER [start nginx] 
    ok: [ansible-inventory-test-vm1]

    PLAY RECAP 
    ansible-inventory-test-vm1 : ok=3    changed=1    unreachable=0    failed=0
    ```

## <a name="test-nginx-installation"></a>Test Nginx kurulumu

Bu bölümde, Nginx'in sanal makinenize yüklenmesini test etmek için bir teknik gösterilmektedir.

1. Sanal makinenin IP adresini almak için [az vm list-ip-adresler](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-list-ip-addresses) komutunu `ansible-inventory-test-vm1` kullanın. Döndürülen değer (sanal makinenin IP adresi) daha sonra sanal makineye bağlanmak için SSH komutuna parametre olarak kullanılır.

    ```azurecli-interactive
    ssh `az vm list-ip-addresses \
    -n ansible-inventory-test-vm1 \
    --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress -o tsv`
    ```

1. `ansible-inventory-test-vm1` Sanal makineye bağlıyken, Nginx yüklü olup olmadığını belirlemek için [nginx -v](https://nginx.org/en/docs/switches.html) komutunu çalıştırın.

    ```console
    nginx -v
    ```

1. Komutu `nginx -v` çalıştırdıktan sonra, Nginx'in yüklü olduğunu gösteren Nginx sürümünü (ikinci satır) görürsünüz.

    ```output
    tom@ansible-inventory-test-vm1:~$ nginx -v

    nginx version: nginx/1.10.3 (Ubuntu)

    tom@ansible-inventory-test-vm1:~$
    ```

1. SSH `<Ctrl>D` oturumunu kesmek için klavye birleşimini tıklatın.

1. `ansible-inventory-test-vm2` Sanal makine için önceki adımları yapmak, Nginx'i nereden alabileceğinizi belirten bir bilgilendirme iletisi verir (bu, bu noktada yüklü olmadığınızı gösterir):

    ```output
    tom@ansible-inventory-test-vm2:~$ nginx -v
    The program 'nginx' can be found in the following packages:
    * nginx-core
    * nginx-extras
    * nginx-full
    * nginx-lightTry: sudo apt install <selected package>
    tom@ansible-inventory-test-vm2:~$
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Quickstart: Ansible kullanarak Azure'daki Linux sanal makinelerini yapılandırın](./ansible-create-vm.md)