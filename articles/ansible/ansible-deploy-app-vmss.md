---
title: Öğretici - Ansible kullanarak Azure'daki sanal makine ölçeği kümelerine uygulamaları dağıtma
description: Azure sanal makine ölçek kümelerini yapılandırmak ve uygulamayı ölçek kümesinde dağıtmak için Ansible'ı nasıl kullanacağınızı öğrenin
keywords: ansible, azure, devops, bash, playbook, sanal makine, sanal makine ölçek kümesi, vmss
ms.topic: tutorial
ms.date: 01/13/2020
ms.openlocfilehash: d638ae3f0c33734b42ef5456772fcd2bc62e35a4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75940855"
---
# <a name="tutorial-deploy-apps-to-virtual-machine-scale-sets-in-azure-using-ansible"></a>Öğretici: Uygulamaları Ansible kullanarak Azure'daki sanal makine ölçeği kümelerine dağıtma

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-vmss.md](../../includes/open-source-devops-intro-vmss.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Bir grup Azure VM'si için ana bilgisayar bilgilerini alma
> * Örnek uygulamayı klonla ve oluşturun
> * JRE'yi (Java Çalışma Zamanı Ortamı) bir ölçek kümesine yükleme
> * Java uygulamasını bir ölçek kümesine dağıtma

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)] 
[!INCLUDE [ansible-prereqs-vm-scale-set.md](../../includes/ansible-prereqs-vm-scale-set.md)]
- **git** - [git](https://git-scm.com) bu öğreticide kullanılan bir Java örneğini indirmek için kullanılır.
- **Java SE Geliştirme Kiti (JDK)** - [JDK](https://aka.ms/azure-jdks) örnek Java projesini oluşturmak için kullanılır.
- **Apache Maven** - [Apache Maven](https://maven.apache.org/download.cgi) örnek Java projesi oluşturmak için kullanılır.

## <a name="get-host-information"></a>Ana bilgisayar bilgilerini alma

Bu bölümdeki oyun kitabı kodu, bir grup sanal makine için ana bilgisayar bilgilerini alır. Kod, ortak IP adreslerini ve yük bakiyesini belirli bir kaynak grubu `scalesethosts` içinde alır ve envanterde adı geçen bir ana bilgisayar grubu oluşturur.

Aşağıdaki örnek playbook'u `get-hosts-tasks.yml` olarak kaydedin:

  ```yml
  - name: Get facts for all Public IPs within a resource groups
    azure_rm_publicipaddress_facts:
      resource_group: "{{ resource_group }}"
    register: output_ip_address

  - name: Get loadbalancer info
    azure_rm_loadbalancer_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ loadbalancer_name }}"
    register: output

  - name: Add all hosts
    add_host:
      groups: scalesethosts
      hostname: "{{ output_ip_address.publicipaddresses[0].ip_address }}_{{ item.properties.frontendPort }}"
      ansible_host: "{{ output_ip_address.publicipaddresses[0].ip_address }}"
      ansible_port: "{{ item.properties.frontendPort }}"
      ansible_ssh_user: "{{ admin_username }}"
      ansible_ssh_pass: "{{ admin_password }}"
    with_items:
      - "{{ output.ansible_info.azure_loadbalancers[0].properties.inboundNatRules }}"
  ```

## <a name="prepare-an-application-for-deployment"></a>Uygulamaları dağıtım için hazırlama

Bu bölümdeki oyun kitabı `git` kodu, GitHub'dan bir Java örnek projesini klonlamak için kullanılır ve projeyi oluşturur. 

Aşağıdaki playbook'u `app.yml` olarak kaydedin:

  ```yml
  - hosts: localhost
    vars:
      repo_url: https://github.com/spring-guides/gs-spring-boot.git
      workspace: ~/src/helloworld

    tasks:
    - name: Git Clone sample app
      git:
        repo: "{{ repo_url }}"
        dest: "{{ workspace }}"

    - name: Build sample app
      shell: mvn package chdir="{{ workspace }}/complete"
  ```

Örnek Ansible playbook'u aşağıdaki komut ile çalıştırın:

  ```bash
  ansible-playbook app.yml
  ```

Oyun kitabını çalıştırdıktan sonra, aşağıdaki sonuçlara benzer çıktı görürsünüz:

  ```Output
  PLAY [localhost] 

  TASK [Gathering Facts] 
  ok: [localhost]

  TASK [Git Clone sample app] 
  changed: [localhost]

  TASK [Build sample app] 
  changed: [localhost]

  PLAY RECAP 
  localhost                  : ok=3    changed=2    unreachable=0    failed=0

  ```

## <a name="deploy-the-application-to-a-scale-set"></a>Uygulamayı bir ölçek kümesine dağıtma

Bu bölümdeki oyun kitabı kodu aşağıdakileri yapmak için kullanılır:

* JRE'yi adlı bir ana bilgisayar grubuna yükleme`saclesethosts`
* Java uygulamasını adlı ana bilgisayar grubuna dağıtma`saclesethosts`

Örnek oyun kitabını almanın iki yolu vardır:

* [Oyun kitabını indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-setup-deploy.yml) ve `vmss-setup-deploy.yml`'ye kaydedin.
* Adlandırılmış `vmss-setup-deploy.yml` yeni bir dosya oluşturun ve aşağıdaki içeriği kopyalayın:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    scaleset_name: myScaleSet
    loadbalancer_name: myScaleSetLb
    admin_username: azureuser
    admin_password: "{{ admin_password }}"
  tasks:
  - include: get-hosts-tasks.yml

- name: Install JRE on a scale set
  hosts: scalesethosts
  become: yes
  vars:
    workspace: ~/src/helloworld
    admin_username: azureuser

  tasks:
  - name: Install JRE
    apt:
      name: default-jre
      update_cache: yes

  - name: Copy app to Azure VM
    copy:
      src: "{{ workspace }}/complete/target/gs-spring-boot-0.1.0.jar"
      dest: "/home/{{ admin_username }}/helloworld.jar"
      force: yes
      mode: 0755

  - name: Start the application
    shell: java -jar "/home/{{ admin_username }}/helloworld.jar" >/dev/null 2>&1 &
    async: 5000
    poll: 0
```

Oyun kitabını çalıştırmadan önce aşağıdaki notalara bakın:

* `vars` Bölümde, yer tutucuyu `{{ admin_password }}` kendi parolanızla değiştirin.
* Parolalarla ssh bağlantı türünü kullanmak için sshpass programını yükleyin:

     Ubuntu:

    ```bash
    apt-get install sshpass
    ```

    Centos:

    ```bash
    yum install sshpass
    ```

* Bazı ortamlarda, anahtar yerine SSH parolası kullanma yla ilgili bir hata görebilirsiniz. Bu hatayı alırsanız, aşağıdaki satırı veya aşağıdaki satırı `/etc/ansible/ansible.cfg` ekleyerek ana `~/.ansible.cfg`bilgisayar anahtarı denetimini devre dışı kullanabilirsiniz:

    ```bash
    [defaults]
    host_key_checking = False
    ```

Playbook'u aşağıdaki komut ile çalıştırın:

  ```bash
  ansible-playbook vmss-setup-deploy.yml
  ```

Ansible-playbook komutu çalıştıran çıktı, örnek Java uygulamasının ölçek kümesinin ana grubu için yüklendiğini gösterir:

  ```Output
  PLAY [localhost]

  TASK [Gathering Facts]
  ok: [localhost]

  TASK [Get facts for all Public IPs within a resource groups]
  ok: [localhost]

  TASK [Get loadbalancer info]
  ok: [localhost]

  TASK [Add all hosts]
  changed: [localhost] ...

  PLAY [Install JRE on scale set]

  TASK [Gathering Facts]
  ok: [40.114.30.145_50000]
  ok: [40.114.30.145_50003]

  TASK [Copy app to Azure VM]
  changed: [40.114.30.145_50003]
  changed: [40.114.30.145_50000]

  TASK [Start the application]
  changed: [40.114.30.145_50000]
  changed: [40.114.30.145_50003]

  PLAY RECAP
  40.114.30.145_50000        : ok=4    changed=3    unreachable=0    failed=0
  40.114.30.145_50003        : ok=4    changed=3    unreachable=0    failed=0
  localhost                  : ok=4    changed=1    unreachable=0    failed=0
  ```

## <a name="verify-the-results"></a>Sonuçları doğrulayın

Ölçek kümeniz için yük dengeleyicisinin URL'sine gezinerek çalışmanızın sonuçlarını doğrulayın:

![Azure'da ayarlanmış bir ölçekte çalışan Java uygulaması.](media/ansible-vmss-deploy/ansible-deploy-app-vmss.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Ansible kullanarak Azure'da otomatik ölçek sanal makine ölçek kümeleri](./ansible-auto-scale-vmss.md)