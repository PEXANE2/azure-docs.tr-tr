---
title: Uzaktan İzleme içinde e-posta eylemi - Azure | Microsoft Dokümanlar
description: Bu nasıl yapılulu kılavuzu, yeni veya varolan bir kurala nasıl bir e-posta eylemi ekleyeceğinizi gösterir.
author: dominicbetts
manager: hegate
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/12/2018
ms.topic: conceptual
ms.openlocfilehash: c192ba73da0cfaf1832b6a1e572bd71b250a976b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74168373"
---
# <a name="add-an-email-action"></a>E-posta eylemi ekleme

E-posta eylemleri uyarıları asla kaçırmadığınızdan emin olun. Varolan bir kurala veya yeni bir kural oluşturduğunuzda bir e-posta eylemi ekleyebilirsiniz.

Bu nasıl yap'ı bulma kılavuzundaki adımları tamamlamak için, Azure aboneliğinizde Uzaktan İzleme çözüm hızlandırıcısının dağıtılmış bir örneğine ihtiyacınız var.

Bir kural oluşturmak veya değiştirmek için Yönetici veya [ **Administrator**doğru izinlere sahip](iot-accelerators-remote-monitoring-rbac.md)olmalısınız.

## <a name="edit-an-existing-rule"></a>Var olan kuralı düzenleme

Varolan bir kurala e-posta eylemi eklemek için aşağıdaki adımları izleyin:

1. Uzaktan İzleme çözümünüze gidin.

1. **Pano'dan** **Kurallar** sayfasına gidin:

    ![Rules (Kurallar) Sayfası](./media/iot-accelerators-remote-monitoring-email-actions/rules-email.png)

1. Değiştirmek için varolan kuralın yanındaki onay kutusunu tıklatın ve sonra en üstte **Edit'i** tıklatın. Bir editable **Kural** paneli görüntülenir.

1. **Eylem** bölümünde, **E-postayı** **anızda**geçiş özelliği etkinleştirildi.

1. Çözüm hızlandırıcısında ilk kez bir e-posta eylemini etkinleştirdiğinizde [Outlook'ta oturum açmanız](#outlook)gerekir.

1. Alıcı kutusuna bir e-posta adresi girin ve eklemek için her e-posta adresi için **Enter** tuşuna basın:

    ![Adres girişi](./media/iot-accelerators-remote-monitoring-email-actions/address-email.png)

1. E-posta için bir konu girin.

1. E-posta alıcıları için herhangi bir ek notu düz metin olarak girin. [E-posta şablonu'nu difyenir,](#htmledit)HTML biçimlendirmesini kullanabilirsiniz.

1. **Kural Durumunun** **Etkin**olarak ayarlandıklarına emin olun.

1. **Uygula**’ya tıklayın.

## <a name="create-a-new-rule"></a>Yeni kural oluşturma

Yeni bir kural oluşturduğunuzda bir e-posta eylemi eklemek için aşağıdaki adımları izleyin:

1. Uzaktan İzleme çözümünüze gidin.

1. **Pano'dan** **Kurallar** sayfasına gidin:

    ![Rules (Kurallar) Sayfası](./media/iot-accelerators-remote-monitoring-email-actions/rules-email.png)

1. [Kural oluşturma bölümündeki](iot-accelerators-remote-monitoring-automate.md#create-a-rule)adımları izleyin. Önem **Düzeyi**ayarladığınız noktaya kadar gelişmiş bir kural bölümü [oluşturmaadımlarını](iot-accelerators-remote-monitoring-automate.md#create-an-advanced-rule) izleyin. Henüz **Uygula'yı** tıklatmayın.

1. **Eylem** bölümünde, **E-postayı** **anızda**geçiş özelliği etkinleştirildi.

1. Çözüm hızlandırıcısında ilk kez bir e-posta eylemini etkinleştirdiğinizde [Outlook'ta oturum açmanız](#outlook)gerekir.

1. Alıcı kutusuna bir e-posta adresi girin ve eklemek için her e-posta adresi için **Enter** tuşuna basın:

    ![Adres girişi](./media/iot-accelerators-remote-monitoring-email-actions/address-email.png)

1. E-posta için bir konu girin.

1. E-posta alıcıları için herhangi bir ek notu düz metin olarak girin. [E-posta şablonu'nu difyenir,](#htmledit)HTML biçimlendirmesini kullanabilirsiniz.

1. **Kural Durumunun** **Etkin**olarak ayarlandıklarına emin olun.

1. **Uygula**’ya tıklayın.

E-posta eylemi kuralınız artık etkinleştirildi. Eylem her tetikleninher zaman alıcılara yeni bir e-posta gönderilir.

## <a name="sign-in-to-outlook"></a>Outlook'ta oturum açma<a name="outlook"></a>

Çözüm hızlandırıcınızda ilk e-posta eylemini etkinleştirdiğinizde Outlook'ta oturum açmanız gerekir. Bu eylem, e-posta bildirimlerini gönderen e-posta hesabını ayarlar.

> [!NOTE]
> Yalnızca çözüm hızlandırıcı bildirimleri için belirli bir Outlook hesabı oluşturmalı ve ilk e-posta eyleminizi etkinleştirdiğinizde bu hesabı kullanmalısınız.

### <a name="contributor-role-outlook-setup"></a>Katılımcı rolü Outlook kurulumu

Abonelikte **katılımcı** rolündeki bir kişi çözüm hızlandırıcısını dağıttıysa, uygulama web kullanıcı arabirimi aracılığıyla e-posta eylemlerini ayarlamak ve doğrulamak için yeterli izine sahip değildir.

Başlamadan önce, çözüm hızlandırıcınızdan e-posta bildirimleri göndermek için kullanmak üzere bir Outlook hesabı oluşturun.

Aşağıdaki adımlar, e-posta eylemlerini el ile nasıl ayarlayıp doğrulayacağınızda size gösteriş ve

1. [Azure portalına](https://portal.azure.com) gidin.

1. Çözüm hızlandırıcınız için Kaynak Grubuna gidin.

1. **Office365-bağlayıcısını**tıklatın:

    ![API Bağlantısı](./media/iot-accelerators-remote-monitoring-email-actions/apiconnector1.png)

1. Yetkilendirme işlemini başlatmak için banner'ı tıklatın:

    ![Yetkilendir](./media/iot-accelerators-remote-monitoring-email-actions/connector1.png)

1. **Yetkilendir'i**tıklatın. Oturum açmanız istendi. Oturum açmada kullandığınız hesap, uygulamanın e-posta bildirimleri göndermek için kullandığı e-posta adresi olmalıdır:

    ![Yetkilendirme düğmesi](./media/iot-accelerators-remote-monitoring-email-actions/authorize.png)

1. En altta **Kaydet'i** tıklatın. Banner gitmişse yetkiniz başarılı olacaktır.

1. Bildirimlerin gönderildiği e-posta adresini değiştirmek için **API bağlantısını edit'i**tıklatın.

    ![e-postayı değiştirme](./media/iot-accelerators-remote-monitoring-email-actions/editemail1.png)

### <a name="owner-role-outlook-setup"></a>Sahibi rol Outlook kurulumu

Abonelikteki **sahip** rolündeki biri çözüm hızlandırıcısını dağıttıysa, uygulama e-posta eylemlerinin Web Kullanıcı Arabirimi aracılığıyla doğru şekilde ayarlandığını doğrulayabilir.

Başlamadan önce, çözüm hızlandırıcınızdan e-posta bildirimleri göndermek için kullanmak üzere bir Outlook hesabı oluşturun.

Aşağıdaki adımlar oturum açmanıza ve e-posta eylemleri ayarlamanıza yardımcı olur:

1. Outlook'ta oturum açabilmek için tıklatın. Azure portalına götürülür:

   ![Outlook'ta oturum açma](./media/iot-accelerators-remote-monitoring-email-actions/owneroutlook-email.png)

1. **Yetkilendir'i**tıklatın. Oturum açmanız istendi. Oturum açmada kullandığınız hesap, uygulamanın e-posta bildirimleri göndermek için kullandığı e-posta adresi olmalıdır:

1. **Kaydet**'e tıklayın. Çözüm hızlandırıcınıza dönün ve sayfayı yenileyin.

1. E-posta bildirimini başarıyla yapılandırıldıysanız, şu iletiyi görürsünüz:

   ![Başarılı Outlook oturum açma](./media/iot-accelerators-remote-monitoring-email-actions/success-email.png)

## <a name="customize-the-email-html"></a>E-posta HTML özelleştirme<a name="htmledit"></a>

Out-of-the-box, Uzaktan İzleme çözüm hızlandırıcı eylem e-postalar için temel bir HTML şablonu sağlar. E-posta şablonu, e-posta eylem ayarlarından gelen değerleri kullanır. İşte örnek bir e-posta:

![e-posta örneği](./media/iot-accelerators-remote-monitoring-email-actions/emailtemplate1.png)

Aşağıdaki adımlar, HTML e-posta şablonu nasıl düzenlenecek gösteriş. Örneğin, daha fazla bilgi ekleyebilir veya özel resimler ekleyebilirsiniz:

1. Java veya .NET Uzaktan İzleme GitHub deposunu klonla:

1. E-posta şablonu konumuna gidin:
  
    `Dotnet: device-telemetry\ActionsAgent\data\EmailTemplate.html`
  
    `Java device-telemetry/app/resources/data/EmailTemplate.html`

1. İletiyi özelleştirmek için bu şablona herhangi bir parametre ekleyebilir veya kaldırabilirsiniz. Gerektiğinde çağrıları ekleyebilir, kaldırabilir veya değiştirebilirsiniz:

    Örneğin, .NET kodunda:`emailTemplate = emailTemplate.Replace("${subject}", emailAction.GetSubject());`

    Örneğin, Java kodunda:`this.emailTemplate.replace("${subject}", emailAction.GetSubject());`

1. Şablondaki parametreler `${...}`. Bir parametreyi silmek için gerekli satırı silin. Parametre eklemek için, eklemek için değer içeren bir satır ekleyin.

1. Resim veya özel metin eklemek için EmailTemplate.HTML dosyasını doğrudan güncelleyin.

## <a name="throttling"></a>Azaltma

Uzaktan İzleme çözüm hızlandırıcısı, e-posta bildirimleri göndermek için Outlook'u kullanır. Outlook, [1 dakikada 30 e-postaya gönderilen e-postaların](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#receiving-and-sending-limits)sayısını sınırlar. E-postaları alan e-posta istemcileri de dakikada alınan e-posta ların sayısını azaltabilir. Sınırlamalar konusunda özel e-posta istemcinize danışın. Bir kural için e-posta bildirimi ayarladığınızda, kural en az bir dakikalık bir süre içinde ortalama değerleri hesaplamalı ve anlık değerleri kullanmamalıdır:

![Ortalama hesaplama](./media/iot-accelerators-remote-monitoring-email-actions/calculation-email.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuz, uzaktan izleme çözümü içinde yeni veya varolan bir kurala nasıl e-posta eylemi ekleyeceğinizgösteriş. Kılavuz ayrıca size ve ileti biçimini tanımlayan HTML'yi nasıl deddiğinizi de gösterdi.

Önerilen bir sonraki [adım, uyarıları nasıl kullanacağınızı ve aygıt sorunlarını nasıl düzelteceklerini](iot-accelerators-remote-monitoring-maintain.md)öğrenmektir.
