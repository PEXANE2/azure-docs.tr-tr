---
title: Öğretici - Ansible kullanarak Azure Kubernetes Hizmetinde (AKS) rol tabanlı erişim denetimi (RBAC) rollerini yapılandırın
description: Azure Kubernetes Hizmeti (AKS) kümesinde RBAC'ı yapılandırmak için Ansible'ı nasıl kullanacağınızı öğrenin
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, konteyner, aks, kubernetes, azure active directory, rbac
ms.topic: tutorial
ms.date: 04/30/2019
ms.openlocfilehash: 5fac42383ee56318cc4b8f39323c02d05853dbb6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76836975"
---
# <a name="tutorial-configure-role-based-access-control-rbac-roles-in-azure-kubernetes-service-aks-using-ansible"></a>Öğretici: Ansible kullanarak Azure Kubernetes Hizmetinde (AKS) rol tabanlı erişim denetimi (RBAC) rollerini yapılandırın

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

AKS, kullanıcı kimlik doğrulaması için [Azure Active Directory (AD)](/azure/active-directory/) kullanacak şekilde yapılandırılabilir. Yapılandırıldıktan sonra, AKS kümesinde oturum açabilmek için Azure AD kimlik doğrulama belirtecinizi kullanırsınız. RBAC, bir kullanıcının kimliğine veya dizin grubu üyeliğine dayalı olabilir.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Azure AD özellikli AKS kümesi oluşturma
> * Kümede bir RBAC rolünü yapılandırma

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
- **RedHat OpenShift kitaplığını yükleme** - `pip install openshift`

## <a name="configure-azure-ad-for-aks-authentication"></a>AKS kimlik doğrulaması için Azure AD'yi yapılandırma

AKS kimlik doğrulaması için Azure AD yapılandırılırken, iki Azure AD uygulaması yapılandırılır. Bu işlem bir Azure kiracı yöneticisi tarafından tamamlanmalıdır. Daha fazla bilgi için azure [etkin dizinini AKS ile tümleştir'e](/azure/aks/aad-integration#create-the-server-application)bakın. 

Azure kiracı yöneticisinden aşağıdaki değerleri alın:

- Sunucu uygulaması gizli
- Sunucu uygulaması Kimliği
- İstemci uygulaması kimliği 
- Kiracı Kimliği

Örnek oyun kitabını çalıştırmak için bu değerler gereklidir.  

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Bu bölümde, [Azure AD uygulaması](#configure-azure-ad-for-aks-authentication)ile bir AKS oluşturursunuz.

Örnek oyun kitabıyla çalışırken göz önünde bulundurulması gereken bazı önemli notlar şunlardır:

- Oyun kitabı `ssh_key` `~/.ssh/id_rsa.pub`yükler . Bunu değiştirirseniz, "ssh-rsa" (tırnak işaretleri olmadan) ile başlayan tek satırlı biçimi kullanın.
- Ve `client_id` `client_secret` `~/.azure/credentials`değerleri, varsayılan kimlik bilgisi dosyası olan birinden yüklenir. Bu değerleri hizmet anabilim ülkenize ayarlayabilir veya bu değerleri ortam değişkenlerinden yükleyebilirsiniz:

    ```yml
    client_id: "{{ lookup('env', 'AZURE_CLIENT_ID') }}"
    client_secret: "{{ lookup('env', 'AZURE_SECRET') }}"
    ```

Aşağıdaki playbook'u `aks-create.yml` olarak kaydedin:

```yml
- name: Create resource group
  azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"

- name: List supported kubernetes version from Azure
  azure_rm_aksversion_facts:
      location: "{{ location }}"
  register: versions

- name: Create AKS cluster with RBAC enabled
  azure_rm_aks:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      dns_prefix: "{{ name }}"
      enable_rbac: yes
      kubernetes_version: "{{ versions.azure_aks_versions[-1] }}"
      agent_pool_profiles:
        - count: 3
          name: nodepool1
          vm_size: Standard_D2_v2
      linux_profile:
          admin_username: azureuser
          ssh_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
      service_principal:
          client_id: "{{ lookup('ini', 'client_id section=default file=~/.azure/credentials') }}"
          client_secret: "{{ lookup('ini', 'secret section=default file=~/.azure/credentials') }}"
      aad_profile:
          client_app_id: "{{ client_app_id }}"
          server_app_id: "{{ server_app_id }}"
          server_app_secret: "{{ server_app_secret }}"
          tenant_id: "{{ app_tenant_id }}"
  register: aks

- name: Save cluster user config
  copy:
      content: "{{ aks.kube_config }}"
      dest: "aks-{{ name }}-kubeconfig-user"

- name: Get admin config of AKS
  azure_rm_aks_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      show_kubeconfig: admin
  register: aks

- name: Save the kubeconfig
  copy:
      content: "{{ aks.aks[0].kube_config }}"
      dest: "aks-{{ name }}-kubeconfig"
```

## <a name="get-the-azure-ad-object-id"></a>Azure AD Nesnekimliği'ni alın

Bir RBAC bağlayıcısı oluşturmak için öncelikle Azure AD Nesne kimliği almanız gerekir. 

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.

1. Sayfanın üst kısmındaki arama alanına `Azure Active Directory`. 

1. `Enter` öğesine tıklayın.

1. **Yönet** menüsünde, **Kullanıcıları**seçin.

1. Ad alanında hesabınızı arayın.

1. **Ad** sütununda, hesabınıza bağlantı seçin.

1. **Kimlik** bölümünde **Nesne Kimliğini**kopyalayın.

    ![Azure AD Nesne kimliği kopyalayın.](./media/ansible-aks-configure-rbac/ansible-aad-object-id.png)

## <a name="create-rbac-binding"></a>RBAC bağlama oluşturma

Bu bölümde, AKS'de rol bağlama veya küme rol bağlama oluşturursunuz. 

Aşağıdaki playbook'u `kube-role.yml` olarak kaydedin:

```yml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: <your-aad-account>
```

Yer `<your-aad-account>` tutucuyu Azure AD kiracı [Nesne Kimliğinizle](#get-the-azure-ad-object-id)değiştirin.

Aks'a yeni rolünüzü dağıtan aşağıdaki oyun `aks-kube-deploy.yml`kitabını kaydedin:

```yml
- name: Apply role to AKS
  k8s:
      src: kube-role.yml
      kubeconfig: "aks-{{ name }}-kubeconfig"
```

## <a name="run-the-sample-playbook"></a>Örnek oyun kitabını çalıştırın

Bu bölümde, bu makalede oluşturan görevleri çağıran tam örnek oyun kitabı listelenir. 

Aşağıdaki playbook'u `aks-rbac.yml` olarak kaydedin:

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

     - name: Create AKS
       vars:
           client_app_id: <client id>
           server_app_id: <server id>
           server_app_secret: <server secret>
           app_tenant_id: <tenant id>
       include_tasks: aks-create.yml

     - name: Enable RBAC
       include_tasks: aks-kube-deploy.yml
```

`vars` Bölümde, aşağıdaki yer tutucuları Azure REKLAM bilgilerinizle değiştirin:

- `<client id>`
- `<server id>`
- `<server secret>`
- `<tenant id>`

Komutu kullanarak oyun `ansible-playbook` kitabının tamamını çalıştırın:

```bash
ansible-playbook aks-rbac.yml
```

## <a name="verify-the-results"></a>Sonuçları doğrulayın

Bu bölümde, bu makalede oluşturan düğümleri kubectl listesini kullanın.

Terminal isteminde aşağıdaki komutu girin:

```bash
kubectl --kubeconfig aks-aksansibletest-kubeconfig-user get nodes
```

Komut sizi bir kimlik doğrulama sayfasına yönlendirir. Azure hesabınızla oturum açın.

Kimlik doğrulaması alındıktan sonra, kubectl düğümleri aşağıdaki sonuçlara benzer şekilde listeler:

```txt
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XXXXXXXX to authenticate.
NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-33413200-0   Ready    agent   49m   v1.12.6
aks-nodepool1-33413200-1   Ready    agent   49m   v1.12.6
aks-nodepool1-33413200-2   Ready    agent   49m   v1.12.6
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, bu makalede oluşturulan kaynakları silin. 

Aşağıdaki kodu aşağıdaki `cleanup.yml`gibi kaydedin:

```yml
---
- hosts: localhost
  vars:
      name: aksansibletest
      resource_group: aksansibletest
  tasks:
      - name: Clean up resource group
        azure_rm_resourcegroup:
            name: "{{ resource_group }}"
            state: absent
            force: yes
      - name: Remove kubeconfig
        file:
            state: absent
            path: "aks-{{ name }}-kubeconfig"
```

Komutu kullanarak oyun `ansible-playbook` kitabını çalıştırın:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure üzerinde Ansible](/azure/ansible/)
