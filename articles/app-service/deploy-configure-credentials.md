---
title: Dağıtım kimlik bilgilerini yapılandırma
description: Azure App Service dağıtım kimlik bilgileri türlerini ve bunların nasıl yapılandırılacağını ve kullanılacağını öğrenin.
ms.topic: article
ms.date: 08/14/2019
ms.reviewer: byvinyal
ms.custom: seodec18
ms.openlocfilehash: 739325f66594667c6973df356e2bcf26a3eb056d
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89300281"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Azure App Service için dağıtım kimlik bilgilerini yapılandırma
[Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) , [Yerel git dağıtımı](deploy-local-git.md) ve [FTP/S dağıtımı](deploy-ftp.md)için iki tür kimlik bilgilerini destekler. Bu kimlik bilgileri, Azure aboneliğiniz kimlik bilgilerinizle aynı değildir.

[!INCLUDE [app-service-deploy-credentials](../../includes/app-service-deploy-credentials.md)]

## <a name="configure-user-level-credentials"></a><a name="userscope"></a>Kullanıcı düzeyi kimlik bilgilerini yapılandırma

Kullanıcı düzeyi kimlik bilgilerinizi, herhangi bir uygulamanın [kaynak sayfasında](../azure-resource-manager/management/manage-resources-portal.md#manage-resources)yapılandırabilirsiniz. Bu kimlik bilgilerini yapılandırdığınız uygulamayı ne olursa olsun, tüm uygulamalar ve Azure hesabınızdaki tüm abonelikler için geçerlidir. 

### <a name="in-the-cloud-shell"></a>Cloud Shell

[Cloud Shell](https://shell.azure.com)dağıtım kullanıcısını yapılandırmak için [az WebApp Deployment User set](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) komutunu çalıştırın. \<username>Ve öğesini \<password> bir dağıtım kullanıcısı Kullanıcı adı ve parolasıyla değiştirin. 

- Kullanıcı adı Azure içinde benzersiz olmalıdır ve yerel git gönderimleri için ' @ ' sembolünü içermemelidir. 
- Parola en az sekiz karakter uzunluğunda olmalıdır ve şu üç öğeden ikisi vardır: harfler, rakamlar ve semboller. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

JSON çıktısı parolayı olarak gösterir `null` . `'Conflict'. Details: 409` hatası alırsanız kullanıcı adını değiştirin. `'Bad Request'. Details: 400` hatası alırsanız daha güçlü bir parola kullanın. 

### <a name="in-the-portal"></a>Portalda

Azure portal, dağıtım kimlik bilgileri sayfasına erişebilmek için en az bir uygulamanız olması gerekir. Kullanıcı düzeyi kimlik bilgilerinizi yapılandırmak için:

1. [Azure Portal](https://portal.azure.com), sol menüden **uygulama hizmetleri**  >  **\<any_app>**  >  **Dağıtım Merkezi**  >  **FTP**  >  **panosu**' nu seçin.

    ![Azure Uygulama Hizmetleri 'nde dağıtım merkezi 'nden FTP panosunu nasıl seçkullanabileceğinizi gösterir.](./media/app-service-deployment-credentials/access-no-git.png)

    Ya da git dağıtımını zaten yapılandırdıysanız **App Services**  >  ** &lt; any_app>**  >  **Deployment Center**  >  **FTP/Credentials**' ı seçin.

    ![Yapılandırılmış git dağıtımınız için Azure Uygulama Hizmetleri 'ndeki dağıtım merkezi 'nden FTP panosunu nasıl seçkullanabileceğinizi gösterir.](./media/app-service-deployment-credentials/access-with-git.png)

2. **Kullanıcı kimlik bilgileri**' ni seçin, Kullanıcı adını ve parolayı yapılandırın ve ardından **kimlik bilgilerini kaydet**' i seçin.

Dağıtım kimlik bilgilerinizi ayarladıktan sonra, *Git* dağıtım Kullanıcı adını uygulamanızın **genel bakış** sayfasında bulabilirsiniz,

![Uygulamanızın genel bakış sayfasında git dağıtım Kullanıcı adının nasıl bulunacağını gösterir.](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

Git dağıtımı yapılandırılırsa, sayfada bir **Git/dağıtım Kullanıcı adı**gösterilir; Aksi takdirde, bir **FTP/Deployment Kullanıcı adı**.

> [!NOTE]
> Azure, Kullanıcı düzeyi dağıtım parolanızı göstermez. Parolayı unutursanız, bu bölümdeki adımları izleyerek kimlik bilgilerinizi sıfırlayabilirsiniz.
>
> 

## <a name="use-user-level-credentials-with-ftpftps"></a>FTP/FTPS ile Kullanıcı düzeyi kimlik bilgilerini kullanma

Kullanıcı düzeyindeki kimlik bilgilerini kullanarak bir FTP/FTPS uç noktası için kimlik doğrulaması, aşağıdaki biçimde bir Kullanıcı adı talep ediyor: `<app-name>\<user-name>`

Kullanıcı düzeyi kimlik bilgileri, belirli bir kaynak değil kullanıcıya bağlı olduğundan, oturum açma işlemini doğru uygulama uç noktasına yönlendirmek için Kullanıcı adının bu biçimde olması gerekir.

## <a name="get-and-reset-app-level-credentials"></a><a name="appscope"></a>Uygulama düzeyi kimlik bilgilerini edinme ve sıfırlama
Uygulama düzeyi kimlik bilgilerini almak için:

1. [Azure Portal](https://portal.azure.com), sol menüden **uygulama hizmetleri**  >  ** &lt; any_app>**  >  **Dağıtım Merkezi**  >  **FTP/kimlik bilgileri**' ni seçin.

2. **Uygulama kimlik bilgilerini**seçin ve Kullanıcı adını veya parolayı kopyalamak için **Kopyala** bağlantısını seçin.

Uygulama düzeyi kimlik bilgilerini sıfırlamak için aynı iletişim kutusunda **kimlik bilgilerini Sıfırla** ' yı seçin.

## <a name="disable-basic-authentication"></a>Temel kimlik doğrulamasını devre dışı bırak

Bazı kuruluşların güvenlik gereksinimlerini karşılaması gerekir ve FTP veya WebDeploy aracılığıyla erişimi devre dışı bırakır. Bu şekilde, kuruluşun üyeleri uygulama hizmetlerine yalnızca Azure Active Directory (Azure AD) tarafından denetlenen API 'Ler aracılığıyla erişebilir.

### <a name="ftp"></a>FTP

Siteye FTP erişimini devre dışı bırakmak için aşağıdaki CLı komutunu çalıştırın. Yer tutucuları kaynak grubunuz ve site adınızla değiştirin. 

```bash
az resource update --resource-group <resource-group> --name ftp --namespace Microsoft.Web --resource-type basicPublishingCredentialsPolicies --parent sites/<site-name> --set properties.allow=false
```

FTP erişiminin engellendiğini onaylamak için FileZilla gibi bir FTP istemcisi kullanarak kimlik doğrulaması yapmayı deneyebilirsiniz. Yayımlama kimlik bilgilerini almak için sitenizin genel bakış dikey penceresine gidin ve yayımlama profilini Indir ' e tıklayın. Kimlik doğrulaması için dosyanın FTP ana bilgisayar adını, Kullanıcı adını ve parolasını kullanın ve yetkili olmadığını belirten bir 401 hata yanıtı alırsınız.

### <a name="webdeploy-and-scm"></a>WebDeploy ve SCM

WebDeploy bağlantı noktası ve SCM sitesine temel kimlik doğrulaması erişimini devre dışı bırakmak için aşağıdaki CLı komutunu çalıştırın. Yer tutucuları kaynak grubunuz ve site adınızla değiştirin. 

```bash
az resource update --resource-group <resource-group> --name scm --namespace Microsoft.Web --resource-type basicPublishingCredentialsPolicies --parent sites/<site-name> --set properties.allow=false
```

WebDeploy 'de yayımlama profili kimlik bilgilerinin engellendiğini onaylamak için, [Visual Studio 2019 kullanarak bir Web uygulaması yayımlamayı](https://docs.microsoft.com/visualstudio/deployment/quickstart-deploy-to-azure?view=vs-2019)deneyin.

### <a name="disable-access-to-the-api"></a>API 'ye erişimi devre dışı bırakma

Önceki bölümde yer alan API, Azure rol tabanlı Access Control (RBAC), [özel bir rol oluşturabileceğiniz](https://docs.microsoft.com/azure/role-based-access-control/custom-roles#steps-to-create-a-custom-role) ve rol üzerinde temel kimlik doğrulaması etkinleştiremeyecek şekilde role daha düşük bir Kullanıcı atayabileceği anlamına gelir. Özel rolü yapılandırmak için [Bu yönergeleri izleyin](https://azure.github.io/AppService/2020/08/10/securing-data-plane-access.html#create-a-custom-rbac-role).

[Azure izleyici](https://azure.github.io/AppService/2020/08/10/securing-data-plane-access.html#audit-with-azure-monitor) 'yi, tüm başarılı kimlik doğrulama isteklerini denetlemek ve [Azure ilkesi](https://azure.github.io/AppService/2020/08/10/securing-data-plane-access.html#enforce-compliance-with-azure-policy) 'ni kullanarak aboneliğinizdeki tüm siteler için bu yapılandırmayı zorunlu kılmak için de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı [Yerel git](deploy-local-git.md) 'Ten veya [FTP/S](deploy-ftp.md)kullanarak dağıtmak için bu kimlik bilgilerini nasıl kullanacağınızı öğrenin.
