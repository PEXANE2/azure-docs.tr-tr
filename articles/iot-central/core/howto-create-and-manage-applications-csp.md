---
title: Azure IoT Central uygulamalarını CSP portalından oluşturun ve yönetin | Microsoft Dokümanlar
description: CSP olarak, müşteriniz adına azure IoT Central uygulaması nasıl oluşturulur?
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: how-to
manager: philmea
ms.openlocfilehash: 02481d5dcbaba15c9b17a27348207d9af64f3355
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80982047"
---
# <a name="create-and-manage-an-azure-iot-central-application-from-the-csp-portal"></a>CSP portalından bir Azure IoT Merkezi uygulaması oluşturma ve yönetme

Microsoft Bulut Çözüm Sağlayıcısı (CSP) programı bir Microsoft Bayi programıdır. Amacı, kanal ortaklarımıza tüm Microsoft Ticari Çevrimiçi Hizmetlerini yeniden satmak için tek noktadan bir program sağlamaktır. Bulut Çözüm [Sağlayıcısı programı](https://partner.microsoft.com/cloud-solution-provider)hakkında daha fazla bilgi edinin.

CSP olarak, [Microsoft İş Ortağı Merkezi](https://partnercenter.microsoft.com/partner/home)aracılığıyla müşterileriniz adına Microsoft Azure IoT Central uygulamaları oluşturabilir ve yönetebilirsiniz. Azure IoT Central uygulamaları, tıpkı diğer CSP yönetilen Azure hizmetlerinde olduğu gibi CSP'ler tarafından müşteriler adına oluşturulduğunda, CSP'ler müşteriler için faturalandırmayı yönetir. Azure IoT Central için bir ücret, Microsoft İş Ortağı Merkezi'ndeki toplam faturanızda görünür.

Başlamak için Microsoft İş Ortağı Portalı'nda hesabınızda oturum açın ve bir Azure IoT Merkezi uygulaması oluşturmak istediğiniz bir müşteri seçin. Sol navigasyondan müşteri için Servis Yönetimi'ne gidin.

![Microsoft İş Ortağı Merkezi, müşteri görünümü](media/howto-create-and-manage-applications-csp/image1.png)

Azure IoT Central, yönetilebilecek bir hizmet olarak listelenir. Yeni uygulamalar oluşturmak veya bu müşteri için varolan uygulamaları yönetmek için sayfadaki Azure IoT Merkezi bağlantısını seçin.

![Azure IoT Central yönetilebilir](media/howto-create-and-manage-applications-csp/image2.png)

Azure IoT Merkezi Uygulama Yöneticisi sayfasına inersiniz. Azure IoT Central, Microsoft İş Ortağı Merkezi'nden geldiğiniz ve belirli bir müşteriyi yönetmek için geldiğiniz bağlamı tutar. Bunu Uygulama Yöneticisi sayfasının üstbilgisinde kabul görürsünüz. Buradan, bu müşterinin yönetmesi veya müşteri için yeni bir uygulama oluşturması için daha önce oluşturduğunuz varolan bir uygulamaya gidebilirsiniz.

![CSP'ler için Yönetici Oluştur](media/howto-create-and-manage-applications-csp/image3.png)

Azure IoT Merkezi uygulaması oluşturmak için sol menüde **Oluştur'u** seçin. Endüstri şablonlarından birini seçin veya sıfırdan bir uygulama oluşturmak için **Özel uygulamayı** seçin. Bu, Uygulama Oluşturma sayfasını yükler. Bu sayfadaki tüm alanları tamamlamanız ve sonra **Oluştur'u**seçmeniz gerekir. Aşağıdaki alanların her biri hakkında daha fazla bilgi bulabilirsiniz.

![CSP'ler için Uygulama Sayfası Oluşturma](media/howto-create-and-manage-applications-csp/image4.png)

![CSP'ler için Uygulama Sayfası Oluşturma](media/howto-create-and-manage-applications-csp/image4-1.png)

![CSP'ler fatura bilgileri için Uygulama Sayfası Oluşturma](media/howto-create-and-manage-applications-csp/image4-2.png)

## <a name="pricing-plan"></a>Fiyatlandırma planı

Yalnızca standart bir fiyatlandırma planını CSP olarak kullanan uygulamalar oluşturabilirsiniz. Azure IoT Central'ı müşterinize göstermek için, ücretsiz fiyatlandırma planını ayrı olarak kullanan bir uygulama oluşturabilirsiniz. [Azure IoT Merkezi fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/iot-central/)ücretsiz ve standart fiyatlandırma planları hakkında daha fazla bilgi edinin.

Yalnızca standart bir fiyatlandırma planını CSP olarak kullanan uygulamalar oluşturabilirsiniz. Azure IoT Central'ı müşterinize göstermek için, ücretsiz fiyatlandırma planını ayrı olarak kullanan bir uygulama oluşturabilirsiniz. [Azure IoT Merkezi fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/iot-central/)ücretsiz ve standart fiyatlandırma planları hakkında daha fazla bilgi edinin.

## <a name="application-name"></a>Uygulama adı

Uygulamanızın adı **Uygulama Yöneticisi** sayfasında ve her Azure IoT Merkezi uygulamasında görüntülenir. Azure IoT Central uygulamanız için istediğiniz adı seçebilirsiniz. Size ve kuruluşunuzdaki diğer kişilere anlamlı bir ad seçin.

## <a name="application-url"></a>Uygulama URL'si

Uygulama URL'si, uygulamanızın bağlantısıdır. Bir yer işaretini tarayıcınızda kaydedebilir veya başkalarıyla paylaşabilirsiniz.

Uygulamanızın adını girdiğinizde, uygulama URL'niz otomatik olarak oluşturulur. İsterseniz, uygulamanız için farklı bir URL seçebilirsiniz. Her Azure IoT Merkezi URL'si Azure IoT Central içinde benzersiz olmalıdır. Seçtiğiniz URL zaten alınmışsa bir hata iletisi görürsünüz.

## <a name="directory"></a>Dizin

Azure IoT Central, Microsoft İş Ortağı Portalı'nda seçtiğiniz müşteriyi yönetmek için geldiğiniz içeriğe sahip olduğundan, Dizin alanında bu müşteri için yalnızca Azure Etkin Dizin kiracısını görürsünüz. 

Azure Etkin Dizin kiracısı kullanıcı kimlikleri, kimlik bilgileri ve diğer kuruluş bilgilerini içerir. Birden çok Azure aboneliği tek bir Azure Etkin Dizin kiracısıyla ilişkilendirilebilir.

Daha fazla bilgi için [Azure Etkin Dizini'ne](https://docs.microsoft.com/azure/active-directory/)bakın.

## <a name="azure-subscription"></a>Azure aboneliği

Azure aboneliği, Azure hizmetlerinin örneklerini oluşturmanıza olanak sağlar. Azure IoT Central, erişebildiğiniz müşterinin tüm Azure Aboneliklerini otomatik olarak bulur ve **bunları Uygulama Oluştur** sayfasında bir açılır sayfada görüntüler. Yeni bir Azure IoT Merkezi Uygulaması oluşturmak için bir Azure aboneliği seçin.

Azure aboneliğiniz yoksa, Microsoft İş Ortağı Merkezi'nde bir abonelik oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra, **Uygulama Oluştur** sayfasına geri gidin. Yeni aboneliğiniz **Azure Aboneliği** açılan listesinde gösterilir.

Daha fazla bilgi için [Azure aboneliklerine](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)bakın.

## <a name="location"></a>Konum

**Konum,** uygulamayı oluşturmak istediğiniz [coğrafyadır.](https://azure.microsoft.com/global-infrastructure/geographies/) Genellikle, en iyi performansı elde etmek için fiziksel olarak aygıtlarınıza en yakın konumu seçmeniz gerekir. Şu anda, **Avustralya,** **Asya Pasifik,** **Avrupa,** **Amerika Birleşik Devletleri,** **İngiltere**ve **Japonya** coğrafyalarında bir IoT Merkezi uygulama oluşturabilirsiniz. Bir konum seçtikten sonra, daha sonra uygulamanızı başka bir konuma taşıyamazsınız.

## <a name="application-template"></a>Uygulama şablonu

Uygulamanız için kullanmak istediğiniz uygulama şablonunu seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Merkezi uygulamasını CSP olarak nasıl oluşturabileceğinizi öğrendiğiniz egöre, önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)
