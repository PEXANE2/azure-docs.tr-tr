---
title: Azure Red Hat OpenShift geliştirme ortamınızı ayarlama
description: Microsoft Azure Red Hat OpenShift ile çalışmaya yönelik önkoşullar aşağıda verilmiştir.
keywords: Red Hat OpenShift kurulumu kuruldu
author: jimzim
ms.author: jzim
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: container-service
ms.custom: devx-track-azurecli
ms.openlocfilehash: b468f967e68b72e3c9da276dc2077fc09256c895
ms.sourcegitcommit: 4feb198becb7a6ff9e6b42be9185e07539022f17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89470043"
---
# <a name="set-up-your-azure-red-hat-openshift-dev-environment"></a>Azure Red Hat OpenShift geliştirme ortamınızı ayarlama

Microsoft Azure Red Hat OpenShift uygulamalarını derlemek ve çalıştırmak için şunları yapmanız gerekir:

* Azure CLı 'nın sürüm 2.0.65 (veya üzeri) sürümünü (veya Azure Cloud Shell kullanın).
* `AROGA`Özellik ve ilişkili kaynak sağlayıcılarına kaydolun.
* Azure Active Directory (Azure AD) kiracısı oluşturun.
* Bir Azure AD uygulama nesnesi oluşturun.
* Bir Azure AD kullanıcısı oluşturun.

Aşağıdaki yönergeler, bu önkoşulların tümünde size yol gösterecektir.

## <a name="install-the-azure-cli"></a>Azure CLI'yi yükleme

Azure Red Hat OpenShift, Azure CLı 'nin sürüm 2.0.65 veya üstünü gerektirir. Azure CLı 'yi zaten yüklediyseniz, şunu çalıştırarak hangi sürüme sahip olmanız gerektiğini kontrol edebilirsiniz:

```azurecli
az --version
```

Çıktının ilk satırı CLı sürümüne sahip olacaktır, örneğin `azure-cli (2.0.65)` .

Yeni bir yükleme veya yükseltme gerekiyorsa [Azure CLI yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest) yönergeleri aşağıda verilmiştir.

Alternatif olarak, [Azure Cloud Shell](../cloud-shell/overview.md)de kullanabilirsiniz. Azure Cloud Shell kullanırken, [Azure Red Hat Openshıft kümesi oluşturma ve yönetme](tutorial-create-cluster.md) öğreticisiyle birlikte Izlemeyi planlıyorsanız **Bash** ortamını seçtiğinizden emin olun.

## <a name="register-providers-and-features"></a>Sağlayıcıları ve özellikleri kaydetme

`Microsoft.ContainerService AROGA` `Microsoft.Solutions` `Microsoft.Compute` `Microsoft.Storage` `Microsoft.KeyVault` `Microsoft.Network` İlk Azure Red Hat OpenShift kümenizi dağıtmadan önce, özelliği,,, ve sağlayıcılarının aboneliğinize el ile kaydedilmesi gerekir.

Bu sağlayıcıları ve özellikleri el ile kaydetmek için, CLı 'yı yüklediyseniz veya Azure portal Azure Cloud Shell (Bash) oturumunda, bir bash kabuğundan aşağıdaki yönergeleri kullanın:

1. Birden çok Azure aboneliğiniz varsa ilgili abonelik KIMLIĞINI belirtin:

    ```azurecli
    az account set --subscription <SUBSCRIPTION ID>
    ```

1. Microsoft. ContainerService AROGA özelliğini kaydedin:

    ```azurecli
    az feature register --namespace Microsoft.ContainerService -n AROGA
    ```

1. Microsoft. Storage sağlayıcısını kaydedin:

    ```azurecli
    az provider register -n Microsoft.Storage --wait
    ```
    
1. Microsoft. COMPUTE sağlayıcısını kaydedin:

    ```azurecli
    az provider register -n Microsoft.Compute --wait
    ```

1. Microsoft. Solutions sağlayıcısını kaydedin:

    ```azurecli
    az provider register -n Microsoft.Solutions --wait
    ```

1. Microsoft. Network sağlayıcısı 'nı kaydedin:

    ```azurecli
    az provider register -n Microsoft.Network --wait
    ```

1. Microsoft. Keykasası sağlayıcısını kaydedin:

    ```azurecli
    az provider register -n Microsoft.KeyVault --wait
    ```

1. Microsoft. ContainerService kaynak sağlayıcısının kaydını yenileyin:

    ```azurecli
    az provider register -n Microsoft.ContainerService --wait
    ```

## <a name="create-an-azure-active-directory-azure-ad-tenant"></a>Azure Active Directory (Azure AD) kiracısı oluşturma

Azure Red Hat OpenShift hizmeti, kuruluşunuzu ve Microsoft ile ilişkisini temsil eden ilişkili bir Azure Active Directory (Azure AD) kiracısı gerektirir. Azure AD kiracınız, uygulamaları kaydetmenizi, oluşturmanızı ve yönetmenizi, ayrıca diğer Azure hizmetlerini kullanmayı sağlar.

Azure Red Hat OpenShift kümeniz için kiracı olarak kullanabileceğiniz bir Azure AD yoksa veya test için bir kiracı oluşturmak istiyorsanız, bu kılavuza devam etmeden önce [Azure Red Hat Openshıft kümeniz Için Azure AD kiracısı oluşturma](howto-create-tenant.md) bölümündeki yönergeleri izleyin.

## <a name="create-an-azure-ad-user-security-group-and-application-object"></a>Azure AD kullanıcısı, güvenlik grubu ve uygulama nesnesi oluşturma

Azure Red Hat OpenShift, kümenizde depolama yapılandırma gibi görevleri gerçekleştirmek için izinler gerektirir. Bu izinler bir [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object)ile temsil edilir. Ayrıca, Azure Red Hat OpenShift kümenizde çalışan uygulamaları test etmek için yeni bir Active Directory Kullanıcı oluşturmak isteyeceksiniz.

Hizmet sorumlusu oluşturmak için [Azure AD uygulama nesnesi ve Kullanıcı oluşturma](howto-aad-app-configuration.md) bölümündeki yönergeleri izleyin, uygulamanız için bir istemci gizli anahtarı ve kimlik doğrulama geri çağırma URL 'si oluşturun ve kümeye erişmek için yeni BIR Azure AD güvenlik grubu ve kullanıcı oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Red Hat OpenShift kullanmaya hazırsınız!

Öğreticiyi deneyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)

[azure-cli-install]: /cli/azure/install-azure-cli