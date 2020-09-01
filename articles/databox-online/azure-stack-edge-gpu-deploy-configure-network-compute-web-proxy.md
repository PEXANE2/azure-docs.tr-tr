---
title: Azure portal 'de GPU ile Azure Stack Edge cihazının ağ ayarlarını yapılandırma öğreticisi | Microsoft Docs
description: Azure Stack Edge GPU 'YU dağıtmaya yönelik öğretici, fiziksel cihazınız için ağ, bilgi işlem ağı ve Web proxy ayarlarını yapılandırmanızı sağlar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 08/28/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Azure Stack Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 6e7dbc2b96a53d220554e07228a5e30857d12d9c
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89262993"
---
# <a name="tutorial-configure-network-for-azure-stack-edge-with-gpu"></a>Öğretici: GPU ile Azure Stack Edge için ağı yapılandırma

Bu öğreticide, yerel Web Kullanıcı arabirimini kullanarak Azure Stack Edge cihazınız için ağın, yerleşik bir GPU ile nasıl yapılandırılacağı açıklanmaktadır.

Bağlantı işleminin tamamlanması 20 dakika sürebilir.

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Ağı yapılandırma
> * İşlem ağını etkinleştir
> * Web proxy yapılandırma


## <a name="prerequisites"></a>Ön koşullar

Azure Stack Edge cihazınızı GPU ile yapılandırmadan ve ayarlamadan önce şunları yaptığınızdan emin olun:

* Fiziksel cihazı [yükleme Azure Stack Edge](azure-stack-edge-gpu-deploy-install.md)bölümünde ayrıntılı olarak yüklediniz.
* [Azure Stack Edge 'e bağlanma](azure-stack-edge-gpu-deploy-connect.md) bölümünde açıklandığı gibi cihazın yerel Web Kullanıcı arabirimine bağlandınız


## <a name="configure-network"></a>Ağı yapılandırma

**Başlarken** sayfasında, fiziksel cihazı Azure Stack Edge hizmetine göre yapılandırmak ve kaydetmek için gereken çeşitli ayarlar görüntülenir. 

Cihazınızın ağını yapılandırmak için bu adımları izleyin.

1. Cihazınızın yerel Web Kullanıcı arabiriminde **Başlarken** sayfasına gidin. 

2. **Ağ** kutucuğunda **Yapılandır**' ı seçin.  
    
    ![Yerel Web Kullanıcı arabirimi "ağ ayarları" kutucuğu](./media/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy/network-1.png)

    Fiziksel cihazınızda altı ağ arabirimi vardır. Bağlantı noktası 1 ve bağlantı noktası 2 1 GB/sn ağ arabirimlerdir. Bağlantı noktası 3, bağlantı noktası 4, bağlantı noktası 5 ve bağlantı noktası 6, 10 Gbps ağ arabirimi olarak da kullanılabilen tüm 25 Gbps ağ arabirimlerdir. BAĞLANTı noktası 1 otomatik olarak yalnızca yönetim bağlantı noktası olarak yapılandırılır ve bağlantı noktası 6 ' dan bağlantı noktası 6 ' dan tüm veri bağlantı noktaları bulunur. Yeni bir cihaz için **ağ ayarları** sayfası aşağıda gösterildiği gibidir.
    
    ![Yerel Web Kullanıcı arabirimi "ağ ayarları" sayfası](./media/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy/network-2a.png)


   
3. Ağ ayarlarını değiştirmek için bir bağlantı noktası seçin ve sağ bölmede, IP adresi, alt ağ, ağ geçidi, birincil DNS ve ikincil DNS ' yi değiştirin. 

    - Bağlantı noktası 1 ' i seçerseniz, bunun statik olarak önceden yapılandırılmış olduğunu görebilirsiniz. 

        ![Yerel Web Kullanıcı arabirimi "bağlantı noktası 1 ağ ayarları"](./media/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy/network-3.png)

    - Bağlantı noktası 2, bağlantı noktası 3, bağlantı noktası 4 veya bağlantı noktası 5 ' i seçerseniz, bu bağlantı noktalarının tümü varsayılan olarak DHCP olarak yapılandırılır.

        ![Yerel Web Kullanıcı arabirimi "bağlantı noktası 3 ağ ayarları"](./media/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy/network-4.png)

    Ağ ayarlarını yapılandırırken şunları göz önünde bulundurun:

   * Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır. IP adresi, alt ağ, ağ geçidi ve DNS otomatik olarak atanır.
   * DHCP etkinleştirilmemişse, gerekirse statik IP 'Ler atayabilirsiniz.
   * Ağ arabiriminizi IPv4 olarak yapılandırabilirsiniz.
   * 25 Gbps arabirimlerde, RDMA (uzaktan doğrudan erişim belleği) modunu ıwarp veya RoCE (Yakınsanan Ethernet üzerinden RDMA) olarak ayarlayabilirsiniz. Düşük gecikme sürelerinin birincil gereksinim olduğu ve ölçeklenebilirliğin bir sorun olmadığı durumlarda, RoCE 'yi kullanın. Gecikme süresi bir anahtar gereksinimidir, ancak kullanımı kolaylaştırma ve ölçeklenebilirlik de yüksek önceliklerdir, ıwarp en iyi adaydır.
   * Herhangi bir bağlantı noktasının seri numarası, düğüm seri numarasına karşılık gelir.

    Cihaz ağı yapılandırıldıktan sonra, sayfa aşağıda gösterildiği gibi güncellenir.

    ![Yerel Web Kullanıcı arabirimi "ağ ayarları" sayfası](./media/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy/network-2.png)


     >[!NOTE]
     >
     > * Cihaza bağlanmak için başka bir IP adresiniz yoksa, ağ arabiriminin yerel IP adresini static 'ten DCHP 'e geçmenizi öneririz. Bir ağ arabirimi kullanılıyorsa ve DHCP 'ye geçerseniz, DHCP adresini belirlemenin bir yolu yoktur. Bir DHCP adresine geçiş yapmak istiyorsanız, cihaz hizmetle etkinleştirilinceye kadar bekleyin ve ardından değiştirin. Daha sonra, hizmetinize yönelik Azure portal **cihaz özelliklerindeki** tüm bağdaştırıcıların IP 'lerini görüntüleyebilirsiniz.


    Ağ ayarlarını yapılandırdıktan ve uyguladıktan sonra, **kullanmaya**başlamak için geri dönün.

## <a name="enable-compute-network"></a>İşlem ağını etkinleştir

İşlem ağını etkinleştirmek ve yapılandırmak için bu adımları izleyin.

1. Cihazınızın yerel Web Kullanıcı arabiriminde **Başlarken** sayfasına gidin. **Ağ** kutucuğunda **işlem ağı**' nı seçin.  

    ![Yerel Kullanıcı arabirimindeki işlem sayfası](./media/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy/compute-network-1.png)

2. **İşlem** sayfasında, işlem için etkinleştirmek istediğiniz bir ağ arabirimi seçin. 

    ![Yerel Kullanıcı arabirimindeki işlem sayfası](./media/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy/compute-network-2.png)

2. **Ağ ayarları** Iletişim kutusunda **Etkinleştir**' i seçin. İşlem ' ı etkinleştirdiğinizde, cihazınızda bu ağ arabirimindeki bir sanal anahtar oluşturulur. Sanal anahtar, cihazdaki işlem altyapısı için kullanılır. 
    
3. **Kubernetes düğüm IP 'leri**atayın. Bu statik IP adresleri, işlem sanal makinesi içindir. 

    *N*düğümlü bir cihaz için, başlangıç ve bitiş IP adreslerini kullanan işlem VM 'si için en az *n + 1* IPv4 adresi (veya daha fazla) bulunur. Verilen Azure Stack Edge 1 düğümlü bir cihazdır, en az 2 bitişik IPv4 adresi sağlanır. 

4. **Kubernetes dış hizmet IP 'leri**atayın. Bunlar ayrıca Yük Dengeleme IP adresleridir. Bu bitişik IP adresleri, Kubernetes kümesi dışında kullanıma sunmak istediğiniz hizmetlere yöneliktir ve sunulan hizmet sayısına bağlı olarak statik IP aralığını belirlersiniz. 
    
    > [!IMPORTANT]
    > İşlem modüllerine erişmek için Azure Stack Edge hub hizmeti için en az 1 IP adresi belirtmenizi önemle tavsiye ederiz. Daha sonra isteğe bağlı olarak, küme dışından erişilmesi gereken diğer hizmetler/IoT Edge modülleri (hizmet/modül başına 1) için ek IP adresleri belirtebilirsiniz. Hizmet IP adresleri daha sonra güncelleştirilebilen olabilir. 
    
5. **Uygula**’yı seçin.

    ![Yerel Kullanıcı arabirimindeki işlem sayfası](./media/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy/compute-network-3.png)


## <a name="configure-web-proxy"></a>Web proxy yapılandırma

Bu, isteğe bağlı bir yapılandırmadır.

> [!IMPORTANT]
> * İşlem ' ı etkinleştirip Azure Stack Edge cihazınızda IoT Edge modülünü kullanırsanız, Web proxy kimlik doğrulamasını **yok**olarak ayarlamanızı öneririz. NTLM desteklenmiyor.
>* Proxy-otomatik yapılandırma (PAC) dosyaları desteklenmez. PAC dosyası, Web tarayıcılarının ve diğer Kullanıcı aracılarının belirli bir URL 'YI getirmek için uygun proxy sunucusunu (erişim yöntemi) otomatik olarak nasıl seçebileceğini tanımlar. Ara sunucunun sertifikası güvenilir olmadığından, tüm trafiği kesmeye ve okumaya çalışır (sonra her şeyi kendi sertifikalarıyla yeniden imzala) uyumlu değildir. Genellikle saydam proxy 'ler Azure Stack Edge ile iyi çalışır. Saydam olmayan Web proxy 'leri desteklenmez.

1. Cihazınızın yerel Web Kullanıcı arabiriminde **Başlarken** sayfasına gidin.
2. **Ağ** kutucuğunda Web proxy sunucusu ayarlarınızı yapılandırın. Web proxy yapılandırması isteğe bağlı olsa da, bir Web Proxy kullanıyorsanız, bunu yalnızca bu sayfada yapılandırabilirsiniz.

   ![Yerel Web Kullanıcı arabirimi "Web proxy ayarları" sayfası](./media/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy/web-proxy-1.png)

3. **Web proxy ayarları** sayfasında, aşağıdaki adımları uygulayın:

    1. **Web proxy URL 'si** kutusuna URL 'yi şu biçimde girin: `http://host-IP address or FQDN:Port number` . HTTPS URL 'Leri desteklenmez.

    2. **Kimlik doğrulaması**altında **hiçbiri** veya **NTLM**' yi seçin. İşlem ' ı etkinleştirip Azure Stack Edge cihazınızda IoT Edge modülünü kullanacaksanız, Web proxy kimlik doğrulamasını **none**olarak ayarlamanızı öneririz. **NTLM** desteklenmiyor.

    3. Kimlik doğrulaması kullanıyorsanız, bir Kullanıcı adı ve parola girin.

    4. Yapılandırılmış Web proxy ayarlarını doğrulamak ve uygulamak için **Uygula**' yı seçin.
    
   ![Yerel Web Kullanıcı arabirimi "Web proxy ayarları" sayfası](./media/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy/web-proxy-2.png)

4. Ayarlar uygulandıktan sonra, **kullanmaya**başlamak için geri dönün.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Ön koşullar
> * Ağı yapılandırma
> * İşlem ağını etkinleştir
> * Web proxy yapılandırma


Azure Stack Edge cihazınızı ayarlama hakkında bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Cihaz ayarlarını yapılandırma](./azure-stack-edge-gpu-deploy-set-up-device-update-time.md)
