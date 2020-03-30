---
title: PowerShell'i kullanarak EŞ ASN'yi Azure aboneliğiyle ilişkilendirin
titleSuffix: Azure
description: PowerShell'i kullanarak EŞ ASN'yi Azure aboneliğiyle ilişkilendirin
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 77cc4732e017d95cbae19578cf26b1111b08fdde
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75908979"
---
# <a name="associate-peer-asn-to-azure-subscription-using-powershell"></a>PowerShell'i kullanarak EŞ ASN'yi Azure aboneliğiyle ilişkilendirin

Bir eşleme isteği göndermeden önce, asn aboneliğinizi aşağıdaki adımları kullanarak Azure aboneliğiyle ilişkilendirmelisiniz.

İsterseniz, [portalı](howto-subscription-association-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="create-peerasn-to-associate-your-asn-with-azure-subscription"></a>ASN'nizi Azure Aboneliği ile ilişkilendirmek için PeerASN oluşturun

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="register-for-peering-resource-provider"></a>Eşleyen kaynak sağlayıcısına kaydolun
Aşağıdaki komutu kullanarak aboneliğinizde eşleyen kaynak sağlayıcıya kaydolun. Bunu yürütmezseniz, eşleme ayarlamak için gereken Azure kaynaklarına erişilemez.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Peering
```

Aşağıdaki komutları kullanarak kayıt durumunu kontrol edebilirsiniz:
```powershell
Get-AzResourceProvider -ProviderNamespace Microsoft.Peering
```

> [!IMPORTANT]
> Devam etmeden önce *RegistrationState'in* "Kayıtlı" olarak dönmesini bekleyin. Komutu çalıştırdıktan sonra 5 ila 30 dakika sürebilir.

### <a name="update-the-peer-information-associated-with-this-subscription"></a>Bu abonelikle ilişkili eş bilgilerini güncelleştirme

Aşağıda eş bilgilerini güncelleştirmek için bir örnek verilmiştir.

```powershell
New-AzPeerAsn `
    -Name "Contoso_1234" `
    -PeerName "Contoso" `
    -PeerAsn 1234 `
    -Email noc@contoso.com, support@contoso.com `
    -Phone "+1 (555) 555-5555"
```

> [!NOTE]
> -Ad kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir. Ancak, -peerName şirketinizin adına karşılık gelir ve PeeringDB profilinize mümkün olduğunca yakın olması gerekir. -peerName değerinin yalnızca a-z, A-Z ve boşluk karakterlerini desteklediğini unutmayın.

Bir aboneliğin birden çok ASN'si olabilir. Her ASN için eşleme bilgilerini güncelleştirin. "Ad"ın her ASN için benzersiz olduğundan emin olun.

Eşlerin [PeeringDB'de](https://www.peeringdb.com)tam ve güncel bir profile sahip olması beklenir. Bu bilgileri kayıt sırasında, noc bilgileri, teknik iletişim bilgileri ve bunların akran tesislerindeki varlığı gibi akran bilgilerini doğrulamak için kullanırız.

Yukarıdaki çıktıda **{subscriptionId}** yerine gerçek abonelik kimliğinin görüntüleneceğini unutmayın.

## <a name="view-status-of-a-peerasn"></a>PeerASN'nin durumunu görüntüleme

Aşağıdaki komutu kullanarak ASN Doğrulama durumunu denetleyin:

```powershell
Get-AzPeerAsn
```

Aşağıda örnek bir yanıt verilmiştir:
```powershell
PeerContactInfo : Microsoft.Azure.PowerShell.Cmdlets.Peering.Models.PSContactInfo
PeerName        : Contoso
ValidationState : Approved
PeerAsnProperty : 1234
Name            : Contoso_1234
Id              : /subscriptions/{subscriptionId}/providers/Microsoft.Peering/peerAsns/Contoso_1234
Type            : Microsoft.Peering/peerAsns
```

> [!IMPORTANT]
> Bir eşleme isteği göndermeden önce Doğrulama Durumunun "Onaylandı"yı döndürmesini bekleyin. Bu onay 12 saat kadar sürebilir.

## <a name="modify-peerasn"></a>PeerAsn değiştir
NOC kişi bilgilerini istediğiniz zaman değiştirebilirsiniz.

Aşağıda bir örnek:

```powershell
Set-PeerAsn -Name Contoso_1234 -Email "newemail@test.com" -Phone "1800-000-0000"
```

## <a name="delete-peerasn"></a>PeerAsn'ı sil
PeerASN silme şu anda desteklenmez. PeerASN'yi silmeniz gerekiyorsa, [Microsoft'a başvurun.](mailto:peering@microsoft.com)

## <a name="next-steps"></a>Sonraki adımlar

* [Doğrudan eşleme oluşturma veya değiştirme](howto-direct-powershell.md)
* [Eski bir Doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-powershell.md)
* [Exchange eşlemi oluşturma veya değiştirme](howto-exchange-powershell.md)
* [Eski bir Exchange eşlemeyi Azure kaynağına dönüştürme](howto-legacy-exchange-powershell.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet'e bakan SSS'leri](faqs.md) ziyaret edin
