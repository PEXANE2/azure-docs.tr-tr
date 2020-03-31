---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: f74d4cbc17e49345534a37e9e6612a36e19be295
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73903694"
---
Ancak, Azure kiracınızın dışındaki görüntüleri ölçekte paylaşmak istiyorsanız, paylaşımı kolaylaştırmak için bir uygulama kaydı oluşturmanız gerekir.  Uygulama kaydı kullanmak, daha karmaşık paylaşım senaryolarına olanak sağlayabilir: 

* Bir şirket başka bir şirket satın aldığında paylaşılan görüntüleri yönetme ve Azure altyapısı ayrı kiracılara yayılır. 
* Azure İş Ortakları, Azure altyapılarını müşterileri adına yönetir. Görüntülerin özelleştirilmesi iş ortağı kiracı içinde yapılır, ancak altyapı dağıtımları müşterinin kiracı sında gerçekleşir. 


## <a name="create-the-app-registration"></a>Uygulama kaydını oluşturma

Resim galerisi kaynaklarını paylaşmak için her iki kiracı tarafından kullanılacak bir uygulama kaydı oluşturun.
1. Azure [portalında Uygulama kayıtlarını (önizleme)](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType//sourceType/)açın.    
1. Sayfanın üst kısmındaki menüden **Yeni kayıt'ı** seçin.
1. **Ad**, *myGalleryApp*yazın.
1. Desteklenen hesap **türlerinde, tüm kuruluş dizininde ve kişisel Microsoft hesaplarındahesapları**seçin. **Supported account types**
1. **URI'yi Yeniden** *https://www.microsoft.com* Yönlendirme'de , yazın ve ardından **Kaydol'u**seçin. Uygulama kaydı oluşturulduktan sonra genel bakış sayfası açılır.
1. Genel bakış sayfasında, **Uygulama (istemci) kimliğini** kopyalayın ve daha sonra kullanılmak üzere kaydedin.   
1. **Sertifikalar & sırları**seçin ve ardından **Yeni istemci sırrını**seçin.
1. **Tanımda**, paylaşılan *resim galerisi çapraz kiracı uygulaması gizli*yazın.
1. **Sona Erme'de,** **1 yıl içinde** varsayılan bırakın ve sonra **Ekle'yi**seçin.
1. Sırrın değerini kopyalayın ve güvenli bir yere kaydedin. Sayfayı terk ettikten sonra geri alamazsınız.


Paylaşılan resim galerisini kullanmak için uygulama kaydına izin verin.
1. Azure portalında, başka bir kiracıyla paylaşmak istediğiniz Paylaşılan Resim Galerisi'ni seçin.
1. **Access denetimi (IAM) seçin**ve rol **atamaekle'nin** altında *Ekle'yi*seçin. 
1. **Rolü**altında, **Reader**seçin.
1. **Access'i Ata altında:**, bunu **Azure AD kullanıcısı, grubu veya hizmet sorumlusu**olarak bırakın.
1. **Select**'in *altında, myGalleryApp* yazın ve listede belirdiğinde seçin. İşiniz bittiğinde **Kaydet**‘i seçin.


## <a name="give-tenant-2-access"></a>Kiracıya 2 erişim ver

Bir tarayıcı kullanarak oturum açma isteğinde bulunarak Kiracı 2'nin uygulamaya erişmesini sağlar. * \<Kiracı2 Kimliği>,* resim galerinizi paylaşmak istediğiniz kiracının kiracı kimliğiyle değiştirin. * \<Uygulama (istemci) kimliğini>* oluşturduğunuz uygulama kaydının uygulama kimliğiyle değiştirin. Değiştirmeler yapıldığında, URL'yi bir tarayıcıya yapıştırın ve Kiracı 2'de oturum açma istemlerini izleyin.

```
https://login.microsoftonline.com/<Tenant 2 ID>/oauth2/authorize?client_id=<Application (client) ID>&response_type=code&redirect_uri=https%3A%2F%2Fwww.microsoft.com%2F 
```

Azure [portalında](https://portal.azure.com) Tenant 2 olarak oturum açın ve vm oluşturmak istediğiniz kaynak grubuna uygulama kaydı erişimi verin.

1. Kaynak grubunu seçin ve ardından **Access denetimi 'ni (IAM)** seçin. **Rol Atama ekle** altında **Ekle'yi**seçin. 
1. **Rolü**altında , **yazın Katkıda .**
1. **Access'i Ata altında:**, bunu **Azure AD kullanıcısı, grubu veya hizmet sorumlusu**olarak bırakın.
1. **Select** *altında myGalleryApp* yazın ve listede belirdiğinde seçin. İşiniz bittiğinde **Kaydet**‘i seçin.

> [!NOTE]
> Başka bir görüntü sürümü oluşturmak için aynı yönetilen görüntüyü kullanabilmeniz için önce görüntü sürümünün tamamen oluşturulmasını ve çoğaltılmasını beklemeniz gerekir.

