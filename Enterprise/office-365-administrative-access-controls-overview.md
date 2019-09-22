---
title: Office 365 관리 액세스 권한 컨트롤
ms.author: robmazz
author: robmazz
manager: laurawi
audience: ITPro
ms.topic: article
ms.service: O365-seccomp
localization_priority: Normal
search.appverid:
- MET150
ms.collection:
- Strat_O365_IP
- M365-security-compliance
description: '요약: Office 365의 관리 액세스 제어 및 데이터 분류에 대 한 개요입니다.'
ms.openlocfilehash: e8cc470c617deea7435841f276b772b0a8ef17a3
ms.sourcegitcommit: 55a046bdf49bf7c62ab74da73be1fd1cf6f0ad86
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "37067709"
---
# <a name="administrative-access-controls-in-office-365"></a><span data-ttu-id="b7b28-103">Office 365 관리 액세스 권한 컨트롤</span><span class="sxs-lookup"><span data-stu-id="b7b28-103">Administrative Access Controls in Office 365</span></span> 

<span data-ttu-id="b7b28-104">Microsoft는 Microsoft의 고객 콘텐츠에 대 한 액세스를 의도적으로 제한 하면서 대부분의 Office 365 운영을 자동화 하는 시스템 및 컨트롤에 크게 투자 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-104">Microsoft has invested heavily in systems and controls that automate most Office 365 operations while intentionally limiting access to customer content by Microsoft.</span></span> <span data-ttu-id="b7b28-105">서비스를 관리 하 고 소프트웨어가 서비스를 운영 하는 사람입니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-105">Humans govern the service, and software operates the service.</span></span> <span data-ttu-id="b7b28-106">이를 통해 Microsoft는 확장 시에 Office 365을 관리 하 고 고객 콘텐츠에 대 한 내부 위협의 위험을 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-106">This enables Microsoft to manage Office 365 at scale and manage the risks of internal threats to customer content.</span></span>

<span data-ttu-id="b7b28-107">기본적으로 Microsoft 엔지니어가 관리 권한을 보유 하 고 있으며 Office 365의 고객 콘텐츠에 대 한 액세스 권한이 제로입니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-107">By default, Microsoft engineers have zero standing administrative privileges and zero standing access to customer content in Office 365.</span></span> <span data-ttu-id="b7b28-108">제한 된 기간 동안 Microsoft 엔지니어가 고객 콘텐츠에 대 한 액세스를 제한 하 고 감사 하 고 보안을 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-108">A Microsoft engineer can have limited, audited, and secured access to a customer's content for a limited amount of time.</span></span> <span data-ttu-id="b7b28-109">액세스는 서비스 작업에 필요한 경우에만 사용 되며 Microsoft의 선임 관리 구성원이 승인 하는 경우에만 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-109">Access is only when necessary for service operations and only when approved by a member of Microsoft senior management.</span></span> <span data-ttu-id="b7b28-110">고객 인증 키 저장소 사용이 허가 된 고객의 경우, 고객은 Office 365에서 호스트 되는 콘텐츠에 대 한 액세스 승인을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-110">For Customer Lockbox licensed customers, the customer provides access approval to their content hosted on Office 365.</span></span>

<span data-ttu-id="b7b28-111">Microsoft는 다양 한 형태의 클라우드 전달을 사용 하 여 온라인 서비스를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-111">Microsoft provides online services using multiple forms of cloud delivery:</span></span>

- <span data-ttu-id="b7b28-112">**공용 클라우드:** 여러 테 넌 트 버전의 Office 365, Azure 및 북미, 남미, 유럽, 아시아, 오스트레일리아 등에 호스트 되는 기타 서비스를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-112">**Public Clouds:** Includes multi-tenant versions of Office 365, Azure, and other services hosted in North America, South America, Europe, Asia, Australia, etc.</span></span>
- <span data-ttu-id="b7b28-113">**국가 클라우드:** 이전에 언급 한 것을 제외 하 고 미국 외부에서 운영 하는 모든 sovereign 클라우드에 및 타사에서 작동 하는 모든 클라우드 (중국에서 운영 하는 Office 365) 및 독일의 Office 365 (예: Microsoft에서 운영 하지만 독일어 데이터를 트러스티로 제공 되는 모델에서)을 포함 합니다. Deutsche Telekom, Microsoft의 고객 데이터 및 고객 데이터를 포함 하는 시스템에 대 한 액세스를 제어 하 고 모니터링 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-113">**National Clouds:** Includes all sovereign and third party-operated clouds outside of the United States (except ones noted previously), such as Office 365 in China (operated by 21Vianet), and Office 365 in Germany (operated by Microsoft, but under a model in which a German data trustee, Deutsche Telekom, controls and monitors Microsoft's access to Customer Data and systems that contain Customer Data).</span></span>
- <span data-ttu-id="b7b28-114">**정부 클라우드:** 미국 정부 고객에 게 제공 되는 Office 365 및 Azure 서비스를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-114">**Government Clouds:** Includes Office 365 and Azure services that are available to United States government customers.</span></span>

<span data-ttu-id="b7b28-115">이 문서에서 제공 하는 Office 365 서비스에는 다음이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-115">For purposes of this article, Office 365 services include:</span></span>

- [<span data-ttu-id="b7b28-116">Exchange Online</span><span class="sxs-lookup"><span data-stu-id="b7b28-116">Exchange Online</span></span>](https://docs.microsoft.com/Exchange/exchange-online)
- [<span data-ttu-id="b7b28-117">Exchange Online Protection</span><span class="sxs-lookup"><span data-stu-id="b7b28-117">Exchange Online Protection</span></span>](https://docs.microsoft.com/Office365/SecurityCompliance/eop/exchange-online-protection-overview)
- [<span data-ttu-id="b7b28-118">SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="b7b28-118">SharePoint Online</span></span>](https://docs.microsoft.com/sharepoint/sharepoint-online)
- [<span data-ttu-id="b7b28-119">비즈니스용 OneDrive</span><span class="sxs-lookup"><span data-stu-id="b7b28-119">OneDrive for Business</span></span>](https://docs.microsoft.com/OneDrive/onedrive)
- [<span data-ttu-id="b7b28-120">비즈니스용 Skype</span><span class="sxs-lookup"><span data-stu-id="b7b28-120">Skype for Business</span></span>](https://docs.microsoft.com/SkypeForBusiness/skype-for-business-online)
- [<span data-ttu-id="b7b28-121">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="b7b28-121">Microsoft Teams</span></span>](https://docs.microsoft.com/MicrosoftTeams/Teams-overview)
- [<span data-ttu-id="b7b28-122">Yammer</span><span class="sxs-lookup"><span data-stu-id="b7b28-122">Yammer</span></span>](https://docs.microsoft.com/yammer/yammer-landing-page)

## <a name="office-365-access-controls"></a><span data-ttu-id="b7b28-123">Office 365 액세스 제어</span><span class="sxs-lookup"><span data-stu-id="b7b28-123">Office 365 Access Controls</span></span>

<span data-ttu-id="b7b28-124">액세스 제어를 위해 Microsoft는 Office 365 데이터를 고객 데이터 또는 기타 데이터 형식으로 분류 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-124">For access control purposes, Microsoft categorizes Office 365 data as Customer data or other types of data.</span></span>

### <a name="customer-data"></a><span data-ttu-id="b7b28-125">고객 데이터</span><span class="sxs-lookup"><span data-stu-id="b7b28-125">Customer data</span></span>

<span data-ttu-id="b7b28-126">고객 데이터는 Office 365 서비스를 사용할 때 또는 고객을 대신 하 여 제공 하는 모든 데이터입니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-126">Customer data is all data provided by or on behalf of a customer when using Office 365 services.</span></span> <span data-ttu-id="b7b28-127">다음을 포함 하 여 Office 365 사용자가 직접 만들거나 업로드 한 고객 콘텐츠입니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-127">This is customer content directly created or uploaded by Office 365 users, including:</span></span>

- <span data-ttu-id="b7b28-128">전자 메일</span><span class="sxs-lookup"><span data-stu-id="b7b28-128">Emails</span></span>
- <span data-ttu-id="b7b28-129">SharePoint Online 콘텐츠</span><span class="sxs-lookup"><span data-stu-id="b7b28-129">SharePoint Online content</span></span>
- <span data-ttu-id="b7b28-130">인스턴트 메시지</span><span class="sxs-lookup"><span data-stu-id="b7b28-130">Instant messages</span></span>
- <span data-ttu-id="b7b28-131">일정 항목</span><span class="sxs-lookup"><span data-stu-id="b7b28-131">Calendar items</span></span>
- <span data-ttu-id="b7b28-132">문서</span><span class="sxs-lookup"><span data-stu-id="b7b28-132">Documents</span></span>
- <span data-ttu-id="b7b28-133">Contacts</span><span class="sxs-lookup"><span data-stu-id="b7b28-133">Contacts</span></span>
- <span data-ttu-id="b7b28-134">EUII (최종 사용자 식별 정보) (사용자가 고유 하거나 개별 사용자에 게 linkable 하지만 고객 콘텐츠를 포함 하지 않는 데이터)</span><span class="sxs-lookup"><span data-stu-id="b7b28-134">End-user identifiable information (EUII) (data that is unique to a user or that is linkable to an individual user but does not include customer content).</span></span>

### <a name="other-types-of-data"></a><span data-ttu-id="b7b28-135">기타 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="b7b28-135">Other types of data</span></span>

<span data-ttu-id="b7b28-136">다른 데이터 형식에는 다음이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-136">Other types of data include:</span></span>

- <span data-ttu-id="b7b28-137">**계정 데이터:** 관리 데이터 (관리자가 서비스를 등록 하거나 구매할 때 제공 하는 정보) 및 지불 데이터 (신용 카드 정보와 같은 결제 방식 정보)를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-137">**Account data:** Includes administrative data (information provided by administrators when they sign-up or purchase services), and payment data (information about payment instruments, such as credit card details).</span></span>
- <span data-ttu-id="b7b28-138">**조직 차원 식별이 가능한 정보:** 테 넌 트, 사용 현황 데이터를 식별 하 고 개별 사용자에 게 linkable 하는 데 사용 되거나 고객 콘텐츠에 포함 되지 않은 데이터를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-138">**Organizationally identifiable information:** Includes data used to identify a tenant, usage data, and not linkable to an individual user or included in customer content.</span></span>
- <span data-ttu-id="b7b28-139">**시스템 메타 데이터:** 구성 설정, 시스템 상태, Microsoft IP 주소 및 구독 및 테 넌 트에 대 한 기술 정보가 포함 된 서비스 로그를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-139">**System metadata:** Includes service logs that contain configuration settings, system status, Microsoft IP addresses, and technical information about subscriptions and tenants.</span></span>

<span data-ttu-id="b7b28-140">Microsoft는 고객 데이터 또는 액세스 제어 데이터에 대 한 승인 되지 않은 액세스를 사용 하지 않도록 액세스 제어 메커니즘을 설정 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-140">Microsoft has established access control mechanisms to ensure that no one has unapproved access to Customer Data or access control data.</span></span> <span data-ttu-id="b7b28-141">액세스 제어 데이터는 고객 콘텐츠 또는 EUII, Microsoft 암호, 보안 인증서 및 기타 인증 관련 데이터에 대 한 액세스를 포함 하 여 환경 내에서 다른 유형의 데이터 또는 기능에 대 한 액세스를 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-141">Access control data manages access to other types of data or functions within the environment, including access to customer content or EUII, Microsoft passwords, security certificates, and other authentication-related data.</span></span> <span data-ttu-id="b7b28-142">또한 액세스 제어 메커니즘은 Office 365 프로덕션 환경에 대 한 승인 되지 않은 물리적, 논리적 또는 원격 액세스를 보호 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-142">Access control mechanisms also guard against unapproved physical, logical, or remote access to the Office 365 production environment.</span></span>

<span data-ttu-id="b7b28-143">Office 365에는 Microsoft에서 사용 하는 세 가지 종류의 access 컨트롤이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-143">There are three categories of access controls used by Microsoft for operating Office 365:</span></span>

- <span data-ttu-id="b7b28-144">격리 컨트롤</span><span class="sxs-lookup"><span data-stu-id="b7b28-144">Isolation Controls</span></span>
- <span data-ttu-id="b7b28-145">인적 컨트롤</span><span class="sxs-lookup"><span data-stu-id="b7b28-145">Personnel Controls</span></span>
- <span data-ttu-id="b7b28-146">기술 컨트롤</span><span class="sxs-lookup"><span data-stu-id="b7b28-146">Technology Controls</span></span>

<span data-ttu-id="b7b28-147">이러한 컨트롤을 결합 하면 Office 365에서 악의적인 작업을 방지 하 고 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-147">When combined, these controls help prevent and detect malicious actions in Office 365.</span></span> <span data-ttu-id="b7b28-148">Microsoft에서 사용 하는 격리, 인력 및 기술 제어 외에도 customer에서 구현 하는 컨트롤의 네 번째 범주를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-148">In addition to the isolation, personnel, and technology controls used by Microsoft, there is a fourth category of controls: those implemented by customers.</span></span>

<span data-ttu-id="b7b28-149">Office 365에서는 온-프레미스 환경에서 데이터를 관리 하는 것과 동일한 방식으로 데이터를 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-149">Office 365 allows you to manage data the same way data is managed in on-premises environments.</span></span> <span data-ttu-id="b7b28-150">Office 365 조직에 등록 하는 사람은 자동으로 전역 관리자가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-150">The person who signs up an organization for Office 365 automatically becomes a global administrator.</span></span> <span data-ttu-id="b7b28-151">전역 관리자는 관리 포털의 모든 기능에 액세스할 수 있으며 다음이 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-151">The global admin has access to all features in Management Portals and can:</span></span>

- <span data-ttu-id="b7b28-152">사용자 만들기 또는 편집</span><span class="sxs-lookup"><span data-stu-id="b7b28-152">Create or edit users</span></span>
- <span data-ttu-id="b7b28-153">다른 사용자에 게 관리자 역할 할당</span><span class="sxs-lookup"><span data-stu-id="b7b28-153">Assign admin roles to others</span></span>
- <span data-ttu-id="b7b28-154">사용자 암호 다시 설정</span><span class="sxs-lookup"><span data-stu-id="b7b28-154">Reset user passwords</span></span>
- <span data-ttu-id="b7b28-155">사용자 라이선스 관리</span><span class="sxs-lookup"><span data-stu-id="b7b28-155">Manage user licenses</span></span>
- <span data-ttu-id="b7b28-156">도메인 관리</span><span class="sxs-lookup"><span data-stu-id="b7b28-156">Manage domains</span></span>
- <span data-ttu-id="b7b28-157">고객 Lockbox 요청 승인</span><span class="sxs-lookup"><span data-stu-id="b7b28-157">Approve Customer Lockbox requests</span></span>

<span data-ttu-id="b7b28-158">각 조직에서는 최소 두 개의 관리자 계정을 구성 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-158">We recommend that each organization configure at least two admin accounts.</span></span> <span data-ttu-id="b7b28-159">대규모 엔터프라이즈 조직의 경우 다른 기능을 제공 하는 특수 한 관리자 계정을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="b7b28-159">For large enterprise organizations, we recommend specialized admin accounts that serve different functions.</span></span>

<span data-ttu-id="b7b28-160">관리자 역할 및 사용 권한을 할당 하는 방법에 대 한 자세한 내용은 [office 365의 관리자 역할 할당](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504) 및 [office 365 관리자 역할 정보](https://support.office.com/article/Permissions-in-Office-365-DA585EEA-F576-4F55-A1E0-87090B6AAA9D)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="b7b28-160">For information about assigning admin roles and permissions, see [Assigning admin roles in Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504) and [About Office 365 admin roles](https://support.office.com/article/Permissions-in-Office-365-DA585EEA-F576-4F55-A1E0-87090B6AAA9D).</span></span>

## <a name="related-links"></a><span data-ttu-id="b7b28-161">관련 링크</span><span class="sxs-lookup"><span data-stu-id="b7b28-161">Related Links</span></span>

- [<span data-ttu-id="b7b28-162">격리 컨트롤</span><span class="sxs-lookup"><span data-stu-id="b7b28-162">Isolation Controls</span></span>](office-365-isolation-controls.md)
- [<span data-ttu-id="b7b28-163">인적 컨트롤</span><span class="sxs-lookup"><span data-stu-id="b7b28-163">Personnel Controls</span></span>](office-365-personnel-controls.md)
- [<span data-ttu-id="b7b28-164">기술 컨트롤</span><span class="sxs-lookup"><span data-stu-id="b7b28-164">Technology Controls</span></span>](office-365-technology-controls.md)
- [<span data-ttu-id="b7b28-165">액세스 제어 모니터링 및 감사</span><span class="sxs-lookup"><span data-stu-id="b7b28-165">Monitoring and Auditing Access Controls</span></span>](office-365-monitoring-and-auditing-access-controls.md)
- [<span data-ttu-id="b7b28-166">Yammer Enterprise 액세스 제어</span><span class="sxs-lookup"><span data-stu-id="b7b28-166">Yammer Enterprise Access Controls</span></span>](office-365-yammer-enterprise-access-controls.md)