---
title: Quickstart - Azure'daki Linux sanal makinelerine Ansible yükle
description: Bu hızlı başlangıçta, Ubuntu, CentOS ve SLES'te Azure kaynaklarını yönetmek için Ansible'ı nasıl yükleyip yapılandırıştırmayı öğrenin
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: gwallace
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 44007000475793005560914fd816cd0c16927f9a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77202413"
---
# <a name="quickstart-install-ansible-on-linux-virtual-machines-in-azure"></a>Quickstart: Azure'da Linux sanal makinelerine Ansible'ı yükleyin

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Bu makalede, en yaygın Linux dağıtımlarından bazıları için Ansible'ın nasıl yapılandırılabildiği gösterilmektedir. Ansible'ı diğer dağıtımlara yüklemek için, yüklü paketleri belirli bir platform için ayarlayın. 

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-sub.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-sp.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
- **Linux veya Linux sanal makinesine erişim** - Linux makineniz yoksa [Linux sanal makinesi](/azure/virtual-network/quick-create-cli) oluşturun.

## <a name="install-ansible-on-an-azure-linux-virtual-machine"></a>Bir Azure Linux sanal makinesine Ansible yükleme

Linux makinenizde oturum açın ve Ansible yükleme adımları için aşağıdaki dağıtımlardan birini seçin:

- [CentOS 7.4](#centos-74)
- Ubuntu 16.04 LTS
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Bu bölümde, Ansible'ı kullanacak şekilde CentOS'u yapılandırAbilirsiniz.

1. Bir terminal penceresi açın.

1. Azure Python SDK modülleri için gerekli paketleri yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
    sudo yum install -y python-pip python-wheel
    ```

1. Gerekli paketleri ansible yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo pip install ansible[azure]
    ```

1. [Azure kimlik bilgilerini oluşturun.](#create-azure-credentials)

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Bu bölümde, Ansible kullanmak için Ubuntu yapılandırır.

1. Bir terminal penceresi açın.

1. Azure Python SDK modülleri için gerekli paketleri yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip
    ```

1. Gerekli paketleri ansible yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo pip install ansible[azure]
    ```

1. [Azure kimlik bilgilerini oluşturun.](#create-azure-credentials)

### <a name="sles-12-sp2"></a>SLES 12 SP2

Bu bölümde, SLES'i Ansible'ı kullanacak şekilde yapılandırırsınız.

1. Bir terminal penceresi açın.

1. Azure Python SDK modülleri için gerekli paketleri yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
        python-devel libopenssl-devel libtool python-pip python-setuptools
    ```

1. Gerekli paketleri ansible yüklemek için aşağıdaki komutu girin:

    ```bash
    sudo pip install ansible[azure]
    ```

1. Çakışan Python şifreleme paketini kaldırmak için aşağıdaki komutu girin:

    ```bash
    sudo pip uninstall -y cryptography
    ```

1. [Azure kimlik bilgilerini oluşturun.](#create-azure-credentials)

## <a name="create-azure-credentials"></a>Azure kimlik bilgilerini oluşturma

Ansible kimlik bilgilerini yapılandırmak için aşağıdaki bilgilere ihtiyacınız vardır:

* Azure abonelik kimliğiniz 
* Hizmet temel değerleri

Ansible Tower veya Jenkins kullanıyorsanız, hizmet temel değerlerini ortam değişkenleri olarak bildirin.

Aşağıdaki tekniklerden birini kullanarak Ansible kimlik bilgilerini yapılandırın:

- [Ansible kimlik bilgileri dosyası oluşturma](#file-credentials)
- [Ansible ortam değişkenlerini kullanma](#env-credentials)

### <a name="span-idfile-credentials-create-ansible-credentials-file"></a><span id="file-credentials"/> Ansible kimlik bilgileri dosyası oluşturma

Bu bölümde, Ansible'a kimlik bilgileri sağlamak için yerel bir kimlik bilgileri dosyası oluşturursunuz. 

Ansible kimlik bilgilerini tanımlama hakkında daha fazla bilgi için [bkz.](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules)

1. Geliştirme ortamı için, ana `credentials` bilgisayar sanal makinesinde adlı bir dosya oluşturun:

    ```bash
    mkdir ~/.azure
    vi ~/.azure/credentials
    ```

1. Aşağıdaki satırları dosyaya ekleyin. Yer tutucuları hizmet temel değerleriyle değiştirin.

    ```bash
    [default]
    subscription_id=<your-subscription_id>
    client_id=<security-principal-appid>
    secret=<security-principal-password>
    tenant=<security-principal-tenant>
    ```

1. Dosyayı kaydedin ve kapatın.

### <a name="span-idenv-credentialsuse-ansible-environment-variables"></a><span id="env-credentials"/>Ansible ortam değişkenlerini kullanma

Bu bölümde, Ansible kimlik bilgilerinizi yapılandırmak için hizmet temel değerlerini dışa aktarın.

1. Bir terminal penceresi açın.

1. Hizmet temel değerlerini dışa aktarın:

    ```bash
    export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
    export AZURE_CLIENT_ID=<security-principal-appid>
    export AZURE_SECRET=<security-principal-password>
    export AZURE_TENANT=<security-principal-tenant>
    ```

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama

Başarılı yapılandırmayı doğrulamak için, Bir Azure kaynak grubu oluşturmak için Ansible'ı kullanın.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Quickstart: Ansible kullanarak Azure'da bir Linux sanal makineyi yapılandırma](./ansible-create-vm.md)