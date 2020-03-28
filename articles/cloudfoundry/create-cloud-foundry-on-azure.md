---
title: Azure'da Önemli Bulut Döküm kümesi oluşturma
description: Azure'da Önemli Bulut Döküm (PCF) kümesini sağlamak için gereken parametreleri nasıl ayarlayabilirsiniz öğrenin
services: Cloud Foundry
documentationcenter: CloudFoundry
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/13/2018
ms.devlang: ''
ms.service: azure
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: 5d4ac5435281f521c71556123f77d737ee6916e9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73161786"
---
# <a name="create-a-pivotal-cloud-foundry-cluster-on-azure"></a>Azure'da Önemli Bulut Döküm kümesi oluşturma

Bu öğretici, Azure'da Bir Pivotal Cloud Foundry (PCF) kümesini sağlamak için ihtiyacınız olan parametreleri oluşturmak ve oluşturmak için hızlı adımlar sağlar. Pivotal Cloud Foundry çözümlerini bulmak için Azure [Marketi'nde](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry)bir arama yapın.

![Azure'da Önemli Bulut Dökümhane'de Arama](media/deploy/pcf-marketplace.png)


## <a name="generate-an-ssh-public-key"></a>SSH ortak anahtarı oluşturma

Windows, Mac veya Linux kullanarak ortak güvenli bir kabuk (SSH) anahtarı oluşturmanın birkaç yolu vardır.

```Bash
ssh-keygen -t rsa -b 2048
```

Daha fazla bilgi için bkz: [Azure'da Windows ile SSH tuşlarını kullan.](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

> [!NOTE]
>
> Bir hizmet sorumlusu oluşturmak için sahibinin hesap iznine ihtiyacınız var. Ayrıca, hizmet ilkesini oluşturmak için bir komut dosyası yazabilirsiniz. Örneğin, Azure CLI az [reklam sp create-for-rbac'ı](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest)kullanabilirsiniz.

1. Azure hesabınızda oturum açın.

    `az login`

    ![Azure CLI girişi](media/deploy/az-login-output.png )
 
    **Abonelik kimliğiniz**olarak "id" değerini kopyalayın ve daha sonra kullanmak üzere "kiracı Kimliği" değerini kopyalayın.

2. Bu yapılandırma için varsayılan aboneliğinizi ayarlayın.

    `az account set -s {id}`

3. PCF'niz için bir Azure Active Directory uygulaması oluşturun. Benzersiz bir alfasayısal parola belirtin. Parolayı daha sonra kullanmak üzere **istemcinizGizli** olarak saklayın.

    `az ad app create --display-name "Svc Principal for OpsManager" --password {enter-your-password} --homepage "{enter-your-homepage}" --identifier-uris {enter-your-homepage}`

    Çıktıdaki "appId" değerini daha sonra kullanmak üzere **müşteri kimliğiniz** olarak kopyalayın.

    > [!NOTE]
    >
    > Kendi uygulama giriş sayfanızı ve uri tanımlayıcınızı seçin,\:örneğin\.http //www contoso.com.

4. Yeni uygulama kimliğinizle bir hizmet müdürü oluşturun.

    `az ad sp create --id {appId}`

5. Hizmet sorumlunuzun izin rolünü Katkıda bulunan olarak belirleyin.

    `az role assignment create --assignee "{enter-your-homepage}" --role "Contributor"`

    Ya da kullanabilirsiniz

    `az role assignment create --assignee {service-principal-name} --role "Contributor"`

    ![Hizmet temel rol ataması](media/deploy/svc-princ.png )

6. Uygulama kimliğini, parolayı ve kiracı kimliğini kullanarak servis müdürünüzde başarılı bir şekilde oturum açabileceğinizi doğrulayın.

    `az login --service-principal -u {appId} -p {your-password}  --tenant {tenantId}`

7. Aşağıdaki biçimde bir .json dosyası oluşturun. Abonelik **kimliğini,** **kiracı kimliğini,** **clientID'yi**ve daha önce kopyaladığınız **clientSecret** değerlerini kullanın. Dosyayı kaydedin.

    ```json
    {
        "subscriptionID": "{enter-your-subscription-Id-here}",
        "tenantID": "{enter-your-tenant-Id-here}",
        "clientID": "{enter-your-app-Id-here}",
        "clientSecret": "{enter-your-key-here}"
    }
    ```

## <a name="get-the-pivotal-network-token"></a>Önemli Ağ belirteci alın

1. [Pivotal Network](https://network.pivotal.io) hesabınıza kaydolun veya oturum açın.
2. Sayfanın sağ üst köşesindeki profil adınızı seçin. **Profili Edit'i**seçin.
3. Sayfanın altına kaydırın ve **LEGACY API TOKEN** değerini kopyalayın. Bu değer, daha sonra kullandığınız **Önemli Ağ Belirteç** değerinizdir.

## <a name="provision-your-cloud-foundry-cluster-on-azure"></a>Bulut Döküm kümenizi Azure'da sağlama

Artık [Azure'da Pivotal Cloud Foundry kümenizi](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry)sağlamak için ihtiyacınız olan tüm parametrelere sahipsiniz.
Parametreleri girin ve PCF kümenizi oluşturun.

## <a name="verify-the-deployment-and-sign-in-to-the-pivotal-ops-manager"></a>Dağıtımı doğrulayın ve Pivotal Ops Yöneticisi'nde oturum açın

1. PCF kümeniz bir dağıtım durumu gösterir.

    ![Azure dağıtım durumu](media/deploy/deployment.png )

2. PCF Ops Manager'ınız için kimlik bilgilerini almak için soldaki gezintide **Dağıtımlar** bağlantısını seçin. Sonraki sayfada **Dağıtım Adı'nı** seçin.
3. Soldaki gezintide, PCF Ops Manager'ın URL'sini, kullanıcı adını ve parolasını görüntülemek için **Çıktılar** bağlantısını seçin. "OPSMAN-FQDN" değeri URL'dir.
 
    ![Bulut Dökümhanesi dağıtım çıktısı](media/deploy/deploy-outputs.png )
 
4. URL'yi bir web tarayıcısında başlatın. Oturum açma için önceki adımdaki kimlik bilgilerini girin.

    ![Önemli oturum açma sayfası](media/deploy/pivotal-login.png )
         
    > [!NOTE]
    >
    > Internet Explorer tarayıcısı "Site güvenli değil" uyarı iletisi nedeniyle başarısız olursa, **daha fazla bilgi** seçin ve web sayfasına gidin. Firefox için **Advance'ı** seçin ve devam etmek için sertifikayı ekleyin.

5. PCF Ops Manager'ınız dağıtılan Azure örneklerini görüntüler. Artık uygulamalarınızı buradan dağıtabilir ve yönetebilirsiniz.
               
    ![Pivotal'da Dağıtılan Azure örneği](media/deploy/ops-mgr.png )
 
