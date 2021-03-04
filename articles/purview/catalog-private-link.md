---
title: Purview 'a güvenli erişim için özel uç noktaları kullanın
description: Bu makalede, purview hesabınız için nasıl özel bir uç noktası ayarlayabileceğiniz açıklanmaktadır
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 03/02/2021
ms.openlocfilehash: 3193c5c00789b793a5b5beaf662f94ab9525888a
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102107837"
---
# <a name="use-private-endpoints-for-your-purview-account"></a>Purview hesabınız için özel uç noktaları kullanın

Bir sanal ağdaki (VNet) istemcilerin ve kullanıcıların kataloğa özel bir bağlantı üzerinden güvenli bir şekilde erişmesini sağlamak için, purview hesaplarınız için özel uç noktaları kullanabilirsiniz. Özel uç nokta, purview hesabınız için VNet adres alanından bir IP adresi kullanır. VNet ve purview hesabı arasındaki ağ trafiği, VNet üzerinden ve Microsoft omurga ağındaki özel bir bağlantı üzerinden, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır.

## <a name="create-purview-account-with-a-private-endpoint"></a>Özel bir uç nokta ile purview hesabı oluşturma

1. [Azure Portal](https://portal.azure.com) ve ardından purview hesabınıza gidin.

1. Temel bilgileri doldur ve **ağ** sekmesinde Özel uç noktaya ayarla bağlantı yöntemi. Özel uç noktanızla eşleştirmek istediğiniz **aboneliğin, VNET 'in ve alt ağın** ayrıntılarını sağlayarak Alım özel uç noktalarınızı ayarlayın.

    :::image type="content" source="media/catalog-private-link/create-pe-azure-portal.png" alt-text="Hesap oluşturma sırasında PE oluşturma":::

1. Ayrıca, isteğe bağlı olarak her alma özel uç noktası için bir **özel DNS bölgesi** ayarlamayı seçebilirsiniz.

1. Purview hesabınız için özel bir uç nokta eklemek için Ekle ' ye tıklayın.

1. Özel uç nokta oluştur dikey penceresinde, purview alt kaynağını **Hesap** olarak ayarlayın, Sanal ağınızı ve alt ağınızı SEÇIN ve DNS 'nin kaydedileceği özel DNS bölgeyi seçin (Ayrıca, sanal makinelerinizdeki konak dosyalarını kullanarak kazanılmış DNS sunucularınızı KULLANABILIR veya DNS kayıtları oluşturabilirsiniz).

    :::image type="content" source="media/catalog-private-link/create-pe-account.png" alt-text="Hesap oluşturma sırasında PE oluşturma":::

1. **Tamam**’ı seçin.

1. **Gözden geçir ve oluştur**’u seçin. Azure’ın yapılandırmanızı doğrulayacağı Gözden geçir ve oluştur sayfasına yönlendirilirsiniz.

1. Doğrulama başarılı iletisini gördüğünüzde **Oluştur**’u seçin.

    :::image type="content" source="media/catalog-private-link/validation-passed.png" alt-text="Hesap oluşturma için doğrulama geçildi":::

## <a name="create-a-private-endpoint-for-the-purview-web-portal"></a>Purview Web portalı için özel bir uç nokta oluşturma

1. Yeni oluşturduğunuz purview hesabına gidin, ayarlar bölümünde özel uç nokta bağlantılarını seçin.
    
1. Yeni bir özel uç nokta oluşturmak için + özel uç noktası ' na tıklayın.

    :::image type="content" source="media/catalog-private-link/pe-portal.png" alt-text="Portal özel uç noktası oluştur":::

1. Temel bilgileri girin.

1. Kaynak sekmesinde kaynak türünü **Microsoft. purview/accounts** olacak şekilde seçin.

1. Yeni oluşturulan purview hesabı olacak kaynağı seçin ve **Portal** olacak hedef alt kaynak ' ı seçin.
    :::image type="content" source="media/catalog-private-link/pe-portal-details.png" alt-text="Portal özel uç noktası ayrıntıları":::

1. Yapılandırma sekmesinde sanal ağ ve Özel DNS bölgesini seçin. Özet sayfasına gidin ve Portal özel uç noktasını oluşturmak için **Oluştur** ' a tıklayın.

## <a name="enabling-access-to-azure-active-directory"></a>Azure Active Directory erişimi etkinleştiriliyor

> [!NOTE]
> VM 'niz, VPN Gateway veya VNET eşleme ağ geçidinizin genel internet erişimi içeriyorsa, bu, purview portalına erişebilir ve özel uç noktalarla etkin bir şekilde hesap görüntüleyebilir ve aşağıdaki yönergeleri izlemeniz gerekmez. Özel ağınızda Ağ güvenlik grubu kuralları tüm genel Internet trafiğini reddedecek şekilde ayarlandıysa, AAD erişimini etkinleştirmek için bazı kurallar eklemeniz gerekir. Bunu yapmak için lütfen aşağıdaki yönergeleri izleyin.

Aşağıdaki yönergeler bir Azure VM 'den güvenli bir şekilde erişim için verilmiştir. VPN veya diğer VNET eşleme ağ geçitleri kullanıyorsanız benzer adımların izlenmesi gerekir.

1. Azure portal sanal makinenize gidin, ayarlar bölümünde Ağ sekmesini seçin. Sonra giden bağlantı noktası kuralları ' nı seçip giden bağlantı noktası kuralı Ekle ' ye tıklayın

    :::image type="content" source="media/catalog-private-link/outbound-rule-add.png" alt-text="Giden kuralı ekleme":::

2. Giden bağlantı noktası kuralı ekle dikey penceresinde, hizmet etiketi *, hedef hizmet* etiketi olarak **AzureActiveDirectory**, hedef bağlantı noktası aralığı, izin verilecek işlem, izin verilen **tüm Internet trafiğini reddeden kuraldan daha yüksek** olmalıdır. Kuralı oluşturun.

    :::image type="content" source="media/catalog-private-link/outbound-rule-details.png" alt-text="Giden kuralı ayrıntıları ekleme":::
 
3. '**AzureResourceManager**' hizmet etiketine izin vermek için başka bir kural oluşturmak üzere aynı adımları izleyin. Azure portal erişmeniz gerekiyorsa, '*Azureportal*' hizmet etiketi için de bir kural ekleyebilirsiniz.

4. VM 'ye bağlanın, tarayıcıyı açın, tarayıcı konsoluna gidin (Ctrl + Shift + J) ve ağ isteklerini izlemek için Ağ sekmesine geçin. URL kutusuna web.purview.azure.com girin ve AAD kimlik bilgilerinizi kullanarak oturum açmayı deneyin. Oturum açma muhtemelen başarısız olabilir ve konsolundaki ağ sekmesinde aadcdn.msauth.net erişmeye çalışan ancak engellenmiş hale getirmeye çalışan AAD 'yi görebilirsiniz.

    :::image type="content" source="media/catalog-private-link/login-fail.png" alt-text="Oturum açma başarısız ayrıntıları":::
 
5. Bu durumda, VM 'de bir komut istemi açın ve bu URL 'yi (aadcdn.msauth.net) ping yapın, IP adresini alın ve ardından VM 'nin ağ güvenlik kurallarında IP için bir giden bağlantı noktası kuralı ekleyin. Hedefi IP adresi olarak ayarlayın ve hedef IP adreslerini aadcdn 'nin IP 'si olarak ayarlayın. Yük dengeleyici ve Traffic Manager nedeniyle AAD CDN 'nin IP 'si dinamik olabilir. IP 'sini aldıktan sonra, tarayıcıyı AAD CDN 'yi almak üzere bu IP 'yi ziyaret etmeye zorlamak için VM 'nin ana bilgisayar dosyasına eklenmesi daha iyidir.

    :::image type="content" source="media/catalog-private-link/ping.png" alt-text="Sınama ping":::

    :::image type="content" source="media/catalog-private-link/aadcdn-rule.png" alt-text="AAD CDN kuralı":::
 
6. Yeni kural oluşturulduktan sonra, sanal makineye geri gidin ve AAD kimlik bilgilerinizi kullanarak yeniden oturum açmayı deneyin. Oturum açma işlemi başarılı olursa, purview portalı kullanıma hazırdır. Ancak bazı durumlarda AAD, müşterinin hesap türüne göre oturum açmak için diğer etki alanlarına yönlendirilir. Örneğin, bir live.com hesabı için AAD, oturum açma için live.com 'e yönlendirir, ardından bu istekler yeniden engellenir. Microsoft çalışan hesapları için AAD, oturum açma bilgileri için msft.sts.microsoft.com 'e erişir. Tarayıcı ağı 'ndaki ağ isteklerini denetleme hangi etki alanı isteklerinin engellendiğini görmek için, önceki adımı ekleyerek IP 'ye yönelik isteklere izin vermek üzere ağ güvenlik grubuna IP ve giden bağlantı noktası kuralları ekleyin (mümkünse, DNS çözümlemesini çözmek için VM 'nin ana bilgisayar dosyasına URL ve IP ekleyin). Tam oturum açma etki alanının IP aralıklarını biliyorsanız, onları doğrudan ağ kuralları ' na de ekleyebilirsiniz.

7. Şimdi AAD 'ye oturum açma işlemi başarılı olmalıdır. Purview portalı başarıyla yüklenir, ancak tüm purview hesaplarının listelenmesi yalnızca belirli bir purview hesabına erişebildiğinden çalışmaz. İçin özel bir uç nokta ayarlamış olduğunuz purview hesabını doğrudan ziyaret etmek üzere *Web. purview. Azure. com/Resource/{PurviewAccountName}* girin.

##  <a name="enable-private-endpoint-on-existing-purview-accounts"></a>Mevcut purview hesaplarında özel uç noktayı etkinleştir

Purview hesabınızı oluşturduktan sonra özel uç noktaları eklemek için 2 yol vardır:
- Azure portal kullanma (purview hesabı)
- Özel bağlantı merkezini kullanma

### <a name="using-the-azure-portal-purview-account"></a>Azure portal kullanma (purview hesabı)

1. Azure portal, **Ayarlar**' ın **ağ** bölümünde özel uç nokta bağlantıları ' nı seçin.

:::image type="content" source="media/catalog-private-link/pe-portal.png" alt-text="Portal özel uç noktası oluştur":::
    
2. Yeni bir özel uç nokta oluşturmak için + özel uç noktası ' na tıklayın.

3. Temel bilgileri girin.

4. Kaynak sekmesinde kaynak türünü **Microsoft. purview/accounts** olacak şekilde seçin.

5. Purview hesabı olacak kaynağı seçin ve hedef alt kaynağı **hesaba** göre seçin.

6. Yapılandırma sekmesinde **sanal ağ** ve **özel DNS bölgesini** seçin. Özet sayfasına gidin ve Portal özel uç noktasını oluşturmak için **Oluştur** ' a tıklayın.

> [!NOTE]
> **Portal** olarak seçilen hedef alt kaynak için yukarıdaki adımların aynısını izlemeniz gerekecektir.

### <a name="using-the-private-link-center"></a>Özel bağlantı merkezini kullanma

1. [Azure Portal](https://portal.azure.com)gidin.

2. Sayfanın üst kısmındaki arama çubuğunda, ' özel bağlantı ' araması yapın ve ilk seçeneğe tıklayarak özel bağlantı dikey penceresine gidin.

3. ' + Ekle ' seçeneğine tıklayın ve temel ayrıntıları girin.

    :::image type="content" source="media/catalog-private-link/private-link-center.png" alt-text="Özel bağlantı merkezinden PE oluştur":::

4. Önceden oluşturulmuş bir takip hesabı olacak kaynağı seçin ve hedef alt kaynağı **hesaba** göre seçin.

5. Yapılandırma sekmesinde sanal ağ ve Özel DNS bölgesini seçin. Özet sayfasına gidin ve hesap özel uç noktasını oluşturmak için **Oluştur** ' a tıklayın.

> [!NOTE]
> **Portal** olarak seçilen hedef alt kaynak için yukarıdaki adımların aynısını izlemeniz gerekecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure purview veri kataloğuna gözatın](how-to-browse-catalog.md)
- [Azure purview Veri Kataloğu 'Nda arama](how-to-search-catalog.md)  