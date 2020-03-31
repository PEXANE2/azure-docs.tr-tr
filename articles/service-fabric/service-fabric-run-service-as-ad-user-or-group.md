---
title: BIR AD kullanıcısı veya grup olarak Azure Hizmet Kumaşı hizmetini çalıştırma
description: Bir Hizmeti, Service Fabric Windows bağımsız kümesinde Active Directory kullanıcısı veya grubu olarak nasıl çalıştırılabildiğinizi öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 03/29/2018
ms.author: dekapur
ms.openlocfilehash: d440aadb66562e32331c9725a9367c12440a315d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464254"
---
# <a name="run-a-service-as-an-active-directory-user-or-group"></a>Etkin Dizin kullanıcısı veya grup olarak bir hizmeti çalıştırma
Windows Server bağımsız kümesinde, bir Hizmeti Bir RunAs ilkesini kullanarak Etkin Dizin kullanıcısı veya grup olarak çalıştırabilirsiniz.  Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işleminin altında çalıştığı hesabın altında çalışır. Uygulamaları paylaşılan barındırılan bir ortamda bile farklı hesaplar altında çalıştırmak, uygulamaları birbirinden daha güvenli hale getirir. Bunun, Azure Etkin Dizini (Azure AD) değil, etki alanınızda etkin dizin kullandığını unutmayın.  Ayrıca bir hizmeti grup [Yönetilen Hizmet Hesabı (gMSA)](service-fabric-run-service-as-gmsa.md)olarak da çalıştırabilirsiniz.

Bir etki alanı kullanıcısını veya grubunu kullanarak, izin verilmiş etki alanındaki diğer kaynaklara (örneğin, dosya paylaşımları) erişebilirsiniz.

Aşağıdaki örnekte, *MyCert*adlı bir sertifika kullanılarak şifrelenmiş etki alanı şifresi ile *TestUser* adlı bir Active Directory kullanıcısı gösterilmektedir. Gizli şifreleme `Invoke-ServiceFabricEncryptText` metnini oluşturmak için PowerShell komutunu kullanabilirsiniz. Ayrıntılar için [Service Fabric uygulamalarında sırları yönetme](service-fabric-application-secret-management.md) ye bakın.

Bant dışı bir yöntem kullanarak (Azure'da, bu Azure Kaynak Yöneticisi aracılığıyla) parolayı yerel makinenin şifresini çözmek için sertifikanın özel anahtarını dağıtmanız gerekir. Daha sonra, Service Fabric servis paketini makineye dağıttığında, gizlinin şifresini çözebilir ve (kullanıcı adı ile birlikte) Active Directory ile bu kimlik bilgileri altında çalışmak üzere kimlik doğrulaması yapabiliyor.

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```

> [!NOTE] 
> Bir hizmete RunAs ilkesi uygularsanız ve hizmet bildirimi HTTP protokolü ile uç nokta kaynaklarını bildirirse, bir **SecurityAccessPolicy**belirtmeniz gerekir.  Daha fazla bilgi için [http ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atay'a](service-fabric-assign-policy-to-endpoint.md)bakın. 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Bir sonraki adım olarak, aşağıdaki makaleleri okuyun:
* [Uygulama modelini anlama](service-fabric-application-model.md)
* [Hizmet bildiriminde kaynakları belirtin](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
