---
title: Hızlı başlangıç-Azure portal kullanarak özel bir bağlantı hizmeti oluşturma
titlesuffix: Azure Private Link
description: Bu hızlı başlangıçta Azure portal kullanarak özel bir bağlantı hizmeti oluşturmayı öğrenin
services: private-link
author: asudbring
ms.service: private-link
ms.topic: quickstart
ms.date: 01/18/2021
ms.author: allensu
ms.openlocfilehash: d394a475c5121607f70c03437382e104a5d0cbee
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98746416"
---
# <a name="quickstart-create-a-private-link-service-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak özel bir bağlantı hizmeti oluşturma

Hizmetinize başvuran bir özel bağlantı hizmeti oluşturmaya başlayın.  Azure Standart Load Balancer arkasında dağıtılan hizmetinize veya kaynağa özel bağlantı erişimi verin.  Hizmetinizin kullanıcılarının sanal ağından özel erişimi vardır.

## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-an-internal-load-balancer"></a>İç yük dengeleyici oluşturma

Bu bölümde, bir sanal ağ ve bir iç Azure Load Balancer oluşturacaksınız.

### <a name="virtual-network"></a>Sanal ağ

Bu bölümde, özel bağlantı hizmetinize erişen yük dengeleyiciyi barındırmak için bir sanal ağ ve alt ağ oluşturursunuz.

1. Ekranın sol üst kısmındaki **Kaynak oluştur > Ağ > Sanal ağ** seçeneğini belirleyin veya arama kutusuna **Sanal ağ** yazarak arama yapın.

2. **Sanal ağ oluştur**' da, **temel** bilgiler sekmesinde bu bilgileri girin veya seçin:

    | **Ayar**          | **Değer**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Proje ayrıntıları**  |                                                                 |
    | Abonelik     | Azure aboneliğinizi seçin                                  |
    | Kaynak Grubu   | **Createprılinkservice-RG** seçin |
    | **Örnek ayrıntıları** |                                                                 |
    | Name             | **Myvnet** girin                                    |
    | Region           | **Doğu ABD 2** seçin |

3. **IP adresleri** sekmesini seçin veya sayfanın altındaki **Sonraki: IP adresleri** düğmesini seçin.

4. **IP adresleri** sekmesinde şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | IPv4 adres alanı | **10.1.0.0/16** girin |

5. **Alt ağ adı** altında, **varsayılan** sözcük ' ı seçin.

6. **Alt ağı Düzenle**' de şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | Alt ağ adı | **Mysubnet** girin |
    | Alt ağ adres aralığı | **10.1.0.0/24** girin |

7. **Kaydet**’i seçin.

8. **Gözden geçir + oluştur** sekmesini seçin ya da **gözden geçir + oluştur** düğmesini seçin.

9. **Oluştur**’u seçin.

### <a name="create-a-standard-load-balancer"></a>Standart yük dengeleyici oluşturma

Standart bir iç yük dengeleyici oluşturmak için portalını kullanın. 

1. Ekranın sol üst kısmında, **kaynak oluştur**  >  **ağ**  >  **Load Balancer**' ı seçin.

2. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde, aşağıdaki bilgileri girin veya seçin: 

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Önceki adımda **Createprılinkservice-RG** oluşturma seçeneğini belirleyin.|
    | Name                   | **Myloadbalancer** girin                                   |
    | Region         | **Doğu ABD 2**’yi seçin.                                        |
    | Tür          | **Dahili**' ı seçin.                                        |
    | SKU           | **Standart** seçin |
    | Sanal ağ | Önceki adımda oluşturulan **Myvnet** öğesini seçin. |
    | Alt ağ  | Önceki adımda oluşturulan **Mysubnet** öğesini seçin. |
    | IP adresi ataması | **Dinamik**' i seçin. |
    | Kullanılabilirlik alanı | **Bölge yedekli** seçeneğini belirleyin |

3. Geri kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin.

4. **Gözden geçir + oluştur** sekmesinde **Oluştur**' u seçin.   

## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde şunları yapılandırırsınız:

* Arka uç adres havuzu için yük dengeleyici ayarları.
* Bir sistem durumu araştırması.
* Yük dengeleyici kuralı.

### <a name="create-a-backend-pool"></a>Arka uç havuzu oluşturma

Arka uç adres havuzu, yük dengeleyiciye bağlı sanal (NIC) IP adreslerini içerir. 

Yük Dengeleme internet trafiği için sanal makineleri dahil etmek üzere **Mybackendpool** arka uç adres havuzunu oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar** altında **arka uç havuzları**' nı seçin ve ardından **Ekle**' yi seçin.

3. **Arka uç Havuzu Ekle** sayfasında, ad için, arka uç havuzunuzun adı olarak **mybackendpool** yazın ve ardından **Ekle**' yi seçin.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyici, uygulamanızın durumunu bir sistem durumu araştırmasıyla izler. 

Sistem durumu araştırması, sistem durumu denetimlerine yönelik yanıtlarını temel alarak yük dengeleyiciden VM 'Ler ekler veya kaldırır. 

Sanal makinelerin durumunu izlemek için **myHealthProbe** adlı bir durum araştırması oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar** altında **sistem durumu araştırmaları**' nı ve ardından **Ekle**' yi seçin.
    
    | Ayar | Değer |
    | ------- | ----- |
    | Ad | **Myhealtharaştırması** girin. |
    | Protokol | **TCP**’yi seçin. |
    | Bağlantı noktası | **80** girin.|
    | Aralık | Yoklama denemeleri arasındaki saniye cinsinden **Aralık** sayısı için **15** girin. |
    | İyi durumda olmayan durum eşiği | Bir VM sağlıksız kabul edilmeden önce gerçekleşmesi gereken **sağlıksız eşik** veya arka arkaya araştırma hatası sayısı için **2** ' yi seçin.|
    | | |

3. Rest varsayılanlarını bırakın ve **Tamam**' ı seçin.

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Trafiği almak için gelen trafik için ön uç IP yapılandırmasını ve arka uç IP havuzunu tanımlarsınız. Kaynak ve hedef bağlantı noktası kuralda tanımlanmıştır. 

Bu bölümde, bir yük dengeleyici kuralı oluşturacaksınız:

* Adlandırılmış **Myhttprule**.
* **Loadbalancerön uç** adında.
* **80 numaralı bağlantı noktasını** dinliyor.
* Yük dengeli trafiği, **80 numaralı bağlantı noktasında** **Mybackendpool** adlı arka uca yönlendirir.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar** bölümünde **Yük Dengeleme kuralları**' nı ve ardından **Ekle**' yi seçin.

3. Yük Dengeleme kuralını yapılandırmak için şu değerleri kullanın:
    
    | Ayar | Değer |
    | ------- | ----- |
    | Ad | **Myhttprule** girin. |
    | IP sürümü | **IPv4** seçin |
    | Ön uç IP adresi | **Loadbalancerön uç** seçin |
    | Protokol | **TCP**’yi seçin. |
    | Bağlantı noktası | **80** girin.|
    | Arka uç bağlantı noktası | **80** girin. |
    | Arka uç havuzu | **Mybackendpool** öğesini seçin.|
    | Durum yoklaması | **Myhealtharaştırması**' ni seçin. |
    | Boşta kalma zaman aşımı (dakika) | Kaydırıcıyı **15** dakikaya taşıyın. |
    | TCP sıfırlaması | **Etkin**'i seçin. |

4. Kalan varsayılan değerleri bırakın ve **Tamam**' ı seçin.

## <a name="create-a-private-link-service"></a>Özel bağlantı hizmeti oluşturma

Bu bölümde, standart bir yük dengeleyicinin arkasında bir özel bağlantı hizmeti oluşturacaksınız.

1. Azure portal sayfanın sol üst kısmında **kaynak oluştur**' u seçin.

2. **Market 'Te arama** yapın kutusunda **özel bağlantı** araması yapın.

3. **Oluştur**’u seçin.

4. **Özel bağlantı merkezi** **' nin altında,** mavi **özel bağlantı oluştur** düğmesini seçin.

5. **Özel bağlantı hizmeti oluştur** altındaki **temel bilgiler** sekmesinde, aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** |  |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak Grubu | **Createprılinkservice-RG** öğesini seçin. |
    | **Örnek ayrıntıları** |  |
    | Name | **Myprivatelinkservice** yazın. |
    | Region | **Doğu ABD 2**’yi seçin. |

6. **Giden ayarları** sekmesini seçin veya sonraki: sayfanın en altındaki **giden ayarlar** ' ı seçin.

7. **Giden ayarları** sekmesinde, aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Yük dengeleyici | **Myloadbalancer** öğesini seçin. |
    | Yük dengeleyici ön uç IP adresi | **Loadbalancerön uç (10.1.0.4)** öğesini seçin. |
    | Kaynak NAT alt ağı | **Mysubnet (10.1.0.0/24)** seçeneğini belirleyin. |
    | TCP proxy v2 'yi etkinleştir | Varsayılan değer olan **Hayır** olarak bırakın. </br> Uygulamanız bir TCP proxy v2 üstbilgisi bekliyorsa, **Evet**' i seçin. |
    | **Özel IP adresi ayarları** |  |
    | Varsayılan ayarları bırak |  |

8. **Erişim güvenliği** sekmesini seçin veya Ileri ' yi seçin: sayfanın en altındaki **güvenlik erişimi** .

9. **Rol tabanlı erişim denetimi varsayılanını yalnızca** **erişim güvenliği** sekmesinde bırakın.

10. **Etiketler** sekmesini seçin veya sayfanın altındaki **Etiketler** ' i seçin.

11. **Gözden geçir + oluştur** sekmesini seçin ya da Ileri ' yi seçin. sayfanın alt kısmındaki **İnceleme + oluştur** ' u seçin.

12. **Gözden geçir + oluştur** sekmesinde **Oluştur** ' u seçin.

Özel bağlantı hizmetiniz oluşturulur ve trafik alabilir. Trafik akışlarını görmek istiyorsanız uygulamanızı standart yük dengeleyicinizin arkasında yapılandırın.


## <a name="create-private-endpoint"></a>Özel uç nokta oluştur

Bu bölümde özel bağlantı hizmetini özel bir uç nokta ile eşlersiniz. Bir sanal ağ, özel bağlantı hizmetinin özel uç noktasını içerir. Bu sanal ağ, özel bağlantı hizmetinize erişecek kaynakları içerir.

### <a name="create-private-endpoint-virtual-network"></a>Özel uç nokta sanal ağı oluştur

1. Ekranın sol üst kısmındaki **Kaynak oluştur > Ağ > Sanal ağ** seçeneğini belirleyin veya arama kutusuna **Sanal ağ** yazarak arama yapın.

2. **Sanal ağ oluştur**' da, **temel** bilgiler sekmesinde bu bilgileri girin veya seçin:

    | **Ayar**          | **Değer**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Proje ayrıntıları**  |                                                                 |
    | Abonelik     | Azure aboneliğinizi seçin                                  |
    | Kaynak Grubu   | **Createprılinkservice-RG** seçin |
    | **Örnek ayrıntıları** |                                                                 |
    | Name             | **Myvnetpe** girin                                    |
    | Region           | **Doğu ABD 2** seçin |

3. **IP adresleri** sekmesini seçin veya sayfanın altındaki **Sonraki: IP adresleri** düğmesini seçin.

4. **IP adresleri** sekmesinde şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | IPv4 adres alanı | **11.1.0.0/16** girin |

5. **Alt ağ adı** altında, **varsayılan** sözcük ' ı seçin.

6. **Alt ağı Düzenle**' de şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | Alt ağ adı | **Mysubnetpe** girin |
    | Alt ağ adres aralığı | **11.1.0.0/24** girin |

7. **Kaydet**’i seçin.

8. **Gözden geçir + oluştur** sekmesini seçin ya da **gözden geçir + oluştur** düğmesini seçin.

9. **Oluştur**’u seçin.

### <a name="create-private-endpoint"></a>Özel uç nokta oluştur

1. Portaldaki ekranın sol üst kısmında, **kaynak oluştur**  >  **ağ**  >  **özel bağlantısı**' nı seçin veya arama kutusuna **özel bağlantı** girin.

2. **Oluştur**’u seçin.

3. **Özel bağlantı merkezi**'nde, sol taraftaki menüden **Özel uç noktalar** ' ı seçin.

4. **Özel uç noktalarında** **+ Ekle**' yi seçin.

5. **Özel uç nokta oluştur**' un **temel bilgiler** sekmesinde, girin veya bu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Createprılinkservice-RG** öğesini seçin. Bu kaynak grubunu önceki bölümde oluşturdunuz.|
    | **Örnek ayrıntıları** |  |
    | Name  | **myPrivateEndpoint** değerini girin. |
    | Region | **Doğu ABD 2**’yi seçin. |

6. Sayfanın alt kısmındaki **kaynak** sekmesini veya **Sonraki: kaynak** düğmesini seçin.
    
7. **Kaynak** bölümünde, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Bağlantı yöntemi | **Dizinimde bir Azure kaynağına bağlan '** ı seçin. |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak türü | **Microsoft. Network/privateLinkServices** öğesini seçin. |
    | Kaynak | **Myprivatelinkservice** öğesini seçin. |

8. Ekranın alt kısmındaki **yapılandırma** sekmesini veya **Sonraki: yapılandırma** düğmesini seçin.

9. **Yapılandırma**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Ağ** |  |
    | Sanal Ağ | **Myvnetpe**' yi seçin. |
    | Alt ağ | **Mysubnetpe**' yi seçin. |

10. Ekranın alt kısmındaki **gözden geçir + oluştur** sekmesini veya **gözden geçir + oluştur** düğmesini seçin.

11. **Oluştur**’u seçin.

### <a name="ip-address-of-private-endpoint"></a>Özel uç noktanın IP adresi

Bu bölümde, yük dengeleyici ve özel bağlantı hizmetine karşılık gelen özel uç noktanın IP adresini bulacaksınız.

1. Azure portal sol taraftaki sütununda **kaynak grupları**' nı seçin.

2. **Createprılinkservice-RG** kaynak grubunu seçin.

3. **Createprılinkservice-RG** kaynak grubunda, **Myprivateendpoint** öğesini seçin.

4. **Myprivateendpoint** **genel bakış** sayfasında, Özel uç noktayla ilişkili ağ arabiriminin adını seçin.  Ağ arabirimi adı **Myprivateendpoint. NIC** ile başlar.

5. Özel uç nokta NIC 'in **genel bakış** sayfasında, uç noktanın IP adresı **özel IP adresi** olarak görüntülenir.
    

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel bağlantı hizmetini kullanarak işiniz bittiğinde, bu hızlı başlangıçta kullanılan kaynakları temizlemek için kaynak grubunu silin.

1. Portalın üst kısmındaki arama kutusuna **Createprılinkservice-RG** yazın ve arama sonuçlarından **createprılinkservice-RG** ' yi seçin.
1. **Kaynak grubunu sil**'i seçin.
1. **Kaynak grubu adını yazın** alanına **Createprılinkservice-RG** girin.
1. **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta:

* Bir sanal ağ ve dahili Azure Load Balancer oluşturulur.
* Özel bir bağlantı hizmeti oluşturuldu.
* Özel bağlantı hizmeti için bir sanal ağ ve özel uç nokta oluşturuldu.

Azure özel uç noktası hakkında daha fazla bilgi edinmek için devam edin:
> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure portal kullanarak özel uç nokta oluşturma](create-private-endpoint-portal.md)