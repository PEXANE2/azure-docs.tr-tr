---
title: Azure destek isteği oluşturma | Microsoft Docs
description: Yardıma ihtiyacı olan müşteriler, self servis çözümlerini bulmak ve destek istekleri oluşturmak ve yönetmek için Azure portal kullanabilir.
services: Azure Supportability
author: ganganarayanan
manager: scotthit
ms.assetid: fd6841ea-c1d5-4bb7-86bd-0c708d193b89
ms.service: azure-supportability
ms.topic: article
ms.date: 01/23/2020
ms.author: kfollis
ms.openlocfilehash: 28a29bbb94f63657a69e873bb8f969e96ee92c76
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79248495"
---
# <a name="how-to-create-an-azure-support-request"></a>Azure destek isteği oluşturma

## <a name="overview"></a>Genel Bakış

Azure, destek bileti olarak da bilinen destek istekleri oluşturmanıza ve yönetmenize olanak sağlar. [Azure Portal](https://portal.azure.com), bu makalede ele alınan istekleri oluşturabilir ve yönetebilirsiniz. Ayrıca, [Azure destek bileti REST API](/rest/api/support)kullanarak istekleri programlı bir şekilde oluşturabilir ve yönetebilirsiniz.

> [!NOTE]
> Azure portal URL 'SI, kuruluşunuzun dağıtıldığı Azure bulutuna özgüdür.
>
>* Ticari kullanım için Azure portal: [https://portal.azure.com](https://portal.azure.com)
>* Almanya için Azure portal: [https://portal.microsoftazure.de](https://portal.microsoftazure.de)
>* Birleşik Devletler kamu Azure portal: [https://portal.azure.us](https://portal.azure.us)
>
>

Müşteri geri bildirimlerine göre, destek isteği deneyimini üç ana hedefle odaklanmak üzere güncelleştirdik:

* **Kolaylaştırılmış**: destek isteği göndermeyi ve yönetmeyi kolaylaştırmak için destek ve sorun gidermeyi kolaylaştırın.
* **Tümleşik**: bir Azure kaynağıyla ilgili sorun giderirken bağlam değiştirmeden kolayca bir destek isteği açabilirsiniz.
* **Verimli**: sorununuzu verimli bir şekilde çözmek için destek aracınızın ihtiyaç duyacağı anahtar bilgilerini toplayın.

## <a name="getting-started"></a>Başlarken

Azure portal **Yardım + Destek** alabilirsiniz. Bu, bir hizmetin Azure portal menüsünde, genel üst bilgisinde veya kaynak menüsünden kullanılabilir. Bir destek isteğini kaydedebilmeniz için, uygun izinlere sahip olmanız gerekir.

### <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Bir destek isteği oluşturmak için, bir yönetici olmanız veya abonelik düzeyinde [destek Isteği katılımcısı](../../role-based-access-control/built-in-roles.md#support-request-contributor) rolüne atanması gerekir.

### <a name="go-to-help--support-from-the-global-header"></a>Genel üst bilgiden yardım + destek 'e gidin

Azure portal her yerden bir destek talebi başlatmak için:

1. Seçin **?** Genel üst bilgisinde. **Yardım + Destek**' i seçin.

   ![Yardım ve Destek](./media/how-to-create-azure-support-request/helpandsupportnewlower.png)

2. **Yeni destek isteği**’ni seçin. Bize sorun hakkında bilgi sağlamak için istemleri izleyin. Bazı olası çözümler önereceğiz, sorunla ilgili ayrıntıların toplanması ve Destek isteğini göndermenize ve izlemenize yardımcı olacak.

   ![Yeni Destek İsteği](./media/how-to-create-azure-support-request/newsupportrequest2lower.png)

### <a name="go-to-help--support-from-a-resource-menu"></a>Kaynak menüsünden Yardım + Destek 'e gidin

Kaynak bağlamında bir destek talebi başlatmak için şu anda çalışıyorsunuz:

1. Kaynak menüsünde, **destek + sorun giderme** bölümünde **Yeni destek isteği**' ni seçin.

   ![Bağlamda](./media/how-to-create-azure-support-request/incontext2lower.png)

2. Bize karşılaştığınız sorun hakkında bilgi sağlamak için istemleri izleyin. Kaynaktan destek isteği sürecini başlattığınızda, bazı seçenekler sizin için önceden seçilmiştir.

## <a name="create-a-support-request"></a>Destek isteği oluşturma

Sorununuz hakkında bilgi toplamak ve çözmenize yardımcı olmak için bazı adımlarda size kılavuzluk ederiz. Her adım aşağıdaki bölümlerde açıklanmıştır.

### <a name="basics"></a>Temel Bilgiler

Destek isteği işleminin ilk adımı, sorununuz ve Destek planınız hakkında temel bilgileri toplar.

**Yeni destek isteğine**Ilişkin **temel bilgiler** sekmesinde, sorunu bize bildirmek için seçicileri kullanın. İlk olarak, sorun türü için bazı genel kategoriler tanımlayacaksınız ve ilgili aboneliği seçmelisiniz. Hizmeti (örneğin, **Windows çalıştıran sanal makine**) seçin. Sanal makinenizin adı gibi kaynağı seçin. Sorunu kendi sözcüklerinizle açıkla, daha fazla bilgi almak için **sorun türünü seçin** .

![Temel Bilgiler dikey penceresi](./media/how-to-create-azure-support-request/basics2lower.png)

> [!NOTE]
> Azure, faturalandırma, kota ayarlamaları ve hesap aktarımları dahil olmak üzere abonelik yönetimi için sınırsız destek sağlar. Teknik destek için bir destek planına ihtiyacınız vardır. [Destek planları hakkında daha fazla bilgi edinin](https://azure.microsoft.com/support/plans).
>
>

### <a name="solutions"></a>Çözümler

Temel bilgiler toplandıktan sonra, bir sonraki adımda deneyebileceğiniz çözümleri göstereceğiz. Bazı durumlarda, bir hızlı tanılama da çalıştırabiliriz. Çözümler Azure mühendisleri tarafından yazılmıştır ve en sık karşılaşılan sorunları çözmeyecektir.

### <a name="details"></a>Ayrıntılar

Daha sonra, sorunla ilgili ek ayrıntıları topladık. Bu adımda kapsamlı ve ayrıntılı bilgiler sağlamak, destek isteğinizi doğru aracıya yönlendirmemize yardımcı olur.

Mümkünse, sorunun ne zaman başlatıldığını ve bunu yeniden oluşturmak için gereken adımları bize söyleyin. Günlük dosyası veya tanılama çıktısı gibi bir dosyayı karşıya yükleyebilirsiniz.

Sorunla ilgili tüm bilgileri aldıktan sonra, nasıl destek alınacağını seçin. **Ayrıntıların** **destek yöntemi** bölümünde, etkinin önem derecesini seçin. Tercih ettiğiniz iletişim yönteminizi, sizinle iletişim kurmak için iyi bir zaman ve destek dilinizi belirtin.

Ardından, sizinle nasıl iletişim kurabileceğinizi bilmemiz için **iletişim bilgileri** bölümünü doldurun.

### <a name="review--create"></a>Gözden geçir + oluştur

Her sekme için gerekli tüm bilgileri doldurun ve ardından **gözden geçir + oluştur**' u seçin. Destek için göndereceğiniz ayrıntıları denetleyin. Gerekirse değişiklik yapmak için herhangi bir sekmeye dönün. Destek isteği tamamlandıktan memnun olduğunuzda **Oluştur**' u seçin.

Bir destek Aracısı belirttiğiniz yöntemi kullanarak sizinle iletişim kuracaktır. İlk yanıt süresi hakkında daha fazla bilgi için bkz. [Destek kapsamı ve yanıt hızı](https://azure.microsoft.com/support/plans/response/).

## <a name="all-support-requests"></a>Tüm destek istekleri

**Tüm destek isteklerini** >  **Yardım + Destek bölümüne** giderek destek isteklerinin ayrıntılarını ve durumunu görüntüleyebilirsiniz.

![Tüm destek istekleri](./media/how-to-create-azure-support-request/allrequestslower.png)

Bu sayfada, destek isteklerini **aboneliğe**, **oluşturma** tarihine (UTC) ve **duruma**göre filtreleyebilirsiniz. Ayrıca, bu sayfadaki destek isteklerini sıralayabilir ve arayabilirsiniz.

Destek aracısının yanıt vermesi için gereken önem ve beklenen süre dahil olmak üzere ayrıntıları görüntülemek için bir destek isteği seçin.

İsteğin önem derecesini değiştirmek istiyorsanız, **iş etkisi**' ni seçin. Atanacak bir dizi önem derecesi listesinden seçim yapın.

> [!NOTE]
> En yüksek önem düzeyi, destek planınıza bağlıdır. [Destek planları hakkında daha fazla bilgi edinin](https://azure.microsoft.com/support/plans).
>
>
Azure 'da kendi kendine yardım destek seçenekleri hakkında daha fazla bilgi edinmek için şu videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/gNhzR5FE9DY]

## <a name="next-steps"></a>Sonraki adımlar

* [Görüşlerinizi ve önerilerinizi bize gönderin](https://feedback.azure.com/forums/266794-support-feedback)
* [Twitter](https://twitter.com/azuresupport) 'da bizimle iletişime katılın
* [MSDN forumlarında](https://social.msdn.microsoft.com/Forums/azure) Meslektaşlarınızdan yardım alın
* [Azure DESTEĞI hakkında](https://azure.microsoft.com/support/faq) daha fazla bilgi
