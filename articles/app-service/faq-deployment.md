---
title: Dağıtım SSS'leri - Azure Uygulama Hizmeti | Microsoft Dokümanlar
description: Azure Uygulama Hizmeti'nin Web Apps özelliğiiçin dağıtım hakkında sık sorulan soruların yanıtlarını alın.
author: genlin
manager: dcscontentpm
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.topic: article
ms.date: 11/01/2018
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: 25f43051f11d42c4f8565039d3168f2ae162eb37
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74671678"
---
# <a name="deployment-faqs-for-web-apps-in-azure"></a>Azure'da Web Uygulaması Dağıtımı Hakkında SSS

Bu makalede, [Azure Uygulama Hizmeti'nin Web Apps özelliğiiçin](https://azure.microsoft.com/services/app-service/web/)dağıtım sorunları yla ilgili sık sorulan soruların (SSS' ler) yanıtları bulunmaktadır.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a>Ben sadece App Service web uygulamaları ile başlıyorum. Kodumu nasıl yayımlarım?

Web uygulama kodunuzu yayınlamak için bazı seçenekler şunlardır:

*   Visual Studio'u kullanarak dağıtın. Visual Studio çözümüne sahipseniz, web uygulama projesine sağ tıklayın ve ardından **Yayımla'yı**seçin.
*   FTP istemcisi kullanarak dağıtın. Azure portalında, kodunuzu dağıtmak istediğiniz web uygulamasının yayımlama profilini indirin. Ardından, aynı yayımlama profili FTP kimlik bilgilerini kullanarak dosyaları \site\wwwroot'a yükleyin.

Daha fazla bilgi için [uygulamanızı Uygulama Hizmetine Dağıt'a](deploy-local-git.md)bakın.

## <a name="i-see-an-error-message-when-i-try-to-deploy-from-visual-studio-how-do-i-resolve-this-error"></a>Visual Studio'dan dağıtmaya çalıştığımda bir hata iletisi görüyorum. Bu hatayı nasıl giderebilirim?

Aşağıdaki iletiyi görürseniz, SDK'nın eski bir sürümünü kullanıyor olabilirsiniz: "Kaynak grubundaki 'Kaynak Adınız': EksikKayıtForLocation: Abonelik için kayıtlı değildir konum 'Merkezi ABD'de kaynak türü 'bileşenleri'. Bu konuma erişmek için bu sağlayıcıya yeniden kaydolun." 

Bu hatayı gidermek için [en son SDK'ya](https://azure.microsoft.com/downloads/)yükseltin. Bu iletiyi görürseniz ve en son SDK'ya sahipseniz, bir destek isteği gönderin.

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-to-app-service"></a>Visual Studio'dan App Service'e ASP.NET bir uygulamayı nasıl dağıtıyorum?
<a id="deployasp"></a>

[Azure'daki ilk ASP.NET web uygulamanızı beş dakikada oluşturma](app-service-web-get-started-dotnet.md) eğitimi, Visual Studio'yu kullanarak Uygulama Hizmeti'ndeki bir web uygulamasına nasıl ASP.NET bir web uygulaması dağıtabileceğinizi gösterir.

## <a name="what-are-the-different-types-of-deployment-credentials"></a>Farklı dağıtım kimlik bilgileri nelerdir?

Uygulama Hizmeti, yerel Git dağıtımı ve FTP/S dağıtımı için iki tür kimlik bilgilerini destekler. Dağıtım kimlik bilgilerini yapılandırma hakkında daha fazla bilgi için Uygulama [Hizmeti için dağıtım kimlik bilgilerini yapılandırın'a](deploy-configure-credentials.md)bakın.

## <a name="what-is-the-file-or-directory-structure-of-my-app-service-web-app"></a>App Service web uygulamamın dosya veya dizin yapısı nedir?

Uygulama Hizmeti uygulamanızın dosya yapısı hakkında bilgi için [Azure'daki Dosya yapısı'na](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure)bakın.

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-the-disk-when-i-try-to-ftp-my-files"></a>Dosyalarımı FTP'ye çalıştığımda "FTP Hatası 550 - Diskte yeterli alan yok" sorununu nasıl çözebilirim?

Bu iletiyi görürseniz, web uygulamanızın hizmet planında bir disk kotası ile karşınıza çıkıyor olabilirsiniz. Disk alanı gereksinimlerinize göre daha yüksek bir hizmet katmanına kadar ölçeklendirmeniz gerekebilir. Fiyatlandırma planları ve kaynak sınırları hakkında daha fazla bilgi için [Uygulama Hizmeti fiyatlandırması'na](https://azure.microsoft.com/pricing/details/app-service/)bakın.

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a>App Service web uygulamam için sürekli dağıtımı nasıl ayarlarım?

Azure DevOps, OneDrive, GitHub, Bitbucket, Dropbox ve diğer Git depoları dahil olmak üzere çeşitli kaynaklardan sürekli dağıtım ayarlayabilirsiniz. Bu seçenekler portalda mevcuttur. [Uygulama Hizmetine sürekli dağıtım,](deploy-continuous-deployment.md) sürekli dağıtımın nasıl ayarlanolacağını açıklayan yararlı bir öğreticidir.

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a>GitHub ve Bitbucket'tan sürekli dağıtım la ilgili sorunları nasıl giderebilirim?

GitHub veya Bitbucket'tan sürekli dağıtımla ilgili sorunları araştırmaya yardımcı olmak için [bkz.](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)

## <a name="i-cant-ftp-to-my-site-and-publish-my-code-how-do-i-resolve-this-issue"></a>FTP'yi siteme ve kodumu yayınlayamazım. Bu sorunu nasıl çözebilirim?

FTP sorunlarını çözmek için:

1. Doğru ana bilgisayar adını ve kimlik bilgilerini girdiğinizi doğrulayın. Farklı kimlik bilgileri türleri ve bunların nasıl kullanılacağı hakkında ayrıntılı bilgi için [Dağıtım kimlik bilgilerine](https://github.com/projectkudu/kudu/wiki/Deployment-credentials)bakın.
2. FTP bağlantı noktalarının bir güvenlik duvarı tarafından engellenmediğini doğrulayın. Bağlantı noktaları şu ayarlara sahip olmalıdır:
    * FTP denetim bağlantı noktası: 21
    * FTP veri bağlantı noktası: 989, 10001-10300

## <a name="how-do-i-publish-my-code-to-app-service"></a>Kodumu App Service'e nasıl yayınlarım?

Azure Quickstart, dağıtım yığınını ve seçtiğiniz yöntemi kullanarak uygulamanızı dağıtmanıza yardımcı olmak için tasarlanmıştır. Azure portalında Quickstart'ı kullanmak için **Dağıtım**altında uygulama hizmetinize gidin , **Hızlı Başlat'ı**seçin.

## <a name="why-does-my-app-sometimes-restart-after-deployment-to-app-service"></a>Uygulamam neden bazen Uygulama Hizmeti'ne dağıtımdan sonra yeniden başlatılır?

Uygulama dağıtımının yeniden başlatılabileceği durumlar hakkında bilgi edinmek için [dağıtım ve çalışma zamanı sorunlarına](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts")bakın. Makalede açıklandığı gibi, App Service dosyaları wwwroot klasörüne dağıtıyor. Uygulamanızı doğrudan yeniden başlatmaz.

## <a name="how-do-i-integrate-azure-devops-code-with-app-service"></a>Azure DevOps kodunu App Service ile nasıl tümleştirebilirim?

Azure DevOps ile sürekli dağıtımı kullanmak için iki seçeneğiniz vardır:

*   Git projesi kullan. Dağıtım Merkezi'ni kullanarak Uygulama Hizmeti üzerinden bağlanın.
*   Takım Temel Sürüm Denetimi (TFVC) projesini kullanın. Uygulama Hizmeti için yapı aracısını kullanarak dağıtın.

Bu seçeneklerin her ikisi için de sürekli kod dağıtımı, varolan geliştirici iş akışlarına ve iade yordamlarına bağlıdır. Daha fazla bilgi için şu makalelere bakın: 

*   [Uygulamanızın bir Azure web sitesine sürekli dağıtımını uygulayın](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs)
*   [Bir web uygulamasına dağıtılabilmek için bir Azure DevOps kuruluşu ayarlama](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)

## <a name="how-do-i-use-ftp-or-ftps-to-deploy-my-app-to-app-service"></a>Uygulamamı App Service'e dağıtmak için FTP veya FTPS'yi nasıl kullanırım?

Web uygulamanızı App Service'e dağıtmak için FTP veya FTPS kullanma hakkında bilgi için, [FTP/S kullanarak uygulamanızı Uygulama Hizmetine Dağıt'a](deploy-ftp.md)bakın.
