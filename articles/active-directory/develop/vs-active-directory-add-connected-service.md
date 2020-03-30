---
title: Active Directory bağlantılı hizmeti kullanma (Visual Studio)
description: Visual Studio Bağlantılı Hizmetler Ekle iletişim kutusunu kullanarak Azure Etkin Dizini ekleme
author: ghogen
manager: jillfra
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.custom: aaddev, vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.openlocfilehash: f139fc8cb59c40ea169e195312326773296b0592
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77159634"
---
# <a name="add-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Visual Studio'da Bağlı Hizmetleri kullanarak Azure Etkin Dizini ekleme

Azure Etkin Dizin (Azure AD) kullanarak, ASP.NET MVC web uygulamaları için Tek Oturum Açma (SSO) veya Web API hizmetlerinde Etkin Dizin Kimlik Doğrulaması'nı destekleyebilirsiniz. Azure AD Kimlik Doğrulaması ile kullanıcılarınız web uygulamalarınıza bağlanmak için hesaplarını Azure Active Directory'den kullanabilir. Web API ile Azure AD Kimlik Doğrulaması'nın avantajları arasında, bir web uygulamasından API açığa çıkarırken gelişmiş veri güvenliği yer almaktadır. Azure AD ile, kendi hesabı ve kullanıcı yönetimiyle ayrı bir kimlik doğrulama sistemini yönetmeniz gerekmez.

Bu makale ve eşlik eden makaleler, Active Directory için Visual Studio Connected Service özelliğini kullanmanın ayrıntılarını sağlar. Bu özellik Visual Studio 2015 ve sonraki yıllarda mevcuttur.

Şu anda Active Directory'ye bağlı hizmet ASP.NET Core uygulamalarını desteklememektedir.

## <a name="prerequisites"></a>Ön koşullar

- Azure hesabı: Azure hesabınız yoksa, [ücretsiz deneme sürümüne kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)
- **Visual Studio 2015** veya sonrası. [Visual Studio'u hemen indirin.](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs)

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Bağlı Hizmetler iletişim kutusunu kullanarak Azure Etkin Dizine bağlanma

1. Visual Studio'da ASP.NET bir MVC projesi veya ASP.NET bir Web API projesi oluşturun veya açın. MVC, Web API, Tek Sayfa Uygulaması, Azure API Uygulaması, Azure Mobil Uygulaması ve Azure Mobil Hizmet şablonlarını kullanabilirsiniz.

1. Bağlı **Hizmet Ekle...** menü komutunu > Proje'yi seçin veya Solution Explorer'da projenin altında bulunan **Bağlı Hizmetler** düğümünü çift tıklatın.

1. Bağlı **Hizmetler** sayfasında, **Azure Etkin Dizini ile Kimlik Doğrulama'yı**seçin.

    ![Bağlantılı Hizmetler sayfası](./media/vs-azure-active-directory/connected-services-add-active-directory.png)

1. **Giriş** sayfasında **İleri'yi**seçin. Bu sayfada hatalar görürseniz, [Azure Etkin Dizin Bağlantılı Hizmetile hata tanılama'ya](vs-active-directory-error.md)bakın.

    ![Giriş sayfası](./media/vs-azure-active-directory/configure-azure-ad-wizard-1.png)

1. Tek **Oturum Açma** sayfasında, **Etki Alanı** açılır listesinden bir etki alanı seçin. Liste, Visual Studio **(Dosya > Hesap Ayarları)** iletişim kutusunda listelenen hesaplar tarafından erişilebilen tüm etki alanlarını içerir. Alternatif olarak, aradığınız ı bulamazsanız bir etki alanı adı girebilirsiniz. `mydomain.onmicrosoft.com` Bir Azure Active Directory uygulaması oluşturma veya ayarları mevcut bir Azure Active Directory uygulamasından kullanma seçeneğini seçebilirsiniz. Bittiğinde **İleri'yi** seçin.

    ![Sayfada tek oturum açma](./media/vs-azure-active-directory/configure-azure-ad-wizard-2.png)

1. **Dizin Erişimi** sayfasında, istediğiniz gibi **Okuma dizin verileri** seçeneğini belirleyin. Geliştiriciler genellikle bu seçeneği içerir.

    ![Dizin erişim sayfası](./media/vs-azure-active-directory/configure-azure-ad-wizard-3.png)

1. Azure AD kimlik doğrulamasını etkinleştirmek için projenizde değişiklik başlatmak için **Bitiş'i** seçin. Visual Studio bu süre içinde ilerleme gösterir:

    ![Etkin Dizin bağlantılı hizmet ilerleme](./media/vs-azure-active-directory/active-directory-connected-service-output.png)

1. İşlem tamamlandığında Visual Studio, proje türünüze uygun olarak tarayıcınızı aşağıdaki makalelerden birine açar:

    - [.NET MVC projelerini kullanmaya başlama](vs-active-directory-dotnet-getting-started.md)
    - [.NET WebAPI projelerini kullanmaya başlama](vs-active-directory-webapi-getting-started.md)

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)Active Directory etki alanını da görebilirsiniz.

## <a name="how-your-project-is-modified"></a>Projeniz nasıl değiştirilir?

Bağlı hizmeti sihirbaza eklediğinizde, Visual Studio projenize Azure Active Directory ve ilişkili referanslar ekler. Projenizdeki yapılandırma dosyaları ve kod dosyaları da Azure AD desteği eklemek üzere değiştirilir. Visual Studio'nun yaptığı belirli değişiklikler proje türüne bağlıdır. Ayrıntılar için aşağıdaki makalelere bakın:

- [.NET MVC projeme ne oldu?](vs-active-directory-dotnet-what-happened.md)
- [Web API projeme ne oldu?](vs-active-directory-webapi-what-happened.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Etkin Dizin için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [ASP.NET bir web uygulamasına Microsoft ile oturum açma ekleme](quickstart-v2-aspnet-webapp.md)
