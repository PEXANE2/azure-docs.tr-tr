---
title: Azure Active Directory bağlı hizmeti ile hataları tanılama
description: Active Directory bağlı hizmeti uyumsuz bir kimlik doğrulama türü algıladı
author: ghogen
manager: jillfra
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e544942029532fdbe998c36917e688d70ce4ed5
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68851988"
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>Azure Active Directory bağlı hizmeti ile hataları tanılama

Önceki kimlik doğrulama kodu algılanırken, Azure Active Director Connect sunucusu uyumsuz bir kimlik doğrulama türü algıladı.

Bir projede önceki kimlik doğrulama kodunu doğru bir şekilde algılamak için, projenin oluşturulması gerekir.  Bu hatayı görürseniz ve projenizde önceki bir kimlik doğrulama kodunuz yoksa, yeniden derleyin ve tekrar deneyin.

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

Bir Web API projesinde Windows kimlik doğrulamasını algılamak için, bağlı hizmet `IISExpressWindowsAuthentication` `.csproj` projenizin dosyasındaki öğesini arar:

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
