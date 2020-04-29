---
title: Uygulama teklifini Yayımla Power BI | Azure Marketi
description: Microsoft AppSource marketi üzerinde Power BI uygulama teklifi yayımlayın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: eb31520c81a4064edbe54a0256b694c4ad88fb49
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82141707"
---
# <a name="publish-a-power-bi-app-offer"></a>Power BI uygulama teklifi yayımlama

>[!Important]
>13 Nisan 2020 ' den itibaren, Power BI uygulama tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Power BI uygulama oluşturmaya genel bakış](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-power-bi-app-offer) ' daki yönergeleri izleyin.

Son adım, Bulut İş Ortağı Portalı bir teklif tanımladıktan ve ilişkili teknik varlıkların oluşturulduktan sonra, teklifi yayımlama için göndermiydi. Bu işlemi başlatmak için **yeni teklif** penceresinin sol bölmesinde **Yayımla**' yı seçin. Daha fazla bilgi için bkz. [Azure Marketi ve AppSource tekliflerini yayımlama](../manage-offers/cpp-publish-offer.md).


## <a name="publishing-steps"></a>Yayımlama adımları

Bunlar yayımlama işlemindeki ana adımlardır:

![Power BI uygulama teklifi için işleme adımları yayımlanıyor](./media/publishing-process-steps.png)

Bu tablo, her adımı açıklar ve tahmini tamamlanma süresini sağlar:

|   Yayımlama adımı            |   Zaman     |   Açıklama                                                                  |
| --------------------         |------------| ----------------                                                               |
| Önkoşulları doğrulama       | 15 dakika     | Teklif bilgileri ve teklif ayarları onaylanır.                            |
| Sertifika                | 1-7 gün   | Power BI sertifika ekibi teklifinizi analiz eder. Takım, uygulamayı belirtilen yükleme URL 'SI aracılığıyla yükleyerek Power BI uygulamanızı el ile doğrulama testi aracılığıyla çalıştırır. Birincil doğrulamalar, uygulama sertifika sürecinin (Bu belgenin ilerleyen kısımlarında açıklanmıştır) bir parçası olarak gerçekleştirilir.         |
| Paketleme                    | \<1 saat  | Teklifin teknik varlıkları müşteri kullanımı için paketlenmiştir.                        |
| Müşteri adayı oluşturma kaydı | \<1 saat  | Lider sistemleri yapılandırılır ve dağıtılır.                                      |
| Yayımcı oturumu kapatma            | \-         | Teklifin etkin olabilmesi için son gözden geçirmeyi ve onayı tamamlarız. Ayrıca, teklifinizi önizlemek için artık bir bağlantınız olacak. Önizlemesinin nasıl göründüğünü beğendikten sonra **durum** sekmesinde **canlı git** ' i seçin. Bu, uygulamanızı AppSource 'ta listelemek için ekleme ekibine bir istek gönderir.    |
| Canlı                         | \<3 saat | Teklifiniz artık AppSource 'ta genel olarak listelenmiş ("canlı") ve müşteriler uygulamanızı görüntüleyebilir ve Power BI aboneliklerinde dağıtabilir. Ayrıca bir onay e-postası da alacaksınız. **Tüm** teklifler sekmesinin sağ sütununda, tüm tekliflerinizin durumunu görebilirsiniz. **Durum** sekmesinde, teklifiniz için ayrıntılı yayımlama akışı durumunu görebilirsiniz. |
|   |   |

Bu işlemin tamamlanabilmesi için sekiz güne kadar izin verin. Bu yayımlama adımlarını tamamladıktan sonra Power BI uygulama teklifiniz [Appsource](https://appsource.microsoft.com/marketplace/apps?product=power-bi%20) Power BI uygulamalar bölümünde listelenecektir.


### <a name="app-certification-process"></a>Uygulama sertifika işlemi

Microsoft ekleme ekibi, Power BI uygulama teklifinizin gönderimini doğrulamak için bu işlemi kullanır:

1. Yasal belge ve yardım bağlantılarını gözden geçirin.
2. Destek iletişim bilgilerini doğrulayın.
3. Doğru yüklemeyi doğrulamak için yükleyici URL 'sini kullanın.
4. Uygulamayı kötü amaçlı yazılımlara ve diğer kötü amaçlı içeriklere karşı tarayın.
5. Görüntülenen içeriğin uygulamanın açıklamasıyla eşleştiğini doğrulayın.
6. Uygulamayla ilgili işlemlerin Power BI beklendiği gibi çalıştığını doğrulayın. Takım, rapor ve panoları örnek verilerle açar, özel veri kaynaklarına bağlanır, verileri yeniler ve bu şekilde devam eder.

Sertifika ekibi herhangi bir sorun bulduklarında geri bildirim sağlar.  Power BI uygulama gereksinimleri hakkında daha fazla bilgi için bkz. [Power BI uygulama belgeleri](https://go.microsoft.com/fwlink/?linkid=2028636).


## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı [Appsource Market](https://appsource.microsoft.com)'te düzenli olarak izlemenizi öneririz.  Market müşterileriniz ve uygulama kullanımınız hakkında Öngörüler almak için [bulut iş ortağı portalı](https://cloudpartner.azure.com/#insights) 'Nin [satıcı öngörüleri](../../cloud-partner-portal-orig/si-getting-started.md) özelliğini de kullanmanız gerekir. Son olarak, [teklifinizi güncelleştirebilirsiniz](./cpp-update-existing-offer.md).
