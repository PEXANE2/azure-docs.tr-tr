---
title: Karma Azure AD join'in kontrollü doğrulaması - Azure AD
description: Tüm kuruluş genelinde tek seferde etkinleştirmeden önce karma Azure AD birleştirmenin kontrollü doğrulamasını nasıl yapacağınızı öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: f43db805ccbb7d4e546c51bbe39350f4bbba2efb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049991"
---
# <a name="controlled-validation-of-hybrid-azure-ad-join"></a>Hibrit Azure AD’ye katılıma yönelik denetimli doğrulama

Ön koşulların tümü yerinde olduğunda, Windows aygıtları otomatik olarak Azure AD kiracınızda aygıt olarak kaydedilir. Azure AD'deki bu aygıt kimliklerinin durumu karma Azure AD join olarak adlandırılır. Bu makalede kapsanan kavramlar hakkında daha fazla bilgiyi Azure [Etkin Dizini'nde cihaz yönetimine giriş](overview.md) ve karma Azure Active [Directory'nizi planlayın](hybrid-azuread-join-plan.md)makalelerinde bulabilirsiniz.

Kuruluşlar, tüm kuruluşlarında tek seferde etkinleştirmeden önce karma Azure AD birleştirmesinin kontrollü bir doğrulamasını yapmak isteyebilir. Bu makalede, karma Azure AD join'in kontrollü bir doğrulamasının nasıl gerçekleştirileceği açıklanmaktadır.

## <a name="controlled-validation-of-hybrid-azure-ad-join-on-windows-current-devices"></a>Windows geçerli cihazlarda karma Azure AD birleştirme denetiminde doğrulama

Windows masaüstü işletim sistemini çalıştıran aygıtlar için desteklenen sürüm Windows 10 Anniversary Update (sürüm 1607) veya daha sonraki sürümdür. En iyi yöntem olarak, Windows 10'un en son sürümüne yükseltin.

Windows geçerli cihazlarda karma Azure AD join'in kontrollü bir şekilde doğrulanmasını yapmak için şunları yapmanız gerekir:

1. Varsa Active Directory (AD)'den Hizmet Bağlantı Noktası (SCP) girişini temizleme
1. Grup İlkesi Nesnesi (GPO) kullanarak etki alanı birleştirilmiş bilgisayarlarınızda SCP için istemci tarafı kayıt defteri ayarını yapılandırın
1. AD FS kullanıyorsanız, SCP için istemci tarafı kayıt defteri ayarını DA Ad FS sunucunuzda bir GPO kullanarak yapılandırmanız gerekir  
1. Aygıt eşitlemesini etkinleştirmek için Azure AD Connect'te [eşitleme seçeneklerini özelleştirmeniz](../hybrid/how-to-connect-post-installation.md#additional-tasks-available-in-azure-ad-connect) de gerekebilir. 


### <a name="clear-the-scp-from-ad"></a>SCP'yi AD'den temizleyin

AD'deki SCP nesnelerini değiştirmek için Active Directory Services Interfaces Editor 'ı (ADSI Edit) kullanın.

1. **ADSI Masaüstü** uygulamasını kurumsal yönetici olarak başlatın ve yönetim iş istasyonundan veya etki alanı denetleyicisinden başlatın.
1. Etki alanınızın **Yapılandırma Adlandırma Bağlamına** bağlanın.
1. **CN=Configuration,DC=contoso,DC=com** > **CN=Services** > **CN=Device Registration Configuration** adresine göz atın
1. Yaprak **nesneCN=62a0ff2e-97b9-4513-943f-0d221bd30080** sağ tıklayın ve **Özellikleri** seçin
   1. **Öznitelik Düzenleyicisi** penceresinden **anahtar kelimeler** seçin ve **Edit'i** tıklatın
   1. **azureADId** ve **azureADName** değerlerini seçin (birer birer) ve **Kaldır'ı** tıklatın
1. **ADSI'yi** Kapat


### <a name="configure-client-side-registry-setting-for-scp"></a>SCP için istemci tarafı kayıt ayarı yapılandırma

Aygıtlarınızın kayıt defterinde bir SCP girişi yapılandıran bir kayıt defteri ayarı dağıtmak için bir Grup İlkesi Nesnesi (GPO) oluşturmak için aşağıdaki örneği kullanın.

1. Grup İlkesi Yönetimi konsolu açın ve etki alanınızda yeni bir Grup İlkesi Nesnesi oluşturun.
   1. Yeni oluşturduğunuz GPO'ya bir ad (örneğin, ClientSideSCP) sağlayın.
1. GPO'yu düzenleme ve aşağıdaki yolu bulma: **Bilgisayar Yapılandırma** > **Tercihleri** > **Windows Ayarları** > **Kayıt Defteri**
1. Kayıt Defteri'ne sağ tıklayın ve **Yeni** > **Kayıt Defteri Öğesi'ni** seçin
   1. **Genel** sekmesinde, aşağıdakileri yapılandırın
      1. Eylem: **Güncelleme**
      1. Kovan: **HKEY_LOCAL_MACHINE**
      1. Anahtar Yol: **SOFTWARE\Microsoft\Windows\CurrentVersion\CDJ\AAD**
      1. Değer adı: **TenantId**
      1. Değer türü: **REG_SZ**
      1. Değer verileri: Azure REKLAM örneğinizin GUID veya **Dizin Kimliği** (Bu değer **Azure portalı** > **Azure Active Directory** > **Properties** > **Directory ID'de**bulunabilir)
   1. **Tamam'ı** tıklatın
1. Kayıt Defteri'ne sağ tıklayın ve **Yeni** > **Kayıt Defteri Öğesi'ni** seçin
   1. **Genel** sekmesinde, aşağıdakileri yapılandırın
      1. Eylem: **Güncelleme**
      1. Kovan: **HKEY_LOCAL_MACHINE**
      1. Anahtar Yol: **SOFTWARE\Microsoft\Windows\CurrentVersion\CDJ\AAD**
      1. Değer adı: **TenantName**
      1. Değer türü: **REG_SZ**
      1. Değer verileri: AD FS gibi federe ortamı kullanıyorsanız doğrulanmış **alan adınız.** Yönetilen ortamı kullanıyorsanız, `contoso.onmicrosoft.com` doğrulanmış alan **adınız** veya onmicrosoft.com alan adınız örneğin
   1. **Tamam'ı** tıklatın
1. Yeni oluşturulan GPO için düzenleyiciyi kapatma
1. Yeni oluşturulan GPO'yu, kontrollü kullanıma alma popülasyonunuza ait etki alanı birleştirilmiş bilgisayarları içeren istediğiniz OU'ya bağla

### <a name="configure-ad-fs-settings"></a>AD FS ayarlarını yapılandırma

AD FS kullanıyorsanız, öncelikle GPO'yu AD FS sunucularınıza bağlayarak yukarıda belirtilen yönergeleri kullanarak istemci tarafındaki SCP'yi yapılandırmanız gerekir. SCP nesnesi aygıt nesneleri için yetki kaynağını tanımlar. Şirket içinde veya Azure AD olabilir. İstemci tarafı SCP AD FS için yapılandırıldığında, aygıt nesnelerinin kaynağı Azure AD olarak belirlenir.

> [!NOTE]
> AD FS sunucularınızda istemci tarafındaki SCP'yi yapılandırmayı başaramadıysanız, aygıt kimlikleri için kaynak şirket içi olarak kabul edilir. ADFS daha sonra ADFS Aygıt Kaydı'nın "MaximumInactiveDays" özelliğinde tanımlanan süre sonunda aygıt nesnelerini şirket içi dizinden silmeye başlar. ADFS Cihaz Kayıt nesneleri [Get-AdfsDeviceRegistration cmdlet](/powershell/module/adfs/get-adfsdeviceregistration?view=win10-ps)kullanılarak bulunabilir.

## <a name="controlled-validation-of-hybrid-azure-ad-join-on-windows-down-level-devices"></a>Windows alt düzey cihazlarda karma Azure AD birleştirme denetimi

Windows alt düzey aygıtları kaydetmek için kuruluşların, Microsoft İndirme Merkezi'nde bulunmayan [Windows 10 bilgisayarlar için Microsoft İşyeri Birleştirme'yi](https://www.microsoft.com/download/details.aspx?id=53554) yüklemesi gerekir.

 [Paketi Microsoft Endpoint Configuration Manager](/configmgr/)gibi bir yazılım dağıtım sistemi kullanarak dağıtabilirsiniz. Paket sessiz parametre ile standart sessiz kurulum seçeneklerini destekler. Configuration Manager'ın geçerli şubesi, tamamlanan kayıtları izleme olanağı gibi önceki sürümlere göre avantajlar sunar.

Yükleyici, kullanıcı bağlamında çalışan sistemde zamanlanmış bir görev oluşturur. Kullanıcı Windows'da kaydolduğunda görev tetiklenir. Görev, Azure AD ile kimlik doğrulaması yaptıktan sonra azure AD ile aygıta sessizce katılır.

Aygıt kaydını denetlemek için, Windows Installer paketini seçtiğiniz Windows alt düzey aygıtgrubuna dağıtmanız gerekir.

> [!NOTE]
> Bir SCP AD'de yapılandırılmamışsa, Grup İlkesi Nesnesi (GPO) kullanarak etki alanı birleştirilmiş bilgisayarlarınızda [SCP için istemci tarafı kayıt defteri ayarını yapılandırmak için](#configure-client-side-registry-setting-for-scp)açıklandığı şekilde aynı yaklaşımı izlemeniz gerekir.


Her şeyin beklendiği gibi çalıştığını doğruladıktan sonra, Azure AD Connect kullanarak [SCP'yi yapılandırarak](hybrid-azuread-join-managed-domains.md#configure-hybrid-azure-ad-join)windows geçerli ve alt düzey aygıtlarınızın geri kalanını Azure AD'ye otomatik olarak kaydedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Karma Azure Etkin Dizininizi uygulamanızı planlayın](hybrid-azuread-join-plan.md)
