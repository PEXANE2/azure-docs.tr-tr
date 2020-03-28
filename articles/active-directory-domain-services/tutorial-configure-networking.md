---
title: Öğretici - Azure AD Etki Alanı Hizmetleri için sanal ağı yapılandırma | Microsoft Dokümanlar
description: Bu eğitimde, Azure portalını kullanarak bir Azure Etkin Dizin Etki Alanı Hizmetleri örneği için bir Azure sanal ağ alt ağı veya ağ eşlemesini nasıl oluşturup yapılandırabileceğinizi öğrenirsiniz.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 10/30/2019
ms.author: iainfou
ms.openlocfilehash: 26122278ad74fb1d383ca7a900810b6060ee78f5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73172693"
---
# <a name="tutorial-configure-virtual-networking-for-an-azure-active-directory-domain-services-instance"></a>Öğretici: Azure Etkin Dizin Etki Alanı Hizmetleri örneği için sanal ağı yapılandırma

Kullanıcılara ve uygulamalara bağlantı sağlamak için, Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) yönetilen etki alanı bir Azure sanal ağ alt ağına dağıtılır. Bu sanal ağ alt ağı yalnızca Azure platformu tarafından sağlanan yönetilen etki alanı kaynakları için kullanılmalıdır. Kendi Sanal M'lerinizi ve uygulamalarınızı oluşturduğunuzda, bunlar aynı sanal ağ alt ağına dağıtılmamalıdır. Bunun yerine, uygulamalarınızı ayrı bir sanal ağ alt ağına veya Azure AD DS sanal ağına bakan ayrı bir sanal ağda oluşturmanız ve dağıtmanız gerekir.

Bu öğretici, özel bir sanal ağ alt netini nasıl oluşturup yapılandırabileceğinizi veya Azure AD DS yönetilen etki alanının sanal ağına farklı bir ağı nasıl eşlediğinizi gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure AD DS'ye etki alanı yla birleştirilmiş kaynaklar için sanal ağ bağlantısı seçeneklerini anlama
> * Azure AD DS sanal ağında bir IP adres aralığı ve ek alt ağ oluşturun
> * Azure AD DS'den ayrı bir ağa bakan sanal ağı yapılandırma

Azure aboneliğiniz yoksa, başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure AD DS'yi etkinleştirmek için Azure AD kiracınızda *genel yönetici* ayrıcalıklarına ihtiyacınız vardır.
* Gerekli Azure AD DS kaynaklarını oluşturmak için Azure aboneliğinizde *Katılımcı* ayrıcalıklarına ihtiyacınız vardır.
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, ilk öğretici [bir Azure Active Directory Etki Alanı Hizmetleri örneği oluşturur ve yapılandırır.][create-azure-ad-ds-instance]

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bu öğreticide, Azure portalını kullanarak Azure AD DS örneğini oluşturur ve yapılandırırsınız. Başlamak için önce [Azure portalında](https://portal.azure.com)oturum açın.

## <a name="application-workload-connectivity-options"></a>Uygulama iş yükü bağlantı seçenekleri

Önceki öğreticide, sanal ağ için bazı varsayılan yapılandırma seçeneklerini kullanan bir Azure AD DS yönetilen etki alanı oluşturulmuştur. Bu varsayılan seçenekler bir Azure sanal ağ ve sanal ağ alt ağı oluşturdu. Yönetilen etki alanı hizmetlerini sağlayan Azure AD DS etki alanı denetleyicileri bu sanal ağ alt ağına bağlıdır.

Azure AD DS yönetilen etki alanını kullanması gereken VM'ler oluşturup çalıştırdığınızda, ağ bağlantısının sağlanması gerekir. Bu ağ bağlantısı aşağıdaki yollardan biriyle sağlanabilir:

* Varsayılan Azure AD DS yönetilen etki alanının sanal ağında ek bir sanal ağ alt ağı oluşturun. Bu ek alt ağ, VM'lerinizi oluşturduğunuz ve bağladığınız yerdir.
    * SANAL M'ler aynı sanal ağın parçası olduğundan, otomatik olarak ad çözümlemesi yapabilir ve Azure AD DS etki alanı denetleyicileriyle iletişim kurabilir.
* Azure AD DS'den ayrılan Azure sanal ağını, etki alanının sanal ağını bir veya daha fazla ayrı sanal ağa göre yapılandırın. Bu ayrı sanal ağlar, Sanal M'lerinizi oluşturduğunuz ve bağladığınız yerlerdir.
    * Sanal ağ eşlemini yapılandırdığınızda, ad çözümlemesi için DNS ayarlarını Azure AD DS etki alanı denetleyicilerine geri kullanmak üzere yapılandırmanız gerekir.

Genellikle bu ağ bağlantısı seçeneklerinden birini kullanırsınız. Bu seçim genellikle Azure kaynaklarınızı nasıl ayrı olarak yönetmek istediğinize bağlı. Azure AD DS'yi ve bağlı VM'leri bir kaynak grubu olarak yönetmek istiyorsanız, VM'ler için ek bir sanal ağ alt ağı oluşturabilirsiniz. Azure AD DS yönetimini ve ardından bağlı tüm Sanal M'leri ayırmak istiyorsanız, sanal ağ eşleme sini kullanabilirsiniz. Ayrıca, Azure ortamınızdaki varolan sanal ağa bağlı olan mevcut VM'lere bağlantı sağlamak için sanal ağ izleme seçeneğini de seçebilirsiniz.

Bu öğreticide, yalnızca bu sanal ağ bağlantı seçeneklerini yapılandırmanız gerekir.

Sanal ağın nasıl planlanıp yapılandırılabildiğini hakkında daha fazla bilgi için bkz: [Azure Active Directory Domain Services[ağ dikkat leri] için ağ dikkat leri.

## <a name="create-a-virtual-network-subnet"></a>Sanal ağ alt ağı oluşturma

Varsayılan olarak, Azure AD DS yönetilen etki alanı yla oluşturulan Azure sanal ağı tek bir sanal ağ alt ağı içerir. Bu sanal ağ alt ağı yalnızca yönetilen etki alanı hizmetleri sağlamak için Azure platformu tarafından kullanılmalıdır. Bu Azure sanal ağında kendi SANAL M'lerinizi oluşturmak ve kullanmak için ek bir alt ağ oluşturun.

VM'ler ve uygulama iş yükleri için sanal ağ alt ağı oluşturmak için aşağıdaki adımları tamamlayın:

1. Azure portalında, *myResourceGroup*gibi Azure AD DS yönetilen etki alanınızın kaynak grubunu seçin. Kaynaklar listesinden, *aadds-vnet*gibi varsayılan sanal ağı seçin.
1. Sanal ağ penceresinin sol menüsünde **Adres alanı'nı**seçin. Sanal ağ, varsayılan alt ağ tarafından kullanılan *10.0.1.0/24*tek bir adres alanı yla oluşturulur.

    Sanal ağa ek bir IP adresi aralığı ekleyin. Bu adres aralığının boyutu ve kullanılacak gerçek IP adresi aralığı, zaten dağıtılan diğer ağ kaynaklarına bağlıdır. IP adres aralığı, Azure veya şirket içi ortamınızdaki varolan adres aralıklarıyla örtüşmemelidir. IP adresi aralığını alt ağa dağıtmayı beklediğiniz VM sayısına göre yeterince büyük boyutlandırdığınızdan emin olun.

    Aşağıdaki örnekte, *10.0.2.0/24* ek bir IP adresi aralığı eklenir. Hazır olduğunda **Kaydet'i**seçin.

    ![Azure portalına ek bir sanal ağ IP adresi aralığı ekleme](./media/tutorial-configure-networking/add-vnet-address-range.png)

1. Ardından, sanal ağ penceresinin sol menüsünde **Alt Ağlar'ı**seçin ve alt ağ eklemek için **+ Subnet'i** seçin.
1. Alt ağ için *iş yükleri*gibi bir ad girin. Gerekirse, önceki adımlarda sanal ağ için yapılandırılan IP adresi aralığının bir alt kümesini kullanmak istiyorsanız **Adres aralığını** güncelleştirin. Şimdilik, ağ güvenlik grubu, rota tablosu, hizmet bitiş noktaları gibi seçenekler için varsayılanları bırakın.

    Aşağıdaki örnekte, *10.0.2.0/24* IP adresi aralığını kullanan *iş yükleri* adlı bir alt ağ oluşturulur:

    ![Azure portalına ek bir sanal ağ alt ağı ekleme](./media/tutorial-configure-networking/add-vnet-subnet.png)

1. Hazır olduğunda **Tamam'ı**seçin. Sanal ağ alt ağı oluşturmak birkaç dakika sürer.

Azure AD DS yönetilen etki alanını kullanması gereken bir VM oluşturduğunuzda, bu sanal ağ alt netini seçtiğinizden emin olun. Varsayılan *aadds-subnet'te*VM'ler oluşturmayın. Farklı bir sanal ağ seçerseniz, sanal ağ eşlemeyi yapılandırmadığınız sürece Azure AD DS yönetilen etki alanına ulaşmak için ağ bağlantısı ve DNS çözünürlüğü yoktur.

## <a name="configure-virtual-network-peering"></a>Sanal ağ eşleme yapılandırma

Sanal MM'ler için mevcut bir Azure sanal ağınız olabilir veya Azure AD DS yönetilen etki alanı sanal ağınızı ayrı tutmak isteyebilirsiniz. Yönetilen etki alanını kullanmak için, diğer sanal ağlardaki VM'lerin Azure AD DS etki alanı denetleyicileriyle iletişim kurmanın bir yolunu niçin kullanması gerekir. Bu bağlantı, Azure sanal ağ eşlemi kullanılarak sağlanabilir.

Azure sanal ağ eşlemesiyle, sanal özel ağ (VPN) aygıtına gerek kalmadan iki sanal ağ birbirine bağlanır. Ağ dansı, sanal ağlara hızla bağlanmanızı ve Azure ortamınızdaki trafik akışlarını tanımlamanızı sağlar. Bakma hakkında daha fazla bilgi için Azure [sanal ağ ağını genel bakış][peering-overview]bilgisine bakın.

Sanal ağı Azure AD DS yönetilen etki alanı sanal ağına eşlemek için aşağıdaki adımları tamamlayın:

1. *Aadds-vnet*adlı Azure AD DS örneğiniz için oluşturulan varsayılan sanal ağı seçin.
1. Sanal ağ penceresinin sol menüsünde, **Peerings'i**seçin.
1. Eşleme oluşturmak için **+ Ekle'yi**seçin. Aşağıdaki örnekte, varsayılan *aadds-vnet* *myVnet*adlı sanal bir ağa bakar. Aşağıdaki ayarları kendi değerlerinizle yapılandırın:

    * **Aadds-vnet'ten uzak sanal ağa bakış adı**: *Aadds-vnet-to-myvnet* gibi iki ağın açıklayıcı tanımlayıcısı
    * **Sanal ağ dağıtım türü**: *Kaynak Yöneticisi*
    * **Abonelik**: *Azure* gibi bakmak istediğiniz sanal ağın aboneliği
    * **Sanal ağ**: *myVnet* gibi eşlemek istediğiniz sanal ağ
    * **myVnet'ten aadds-vnet'e bakışın adı**: *Myvnet-to-aadds-vnet* gibi iki ağın açıklayıcı tanımlayıcısı

    ![Azure portalında sanal ağ eşlemesini yapılandırma](./media/tutorial-configure-networking/create-peering.png)

    Ortamınız için özel gereksinimleriniz yoksa sanal ağ erişimi veya iletilen trafik için diğer varsayılanları bırakın, ardından **Tamam'ı**seçin.

1. Hem Azure AD DS sanal ağında hem de seçtiğiniz sanal ağda eşleme oluşturmak birkaç dakika sürer. Hazır olduğunda, Aşağıdaki örnekte gösterildiği **gibi, Bağlı Eşleme durumu** *raporları:*

    ![Azure portalında başarılı bir şekilde bağlanan eşlenmiş ağlar](./media/tutorial-configure-networking/connected-peering.png)

Eşlenen sanal ağdaki Sanal Aitler Azure AD DS yönetilen etki alanını kullanmadan önce, DNS sunucularını doğru ad çözümlemesine izin verecek şekilde yapılandırın.

### <a name="configure-dns-servers-in-the-peered-virtual-network"></a>Eşlenen sanal ağdaki DNS sunucularını yapılandırma

Bakan sanal ağdaki SANAL M'ler ve uygulamaların Azure AD DS yönetilen etki alanıyla başarılı bir şekilde konuşabilmesi için DNS ayarlarının güncellenmesi gerekir. Azure AD DS etki alanı denetleyicilerinin IP adresleri, bakan sanal ağdaki DNS sunucuları olarak yapılandırılmalıdır. Etki alanı denetleyicilerini, denetlenen sanal ağ için DNS sunucuları olarak yapılandırmanın iki yolu vardır:

* Azure sanal ağı DNS sunucularını Azure AD DS etki alanı denetleyicilerini kullanacak şekilde yapılandırın.
* Eşlenen sanal ağda kullanılan varolan DNS sunucusunu, koşullu DNS iletmesini kullanarak Azure AD DS yönetilen etki alanına doğrudan sorgular için yapılandırın. Bu adımlar, kullanılan varolan DNS sunucusuna bağlı olarak değişir.

Bu öğreticide, tüm sorguları Azure AD DS etki alanı denetleyicilerine yönlendirecek şekilde Azure sanal ağ DNS sunucularını yapılandıralım.

1. Azure portalında, *myResourceGroup*gibi eşlenen sanal ağın kaynak grubunu seçin. Kaynaklar listesinden *myVnet*gibi eşlenmiş sanal ağı seçin.
1. Sanal ağ penceresinin sol menüsünde **DNS sunucularını**seçin.
1. Varsayılan olarak, sanal ağ yerleşik Azure tarafından sağlanan DNS sunucularını kullanır. **Özel** DNS sunucularını kullanmayı seçin. Genellikle *10.0.1.4* ve *10.0.1.5*olan Azure AD DS etki alanı denetleyicilerinin IP adreslerini girin. Bu IP adreslerini portaldaki Azure AD DS yönetilen etki alanınızın **Genel Bakış** penceresinde onaylayın.

    ![Sanal ağ DNS sunucularını Azure AD DS etki alanı denetleyicilerini kullanacak şekilde yapılandırın](./media/tutorial-configure-networking/custom-dns.png)

1. Hazır olduğunda **Kaydet'i**seçin. Sanal ağ için DNS sunucularını güncelleştirmek birkaç dakika sürer.
1. Güncelleştirilmiş DNS ayarlarını VM'lere uygulamak için, eşlenen sanal ağa bağlı VM'leri yeniden başlatın.

Azure AD DS yönetilen etki alanını kullanması gereken bir VM oluşturduğunuzda, bu bakan sanal ağı seçtiğinizden emin olun. Farklı bir sanal ağ seçerseniz, Azure AD DS yönetilen etki alanına ulaşmak için ağ bağlantısı ve DNS çözünürlüğü yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure AD DS'ye etki alanı yla birleştirilmiş kaynaklar için sanal ağ bağlantısı seçeneklerini anlama
> * Azure AD DS sanal ağında bir IP adres aralığı ve ek alt ağ oluşturun
> * Azure AD DS'den ayrı bir ağa bakan sanal ağı yapılandırma

Yönetilen bu etki alanını iş başında görmek için, etki alanına sanal bir makine oluşturun ve katılın.

> [!div class="nextstepaction"]
> [Yönetilen etki alanınızda bir Windows Server sanal makinesine katılma](join-windows-vm.md)

<!-- INTERNAL LINKS --> 
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[create-join-windows-vm]: join-windows-vm.md
[peering-overview]: ../virtual-network/virtual-network-peering-overview.md
