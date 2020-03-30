---
title: IPv6 - Azure şablonu yla Internet'e bakan bir yük dengeleyicisi dağıtma
titleSuffix: Azure Load Balancer
description: Azure şablonu kullanarak Azure Yük Dengeleyicisi ve yük dengesi olan VM'ler için IPv6 desteğini nasıl dağıtacaklarını öğrenin.
services: load-balancer
documentationcenter: na
author: asudbring
keywords: ipv6, azure yük dengeleyici, çift yığın, genel ip, yerli ipv6, mobil, iot
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: eb9703a1944a650f41d76c05d79764f8bdf8cd52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76045440"
---
# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Şablon kullanarak IPv6 ile Internet'e bakan bir yük dengeleyici çözümü dağıtma

> [!div class="op_single_selector"]
> * [Powershell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Şablon](load-balancer-ipv6-internet-template.md)


>[!NOTE] 
>Bu makalede, Temel Yük Dengeleyicileri hem IPv4 ve IPv6 bağlantısı sağlamak için izin vermek için bir giriş IPv6 özelliği açıklanır. IPv6 bağlantılarını Sanal Ağlarınızla birleştiren ve IPv6 Ağ Güvenlik Grubu kuralları, IPv6 Kullanıcı tanımlı yönlendirme, IPv6 Temel ve Standart yük dengeleme ve daha fazlası gibi temel özellikleri içeren [Azure VNET'ler için](../virtual-network/ipv6-overview.md) IPv6 ile kapsamlı IPv6 bağlantısı artık kullanılabilir.  Azure VNET'ler için IPv6, Azure'daki IPv6 uygulamaları için önerilen standarttır. [Azure VNET Powershell Dağıtımı için IPv6'ya](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) bakın  

Azure Load Balancer bir Katman 4 (TCP, UDP) yük dengeleyicidir. Yük dengeleyici, gelen trafiği bulut hizmetlerindeki sağlıklı hizmet örnekleri veya bir yük dengeleyici kümesindeki sanal makineler arasında dağıtarak yüksek kullanılabilirlik sağlar. Ayrıca, Azure Load Balancer bu hizmetleri birden çok bağlantı noktasında, birden çok IP adresinde ya da her ikisinde birden sağlayabilir.

## <a name="example-deployment-scenario"></a>Örnek dağıtım senaryosu

Aşağıdaki diyagram, bu makalede açıklanan örnek şablonu kullanılarak dağıtılan yük dengeleme çözümlerini göstermektedir.

![Yük dengeleyici senaryosu](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

Bu senaryoda aşağıdaki Azure kaynaklarını oluşturursunuz:

* hem IPv4 hem de IPv6 adresleri atanmış her VM için bir sanal ağ arabirimi
* IPv4 ve IPv6 Genel IP adresi ne kadar internete bakan bir Yük Dengeleyicisi
* kamu VIP'lerini özel uç noktalarına eşlemek için iki yük dengeleme kuralı
* iki VM içeren bir Kullanılabilirlik Kümesi
* iki sanal makine (VM)

## <a name="deploying-the-template-using-the-azure-portal"></a>Azure portalını kullanarak şablonu dağıtma

Bu makalede, [Azure Quickstart Şablonları](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) galerisinde yayınlanan bir şablonbaşvurur. Şablonu galeriden indirebilir veya Azure'daki dağıtımı doğrudan galeriden başlatabilirsiniz. Bu makalede, şablonu yerel bilgisayarınıza indirdiğinizvarsakabul edilir.

1. Azure portalını açın ve Azure aboneliği nde VM'ler ve ağ kaynakları oluşturma izni olan bir hesapla oturum açın. Ayrıca, varolan kaynakları kullanmıyorsanız, hesabın bir kaynak grubu ve depolama hesabı oluşturmak için izni gerekir.
2. Menüden "+Yeni"yi tıklatın ve ardından arama kutusuna "şablon" yazın. Arama sonuçlarından "Şablon dağıtımı"nı seçin.

    ![lb-ipv6-portal-step2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. Her Şey bıçağında "Şablon dağıtımı"na tıklayın.

    ![lb-ipv6-portal-adım3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. "Oluştur"u tıklatın.

    ![lb-ipv6-portal-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. "Şablonu edit" seçeneğini tıklatın. Varolan içeriği silin ve şablon dosyasının tüm içeriğinde kopyalayın/yapıştırın (başlangıç ve bitiş { }'ü eklemek için), ardından "Kaydet"i tıklatın.

    > [!NOTE]
    > Microsoft Internet Explorer kullanıyorsanız, yapıştırdığınızda Windows panosuna erişime izin vermenizi isteyen bir iletişim kutusu alırsınız. "Erişime izin ver"i tıklatın.

    ![lb-ipv6-portal-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. "Parametreleri edit" seçeneğini tıklatın. Parametreler bıçağında, Şablon parametreleri bölümündeki kılavuz başına değerleri belirtin ve parametreler ilerleğini kapatmak için "Kaydet"i tıklatın. Özel Dağıtım bıçağında aboneliğinizi, varolan bir kaynak grubunu seçin veya bir tane oluşturun. Bir kaynak grubu oluşturuyorsanız, kaynak grubu için bir konum seçin. Ardından, **Yasal terimleri**tıklatın, ardından yasal terimler için **Satın Al'ı** tıklatın. Azure kaynakları dağıtmaya başlar. Tüm kaynakları dağıtmak birkaç dakika sürer.

    ![lb-ipv6-portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Bu parametreler hakkında daha fazla bilgi için, bu makalenin ilerleyen bölümlerinde [Şablon parametreleri ve değişkenler](#template-parameters-and-variables) bölümüne bakın.

7. Şablon tarafından oluşturulan kaynakları görmek için Gözat'ı tıklatın ve "Kaynak grupları"nı görene kadar listeyi aşağı kaydırın ve ardından tıklatın.

    ![lb-ipv6-portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Kaynak grupları çubuğunda, adım 6'da belirttiğiniz kaynak grubunun adını tıklatın. Dağıtılan tüm kaynakların listesini görürsünüz. Her şey yolunda giderse, "Son dağıtım" altında "Başarılı" demelidir. Değilse, kullanmakta olduğunuz hesabın gerekli kaynakları oluşturmak için izinleri olduğundan emin olun.

    ![lb-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [!NOTE]
    > 6. adımı tamamladıktan hemen sonra Kaynak Gruplarınıza göz atarsanız, kaynaklar dağıtılırken "Son dağıtım" "Dağıtım" durumunu görüntüler.

9. Kaynak listesinde "myIPv6PublicIP" seçeneğini tıklayın. IP adresi nin altında bir IPv6 adresi olduğunu ve DNS adının adım 6'daki dnsNameforIPv6LbIP parametresi için belirlediğiniz değer olduğunu görürsünüz. Bu kaynak, Internet istemcileri tarafından erişilebilen ortak IPv6 adresi ve ana bilgisayar adıdır.

    ![lb-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Bağlantıyı doğrulama

Şablon başarıyla dağıtıldığında, aşağıdaki görevleri tamamlayarak bağlantıyı doğrulayabilirsiniz:

1. Azure portalında oturum açın ve şablon dağıtımı tarafından oluşturulan VM'lerin her birine bağlanın. Bir Windows Server VM dağıttıysanız, ipconfig /all'ı komut isteminden çalıştırın. VM'lerin hem IPv4 hem de IPv6 adreslerine sahip olduğunu görüyorsunuz. Linux VM'leri dağıttıysanız, Linux dağıtımınız için sağlanan yönergeleri kullanarak dinamik IPv6 adreslerini alacak şekilde Linux işletim sistemi yapılandırmanız gerekir.
2. IPv6 Internet'e bağlı bir istemciden, yük bakiyeleyicisinin genel IPv6 adresine bağlantı başlatın. Yük dengeleyicisinin iki VM arasında denge deşe olduğunu doğrulamak için, Her VM'ye Microsoft Internet Information Services (IIS) gibi bir web sunucusu yükleyebilirsiniz. Her sunucudaki varsayılan web sayfası, benzersiz olarak tanımlamak için "Server0" veya "Server1" metnini içerebilir. Ardından, IPv6 Internet'e bağlı bir istemcide bir Internet tarayıcısı açın ve her VM'ye uçtan uca IPv6 bağlantısını onaylamak için yük bakiyesinin dnsNameforIPv6LbIP parametresi için belirttiğiniz ana stara göz atın. Web sayfasını yalnızca bir sunucudan görüyorsanız, tarayıcı önbelleğinizi temizlemeniz gerekebilir. Birden çok özel tarama oturumu açın. Her sunucudan bir yanıt görmeniz gerekir.
3. IPv4 Internet'e bağlı bir istemciden, yük bakiyeleyicisinin genel IPv4 adresine bağlantı başlatın. Yük dengeleyicisinin iki VM'yi dengeleyen yük olduğunu doğrulamak için, Adım 2'de ayrıntılı olarak belirtildiği gibi IIS kullanarak test edebilirsiniz.
4. Her VM'den, IPv6 veya IPv4'e bağlı bir Internet aygıtına giden bağlantı başlatın. Her iki durumda da, hedef aygıt tarafından görülen kaynak IP, yük dengeleyicisinin genel IPv4 veya IPv6 adresidir.

> [!NOTE]
> Hem IPv4 hem de IPv6 icmp Azure ağında engellenir. Sonuç olarak, ping gibi ICMP araçları her zaman başarısız. Bağlantıyı test etmek için TCPing veya PowerShell Test-NetConnection cmdlet gibi bir TCP alternatifi kullanın. Diyagramda gösterilen IP adreslerinin görebileceğiniz değerlere örnekler olduğunu unutmayın. IPv6 adresleri dinamik olarak atandığından, aldığınız adresler farklı olacaktır ve bölgeye göre değişiklik gösterebilir. Ayrıca, yük dengeleyicisindeki genel IPv6 adresinin arka uç havuzundaki özel IPv6 adreslerinden farklı bir önek ile başlaması yaygındır.

## <a name="template-parameters-and-variables"></a>Şablon parametreleri ve değişkenleri

Azure Kaynak Yöneticisi şablonu, gereksinimlerinize göre özelleştirebileceğiniz birden çok değişken ve parametre içerir. Değişkenler, kullanıcının değiştirmesini istemediğiniz sabit değerler için kullanılır. Parametreler, şablonu dağıtırken bir kullanıcının sağlamasını istediğiniz değerler için kullanılır. Örnek şablon, bu makalede açıklanan senaryo için yapılandırılır. Bunu ortamınızın gereksinimlerine göre özelleştirebilirsiniz.

Bu makalede kullanılan örnek şablon aşağıdaki değişkenleri ve parametreleri içerir:

| Parametre / Değişken | Notlar |
| --- | --- |
| adminUsername |Sanal makinelerde oturum açmak için kullanılan yönetici hesabının adını belirtin. |
| adminPassword |Sanal makinelerde oturum açmak için kullanılan yönetici hesabının parolasını belirtin. |
| dnsNameforIPv4LbIP |Yük bakiyesinin ortak adı olarak atamak istediğiniz DNS ana bilgisayar adını belirtin. Bu ad, yük bakiyesinin genel IPv4 adresine gider. Adı küçük olmalı ve regex maç: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP |Yük bakiyesinin ortak adı olarak atamak istediğiniz DNS ana bilgisayar adını belirtin. Bu ad, yük bakiyesinin genel IPv6 adresine gider. Adı küçük olmalı ve regex maç: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$. Bu, IPv4 adresiyle aynı ad olabilir. Bir istemci bu ad için bir DNS sorgusu gönderdiğinde Azure, ad paylaşıldığında hem A hem de AAAA kayıtlarını döndürür. |
| vmNameÖnek |VM adı önekini belirtin. Şablon, VM'ler oluşturulduğunda ada bir sayı (0, 1, vb.) ekler. |
| nicNamePrefix |Ağ arabirimi adı önekini belirtin. Şablon, ağ arabirimleri oluşturulduğunda ada bir sayı (0, 1, vb.) ekler. |
| storageAccountName |Varolan bir depolama hesabının adını girin veya şablon tarafından oluşturulacak yeni bir hesabın adını belirtin. |
| availabilitySetName |VM'lerle birlikte kullanılacak kullanılabilirlik kümesinin adını girin |
| addressPrefix |Sanal Ağ'ın adres aralığını tanımlamak için kullanılan adres öneki |
| subnetName |VNet için oluşturulan alt netin adı |
| subnetPrefix |Alt netin adres aralığını tanımlamak için kullanılan adres öneki |
| vnetName |VM'ler tarafından kullanılan VNet'in adını belirtin. |
| ipv4PrivateIPAddressType |Özel IP adresi (Statik veya Dinamik) için kullanılan ayırma yöntemi |
| ipv6PrivateIPAddressType |Özel IP adresi (Dinamik) için kullanılan ayırma yöntemi. IPv6 yalnızca Dinamik ayırmayı destekler. |
| numberOfInstances |Şablon tarafından dağıtılan yük dengeli örneklerinin sayısı |
| ipv4PublicIPAddressName |Yük bakiyesinin ortak IPv4 adresiyle iletişim kurmak için kullanmak istediğiniz DNS adını belirtin. |
| ipv4PublicIPAddressType |Genel IP adresi için kullanılan ayırma yöntemi (Statik veya Dinamik) |
| Ipv6PublicIPAdres Adı |Yük bakiyesinin ortak IPv6 adresiyle iletişim kurmak için kullanmak istediğiniz DNS adını belirtin. |
| ipv6PublicIPAddressType |Ortak IP adresi (Dinamik) için kullanılan ayırma yöntemi. IPv6 yalnızca Dinamik ayırmayı destekler. |
| lbName |Yük bakiyesinin adını belirtin. Bu ad portalda görüntülenir veya cli veya PowerShell komutuyla atıfta bulunulurken kullanılır. |

Şablonda kalan değişkenler, Azure kaynakları oluşturduğunda atanan türetilmiş değerleri içerir. Bu değişkenleri değiştirmeyin.

## <a name="next-steps"></a>Sonraki adımlar

Şablondaki bir yük dengeleyicisinin JSON sözdizimi ve özellikleri için [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers)bölümüne bakın.
