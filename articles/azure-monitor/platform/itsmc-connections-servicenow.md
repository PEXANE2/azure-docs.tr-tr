---
title: ServiceNow 'ı BT Hizmet Yönetimi Bağlayıcısı bağlama
description: ITSM iş öğelerini merkezi olarak izlemek ve yönetmek için Azure Izleyici 'de ServiceNow 'ı BT Hizmet Yönetimi Bağlayıcısı (ITSMC) ile bağlamayı öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 221678da819d32539d0078b72a87c61851ed3e8f
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99089267"
---
# <a name="connect-servicenow-with-it-service-management-connector"></a>ServiceNow 'ı BT Hizmet Yönetimi Bağlayıcısı bağlama

Bu makalede, BT hizmet yönetimi (ıTSM) iş öğelerinizi merkezi olarak yönetebilmeniz için Log Analytics bir ServiceNow örneği ve BT Hizmet Yönetimi Bağlayıcısı (ITSMC) arasındaki bağlantıyı nasıl yapılandıracağınız gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar
Bağlantı için aşağıdaki önkoşulları karşıladığınızdan emin olun.

### <a name="itsmc-installation"></a>ISMC yüklemesi

ISMC yükleme hakkında bilgi için bkz. [BT hizmet yönetimi Bağlayıcısı çözümü ekleme](./itsmc-definition.md#add-it-service-management-connector).

> [!NOTE]
> ISMC yalnızca ServiceNow 'dan hizmet olarak yazılım (SaaS) sunumunu destekler. ServiceNow 'ın özel dağıtımları desteklenmez.

### <a name="oauth-setup"></a>OAuth kurulumu

ServiceNow desteklenen sürümler Orlando, New York, Madrid, Londra, Kingston, Jakarta, Istanbul, Helsinki ve Genfiliz içerir.

ServiceNow yöneticileri, ServiceNow örneği için bir istemci KIMLIĞI ve istemci parolası üretmelidir. Aşağıdaki bilgileri gerekli şekilde görüntüleyin:

- [Orlando için OAuth ayarla](https://docs.servicenow.com/bundle/orlando-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
- [New York için OAuth ayarlama](https://docs.servicenow.com/bundle/newyork-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
- [Madrid için OAuth ayarlama](https://docs.servicenow.com/bundle/madrid-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
- [Londra için OAuth ayarlama](https://docs.servicenow.com/bundle/london-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
- [Kingston için OAuth ayarla](https://docs.servicenow.com/bundle/kingston-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
- [Cakarta için OAuth ayarlama](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
- [Istanbul için OAuth ayarlama](https://docs.servicenow.com/bundle/istanbul-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
- [Helsinki için OAuth ayarlama](https://docs.servicenow.com/bundle/helsinki-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
- [Genfiliz için OAuth ayarlama](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/security/task/t_SettingUpOAuth.html)

OAuth ayarlamanın bir parçası olarak şunları öneririz:

1. [İstemcilere örneğe erişmesi için bir uç nokta oluşturun](https://docs.servicenow.com/bundle/newyork-platform-administration/page/administer/security/task/t_CreateEndpointforExternalClients.html).

1. Yenileme belirtecinin kullanım süresini güncelleştirin:

   1. **ServiceNow** bölmesinde, **sistem OAuth**' ı arayın ve ardından **uygulama kayıt defteri**' ni seçin. 
   1. Tanımlanan OAuth adı ' nı seçin ve **yenileme belirtecini** kullanım süresini **7.776.000 saniye** (90 gün) olarak değiştirin. 
   1. **Güncelleştir**’i seçin. 

1. Bağlantının etkin kalmasını sağlamak için bir iç yordam oluşturun. Yenileme belirtecinin beklenen süre sonundan bir kaç gün önce, şu işlemleri gerçekleştirin:

   1. [ITSM Bağlayıcısı yapılandırması için el ile eşitleme Işlemini tamamlar](./itsmc-resync-servicenow.md).

   1. Eski yenileme belirtecine iptal edin. Güvenlik nedenleriyle eski anahtarların saklanması önerilmez. 
   
      1. **ServiceNow** bölmesinde **sistem OAuth**' ı arayın ve ardından **belirteçleri Yönet**' i seçin. 
      
      1. OAuth adı ve sona erme tarihine göre listeden eski belirteci seçin.

         ![OAuth için belirteçlerin listesini gösteren ekran görüntüsü.](media/itsmc-connections/snow-system-oauth.png)
      1. **Erişimi** iptal etmeyi iptal et ' i seçin  >  .

## <a name="install-the-user-app-and-create-the-user-role"></a>Kullanıcı uygulamasını yükler ve Kullanıcı rolünü oluşturma

Hizmeti şimdi Kullanıcı uygulamasını yüklemek ve tümleştirme Kullanıcı rolünü oluşturmak için aşağıdaki yordamı kullanın. Azure 'da ServiceNow bağlantısı kurmak için bu kimlik bilgilerini kullanacaksınız.

> [!NOTE]
> ISMC, yalnızca ServiceNow mağazasından indirilen Microsoft Log Analytics tümleştirmesi için resmi Kullanıcı uygulamasını destekler. ISMC, ServiceNow tarafında veya resmi ServiceNow çözümüne dahil olmayan herhangi bir uygulamada herhangi bir kod alımı desteklemez. 

1. ServiceNow örneğinizin ServiceNow **ve MICROSOFT OMS tümleştirmesi Için** [ServiceNow Store](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1) ve Install User App ' i ziyaret edin.
   
   >[!NOTE]
   >Microsoft Operations Management Suite (OMS) ile Azure Izleyici arasında devam eden geçişin bir parçası olarak, OMS artık Log Analytics olarak adlandırılır.     
2. Yüklemeden sonra ServiceNow örneğinin sol gezinti çubuğuna gidin ve sonra **MICROSOFT OMS tümleştirici**' i arayıp seçin.  
3. **Yükleme denetim listesini** seçin.

   Kullanıcı rolü henüz oluşturulmadığından durum,  **tamamlanmamış** olarak görüntülenir.

4. **Tümleştirme kullanıcısı oluştur**' un yanındaki metin kutusunda, Azure 'DA ısmc 'a bağlanabilecek kullanıcının adını girin.
5. Bu kullanıcının parolasını girip **Tamam**' ı seçin.  

Yeni oluşturulan kullanıcı varsayılan rollerle birlikte görüntülenir:

- personalize_choices
- import_transformer
- x_mioms_microsoft. Kullanıcı
- ITIL
- template_editor
- view_changer

Kullanıcıyı başarıyla oluşturduktan sonra, **yükleme denetim listesinin** durumu **tamamlandı** olarak getirilir ve uygulama için oluşturulan kullanıcı rolünün ayrıntılarını listeler.

> [!NOTE]
> ISMC, ServiceNow Örneğinizde yüklü başka bir modül olmadan ServiceNow 'a olay gönderebilir. ServiceNow Örneğinizde EventManagement modülünü kullanıyorsanız ve bağlayıcıyı kullanarak ServiceNow 'da olay veya uyarı oluşturmak istiyorsanız, tümleştirme kullanıcısına aşağıdaki rolleri ekleyin:
> 
> - evt_mgmt_integration
> - evt_mgmt_operator  

## <a name="create-a-connection"></a>Bağlantı oluşturma
Bir ServiceNow bağlantısı oluşturmak için aşağıdaki yordamı kullanın.

> [!NOTE]
> Azure Izleyici 'den gönderilen uyarılar ServiceNow: olaylar, olaylar veya uyarılarda aşağıdaki öğelerden birini oluşturabilir. 

1. Azure portal, **tüm kaynaklar** ' a gidin ve **ServiceDesk (yourçalışmaalanıadı)** öğesini arayın.

2. **Çalışma alanı veri kaynakları** altında **ITSM bağlantıları**' nı seçin.

   ![Bir veri kaynağının seçimini gösteren ekran görüntüsü.](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin üst kısmında **Ekle**' yi seçin.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri girip **Tamam**' ı seçin.

   | **Alan** | **Açıklama** |
   | --- | --- |
   | **Bağlantı adı**   | ISMC ile bağlanmak istediğiniz ServiceNow örneği için bir ad girin. Bu adı daha sonra, ıTSM iş öğelerini yapılandırdığınızda ve ayrıntılı analizi görüntülerken Log Analytics ' de kullanırsınız. |
   | **İş ortağı türü**   | **ServiceNow**' ı seçin. |
   | **Sunucu URL'si**   | ISMC 'a bağlamak istediğiniz ServiceNow örneğinin URL 'sini girin. URL, suffix *. ServiceNow.com* ile desteklenen bir SaaS sürümüne işaret etmelidir (örneğin, https://XXXXX.service-now.com/) .|
   | **Kullanıcı adı**   | ISMC bağlantısını desteklemek için ServiceNow uygulamasında oluşturduğunuz tümleştirme Kullanıcı adı ' nı girin.|
   | **Parola**   | Bu kullanıcı adıyla ilişkili parolayı girin. **Note**: Kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır. Bunlar, ıSMC hizmetinin içinde herhangi bir yerde depolanmaz.  |
   | **İstemci kimliği**   | Daha önce oluşturduğunuz OAuth2 kimlik doğrulaması için kullanmak istediğiniz istemci KIMLIĞINI girin. İstemci KIMLIĞI ve gizli anahtar oluşturma hakkında daha fazla bilgi için bkz. [OAuth ayarlama](https://old.wiki/index.php/OAuth_Setup). |
   | **İstemci parolası**   | Bu KIMLIK için oluşturulan istemci gizli anahtarını girin.   |
   | **Veri eşitleme kapsamı (gün)** | Verilerin içinden istediğiniz geçmiş gün sayısını girin. Sınır 120 gündür. |
   | **Eşitlenecek çalışma öğeleri**   | ISMC aracılığıyla Azure Log Analytics eşitlemek istediğiniz ServiceNow iş öğelerini seçin. Seçilen değerler Log Analytics içine aktarılır. Seçenekler olaylar ve değişiklik isteklerdir.|
   | **ITSM ürününde yeni yapılandırma öğesi oluştur** | ITSM ürününde yapılandırma öğelerini oluşturmak istiyorsanız bu seçeneği belirleyin. Seçildiğinde, ıSMC desteklenen ITSM sisteminde yapılandırma öğeleri (yoksa, yoksa) oluşturur. Varsayılan olarak devre dışıdır. |

![ServiceNow bağlantısı eklemek için kutuların ve seçeneklerin ekran görüntüsü.](media/itsmc-connections/itsm-connection-servicenow-connection-latest.png)

Başarılı bir şekilde bağlanıp eşitlendikten sonra:

- ServiceNow örneğinden seçilen iş öğeleri Log Analytics içine aktarılır. **BT hizmet yönetimi Bağlayıcısı** kutucuğunda bu iş öğelerinin özetini görüntüleyebilirsiniz.

- Log Analytics uyarılarından veya günlük kayıtlarından veya bu ServiceNow örneğindeki Azure uyarılarından olaylar oluşturabilirsiniz.

> [!NOTE]
> ServiceNow, saat başına istekler için bir hız sınırına sahiptir. Limiti yapılandırmak için ServiceNow örneğinde **gelen REST API hız sınırını** tanımlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [ITSM Bağlayıcısı genel bakış](itsmc-overview.md)
* [Azure uyarılarından ıTSM iş öğeleri oluşturma](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [ITSM Bağlayıcısı sorunlarını giderme](./itsmc-resync-servicenow.md)