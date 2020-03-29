---
title: Yeni oturum ana bilgisayarlarıyla mevcut ana bilgisayar havuzunun genişletilmesi - Azure
description: Windows Sanal Masaüstü'nde yeni oturum ana bilgisayarları ile varolan ana bilgisayar havuzu genişletme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: aee5195fe86fed3e631908a38d3bdb7d5e4883b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79365228"
---
# <a name="expand-an-existing-host-pool-with-new-session-hosts"></a>Yeni oturum ana bilgisayarlarıyla varolan ana bilgisayar havuzunun genişletilmesi

Ana bilgisayar havuzunuzdaki kullanımı artırırken, yeni yükü işlemek için mevcut ana bilgisayar havuzunu yeni oturum ana bilgisayarlarıyla genişletmeniz gerekebilir.

Bu makalede, yeni oturum ana bilgisayarları ile varolan bir ana bilgisayar havuzu genişletmek nasıl anlatabilirsiniz.

## <a name="what-you-need-to-expand-the-host-pool"></a>Ana bilgisayar havuzunu genişletmek için gerekenler

Başlamadan önce, aşağıdaki yöntemlerden birini kullanarak bir ana bilgisayar havuzu ve oturum ana bilgisayar sanal makineleri (VM) oluşturduğunuzdan emin olun:

- [Azure Marketi teklifi](./create-host-pools-azure-marketplace.md)
- [GitHub Azure Kaynak Yöneticisi şablonu](./create-host-pools-arm-template.md)
- [PowerShell ile ana bilgisayar havuzu oluşturma](./create-host-pools-powershell.md)

Ayrıca, ana bilgisayar havuzunu ve oturum ana bilgisayar VM'lerini ilk oluşturduğunuz da aşağıdaki bilgilere ihtiyacınız vardır:

- VM boyutu, görüntü ve ad öneki
- Etki alanı birleştirme ve Windows Sanal Masaüstü kiracı yönetici kimlik bilgileri
- Sanal ağ adı ve alt net adı

Sonraki üç bölüm, ana bilgisayar havuzunu genişletmek için kullanabileceğiniz üç yöntemdir. Hangi dağıtım aracıyla rahat olursanız o labilirsiniz.

>[!NOTE]
>Dağıtım aşamasında, önceki oturum ana bilgisayar VM kaynaklarının hata iletilerini şu anda kapalıysa görürsünüz. Bu hatalar, Azure'un oturum ana bilgisayar VM'lerinin varolan ana bilgisayar havuzunuza doğru şekilde kaydolduğunu doğrulamak için PowerShell DSC uzantısını çalıştıramaması nedeniyle olur. Bu hataları güvenle yoksayabilirsiniz veya dağıtım işlemini başlatmadan önce varolan ana bilgisayar havuzunda tüm oturum ana bilgisayar VM'lerini başlatarak hataları önleyebilirsiniz.

## <a name="redeploy-from-azure"></a>Azure'dan yeniden dağıtma

[Azure Marketi teklifini](./create-host-pools-azure-marketplace.md) veya [GitHub Azure Kaynak Yöneticisi şablonunu](./create-host-pools-arm-template.md)kullanarak bir ana bilgisayar havuzu ve oturum ana bilgisayar VM'leri zaten oluşturduysanız, aynı şablonu Azure portalından yeniden dağıtabilirsiniz. Şablonu yeniden dağıtma, parolalar dışında orijinal şablona girdiğiniz tüm bilgileri otomatik olarak yeniden girer.

Ana bilgisayar havuzunu genişletmek için Azure Kaynak Yöneticisi şablonu şu şekilde yeniden dağıtılır:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Azure portalının üst kısmındaki arama çubuğundan **Kaynak gruplarını** arayın ve **Hizmetler**altındaki öğeyi seçin.
3. Ana bilgisayar havuzunu oluşturduğunuz kaynak grubunu bulun ve seçin.
4. Tarayıcının sol tarafındaki panelde **Dağıtımlar'ı**seçin.
5. Ana bilgisayar havuzu oluşturma işleminiz için uygun dağıtımı seçin:
     - Azure Marketi teklifiyle orijinal ana bilgisayar havuzunu oluşturduysanız, **rds.wvd-provision-host-pool**ile başlayan dağıtımı seçin.
     - GitHub Azure Kaynak Yöneticisi şablonuyla orijinal ana bilgisayar havuzunu oluşturduysanız, **Microsoft.Template**adlı dağıtımı seçin.
6. **Yeniden Dağıt'ı**seçin.
     
     >[!NOTE]
     >**Yeniden Dağıt'ı**seçtiğinizde şablon otomatik olarak yeniden dağıtılmıyorsa, tarayıcınızın sol tarafındaki panelde **Şablon'u** seçin ve ardından **Dağıt'ı**seçin.

7. Varolan ana bilgisayar havuzunda geçerli oturum ana bilgisayar VM'leri içeren kaynak grubunu seçin.
     
     >[!NOTE]
     >Girdiğiniz kaynak doğru olsa bile farklı bir kaynak grubu seçmenizi söyleyen bir hata görürseniz, başka bir kaynak grubu seçin ve ardından özgün kaynak grubunu seçin.

8. *_artifactsLocation*için aşağıdaki URL'yi girin:`https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/`
9. Rdsh *Örnek Sayısı'na*istediğiniz yeni toplam oturum ana bilgisayar sayısını girin. Örneğin, ana bilgisayar havuzunuzu beş oturum ana bilgisayardan sekiz oturuma genişletiyorsanız, **8**girin.
10. Varolan etki alanı UPN için kullandığınız aynı varolan etki alanı parolasını girin. Şablonu çalıştırdığınızda hataya neden olacağından, kullanıcı adını değiştirmeyin.
11. *Kiracı Yönetici Upn Veya Uygulama Kimliği*için girdiğiniz kullanıcı veya uygulama kimliği için kullandığınız aynı kiracı yönetici parolasını girin. Bir kez daha, kullanıcı adını değiştirmeyin.
12. Ev sahibi havuzunuzu genişletmek için gönderiyi tamamlayın.

## <a name="run-the-azure-marketplace-offering"></a>Azure Marketi teklifini çalıştırın

Yeni bir ana bilgisayar havuzu sağlamak için Azure [Marketi'ni çalıştır teklifine ulaşana](./create-host-pools-azure-marketplace.md#run-the-azure-marketplace-offering-to-provision-a-new-host-pool)kadar Azure Marketi'ni kullanarak ana bilgisayar havuzu [oluştur'daki](./create-host-pools-azure-marketplace.md) yönergeleri izleyin. Bu noktaya geldiğinizde, her sekme için aşağıdaki bilgileri girmeniz gerekir:

### <a name="basics"></a>Temel Bilgiler

Varsayılan *masaüstü kullanıcıları*dışında, ana bilgisayar havuzunu ve oturum ana bilgisayar VM'lerini ilk oluşturduğunuzda bu bölümdeki tüm değerler, sağladığınız değerlerle eşleşmelidir:

1.    *Abonelik*için, ana bilgisayar havuzunu ilk oluşturduğunuz aboneliği seçin.
2.    *Kaynak grubu*için, varolan ana bilgisayar oturumu ana bilgisayarı VM'lerin bulunduğu aynı kaynak grubunu seçin.
3.    *Bölge*için, varolan ana bilgisayar havuzu oturum ana bilgisayarı VM'lerinin bulunduğu aynı bölgeyi seçin.
4.    *Hostpool adı için,* varolan ana bilgisayar havuzunun adını girin.
5.    *Masaüstü türü için,* varolan ana bilgisayar havuzuyla eşleşen masaüstü türünü seçin.
6.    *Varsayılan masaüstü kullanıcıları*için, Windows Sanal Masaüstü istemcilerinde oturum açmanız ve Azure Marketi teklifi bittikten sonra masaüstüne erişmek istediğiniz ek kullanıcıların virgülle ayrılmış bir listesini girin. Örneğin, user3@contoso.com atamak user4@contoso.com ve erişmek istiyorsanız, user3@contoso.com.user4@contoso.com
7.    İleri'yi Seçin **: Sanal makineyi yapılandır.**

>[!NOTE]
>Varsayılan *masaüstü kullanıcıları*dışında, tüm alanlar varolan ana bilgisayar havuzunda yapılandırılabildikleriyle tam olarak eşleşmelidir. Yeni bir ana bilgisayar havuzuyla sonuçlanacak bir uyumsuzluk varsa.

### <a name="configure-virtual-machines"></a>Sanal makineleri yapılandırma

Bu bölümdeki tüm parametre değerleri, toplam VM sayısı dışında ana bilgisayar havuzunu ve oturum ana bilgisayarı VM'lerini ilk oluşturduğunuzda sağladığınız değerlerle eşleşmelidir. Girdiğiniz VM sayısı genişletilmiş ana bilgisayar havuzunuzdaki VM sayısı olacaktır:

1. Varolan oturum ana bilgisayar VM'leri ile eşleşen VM boyutunu seçin.
    
    >[!NOTE]
    >Aradığınız belirli VM boyutu VM boyut seçicisinde görünmüyorsa, bunun nedeni henüz Azure Marketi aracına binmemiş olmamızdır. VM boyutu istemek için, windows [sanal masaüstü uservoice forumunda](https://windowsvirtualdesktop.uservoice.com/forums/921118-general)bir istek oluşturun veya varolan bir isteği yüksek oylandırın.

2. Ev sahibi havuzunuzda olmasını istediğiniz toplam oturum ana bilgisayar sayısını seçmek için *Kullanım Profilini,* *Toplam kullanıcı*sayısını ve *sanal makine* sayısını özelleştirin. Örneğin, ana bilgisayar havuzunuzu beş oturum ana bilgisayardan sekiz oturuma genişletiyorsanız, bu seçenekleri 8 sanal makineye ulaşmak için yapılandırın.
3. Sanal makinelerin adları için bir önek girin. Örneğin, "önek" adını girerseniz, sanal makinelere "önek-0", "önek-1" ve benzeri adlar verilir.
4. Sonraki seçin **: Sanal makine ayarları**.

### <a name="virtual-machine-settings"></a>Sanal makine ayarları

Bu bölümdeki tüm parametre değerleri, ana bilgisayar havuzunu ve oturum ana bilgisayar VM'lerini ilk oluşturduğunuzda sağladığınız değerlerle eşleşmelidir:

1. *Resim kaynağı* ve *Resim İşletim Sistemi sürümü*için, ana bilgisayar havuzunu ilk oluşturduğunuzda sağladığınız aynı bilgileri girin.
2. *AD etki alanına UPN'ye* ve ilişkili parolalara katılmak için, Active Directory etki alanına Katılmak için ana bilgisayar havuzunu ilk oluşturduğunuzda sağladığınız aynı bilgileri girin. Bu kimlik bilgileri, sanal makinelerinizde yerel bir hesap oluşturmak için kullanılır. Kimlik bilgilerini daha sonra değiştirmek için bu yerel hesapları sıfırlayabilirsiniz.
3. Sanal ağ bilgileri için, mevcut ana bilgisayar havuzu oturum ana bilgisayarınızın bulunduğu yer için aynı sanal ağı ve alt ağı seçin.
4. İleri'yi Seçin **: Windows Sanal Masaüstü bilgilerini yapılandırın.**

### <a name="windows-virtual-desktop-information"></a>Windows Sanal Masaüstü bilgileri

Bu bölümdeki tüm parametre değerleri, ana bilgisayar havuzunu ve oturum ana bilgisayar VM'lerini ilk oluşturduğunuzda sağladığınız değerlerle eşleşmelidir:

1. *Windows Sanal Masaüstü kiracı grup adı için,* kiracınızı içeren kiracı grubunun adını girin. Belirli bir kiracı grup adı verilmedikçe varsayılan olarak bırakın.
2. *Windows Sanal Masaüstü kiracı adı için,* bu ana bilgisayar havuzunu oluşturacak kiracının adını girin.
3. Ana bilgisayar havuzunu ve oturum ana bilgisayar VM'lerini ilk oluşturduğunuzda kullandığınız kimlik bilgilerini belirtin. Bir hizmet yöneticisi kullanıyorsanız, hizmet yöneticinizin bulunduğu Azure Etkin Dizin örneğinin kimliğini girin.
4. Sonraki'ni Seçin **: Gözden Geçirin + oluşturun**.

## <a name="run-the-github-azure-resource-manager-template"></a>GitHub Azure Kaynak Yöneticisi şablonu çalıştırın

Yeni bir [ana bilgisayar havuzu sağlamak için Azure Kaynak Yöneticisi şablonundaki](./create-host-pools-arm-template.md#run-the-azure-resource-manager-template-for-provisioning-a-new-host-pool) yönergeleri çalıştır'dan izleyin ve Rdsh Örnek Sayısı dışında aynı parametre *değerlerinin tümünün*sağlanmasını sağlayın. Şablonu çalıştırdıktan sonra ana bilgisayar havuzuna istediğiniz oturum ana bilgisayar VM sayısını girin. Örneğin, ana bilgisayar havuzunuzu beş oturum ana bilgisayardan sekiz oturuma genişletiyorsanız, **8**girin.

## <a name="next-steps"></a>Sonraki adımlar

Mevcut ana bilgisayar havuzunuzu genişlettiğinize göre, kullanıcı oturumunun bir parçası olarak test etmek için Windows Sanal Masaüstü istemcisinde oturum açabilirsiniz. Aşağıdaki istemcilerden herhangi biriyle bir oturuma bağlanabilirsiniz:

- [Windows Masaüstü istemcisine bağlanma](./connect-windows-7-and-10.md)
- [Web istemcisiyle bağlanma](./connect-web.md)
- [Android istemcisiyle bağlanma](./connect-android.md)
- [macOS istemcisiyle bağlanma](./connect-macos.md)
- [iOS istemcisiyle bağlanma](./connect-ios.md)
