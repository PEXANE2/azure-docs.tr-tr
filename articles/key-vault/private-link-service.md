---
title: Azure özel bağlantı hizmeti ile tümleştirme
description: Azure Key Vault Azure özel bağlantı hizmeti ile tümleştirme hakkında bilgi edinin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 01/28/2020
ms.service: key-vault
ms.topic: quickstart
ms.openlocfilehash: e058e643f4c37336f09b43c41cd09aa361a23d15
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76907078"
---
# <a name="integrate-key-vault-with-azure-private-link-preview"></a>Key Vault Azure özel bağlantısıyla tümleştirin (Önizleme)

Azure özel bağlantı hizmeti, Azure hizmetlerine (örneğin, Azure Key Vault, Azure depolama ve Azure Cosmos DB) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki özel bir uç nokta üzerinden erişmenizi sağlar.

Azure özel uç noktası, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Tüm hizmet trafiği özel uç nokta aracılığıyla yönlendirilebilir. Bu nedenle hiçbir ağ geçidi, NAT cihazı, ExpressRoute veya VPN bağlantısı ya da genel IP adresi gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağı örneğine bağlanarak, erişim denetimi için en yüksek düzeyde ayrıntı düzeyi sağlayabilirsiniz.

Daha fazla bilgi için bkz. [Azure özel bağlantısı (Önizleme) nedir?](../private-link/private-link-overview.md)

## <a name="prerequisites"></a>Ön koşullar

Bir anahtar kasasını Azure özel bağlantısı (Önizleme) ile bütünleştirmek için şunlar gerekir:

- Bir Anahtar Kasası.
- Bir Azure sanal ağı.
- Sanal ağdaki bir alt ağ.
- Hem Anahtar Kasası hem de sanal ağ için sahip veya katkıda bulunan izinleri.

Özel uç noktanız ve sanal ağınız aynı bölgede olmalıdır. Portalı kullanarak özel uç nokta için bir bölge seçtiğinizde bu, yalnızca o bölgedeki sanal ağları otomatik olarak filtreleyecek. Anahtar kasanızın farklı bir bölgede olması olabilir.

Özel uç noktanız sanal ağınızda özel bir IP adresi kullanıyor.

## <a name="establish-a-private-link-connection-to-key-vault"></a>Anahtar Kasası ile özel bağlantı kurun

İlk olarak, [Azure Portal kullanarak sanal ağ oluşturma](../virtual-network/quick-create-portal.md) bölümündeki adımları izleyerek bir sanal ağ oluşturun

Daha sonra yeni bir Anahtar Kasası oluşturabilir veya var olan bir anahtar kasası ile özel bağlantı kurabilirsiniz.

### <a name="create-a-new-key-vault-and-establish-a-private-link-connection"></a>Yeni bir Anahtar Kasası oluşturma ve özel bağlantı bağlantısı kurma

[Azure Key Vault Azure Portal kullanarak bir gizli anahtarı ayarlama ve alma](quick-create-portal.md) bölümündeki adımları izleyerek yeni bir Anahtar Kasası oluşturabilirsiniz

Anahtar Kasası temel bilgilerini yapılandırdıktan sonra, ağ sekmesini seçin ve şu adımları izleyin:

1. Ağ sekmesinde Özel uç nokta (Önizleme) radyo düğmesini seçin.
1. Özel uç nokta eklemek için "+ Ekle" düğmesine tıklayın.

    ![Resim](./media/private-link-service-1.png)
 
1. Özel uç nokta oluştur dikey penceresinin "konum" alanında, sanal ağınızın bulunduğu bölgeyi seçin. 
1. "Ad" alanında, bu özel uç noktayı tanımlamanızı sağlayacak açıklayıcı bir ad oluşturun. 
1. Bu özel bitiş noktasının, açılan menüden oluşturulmasını istediğiniz sanal ağı ve alt ağı seçin. 
1. "Özel bölge DNS ile tümleştirin" seçeneğini değiştirmeden bırakın.  
1. "Tamam" ı seçin.

    ![Resim](./media/private-link-service-2.png)
 
Artık yapılandırılmış özel uç noktayı görebileceksiniz. Artık bu özel uç noktayı silme ve düzenleme seçeneğiniz vardır. "Gözden geçir + oluştur" düğmesini seçin ve Anahtar Kasası oluşturun. Dağıtımın tamamlanması 5-10 dakika sürer. 

### <a name="establish-a-private-link-connection-to-an-existing-key-vault"></a>Mevcut bir anahtar kasası ile özel bağlantı kurun

Zaten bir anahtar kasanız varsa, aşağıdaki adımları izleyerek bir özel bağlantı bağlantısı oluşturabilirsiniz:

1. Azure Portal’da oturum açın. 
1. Arama çubuğuna "Anahtar Kasası" yazın
1. Özel uç nokta eklemek istediğiniz listeden anahtar kasasını seçin.
1. Ayarlar altında "ağ" sekmesini seçin
1. Sayfanın üst kısmındaki özel uç nokta bağlantıları (Önizleme) sekmesini seçin
1. Sayfanın üst kısmındaki "+ özel uç nokta" düğmesini seçin.

    ![resim](./media/private-link-service-3.png) ![görüntü](./media/private-link-service-4.png)

Bu dikey pencereyi kullanarak, içindeki herhangi bir Azure kaynağı için özel bir uç nokta oluşturmayı tercih edebilirsiniz. Açılır menüleri kullanarak bir kaynak türü seçebilir ve dizininizdeki bir kaynağı seçebilir ya da kaynak KIMLIĞI kullanarak herhangi bir Azure kaynağına bağlanabilirsiniz. "Özel bölge DNS ile tümleştirin" seçeneğini değiştirmeden bırakın.  

![resim](./media/private-link-service-3.png)
![görüntü](./media/private-link-service-4.png)

## <a name="manage-private-link-connection"></a>Özel bağlantı bağlantısını Yönet

Özel bir uç nokta oluşturduğunuzda bağlantının onaylanması gerekir. Özel bir uç noktası oluşturduğunuz kaynak dizininizdeki ise, yeterli izinlere sahip olduğunuz için bağlantı isteğini onaylayabileceksiniz; başka bir dizindeki bir Azure kaynağına bağlanıyorsanız, bu kaynağın sahibinin bağlantı isteğinizi onaylamasını beklemeniz gerekir.

Dört sağlama durumu vardır:

| Hizmet eylemi sağla | Hizmet tüketicisi özel uç nokta durumu | Açıklama |
|--|--|--|
| Hiçbiri | Beklemede | Bağlantı el ile oluşturulur ve özel bağlantı kaynağı sahibinden onay bekliyor. |
| Onaylama | Onaylandı | Bağlantı otomatik olarak veya el ile onaylandı ve kullanılabilir hale gelmiştir. |
| Reddet | Reddedilecek | Bağlantı, özel bağlantı kaynağı sahibi tarafından reddedildi. |
| Kaldır | Bağlantısı kesildi | Bağlantı, özel bağlantı kaynağı sahibi tarafından kaldırıldı, Özel uç nokta bilgilendirici hale gelir ve temizlik için silinmelidir. |
 
###  <a name="how-to-manage-a-private-endpoint-connection-to-key-vault"></a>Anahtar Kasası ile özel bir uç nokta bağlantısını yönetme

1. Azure portalında oturum açın.
1. Arama çubuğuna "Anahtar Kasası" yazın
1. Yönetmek istediğiniz anahtar kasasını seçin.
1. "Ağ" sekmesini seçin.
1. Bekleyen bağlantı varsa, sağlama durumunda "bekliyor" ile listelenmiş bir bağlantı görürsünüz. 
1. Onaylamak istediğiniz özel uç noktayı seçin
1. Onayla düğmesini seçin.
1. Reddetmek istediğiniz özel uç nokta bağlantıları varsa, bekleyen bir istek ya da var olan bağlantı olup olmadığına bakılmaksızın bağlantıyı seçin ve "Reddet" düğmesine tıklayın.

    ![Resim](./media/private-link-service-7.png)

## <a name="validate-that-the-private-link-connection-works"></a>Özel bağlantı bağlantısının çalışıp çalışmadığını doğrulama

Özel uç nokta kaynağının aynı alt ağı içindeki kaynakların, özel bir IP adresi üzerinden anahtar Kasanıza bağlı olduğunu ve doğru özel DNS bölge tümleştirmesini olduğunu doğrulamanız gerekir.

İlk olarak, [Azure Portal Windows sanal makinesi oluşturma](../virtual-machines/windows/quick-create-portal.md) bölümündeki adımları izleyerek bir sanal makine oluşturun

"Ağ" sekmesinde:

1. Sanal ağ ve alt ağ belirtin. Yeni bir sanal ağ oluşturabilir veya var olan bir sanal ağı seçebilirsiniz. Mevcut bir tane seçilirse, bölgenin eşleştiğinden emin olun.
1. Genel bir IP kaynağı belirtin.
1. "NIC ağ güvenlik grubu" nda "hiçbiri" ni seçin.
1. "Yük Dengeleme" içinde "Hayır" ı seçin.

Komut satırını açın ve şu komutu çalıştırın:

```console
nslookup <your-key-vault-name>.vault.azure.net
```

Ortak bir uç nokta üzerinden bir anahtar kasasının IP adresini çözümlemek için NS arama komutunu çalıştırırsanız şuna benzer bir sonuç görürsünüz:

```console
c:\ >nslookup <your-key-vault-name>.vault.azure.net

Non-authoritative answer:
Name:    
Address:  (public IP address)
Aliases:  <your-key-vault-name>.vault.azure.net
```

Özel bir uç nokta üzerinden bir anahtar kasasının IP adresini çözümlemek için NS arama komutunu çalıştırırsanız şuna benzer bir sonuç görürsünüz:

```console
c:\ >nslookup your_vault_name.vault.azure.net

Non-authoritative answer:
Name:    
Address:  10.1.0.5 (private IP address)
Aliases:  <your-key-vault-name>.vault.azure.net
          <your-key-vault-name>.privatelink.vaultcore.azure.net
```

## <a name="limitations-and-design-considerations"></a>Sınırlamalar ve tasarım konuları

**Fiyatlandırma**: fiyatlandırma bilgileri için bkz. [Azure özel bağlantı (Önizleme) fiyatlandırması](https://azure.microsoft.com/pricing/details/private-link/).

**Sınırlamalar**: Azure Key Vault Için özel uç nokta genel önizlemede. Bu özellik tüm Azure genel bölgelerinde kullanılabilir.

**Key Vault başına en fazla özel uç nokta sayısı**: 64.

**Abonelik başına özel uç noktalara sahip maksimum Anahtar Kasası sayısı**: 64.

Daha fazla bilgi için bkz [. Azure özel bağlantı hizmeti: sınırlamalar](../private-link/private-link-service-overview.md#limitations)

## <a name="next-steps"></a>Sonraki Adımlar

- Azure özel bağlantısı hakkında daha fazla bilgi edinin [(Önizleme)](../private-link/private-link-service-overview.md)
- [Azure Key Vault](key-vault-overview.md) hakkında daha fazla bilgi edinin
