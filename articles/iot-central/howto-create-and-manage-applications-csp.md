---
title: Azure IoT Central uygulamalarını CSP olarak oluşturma ve yönetme | Microsoft Docs
description: Bir CSP olarak, müşteriniz adına bir Azure IoT Central uygulaması oluşturma.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 4d20256381eab155d4b7532591cea85e43080ec0
ms.sourcegitcommit: 80dff35a6ded18fa15bba633bf5b768aa2284fa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70019791"
---
# <a name="as-a-csp-create-and-manage-an-azure-iot-central-application-on-behalf-of-your-customer"></a>Bir CSP olarak, müşteriniz adına bir Azure IoT Central uygulaması oluşturun ve yönetin 

Microsoft Bulut çözümü sağlayıcısı (CSP) programı bir Microsoft satıcı programıdır. Amaç, tüm Microsoft ticari çevrimiçi hizmetlerini yeniden satmak için tek seferlik bir programla kanal iş ortaklarımızı sağlamaktır. [Bulut çözümü sağlayıcısı programı](https://partner.microsoft.com/cloud-solution-provider)hakkında daha fazla bilgi edinin.

Bir CSP olarak, [Microsoft Iş Ortağı Merkezi](https://partnercenter.microsoft.com/partner/home)aracılığıyla müşterileriniz adına Microsoft Azure IoT Central uygulamaları oluşturabilir ve yönetebilirsiniz. Azure IoT Central uygulamaları, diğer CSP ile yönetilen Azure hizmetlerinde olduğu gibi CSP 'Ler tarafından müşteriler adına oluşturulduğunda, CSP 'Ler müşteriler için faturalandırmayı yönetir. Azure IoT Central ücreti, Microsoft Iş Ortağı Merkezi 'nde toplam faturanızda görünür.

Başlamak için Microsoft Iş ortağı portalı 'nda hesabınızda oturum açın ve Azure IoT Central uygulaması oluşturmak istediğiniz müşteriyi seçin. Sol gezinti çubuğunda müşteri için hizmet yönetimi ' ne gidin.

![Microsoft Iş Ortağı Merkezi, müşteri görünümü](media/howto-create-application-csp/image1.png)

Azure IoT Central, yönetmek için kullanılabilen bir hizmet olarak listelenir. Yeni uygulamalar oluşturmak veya bu müşteri için mevcut uygulamaları yönetmek üzere sayfadaki Azure IoT Central bağlantısını seçin.

![Azure IoT Central yönetilecek kullanılabilir](media/howto-create-application-csp/image2.png)

Azure IoT Central uygulama Yöneticisi sayfasını kullanabilirsiniz. Azure IoT Central, Microsoft Iş Ortağı Merkezi 'nden gelen ve söz konusu müşteriyi yönetmek için aldığınız bağlamı korur. Bu bildirim, uygulama Yöneticisi sayfasının üstbilgisinde görüntülenir. Buradan, bu müşteri için daha önce oluşturduğunuz mevcut bir uygulamaya gidebilir veya müşteri için yeni bir uygulama oluşturabilirsiniz.

![CSP 'Ler için yönetici oluşturma](media/howto-create-application-csp/image3.png)

Azure IoT Central uygulaması oluşturmak için **Yeni uygulama** kutucuğunu seçin. Bu işlem, uygulama oluşturma sayfasını yükler. Bu sayfadaki tüm alanları tamamlayıp **Oluştur**' u seçmeniz gerekir. Aşağıdaki alanların her biri hakkında daha fazla bilgi bulabilirsiniz.

![CSP 'Ler için uygulama sayfası oluştur](media/howto-create-application-csp/image4.png)

![CSP 'Ler için uygulama sayfası oluştur](media/howto-create-application-csp/image4-1.png)

## <a name="payment-plan"></a>Ödeme planı

Yalnızca bir CSP olarak ücretli Kullandıkça Öde uygulamaları oluşturabilirsiniz. Müşterinizin Azure IoT Central göstermek için ayrı bir deneme uygulaması oluşturabilirsiniz. [Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/)deneme ve kullandıkça öde uygulamaları hakkında daha fazla bilgi edinin.

## <a name="application-name"></a>Uygulama Adı

Uygulamanızın adı, **Uygulama Yöneticisi** sayfasında ve her bir Azure IoT Central uygulaması içinde görüntülenir. Azure IoT Central uygulamanız için herhangi bir ad seçebilirsiniz. Sizin ve kuruluşunuzdaki diğer diğerlerine anlamlı bir ad seçin.

## <a name="application-url"></a>Uygulama URL'si

Uygulama URL 'SI, uygulamanızın bağlantıdır. Bir yer işaretini tarayıcınızda kaydedebilir veya başkalarıyla paylaşabilirsiniz.

Uygulamanızın adını girdiğinizde, uygulama URL 'niz otomatik olarak oluşturulur. İsterseniz, uygulamanız için farklı bir URL seçebilirsiniz. Her Azure IoT Central URL 'SI Azure IoT Central içinde benzersiz olmalıdır. Seçtiğiniz URL zaten alınmışsa bir hata iletisi görürsünüz.

## <a name="directory"></a>Dizin

Azure IoT Central, Microsoft Iş ortağı portalında seçtiğiniz müşteriyi yönetmek için aldığınız bağlamdan, Dizin alanında o müşteri için yalnızca Azure Active Directory kiracısını görürsünüz. 

Azure Active Directory Kiracı Kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Birden çok Azure aboneliği, tek bir Azure Active Directory kiracısıyla ilişkilendirilebilir.

Daha fazla bilgi için bkz. [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Azure aboneliği

Azure aboneliği, Azure Hizmetleri örnekleri oluşturmanıza olanak sağlar. Azure IoT Central, erişiminiz olan müşterinin tüm Azure aboneliklerini otomatik olarak bulur ve bunları **uygulama oluştur** sayfasında bir açılan pencerede görüntüler. Yeni bir Azure IoT Central uygulaması oluşturmak için bir Azure aboneliği seçin.

Azure aboneliğiniz yoksa, Microsoft Iş Ortağı Merkezi ' nde bir tane oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra, **Uygulama Oluştur** sayfasına geri gidin. Yeni aboneliğiniz **Azure Aboneliği** açılan listesinde gösterilir.

Daha fazla bilgi için bkz. [Azure abonelikleri](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Bölge

Azure IoT Central uygulamanızı oluşturmak istediğiniz bölgeyi seçin. Genellikle en iyi performansı elde etmek için cihazlarınıza en yakın fiziksel bölgeyi seçmeniz gerekir.

Daha fazla bilgi için bkz. [Azure bölgeleri](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#azure-regions).

Azure IoT Central 'ın [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/) sayfasında kullanılabildiği bölgeleri görebilirsiniz.

> [!Note]
> Bölgeyi bir kez seçtiğinizde, uygulamanızı daha sonra başka bir bölgeye taşıyamazsınız.

## <a name="application-template"></a>Uygulama şablonu

Yeni Azure IoT Central uygulamanız için kullanılabilir uygulama şablonlarından birini seçebilirsiniz. Uygulama şablonu, başlamanıza yardımcı olacak cihaz şablonları ve panolar gibi önceden tanımlanmış öğeleri içerebilir.

| Uygulama şablonu | Açıklama |
| -------------------- | ----------- |
| Özel uygulama   | Kendi cihaz şablonlarınız ve cihazlarınızla doldurabileceğiniz boş bir uygulama oluşturur. |
| Örnek Contoso       | Basit bağlı bir cihaz için cihaz şablonu içeren bir uygulama oluşturur. Azure IoT Central'ı incelemeye başlamak için bu şablonu kullanın. |
| Devkits Örneği       | MXChip veya Raspberry Pi cihazını bağlamak amacıyla sizin için hazırlanmış cihaz şablonlarıyla bir uygulama oluşturur. Bu cihazlardan birine yönelik bir cihaz geliştiricisi kullanıyorsanız bu şablonu kullanın. |

## <a name="next-steps"></a>Sonraki adımlar

Artık bir Azure IoT Central uygulamasının CSP olarak nasıl oluşturulacağını öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)