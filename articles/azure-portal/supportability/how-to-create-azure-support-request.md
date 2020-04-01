---
title: Azure destek isteği oluşturma | Microsoft Dokümanlar
description: Yardıma ihtiyacı olan müşteriler, self servis çözümleri bulmak ve destek isteklerioluşturmak ve yönetmek için Azure portalını kullanabilir.
services: Azure Supportability
author: ganganarayanan
manager: scotthit
ms.assetid: fd6841ea-c1d5-4bb7-86bd-0c708d193b89
ms.service: azure-supportability
ms.topic: article
ms.date: 03/31/2020
ms.author: kfollis
ms.openlocfilehash: 2b68b6ddf19d6b07475e7009b47e162bfb2d0d2f
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478992"
---
# <a name="how-to-create-an-azure-support-request"></a>Azure destek isteği oluşturma

## <a name="overview"></a>Genel Bakış

Azure, destek biletleri olarak da bilinen destek isteklerioluşturmanıza ve yönetmenize olanak tanır. Bu makalede yer alan [Azure portalında](https://portal.azure.com)istekler oluşturabilir ve yönetebilirsiniz. [Ayrıca Azure destek bileti REST API'yi](/rest/api/support)kullanarak istekleri programlı bir şekilde oluşturabilir ve yönetebilirsiniz.

> [!NOTE]
> Azure portal URL'si, kuruluşunuzun dağıtıldığı Azure bulutuna özgüdür.
>
>* Ticari kullanım için Azure portalı:[https://portal.azure.com](https://portal.azure.com)
>* Almanya için Azure portalı:[https://portal.microsoftazure.de](https://portal.microsoftazure.de)
>* ABD hükümeti için Azure portalı:[https://portal.azure.us](https://portal.azure.us)
>
>

Müşteri geri bildirimlerine dayanarak, destek isteği deneyimini üç ana hedefe odaklanmak üzere güncelledik:

* **Kolaylaştırılmış**: Destek ve sorun giderme yi bulmayı kolaylaştırın ve destek isteğini nasıl gönderdiğinizi basitleştirin.
* **Tümleşik**: Bir Azure kaynağıyla ilgili bir sorunu giderirken, bağlam değiştirmeden bir destek isteğini kolayca açabilirsiniz.
* **Verimli**: Sorununuzu verimli bir şekilde çözmek için destek aracınızın ihtiyaç duyduğu temel bilgileri toplayın.

## <a name="getting-started"></a>Başlarken

Azure portalında **Yardım + desteğine** ulaşabilirsiniz. Azure portal menüsünden, genel üstbilgiden veya bir hizmetin kaynak menüsünden edinilebilir. Bir destek isteği nde bulunabiliyor sanız, uygun izinlere sahip olmalısınız.

### <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Bir destek isteği oluşturmak için, bir [Sahip](../../role-based-access-control/built-in-roles.md#owner), [Katılımcı](../../role-based-access-control/built-in-roles.md#contributor) veya abonelik düzeyinde [Destek İstek Katılımcısı](../../role-based-access-control/built-in-roles.md#support-request-contributor) rolüne atanmış olmalısınız. Abonelik olmadan bir destek isteği oluşturmak için (örneğin Azure Active Directory (AAD) senaryosu, [yönetici](../../active-directory/users-groups-roles/directory-assign-admin-roles.md)olmalısınız.

### <a name="go-to-help--support-from-the-global-header"></a>Global üstbilgiden Yardım + desteğine git

Azure portalının herhangi bir yerinden destek isteği başlatmak için:

1. Power BI hizmetinde **?** genel üstbilgide. Sonra **Yardım + destek'i**seçin.

   ![Yardım ve Destek](./media/how-to-create-azure-support-request/helpandsupportnewlower.png)

2. **Yeni destek isteği**’ni seçin. Sorununuzun hakkında bize bilgi vermek için istemleri izleyin. Bazı olası çözümler önereceğiz, sorunla ilgili ayrıntıları toplayacağız ve destek isteğini göndermenize ve izlemenize yardımcı olacağız.

   ![Yeni Destek İsteği](./media/how-to-create-azure-support-request/newsupportrequest2lower.png)

### <a name="go-to-help--support-from-a-resource-menu"></a>Kaynak menüsünden Yardım + desteğine git

Kaynak bağlamında bir destek isteği başlatmak için şu anda şu anda çalışıyorsunuz:

1. Kaynak menüsünden Destek **+ Sorun Giderme** bölümünde **Yeni destek isteği'ni**seçin.

   ![Bağlamda](./media/how-to-create-azure-support-request/incontext2lower.png)

2. Yaşadığınız sorun hakkında bize bilgi vermek için istemleri izleyin. Kaynaktan destek isteği işlemini başlattığınızda, bazı seçenekler sizin için önceden seçilir.

## <a name="create-a-support-request"></a>Destek isteği oluşturma

Sorununuz hakkında bilgi toplamak ve sorunu çözmenize yardımcı olmak için size bazı adımlar atacağız. Her adım aşağıdaki bölümlerde açıklanmıştır.

### <a name="basics"></a>Temel Bilgiler

Destek isteği sürecinin ilk adımı, sorununuzun ve destek planınız hakkındaki temel bilgileri toplar.

**Yeni destek isteğinin** **Temeller** sekmesinde, sorun hakkında bize bilgi vermek için seçicileri kullanın. İlk olarak, sorun türü için bazı genel kategorileri tanımlar ve ilgili aboneliği seçersiniz. Windows **çalıştıran Sanal Makine**gibi hizmeti seçin. Sanal makinenizin adı gibi kaynağı seçin. Sorunu kendi sözcüklerinizle açıklayın, ardından daha spesifik hale getirmek için **sorun türünü seçin.**

![Temel Bilgiler dikey penceresi](./media/how-to-create-azure-support-request/basics2lower.png)

> [!NOTE]
> Azure, abonelik yönetimi için faturalandırma, kota ayarlamaları ve hesap aktarımlarını içeren sınırsız destek sağlar. Teknik destek için bir destek planına ihtiyacınız var. [Destek planları hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/support/plans)
>
>

### <a name="solutions"></a>Çözümler

Temel bilgileri topladıktan sonra, bir sonraki size kendi denemeniz için çözümler gösteririz. Bazı durumlarda, hızlı bir teşhis bile yapabiliriz. Çözümler Azure mühendisleri tarafından yazılır ve en sık karşılaşılan sorunları çözer.

### <a name="details"></a>Ayrıntılar

Ardından, sorun hakkında ek ayrıntılar toplarız. Bu adımda ayrıntılı ve ayrıntılı bilgi sağlamak, destek isteğinizi doğru temsilciye yönlendirmemize yardımcı olur.

Mümkünse, sorunun ne zaman başladığını ve yeniden oluşturmak için herhangi bir adım bize bildirin. Günlük dosyası veya tanılamadan çıktı gibi bir dosya yükleyebilirsiniz.

Sorunla ilgili tüm bilgilere sahip olduktan sonra, nasıl destek alacağınızı seçin. **Ayrıntılar'ın** **Destek yöntemi** bölümünde, etkinin önem derecesini seçin. Tercih ettiğiniz iletişim yöntemini, sizinle iletişim kurmak için iyi bir zaman ve destek dilinizi sağlayın.

Ardından, sizinle nasıl iletişim kuracağını bilmemiz için **İletişim bilgileri** bölümünü tamamlayın.

### <a name="review--create"></a>İnceleme + oluşturma

Her sekmede gerekli tüm bilgileri tamamlayın, ardından **Gözden Geçir + oluştur'u**seçin. Destek'e göndereceğiniz ayrıntıları kontrol edin. Gerekirse değişiklik yapmak için herhangi bir sekmeye geri dön. Destek isteği tamamlandığından emin olduğunuzda **Oluştur'u**seçin.

Bir destek aracısı belirttiğiniz yöntemi kullanarak sizinle irtibata geçecektir. İlk yanıt süresi hakkında bilgi için [Destek kapsamı ve yanıt verme](https://azure.microsoft.com/support/plans/response/)süresine bakın.

## <a name="all-support-requests"></a>Tüm destek istekleri

Destek isteklerinin ayrıntılarını ve durumunu Yardım **+ destek** >  **tüm destek isteklerine**giderek görüntüleyebilirsiniz.

![Tüm destek istekleri](./media/how-to-create-azure-support-request/allrequestslower.png)

Bu sayfada, destek isteklerini **Abonelik,** **Oluşturulan** tarih (UTC) ve **Durum'a**göre filtreleyebilirsiniz. Ayrıca, bu sayfada destek isteklerini sıralayabilir ve arayabilirsiniz.

Önem derecesi ve bir destek aracısının yanıt vermesi için beklenen süre yi de dahil olmak üzere ayrıntıları görüntülemek için bir destek isteği seçin.

İsteğin önem derecesini değiştirmek istiyorsanız, **İş etkisi'ni**seçin. Atamak için önemleri listesinden seçim yapın.

> [!NOTE]
> Maksimum önem düzeyi destek planınıza bağlıdır. [Destek planları hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/support/plans)
>
>
Azure'da kendi kendine yardım destek seçenekleri hakkında daha fazla bilgi edinmek için şu videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/gNhzR5FE9DY]

## <a name="next-steps"></a>Sonraki adımlar

* [Görüş ve önerilerinizi bize gönderin](https://feedback.azure.com/forums/266794-support-feedback)
* [Twitter'da](https://twitter.com/azuresupport) bizimle etkileşime
* [MSDN forumlarında](https://social.msdn.microsoft.com/Forums/azure) yaşıtlarınızdan yardım alın
* [Azure Destek SSS'sinde](https://azure.microsoft.com/support/faq) daha fazla bilgi edinin
