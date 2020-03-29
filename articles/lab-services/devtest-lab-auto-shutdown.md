---
title: Azure DevTest Labs'da otomatik kapatma ilkelerini yönetme | Microsoft Dokümanlar
description: Sanal makinelerin kullanılmadıklarında otomatik olarak kapatılması için bir laboratuvar için otomatik kapatma ilkesini nasıl ayarladığınızı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2020
ms.author: spelluru
ms.openlocfilehash: a2d0b9bdfba1b96ad42e45d54faf106b2361e29d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76264805"
---
# <a name="configure-autoshutdown-for-lab-and-compute-virtual-machines-in-azure-devtest-labs"></a>Azure DevTest Labs'da laboratuvar için otomatik kapatmayı yapılandırın ve sanal makineleri hesaplatın

Bu makalede, DevTest Labs laboratuvar VM'ler için otomatik kapatma ayarları yapılandırmak ve VMs hesaplamak nasıl açıklanmaktadır. 

## <a name="configure-autoshutdown-for-lab-vms-devtest-labs"></a>Laboratuvar VM'leri (DevTest Labs) için otomatik kapatmayı yapılandırın
Azure DevTest Labs, her laboratuvar için ilkeler (ayarlar) yöneterek laboratuvarlarınızdaki maliyeti kontrol etmenizi ve atıkları en aza indirmenizi sağlar. Bu makalede, bir laboratuvar hesabı için otomatik kapatma ilkesini nasıl yapılandırabileceğinizve laboratuvar hesabındaki bir laboratuvar için otomatik kapatma ayarlarını nasıl yapılandırabileceğiniz gösterilmektedir. Her laboratuvar ilkesini nasıl ayarlayacağınızda görüntülemek için Azure [DevTest Labs'daki laboratuvar ilkelerini tanımla'ya](devtest-lab-set-lab-policy.md)bakın.  

### <a name="set-auto-shut-down-policy-for-a-lab"></a>Laboratuvar için otomatik kapatma ilkesini ayarlama
Laboratuvar sahibi olarak, laboratuarınızdaki tüm VM'ler için bir kapatma zamanlamasını yapılandırabilirsiniz. Bunu yaparak, kullanılmayan (boşta) çalışan makinelerden maliyet kaydedebilirsiniz. Tüm laboratuvar VM'lerinizde bir kapatma ilkesi uygulayabilir ve aynı zamanda laboratuvar kullanıcılarınızı tek tek makineleri için bir zamanlama ayarlamaktan kurtarabilirsiniz. Bu özellik, hiçbir denetim sunmadan tam denetime, laboratuvar kullanıcılarınıza çalışmaktan başlayarak ilkeyi laboratuvar zamanlamanızda ayarlamanızı sağlar. Bir laboratuvar sahibi olarak, aşağıdaki adımları atarak bu ilkeyi yapılandırabilirsiniz:

1. Laboratuvarınızın ana sayfasında Yapılandırma **ve ilkeler'i**seçin.
2. Sol menünün **Zamanlamalar** bölümünde **Otomatik kapatma ilkesini** seçin.
3. Seçeneklerden birini seçin. Aşağıdaki bölümler size bu seçenekler hakkında daha fazla ayrıntı verir: Ayarlanan ilke yalnızca laboratuvarda oluşturulan yeni VM'ler için geçerlidir, zaten var olan VM'ler için geçerli değildir. 

    ![Otomatik kapatma ilkesi seçenekleri](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

### <a name="configure-auto-shutdown-settings"></a>Otomatik kapatma ayarlarını yapılandırma
Otomatik kapatma ilkesi, bu laboratuvarın VM'lerinin kapattığı zamanı belirtmenize izin vererek laboratuvar atıklarını en aza indirmenize yardımcı olur.

Bir laboratuvar ilkelerini görüntülemek (ve değiştirmek) için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Tüm hizmetleri**seçin ve ardından listeden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.   
4. **Yapılandırma ve ilkeleri**seçin.

    ![İlke ayarları bölmesi](./media/devtest-lab-set-lab-policy/policies-menu.png)
5. Laboratuvarın Yapılandırma **ve ilkeler** bölmelerinde, **Zamanlamalar**altında **Otomatik kapatma'yı** seçin.
   
    ![Otomatik kapatma](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
6. Bu ilkeyi etkinleştirmek **için** **On'u** ve devre dışı bırak'ı seçin.
7. Bu ilkeyi etkinleştiriseniz, geçerli laboratuardaki tüm VM'leri kapatmak için saati (ve saat dilimini) belirtin.
8. Belirtilen otomatik kapatma saatinden 30 dakika önce bildirim gönderme seçeneği için **Evet** veya **Hayır** belirtin. **Evet'i**seçerseniz, bildirimin nerede gönderilmesini veya gönderilmesini istediğinizi belirten bir webhook URL bitiş noktası veya e-posta adresi girin. Kullanıcı bildirim alır ve kapatmayı geciktirme seçeneği verilir. Daha fazla bilgi için [Bildirimler](#notifications) bölümüne bakın. 
9. **Kaydet'i**seçin.

    Varsayılan olarak, bir kez etkinleştirildiğinde, bu ilke geçerli laboratuardaki tüm VM'ler için geçerlidir. Bu ayarı belirli bir VM'den kaldırmak için VM'nin yönetim bölmesini açın ve **Otomatik kapatma** ayarını değiştirin.
    
> [!NOTE]
> Geçerli zamanlanan saate 30 dakika içinde laboratuarınızın veya belirli bir laboratuvar sanal makinesinin otomatik kapatma zamanlamasını güncellerseniz, güncelleştirilmiş kapatma süresi bir sonraki günün zamanlamasına doğru geçerli olur. 

### <a name="user-sets-a-schedule-and-can-opt-out"></a>Kullanıcı bir zamanlama ayarlar ve devre dışı kalabilir
Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuar kullanıcıları geçersiz kılınabilir veya laboratuvar zamanlamasını devre dışı bırakabilirsiniz. Bu seçenek, laboratuvar kullanıcılarına VM'lerinin otomatik kapatma zamanlaması üzerinde tam denetim sağlar. Laboratuvar kullanıcıları VM otomatik kapatma zamanlama sayfalarında değişiklik görmezler.

![Otomatik kapatma ilkesi seçeneği - 1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>Kullanıcı bir zamanlama ayarlar ve devre dışı kaldıramaz
Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuvar kullanıcıları laboratuvar zamanlamasını geçersiz kılabilir. Ancak, otomatik kapatma ilkesini devre dışı kaldırabilirsiniz. Bu seçenek, laboratuarınızdaki her makinenin otomatik kapatma zamanlaması altında olmasını sağlar. Laboratuvar kullanıcıları, VM'lerinin otomatik kapatma zamanlamasını güncelleyebilir ve kapatma bildirimlerini ayarlayabilir.

![Otomatik kapatma ilkesi seçeneği - 2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>Kullanıcı nın laboratuvar yöneticisi tarafından ayarlanan zamanlama üzerinde hiçbir denetimi yoktur
Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuar kullanıcıları geçersiz kalamaz veya laboratuvar zamanlamasını devre dışı bırakamaz. Bu seçenek, laboratuvar yöneticisine laboratuardaki her makine için zamanlamada tam denetim sunar. Laboratuvar kullanıcıları yalnızca VM'leri için otomatik kapatma bildirimleri ayarlayabilir.

![Otomatik kapatma ilkesi seçeneği - 3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

### <a name="notifications"></a>Bildirimler
Laboratuvar sahibi tarafından otomatik kapatma ayarlandıktan sonra, vm'lerinden herhangi biri etkilenecekse otomatik kapatma tetiklenmeden 30 dakika önce bildirimler laboratuvar kullanıcılarına gönderilir. Bu seçenek, laboratuvar kullanıcılarına kapatmadan önce çalışmalarını kaydetme şansı verir. Bildirim ayrıca aşağıdaki eylemler için her VM için bağlantılar sağlar:

- Bu süre için otomatik kapatmayı atla
- VM üzerinde çalışmaya devam edebilmeleri için otomatik kapatmayı bir saat veya 2 saat erteleyebilirsiniz.

Bildirim, otomatik kapatma ayarlarında laboratuvar sahipleri tarafından belirtilen yapılandırılmış web kancası bitiş noktası veya e-posta adresi üzerinden gönderilir. Webhooks, belirli olaylara abone olan tümleştirmeler oluşturmanıza veya ayarlamanıza olanak sağlar. Bu olaylardan biri tetiklendiğinde, DevTest Labs webhook'un yapılandırılmış URL'sine bir HTTP POST yükü gönderir. Web hooks hakkında daha fazla bilgi için [bkz.](../azure-functions/functions-create-a-web-hook-or-api-function.md) 

Çeşitli uygulamalar (örneğin, Slack, Azure Logic Apps vb.) tarafından kapsamlı olarak desteklendikleri ve bildirim göndermek için kendi yolunuzu uygulamanıza olanak sağladığı ndan web kancalarını kullanmanızı öneririz. Örnek olarak, bu makale, Azure Logic Apps'ı kullanarak e-postalardan otomatik kapatma bildirimi alma konusunda size yol sunar. Öncelikle, laboratuarınızda otomatik kapatma bildirimini etkinleştirmek için temel adımları hızla gözden geçirelim.   

### <a name="create-a-logic-app-that-receives-email-notifications"></a>E-posta bildirimleri alan bir mantık uygulaması oluşturma
[Azure Logic Apps,](../logic-apps/logic-apps-overview.md) bir hizmeti Office 365 ve twitter gibi diğer istemcilerle tümleştirmeyi kolaylaştıran birçok hazır bağlayıcı sağlar. Yüksek düzeyde, e-posta bildirimi için bir Mantık Uygulaması kurma adımları dört aşamaya ayrılabilir: 

- Bir mantık uygulaması oluşturun. 
- Yerleşik şablonu yapılandırın.
- E-posta istemcinizle tümleştirme
- Webhook URL'sini alın.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
Başlamak için aşağıdaki adımları kullanarak Azure aboneliğinizde bir mantık uygulaması oluşturun:

1. Sol menüde **+ Kaynak Oluştur'u** seçin, **Tümleştirme'yi**seçin ve **Mantık Uygulaması'nı**seçin. 

    ![Yeni mantık uygulaması menüsü](./media/devtest-lab-auto-shutdown/new-logic-app.png)
2. Mantık **Uygulamasında - Oluştur** sayfasında aşağıdaki adımları izleyin: 
    1. Mantık uygulaması için bir **ad** girin.
    2. Azure **aboneliğinizi**seçin.
    3. Yeni bir **kaynak grubu** oluşturun veya varolan bir kaynak grubu seçin. 
    4. Mantık uygulaması için bir **konum** seçin. 

        ![Yeni mantık uygulaması - ayarlar](./media/devtest-lab-auto-shutdown/new-logic-app-page.png)
3. **Bildirimler'de,** bildirimdeki **kaynağa Git'i** seçin. 

    ![Kaynağa git](./media/devtest-lab-auto-shutdown/go-to-resource.png)
4. **Dağıtım Araçları** kategorisi altında Logic **uygulama tasarımcısını** seçin.

    ![HTTP İstek/Yanıt'ı seçin](./media/devtest-lab-auto-shutdown/select-http-request-response-option.png)
5. HTTP **İstek-Yanıt** sayfasında **bu şablonu kullan'ı**seçin. 

    ![Bu şablonseçeneğini kullan'ı seçin](./media/devtest-lab-auto-shutdown/select-use-this-template.png)
6. **İstek Gövdesi JSON Schema** bölümüne aşağıdaki JSON'u kopyalayın: 

    ```json
    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "properties": {
            "delayUrl120": {
                "type": "string"
            },
            "delayUrl60": {
                "type": "string"
            },
            "eventType": {
                "type": "string"
            },
            "guid": {
                "type": "string"
            },
            "labName": {
                "type": "string"
            },
            "owner": {
                "type": "string"
            },
            "resourceGroupName": {
                "type": "string"
            },
            "skipUrl": {
                "type": "string"
            },
            "subscriptionId": {
                "type": "string"
            },
            "text": {
                "type": "string"
            },
            "vmName": {
                "type": "string"
            }
        },
        "required": [
            "skipUrl",
            "delayUrl60",
            "delayUrl120",
            "vmName",
            "guid",
            "owner",
            "eventType",
            "text",
            "subscriptionId",
            "resourceGroupName",
            "labName"
        ],
        "type": "object"
    }
    ```
    
    ![İstek Vücut JSON Schema](./media/devtest-lab-auto-shutdown/request-json.png)
7. Tasarımcıda **+ Yeni adım** seçin ve aşağıdaki adımları izleyin:
    1. Office **365 Outlook'u ara - E-posta gönderin.** 
    2. **Eylemlerden** **e-posta gönder'i** seçin. 
    
        ![E-posta gönder seçeneği](./media/devtest-lab-auto-shutdown/select-send-email.png)
    3. E-posta hesabınızda oturum açabilmek için **Oturum Aç'ı** seçin. 
    4. **TO** alanını seçin ve sahibini seçin.
    5. **SUBJECT'i**seçin ve e-posta bildiriminin konusunu girdi. Örneğin: "Lab için makine vmName kapatma: labName."
    6. **BODY'yi**seçin ve e-posta bildirimi için gövde içeriğini tanımlayın. Örneğin: "vmName 15 dakika içinde kapanacak şekilde zamanlanır. Bu kapatmayı tıklayarak atlayın: URL. Bir saat gecikme kapatma: delayUrl60. 2 saat gecikme kapatma: delayUrl120."

        ![İstek Vücut JSON Schema](./media/devtest-lab-auto-shutdown/email-options.png)
1. Araç çubuğunda **Kaydet**’i seçin. Şimdi, **HTTP POST URL**kopyalayabilirsiniz. URL'yi panoya kopyalamak için kopyalama düğmesini seçin. 

    ![WebHook URL](./media/devtest-lab-auto-shutdown/webhook-url.png)

## <a name="configure-autoshutdown-for-compute-vms"></a>Bilgi işlem VM'leri için otomatik kapatmayı yapılandırma

1. Sanal **makine** sayfasında, sol menüde **Otomatik kapatma'yı** seçin. 
2. Otomatik **kapatma** sayfasında, bu ilkeyi etkinleştirmek **için** **On'u** ve devre dışı bırakma'yı seçin.
3. Bu ilkeyi etkinleştiriseniz, VM'nin kapatılması gereken **saati** (ve **saat dilimini)** belirtin.
4. Belirtilen otomatik kapatma saatinden 30 dakika önce bildirim gönderme seçeneği için **Evet** veya **Hayır** belirtin. **Evet'i**seçerseniz, bildirimin nerede gönderilmesini veya gönderilmesini istediğinizi belirten bir webhook URL bitiş noktası veya e-posta adresi girin. Kullanıcı bildirim alır ve kapatmayı geciktirme seçeneği verilir. Daha fazla bilgi için [Bildirimler](#notifications) bölümüne bakın. 
9. **Kaydet'i**seçin.

    ![İşlem VM için otomatik kapatmayı yapılandırma](./media/devtest-lab-auto-shutdown/comnpute-auto-shutdown.png)

## <a name="next-steps"></a>Sonraki adımlar
Tüm ilkeleri nasıl ayarlayacağınızı öğrenmek için Azure [DevTest Labs'da laboratuvar ilkelerini](devtest-lab-set-lab-policy.md)tanımlayın'a bakın.

