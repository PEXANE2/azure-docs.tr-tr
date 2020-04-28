---
title: Azure AD bağlı hizmeti ile hataları tanılama (Visual Studio)
description: Active Directory bağlı hizmeti uyumsuz bir kimlik doğrulama türü algıladı
author: ghogen
manager: jillfra
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: 4b39aa77ea3895a606ad34a3bc9b70dba924a23f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80886101"
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>Azure Active Directory bağlı hizmeti ile hataları tanılama

Azure Active Directory bağlı hizmet, önceki kimlik doğrulama kodunu algılarken uyumsuz bir kimlik doğrulama türü algıladı.

Bir projede önceki kimlik doğrulama kodunu doğru bir şekilde algılamak için projenin yeniden oluşturulması gerekir. Bu hatayı görürseniz ve projenizde önceki bir kimlik doğrulama kodunuz yoksa, yeniden derleyin ve tekrar deneyin.

## <a name="project-types"></a>Proje türleri

Bağlı hizmet, projenin doğru kimlik doğrulama mantığını ekleyebilmesi için, geliştirmekte olduğunuz projenin türünü denetler. Projede türetilen `ApiController` herhangi bir denetleyici varsa, proje bir WebAPI projesi olarak kabul edilir. Yalnızca projede türetilen `MVC.Controller` denetleyiciler varsa, proje bir MVC projesi olarak kabul edilir. Bağlı hizmet diğer proje türlerini desteklemiyor.

## <a name="compatible-authentication-code"></a>Uyumlu kimlik doğrulama kodu

Bağlı hizmet, daha önce yapılandırılmış veya hizmetle uyumlu olan kimlik doğrulama ayarlarını da denetler. Tüm ayarlar varsa, bu bir re-entrant durumu olarak kabul edilir ve bağlı hizmet, ayarları görüntüle ' yi açar.  Ayarlardan yalnızca bazıları varsa, bu durum bir hata durumu olarak kabul edilir.

Bir MVC projesinde, bağlı hizmet aşağıdaki ayarlardan herhangi birini denetler ve bu da hizmetin önceki kullanımıyla sonuçlanır:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Ayrıca, bağlı hizmet, bir Web API projesinde aşağıdaki ayarlardan herhangi birini denetler ve bu da hizmetin önceki kullanımı ile sonuçlanır:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

## <a name="incompatible-authentication-code"></a>Uyumsuz kimlik doğrulama kodu

Son olarak, bağlı hizmet, Visual Studio 'nun önceki sürümleriyle yapılandırılmış kimlik doğrulama kodu sürümlerini algılamaya çalışır. Bu hatayı aldıysanız, projenizin uyumsuz bir kimlik doğrulama türü içerdiği anlamına gelir. Bağlı hizmet, Visual Studio 'nun önceki sürümlerinden aşağıdaki kimlik doğrulama türlerini algılar:

* Windows Kimlik Doğrulaması
* Bireysel kullanıcı hesapları
* Kuruluş hesapları

MVC projesinde Windows kimlik doğrulamasını algılamak için, bağlantılı `authentication` öğeyi `web.config` dosyanızda arar.

```xml
<configuration>
    <system.web>
        <authentication mode="Windows" />
    </system.web>
</configuration>
```

Bir Web API projesinde Windows kimlik doğrulamasını algılamak için, bağlı hizmet projenizin `IISExpressWindowsAuthentication` `.csproj` dosyasındaki öğesini arar:

```xml
<Project>
    <PropertyGroup>
        <IISExpressWindowsAuthentication>enabled</IISExpressWindowsAuthentication>
    </PropertyGroup>
</Project>
```

Bireysel kullanıcı hesapları kimlik doğrulamasını algılamak için, bağlı hizmet `packages.config` dosyanızdaki paket öğesini arar.

```xml
<packages>
    <package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" />
</packages>
```

Eski bir kurumsal hesap kimlik doğrulaması biçimini algılamak için bağlı hizmet içinde`web.config`aşağıdaki öğeyi arar:

```xml
<configuration>
    <appSettings>
        <add key="ida:Realm" value="***" />
    </appSettings>
</configuration>
```

Kimlik doğrulama türünü değiştirmek için, uyumsuz kimlik doğrulama türünü kaldırın ve bağlı hizmeti yeniden eklemeyi deneyin.

Daha fazla bilgi için bkz. [Azure AD Için kimlik doğrulama senaryoları](authentication-scenarios.md).
