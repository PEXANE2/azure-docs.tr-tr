---
title: Azure AD'ye bağlı hizmetle hataları tanılama (Visual Studio)
description: Etkin dizin bağlantılı hizmet, uyumsuz bir kimlik doğrulama türü algılandı
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
ms.openlocfilehash: a6ec31f0d60c7f6e3737dc4042b05a6d8bf3dd5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76699980"
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>Azure Etkin Dizin Bağlantılı Hizmetile hataları tanılama

Önceki kimlik doğrulama kodunu algılarken, Azure Etkin Dizin bağlı hizmet uyumsuz bir kimlik doğrulama türü algılatmıştır.

Projedeki önceki kimlik doğrulama kodunu doğru algılamak için projenin yeniden oluşturulması gerekir. Bu hatayı görürseniz ve projenizde önceki bir kimlik doğrulama kodunuz yoksa, yeniden yeniden oluşturun ve yeniden deneyin.

## <a name="project-types"></a>Proje türleri

Bağlı hizmet, projeye doğru kimlik doğrulama mantığını enjekte edebilmek için geliştirmekte olduğunuz proje türünü denetler. `ApiController` Projede türetilen herhangi bir denetleyici varsa, proje bir WebAPI projesi olarak kabul edilir. Yalnızca `MVC.Controller` projede türetilen denetleyiciler varsa, proje bir MVC projesi olarak kabul edilir. Bağlı hizmet başka bir proje türünü desteklemez.

## <a name="compatible-authentication-code"></a>Uyumlu kimlik doğrulama kodu

Bağlı hizmet, daha önce yapılandırılan veya hizmetle uyumlu olan kimlik doğrulama ayarlarını da denetler. Tüm ayarlar varsa, yeniden giren bir servis olarak kabul edilir ve bağlı hizmet açılır ayarları görüntüler.  Yalnızca bazı ayarlar varsa, bu bir hata durumu olarak kabul edilir.

Bir MVC projesinde, bağlı hizmet, hizmetin önceki kullanımından kaynaklanan aşağıdaki ayarlardan herhangi birini denetler:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Ayrıca, bağlı hizmet, bir Web API projesinde, hizmetin önceki kullanımından kaynaklanan aşağıdaki ayarlardan herhangi birini denetler:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

## <a name="incompatible-authentication-code"></a>Uyumsuz kimlik doğrulama kodu

Son olarak, bağlı hizmet Visual Studio'nun önceki sürümleriyle yapılandırılan kimlik doğrulama kodunun sürümlerini algılamaya çalışır. Bu hatayı aldıysanız, bu projenizin uyumsuz bir kimlik doğrulama türü içerdiği anlamına gelir. Bağlı hizmet Visual Studio'nun önceki sürümlerinden aşağıdaki kimlik doğrulama türlerini algılar:

* Windows Kimlik Doğrulaması
* Bireysel Kullanıcı Hesapları
* Kuruluş Hesapları

Bir MVC projesinde Windows Kimlik Doğrulaması'nı `authentication` algılamak `web.config` için bağlı olan dosyanızdaki öğeyi arar.

```xml
<configuration>
    <system.web>
        <authentication mode="Windows" />
    </system.web>
</configuration>
```

Bir Web API projesinde Windows Kimlik Doğrulaması'nı `IISExpressWindowsAuthentication` algılamak için `.csproj` bağlı hizmet projenizin dosyasındaki öğeyi arar:

```xml
<Project>
    <PropertyGroup>
        <IISExpressWindowsAuthentication>enabled</IISExpressWindowsAuthentication>
    </PropertyGroup>
</Project>
```

Bireysel Kullanıcı Hesapları kimlik doğrulamasını algılamak için bağlı `packages.config` hizmet dosyanızdaki paket öğesini arar.

```xml
<packages>
    <package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" />
</packages>
```

Eski bir Kuruluş Hesabı kimlik doğrulaması biçimini algılamak için,`web.config`bağlı hizmet aşağıdaki öğeyi arar:

```xml
<configuration>
    <appSettings>
        <add key="ida:Realm" value="***" />
    </appSettings>
</configuration>
```

Kimlik doğrulama türünü değiştirmek için uyumsuz kimlik doğrulama türünü kaldırın ve bağlı hizmeti yeniden eklemeyi deneyin.

Daha fazla bilgi için Azure [AD için Kimlik Doğrulama Senaryoları'na](authentication-scenarios.md)bakın.
