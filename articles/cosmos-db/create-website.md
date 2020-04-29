---
title: Şablonla bir Web uygulaması dağıtma-Azure Cosmos DB
description: Bir Azure Cosmos DB hesabını, Azure App Service Web Apps ve örnek bir Web uygulamasını Azure Resource Manager şablonu kullanarak dağıtmayı öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: sngun
ms.openlocfilehash: 2306dbe234e171ac613c33458df1990b767637df
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79128380"
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak Web Apps Azure Cosmos DB ve Azure App Service dağıtma
Bu öğreticide, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) Web uygulamasını ve örnek bir Web uygulamasını dağıtmak ve bütünleştirmek için bir Azure Resource Manager şablonunun nasıl kullanılacağı gösterilmektedir.

Azure Resource Manager şablonları kullanarak, Azure kaynaklarınızın dağıtımını ve yapılandırmasını kolayca otomatikleştirebilmeniz gerekir.  Bu öğretici, bir Web uygulamasının nasıl dağıtılacağını ve Azure Cosmos DB hesabı bağlantı bilgilerinin otomatik olarak nasıl yapılandırılacağını gösterir.

Bu Öğreticiyi tamamladıktan sonra aşağıdaki soruları cevaplayabilirsiniz:  

* Azure App Service ' de bir Azure Cosmos DB hesabı ve bir Web uygulaması dağıtmak ve bütünleştirmek için bir Azure Resource Manager şablonu nasıl kullanabilirim?
* Bir Azure Cosmos DB hesabını, App Service Web Apps bir Web uygulamasını ve WebDeploy uygulamasını dağıtmak ve bütünleştirmek için bir Azure Resource Manager şablonu nasıl kullanabilirim?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Ön koşullar
> [!TIP]
> Bu öğretici Azure Resource Manager şablonları veya JSON ile önceki deneyimle karşılaşmadığından, başvurulan şablonları veya dağıtım seçeneklerini değiştirmek ister, bu alanların her biri için bilgi gereklidir.
> 
> 

Bu öğreticideki yönergeleri takip etmeden önce, Azure aboneliğine sahip olduğunuzdan emin olun. Azure, abonelik tabanlı bir platformdur.  Abonelik edinme hakkında daha fazla bilgi için bkz. [satın alma seçenekleri](https://azure.microsoft.com/pricing/purchase-options/), [üye teklifleri](https://azure.microsoft.com/pricing/member-offers/)veya [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-download-the-template-files"></a><a id="CreateDB"></a>1. Adım: şablon dosyalarını Indirme
Bu öğreticinin gerektirdiği şablon dosyalarını indirerek başlayalım.

1. [Azure Cosmos DB hesabı oluştur, Web Apps ve bir demo uygulaması örnek](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) şablonunu yerel bir klasöre (örneğin, C:\azure Cosmos dbtemplates) dağıtın. Bu şablon bir Azure Cosmos DB hesabı, bir App Service Web uygulaması ve bir Web uygulaması dağıtır.  Ayrıca, Web uygulamasını Azure Cosmos DB hesabına bağlanacak şekilde otomatik olarak yapılandırır.
2. [Azure Cosmos DB hesabı oluştur ve örnek şablonu Web Apps](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) bir yerel klasöre indirin (örneğin, C:\azure Cosmos dbtemplates). Bu şablon bir Azure Cosmos DB hesabını, bir App Service Web uygulamasını dağıtır ve site uygulama ayarlarını Azure Cosmos DB bağlantı bilgilerini kolayca yüzey olarak değiştirir, ancak bir Web uygulaması içermez.  

<a id="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>2. Adım: Azure Cosmos DB hesabı, App Service Web uygulaması ve tanıtım uygulaması örneğini dağıtma
Şimdi ilk şablonunuzu dağıtalım.

> [!TIP]
> Şablon, aşağıdaki şablonda girilen Web uygulaması adı ve Azure Cosmos DB hesap adının geçerli ve b) kullanılabilir olduğunu doğrulamaz.  Dağıtımı göndermeden önce sağlamayı planladığınız adların kullanılabilirliğini doğrulamanız kesinlikle önerilir.
> 
> 

1. [Azure portalında](https://portal.azure.com)oturum açın, yeni ' ye tıklayın ve "şablon dağıtımı" araması yapın.
    ![Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü](./media/create-website/TemplateDeployment1.png)
2. Şablon dağıtımı öğesini seçin ve şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü **Oluştur** ![' a tıklayın.](./media/create-website/TemplateDeployment2.png)
3. **Şablonu Düzenle**' ye tıklayın, DocDBWebsiteTodo. JSON şablon dosyasının içeriğini yapıştırın ve **Kaydet**' e tıklayın.
   ![Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü](./media/create-website/TemplateDeployment3.png)
4. **Parametreleri Düzenle**' ye tıklayın, zorunlu parametrelerin her biri için değerler sağlayın ve **Tamam**' a tıklayın.  Parametreler şu şekildedir:
   
   1. SITENAME: App Service Web uygulaması adını belirtir ve Web uygulamasına erişmek için kullandığınız URL 'YI oluşturmak için kullanılır (örneğin, "mydemodocdbwebapp" belirtirseniz, Web uygulamasına erişebileceğiniz URL `mydemodocdbwebapp.azurewebsites.net`).
   2. HOSTINGPLANNAME: oluşturulacak App Service barındırma planının adını belirtir.
   3. Konum: Azure Cosmos DB ve Web uygulaması kaynaklarının oluşturulacağı Azure konumunu belirtir.
   4. DATABASEACCOUNTNAME: oluşturulacak Azure Cosmos DB hesabının adını belirtir.   
      
      ![Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü](./media/create-website/TemplateDeployment4.png)
5. Mevcut bir kaynak grubu seçin veya yeni bir kaynak grubu oluşturmak için bir ad girin ve kaynak grubu için bir konum seçin.

    ![Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü](./media/create-website/TemplateDeployment5.png)
6. Dağıtımı başlatmak için **yasal koşulları gözden geçir**, **satın al**' a ve ardından **Oluştur** ' a tıklayın.  Ortaya çıkan dağıtımın Azure portal giriş sayfanızda kolayca görünmesini sağlamak **için panoya sabitle ' yi** seçin.
   ![Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü](./media/create-website/TemplateDeployment6.png)
7. Dağıtım tamamlandığında, kaynak grubu bölmesi açılır.
   ![Kaynak grubu bölmesinin ekran görüntüsü](./media/create-website/TemplateDeployment7.png)  
8. Uygulamayı kullanmak için Web uygulaması URL 'sine gidin (Yukarıdaki örnekte URL 'nin olması `http://mydemodocdbwebapp.azurewebsites.net`gerekir).  Aşağıdaki Web uygulamasını görürsünüz:
   
   ![Örnek ToDo uygulaması](./media/create-website/image2.png)
9. Devam edin ve Web uygulamasında birkaç görev oluşturun ve ardından Azure portal kaynak grubu bölmesine geri dönün. Kaynaklar listesinde Azure Cosmos DB hesap kaynağına tıklayın ve **Veri Gezgini**' a tıklayın.
10. "SELECT * FROM c" varsayılan sorgusunu çalıştırın ve sonuçları inceleyin.  Sorgunun, yukarıdaki 7. adımda oluşturduğunuz Todo öğelerinin JSON temsilini alındığını unutmayın.  Sorguları denemekten çekinmeyin; Örneğin, tamamlanmış olarak işaretlenen tüm Todo öğelerini döndürmek için SELECT * FROM c, c. IsAll = true çalıştırmayı deneyin.
11. Azure Cosmos DB portalı deneyimini keşfetmekten veya örnek Todo uygulamasını değiştirebilirsin.  Hazırsanız, başka bir şablon dağıtalım.

<a id="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>3. Adım: belge hesabını ve Web uygulaması örneğini dağıtma
Şimdi ikinci şablonunuzu dağıtalım.  Bu şablon, hesap uç noktası ve ana anahtar gibi Azure Cosmos DB bağlantı bilgilerini bir Web uygulamasına uygulama ayarları olarak veya özel bir bağlantı dizesi olarak nasıl ekleyebileceğini göstermek için yararlıdır. Örneğin, bir Azure Cosmos DB hesabıyla dağıtmak istediğiniz kendi Web uygulamanız vardır ve dağıtım sırasında bağlantı bilgilerinin otomatik olarak doldurulmasını sağlayabilirsiniz.

> [!TIP]
> Şablon, aşağıda girilen Web uygulaması adının ve Azure Cosmos DB hesap adının geçerli ve b) kullanılabilir olduğunu doğrulamaz.  Dağıtımı göndermeden önce sağlamayı planladığınız adların kullanılabilirliğini doğrulamanız kesinlikle önerilir.
> 
> 

1. [Azure portalında](https://portal.azure.com)yeni ' ye tıklayın ve "şablon dağıtımı" araması yapın.
    ![Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü](./media/create-website/TemplateDeployment1.png)
2. Şablon dağıtımı öğesini seçin ve şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü **Oluştur** ![' a tıklayın.](./media/create-website/TemplateDeployment2.png)
3. **Şablonu Düzenle**' ye tıklayın, DocDBWebSite. JSON şablon dosyasının içeriğini yapıştırın ve **Kaydet**' e tıklayın.
   ![Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü](./media/create-website/TemplateDeployment3.png)
4. **Parametreleri Düzenle**' ye tıklayın, zorunlu parametrelerin her biri için değerler sağlayın ve **Tamam**' a tıklayın.  Parametreler şu şekildedir:
   
   1. SITENAME: App Service Web uygulaması adını belirtir ve Web uygulamasına erişmek için kullanacağınız URL 'YI oluşturmak için kullanılır (örneğin, "mydemodocdbwebapp" belirtirseniz, Web uygulamasına erişiminizin URL 'SI mydemodocdbwebapp.azurewebsites.net).
   2. HOSTINGPLANNAME: oluşturulacak App Service barındırma planının adını belirtir.
   3. Konum: Azure Cosmos DB ve Web uygulaması kaynaklarının oluşturulacağı Azure konumunu belirtir.
   4. DATABASEACCOUNTNAME: oluşturulacak Azure Cosmos DB hesabının adını belirtir.   
      
      ![Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü](./media/create-website/TemplateDeployment4.png)
5. Mevcut bir kaynak grubu seçin veya yeni bir kaynak grubu oluşturmak için bir ad girin ve kaynak grubu için bir konum seçin.

    ![Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü](./media/create-website/TemplateDeployment5.png)
6. Dağıtımı başlatmak için **yasal koşulları gözden geçir**, **satın al**' a ve ardından **Oluştur** ' a tıklayın.  Ortaya çıkan dağıtımın Azure portal giriş sayfanızda kolayca görünmesini sağlamak **için panoya sabitle ' yi** seçin.
   ![Şablon dağıtımı Kullanıcı arabiriminin ekran görüntüsü](./media/create-website/TemplateDeployment6.png)
7. Dağıtım tamamlandığında, kaynak grubu bölmesi açılır.
   ![Kaynak grubu bölmesinin ekran görüntüsü](./media/create-website/TemplateDeployment7.png)  
8. Kaynaklar listesinde Web uygulaması kaynağına tıklayın ve ardından kaynak grubunun **uygulama ayarları** ![ekran görüntüsü ' ne tıklayın.](./media/create-website/TemplateDeployment9.png)  
9. Azure Cosmos DB uç noktası ve Azure Cosmos DB ana anahtarlarının her biri için nasıl uygulama ayarlarının mevcut olduğunu aklınızda edin.

    ![Uygulama ayarlarının ekran görüntüsü](./media/create-website/TemplateDeployment10.png)  
10. Azure portalını araştırmaya devam edebilir veya kendi Azure Cosmos DB uygulamanızı oluşturmak için Azure Cosmos DB [örneklerimizden](https://go.microsoft.com/fwlink/?LinkID=402386) birini takip edebilirsiniz.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Azure Resource Manager şablonları kullanarak Azure Cosmos DB, App Service Web uygulaması ve örnek bir Web uygulaması dağıttınız.

* Azure Cosmos DB hakkında daha fazla bilgi edinmek için [buraya](https://azure.microsoft.com/services/cosmos-db/)tıklayın.
* Azure App Service Web Apps hakkında daha fazla bilgi edinmek için [buraya](https://go.microsoft.com/fwlink/?LinkId=325362)tıklayın.
* Azure Resource Manager şablonları hakkında daha fazla bilgi edinmek için [buraya](https://msdn.microsoft.com/library/azure/dn790549.aspx)tıklayın.

## <a name="whats-changed"></a>Yapılan değişiklikler
* Web Sitelerinden App Service’e kadar değiştirme kılavuzu için bkz. [Azure App Service ve Mevcut Azure Hizmetlerine Etkileri](https://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’de hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://go.microsoft.com/fwlink/?LinkId=523751) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

