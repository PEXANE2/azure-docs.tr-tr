---
title: Azure Uygulama Ağ Geçidi Oluşturma - Azure klasik CLI
description: Kaynak Yöneticisi'nde Azure klasik CLI'yi kullanarak Uygulama Ağ Geçidi oluşturmayı öğrenin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 4/15/2019
ms.author: victorh
ms.openlocfilehash: ed7bbbf55956d655115b94c8ac46432b3e36c49b
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81312640"
---
# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Azure CLI'yi kullanarak bir uygulama ağ geçidi oluşturma

Azure Application Gateway, bir katman 7 yük dengeleyicidir. Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar. Uygulama ağ geçidi aşağıdaki uygulama teslim özelliklerine sahiptir: HTTP yük dengeleme, çerez tabanlı oturum yakınlığı ve Daha önce Güvenli Soketler Katmanı (SSL), boşaltma, özel sağlık probları ve çoklu site desteği olarak bilinen Taşıma Katmanı Güvenliği (TLS).

## <a name="prerequisite-install-the-azure-cli"></a>Ön koşul: Azure CLI'yi yükleyin

Bu makaledeki adımları gerçekleştirmek için [Azure CLI'yi yüklemeniz](../xplat-cli-install.md) ve [Azure'da oturum açmanız](/cli/azure/authenticate-azure-cli)gerekir. 

> [!NOTE]
> Azure hesabınız yoksa, hesabınız gerekir. [Buradaki ücretsiz deneme sürümüyle](../active-directory/fundamentals/sign-up-organization.md) kaydolun.

## <a name="scenario"></a>Senaryo

Bu senaryoda, Azure portalını kullanarak bir uygulama ağ geçidi oluşturmayı öğrenirsiniz.

Bu senaryo şunları yapacaktır:

* İki örnekli orta uygulama ağ geçidi oluşturun.
* 10.0.0.0/16'lık ayrılmış CIDR bloğuyla ContosoVNET adında bir sanal ağ oluşturun.
* CIDR bloğu olarak 10.0.0.0/28 kullanan subnet01 adlı bir alt ağ oluşturun.

> [!NOTE]
> Özel sistem durumu sondaları, arka uç havuzu adresleri ve uygulama ağ geçidi ilk dağıtım sırasında değil, yapılandırıldıktan sonra ek kurallar da dahil olmak üzere uygulama ağ geçidinin ek yapılandırması yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Azure Application Gateway kendi alt netini gerektirir. Sanal ağ oluştururken, birden çok alt ağa sahip olmak için yeterli adres alanı bıraktığından emin olun. Bir alt ağa uygulama ağ geçidi dağıttıktan sonra, alt ağa yalnızca ek uygulama ağ geçitleri eklenebilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Microsoft **Azure Komut İstemi'ni**açın ve oturum açın.

```azurecli-interactive
az login
```

Önceki örneği yazdıktan sonra, bir kod sağlanır. Oturum https://aka.ms/devicelogin açma işlemine devam etmek için tarayıcıda gidin.

![cmd gösteren cihaz girişi][1]

Tarayıcıya, aldığınız kodu girin. Oturum açma sayfasına yönlendirilirsiniz.

![kod girmek için tarayıcı][2]

Kod girildikten sonra oturum açmış olduğunuz da, senaryoya devam etmek için tarayıcıyı kapatın.

![başarılı bir şekilde oturum][3]

## <a name="switch-to-resource-manager-mode"></a>Kaynak Yöneticisi Moduna Geç

```azurecli-interactive
azure config mode arm
```

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Uygulama ağ geçidini oluşturmadan önce, uygulama ağ geçidini içerecek bir kaynak grubu oluşturulur. Aşağıda, komut gösterilmektedir.

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Kaynak grubu oluşturulduktan sonra, uygulama ağ geçidi için sanal bir ağ oluşturulur.  Aşağıdaki örnekte, adres alanı önceki senaryo notlarında tanımlandığı gibi 10.0.0.0/16 olarak tanımlanmıştır.

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a>Alt ağ oluşturma

Sanal ağ oluşturulduktan sonra, uygulama ağ geçidi için bir alt ağ eklenir.  Uygulama ağ geçidiyle aynı sanal ağda barındırılan bir web uygulamasıyla uygulama ağ geçidi kullanmayı planlıyorsanız, başka bir alt ağ için yeterli alan bıraktığından emin olun.

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Sanal ağ ve alt ağ oluşturulduktan sonra, uygulama ağ geçidi nin ön koşulları tamamlanır. Ayrıca, daha önce dışa aktarılan .pfx sertifikası ve sertifikanın şifresi aşağıdaki adım için gereklidir: Arka uç için kullanılan IP adresleri arka uç sunucunuzun IP adresleridir. Bu değerler, sanal ağdaki özel IP'ler, genel ip'ler veya arka uç sunucularınız için tam nitelikli alan adları olabilir.

```azurecli-interactive
azure network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group ContosoRG \
--vnet-name ContosoVNET \
--subnet-name subnet01 \
--servers 134.170.185.46,134.170.188.221,134.170.185.50 \
--capacity 2 \
--sku-tier Standard \
--routing-rule-type Basic \
--frontend-port 80 \
--http-settings-cookie-based-affinity Enabled \
--http-settings-port 80 \
--http-settings-protocol http \
--frontend-port http \
--sku-name Standard_Medium
```

> [!NOTE]
> Oluşturma sırasında sağlanabilecek parametrelerin listesi için aşağıdaki komutu çalıştırın: **azure ağ uygulaması ağ geçidi oluşturma --yardım**.

Bu örnek, dinleyici, arka uç havuzu, arka uç http ayarları ve kuralları için varsayılan ayarları içeren temel bir uygulama ağ geçidi oluşturur. Sağlama başarılı olduğunda bu ayarları dağıtımınıza uyacak şekilde değiştirebilirsiniz.
Web uygulamanız önceden belirtilen adımlarda arka uç havuzuyla tanımlandıysa, oluşturulduktan sonra yük dengeleme başlar.

## <a name="next-steps"></a>Sonraki adımlar

[Özel bir sistem durumu sondası oluştur'u](application-gateway-create-probe-portal.md) ziyaret ederek özel sistem sondaları oluşturmayı öğrenin

TLS Boşaltma'yı nasıl yapılandıracağınızı ve [TLS Boşaltmayı Yapılandırma'yı](application-gateway-ssl-arm.md) ziyaret ederek pahalı TLS şifre çözmeyi web sunucularınızdan nasıl kaldıracağınızı öğrenin

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
