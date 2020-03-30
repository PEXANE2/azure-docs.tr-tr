---
title: IoT çözüm hızlandırıcıları SSS - Azure | Microsoft Dokümanlar
description: Bu makalede, IoT çözüm hızlandırıcıları için sık sorulan soruları yanıtlar. GitHub depolarına bağlantılar içerir.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: 7a2b167f90b4ec79f5fa515ded1c676ca7085e72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75647722"
---
# <a name="frequently-asked-questions-for-iot-solution-accelerators"></a>IoT çözüm hızlandırıcıları için sık sorulan sorular

Ayrıca bakınız, [Bağlı Fabrikaya özel SSS](iot-accelerators-faq-cf.md) ve [Uzaktan İzlemeye Özel SSS.](iot-accelerators-faq-rm-v2.md)

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerators"></a>Çözüm hızlandırıcıları için kaynak kodu nerede bulabilirim?

Kaynak kodu aşağıdaki GitHub depolarında depolanır:

* [Uzaktan İzleme çözüm hızlandırıcısı (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Uzaktan İzleme çözüm hızlandırıcısı (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Predictive Bakım çözüm hızlandırıcı](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Bağlı Fabrika çözüm hızlandırıcısı](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-sdks-can-i-use-to-develop-device-clients-for-the-solution-accelerators"></a>Çözüm hızlandırıcıları için cihaz istemcileri geliştirmek için hangi SDK'ları kullanabilirim?

[Microsoft Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) GitHub depolarında farklı dillere (C, .NET, Java, Node.js, Python) IoT aygıtı SDK'ları bağlantılarını bulabilirsiniz.

DevKit aygıtını kullanıyorsanız, kaynakları ve örnekleri [IoT DevKit SDK](https://github.com/Microsoft/devkit-sdk) GitHub deposunda bulabilirsiniz.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-solution-accelerators"></a>Yeni microservices mimarisi üç çözüm hızlandırıcısı için de kullanılabilir mi?

Şu anda, yalnızca Uzaktan İzleme çözümü en geniş senaryoyu kapsadığı için mikro hizmetler mimarisini kullanır.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Yeni açık kaynaklı mikrohizmetler tabanlı mimari yeni güncelleştirmede ne gibi avantajlar sağlar?

Son iki yılda bulut mimarisi büyük ölçüde gelişti. Mikrohizmetler, geliştirme hızından ödün vermeden ölçek ve esneklik elde etmek için harika bir model olarak ortaya çıkmıştır. Bu mimari desen, büyük güvenilirlik ve ölçeklenebilirlik sonuçları yla dahili olarak birkaç Microsoft hizmetinde kullanılır. Microsoft, müşterilerin bu öğrenmelerden yararlanabilmesi için çözüm hızlandırıcılarında bu öğrenmeleri uygulamaya koyuyor.

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-azure-ad-tenant-how-do-i-complete-this-task"></a>Bir hizmet yöneticisiyim ve aboneliğim ve belirli bir Azure AD kiracısı arasındaki dizin eşlemi değiştirmek istiyorum. Bu görevi nasıl tamamlarım?

Bkz. [Azure REKLAM dizininize varolan bir abonelik eklemek için](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>Kuruluş hesabıyla oturum açtığınızda bir Hizmet Yöneticisini veya Yardımcı Yöneticiyi değiştirmek istiyorum

[Kuruluş hesabıyla oturum açtığınızda Hizmet Yöneticisi ve Yardımcı Yönetici Değiştirme](https://azure.microsoft.com/support/changing-service-admin-and-co-admin)destek makalesine bakın.

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Bu hatayı neden görüyorum? "Hesabınız bir çözüm oluşturmak için uygun izinlere sahip değil. Lütfen hesap yöneticinize başvurun veya farklı bir hesap ile deneyin."

Kılavuz için aşağıdaki diyagrama bakın:

![İzinler akış şeması](media/iot-accelerators-faq/flowchart.png)

> [!NOTE]
> Azure AD kiracısının genel yöneticisi ve aboneliğin ortak yöneticisi olduğunuzu doğruladıktan sonra hatayı görmeye devam ederseniz, hesap yöneticinizin kullanıcıyı kaldırmasını ve bu sırada gerekli izinleri yeniden atamasını sorun. İlk olarak, kullanıcıyı genel yönetici olarak ekleyin ve ardından Azure aboneliğinin yardımcı yöneticisi olarak kullanıcıyı ekleyin. Sorunlar devam ederse, [Yardım & Destek'e](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)başvurun.

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Bir Azure aboneliğim varken bu hatayı neden görüyorum? "Önceden yapılandırılmış çözümler oluşturmak için Azure aboneliği gereklidir. Birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz."

Azure aboneliğiniz olduğundan eminseniz, aboneliğiniz için kiracı eşleciliğini doğrulayın ve açılır açılır durumda doğru kiracının seçilip seçilmediğini kontrol edin. Kiracının doğru olduğunu doğruladıysanız, önceki diyagramı izleyin ve aboneliğinizin ve bu Azure AD kiracısının eşlenesini doğrulayın.

### <a name="where-can-i-find-information-about-the-previous-version-of-the-remote-monitoring-solution"></a>Uzaktan İzleme çözümünün önceki sürümü hakkında nerede bilgi bulabilirim?

Uzaktan İzleme çözüm hızlandırıcısının önceki sürümü, Önceden yapılandırılmış çözüm IoT Suite Uzaktan İzleme olarak biliniyordu. Arşivlenmiş belgeleri [https://docs.microsoft.com/previous-versions/azure/iot-suite/](https://docs.microsoft.com/previous-versions/azure/iot-suite/).

### <a name="is-the-new-solution-accelerator-available-in-the-same-geographic-region-as-the-existing-solution"></a>Yeni çözüm hızlandırıcısı mevcut çözümle aynı coğrafi bölgede kullanılabilir mi?

Evet, yeni Uzaktan İzleme aynı coğrafi bölgelerde kullanılabilir.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-solution-accelerator-in-azureiotsolutionscom"></a>Azure portalında bir kaynak grubunu silmek le azureiotsolutions.com'da çözüm hızlandırıcısına silmek arasındaki fark nedir?

* [azureiotsolutions.com'da](https://www.azureiotsolutions.com/)çözüm hızlandırıcısını silerseniz, çözüm hızlandırıcısını oluşturduğunuzda dağıtılan tüm kaynakları silersiniz. Kaynak grubuna ek kaynaklar eklediyseniz, bu kaynaklar da silinir.
* [Azure portalındaki](https://portal.azure.com)kaynak grubunu silerseniz, yalnızca bu kaynak grubundaki kaynakları silersiniz. Ayrıca çözüm hızlandırıcısıyla ilişkili Azure Active Directory uygulamasını da silmeniz gerekir.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-solution-accelerators"></a>Azure IoT çözüm hızlandırıcılarına yaptığım mevcut yatırımlardan yararlanmaya devam edebilir miyim?

Evet. Bugün var olan herhangi bir çözüm Azure aboneliğinizde çalışmaya devam ediyor ve kaynak kodu GitHub'da kullanılabilir durumda kalır.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç IoT Hub örneği sağlayabilirim?

Varsayılan olarak [abonelik başına 10 IoT hub'ı](../azure-resource-manager/management/azure-subscription-service-limits.md#iot-hub-limits)sağlayabilirsiniz. Bu sınırı yükseltmek için bir [Azure destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) oluşturabilirsiniz. Sonuç olarak, her çözüm hızlandırıcısı yeni bir IoT Hub'ı karşıladığından, belirli bir abonelikte yalnızca en fazla 10 çözüm hızlandırıcısı sağlayabilirsiniz.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç tane Azure Cosmos DB örneği sağlayabilirim?

Elli. Bu sınırı yükseltmek için bir [Azure destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) oluşturabilirsiniz, ancak varsayılan olarak abonelik başına yalnızca 50 Cosmos DB örneği sağlayabilirsiniz.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç tane Ücretsiz Bing Haritaları API'si sağlayabilirim?

İki. Bir Azure aboneliğinde Enterprise planları için yalnızca iki adet İç İşlemler Düzey 1 Bing Haritası oluşturabilirsiniz. Uzaktan İzleme çözümü, İç İşlemler Düzey 1 planıyla varsayılan olarak sağlanır. Sonuç olarak, hiçbir değişiklik olmadan bir abonelikte yalnızca en fazla iki Uzaktan İzleme çözümü sağlayabilirsiniz.

### <a name="can-i-create-a-solution-accelerator-if-i-have-microsoft-azure-for-dreamspark"></a>DreamSpark için Microsoft Azure'a sahipsem çözüm hızlandırıcısı oluşturabilir miyim?

> [!NOTE]
> DreamSpark için Microsoft Azure artık öğrenciler için Microsoft Imagine olarak bilinir.

Şu anda, [DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/) hesabı için Microsoft Azure hesabıyla bir çözüm hızlandırıcı oluşturamazsınız. Ancak, bir çözüm hızlandırıcı oluşturmanıza olanak tanıyan azure için birkaç dakika içinde ücretsiz bir [deneme hesabı](https://azure.microsoft.com/free/) oluşturabilirsiniz.

### <a name="how-do-i-delete-an-azure-ad-tenant"></a>Azure AD kiracılarını nasıl silerim?

Eric Golpe'nin blog gönderisine bakın Azure REKLAM Kiracısı Silme nin [Walkthrough'u.](https://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx)

### <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [Uzaktan İzleme çözüm hızlandırıcısının yeteneklerini keşfedin](quickstart-remote-monitoring-deploy.md)
* [Tahmine Dayalı Bakım çözüm hızlandırıcıya genel bakış](iot-accelerators-predictive-overview.md)
* [Bağlı Fabrika çözüm hızlandırıcıyı dağıtın](quickstart-connected-factory-deploy.md)
* [Baştan sona IoT güvenliği](/azure/iot-fundamentals/iot-security-ground-up)
