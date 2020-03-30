---
title: Dağıtım kimlik bilgilerini yapılandırma
description: Azure Uygulama Hizmeti'nde ne tür dağıtım kimlik bilgileri olduğunu ve bunları nasıl yapılandırıp kullanacağınızı öğrenin.
ms.topic: article
ms.date: 08/14/2019
ms.reviewer: byvinyal
ms.custom: seodec18
ms.openlocfilehash: a9d875e2c3899fa91b9cc41c0ee3b5a93ec5b8c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266084"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Azure Uygulama Hizmeti için dağıtım kimlik bilgilerini yapılandırma
[Azure Uygulama Hizmeti,](https://go.microsoft.com/fwlink/?LinkId=529714) yerel [Git dağıtımı](deploy-local-git.md) ve [FTP/S dağıtımı](deploy-ftp.md)için iki tür kimlik bilgilerini destekler. Bu kimlik bilgileri, Azure abonelik kimlik bilgilerinizle aynı değildir.

* **Kullanıcı düzeyindekimlik bilgileri:** Tüm Azure hesabı için bir kimlik bilgileri kümesi. Azure hesabının erişim izni olan herhangi bir uygulama için, herhangi bir abonelikte Uygulama Hizmeti'ne dağıtmak için kullanılabilir. Gui portalında su yüzüne çıkan varsayılan kümedir (uygulamanın [kaynak sayfasının](../azure-resource-manager/management/manage-resources-portal.md#manage-resources) **Genel Görünümü** ve **Özellikleri** gibi). Bir kullanıcıya Role-Based Access Control (RBAC) veya coadmin izinleri aracılığıyla uygulama erişimi verildiğinde, bu kullanıcı erişim iptal edilene kadar kendi kullanıcı düzeyindeki kimlik bilgilerini kullanabilir. Bu kimlik bilgilerini diğer Azure kullanıcılarıyla paylaşmayın.

* **Uygulama düzeyindekimlik bilgileri:** her uygulama için bir kimlik bilgileri kümesi. Yalnızca bu uygulamaya dağıtmak için kullanılabilir. Her uygulamanın kimlik bilgileri uygulama oluşturmada otomatik olarak oluşturulur. Bunlar el ile yapılandırılamaz, ancak her zaman sıfırlanabilir. Bir kullanıcıya (RBAC) uygulama düzeyindeki kimlik bilgilerine erişim hakkı verilebilmek için, bu kullanıcının uygulamada katkıda bulunan veya daha yüksek olması gerekir (Web Sitesi Katılımcısı yerleşik rolü dahil). Okuyucuların yayımlamasına izin verilmez ve bu kimlik bilgilerine erişemez.

## <a name="configure-user-level-credentials"></a><a name="userscope"></a>Kullanıcı düzeyinde kimlik bilgilerini yapılandırma

Kullanıcı düzeyindeki kimlik bilgilerinizi herhangi bir uygulamanın [kaynak sayfasında](../azure-resource-manager/management/manage-resources-portal.md#manage-resources)yapılandırabilirsiniz. Bu kimlik bilgilerini hangi uygulamada yapılandırdığınızdan bağımsız olarak, Azure hesabınızdaki tüm uygulamalar ve tüm abonelikler için geçerlidir. 

### <a name="in-the-cloud-shell"></a>Bulut Kabuğunda

[Bulut Kabuğu'ndaki](https://shell.azure.com)dağıtım kullanıcısını yapılandırmak için [az webapp dağıtım kullanıcı kümesi](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) komutunu çalıştırın. Kullanıcı \<adı> \<ve parola> dağıtım kullanıcı adı ve parolasıyla değiştirin. 

- Kullanıcı adı Azure içinde benzersiz olmalıdır ve yerel Git itmeleri için '@' simgesi içermemelidir. 
- Parola, aşağıdaki üç öğeden ikisi olan en az sekiz karakter uzunluğunda olmalıdır: harfler, sayılar ve semboller. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

JSON çıkışı parolayı `null`. `'Conflict'. Details: 409` hatası alırsanız kullanıcı adını değiştirin. `'Bad Request'. Details: 400` hatası alırsanız daha güçlü bir parola kullanın. 

### <a name="in-the-portal"></a>Portalda

Azure portalında, dağıtım kimlik bilgileri sayfasına erişemeden önce en az bir uygulamanız olması gerekir. Kullanıcı düzeyindekimlik bilgilerinizi yapılandırmak için:

1. Azure [portalında,](https://portal.azure.com)sol menüden, **Dağıtım merkezi** > **FTP** > **\<Pano'>**  >  **uygulama hizmetleri** > any_app>seçin.**Dashboard**

    ![](./media/app-service-deployment-credentials/access-no-git.png)

    Veya, Git dağıtımını zaten yapılandırmışsanız, **Uygulama Hizmetleri** > **&lt;any_app>**  >  **Dağıtım merkezi** > **FTP/Credentials'ı**seçin.

    ![](./media/app-service-deployment-credentials/access-with-git.png)

2. **Kullanıcı Kimlik Bilgilerini**seçin, kullanıcı adını ve parolayı yapılandırın ve ardından Kimlik Bilgilerini **Kaydet'i**seçin.

Dağıtım kimlik bilgilerinizi ayarladıktan sonra, uygulamanızın **Genel Bakış** sayfasında *Git* dağıtım kullanıcı adını bulabilirsiniz,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

Git dağıtımı yapılandırılırsa, sayfada **git/dağıtım kullanıcı adı**gösterilmektedir; aksi takdirde, bir **FTP/dağıtım kullanıcı adı.**

> [!NOTE]
> Azure, kullanıcı düzeyinde dağıtım parolanızı göstermez. Parolayı unutursanız, bu bölümdeki adımları izleyerek kimlik bilgilerinizi sıfırlayabilirsiniz.
>
> 

## <a name="use-user-level-credentials-with-ftpftps"></a>FTP/FTPS ile kullanıcı düzeyinde kimlik bilgilerini kullanma

Kullanıcı düzeyindekimlik bilgilerini kullanarak FTP/FTPS bitiş noktasına doğrulanması aşağıdaki biçimde bir kullanıcı adı gerektirir:`<app-name>\<user-name>`

Kullanıcı düzeyindeki kimlik bilgileri belirli bir kaynağa değil kullanıcıya bağlı olduğundan, oturum açma eylemini doğru uygulama bitiş noktasına yönlendirmek için kullanıcı adı bu biçimde olmalıdır.

## <a name="get-and-reset-app-level-credentials"></a><a name="appscope"></a>Uygulama düzeyindeki kimlik bilgilerini alın ve sıfırla
Uygulama düzeyindeki kimlik bilgilerini almak için:

1. Azure [portalında,](https://portal.azure.com)sol menüden **Uygulama Hizmetleri** > **&lt;any_app>**  >  **Dağıtım merkezi** > **FTP/Credentials'ı**seçin.

2. **Uygulama Kimlik Bilgilerini**seçin ve kullanıcı adını veya parolayı kopyalamak için **Kopyala** bağlantısını seçin.

Uygulama düzeyindeki kimlik bilgilerini sıfırlamak için, aynı iletişim kutusunda **Kimlik Bilgilerini Sıfırla'yı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı [yerel Git'den](deploy-local-git.md) veya [FTP/S'yi](deploy-ftp.md)kullanmak için bu kimlik bilgilerini nasıl kullanacağınızı öğrenin.
