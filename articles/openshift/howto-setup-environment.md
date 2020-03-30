---
title: Azure Red Hat OpenShift geliştirme ortamınızı ayarlama
description: Microsoft Azure Red Hat OpenShift ile çalışmak için ön koşullar aşağıda verilmiştir.
keywords: kırmızı şapka openshift kurulum kurulumu
author: jimzim
ms.author: jzim
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: container-service
ms.openlocfilehash: e7396ce9fbed46688d59b582f246e5454d063fb3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477043"
---
# <a name="set-up-your-azure-red-hat-openshift-dev-environment"></a>Azure Red Hat OpenShift geliştirme ortamınızı ayarlama

Microsoft Azure Red Hat OpenShift uygulamalarını oluşturmak ve çalıştırmak için şunları yapmanız gerekir:

* Azure CLI sürümünü 2.0.65 (veya daha yüksek) yükleyin (veya Azure Bulut Kabuğu'nu kullanın).
* `AROGA` Özellik ve ilişkili kaynak sağlayıcıları için kaydolun.
* Azure Etkin Dizin (Azure AD) kiracısı oluşturun.
* Azure AD uygulama nesnesi oluşturun.
* Bir Azure AD kullanıcısı oluşturun.

Aşağıdaki talimatlar tüm bu ön koşullar üzerinden size yol verecektir.

## <a name="install-the-azure-cli"></a>Azure CLI'yı yükleme

Azure Red Hat OpenShift, Azure CLI sürümü 2.0.65 veya daha yüksek gerektirir. Azure CLI'yi zaten yüklediyseniz, aşağıdakileri çalıştırarak hangi sürüme sahip olduğunuzu kontrol edebilirsiniz:

```azurecli
az --version
```

İlk çıktı satırı, örneğin `azure-cli (2.0.65)`CLI sürümüne sahip olacaktır.

Yeni bir yükleme veya yükseltme gerektiren [Azure CLI'yi yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yönergeleri aşağıda veda edilmiştir.

Alternatif olarak, Azure [Bulut Kabuğu'nu](https://docs.microsoft.com/azure/cloud-shell/overview)kullanabilirsiniz. Azure Bulut Kabuğu'nu kullanırken, [Bir Azure Kırmızı Şapka OpenShift küme](tutorial-create-cluster.md) öğretici serisiyle birlikte izlemeyi ve yönetmeyi planlıyorsanız **Bash** ortamını seçtiğinizden emin olun.

## <a name="register-providers-and-features"></a>Kayıt sağlayıcıları ve özellikleri

Özellik, `Microsoft.ContainerService AROGA` `Microsoft.Solutions`, `Microsoft.Compute` `Microsoft.Storage`, `Microsoft.KeyVault` `Microsoft.Network` , ve sağlayıcıları n ilk Azure Red Hat OpenShift kümenizi dağıtmadan önce aboneliğinize el ile kaydedilmelidir.

Bu sağlayıcıları ve özellikleri el ile kaydetmek için, CLI'yi yüklediyseniz veya Azure portalınızda Azure Bulut Kabuğu (Bash) oturumundan aşağıdaki yönergeleri kullanın:

1. Birden fazla Azure aboneliğiniz varsa, ilgili abonelik kimliğini belirtin:

    ```azurecli
    az account set --subscription <SUBSCRIPTION ID>
    ```

1. Microsoft.ContainerService AROGA özelliğini kaydedin:

    ```azurecli
    az feature register --namespace Microsoft.ContainerService -n AROGA
    ```

1. Microsoft.Storage sağlayıcısını kaydedin:

    ```azurecli
    az provider register -n Microsoft.Storage --wait
    ```
    
1. Microsoft.Compute sağlayıcısını kaydedin:

    ```azurecli
    az provider register -n Microsoft.Compute --wait
    ```

1. Microsoft.Solutions sağlayıcısını kaydedin:

    ```azurecli
    az provider register -n Microsoft.Solutions --wait
    ```

1. Microsoft.Network sağlayıcısını kaydedin:

    ```azurecli
    az provider register -n Microsoft.Network --wait
    ```

1. Microsoft.KeyVault sağlayıcısını kaydedin:

    ```azurecli
    az provider register -n Microsoft.KeyVault --wait
    ```

1. Microsoft.ContainerService kaynak sağlayıcısının kaydını yenileyin:

    ```azurecli
    az provider register -n Microsoft.ContainerService --wait
    ```

## <a name="create-an-azure-active-directory-azure-ad-tenant"></a>Azure Etkin Dizin (Azure AD) kiracısı oluşturma

Azure Red Hat OpenShift hizmeti, kuruluşunuzun ve Microsoft ile olan ilişkisini temsil eden ilişkili bir Azure Etkin Dizin (Azure AD) kiracısı gerektirir. Azure AD kiracınız, uygulamaları kaydetmenizi, oluşturmanızı ve yönetmenizin yanı sıra diğer Azure hizmetlerini kullanmanızı sağlar.

Azure Red Hat OpenShift kümenizin kiracısı olarak kullanabileceğiniz bir Azure REKLAM'ınız yoksa veya sınama için bir kiracı oluşturmak istiyorsanız, bu kılavuza devam etmeden önce [Azure Red Hat OpenShift kümeniz için Azure AD kiracısı oluştur'daki](howto-create-tenant.md) yönergeleri izleyin.

## <a name="create-an-azure-ad-user-security-group-and-application-object"></a>Azure AD kullanıcısı, güvenlik grubu ve uygulama nesnesi oluşturma

Azure Red Hat OpenShift, depolama alanını yapılandırma gibi kümenizde görevleri gerçekleştirmek için izinler gerektirir. Bu izinler bir [hizmet sorumlusu](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object)aracılığıyla temsil edilir. Azure Red Hat OpenShift kümenizde çalışan uygulamaları test etmek için yeni bir Active Directory kullanıcısı da oluşturmak isteyebilirsiniz.

Bir hizmet sorumlusu oluşturmak, uygulamanız için bir istemci gizli ve kimlik doğrulama geri arama URL'si oluşturmak ve kümeye erişmek için yeni bir Azure AD güvenlik grubu ve kullanıcı oluşturmak için [Bir Azure AD uygulaması nesnesi ve kullanıcısı oluşturun'daki](howto-aad-app-configuration.md) yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Red Hat OpenShift'i kullanmaya hazırsınız!

Öğreticiyi deneyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
