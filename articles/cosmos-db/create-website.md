---
title: Şablonlu bir web uygulaması dağıtma - Azure Cosmos DB
description: Azure Kaynak Yöneticisi şablonu kullanarak bir Azure Cosmos DB hesabını, Azure Uygulama Hizmeti Web Uygulamalarını ve örnek bir web uygulamasını nasıl dağıtılacak öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: sngun
ms.openlocfilehash: 2306dbe234e171ac613c33458df1990b767637df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128380"
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi Şablonu kullanarak Azure Cosmos DB ve Azure Uygulama Hizmeti Web Uygulamalarını dağıtma
Bu öğretici, [Microsoft Azure Cosmos DB,](https://azure.microsoft.com/services/cosmos-db/) [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) web uygulamasını ve örnek bir web uygulamasını dağıtmak ve tümleştirmek için Azure Kaynak Yöneticisi şablonu nasıl kullanacağınızı gösterir.

Azure Kaynak Yöneticisi şablonlarını kullanarak Azure kaynaklarınızın dağıtımını ve yapılandırmasını kolayca otomatikleştirebilirsiniz.  Bu öğretici, bir web uygulamasının nasıl dağıtılanacağımı ve Azure Cosmos DB hesap bağlantı bilgilerini otomatik olarak nasıl yapılandıracağımı gösterir.

Bu öğreticiyi tamamladıktan sonra aşağıdaki soruları yanıtlayabilirsiniz:  

* Azure Cosmos DB hesabını ve bir web uygulamasını Azure Uygulama Hizmeti'nde dağıtmak ve tümleştirmek için Azure Kaynak Yöneticisi şablonu nasıl kullanabilirim?
* Bir Azure Cosmos DB hesabını, App Service Web Apps'taki bir web uygulamasını ve Web dağıtımı uygulamasını dağıtmak ve tümleştirmek için Azure Kaynak Yöneticisi şablonu nasıl kullanabilirim?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Ön koşullar
> [!TIP]
> Bu öğretici, Azure Kaynak Yöneticisi şablonları veya JSON ile önceden deneyim üstlenmese de, başvurulan şablonları veya dağıtım seçeneklerini değiştirmek isterseniz, bu alanların her biri hakkında bilgi sahibi olmak gerekir.
> 
> 

Bu öğreticideki yönergeleri takip etmeden önce, bir Azure aboneliğine sahip olduğundan emin olun. Azure, abonelik tabanlı bir platformdur.  Abonelik alma hakkında daha fazla bilgi için [Satın Alma Seçenekleri,](https://azure.microsoft.com/pricing/purchase-options/) [Üye Teklifleri](https://azure.microsoft.com/pricing/member-offers/)veya [Ücretsiz Deneme'ye](https://azure.microsoft.com/pricing/free-trial/)bakın.

## <a name="step-1-download-the-template-files"></a><a id="CreateDB"></a>Adım 1: Şablon dosyalarını indirin
Bu öğreticinin gerektirdiği şablon dosyalarını indirerek başlayalım.

1. Azure [Cosmos DB hesabı, Web Uygulamaları Oluştur'u](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) indirin ve demo uygulama örnek şablonlarını yerel bir klasöre (örneğin, C:\Azure Cosmos DBTemplates) dağıtın. Bu şablon, bir Azure Cosmos DB hesabı, bir App Service web uygulaması ve bir web uygulaması dağıtır.  Ayrıca, Azure Cosmos DB hesabına bağlanmak için web uygulamasını otomatik olarak yapılandırır.
2. Azure [Cosmos DB hesabı ve Web Uygulamaları örnek](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) şablonu oluşturma şablonundan yerel bir klasöre (örneğin, C:\Azure Cosmos DBTemplates) indirin. Bu şablon, bir Azure Cosmos DB hesabı, bir App Service web uygulaması dağıtır ve sitenin uygulama ayarlarını Azure Cosmos DB bağlantı bilgilerini kolayca yüzeye çıkarmak için değiştirir, ancak bir web uygulaması içermez.  

<a id="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>Adım 2: Azure Cosmos DB hesabını, App Service web uygulamasını ve demo uygulama örneğini dağıtma
Şimdi ilk şablonunuzu dağıtalım.

> [!TIP]
> Şablon, aşağıdaki şablona girilen web uygulama adı ve Azure Cosmos DB hesap adının a) geçerli ve b) kullanılabilir olduğunu doğrulamaz.  Dağıtımı göndermeden önce sağlamayı planladığınız adların kullanılabilirliğini doğrulamanız önerilir.
> 
> 

1. [Azure Portalı'na](https://portal.azure.com)giriş yapın, Yeni'yi tıklatın ve "Şablon dağıtımı"nı arayın.
    ![Şablon dağıtım ui ekran görüntüsü](./media/create-website/TemplateDeployment1.png)
2. Şablon dağıtım öğesini seçin ve şablon dağıtım UI'sinin Ekran Görüntüsünü **Oluştur'u** ![tıklatın](./media/create-website/TemplateDeployment2.png)
3. **Şablonu Edit'i**tıklatın, DocDBWebsiteTodo.json şablon dosyasının içeriğini yapıştırın ve **Kaydet'i**tıklatın.
   ![Şablon dağıtım ui ekran görüntüsü](./media/create-website/TemplateDeployment3.png)
4. **Parametreleri İyile'yi**tıklatın, zorunlu parametrelerin her biri için değerler sağlayın ve **Tamam'ı**tıklatın.  Parametreler şu şekildedir:
   
   1. SİTE ADI: App Service web uygulama adını belirtir ve web uygulamasına erişmek için kullandığınız URL'yi oluşturmak için kullanılır (örneğin, "mydemodocdbwebapp" `mydemodocdbwebapp.azurewebsites.net`belirtirseniz, web uygulamasına erişebildiğiniz URL'dir).
   2. HOSTINGPLANNAME: Oluşturmak için App Service hosting planıadını belirtir.
   3. KONUM: Azure Cosmos DB ve web uygulaması kaynaklarını oluşturmak için Azure konumunu belirtir.
   4. DATABASEACCOUNTNAME: Oluşturmak için Azure Cosmos DB hesabının adını belirtir.   
      
      ![Şablon dağıtım ui ekran görüntüsü](./media/create-website/TemplateDeployment4.png)
5. Varolan bir Kaynak grubu seçin veya yeni bir kaynak grubu yapmak için bir ad sağlayın ve kaynak grubu için bir konum seçin.

    ![Şablon dağıtım ui ekran görüntüsü](./media/create-website/TemplateDeployment5.png)
6. **Yasal Terimleri Gözden Geçir'i**, Satın **Al'ı**ve dağıtımı başlatmak için **Oluştur'u** tıklatın.  Ortaya çıkan dağıtımın Azure portalı giriş sayfanızda kolayca görülebilmesi için **panoya** Sabitle'yi seçin.
   ![Şablon dağıtım ui ekran görüntüsü](./media/create-website/TemplateDeployment6.png)
7. Dağıtım bittiğinde, Kaynak grubu bölmesi açılır.
   ![Kaynak grubu bölmesinin ekran görüntüsü](./media/create-website/TemplateDeployment7.png)  
8. Uygulamayı kullanmak için web uygulaması URL'sine gidin (yukarıdaki örnekte `http://mydemodocdbwebapp.azurewebsites.net`URL olacaktır).  Aşağıdaki web uygulamasını görürsünüz:
   
   ![Örnek Todo uygulaması](./media/create-website/image2.png)
9. Devam edin ve web uygulamasında birkaç görev oluşturun ve ardından Azure portalındaki Kaynak grup bölmesine geri dönün. Kaynaklar listesindeki Azure Cosmos DB hesap kaynağını tıklatın ve ardından **Veri Gezgini'ni**tıklatın.
10. Varsayılan sorguyu çalıştırın, "SELECT * FROM c" ve sonuçları inceleyin.  Sorgunun, yukarıdaki adım 7'de oluşturduğunuz todo öğelerinin JSON temsilini aldığını unutmayın.  Sorguları deneme çekinmeyin; örneğin, SELECT * FROM c WHERE c.isComplete = tam olarak işaretlenmiş tüm todo öğelerini döndürmek için doğru çalıştırmayı deneyin.
11. Azure Cosmos DB portal deneyimini keşfetmekten veya örnek Todo uygulamasını değiştirmekten çekinmeyin.  Hazır olduğunuzda, başka bir şablon dağıtalım.

<a id="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>Adım 3: Belge hesabını ve web uygulaması örneğini dağıtma
Şimdi ikinci şablonunuzu dağıtalım.  Bu şablon, hesap bitiş noktası ve ana anahtar gibi Azure Cosmos DB bağlantı bilgilerini uygulama ayarları olarak veya özel bağlantı dizesi olarak bir web uygulamasına nasıl enjekte edebileceğinizi göstermek için yararlıdır. Örneğin, belki de bir Azure Cosmos DB hesabıyla dağıtmak istediğiniz kendi web uygulamanız vardır ve dağıtım sırasında bağlantı bilgilerinin otomatik olarak doldurulmasını sağlar.

> [!TIP]
> Şablon, aşağıda girilen web uygulaması adı ve Azure Cosmos DB hesap adının a) geçerli ve b) kullanılabilir olduğunu doğrulamaz.  Dağıtımı göndermeden önce sağlamayı planladığınız adların kullanılabilirliğini doğrulamanız önerilir.
> 
> 

1. Azure [Portalı'nda](https://portal.azure.com)Yeni'yi tıklatın ve "Şablon dağıtımı"nı arayın.
    ![Şablon dağıtım ui ekran görüntüsü](./media/create-website/TemplateDeployment1.png)
2. Şablon dağıtım öğesini seçin ve şablon dağıtım UI'sinin Ekran Görüntüsünü **Oluştur'u** ![tıklatın](./media/create-website/TemplateDeployment2.png)
3. **Şablonu Edit'i**tıklatın, DocDBWebSite.json şablon dosyasının içeriğini yapıştırın ve **Kaydet'i**tıklatın.
   ![Şablon dağıtım ui ekran görüntüsü](./media/create-website/TemplateDeployment3.png)
4. **Parametreleri İyile'yi**tıklatın, zorunlu parametrelerin her biri için değerler sağlayın ve **Tamam'ı**tıklatın.  Parametreler şu şekildedir:
   
   1. SİTE ADI: App Service web uygulama adını belirtir ve web uygulamasına erişmek için kullanacağınız URL'yi oluşturmak için kullanılır (örneğin, "mydemodocdbwebapp" belirtirseniz, web uygulamasına erişirdiğiniz URL mydemodocdbwebapp.azurewebsites.net).
   2. HOSTINGPLANNAME: Oluşturmak için App Service hosting planıadını belirtir.
   3. KONUM: Azure Cosmos DB ve web uygulaması kaynaklarını oluşturmak için Azure konumunu belirtir.
   4. DATABASEACCOUNTNAME: Oluşturmak için Azure Cosmos DB hesabının adını belirtir.   
      
      ![Şablon dağıtım ui ekran görüntüsü](./media/create-website/TemplateDeployment4.png)
5. Varolan bir Kaynak grubu seçin veya yeni bir kaynak grubu yapmak için bir ad sağlayın ve kaynak grubu için bir konum seçin.

    ![Şablon dağıtım ui ekran görüntüsü](./media/create-website/TemplateDeployment5.png)
6. **Yasal Terimleri Gözden Geçir'i**, Satın **Al'ı**ve dağıtımı başlatmak için **Oluştur'u** tıklatın.  Ortaya çıkan dağıtımın Azure portalı giriş sayfanızda kolayca görülebilmesi için **panoya** Sabitle'yi seçin.
   ![Şablon dağıtım ui ekran görüntüsü](./media/create-website/TemplateDeployment6.png)
7. Dağıtım bittiğinde, Kaynak grubu bölmesi açılır.
   ![Kaynak grubu bölmesinin ekran görüntüsü](./media/create-website/TemplateDeployment7.png)  
8. Kaynaklar listesindeki Web Uygulaması kaynağını tıklatın ve ardından kaynak grubunun **Uygulama ayarları** ![Ekran Görüntüsü'ne tıklayın](./media/create-website/TemplateDeployment9.png)  
9. Azure Cosmos DB bitiş noktası ve Azure Cosmos DB ana anahtarlarının her biri için uygulama ayarlarının nasıl mevcut olduğunu unutmayın.

    ![Uygulama ayarlarının ekran görüntüsü](./media/create-website/TemplateDeployment10.png)  
10. Azure Portalını keşfetmeye devam edin veya kendi Azure Cosmos DB uygulamanızı oluşturmak için Azure Cosmos DB [örneklerimizden](https://go.microsoft.com/fwlink/?LinkID=402386) birini takip edin.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Cosmos DB, App Service web uygulaması ve örnek bir web uygulaması dağıttınız.

* Azure Cosmos DB hakkında daha fazla bilgi edinmek için [buraya](https://azure.microsoft.com/services/cosmos-db/)tıklayın.
* Azure App Service Web uygulamaları hakkında daha fazla bilgi edinmek için [buraya](https://go.microsoft.com/fwlink/?LinkId=325362)tıklayın.
* Azure Kaynak Yöneticisi şablonları hakkında daha fazla bilgi edinmek için [burayı](https://msdn.microsoft.com/library/azure/dn790549.aspx)tıklatın.

## <a name="whats-changed"></a>Yapılan değişiklikler
* Web Sitelerinden App Service’e kadar değiştirme kılavuzu için bkz. [Azure App Service ve Mevcut Azure Hizmetlerine Etkileri](https://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’de hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://go.microsoft.com/fwlink/?LinkId=523751) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

