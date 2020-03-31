---
title: Azure Red Hat OpenShift için Azure AD kiracıoluşturma
description: Microsoft Azure Red Hat OpenShift kümenizi barındırmak için azure etkin dizin (Azure AD) kiracısını şu şekilde oluşturabilirsiniz.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: ad03538cafcce9c1d660d0f2ac5eb3c6ae5f4f38
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243698"
---
# <a name="create-an-azure-ad-tenant-for-azure-red-hat-openshift"></a>Azure Red Hat OpenShift için Azure AD kiracıoluşturma

Microsoft Azure Red Hat OpenShift, kümenizi oluşturmak için bir [Azure Etkin Dizin (Azure AD)](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) kiracısı gerektirir. *Kiracı,* bir kuruluşun veya uygulama geliştiricisinin Azure, Microsoft Intune veya Microsoft 365'e kaydolarak Microsoft ile ilişki oluşturduğunda aldığı özel bir Azure REKLAM örneğidir. Her Azure AD kiracısı diğer Azure AD kiracılarından farklıdır ve farklıdır ve kendi iş ve okul kimlikleri ve uygulama kayıtları vardır.

Azure AD kiracınız yoksa, bir Azure REKLAM kiracınız oluşturmak için aşağıdaki yönergeleri izleyin.

## <a name="create-a-new-azure-ad-tenant"></a>Yeni Azure AD kiracısı oluşturma

Kiracı oluşturmak için:

1. Azure Kırmızı Şapka OpenShift kümenizle ilişkilendirmek istediğiniz hesabı kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.
2. Yeni bir kiracı (yeni Azure Etkin Dizin olarak da bilinir) oluşturmak için [Azure Etkin Dizin bıçağını](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) açın. *Azure Active Directory*
3. Kuruluş **adı**sağlayın.
4. İlk **etki alanı adı**sağlayın. Bu *onmicrosoft.com* eklenecek. *Kuruluş adının* değerini buradan yeniden kullanabilirsiniz.
5. Kiracının oluşturulacağı bir ülke veya bölge seçin.
6. **Oluştur'u**tıklatın.
7. Azure AD kiracınız oluşturulduktan sonra, **yeni dizin bağlantınızı yönetmek için buraya tıklayın'ı** seçin. Yeni kiracı adınız Azure portalının sağ üst kısmında görüntülenmelidir:  

    ![Sağ üstte kiracı adını gösteren portalın ekran görüntüsü][tenantcallout]  

8. Daha sonra Azure Red Hat OpenShift kümenizi nerede oluşturacağınızı belirtebilmek için *kiracı kimliğine* dikkat edin. Portalda, yeni kiracınız için Azure Etkin Dizin genel bakış bıçağını görmeniz gerekir. **Özellikler'i** seçin ve **Dizin Kimliğinizin**değerini kopyalayın. Bu değere Azure `TENANT` Kırmızı [Şapka OpenShift küme](tutorial-create-cluster.md) oluşturma öğreticisinde olduğu gibi atıfta bulunacağız.

[tenantcallout]: ./media/howto-create-tenant/tenant-callout.png

## <a name="resources"></a>Kaynaklar

[Azure AD kiracıları](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)hakkında daha fazla bilgi için [Azure Etkin Dizin belgelerine](https://docs.microsoft.com/azure/active-directory/) göz atın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Red Hat OpenShift kümenizde uygulamaları test etmek için nasıl bir hizmet sorumlusu oluşturacağınızı, istemci gizli ve kimlik doğrulama geri arama URL'sini nasıl oluşturacağınızı öğrenin.

[Azure AD uygulama nesnesi veya kullanıcısı oluşturma](howto-aad-app-configuration.md)
