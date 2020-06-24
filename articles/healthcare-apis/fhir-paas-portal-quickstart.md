---
title: 'Hızlı başlangıç: Azure portal kullanarak FHıR için Azure API dağıtma'
description: Bu hızlı başlangıçta, FHıR için Azure API 'SI dağıtmayı ve Azure portal kullanarak ayarları yapılandırmayı öğreneceksiniz.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: e729597e9d83c4e6096fe52b577b052d94ca4799
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "84820255"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak FHıR için Azure API dağıtma

Bu hızlı başlangıçta, Azure portal kullanarak FHıR için Azure API 'yi dağıtmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-new-resource"></a>Yeni kaynak oluştur

[Azure Portal](https://portal.azure.com) açın ve **kaynak oluştur ' a** tıklayın

![Kaynak oluşturma](media/quickstart-paas-portal/portal-create-resource.png)

## <a name="search-for-azure-api-for-fhir"></a>FHıR için Azure API arama

Arama kutusuna "FHIR" yazarak FHıR için Azure API 'sini bulabilirsiniz:

![Sağlık API 'Leri arama](media/quickstart-paas-portal/portal-search-healthcare-apis.png)

## <a name="create-azure-api-for-fhir-account"></a>FHıR hesabı için Azure API oluşturma

FHıR hesabı için yeni bir Azure API 'SI oluşturmak için **Oluştur** ' u seçin:

![FHıR hesabı için Azure API oluşturma](media/quickstart-paas-portal/portal-create-healthcare-apis.png)

## <a name="enter-account-details"></a>Hesap ayrıntılarını girin

Var olan bir kaynak grubunu seçin veya yeni bir tane oluşturun, hesap için bir ad seçin ve son olarak **gözden geçir + oluştur**' a tıklayın:

![Yeni sağlık API 'si ayrıntıları](media/quickstart-paas-portal/portal-new-healthcareapi-details.png)

Oluşturma ve bekleme API 'SI dağıtımını onaylayın.

## <a name="additional-settings"></a>Ek ayarlar

Ileri ' ye tıklayın: FHıR için bu Azure API 'sine erişmesine izin verilmesi gereken yetkili, hedef kitle, kimlik nesne kimliklerini yapılandırmak için **ek ayarlar** , gerekirse fhır 'de akıllı etkinleştirme ve veritabanı üretimini yapılandırma:

- **Yetkili:** Hizmet için kimlik doğrulama yetkilisi olarak oturum açtığınız bilgisayardan farklı bir Azure AD kiracısı belirtebilirsiniz.
- **Hedef kitle:** En iyi yöntem ve varsayılan ayar, izleyicinin FHıR sunucusunun URL 'sine ayarlanlarıdır. Burada bu değişikliği yapabilirsiniz. Hedef kitle, belirtecin hedeflenen alıcıyı tanımlar. Bu bağlamda, FHıR API 'sini temsil eden bir şeye ayarlanmalıdır.
- **Izin verilen nesne kimlikleri:** FHıR için bu Azure API 'sine erişmesine izin verilmesi gereken kimlik nesnesi kimliklerini belirtebilirsiniz. [Kimlik bulma nesne kimlikleri](find-identity-object-ids.md) nasıl yapılır Kılavuzu ' nda kullanıcılar ve hizmet sorumluları IÇIN nesne kimliğini bulma hakkında daha fazla bilgi edinebilirsiniz.  
- **FHıR proxy üzerinde akıllı:** FHıR proxy üzerinde akıllı ' i etkinleştirebilirsiniz. FHıR proxy üzerinde akıllı yapılandırma hakkında ayrıntılı bilgi için bkz. fhır [proxy ÜZERINDE FHIR Smart için öğretici Azure API](https://docs.microsoft.com/azure/healthcare-apis/use-smart-on-fhir-proxy) 'si  
- **Sağlanan aktarım hızı (ru/s):** Burada, FHıR için Azure API 'niz için temel alınan veritabanı için üretilen iş ayarlarını belirtebilirsiniz. Bu ayarı daha sonra veritabanı dikey penceresinde değiştirebilirsiniz. Daha fazla ayrıntı için lütfen [veritabanı ayarlarını yapılandırma](configure-database.md) sayfasına bakın.


![İzin verilen nesne kimliklerini yapılandırma](media/quickstart-paas-portal/configure-audience.png)

## <a name="fetch-fhir-api-capability-statement"></a>FHıR API özelliği bildirisini getir

Yeni FHıR API hesabının sağlandığını doğrulamak için, bir tarayıcı göstererek bir yetenek beyanı getirin `https://<ACCOUNT-NAME>.azurehealthcareapis.com/metadata` .

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, kaynak grubunu, FHıR için Azure API 'yi ve tüm ilgili kaynakları silebilirsiniz. Bunu yapmak için, FHıR için Azure API hesabı 'nı içeren kaynak grubunu seçin, kaynak grubunu **Sil**' i seçin ve ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç kılavuzunda, FHıR için Azure API 'sini aboneliğinize dağıttınız. FHıR için Azure API 'niz üzerinde ek ayarlar ayarlamak için, ek ayarlar nasıl yapılır Kılavuzu ' na ilerleyin.

>[!div class="nextstepaction"]
>[FHıR için Azure API 'de ek ayarlar](azure-api-for-fhir-additional-settings.md)
