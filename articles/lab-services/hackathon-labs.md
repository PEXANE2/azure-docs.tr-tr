---
title: Hackathon için Azure Lab Services kullanma
description: Bu makalede, hack maratonları çalıştırmak için kullanabileceğiniz Labs oluşturmak için Azure Lab Services nasıl kullanılacağı açıklanır.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 8727fad6f37d1365983127a1a514e87e3a5003b9
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85445891"
---
# <a name="use-azure-lab-services-for-your-next-hackathon"></a>Sonraki Hackathon için Azure Lab Services kullanın
Azure Lab Services, Hackathon için yeni bir sanal makine Laboratuvarı (VM) hızlı bir şekilde çalıştırabilmeniz için hafif ve kullanımı kolay olacak şekilde tasarlanmıştır.  Hackathon 'nizin mümkün olduğunca sorunsuz bir şekilde gitmemesini sağlamak için aşağıdaki denetim listesini kullanın. Bu denetim listesi, Hackathon laboratuvarınızı oluşturmaktan ve yönetmekten sorumlu BT departmanınız veya fakülte tamamlanmalıdır. 

Hackathon için Laboratuvar Hizmetleri 'ni kullanmak için, hem laboratuvar hesabının hem de laboratuvarınızın, Hackathon başlamadan en az birkaç gün önce oluşturulduğundan emin olun. Ayrıca, aşağıdaki yönergeleri izleyin:

## <a name="guidance"></a>Rehber

- **Laboratuvarı, katılımcılara en yakın bir bölgede veya konumda oluşturun**. 

    Gecikme süresini azaltmak için laboratuvarınızı Hackathon katılımcılarınız için en yakın bölgede oluşturun.  Katılımcılarınız dünyanın her yerinden bulunursa, merkezi olarak bulunan bir laboratuvar oluşturmak için en iyi yargılarınızı kullanmanız gerekir.  Ya da Hackathon ' yi, katılımcıların bulunduğu konumlara göre birden çok Laboratuvarı kullanacak şekilde bölebilirsiniz.
- **Kullanım ihtiyaçları için en uygun işlem boyutunu seçin**.

    Genellikle, işlem boyutu arttıkça, sanal makinenin daha hızlı bir şekilde gerçekleştireceği olur. Ancak maliyetleri sınırlandırmak için, katılımcıların ihtiyaçlarına göre uygun işlem boyutunu seçmeniz gerekir. Kullanılabilir işlem boyutları hakkında ayrıntılı bilgi için bkz. [yönetici kılavuzundaki VM boyutlandırma bilgileri](administrator-guide.md#vm-sizing) .
- **Linux VM 'lerine Uzak Masaüstü bağlantısı için RDP\SSH yapılandırma**.

    Hackathon, Linux sanal makinelerini kullanıyorsa, katılımcıların sanal makinelere bağlanmak için RDP (Uzak Masaüstü Protokolü) veya SSH (Secure Shell) kullanabilmesi için Uzak Masaüstü 'nün etkinleştirildiğinden emin olun. Bu adım yalnızca Linux sanal makineleri için gereklidir ve laboratuvar oluşturulurken etkinleştirilmelidir. Ayrıca, RDP için, yayımlamadan önce, RDP sunucusunu ve GUI paketlerini şablon VM 'de yükleyip yapılandırmanız gerekebilir.  Daha fazla bilgi için bkz. [Linux için Uzak Masaüstü 'nü etkinleştirme hakkında nasıl yapılır Kılavuzu](how-to-enable-remote-desktop-linux.md).

- **Windows güncelleştirmelerini yükleyip durdurun**. 

    Bir Windows görüntüsü kullanıyorsanız, laboratuvarın VM 'Leri oluşturmak için yayımlamadan önce en son Windows güncelleştirmelerini laboratuvarın [şablon VM 'sine](how-to-create-manage-template.md) yüklemenizi öneririz. Bu, güvenlik açısından ve Hackathon sırasında katılımcıların kesintiye uğramasını engellemek ve bu da VM 'lerinin yeniden başlatılmasına neden olabilir. Gelecekteki kesintileri engellemek için Windows güncelleştirmelerini kapatmayı de düşünebilirsiniz. [Windows güncelleştirmelerini yükleme ve yapılandırma hakkında nasıl yapılır kılavuzuna](how-to-prepare-windows-template.md#install-and-configure-updates)bakın.
- **Öğrencilerin çalışmalarını nasıl Yedekleyeceğine karar verin**. 

    Öğrenciler, Hackathon ömrü boyunca her birine bir sanal makine atanır. Çalışmalarını doğrudan makineye kaydedebilirler, ancak Hackathon bittikten sonra bu kişilerin erişimine sahip olmaları için öğrencilerin çalışmalarını yedeklemesi önerilir. Örneğin, OneDrive, GitHub vb. gibi bir dış konuma kaydedilmeleri gerekir. OneDrive 'ı kullanmak için laboratuvar sanal makinelerindeki öğrenciler için otomatik olarak yapılandırmayı tercih edebilirsiniz. OneDrive 'ı [yüklemek ve yapılandırmak için nasıl yapılır kılavuzuna](how-to-prepare-windows-template.md#install-and-configure-onedrive)bakın.
- **VM kapasitesini katılımcı sayısına göre ayarlayın**. 

    Laboratuvarınızın sanal makine kapasitesinin, Hackathon size bekleyen katılımcı sayısına göre ayarlandığından emin olun. Şablon sanal makinesini yayımladığınızda, laboratuvardaki makinelerin tümünün oluşturulması birkaç saat sürebilir. Bu nedenle, Hackathon 'in başlangıcına daha iyi bir şekilde başlamanız önerilir. Daha fazla bilgi için bkz. [Laboratuvar kapasitesini güncelleştirme hakkında nasıl yapılır Kılavuzu](how-to-set-virtual-machine-passwords.md#update-the-lab-capacity).

- **Laboratuvar erişimini kısıtlayıp kısıtlamamaya karar verin**. 

    Laboratuvara Kullanıcı eklerken, varsayılan olarak etkinleştirilen erişimi kısıtla seçeneği vardır. Bu özellik, kayıt bağlantısını kullanarak laboratuvara kaydolmadan ve bunlara erişebilmek için Hackathon katılımcılarının tüm e-postalarını listeye eklemenizi gerektirir. Katılanların olaydan önce nerede olacağını bildiğiniz bir Hackathon varsa, erişimi kısıtla seçeneğini devre dışı bırakmayı seçebilirsiniz ve bu, herkesin kayıt bağlantısını kullanarak laboratuvara kaydolmanızı sağlar. Daha fazla bilgi için bkz. [Kullanıcı ekleme hakkında nasıl yapılır Kılavuzu](how-to-configure-student-usage.md#add-users-to-a-lab).

- **Zamanlamayı, kotayı ve oto kapatma ayarlarını doğrulayın**. 

    Laboratuvar Hizmetleri, VM 'lerin kullanımını sınırlamak için çeşitli maliyet denetimleri sağlar. Ancak, bu ayarlar yanlış yapılandırıldıklarında, laboratuvarın sanal makinelerinin beklenmedik şekilde kapanmasına neden olabilirler. Bu ayarların Hackathon için uygun şekilde yapılandırıldığından emin olmak için aşağıdaki ayarları doğrulayın:

    **Zamanlama**: bir [zamanlama](how-to-create-schedules.md) , laboratuvarlarınızın makinelerinizin ne zaman başlatıldığını ve kapatıldığını otomatik olarak denetlemenize olanak tanır. Varsayılan olarak, yeni bir laboratuvar oluşturduğunuzda hiçbir zamanlama yapılandırılmaz. Ancak, laboratuvarınızın zamanlamasının, Hackathon için anlamlı hale göre ayarlandığından emin olmanız gerekir.  Örnek olarak, Hackathon 'niz Cumartesi günü 8:00:00 ' da başlıyorsa ve 5:00 PM 'de Pazar günü sona erdiğinde, 7:30 makineyi Cumartesi (Hackathon başlamadan yaklaşık 30 dakika) ile otomatik olarak başlatan bir zamanlama oluşturabilir ve Pazar günü 5:00:00 ' da kapanıyor. Bunun yerine, bir zamanlamayı da kullanmamaya karar verebilirsiniz.

    **Kota**: [Kota](how-to-configure-student-usage.md#set-quotas-for-users) , katılımcıların zamanlanan saatlerin dışında bir sanal makineye erişiminin olacağı saat sayısını denetler. Bir katılımcı onu kullanırken kotaya ulaşıldığında, makine otomatik olarak kapatılır ve kota arttırılmadığı sürece katılımcı bunu yeniden başlatabilir. Varsayılan olarak, bir laboratuvar oluşturduğunuzda kota 10 saat olarak ayarlanır. Yine, kotayı, Hackathon için yeterli zaman sağlayacak şekilde ayarladığınızdan emin olun; bu, özellikle bir zamanlama oluşturmadıysanız önemlidir.

    **Otomatik kapatma**: etkinleştirildiğinde, [Otomatik kapatma](how-to-enable-shutdown-disconnect.md) AYARı, bir öğrencinin RDP oturumlarından bağlantısı kesildiğinde belirli bir süre sonra Windows sanal makinelerinin otomatik olarak kapatılmasını sağlar. Bu ayar varsayılan olarak devre dışıdır.

- **Laboratuvar VM 'lerine yönelik bağlantılara izin vermek için güvenlik duvarı ayarlarını yapılandırın**. 

    Okulunuzun veya kuruluşunuzun güvenlik duvarı ayarlarının,, rdp\sshkullanarak laboratuvar VM 'lerine bağlanmasına izin verildiğinden emin olun. Daha fazla bilgi için, [ağınızın güvenlik duvarı ayarlarını yapılandırma hakkında nasıl yapılır Kılavuzu](how-to-configure-firewall-settings.md)' na bakın.

- Bu **kullanıcıların tabletlerini, Mac 'leri, bilgisayarlarını vb. katılımcılarına rdp\ SSH Istemcisi yükleyin**.

    Hackathon katılımcılarının, tabletlerinde veya dizüstü bilgisayarlarda laboratuvar VM 'lerine bağlanmak için kullandıkları bir RDP ve/veya SSH istemcisinin yüklü olması gerekir. Farklı RDP veya SSH istemcilerinden seçim yapabilirsiniz, örneğin:

    - Microsoft 'un RDP bağlantıları için **Uzak Masaüstü bağlantısı** uygulaması. Uzak Masaüstü Bağlantısı uygulaması, Kmebooks ve [Mac](https://techcommunity.microsoft.com/t5/azure-lab-services/connecting-to-azure-lab-services-environments-on-your-macos/ba-p/1290162)gibi farklı türlerde platformlarda desteklenir.
    - Bir Linux sanal makinesine bağlanmak için SSH kullanımı için [Putty](https://techcommunity.microsoft.com/t5/azure-lab-services/connecting-to-azure-lab-services-environments-on-your-macos/ba-p/1290162) .
- **Laboratuvar sanal makinelerini doğrulayın**. 

    Laboratuvar VM 'lerini yayımladıktan sonra, bunların düzgün şekilde yapılandırıldığını doğrulamanız gerekir. Yalnızca katılımcının laboratuvar sanal makinelerinden biri için bu doğrulamayı yapmanız gerekir:

    1. RDP and\veya SSH kullanarak bağlanın.
    2. Temel sanal makine görüntüsünü özelleştirmek için yüklediğiniz her bir ek uygulamayı ve aracı açın.
    3. Seçilen işlem boyutuna bağlı olarak, VM performansının yeterli olduğundan emin olmak için katılımcıların verdiği etkinlikleri temsil eden birkaç temel senaryoyu gözden geçir.

## <a name="on-the-day-of-hackathon"></a>Hackathon gününde
Bu bölümde, Hackathon gününü tamamlamaya yönelik adımlar özetlenmektedir.

1. **Laboratuvar VM 'Leri başlatın**.

    IŞLETIM sisteminize bağlı olarak, laboratuvar makinenizin başlaması 30 dakika kadar sürebilir. Sonuç olarak, Hackathon başlamadan önce makineleri başlatmak önemlidir. böylece katılımcılar beklemek zorunda kalmaz. Bir zamanlama kullanıyorsanız, VM 'Lerin daha önce en az 30 dakika içinde otomatik olarak başlatıldığından emin olun.
2. **Öğrencileri laboratuvar sanal makinesine kaydetmek ve bunlara erişmek Için davet edin**. 

    Katılımcıların laboratuvar VM 'lerine erişebilmesi için katılımcılara aşağıdaki bilgileri sağlayın. 

    - Laboratuvarın kayıt bağlantısı. 
    - Makineye bağlanmak için kullanılması gereken kimlik bilgileri. Bu adım yalnızca laboratuvarınız Windows tabanlı bir görüntü kullanıyorsa ve tüm VM 'Leri aynı parolayı kullanacak şekilde yapılandırdıysanız geçerlidir.
    - Katılanların SSH ve/veya RDP makinelerine nasıl bağlanacağını gösteren yönergeler.

        Daha fazla bilgi için bkz. [nasıl yapılır kılavuzu kullanıcılara davetiye gönderme](how-to-configure-student-usage.md?branch=master#send-invitations-to-users) ve [Linux VM 'lerine bağlanma](how-to-use-remote-desktop-linux-student.md?branch=master). 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaledeki yönergeleri izleyerek ders laboratuvarlarında bir laboratuvar hesabı oluşturmaya başlayın: [öğretici: Azure Lab Services bir laboratuvar hesabı kurma](tutorial-setup-lab-account.md).