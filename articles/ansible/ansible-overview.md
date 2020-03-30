---
title: Azure ile Ansible'ı Kullanma
description: Bulut sağlama, yapılandırma yönetimi ve uygulama dağıtımlarını otomatikleştirmek için Ansible'ı kullanmaya giriş.
keywords: ansible, azure, devops, genel bakış, bulut sağlama, yapılandırma yönetimi, uygulama dağıtımı, ansible modülleri, ansible playbooks
ms.topic: overview
ms.date: 04/30/2019
ms.openlocfilehash: e9d5b8858f052083b157c7d4809fe21018518bcd
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77193574"
---
# <a name="using-ansible-with-azure"></a>Azure ile Ansible'ı Kullanma

[Ansible,](https://www.ansible.com) bulut sağlama, yapılandırma yönetimi ve uygulama dağıtımlarını otomatikleştiren açık kaynak kodlu bir üründür. Ansible'ı kullanarak sanal makineler, konteynerler ve ağ ve eksiksiz bulut altyapıları sağlayabilirsiniz. Ayrıca, Ansible ortamınızdaki kaynakların dağıtımını ve yapılandırmasını otomatikleştirmenize olanak tanır.

Bu makalede, Azure ile Ansible kullanmanın bazı avantajlarıtemel bir genel bakış sağlar.

## <a name="ansible-playbooks"></a>Ansible oyun kitapları

[Ansible oyun kitapları,](https://docs.ansible.com/ansible/latest/playbooks.html) ortamınızı yapılandırmak için Ansible'ı yönlendirmenize olanak sağlar. Playbook'lar yaml kullanılarak kodlanır, böylece insan tarafından okunabilir hale verilir. Öğreticiler bölümü, Azure kaynaklarını yüklemek ve yapılandırmak için oyun kitaplarını kullanmanın birçok örneğini verir. 

## <a name="ansible-modules"></a>Ani modüller

Ansible, doğrudan uzak ana bilgisayarlarda veya [oyun kitapları](https://docs.ansible.com/ansible/latest/playbooks.html)aracılığıyla çalıştırılabilen bir dizi [Ansible modüliçerir.](https://docs.ansible.com/ansible/latest/modules_by_category.html) Kullanıcılar kendi modüllerini oluşturabilirler. Modüller, hizmetler, paketler veya dosyalar gibi sistem kaynaklarını denetlemek veya sistem komutlarını yürütmek için kullanılır.

Azure hizmetleriyle etkileşim kurmak için Ansible, bir [dizi Ansible bulut modülü](https://docs.ansible.com/ansible/list_of_cloud_modules.html#azure)içerir. Bu modüller, Azure'da altyapınızı oluşturmanıza ve düzenlemenize olanak tanır. 

## <a name="migrate-existing-workload-to-azure"></a>Varolan iş yükünü Azure'a geçirme

Altyapınızı tanımlamak için Ansible'i kullandıktan sonra, Azure'un ortamınızı gerektiği gibi otomatik olarak ölçeklendirmesine izin vererek uygulamanızın oyun kitabını uygulayabilirsiniz. 

## <a name="automate-cloud-native-application-in-azure"></a>Azure'da bulut ayarı uygulamasını otomatikleştirin

Ansible, [Azure'da Azure Fonksiyonları](https://azure.microsoft.com//services/functions/) ve Azure'daki [Kubernetes](https://azure.microsoft.com/services/container-service/kubernetes/)gibi Azure mikro hizmetlerini kullanarak Azure'da bulut ayarı uygulamaları otomatikleştirmenize olanak tanır.  

## <a name="manage-deployments-with-dynamic-inventory"></a>Dinamik envanterle dağıtımları yönetme

Dinamik [envanter](https://docs.ansible.com/ansible/intro_dynamic_inventory.html) özelliği sayesinde Ansible, Azure kaynaklarından envanter çekme olanağı sağlar. Daha sonra varolan Azure dağıtımlarınızı etiketleyebilir ve bu etiketli dağıtımları Ansible aracılığıyla yönetebilirsiniz.

## <a name="additional-azure-marketplace-options"></a>Ek Azure Marketi seçenekleri

[Ansible Tower,](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) Red Hat'in Azure Marketi'ndeki bir görüntüsüdür. 

Ansible Tower, Aşağıdaki özelliklere sahip Ansible için web tabanlı bir web tabanlı ui ve panodur:

* Rol tabanlı erişim denetimini, iş zamanlamasını ve grafik envanter yönetimini tanımlamanızı sağlar. 
* Mevcut araç ve işlemlere Kule'yi ekleyebilmeniz için bir REST API ve CLI içerir. 
* Oyun kitabı çalıştırmalarının gerçek zamanlı çıktısını destekler. 
* Kimlik bilgilerini (Azure ve SSH tuşları gibi) şifreler, böylece kimlik bilgilerini açığa çıkarmadan görevleri temsilciolarak devralabilirsiniz.

## <a name="ansible-module-and-version-matrix-for-azure"></a>Azure için saklı modül ve sürüm matrisi

Ansible, Azure kaynaklarının sağlanması nda ve yapılandırılmasında kullanılmak üzere bir modül paketi içerir. Bu kaynaklar sanal makineleri, ölçek kümelerini, ağ hizmetlerini ve kapsayıcı hizmetlerini içerir. [Ansible matrisi,](./ansible-matrix.md) Azure için Ansible modüllerini ve içinde gönderiyaptıkları Ansible sürümlerini listeler.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: Azure için Ansible çözüm şablonu yla CentOS'a dağıtın](./ansible-deploy-solution-template.md)
- [Quickstart: Ansible kullanarak Azure'daki Linux sanal makinelerini yapılandırın](./ansible-install-configure.md)