---
title: Bilinen sorunlar-Azure dijital TWINS
description: Azure dijital TWINS ile ilgili bilinen sorunları algılamayı ve azaltıcı yardım alın.
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.service: digital-twins
ms.date: 07/14/2020
ms.openlocfilehash: 0c008061d2d4fafa96eda934d5026c92839a0bdb
ms.sourcegitcommit: d18a59b2efff67934650f6ad3a2e1fe9f8269f21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88661495"
---
# <a name="known-issues-in-azure-digital-twins"></a>Azure dijital TWINS 'de bilinen sorunlar

Bu makalede, Azure dijital TWINS ile ilişkili bilinen sorunlar hakkında bilgi sağlanır.

## <a name="400-client-error-bad-request-in-cloud-shell"></a>"400 istemci hatası: Cloud Shell içinde bozuk Istek"

Cloud Shell Komutları zaman zaman "400 Istemci hatası: URL için hatalı Istek: http://localhost:50342/oauth2/token " ve ardından tam yığın izleme tarafından hata vererek başarısız olabilir.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

Bu, komutu yeniden çalıştırarak `az login` ve sonraki oturum açma adımları tamamlanırken çözülebilir.

Bundan sonra, komutunu yeniden çalıştırabilmelisiniz.

### <a name="possible-causes"></a>Olası nedenler

Bu Cloud Shell, bilinen bir sorunun sonucudur: [*Cloud Shell Token alma işlemi, 400 Istemci hatasıyla başarısız oluyor: Hatalı istek*](https://github.com/Azure/azure-cli/issues/11749).

## <a name="missing-role-assignment-after-scripted-setup"></a>Komut dosyası kurulumundan sonra eksik rol ataması

Bazı kullanıcılar [*nasıl yapılır: bir örnek ve kimlik doğrulaması (komut dosyası) ayarlama*](how-to-set-up-instance-scripted.md)rol atama bölümüyle ilgili sorunlar yaşayabilir. Betik hata göstermez, ancak *Azure dijital TWINS sahibi (Önizleme)* rolü kullanıcıya başarıyla atanmaz ve bu, yolda diğer kaynakları oluşturma yeteneğini etkiler.

Rol atamalarınızın betiği çalıştırdıktan sonra başarıyla ayarlandığını anlamak için, kurulum makalesinin [*Kullanıcı rolü atamasını doğrula*](how-to-set-up-instance-scripted.md#verify-user-role-assignment) bölümüne ait yönergeleri izleyin. Kullanıcılarınız bu rolle gösterilmezse, bu sorun sizi etkiler.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

Çözümlemek için, CLı veya Azure portal kullanarak rol atamasını el ile ayarlayabilirsiniz. 

Aşağıdaki yönergeleri izleyin:
* [CLI](how-to-set-up-instance-cli.md#set-up-user-access-permissions)
* [Portal@@](how-to-set-up-instance-portal.md#set-up-user-access-permissions)

### <a name="possible-causes"></a>Olası nedenler

Kişisel [Microsoft hesabı (MSA)](https://account.microsoft.com/account)ile oturum açan kullanıcılar için, kullanıcının oturum açma e-postalarından sizin gibi komutları TANıMLAYAN asıl kimliği, kullanıcının oturum açma e-postaınızdan farklı olabilir, bu da komut dosyasının rolü doğru şekilde atamak için bulmasını ve kullanmasını zorlaştırır.

## <a name="issue-with-interactive-browser-authentication"></a>Etkileşimli tarayıcı kimlik doğrulamasıyla ilgili sorun

Azure ** [. Identity](https://docs.microsoft.com/dotnet/api/azure.identity?view=azure-dotnet) kitaplığı**'nın en son sürümünü (sürüm **1.2.0**) kullanarak Azure dijital TWINS uygulamalarınıza kimlik doğrulama kodu yazarken [ınteractivebrowsercredential](https://docs.microsoft.com/dotnet/api/azure.identity.interactivebrowsercredential?view=azure-dotnet) yöntemiyle ilgili sorunlarla karşılaşabilirsiniz.

Etkilenen yöntem aşağıdaki makalelerde kullanılır: 
* [*Öğretici: istemci uygulamasını kodlayın*](tutorial-code.md)
* [*Nasıl yapılır: uygulama kimlik doğrulama kodunu yazma*](how-to-authenticate-client.md)
* [*Nasıl yapılır: Azure dijital TWINS API 'Leri ve SDK 'Larını kullanma*](how-to-use-apis-sdks.md)

Sorun, bir tarayıcı penceresinde kimlik doğrulamaya çalışırken "Azure. Identity. AuthenticationFailedException" hata yanıtını içerir. Tarayıcı penceresi tamamen başlayamayabilir veya kullanıcının kimlik doğrulamasını başarıyla başarabilirken, istemci uygulaması yine de hata vererek başarısız olabilir.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

Bu sorunu çözmek için, uygulamalarınızın Azure. Identity sürüm **1.1.1**'i açık bir şekilde kullanmasını sağlayabilirsiniz. Kitaplığın bu sürümüyle, tarayıcının beklendiği gibi yüklemesi ve kimlik doğrulaması gerekir.

>[!NOTE]
> Hala en son **1.2.0**için varsayılan değer olacak şekilde, herhangi bir sürüm belirtmeden kitaplığı eklemek yeterli değildir. Sürüm **1.1.1** öğesini açıkça belirtmeniz gerekir.

### <a name="possible-causes"></a>Olası nedenler

Bu, Azure. Identity Library 'nin (sürüm **1.2.0**) en son sürümündeki bir açık sorun ile Ilgilidir: [*ınteractivebrowsercredential kullanılırken kimlik doğrulaması başarısız olur*](https://github.com/Azure/azure-sdk-for-net/issues/13940).

Bu sorunu, Azure dijital TWINS uygulamanızda sürüm **1.2.0** kullanıyorsanız veya bir sürüm belirtmeden kitaplığı projenize eklerseniz (Bu en son sürümü varsayılan olarak da budur) görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS üzerinde güvenlik ve izinler hakkında daha fazla bilgi edinin:
* [*Kavramlar: Azure dijital TWINS çözümleri için güvenlik*](concepts-security.md)