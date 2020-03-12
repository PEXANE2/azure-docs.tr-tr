---
title: Bir şablon - Azure Cosmos DB ile bir web uygulaması dağıtma
description: Azure Cosmos DB hesabı, Azure App Service Web Apps ve Azure Resource Manager şablonu kullanarak örnek bir web uygulamasına dağıtmayı öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: sngun
ms.openlocfilehash: 2306dbe234e171ac613c33458df1990b767637df
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128380"
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Azure Cosmos DB ile bir Azure Resource Manager şablonu kullanarak Azure App Service Web Apps'e dağıtın
Bu öğreticide, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) Web uygulamasını ve örnek bir Web uygulamasını dağıtmak ve bütünleştirmek için bir Azure Resource Manager şablonunun nasıl kullanılacağı gösterilmektedir.

Azure Resource Manager şablonlarını kullanarak, dağıtımını ve yapılandırmasını, Azure kaynaklarınızın kolayca otomatikleştirebilirsiniz.  Bu öğreticide, bir web uygulamasını dağıtma ve otomatik olarak Azure Cosmos DB hesabı bağlantı bilgilerini yapılandırma gösterilmektedir.

Bu öğreticiyi tamamladıktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır:  

* Bir Azure Resource Manager şablonu dağıtma ve Azure Cosmos DB hesabı ve Azure App Service'te bir web uygulaması tümleştirme için nasıl kullanabilirim?
* Bir Azure Resource Manager şablonu dağıtma ve Azure Cosmos DB hesabı, bir web uygulamasını App Service Web Apps ve Webdeploy uygulama tümleştirme için nasıl kullanabilirim?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar
> [!TIP]
> Bu öğreticide, Azure Resource Manager şablonları ya da JSON deneyiminiz olmasa varsaymaz olsa da başvurulan şablonları veya dağıtım seçenekleri değiştirmek istemeniz durumunda sonra bu alanların her birini bilgisi gereklidir.
> 
> 

Bu öğreticide yönergeleri izlemeden önce sahip olduğunuzdan emin olun Azure aboneliği. Azure abonelik tabanlı bir platformdur.  Abonelik edinme hakkında daha fazla bilgi için bkz. [satın alma seçenekleri](https://azure.microsoft.com/pricing/purchase-options/), [üye teklifleri](https://azure.microsoft.com/pricing/member-offers/)veya [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>1. Adım: şablon dosyalarını Indirme
Bu öğretici şablon dosyaları yükleyerek başlayalım.

1. [Azure Cosmos DB hesabı oluştur, Web Apps ve bir demo uygulaması örnek](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) şablonunu yerel bir klasöre (örneğin, C:\azure Cosmos dbtemplates) dağıtın. Bu şablon, bir Azure Cosmos DB hesabı, bir App Service web uygulaması ve bir web uygulaması dağıtır.  Azure Cosmos DB hesabına bağlanmak için web uygulaması da otomatik olarak yapılandırır.
2. [Azure Cosmos DB hesabı oluştur ve örnek şablonu Web Apps](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) bir yerel klasöre indirin (örneğin, C:\azure Cosmos dbtemplates). Bu şablon, bir Azure Cosmos DB hesabı, bir App Service web uygulaması dağıtır ve sitenin uygulama ayarlarını kolayca yüzey Azure Cosmos DB bağlantı bilgilerini değiştirir ancak bir web uygulaması içermez.  

<a id="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>2\. adım: Azure Cosmos DB hesabı, App Service web uygulaması ve tanıtım uygulaması örneği dağıtma
Artık ilk şablonunuzu dağıtalım.

> [!TIP]
> Şablon aşağıdaki şablonda girdiğiniz Azure Cosmos DB hesabı adını ve web uygulaması adı geçerli bir) ve (b) kullanılabilir olduğunu doğrulamaz.  Dağıtım gönderiliyor önce sağlamak için plan adları kullanılabilirliğini doğrulamak önerilir.
> 
> 

1. [Azure portalında](https://portal.azure.com)oturum açın, yeni ' ye tıklayın ve "şablon dağıtımı" araması yapın.
    Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsünü ![](./media/create-website/TemplateDeployment1.png)
2. Şablon dağıtımı öğesini seçin ve şablon dağıtımı Kullanıcı arabirimindeki ![ekran görüntüsü **Oluştur** ' a tıklayın](./media/create-website/TemplateDeployment2.png)
3. **Şablonu Düzenle**' ye tıklayın, DocDBWebsiteTodo. JSON şablon dosyasının içeriğini yapıştırın ve **Kaydet**' e tıklayın.
   Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsünü ![](./media/create-website/TemplateDeployment3.png)
4. **Parametreleri Düzenle**' ye tıklayın, zorunlu parametrelerin her biri için değerler sağlayın ve **Tamam**' a tıklayın.  Parametreleri aşağıdaki gibidir:
   
   1. SITENAME: App Service Web uygulaması adını belirtir ve Web uygulamasına erişmek için kullandığınız URL 'YI oluşturmak için kullanılır (örneğin, "mydemodocdbwebapp" belirtirseniz, Web uygulamasına erişiminizin URL 'SI `mydemodocdbwebapp.azurewebsites.net`).
   2. HOSTINGPLANNAME: App Service barındırma planı oluşturmak için adını belirtir.
   3. KONUM: Azure Cosmos DB ile web uygulaması kaynakları oluşturmak Azure konumu belirtir.
   4. DATABASEACCOUNTNAME: Azure Cosmos DB hesabı oluşturmak için adını belirtir.   
      
      ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment4.png)
5. Mevcut bir kaynak grubu seçin veya yeni bir kaynak grubu için bir ad belirtin ve kaynak grubu için bir konum seçin.

    ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment5.png)
6. Dağıtımı başlatmak için **yasal koşulları gözden geçir**, **satın al**' a ve ardından **Oluştur** ' a tıklayın.  Ortaya çıkan dağıtımın Azure portal giriş sayfanızda kolayca görünmesini sağlamak **için panoya sabitle ' yi** seçin.
   Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsünü ![](./media/create-website/TemplateDeployment6.png)
7. Dağıtım tamamlandığında kaynak grubu bölmesi açılır.
   kaynak grubu bölmesinin ekran görüntüsünü ![](./media/create-website/TemplateDeployment7.png)  
8. Uygulamayı kullanmak için Web uygulaması URL 'sine gidin (Yukarıdaki örnekte URL `http://mydemodocdbwebapp.azurewebsites.net`).  Aşağıdaki web uygulaması görürsünüz:
   
   ![Örnek TODO uygulaması](./media/create-website/image2.png)
9. Devam edin ve birkaç görev web uygulaması oluşturun ve ardından Azure portalında kaynak grubu bölmesine dönün. Kaynaklar listesinde Azure Cosmos DB hesap kaynağına tıklayın ve **Veri Gezgini**' a tıklayın.
10. Varsayılan sorguyu çalıştırma "seçin * c" ve sonuçları inceleyin.  Sorgu, yukarıdaki 7. adımda oluşturduğunuz todo öğelerini JSON temsili almıştır dikkat edin.  Deneme sorgularıyla çekinmeyin; Örneğin, SELECT çalıştıran deneyin * c WHERE c.isComplete = tamamlandı olarak işaretlenmiş tüm todo öğeleri döndürmek için true.
11. Azure Cosmos DB portal deneyimi keşfedin veya örnek Todo uygulaması değiştirmek çekinmeyin.  Hazır olduğunuzda, başka bir şablon dağıtalım.

<a id="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>3\. adım: belge hesabı ve web uygulaması örneği dağıtma
Şimdi ikinci şablonunuzu dağıtalım.  Bu şablonu nasıl, uygulama ayarları veya özel bir bağlantı dizesi olarak web uygulamasıyla Azure Cosmos DB hesabınızın uç noktası ve ana anahtarı gibi bağlantı bilgilerini ekleyebilir göstermek kullanışlıdır. Örneğin, bir Azure Cosmos DB hesabı dağıtabiliyorum ve dağıtım sırasında otomatik olarak doldurulur bağlantı bilgilerini istediğiniz kendi web Uygulamam var belki de.

> [!TIP]
> Şablon, web uygulaması adı ve Azure Cosmos DB hesap adınızı aşağıda girilen bir) geçerli ve (b) kullanılabilir olduğunu doğrulamaz.  Dağıtım gönderiliyor önce sağlamak için plan adları kullanılabilirliğini doğrulamak önerilir.
> 
> 

1. [Azure portalında](https://portal.azure.com)yeni ' ye tıklayın ve "şablon dağıtımı" araması yapın.
    Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsünü ![](./media/create-website/TemplateDeployment1.png)
2. Şablon dağıtımı öğesini seçin ve şablon dağıtımı Kullanıcı arabirimindeki ![ekran görüntüsü **Oluştur** ' a tıklayın](./media/create-website/TemplateDeployment2.png)
3. **Şablonu Düzenle**' ye tıklayın, DocDBWebSite. JSON şablon dosyasının içeriğini yapıştırın ve **Kaydet**' e tıklayın.
   Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsünü ![](./media/create-website/TemplateDeployment3.png)
4. **Parametreleri Düzenle**' ye tıklayın, zorunlu parametrelerin her biri için değerler sağlayın ve **Tamam**' a tıklayın.  Parametreleri aşağıdaki gibidir:
   
   1. SITENAME: App Service web uygulaması adını belirtir ve (örneğin, "mydemodocdbwebapp", ardından web uygulaması tarafından erişim URL'si olan mydemodocdbwebapp.azurewebsites.net belirtirseniz), web uygulamasına erişmek için kullanacağı URL'yi oluşturmak için kullanılır.
   2. HOSTINGPLANNAME: App Service barındırma planı oluşturmak için adını belirtir.
   3. KONUM: Azure Cosmos DB ile web uygulaması kaynakları oluşturmak Azure konumu belirtir.
   4. DATABASEACCOUNTNAME: Azure Cosmos DB hesabı oluşturmak için adını belirtir.   
      
      ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment4.png)
5. Mevcut bir kaynak grubu seçin veya yeni bir kaynak grubu için bir ad belirtin ve kaynak grubu için bir konum seçin.

    ![Şablon dağıtımı kullanıcı Arabirimi ekran görüntüsü](./media/create-website/TemplateDeployment5.png)
6. Dağıtımı başlatmak için **yasal koşulları gözden geçir**, **satın al**' a ve ardından **Oluştur** ' a tıklayın.  Ortaya çıkan dağıtımın Azure portal giriş sayfanızda kolayca görünmesini sağlamak **için panoya sabitle ' yi** seçin.
   Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsünü ![](./media/create-website/TemplateDeployment6.png)
7. Dağıtım tamamlandığında kaynak grubu bölmesi açılır.
   kaynak grubu bölmesinin ekran görüntüsünü ![](./media/create-website/TemplateDeployment7.png)  
8. Kaynaklar listesinde Web uygulaması kaynağına tıklayın ve ardından kaynak grubunun ekran görüntüsünü ![**uygulama ayarları** ' na tıklayın](./media/create-website/TemplateDeployment9.png)  
9. Nasıl uygulama ayarları, Azure Cosmos DB uç noktası ve her bir Azure Cosmos DB ana anahtarı için mevcut dikkat edin.

    ![Uygulama ayarları görüntüsü](./media/create-website/TemplateDeployment10.png)  
10. Azure portalını araştırmaya devam edebilir veya kendi Azure Cosmos DB uygulamanızı oluşturmak için Azure Cosmos DB [örneklerimizden](https://go.microsoft.com/fwlink/?LinkID=402386) birini takip edebilirsiniz.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Azure Cosmos DB, App Service web uygulaması ve Azure Resource Manager şablonlarını kullanarak örnek bir web uygulamasına dağıttınız.

* Azure Cosmos DB hakkında daha fazla bilgi edinmek için [buraya](https://azure.microsoft.com/services/cosmos-db/)tıklayın.
* Azure App Service Web Apps hakkında daha fazla bilgi edinmek için [buraya](https://go.microsoft.com/fwlink/?LinkId=325362)tıklayın.
* Azure Resource Manager şablonları hakkında daha fazla bilgi edinmek için [buraya](https://msdn.microsoft.com/library/azure/dn790549.aspx)tıklayın.

## <a name="whats-changed"></a>Yapılan değişiklikler
* Web Sitelerinden App Service’e kadar değiştirme kılavuzu için bkz. [Azure App Service ve Mevcut Azure Hizmetlerine Etkileri](https://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://go.microsoft.com/fwlink/?LinkId=523751) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

