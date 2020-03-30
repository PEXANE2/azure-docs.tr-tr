---
title: ASE v1'de bir web uygulaması oluşturma
description: Bir Uygulama Hizmet Ortamı v1'de web uygulamaları oluşturmayı öğrenin. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: ccompy
ms.assetid: 983ba055-e9e4-495a-9342-fd3708dcc9ac
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 5c947617f0c27708e72f9bff92e2b0041473cd92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266201"
---
# <a name="create-a-web-app-in-an-app-service-environment-v1"></a>Bir Uygulama Hizmet Ortamında web uygulaması oluşturma v1

> [!NOTE]
> Bu makale, App Service Environment v1 hakkındadır.  Uygulama Hizmet Ortamı'nın kullanımı daha kolay olan ve daha güçlü altyapıda çalışan daha yeni bir sürümü vardır. Yeni sürüm hakkında daha fazla bilgi edinmek [için Uygulama Hizmet Ortamına Giriş](intro.md)ile başlayın.
> 

## <a name="overview"></a>Genel Bakış
Bu öğretici, bir [Uygulama Hizmet Ortamı v1](app-service-app-service-environment-intro.md) 'de (ASE) web uygulamaları nın ve Uygulama Hizmeti planlarının nasıl oluşturulacağımı gösterir. 

> [!NOTE]
> Bir web uygulaması oluşturmayı öğrenmek istiyorsanız ancak bunu Bir Uygulama Hizmet Ortamında yapmanız gerekmiyorsa, [bkz.](../app-service-web-get-started-dotnet.md)
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici, bir Uygulama Hizmet Ortamı oluşturduğunuzu varsayar. Bunu henüz yapmadıysanız, [bkz.](app-service-web-how-to-create-an-app-service-environment.md) 

## <a name="create-a-web-app"></a>Web uygulaması oluşturma
1. Azure [Portalı'nda,](https://portal.azure.com/) **Web + Mobil > Web Uygulaması > kaynak oluştur'u**tıklatın. 
   
    ![][1]
2. Aboneliğinizi seçin.  
   
    Birden fazla aboneliğiniz varsa, Uygulama Hizmeti Ortamınızda bir uygulama oluşturmak için ortamı oluştururken kullandığınız aboneliği kullanmanız gerektiğini unutmayın. 
3. Kaynak grubunu seçin veya oluşturun.
   
    *Kaynak grupları,* ilgili Azure kaynaklarını birim olarak yönetmenize olanak tanır ve uygulamalarınız için *rol tabanlı erişim denetimi* (RBAC) kuralları oluştururken kullanışlıdır. Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış][ResourceGroups]. 
4. Bir App Service planı seçin ya da oluşturun.
   
    *Uygulama Hizmeti planları,* yönetilen web uygulamaları kümeleridir.  Normalde fiyatlandırmayı seçtiğinizde, ücretlendirilen fiyat tek tek uygulamalar yerine Uygulama Hizmeti planına uygulanır. BIR ASE'de, ASP'nizle listelediğiniz işlem yerine ASE'ye tahsis edilen işlem örnekleri için ödeme yapmışsınız.  Bir web uygulamasının örnek sayısını ölçeklendirmek için Uygulama Hizmeti planınızın örneklerini ölçeklendirin ve bu plandaki tüm web uygulamalarını etkiler.  Site yuvaları veya VNET Tümleştirmesi gibi bazı özelliklerin de plan içinde miktar kısıtlamaları vardır.  Daha fazla bilgi için Azure [Uygulama Hizmeti planlarına genel bakış](../overview-hosting-plans.md)
   
    Plan adı altında belirtilen konuma bakarak ASE'nizdeki Uygulama Hizmeti planlarını tanımlayabilirsiniz.  
   
    ![][5]
   
    Uygulama Hizmeti Ortamınızda zaten var olan bir Uygulama Hizmeti planı kullanmak istiyorsanız, bu planı seçin. Yeni bir Uygulama Hizmeti planı oluşturmak istiyorsanız, bu öğreticinin aşağıdaki bölümüne bakın, [Uygulama Hizmeti Ortamında Bir Uygulama Hizmeti planı oluşturun.](#createplan)
5. Web uygulamanızın adını girin ve ardından **Oluştur'u**tıklatın. 
   
    ASE'niz Harici VIP kullanıyorsa, ASE'deki bir uygulamanın URL'si: [*site adı*]. [*Uygulama Hizmet Ortamınızın adı*].p.azurewebsites.net yerine [ site*adı*].azurewebsites.net
   
    ASE'niz bir Dahili VIP kullanıyorsa, o ASE'deki bir uygulamanın URL'si şudur: [*site adı*]. [*ASE oluşturma sırasında belirtilen alt etki alanı*]   
    ASE oluşturma sırasında ASP'nizi seçtikten sonra **Ad'in** altındaki alt alan adı güncelleştirmesini görürsünüz

## <a name="create-an-app-service-plan"></a><a name="createplan"></a>Uygulama Hizmeti planı oluşturma
Bir Uygulama Hizmet Ortamında bir Uygulama Hizmeti planı oluşturduğunuzda, ASE'de paylaşılan çalışan olmadığı için çalışan seçimleriniz farklıdır.  Kullanmanız gereken işçiler, yönetici tarafından ASE'ye tahsis edilen lerdir.  Bu, yeni bir plan oluşturmak için, ASE işçi havuzunuza, o çalışan havuzundaki tüm planlarınızdaki toplam örnek sayısından daha fazla işçi ayırmanız gerektiği anlamına gelir.  Planınızı oluşturmak için ASE işçi havuzunuzda yeterli işçi yoksa, bunları eklemek için ASE yöneticinizle birlikte çalışmanız gerekir.

App Service Ortamı tarafından barındırılan Uygulama Hizmeti planlarından bir diğer fark da, fiyatlandırma seçiminin olmamasıdır.  Bir Uygulama Hizmet Ortamınız varsa, sistem tarafından kullanılan hesaplama kaynakları için ödeme yaparsınız ve bu ortamdaki planlar için ek ücret ödemezsiniz.  Normalde bir Uygulama Hizmeti planı oluşturduğunuzda, faturalandırmanızı belirleyen bir fiyatlandırma planı seçersiniz.  Uygulama Hizmeti Ortamı, temelde içerik oluşturabileceğiniz özel bir konumdur.  Ortamın ücretini ödersiniz ve içeriğinizi barındırmazsınız.

Aşağıdaki talimatlar, öğreticinin önceki bölümünde açıklandığı gibi bir web uygulaması oluştururken bir Uygulama Hizmeti planının nasıl oluşturulacağını gösterir.

1. Plan seçimi nde **Yeni Oluştur'u** tıklatın ve planınız için bir ad sağlayın, tıpkı normalde bir ASE dışında olduğu gibi.
2. Konum seçicinizden kullanmak istediğiniz ASE'yi seçin.
   
    Uygulama Hizmet Ortamı aslında özel bir dağıtım konumu olduğundan, Konum altında gösterir. 
   
    ![][2]
   
    Konum seçicide bir ASE seçildikten sonra, Uygulama Hizmeti oluşturma ui güncelleştirmeleri planı.  Konum artık ASE sisteminin adını ve içinde bulunduğu bölgenin adını gösterir ve fiyatlandırma planı seçicibir işçi havuzu seçici ile değiştirilir.  
   
    ![][3]

### <a name="selecting-a-worker-pool"></a>İşçi havuzu seçme
Normalde Azure Uygulama Hizmeti'nde ve Bir Uygulama Hizmet Ortamı dışında, özel bir fiyat planı seçimiyle kullanılabilen 3 işlem boyutu vardır.  Benzer bir şekilde, bir ASE için en fazla 3 işçi havuzu tanımlayabilir ve bu işçi havuzu için kullanılan bilgi işlem boyutunu belirtebilirsiniz.  BUNUN ASE'nin kiracıları için anlamı, Uygulama Hizmeti planınız için işlem boyutuna sahip bir fiyatlandırma planı seçmek yerine, *işçi havuzu*olarak adlandırılan şeyi seçmenizdir.  

İşçi havuzu seçimi Kullanıcı Arabirimi, adın altındaki o alt havuz için kullanılan işlem boyutunu gösterir.  Kullanılabilir miktar, bu havuzda kullanılmak üzere kaç işlem örneğinin kullanılabildiğini ifade eder.  Toplam havuz aslında bu sayıdan daha fazla örneği olabilir, ancak bu değer yalnızca kaç tanesinin kullanılmadığını ifade eder.  Daha fazla bilgi kaynağı eklemek için Uygulama Hizmet Ortamınızı ayarlamanız gerekiyorsa, [Bkz. Uygulama Hizmet Ortamınızı Yapılandırma.](app-service-web-configure-an-app-service-environment.md)

![][4]

Bu örnekte yalnızca iki alt havuz görüyorsunuz. Bunun nedeni, ASE yöneticisinin yalnızca bu iki alt havuza ana bilgisayar ayırmasıdır.  Üçüncü sü, içine ayrılan VM'ler olduğunda ortaya çıkar.  

## <a name="after-web-app-creation"></a>Web uygulaması oluşturulduktan sonra
Web uygulamalarını çalıştırmak ve App Service planlarını dikkate alınması gereken bir ASE'de yönetmek için birkaç husus vardır.  

Daha önce de belirtildiği gibi, ASE'nin sahibi sistemin büyüklüğünden sorumludur ve sonuç olarak da istenilen App Service planlarını barındırmak için yeterli kapasitenin olduğundan sorumludurlar. Kullanılabilir çalışan yoksa, Uygulama Hizmeti planınızı oluşturamazsınız.  Bu, web uygulamanızı ölçekleme için de geçerlidir.  Daha fazla örneğe ihtiyacınız varsa, daha fazla işçi eklemek için App Service Environment yöneticinizi almanız gerekir.

Web uygulamanızı ve Uygulama Hizmeti planınızı oluşturduktan sonra, bu uygulamayı ölçeklendirmek iyi bir fikirdir.  Bir ASE'de, uygulamalarınız için hata toleransı sağlamak için Uygulama Hizmeti planınızın en az 2 örneğine sahip olmanız gerekir.  Bir ASE'de Bir Uygulama Hizmeti planını ölçekleme, Uygulama Hizmeti planı ui ile normal le aynıdır.  Ölçekleme hakkında daha fazla bilgi için, [Uygulama Hizmeti Ortamında bir web uygulaması nasıl ölçeklendirilir?](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[Appserviceplans]: ../overview-hosting-plans.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoScale]: app-service-web-scale-a-web-app-in-an-app-service-environment.md
[HowtoConfigureASE]: app-service-web-configure-an-app-service-environment.md
[ResourceGroups]: ../../azure-resource-manager/management/overview.md
[AzurePowershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
