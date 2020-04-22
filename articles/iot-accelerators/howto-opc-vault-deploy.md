---
title: OPC Vault sertifika yönetimi hizmeti nasıl dağıtılır - Azure | Microsoft Dokümanlar
description: OPC Vault sertifika yönetimi hizmetini sıfırdan nasıl dağıtabilirsiniz?
author: mregen
ms.author: mregen
ms.date: 08/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 7ee186684b702a42335c6e1a7832cc5c761a69d0
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686933"
---
# <a name="build-and-deploy-the-opc-vault-certificate-management-service"></a>OPC Vault sertifika yönetim hizmetini oluşturun ve dağıtın

Bu makalede, Azure'da OPC Vault sertifika yönetimi hizmetinin nasıl dağıtılanılacaaçıklanmıştır.

> [!NOTE]
> Daha fazla bilgi için GitHub [OPC Vault deposuna](https://github.com/Azure/azure-iiot-opc-vault-service)bakın.

## <a name="prerequisites"></a>Ön koşullar

### <a name="install-required-software"></a>Gerekli yazılımı yükleme

Şu anda yapı ve dağıtma işlemi Windows ile sınırlıdır.
Örneklerin tümü C# .NET Standard için yazılmıştır ve dağıtım için hizmet ve örnekler oluşturmanız gerekir.
.NET Standard için ihtiyacınız olan tüm araçlar .NET Core araçlarıyla birlikte gelir. Bkz. [.NET Core ile başlayın.](https://docs.microsoft.com/dotnet/articles/core/getting-started)

1. [Yükleyin .NET Core 2.1+][dotnet-install].
2. [Docker'ı yükleyin][docker-url] (yalnızca yerel Docker yapısı gerekiyorsa isteğe bağlıdır).
4. [PowerShell için Azure komut satırı araçlarını yükleyin.][powershell-install]
5. [Azure aboneliği][azure-free]için kaydolun.

### <a name="clone-the-repository"></a>Depoyu kopyalama

Henüz yapmadıysanız, bu GitHub deposunu klonla. Bir komut istemi veya terminal açın ve aşağıdakileri çalıştırın:

```bash
git clone https://github.com/Azure/azure-iiot-opc-vault-service
cd azure-iiot-opc-vault-service 
```

Alternatif olarak, repo'yu doğrudan Visual Studio 2017'de kopyalayabilirsiniz.

### <a name="build-and-deploy-the-azure-service-on-windows"></a>Azure hizmetini Windows'da oluşturma ve dağıtma

PowerShell komut dosyası, OPC Vault microservice'i ve uygulamayı dağıtmak için kolay bir yol sağlar.

1. Repo kökünde bir PowerShell penceresi açın. 
3. Dağıt klasörüne `cd deploy`gidin.
3. Dağıtılan diğer `myResourceGroup` web sayfalarıyla çakışma olasılığı düşük olan bir ad seçin. Bu makalenin ilerleyen bölümlerinde "Zaten kullanımda olan web sitesi adı" bölümüne bakın.
5. Etkileşimli yükleme `.\deploy.ps1` için dağıtımı başlatın veya tam komut satırı girin:  
`.\deploy.ps1  -subscriptionName "MySubscriptionName" -resourceGroupLocation "East US" -tenantId "myTenantId" -resourceGroupName "myResourceGroup"`
7. Bu dağıtımla geliştirmeyi planlıyorsanız, Swagger UI'yi etkinleştirmek ve hata ayıklama yapılarını dağıtmak için ekleyin. `-development 1`
6. Aboneliğinizde oturum açmak ve ek bilgi sağlamak için komut dosyasındaki yönergeleri izleyin.
9. Başarılı bir yapı ve dağıtım işleminden sonra aşağıdaki iletiyi görmeniz gerekir:
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

   > [!NOTE]
   > Sorun durumunda, makalenin daha sonra "Sorun Giderme dağıtım hataları" bölümüne bakın.

8. En sevdiğiniz tarayıcıyı açın ve uygulama sayfasını açın:`https://myResourceGroup.azurewebsites.net`
8. Dağıtımdan sonra ısınması için web uygulamasına ve OPC Vault mikro hizmetine birkaç dakika verin. Web sayfası, ilk yanıtları alana kadar bir dakikaya kadar ilk kullanıma bağlı kalabilir.
11. Swagger API'ye göz atmak için:`https://myResourceGroup-service.azurewebsites.net`
13. Dotnet ile yerel bir GDS `.\myResourceGroup-gds.cmd`sunucusu başlatmak için başlatın. Docker ile `.\myResourceGroup-dockergds.cmd`başla.

Bir yapıyı tam olarak aynı ayarlarla yeniden dağıtmak mümkündür. Böyle bir işlemin tüm uygulama sırlarını yenileyebileceğini ve Azure Etkin Dizin (Azure AD) uygulama kayıtlarındaki bazı ayarları sıfırlayabildiğini unutmayın.

Ayrıca, sadece web uygulaması ikililerini yeniden dağıtmak da mümkündür. Parametre `-onlyBuild 1`ile, hizmet ve uygulamanın yeni zip paketleri web uygulamalarına dağıtılır.

Başarılı dağıtımdan sonra, hizmetleri kullanmaya başlayabilirsiniz. Bkz. [OPC Vault sertifika yönetimi hizmetini yönetin.](howto-opc-vault-manage.md)

## <a name="delete-the-services-from-the-subscription"></a>Hizmetleri abonelikten silme

Bunu yapmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hizmetin dağıtıldığı kaynak grubuna gidin.
3. **Kaynak grubunu sil**'i seçip onaylayın.
4. Kısa bir süre sonra, dağıtılan tüm hizmet bileşenleri silinir.
5. Azure **Active Directory** > **App kayıtlarına**gidin.
6. Dağıtılan her kaynak grubu için listelenen üç kayıt olmalıdır. Kayıtlarda aşağıdaki isimler `resourcegroup-client`vardır: `resourcegroup-module` `resourcegroup-service`, , . Her kaydı ayrı olarak silin.

Şimdi dağıtılan tüm bileşenler kaldırılır.

## <a name="troubleshooting-deployment-failures"></a>Sorun giderme dağıtım hataları

### <a name="resource-group-name"></a>Kaynak grubu adı

Kısa ve basit bir kaynak grubu adı kullanın. Ad, kaynakları ve hizmet URL önekini adlandırmak için de kullanılır. Bu nedenle, kaynak adlandırma gereksinimlerine uyması gerekir.  

### <a name="website-name-already-in-use"></a>Zaten kullanılmakta olan web sitesi adı

Web sitesinin adının zaten kullanılıyor olması mümkündür. Farklı bir kaynak grubu adı kullanmanız gerekir. Dağıtım komut dosyası tarafından kullanılan ana bilgisayar\/adları şunlardır: https: /resourcegroupname.azurewebsites.net ve https:\//resourgroupname-service.azurewebsites.net.
Hizmetlerin diğer adları kısa ad karmalarının birleşimi tarafından oluşturulur ve diğer hizmetlerle çakışma olasılığı düşüktür.

### <a name="azure-ad-registration"></a>Azure AD kaydı 

Dağıtım komut dosyası, Azure AD'de üç Azure REKLAM uygulamasını kaydetmeye çalışır. Seçili Azure AD kiracısındaki izinlerinize bağlı olarak, bu işlem başarısız olabilir. İki seçenek vardır:

- Kiracılar listesinden bir Azure AD kiracıseçtiyseniz, komut dosyasını yeniden başlatın ve listeden farklı bir tane seçin.
- Alternatif olarak, başka bir abonelikte özel bir Azure AD kiracısı dağıtın. Komut dosyasını yeniden başlatın ve kullanmak için seçin.

## <a name="deployment-script-options"></a>Dağıtım komut dosyası seçenekleri

Komut dosyası aşağıdaki parametreleri alır:


```
-resourceGroupName
```

Bu, varolan veya yeni bir kaynak grubunun adı olabilir.

```
-subscriptionId
```


Bu, kaynakların dağıtılacayacağı abonelik kimliğidir. İsteğe bağlıdır.

```
-subscriptionName
```


Alternatif olarak, abonelik adını kullanabilirsiniz.

```
-resourceGroupLocation
```


Bu bir kaynak grubu konumudur. Belirtilirse, bu parametre bu konumda yeni bir kaynak grubu oluşturmaya çalışır. Bu parametre de isteğe bağlıdır.


```
-tenantId
```


Bu, kullanılacak Azure AD kiracısıdır. 

```
-development 0|1
```

Bu geliştirme için dağıtmak tır. Hata ayıklama oluşturmayı kullanın ve ASP.NET ortamını Geliştirme olarak ayarlayın. Uygulamayı `.publishsettings` ve hizmeti doğrudan dağıtmasına izin vermek için Visual Studio 2017'de içe aktarılabilmek için oluşturun. Bu parametre de isteğe bağlıdır.

```
-onlyBuild 0|1
```

Bu, yalnızca web uygulamalarını yeniden oluşturmak ve yeniden dağıtmak ve Docker kapsayıcılarını yeniden oluşturmaktır. Bu parametre de isteğe bağlıdır.

[azure-free]:https://azure.microsoft.com/free/
[powershell-install]:https://azure.microsoft.com/downloads/#powershell
[docker-url]: https://www.docker.com/
[dotnet-install]: https://www.microsoft.com/net/learn/get-started

## <a name="next-steps"></a>Sonraki adımlar

Artık OPC Vault'u sıfırdan dağıtmayı öğrendiğiniz için şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [OPC Vault'u Yönet](howto-opc-vault-manage.md)
