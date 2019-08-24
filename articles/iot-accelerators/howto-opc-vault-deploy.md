---
title: OPC Kasası sertifika yönetimi hizmetini dağıtma-Azure | Microsoft Docs
description: OPC Kasası sertifika yönetimi hizmetini sıfırdan dağıtma.
author: mregen
ms.author: mregen
ms.date: 08/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 347e7c2aa2ff4fb4f188847b81d03006c1909166
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69997639"
---
# <a name="build-and-deploy-the-opc-vault-certificate-management-service"></a>OPC Kasası sertifika yönetimi hizmetini derleme ve dağıtma

Bu makalede, OPC Kasası sertifika yönetimi hizmetinin Azure 'da nasıl dağıtılacağı açıklanır.

> [!NOTE]
> Dağıtım ayrıntıları ve yönergeleri hakkında daha fazla bilgi için GitHub [OPC Kasası deposuna](https://github.com/Azure/azure-iiot-opc-vault-service)bakın.

## <a name="prerequisites"></a>Önkoşullar

### <a name="install-required-software"></a>Gerekli yazılımları yükler

Şu anda derleme ve dağıtma işlemi Windows ile sınırlıdır.
Örnek, .NET Standard için C# yazılmıştır ve hizmet ve dağıtım örnekleri oluşturmak için gereklidir.
.NET Standard için ihtiyacınız olan tüm araçlar .Net Core araçlarıyla birlikte gelir. İhtiyacınız olan şeyler için [buraya](https://docs.microsoft.com/dotnet/articles/core/getting-started) bakın.

1. [.NET Core 2.1 + 'Yi yükler][dotnet-install].
2. [Docker 'ı yükler][docker-url] (isteğe bağlı, yalnızca yerel Docker derlemesi gerekliyse).
4. [PowerShell Için Azure komut satırı araçlarını][powershell-install]yükler.
5. Bir [Azure aboneliğine][azure-free]kaydolun.

### <a name="clone-the-repository"></a>Depoyu kopyalama

Henüz yapmadıysanız, bu GitHub deposunu kopyalayın.  Bir komut istemi veya Terminal açın ve şunu çalıştırın:

```bash
git clone https://github.com/Azure/azure-iiot-opc-vault-service
cd azure-iiot-opc-vault-service 
```

veya depoyu doğrudan Visual Studio 2017 ' de klonlayın.

### <a name="build-and-deploy-the-azure-service-on-windows"></a>Windows üzerinde Azure hizmetini derleme ve dağıtma

PowerShell betiği OPC Kasası mikro hizmetini ve uygulamayı dağıtmanın kolay bir yolunu sağlar.<br>

1. Depo kökünde bir PowerShell penceresi açın. 
3. Dağıtım klasörüne git`cd deploy`
3. Diğer dağıtılan Web sayfalarıyla `myResourceGroup` çakışmaya neden olması olası olmayan bir ad seçin. Bkz [](#website-name-already-in-use) . Web sayfası adları, kaynak grubunun adına göre nasıl seçilir.
5. Etkileşimli yükleme için ile `.\deploy.ps1` dağıtımı başlatma<br>
ya da bir tam komut satırı girin:  
`.\deploy.ps1  -subscriptionName "MySubscriptionName" -resourceGroupLocation "East US" -tenantId "myTenantId" -resourceGroupName "myResourceGroup"`
7. Bu dağıtım ile geliştirmeyi planlıyorsanız, Swagger Kullanıcı arabirimini etkinleştirmek `-development 1` ve hata ayıklama yapılarını dağıtmak için ekleyin.
6. Aboneliğinizde oturum açmak ve ek bilgi sağlamak için betikteki yönergeleri izleyin.
9. Başarılı bir derleme ve dağıtım işleminden sonra şu iletiyi görmeniz gerekir:
   ```
   To access the web client go to:
   https://myResourceGroup.azurewebsites.net

   To access the web service go to:
   https://myResourceGroup-service.azurewebsites.net

   To start the local docker GDS server:
   .\myResourceGroup-dockergds.cmd

   To start the local dotnet GDS server:
   .\myResourceGroup-gds.cmd
   ```

Sorunlarla karşılaşalım durumunda [aşağıdaki](#troubleshooting-deployment-failures)adımları izleyin.

8. En sevdiğiniz tarayıcıyı açın ve uygulama sayfasını açın:`https://myResourceGroup.azurewebsites.net`
8. Web uygulamasına ve OPC Kasası mikro hizmetine dağıtımdan sonra ısınma için birkaç dakika verin. Web giriş sayfası, ilk yanıtları yapana kadar bir dakika kadar ilk kullanımda askıda kalabilir.
11. Swagger API 'Si açık ' a göz atmak için:`https://myResourceGroup-service.azurewebsites.net`
13. DotNet Start `.\myResourceGroup-gds.cmd` ile veya Docker Start `.\myResourceGroup-dockergds.cmd`ile yerel bir GDS sunucusunu başlatmak için.

Bir derlemeyi, tam olarak aynı ayarlarla yeniden dağıtmak mümkündür. Bu tür bir işlemin tüm uygulama gizli dizilerini yenilediğini ve Azure Active Directory (Azure AD) uygulama kayıtlarında bazı ayarları sıfırladığının farkında olun.

Yalnızca Web uygulaması ikili dosyalarını yeniden dağıtmak da mümkündür. Parametresi `-onlyBuild 1` ile hizmetin yeni ZIP paketleri ve uygulama Web uygulamalarına dağıtılır.

Dağıtım başarılı olduktan sonra Hizmetleri kullanmaya başlayabilirsiniz: [OPC Kasası sertifika yönetimi hizmetini yönetme](howto-opc-vault-manage.md)

## <a name="delete-the-certificate-management-service-from-the-subscription"></a>Sertifika Yönetimi hizmetini abonelikten silme

1. Azure portal oturum açın: `https://portal.azure.com`.
2. Hizmetin dağıtıldığı kaynak grubuna gidin.
3. Seçin `Delete resource group` ve onaylayın.
4. Daha kısa bir süre sonra dağıtılan tüm hizmet bileşenleri silinir.
5. Şimdi adresine gidin `Azure Active Directory/App registrations`.
6. Şu adlarla dağıtılan her kaynak grubu için üç kayıt listelenmelidir: `resourcegroup-client`, `resourcegroup-module`, `resourcegroup-service`.
Her kaydın ayrı olarak silinmesi gerekir.
7. Artık dağıtılan tüm bileşenler kaldırılır.

## <a name="troubleshooting-deployment-failures"></a>Dağıtım hatalarıyla ilgili sorunları giderme

### <a name="resource-group-name"></a>Kaynak grubu adı

Kısa ve basit kaynak grubu adı kullandığınızdan emin olun.  Ad, kaynakları ve hizmet URL önekini adlandırmak için de kullanılır ve bu nedenle kaynak adlandırma gereksinimleriyle uyumlu olmalıdır.  

### <a name="website-name-already-in-use"></a>Web sitesi adı zaten kullanımda

Web sitesinin adı zaten kullanımda olabilir.  Bu hatayla karşılaşırsanız, farklı bir kaynak grubu adı kullanmanız gerekir. Dağıtım betiği tarafından kullanılan ana bilgisayar adları şunlardır: https://resourcegroupname.azurewebsites.net ve. https://resourgroupname-service.azurewebsites.net
Diğer hizmet adları, kısa ad karmalarının birleşimi ile oluşturulmuştur ve diğer hizmetlerle çakışmadan daha düşüktür.

### <a name="azure-active-directory-azure-ad-registration"></a>Azure Active Directory (Azure AD) kaydı 

Dağıtım betiği, Azure Active Directory üç Azure AD uygulamasını kaydetmeye çalışır.  
Seçili Azure AD kiracısındaki izinlerinize bağlı olarak, bu işlem başarısız olabilir.   İki seçenek vardır:

1. Kiracılar listesinden bir Azure AD kiracısı seçerseniz, betiği yeniden başlatın ve listeden farklı bir tane seçin.
2. Alternatif olarak, özel bir Azure AD kiracısını başka bir abonelikte dağıtın, betiği yeniden başlatın ve kullanmayı seçin.

## <a name="deployment-script-options"></a>Dağıtım betiği seçenekleri

Betik aşağıdaki parametreleri alır:


```
-resourceGroupName
```

Var olan veya yeni bir kaynak grubunun adı olabilir.

```
-subscriptionId
```


İsteğe bağlı, kaynakların dağıtılacağı abonelik KIMLIĞI.

```
-subscriptionName
```


Ya da başka bir abonelik adı.

```
-resourceGroupLocation
```


İsteğe bağlı, bir kaynak grubu konumu. Belirtilmişse, bu konumda yeni bir kaynak grubu oluşturmaya çalışır.


```
-tenantId
```


Kullanılacak Azure AD kiracısı. 

```
-development 0|1
```

Geliştirme için dağıtmak üzere isteğe bağlı. Hata ayıklama derlemesini kullanın ve ASP.Net ortamını geliştirme olarak ayarlayın. Uygulamayı ve hizmeti doğrudan dağıtmasını sağlamak için Visual Studio 2017 ' de içeri aktarma için '. publishsettings ' oluşturun.

```
-onlyBuild 0|1
```

İsteğe bağlı, yalnızca Web uygulamalarını yeniden dağıtmak ve Docker kapsayıcılarını yeniden derlemek için.

[azure-free]:https://azure.microsoft.com/free/
[powershell-install]:https://azure.microsoft.com/downloads/#powershell
[docker-url]: https://www.docker.com/
[dotnet-install]: https://www.microsoft.com/net/learn/get-started

## <a name="next-steps"></a>Sonraki adımlar

Artık OPC kasasını sıfırdan dağıtmayı öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [OPC kasasını yönetme](howto-opc-vault-manage.md)
