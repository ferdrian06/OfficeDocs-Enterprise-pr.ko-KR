---
title: Office 365용 ExpressRoute 구현
ms.author: kvice
author: kelleyvice-msft
manager: laurawi
ms.date: 12/5/2017
ms.audience: ITPro
ms.topic: conceptual
ms.service: o365-administration
localization_priority: Normal
ms.collection: Ent_O365
ms.custom: Adm_O365
search.appverid:
- MET150
- MOE150
- BCS160
ms.assetid: 77735c9d-8b80-4d2f-890e-a8598547dea6
description: Office 365에 대 한 ExpressRoute 대체 라우팅 경로를 제공 많은 인터넷 마주보는 Office 365 서비스 합니다. Office 365에 대 한 ExpressRoute의 아키텍처는 이미 액세스할 수 있는 인터넷을 통해 해당 IP 접두사에 후속 재배포에 대 한 프로 비전 된 ExpressRoute 회로에 Office 365 서비스의 공용 IP 접두사 광고 기반 네트워크입니다. ExpressRoute와 여러 서로 다른 라우팅 경로 인터넷을 통해 및 ExpressRoute를 통해 다양 한 Office 365 서비스에 대 한 효과적으로 사용 합니다. 네트워크에서 라우팅의이 상태는 내부 네트워크 토폴로지를 설계 하는 방법에 중요 한 변경이 나타낼 수 있습니다.
ms.openlocfilehash: c4479a236d1419293dbd433e8d3c10a11ea5fb45
ms.sourcegitcommit: 69d60723e611f3c973a6d6779722aa9da77f647f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "22542264"
---
# <a name="implementing-expressroute-for-office-365"></a><span data-ttu-id="a23ba-106">Office 365용 ExpressRoute 구현</span><span class="sxs-lookup"><span data-stu-id="a23ba-106">Implementing ExpressRoute for Office 365</span></span>

<span data-ttu-id="a23ba-p102">Office 365에 대 한 ExpressRoute 대체 라우팅 경로를 제공 많은 인터넷 마주보는 Office 365 서비스 합니다. Office 365에 대 한 ExpressRoute의 아키텍처는 이미 액세스할 수 있는 인터넷을 통해 해당 IP 접두사에 후속 재배포에 대 한 프로 비전 된 ExpressRoute 회로에 Office 365 서비스의 공용 IP 접두사 광고 기반 네트워크입니다. ExpressRoute와 여러 서로 다른 라우팅 경로 인터넷을 통해 및 ExpressRoute를 통해 다양 한 Office 365 서비스에 대 한 효과적으로 사용 합니다. 네트워크에서 라우팅의이 상태는 내부 네트워크 토폴로지를 설계 하는 방법에 중요 한 변경이 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p102">ExpressRoute for Office 365 provides an alternate routing path to many internet facing Office 365 services. The architecture of ExpressRoute for Office 365 is based on advertising public IP prefixes of Office 365 services that are already accessible over the Internet into your provisioned ExpressRoute circuits for subsequent redistribution of those IP prefixes into your network. With ExpressRoute you effectively enable several different routing paths, through the internet and through ExpressRoute, for many Office 365 services. This state of routing on your network may represent a significant change to how your internal network topology is designed.</span></span>
  
 <span data-ttu-id="a23ba-111">**상태:** 전체 가이드 v2</span><span class="sxs-lookup"><span data-stu-id="a23ba-111">**Status:** Complete Guide v2</span></span>
  
<span data-ttu-id="a23ba-p103">핵심 네트워크와 인터넷에 삽입 하는 경로 사용 하 여 두 전용된 회선을 통해 사용할 수 있는 라우팅을 필요 없이의 네트워크 복잡성에 대 한 수용 하기 위해 Office 365 구현에 대 한 사용자 ExpressRoute를 신중 하 게 계획 해야 합니다. 및 팀 세부 계획 및이 가이드에서 테스트를 수행 하지 않아도, 하는 경우는 간헐적으로 발생 하는 높은 위험 수준 또는 ExpressRoute 회선을 사용 하는 경우 Office 365에 대 한 연결의 총 손실 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p103">You must carefully plan your ExpressRoute for Office 365 implementation to accommodate for the network complexities of having routing available via both a dedicated circuit with routes injected into your core network and the internet. If you and your team don't perform the detailed planning and testing in this guide, there is a high risk you'll experience intermittent or a total loss of connectivity to Office 365 services when the ExpressRoute circuit is enabled.</span></span>
  
<span data-ttu-id="a23ba-p104">성공적인 구현 해야할 인프라 요구 사항 분석, 상세 네트워크 평가 통해 이동 하 고 디자인, 신중 하 게 미리 구성 된 고 통제 된 방식으로 롤아웃 계획 및 구축 자세한 유효성 검사 및 테스트 계획 해야 합니다. 분산 된 대규모 환경에 대 한은 몇 개월에 걸쳐 구현을 참조 하는 경우가 종종 발생 하지 않습니다. 이 가이드는 대비할 수 있도록 설계 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p104">To have a successful implementation, you will need to analyze your infrastructure requirements, go through detailed network assessment and design, carefully plan the rollout in a staged and controlled manner, and build a detailed validation and testing plan. For a large, distributed environment it's not uncommon to see implementations span several months. This guide is designed to help you plan ahead.</span></span>
  
<span data-ttu-id="a23ba-p105">성공적인 대규모 배포의 경우 6 개월 계획에서 사용 하 고 자주 네트워킹, 방화벽 및 프록시 서버 관리자, Office 365 관리자, 보안, 최종 사용자 지원, 프로젝트를 포함 하 여 조직에서 다양 한 영역에서 팀 구성원을 포함 될 수 있습니다. 관리 및 프로젝트 스폰서 합니다. 계획 프로세스에 대 한 투자 가동 중지 시간이 나 복잡 하 고 비용이 많이 드는 문제를 해결 하는 배포 오류를 겪을 수 있는 가능성을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p105">Large successful deployments may take six months in planning and often include team members from many areas in the organization including networking, Firewall and Proxy server administrators, Office 365 administrators, security, end-user support, project management, and executive sponsorship. Your investment in the planning process will reduce the likelihood that you'll experience deployment failures resulting in downtime or complex and expensive troubleshooting.</span></span>
  
<span data-ttu-id="a23ba-119">다음과 같은 필수 구성 요소가이 구현 가이드를 시작 하기 전에 완료 될 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-119">We expect the following pre-requisites to be completed before this implementation guide is started.</span></span>
  
1. <span data-ttu-id="a23ba-120">ExpressRoute는 권장 하 고 승인 하는 경우를 결정 하는 네트워크 평가 완료 했습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-120">You've completed a network assessment to determine if ExpressRoute is recommended and approved.</span></span>

2. <span data-ttu-id="a23ba-p106">ExpressRoute 네트워크 서비스 공급자를 선택 했습니다. [ExpressRoute 파트너 및 피어 링 위치](https://azure.microsoft.com/documentation/articles/expressroute-locations/)하는 방법에 대 한 세부 정보를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p106">You've selected an ExpressRoute network service provider. Find details about the [ExpressRoute partners and peering locations](https://azure.microsoft.com/documentation/articles/expressroute-locations/).</span></span>

3. <span data-ttu-id="a23ba-123">했을 때 이미 잘 읽고 이해 [ExpressRoute 설명서](https://azure.microsoft.com/documentation/services/expressroute/) 및 내부 네트워크는 ExpressRoute 필수 구성 요소가 끝을 충족 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-123">You've already read and understand the [ExpressRoute documentation](https://azure.microsoft.com/documentation/services/expressroute/) and your internal network is able to meet ExpressRoute pre-requisites end to end.</span></span>

4. <span data-ttu-id="a23ba-124">팀의 모든 공용 지침 및 설명서의 읽기 [https://aka.ms/expressrouteoffice365](https://aka.ms/expressrouteoffice365), [https://aka.ms/ert](https://aka.ms/ert)를 포함 하 여 중요 한 기술 세부 이해 하기 위해 채널 9에서 [Office 365 교육에 대 한 Azure ExpressRoute](https://channel9.msdn.com/series/aer) 시리즈를 조사 하 고:</span><span class="sxs-lookup"><span data-stu-id="a23ba-124">Your team has read all of the public guidance and documentation at [https://aka.ms/expressrouteoffice365](https://aka.ms/expressrouteoffice365), [https://aka.ms/ert](https://aka.ms/ert), and watched the [Azure ExpressRoute for Office 365 Training](https://channel9.msdn.com/series/aer) series on Channel 9 to gain an understanding of critical technical details including:</span></span>

      - <span data-ttu-id="a23ba-125">SaaS 서비스의 인터넷 종속성을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-125">The internet dependencies of SaaS services.</span></span>

      - <span data-ttu-id="a23ba-126">비대칭 경로 방지 하 고 복잡 한 라우팅을 처리 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-126">How to avoid asymmetric routes and handle complex routing.</span></span>

      - <span data-ttu-id="a23ba-127">경계 보안, 가용성 및 응용 프로그램 수준 컨트롤을 통합 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-127">How to incorporate perimeter security, availability, and application level controls.</span></span>

## <a name="begin-by-gathering-requirements"></a><span data-ttu-id="a23ba-128">요구 사항 수집 하 여 시작</span><span class="sxs-lookup"><span data-stu-id="a23ba-128">Begin by gathering requirements</span></span>
<span data-ttu-id="a23ba-129"><a name="requirements"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-129"></span></span>

<span data-ttu-id="a23ba-p107">시작 하는 기능 및 조직 내에서 채택할 것을 계획 하는 서비스를 확인 합니다. 다른 Office 365 서비스의 기능 사용 되 고 네트워크에 있는 위치 이러한 기능을 사용 하는 사람을 호스트를 결정 해야 합니다. 네트워크 되는 특성을 추가 해야하는 시나리오의 카탈로그를 함께 이러한 시나리오의 각 필요 합니다. 예: 인바운드 및 아웃 바운드 네트워크 트래픽 흐름 및 경우 Office 365 끝점은 사용할 수 있는 ExpressRoute을 통해 또는 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p107">Start by determining which features and services you plan to adopt within your organization. You need to determine which features of the different Office 365 services will be used and which locations on your network will host people using those features. With the catalog of scenarios, you need to add the network attributes that each of those scenarios require; such as inbound and outbound network traffic flows and if the Office 365 endpoints are available over ExpressRoute or not.</span></span>
  
<span data-ttu-id="a23ba-133">조직의 요구 사항을 수집 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-133">To gather your organization's requirements:</span></span>
  
- <span data-ttu-id="a23ba-p108">조직에서 사용 하는 Office 365 서비스에 대 한 인바운드 및 아웃 바운드 네트워크 트래픽을 카탈로그 합니다. 다른 Office 365 시나리오를 필요로 하는 흐름에 대 한 설명을 Office 365 Url 및 IP 주소 범위 페이지를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p108">Catalog the inbound and outbound network traffic for the Office 365 services your organization is using. Consult Office 365 URLs and IP address ranges page for the description of flows that different Office 365 scenarios require.</span></span>

- <span data-ttu-id="a23ba-136">내부 WAN 백본 및 토폴로지에 대 한 세부 정보, 위성 사이트, 네트워크 경계 탈출 포인트 및 프록시 서비스를 라우팅, 마지막 마일 사용자 연결의 연결을 표시 하는 기존 네트워크 토폴로지에 대 한 설명서를 수집 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-136">Gather documentation of existing network topology showing details of your internal WAN backbone and topology, connectivity of satellite sites, last mile user connectivity, routing to network perimeter egress points, and proxy services.</span></span>

  - <span data-ttu-id="a23ba-137">Office 365 및 기타 Microsoft 서비스에 연결 하는, 인터넷 및 제안 된 ExpressRoute 연결 경로 모두 표시 되는 네트워크 다이어그램에서 인바운드 서비스 끝점을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-137">Identify inbound service endpoints on the network diagrams that Office 365 and other Microsoft services will connect to, showing both internet and proposed ExpressRoute connection paths.</span></span>

  - <span data-ttu-id="a23ba-138">모든 지리적 사용자 위치 및 위치는 위치에 아직 인터넷에는 송신 및 ExpressRoute 피어 링 위치는 탈출 해야 하는 위치는 제안 된 함께 간의 WAN 연결을 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-138">Identify all geographic user locations and WAN connectivity between locations along with which locations currently have an egress to the internet and which locations are proposed to have an egress to an ExpressRoute peering location.</span></span>

  - <span data-ttu-id="a23ba-139">프록시, 방화벽, 등의 모든에 지 장치를 식별 하 고 카탈로그 페이지의 관계에 흐름 인터넷 및 ExpressRoute 위로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-139">Identify all edge devices, such as proxies, firewalls, and so on and catalog their relationship to flows going over the Internet and ExpressRoute.</span></span>

  - <span data-ttu-id="a23ba-140">최종 사용자가 인터넷과 ExpressRoute 모두 흐름에 대 한 직접 라우팅 또는 간접 응용 프로그램 프록시를 통해 Office 365 서비스에 액세스 하는지 여부를 문서화 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-140">Document whether end users will access Office 365 services via direct routing or indirect application proxy for both Internet and ExpressRoute flows.</span></span>

- <span data-ttu-id="a23ba-141">테 넌 트의 위치를 추가 하 고 충족-나에 게 네트워크 다이어그램에 위치 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-141">Add the location of your tenant and meet-me locations to your network diagram.</span></span>

- <span data-ttu-id="a23ba-p109">예상 및 관찰 된 네트워크 성능 및 대기 시간 특성 주요 사용자 위치에서 Office 365를 예측 합니다. Office 365는 서비스의 전역 및 분산 집합 하 고 사용자가 자신의 테 넌 트의 위치와에서 다른 될 수 있는 위치에 연결할 모방 일에 유의 합니다. 이러한 이유로 측정 하 고 ExpressRoute 및 인터넷 연결을 통해 사용자와 Microsoft 글로벌 네트워크의 가장 가까운 가장자리 사이의 대기 시간을 최적화 하는 것이 좋습니다. 이 작업을 지원 하기 위해 네트워크 평가 대 한 결과 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p109">Estimate the expected and observed network performance and latency characteristics from major user locations to Office 365. Keep in mind that Office 365 is a global and distributed set of services and users will be connecting to locations that may be different from the location of their tenant. For this reason, it is recommended to measure and optimize for latency between the user and the closest edge of Microsoft global network over ExpressRoute and Internet connections. You can use your findings from the network assessment to aid with this task.</span></span>

- <span data-ttu-id="a23ba-p110">회사 네트워크 보안 및 새 ExpressRoute 연결 된 충족 하는 고가용성 요구 사항을 나열 합니다. 예, 어떻게 수행 사용자를 계속 받을 인터넷 탈출 또는 ExpressRoute 회로 오류 발생 시 Office 365에 대 한 액세스 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p110">List company network security and high availability requirements that need to be met with the new ExpressRoute connection. For example, how do users continue to get access to Office 365 in the event of the Internet egress or ExpressRoute circuit failure.</span></span>

- <span data-ttu-id="a23ba-p111">문서는 인바운드 및 아웃 바운드 Office 365 네트워크 흐름 인터넷 경로 사용 하면 하 고 ExpressRoute 사용할 하는 키를 누릅니다. 사용자의 지리적 위치 및 온-프레미스 네트워크 토폴로지에 대 한 세부 정보는 구체적인 다른 사용자가 한 명 위치에서 일치 하지 않아도 계획이 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p111">Document which inbound and outbound Office 365 network flows will use the Internet path and which will use ExpressRoute. The specifics of geographical locations of your users and details of your on-premises network topology may require the plan to be different from one user location to another.</span></span>

### <a name="catalog-your-outbound-and-inbound-network-traffic"></a><span data-ttu-id="a23ba-150">카탈로그에 아웃 바운드 및 인바운드 네트워크 트래픽</span><span class="sxs-lookup"><span data-stu-id="a23ba-150">Catalog your outbound and inbound network traffic</span></span>
<span data-ttu-id="a23ba-151"><a name="trafficCatalog"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-151"></span></span>

<span data-ttu-id="a23ba-p112">라우팅 및 기타 네트워크 복잡성을 최소화 하려면만 사용 하는 ExpressRoute Office 365에 대 한 규정 요구 사항으로 인해 또는 네트워크 평가의 결과 전용된 연결을 통해 이동 하는 데 필요한 네트워크 트래픽 흐름에 대 한 하는 것이 좋습니다. 또한 구현 프로젝트의 서로 다른 프로세스와 다른 시점으로 ExpressRoute 라우팅 및 접근 방식 아웃 바운드 및 인바운드 네트워크 트래픽 흐름의 범위를 준비 하는 것이 좋습니다. 아웃 바운드 네트워크 트래픽 흐름 및 인터넷을 통해 인바운드 네트워크 트래픽 흐름 토폴로지 복잡성과 추가 소개 (영문)의 위험에 증가 제어 하는 데 도움이 수 나가기 방금 사용자 시작에 대 한 ExpressRoute Office 365에 대 한 배포 비대칭 라우팅 방법을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p112">To minimize routing and other network complexities, we recommend that you only use ExpressRoute for Office 365 for the network traffic flows that are required to go over a dedicated connection due to regulatory requirements or as the result of the network assessment. Additionally, we recommend that you stage the scope of ExpressRoute routing and approach outbound and inbound network traffic flows as different and distinct stages of the implementation project. Deploy ExpressRoute for Office 365 for just user initiated outbound network traffic flows and leave inbound network traffic flows across the Internet can help to control the increase in topological complexity and risks of introducing additional asymmetric routing possibilities.</span></span>
  
<span data-ttu-id="a23ba-155">네트워크 트래픽 카탈로그 온-프레미스 네트워크와 Microsoft 간의 있어야 하는 모든 인바운드 및 아웃 바운드 네트워크 연결의 목록에 포함 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-155">Your network traffic catalog should contain listings of all the inbound and outbound network connections that you'll have between your on-premises network and Microsoft.</span></span>
  
- <span data-ttu-id="a23ba-p113">아웃 바운드 네트워크 트래픽 흐름은 여기에서 연결이 시작 됩니다 온-프레미스 환경에서와 같이 내부 클라이언트 또는 Microsoft 서비스의 대상 서버에서 모든 시나리오입니다. 이러한 연결은 Office 365에 직접 또는 간접적 때 연결 함으로 프록시 서버, 방화벽 또는 다른 네트워킹 장치를 통해 경로에서 Office 365와 같은 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p113">Outbound network traffic flows are any scenarios where a connection is initiated from your on-premises environment, such as from internal clients or servers, with a destination of the Microsoft services. These connections may be direct to Office 365 or indirect, such as when the connection goes through proxy servers, firewalls, or other networking devices on the path to Office 365.</span></span>

- <span data-ttu-id="a23ba-p114">인바운드 네트워크 트래픽 흐름은 Microsoft 클라우드에서 온-프레미스 호스트에 대 한 연결을 시작 하는 있는 모든 시나리오입니다. 이러한 연결은 일반적으로 방화벽 및 외부에서 발생 한 흐름에 대 한 고객 보안 정책 요구 하는 기타 보안 인프라를 통해 이동 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p114">Inbound network traffic flows are any scenarios where a connection is initiated from the Microsoft cloud to an on-premises host. These connections typically need to go through firewall and other security infrastructure that customer security policy requires for externally originated flows.</span></span>

<span data-ttu-id="a23ba-160">[Office 365에 대 한 ExpressRoute와 라우팅](https://support.office.com/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408) 서비스는 인바운드 트래픽을 보냅니다를 찾아서 [Office 365에서에서 **Office 365에 대 한 ExpressRoute** 를 표시 하는 열에 대 한 결정 하는 문서의 **보장 경로 대칭** 섹션을 읽으십시오 끝점](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) 참조 (영문) 문서를 나머지 연결 정보를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-160">Read the **Ensuring route symmetry** section of the article [Routing with ExpressRoute for Office 365](https://support.office.com/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408) to determine which services will send inbound traffic and look for the column marked **ExpressRoute for Office 365** in the [Office 365 endpoints](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) reference article to determine the rest of the connectivity information.</span></span>
  
<span data-ttu-id="a23ba-161">아웃 바운드 연결을 필요로 하는 각 서비스에 대 한 네트워크 라우팅, 프록시 구성, 패킷 검사를 포함 하 여 서비스에 대 한 계획 된 연결을 설명 하기 위해 알면 및 대역폭 요구 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-161">For each service that requires an outbound connection, you'll want to describe the planned connectivity for the service including network routing, proxy configuration, packet inspection, and bandwidth needs.</span></span>
  
<span data-ttu-id="a23ba-p115">인바운드 연결을 필요로 하는 각 서비스에 대 한 추가 정보가 필요 합니다. Microsoft 클라우드의 서버는 온-프레미스 네트워크에 대 한 연결을 설정 합니다. 연결을 제대로 설정할 확인, 원하는;을 포함 하 여이 연결의 모든 측면을 설명 하기 위해 이러한 인바운드 연결을 허용할 서비스에 대 한 공용 DNS 항목은 CIDR 서식이 지정 된는 ISP 장비는 작업이 포함 되는 IPv4 IP 주소 및 어떻게 인바운드 NAT 또는 이러한 연결에 대 한 원본 NAT 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p115">For each service that requires an inbound connection, you'll need some additional information. Servers in the Microsoft cloud will establish connections to your on-premises network. to ensure the connections are made correctly, you'll want to describe all aspects of this connectivity, including; the public DNS entries for the services that will accept these inbound connections, the CIDR formatted IPv4 IP addresses, which ISP equipment is involved, and how inbound NAT or source NAT is handled for these connections.</span></span>
  
<span data-ttu-id="a23ba-p116">인바운드 연결을 검토 하 여 인터넷을 통해 연결 중인 여부에 관계 없이 해야 또는 비대칭 라우팅이 이루어지도록 ExpressRoute 하지 않은 도입 되었습니다. 경우에 따라 온-프레미스 끝점을 Office 365 서비스 5 월에 대 한 인바운드 연결도 시작 해야 다른 Microsoft 및 비 Microsoft 서비스에서 액세스할 수 있습니다. 것이 가장 중요 ExpressRoute 라우팅 Office 365 목적을 위해 이러한 서비스를 사용 하면 다른 시나리오 중단 하지 않습니다. 대부분의 경우 소스 기반 ExpressRoute 사용 하도록 설정한 후 Microsoft에서 인바운드 흐름 대칭으로 유지 되도록 하려면 NAT와 같은 고객이 내부 네트워크에 특정 변경을 구현 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p116">Inbound connections should be reviewed regardless of whether they're connecting over the internet or ExpressRoute to ensure asymmetric routing hasn't been introduced. In some cases, on-premises endpoints that Office 365 services initiate inbound connections to may also need to be accessed by other Microsoft and non-Microsoft services. It is paramount that enabling ExpressRoute routing to these services for Office 365 purposes doesn't break other scenarios. In many cases, customers may need to implement specific changes to their internal network, such as source based NAT, to ensure that inbound flows from Microsoft remain symmetric after ExpressRoute is enabled.</span></span>
  
<span data-ttu-id="a23ba-p117">필요한 세부 수준 예제는 다음과 같습니다. 이 경우 Exchange 하이브리드 ExpressRoute을 통해 온-프레미스 시스템에 라우팅합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p117">Here's a sample of the level of detail required. In this case Exchange Hybrid would route to the on-premises system over ExpressRoute.</span></span>

|<span data-ttu-id="a23ba-171">**Connection 속성**</span><span class="sxs-lookup"><span data-stu-id="a23ba-171">**Connection property**</span></span>|<span data-ttu-id="a23ba-172">**값**</span><span class="sxs-lookup"><span data-stu-id="a23ba-172">**Value**</span></span>|
|:-----|:-----|
|<span data-ttu-id="a23ba-173">**네트워크 트래픽 방향**</span><span class="sxs-lookup"><span data-stu-id="a23ba-173">**Network traffic direction**</span></span> <br/> |<span data-ttu-id="a23ba-174">인바운드</span><span class="sxs-lookup"><span data-stu-id="a23ba-174">Inbound</span></span>  <br/> |
|<span data-ttu-id="a23ba-175">**서비스**</span><span class="sxs-lookup"><span data-stu-id="a23ba-175">**Service**</span></span> <br/> |<span data-ttu-id="a23ba-176">Exchange 하이브리드</span><span class="sxs-lookup"><span data-stu-id="a23ba-176">Exchange Hybrid</span></span>  <br/> |
|<span data-ttu-id="a23ba-177">**공용 Office 365 끝점 (원본)**</span><span class="sxs-lookup"><span data-stu-id="a23ba-177">**Public Office 365 endpoint (source)**</span></span> <br/> |<span data-ttu-id="a23ba-178">Exchange Online (IP 주소)</span><span class="sxs-lookup"><span data-stu-id="a23ba-178">Exchange Online (IP addresses)</span></span>  <br/> |
|<span data-ttu-id="a23ba-179">**공용 온-프레미스 끝점 (대상)**</span><span class="sxs-lookup"><span data-stu-id="a23ba-179">**Public On-Premises Endpoint (destination)**</span></span> <br/> |<span data-ttu-id="a23ba-180">5.5.5.5</span><span class="sxs-lookup"><span data-stu-id="a23ba-180">5.5.5.5</span></span>  <br/> |
|<span data-ttu-id="a23ba-181">**공용 (인터넷) DNS 항목**</span><span class="sxs-lookup"><span data-stu-id="a23ba-181">**Public (Internet) DNS entry**</span></span> <br/> |<span data-ttu-id="a23ba-182">Autodiscover.contoso.com</span><span class="sxs-lookup"><span data-stu-id="a23ba-182">Autodiscover.contoso.com</span></span>  <br/> |
|<span data-ttu-id="a23ba-183">**이 온-프레미스 끝점에 사용할 다른 (비-Office 365) Microsoft 서비스에 의해**</span><span class="sxs-lookup"><span data-stu-id="a23ba-183">**Will this on-premises endpoint be used for by other (non-Office 365) Microsoft services**</span></span> <br/> |<span data-ttu-id="a23ba-184">아니요</span><span class="sxs-lookup"><span data-stu-id="a23ba-184">No</span></span>  <br/> |
|<span data-ttu-id="a23ba-185">**인터넷에서 사용자/시스템에 사용 될이 온-프레미스 끝점**</span><span class="sxs-lookup"><span data-stu-id="a23ba-185">**Will this on-premises endpoint be used by users/systems on the Internet**</span></span> <br/> |<span data-ttu-id="a23ba-186">예</span><span class="sxs-lookup"><span data-stu-id="a23ba-186">Yes</span></span>  <br/> |
|<span data-ttu-id="a23ba-187">**공용 끝점을 통해 게시 되는 내부 시스템**</span><span class="sxs-lookup"><span data-stu-id="a23ba-187">**Internal systems published through public endpoints**</span></span> <br/> |<span data-ttu-id="a23ba-188">Exchange Server 클라이언트 액세스 역할 (온-프레미스) 192.168.101, 192.168.102, 192.168.103</span><span class="sxs-lookup"><span data-stu-id="a23ba-188">Exchange Server client access role (on-premises) 192.168.101, 192.168.102, 192.168.103</span></span>  <br/> |
|<span data-ttu-id="a23ba-189">**공용 끝점의 IP 광고**</span><span class="sxs-lookup"><span data-stu-id="a23ba-189">**IP advertisement of the public endpoint**</span></span> <br/> |<span data-ttu-id="a23ba-190">**인터넷**: 5.5.0.0/16</span><span class="sxs-lookup"><span data-stu-id="a23ba-190">**To Internet**: 5.5.0.0/16</span></span>  <br/> <span data-ttu-id="a23ba-191">**ExpressRoute를**: 5.5.5.0/24</span><span class="sxs-lookup"><span data-stu-id="a23ba-191">**To ExpressRoute**: 5.5.5.0/24</span></span>  <br/> |
|<span data-ttu-id="a23ba-192">**보안/경계 컨트롤**</span><span class="sxs-lookup"><span data-stu-id="a23ba-192">**Security/Perimeter Controls**</span></span> <br/> |<span data-ttu-id="a23ba-193">**인터넷 경로**: DeviceID_002</span><span class="sxs-lookup"><span data-stu-id="a23ba-193">**Internet path**: DeviceID_002</span></span>  <br/> <span data-ttu-id="a23ba-194">**ExpressRoute 경로**: DeviceID_003</span><span class="sxs-lookup"><span data-stu-id="a23ba-194">**ExpressRoute path**: DeviceID_003</span></span>  <br/> |
|<span data-ttu-id="a23ba-195">**고가용성**</span><span class="sxs-lookup"><span data-stu-id="a23ba-195">**High Availability**</span></span> <br/> |<span data-ttu-id="a23ba-196">액티브/액티브 2 지리적으로 분산 중복 별</span><span class="sxs-lookup"><span data-stu-id="a23ba-196">Active/Active across 2 geo-redundant</span></span>  <br/> <span data-ttu-id="a23ba-197">ExpressRoute 회로-시카고 및 달라스</span><span class="sxs-lookup"><span data-stu-id="a23ba-197">ExpressRoute circuits - Chicago and Dallas</span></span>  <br/> |
|<span data-ttu-id="a23ba-198">**경로 대칭 컨트롤**</span><span class="sxs-lookup"><span data-stu-id="a23ba-198">**Path symmetry control**</span></span> <br/> |<span data-ttu-id="a23ba-199">**방법**: NAT 원본</span><span class="sxs-lookup"><span data-stu-id="a23ba-199">**Method**: Source NAT</span></span>  <br/> <span data-ttu-id="a23ba-200">**인터넷 경로**: 소스 NAT 인바운드 192.168.5.5에 대 한 연결</span><span class="sxs-lookup"><span data-stu-id="a23ba-200">**Internet path**: Source NAT inbound connections to 192.168.5.5</span></span>  <br/> |<span data-ttu-id="a23ba-201">**ExpressRoute 경로**: 192.168.1.0 (Chicago) 및 192.168.2.0 (달라스)에 대 한 원본 NAT 연결</span><span class="sxs-lookup"><span data-stu-id="a23ba-201">**ExpressRoute path**: Source NAT connections to 192.168.1.0 (Chicago) and 192.168.2.0 (Dallas)</span></span>  <br/> |

<span data-ttu-id="a23ba-202">아웃 바운드만 되는 서비스의 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-202">Here's a sample of a service that is outbound only:</span></span>
|<span data-ttu-id="a23ba-203">**Connection 속성**</span><span class="sxs-lookup"><span data-stu-id="a23ba-203">**Connection property**</span></span>|<span data-ttu-id="a23ba-204">**값**</span><span class="sxs-lookup"><span data-stu-id="a23ba-204">**Value**</span></span>|
|:-----|:-----|
|<span data-ttu-id="a23ba-205">**네트워크 트래픽 방향**</span><span class="sxs-lookup"><span data-stu-id="a23ba-205">**Network traffic direction**</span></span> <br/> |<span data-ttu-id="a23ba-206">아웃바운드</span><span class="sxs-lookup"><span data-stu-id="a23ba-206">Outbound</span></span>  <br/> |
|<span data-ttu-id="a23ba-207">**서비스**</span><span class="sxs-lookup"><span data-stu-id="a23ba-207">**Service**</span></span> <br/> |<span data-ttu-id="a23ba-208">SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="a23ba-208">SharePoint Online</span></span>  <br/> |
|<span data-ttu-id="a23ba-209">**온-프레미스 끝점 (원본)**</span><span class="sxs-lookup"><span data-stu-id="a23ba-209">**On-premises endpoint (source)**</span></span> <br/> |<span data-ttu-id="a23ba-210">사용자 워크스테이션</span><span class="sxs-lookup"><span data-stu-id="a23ba-210">User workstation</span></span>  <br/> |
|<span data-ttu-id="a23ba-211">**공용 Office 365 끝점 (대상)**</span><span class="sxs-lookup"><span data-stu-id="a23ba-211">**Public Office 365 endpoint (destination)**</span></span> <br/> |<span data-ttu-id="a23ba-212">SharePoint Online (IP 주소)</span><span class="sxs-lookup"><span data-stu-id="a23ba-212">SharePoint Online (IP addresses)</span></span>  <br/> |
|<span data-ttu-id="a23ba-213">**공용 (인터넷) DNS 항목**</span><span class="sxs-lookup"><span data-stu-id="a23ba-213">**Public (Internet) DNS entry**</span></span> <br/> |<span data-ttu-id="a23ba-214">\*. sharepoint.com (및 추가 Fqdn)</span><span class="sxs-lookup"><span data-stu-id="a23ba-214">\*.sharepoint.com (and additional FQDNs)</span></span>  <br/> |
|<span data-ttu-id="a23ba-215">**CDN 조회**</span><span class="sxs-lookup"><span data-stu-id="a23ba-215">**CDN Referrals**</span></span> <br/> |<span data-ttu-id="a23ba-216">cdn.sharepointonline.com (또는 추가 Fqdn)-CDN 공급자가 유지 관리 하는 IP 주소)</span><span class="sxs-lookup"><span data-stu-id="a23ba-216">cdn.sharepointonline.com (and additional FQDNs) - IP addresses maintained by CDN providers)</span></span>  <br/> |
|<span data-ttu-id="a23ba-217">**IP 광고 및 NAT 사용**</span><span class="sxs-lookup"><span data-stu-id="a23ba-217">**IP advertisement and NAT in use**</span></span> <br/> |<span data-ttu-id="a23ba-218">**인터넷 경로/소스 NAT**: 1.1.1.0/24</span><span class="sxs-lookup"><span data-stu-id="a23ba-218">**Internet path/Source NAT**: 1.1.1.0/24</span></span>  <br/> <span data-ttu-id="a23ba-219">**ExpressRoute 경로/소스 NAT**: 1.1.2.0/24 (Chicago) 및 1.1.3.0/24 (달라스)</span><span class="sxs-lookup"><span data-stu-id="a23ba-219">**ExpressRoute path/Source NAT**: 1.1.2.0/24 (Chicago) and 1.1.3.0/24 (Dallas)</span></span>  <br/> |
|<span data-ttu-id="a23ba-220">**연결 메서드**</span><span class="sxs-lookup"><span data-stu-id="a23ba-220">**Connectivity method**</span></span> <br/> |<span data-ttu-id="a23ba-221">**인터넷**: 계층 7 프록시 (.pac 파일)를 통해</span><span class="sxs-lookup"><span data-stu-id="a23ba-221">**Internet**: via layer 7 proxy (.pac file)</span></span>  <br/> <span data-ttu-id="a23ba-222">**ExpressRoute**: 직접 라우팅 (프록시 없음)</span><span class="sxs-lookup"><span data-stu-id="a23ba-222">**ExpressRoute**: direct routing (no proxy)</span></span>  <br/> |
|<span data-ttu-id="a23ba-223">**보안/경계 컨트롤**</span><span class="sxs-lookup"><span data-stu-id="a23ba-223">**Security/Perimeter Controls**</span></span> <br/> |<span data-ttu-id="a23ba-224">**인터넷 경로**: DeviceID_002</span><span class="sxs-lookup"><span data-stu-id="a23ba-224">**Internet path**: DeviceID_002</span></span>  <br/> <span data-ttu-id="a23ba-225">**ExpressRoute 경로**: DeviceID_003</span><span class="sxs-lookup"><span data-stu-id="a23ba-225">**ExpressRoute path**: DeviceID_003</span></span>  <br/> |
|<span data-ttu-id="a23ba-226">**고가용성**</span><span class="sxs-lookup"><span data-stu-id="a23ba-226">**High Availability**</span></span> <br/> |<span data-ttu-id="a23ba-227">**인터넷 경로**: 중복 인터넷 송신</span><span class="sxs-lookup"><span data-stu-id="a23ba-227">**Internet path**: Redundant internet egress</span></span>  <br/> <span data-ttu-id="a23ba-228">**ExpressRoute 경로**: 2 지리적으로 분산 중복 ExpressRoute 회로-시카고 및 달라스 별 액티브/액티브 ' 곤란 한 ' 라우팅</span><span class="sxs-lookup"><span data-stu-id="a23ba-228">**ExpressRoute path**: Active/Active 'hot potato' routing across 2 geo-redundant ExpressRoute circuits - Chicago and Dallas</span></span>  <br/> |
|<span data-ttu-id="a23ba-229">**경로 대칭 컨트롤**</span><span class="sxs-lookup"><span data-stu-id="a23ba-229">**Path symmetry control**</span></span> <br/> |<span data-ttu-id="a23ba-230">**방법**: 모든 연결에 대 한 NAT 소스</span><span class="sxs-lookup"><span data-stu-id="a23ba-230">**Method**: Source NAT for all connections</span></span>  <br/> |

### <a name="your-network-topology-design-with-regional-connectivity"></a><span data-ttu-id="a23ba-231">지역 연결이 설정 된 네트워크 토폴로지 디자인</span><span class="sxs-lookup"><span data-stu-id="a23ba-231">Your network topology design with regional connectivity</span></span>
<span data-ttu-id="a23ba-232"><a name="topology"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-232"></span></span>

<span data-ttu-id="a23ba-p118">서비스 및 해당 연결 된 네트워크 트래픽 흐름을 이해 하 고 나면 이러한 새 연결 요구 사항을 통합 하 고 Office 365에 대 한 ExpressRoute를 사용 하 여 할 변경 된 내용을 설명 하는 네트워크 다이어그램을 만들 수 있습니다. 다이어그램에는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p118">Once you understand the services and their associated network traffic flows, you can create a network diagram that incorporates these new connectivity requirements and illustrates the changes you'll make to use ExpressRoute for Office 365. Your diagram should include:</span></span>
  
1. <span data-ttu-id="a23ba-235">모든 사용자 위치에서 Office 365 및 기타 서비스를 액세스 하는 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-235">All user locations where Office 365 and other services will be accessed from.</span></span>

2. <span data-ttu-id="a23ba-236">모든 인터넷 및 ExpressRoute 탈출 포인트입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-236">All internet and ExpressRoute egress points.</span></span>

3. <span data-ttu-id="a23ba-237">아웃 바운드 및 인바운드 하는 모든 장치 라우터, 방화벽, 응용 프로그램 프록시 서버 및 침입 검색/방지를 포함 하는 네트워크 및 로그 아웃할 때 연결을 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-237">All outbound and inbound devices that manage connectivity in and out of the network, including routers, firewalls, application proxy servers, and intrusion detection/prevention.</span></span>

4. <span data-ttu-id="a23ba-238">ADFS 웹 응용 프로그램 프록시 서버에서 연결을 허용 하는 내부 ad FS 서버와 같은 모든 인바운드 트래픽에 대 한 내부 대상입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-238">Internal destinations for all inbound traffic, such as internal ADFS servers that accept connections from the ADFS web application proxy servers.</span></span>

5. <span data-ttu-id="a23ba-239">광고가 모든 IP 서브넷의 카탈로그</span><span class="sxs-lookup"><span data-stu-id="a23ba-239">Catalog of all IP subnets that will be advertised</span></span>

6. <span data-ttu-id="a23ba-240">사용자가 Office 365에서 액세스 하 고는 모임 나열 하는 여기에서 각 위치를 식별-나에 게 ExpressRoute에 사용 되는 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-240">Identify each location where people will access Office 365 from and list the meet-me locations that will be used for ExpressRoute.</span></span>

7. <span data-ttu-id="a23ba-241">위치 및 일부 ExpressRoute에서 얻은 Microsoft IP 접두사 수락 수를 내부 네트워크 토폴로지를 필터링 하 고 전파 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-241">Locations and portions of your internal network topology, where Microsoft IP prefixes learned from ExpressRoute will be accepted, filtered and propagated to.</span></span>

8. <span data-ttu-id="a23ba-242">네트워크 토폴로지는 각 네트워크 세그먼트의 지리적 위치 및 ExpressRoute 및/또는 인터넷을 통해 Microsoft 네트워크에 연결 하는 방법을 보여주는 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-242">The network topology should illustrate the geographic location of each network segment and how it connects to the Microsoft network over ExpressRoute and/or the Internet.</span></span>

<span data-ttu-id="a23ba-243">아래 다이어그램 있는 사람들이 사용할 Office 365에서 Office 365로 인바운드 및 아웃 바운드 라우팅 광고와 함께 각 위치를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-243">The diagram below shows each location where people will be using Office 365 from along with the inbound and outbound routing advertisements to Office 365.</span></span>
  
![ExpressRoute 지역 지리적 모임-나에 게](media/d866b36b-49bf-416b-af1b-d054e24989d2.png)
  
<span data-ttu-id="a23ba-245">아웃 바운드 트래픽에 대 한 사람 세가지 방법 중 하나에서 Office 365에 액세스 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-245">For outbound traffic, the people access Office 365 in one of three ways:</span></span>
  
1. <span data-ttu-id="a23ba-246">모임을 통해-me 캘리포니아의 사용자를 위한 북미에서의 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-246">Through a meet-me location in North America for the people in California.</span></span>

2. <span data-ttu-id="a23ba-247">모임을 통해-me 홍콩 특별 행정구의 사용자를 위한 홍콩 특별 행정구에서의 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-247">Through a meet-me location in Hong Kong for the people in Hong Kong.</span></span>

3. <span data-ttu-id="a23ba-248">여기에서 더 적은 수의 사용자 및 없음 ExpressRoute 회로 프로 비전 방글라데시에서 인터넷을 통해.</span><span class="sxs-lookup"><span data-stu-id="a23ba-248">Through the internet in Bangladesh where there are fewer people and no ExpressRoute circuit provisioned.</span></span>

![지역 다이어그램에 대 한 아웃 바운드 연결](media/8319943d-08f0-4781-9ef3-d23de2ad4671.png)
  
<span data-ttu-id="a23ba-250">마찬가지로, Office 365의 인바운드 네트워크 트래픽을 세가지 방법 중 하나를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-250">Similarly, the inbound network traffic from Office 365 returns in one of three ways:</span></span>
  
1. <span data-ttu-id="a23ba-251">모임을 통해-me 캘리포니아의 사용자를 위한 북미에서의 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-251">Through a meet-me location in North America for the people in California.</span></span>

2. <span data-ttu-id="a23ba-252">모임을 통해-me 홍콩 특별 행정구의 사용자를 위한 홍콩 특별 행정구에서의 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-252">Through a meet-me location in Hong Kong for the people in Hong Kong.</span></span>

3. <span data-ttu-id="a23ba-253">여기에서 더 적은 수의 사용자 및 없음 ExpressRoute 회로 프로 비전 방글라데시에서 인터넷을 통해.</span><span class="sxs-lookup"><span data-stu-id="a23ba-253">Through the internet in Bangladesh where there are fewer people and no ExpressRoute circuit provisioned.</span></span>

![지역 다이어그램에 대 한 인바운드 연결](media/d6d6160d-bf28-4de3-a787-186c7432b306.png)
  
### <a name="determine-the-appropriate-meet-me-location"></a><span data-ttu-id="a23ba-255">적절 한 모임 결정-나에 게 위치</span><span class="sxs-lookup"><span data-stu-id="a23ba-255">Determine the appropriate meet-me location</span></span>

<span data-ttu-id="a23ba-p119">모임 선택-나에 게 ExpressRoute 회로 Microsoft 네트워크에 네트워크를 연결 하는 실제 위치는 위치는 사용자가에서 Office 365에 액세스할 수 있는 위치에 따라 영향을 받습니다. 제공 하는 SaaS,으로 Office 365 IaaS 또는 PaaS 지역 모델에서 Azure는 동일한 방식으로 작동 하지 않습니다. 대신 Office 365 사용자가 여러 데이터 센터와 같은 위치 또는 사용자의 테 넌 트 호스팅되는 지역에 반드시 아닐 수도 있는 영역 끝점에 연결할 할 수 있는 된 분산된 집합, 공동 작업 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p119">The selection of meet-me locations, which are the physical location where your ExpressRoute circuit connects your network to the Microsoft network, is influenced by the locations where people will access Office 365 from. As a SaaS offering, Office 365 does not operate under the IaaS or PaaS regional model in the same way Azure does. Instead, Office 365 is a distributed set of collaboration services, where users may need to connect to endpoints across multiple datacenters and regions, which may not necessarily be in the same location or region where the user's tenant is hosted.</span></span>
  
<span data-ttu-id="a23ba-p120">즉, 모임을 선택할 때 해야하는 가장 중요 한 고려 사항-나에 게 Office 365에 대 한 ExpressRoute에 대 한 위치에서 조직의 사용자가 연결 하는 위치는 합니다. 일반적인 권장 사항과 최적의 Office 365 연결은 Office 365 서비스에 대 한 사용자 요청 전달 됩니다 Microsoft 네트워크에 가장 짧은 네트워크 경로 통해 있도록 라우팅, 구현에 대 한이 또한 되 고 이라고 ' 곤란 한 ' 라우팅. 대부분의 Office 365 사용자가 하나 또는 두 위치에 있으면 모임을 선택 하는 예-나에 게 해당 사용자의 위치를 가장 가까운 근접 한 위치에 있는 위치 최적의 디자인을 만듭니다. 회사의 많은 다른 지역에서 대규모 사용자 수가 있으면 하려는 경우 여러 ExpressRoute 회로 것을 고려 하 고 충족-나에 게 위치입니다. 일부 사용자 위치에 대 한 작업 Microsoft 네트워크 및 Office 365에 대 한 가장 짧은/가장 최적의 경로은 내부 사용자 WAN 및 ExpressRoute 모임 통해 되지 않을 수 있습니다-가리키는 me 하지만 인터넷을 통해 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p120">This means the most important consideration you need to make when selecting meet-me locations for ExpressRoute for Office 365 is where the people in your organization will be connecting from. The general recommendation for optimal Office 365 connectivity is implement routing, so that user requests to Office 365 services are handed off into the Microsoft network over the shortest network path, this is also often being referred to as 'hot potato' routing. For example, if most of the Office 365 users are in one or two locations, selecting meet-me locations that are in the closest proximity to the location of those users will create the optimal design. If your company has large user populations in many different regions, you may want to consider having multiple ExpressRoute circuits and meet-me locations. For some of your user locations, the shortest/most optimal path into Microsoft network and Office 365, may not be through your internal WAN and ExpressRoute meet-me points, but via the Internet.</span></span>
  
<span data-ttu-id="a23ba-p121">종종 시간, 여러 모임 가지-내 사용자에 게 상대 근접 영역 내에서 선택할 수 있는 위치입니다. 결정을 안내 하는 다음 표를 채웁니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p121">Often times, there are multiple meet-me locations that could be selected within a region with relative proximity to your users. Fill out the following table to guide your decisions.</span></span>

|<span data-ttu-id="a23ba-266">**계획 된 ExpressRoute 모임-me 캘리포니아 및 뉴욕 위치에**</span><span class="sxs-lookup"><span data-stu-id="a23ba-266">**Planned ExpressRoute meet-me locations in California and New York**</span></span>||
|:-----|:-----|
|<span data-ttu-id="a23ba-267">위치</span><span class="sxs-lookup"><span data-stu-id="a23ba-267">Location</span></span>  <br/> |<span data-ttu-id="a23ba-268">사용자 수</span><span class="sxs-lookup"><span data-stu-id="a23ba-268">Number of people</span></span>  <br/> |<span data-ttu-id="a23ba-269">인터넷 송신을 통해 Microsoft 네트워크 대기 시간 예상</span><span class="sxs-lookup"><span data-stu-id="a23ba-269">Expected latency to Microsoft network over Internet egress</span></span>  <br/> |<span data-ttu-id="a23ba-270">ExpressRoute 통해 Microsoft 네트워크에 예상된 대기 시간</span><span class="sxs-lookup"><span data-stu-id="a23ba-270">Expected latency to Microsoft network over ExpressRoute</span></span>  <br/> |
|<span data-ttu-id="a23ba-271">로스앤젤레스</span><span class="sxs-lookup"><span data-stu-id="a23ba-271">Los Angeles</span></span>  <br/> |<span data-ttu-id="a23ba-272">10,000</span><span class="sxs-lookup"><span data-stu-id="a23ba-272">10,000</span></span>  <br/> |<span data-ttu-id="a23ba-273">~ 15ms</span><span class="sxs-lookup"><span data-stu-id="a23ba-273">~15ms</span></span>  <br/> |<span data-ttu-id="a23ba-274">~ 10ms (실리콘 밸리)를 통해</span><span class="sxs-lookup"><span data-stu-id="a23ba-274">~10ms (via Silicon Valley)</span></span>  <br/> |
|<span data-ttu-id="a23ba-275">워싱턴 DC</span><span class="sxs-lookup"><span data-stu-id="a23ba-275">Washington DC</span></span>  <br/> |<span data-ttu-id="a23ba-276">15, 000</span><span class="sxs-lookup"><span data-stu-id="a23ba-276">15,000</span></span>  <br/> |<span data-ttu-id="a23ba-277">~ 20ms</span><span class="sxs-lookup"><span data-stu-id="a23ba-277">~20ms</span></span>  <br/> |<span data-ttu-id="a23ba-278">~ 10ms (뉴욕)를 통해</span><span class="sxs-lookup"><span data-stu-id="a23ba-278">~10ms (via New York)</span></span>  <br/> |
|<span data-ttu-id="a23ba-279">달라스</span><span class="sxs-lookup"><span data-stu-id="a23ba-279">Dallas</span></span>  <br/> |<span data-ttu-id="a23ba-280">5,000</span><span class="sxs-lookup"><span data-stu-id="a23ba-280">5,000</span></span>  <br/> |<span data-ttu-id="a23ba-281">~ 15ms</span><span class="sxs-lookup"><span data-stu-id="a23ba-281">~15ms</span></span>  <br/> |<span data-ttu-id="a23ba-282">~ 40ms (뉴욕)를 통해</span><span class="sxs-lookup"><span data-stu-id="a23ba-282">~40ms (via New York)</span></span>  <br/> |

<span data-ttu-id="a23ba-p122">Office 365 영역이 표시 되는 전역 네트워크 아키텍처, ExpressRoute 네트워크 서비스 공급자 충족 되 면-나에 게 위치에 따라 사용자의 수량 및 위치, 개발 된, 모든 최적화를 설정할 수 경우 식별 하기 위해 사용할 수 있습니다. 또한 표시 될 전역 헤어핀 네트워크 연결은 모임에 나오고 트래픽을 멀리 떨어져 있는 위치를 회람 하는 위치-나에 게 위치입니다. 글로벌 네트워크에서 헤어핀 계속 하기 전에 설정을 수 해야 발견 되는지 합니다. 다른 모임 찾기 하나-나에 게 위치 또는 사용 하 여 선택적 인터넷 소 탈출 포인트의 헤어핀을 방지 하기 위해 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p122">Once the global network architecture showing the Office 365 region, ExpressRoute network service provider meet-me locations, and the quantity of people by location has been developed, it can be used to identify if any optimizations can be made. It may also show global hairpin network connections where traffic routes to a distant location in order to get the meet-me location. If a hairpin on the global network is discovered it should be remediated before continuing. Either find another meet-me location, or use selective Internet breakout egress points to avoid the hairpin.</span></span>
  
<span data-ttu-id="a23ba-p123">첫번째 다이어그램 북미에서 두 실제 위치와 고객의 예를 보여줍니다. 사무실 위치, Office 365 테 넌 트 위치에 대 한 정보를 볼 수 및 ExpressRoute에 대 한 여러 선택 충족-나에 게 위치입니다. 이 예제에서는 고객의 모임이 선택한-me 순서로 두 원칙에 기반 하는 위치:</span><span class="sxs-lookup"><span data-stu-id="a23ba-p123">The first diagram, shows an example of a customer with two physical locations in North America. You can see the information about office locations, Office 365 tenant locations, and several choices for ExpressRoute meet-me locations. In this example, the customer has selected the meet-me location based on two principles, in order:</span></span>
  
1. <span data-ttu-id="a23ba-290">조직에서 사용자를 가장 가까운 근접 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-290">Closest proximity to the people in their organization.</span></span>

2. <span data-ttu-id="a23ba-291">Office 365가 호스팅되는 Microsoft 데이터 센터에 근접에서 가장 가까운 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-291">Closest in proximity to a Microsoft datacenter where Office 365 is hosted.</span></span>

![ExpressRoute 미국 지역 모임-나에 게](media/5ec38274-b317-4ec1-91c8-90c2a7fd32ca.png)
  
<span data-ttu-id="a23ba-p124">약간이 개념을 확장 하면 한 자세한 내용은, 두번째 다이어그램에서 볼 수 있는 예제 다국적 고객 비슷한 정보 및 의사 결정에 직면 합니다. 이 고객 지역에 자신의 메모리 사용량이 증가에 초점을 맞춥니다 10 명의 소규모 팀만 함께 방글라데시에 작은 사무실을 있습니다. 모임 방법이-나에 게 체나 및 Office 365와 Microsoft 데이터 센터에서의 위치에에서 호스트 체나 하므로 모임-나에 게 위치는 의미가 있습니다. 그러나 10 명의 사용자에 대 한 추가 회로 비용을 들이지가 부담을 줄여주. 네트워크 연결을 보면으로 네트워크를 통해 네트워크 트래픽을 보낼 관련 된 대기 시간을 다른 ExpressRoute 회로 구입 하려면 자본 지출 보다 더 효율적으로 되었는지 확인 하려면 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p124">Expanding this concept slightly further, the second diagram shows an example multi-national customer faced with similar information and decision making. This customer has a small office in Bangladesh with only a small team of ten people focused on growing their footprint in the region. There is a meet-me location in Chennai and a Microsoft datacenter with Office 365 hosted in Chennai so a meet-me location would make sense; however, for ten people, the expense of the additional circuit is burdensome. As you look at your network, you'll need to determine if the latency involved in sending your network traffic across your network is more effective than spending the capital to acquire another ExpressRoute circuit.</span></span>
  
<span data-ttu-id="a23ba-297">방글라데시에 10 명의 사용자가 자신의 네트워크 트래픽을 소개 다이어그램의 결과 따르면 하 고 재현가 내부 네트워크에서 라우팅 것 보다 Microsoft 네트워크에 인터넷을 통해 전송 된 성능을 개선 하려면 위의 겪을 수는 또는 아래 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-297">Alternatively, the ten people in Bangladesh may experience better performance with their network traffic sent over the internet to the Microsoft network than they would routing on their internal network as we showed in the introductory diagrams and reproduced below.</span></span>
  
![지역 다이어그램에 대 한 아웃 바운드 연결](media/8319943d-08f0-4781-9ef3-d23de2ad4671.png)
  
## <a name="create-your-expressroute-for-office-365-implementation-plan"></a><span data-ttu-id="a23ba-299">Office 365 구현 계획에 대 한 사용자 ExpressRoute 만들기</span><span class="sxs-lookup"><span data-stu-id="a23ba-299">Create your ExpressRoute for Office 365 implementation plan</span></span>
<span data-ttu-id="a23ba-300"><a name="implementation"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-300"></span></span>

<span data-ttu-id="a23ba-301">구현 계획에는 다음과 같은 네트워크에서 다른 모든 인프라를 구성 하는의 세부 정보 뿐아니라 ExpressRoute 구성의 기술 정보 포괄적 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-301">Your implementation plan should encompass both the technical details of configuring ExpressRoute as well as the details of configuring all the other infrastructure on your network, such as the following.</span></span>
  
- <span data-ttu-id="a23ba-302">ExpressRoute와 인터넷 사이의 분할 하는 서비스를 계획 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-302">Plan which services split between ExpressRoute and Internet.</span></span>

- <span data-ttu-id="a23ba-303">대역폭, 보안, 고가용성 및 장애 조치에 대 한 계획 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-303">Plan for bandwidth, security, high availability and failover.</span></span>

- <span data-ttu-id="a23ba-304">인바운드 및 아웃 바운드 라우팅, 다른 위치에 대 한 적절 한 라우팅 경로 최적화를 포함 하 여 디자인</span><span class="sxs-lookup"><span data-stu-id="a23ba-304">Design inbound and outbound routing, including proper routing path optimizations for different locations</span></span>

- <span data-ttu-id="a23ba-305">네트워크에 ExpressRoute 경로 광고가 정도 결정 하 고 인터넷 이나 ExpressRoute 경로;를 선택 하는 클라이언트에 대 한 메커니즘 이란 라우팅 또는 응용 프로그램 프록시를 직접 예입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-305">Decide how far ExpressRoute routes will be advertised into your network and what is the mechanism for clients to select Internet or ExpressRoute path; for example, direct routing or application proxy.</span></span>

- <span data-ttu-id="a23ba-306">[보낸 사람이 정책 프레임 워크](https://technet.microsoft.com/library/dn789058%28v=exchg.150%29.aspx) 항목을 포함 하 여 DNS 레코드 변경을 계획 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-306">Plan DNS record changes, including [Sender Policy Framework](https://technet.microsoft.com/library/dn789058%28v=exchg.150%29.aspx) entries.</span></span>

- <span data-ttu-id="a23ba-307">아웃 바운드 및 인바운드 원본 NAT.를 포함 하 여 NAT 전략 계획</span><span class="sxs-lookup"><span data-stu-id="a23ba-307">Plan NAT strategy including outbound and inbound source NAT.</span></span>

### <a name="plan-your-routing-with-both-internet-and-expressroute-network-paths"></a><span data-ttu-id="a23ba-308">인터넷 및 ExpressRoute 네트워크 경로 모두 사용 하 여 라우팅 계획</span><span class="sxs-lookup"><span data-stu-id="a23ba-308">Plan your routing with both internet and ExpressRoute network paths</span></span>
<span data-ttu-id="a23ba-309"><a name="paths"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-309"></span></span>

- <span data-ttu-id="a23ba-310">초기 배포에 대 한 인바운드 전자 메일 또는 하이브리드 연결 등의 모든 인바운드 서비스는 인터넷을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-310">For your initial deployment, all inbound services, such as inbound email or hybrid connectivity, are recommended to use the internet.</span></span>

- <span data-ttu-id="a23ba-311">최종 사용자 클라이언트 LAN 라우팅, [PAC/WPAD 파일을 구성](https://aka.ms/manageo365endpoints)하는 등, 기본 경로, 프록시 서버 및 BGP 경로 광고를 계획 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-311">Plan end user client LAN routing, such as [configuring a PAC/WPAD file](https://aka.ms/manageo365endpoints), default route, proxy servers, and BGP route advertisements.</span></span>

- <span data-ttu-id="a23ba-312">경계 라우팅, 프록시 서버, 방화벽 및 클라우드 프록시를 포함 하 여 계획 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-312">Plan perimeter routing, including proxy servers, firewalls, and cloud proxies.</span></span>

### <a name="plan-your-bandwidth-security-high-availability-and-failover"></a><span data-ttu-id="a23ba-313">대역폭, 보안, 고가용성 및 장애 조치 계획</span><span class="sxs-lookup"><span data-stu-id="a23ba-313">Plan your bandwidth, security, high availability and failover</span></span>
<span data-ttu-id="a23ba-314"><a name="availability"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-314"></span></span>

<span data-ttu-id="a23ba-p125">Office 365의 각 주요 작업에 필요한 대역폭에 대 한 계획을 만듭니다. 비즈니스 온라인 대역폭 요구 사항에 대 한 Exchange Online, SharePoint Online 및 Skype를 개별적으로 예측 합니다. Exchange Online 및 시작 위치로 비즈니스용 Skype 제공 하 고 예측 계산기를 사용할 수 있습니다. 그러나 사용자 프로필 및 위치의 대표 샘플으로 파일럿 테스트는 완벽 하 게 하 여 조직의 대역폭 요구 사항을 이해 하는 데 필요한.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p125">Create a plan for bandwidth required for each major Office 365 workload. Separately estimate Exchange Online, SharePoint Online, and Skype for Business Online bandwidth requirements. You can use the estimation calculators we've provided for Exchange Online and Skype for Business as a starting place; however, a pilot test with a representative sample of the user profiles and locations is required to fully understand the bandwidth needs of your organization.</span></span>
  
<span data-ttu-id="a23ba-318">각 인터넷 및 계획에 ExpressRoute 탈출 위치에서 보안을 처리 하는 방법을 추가, Office 365에 대 한 모든 ExpressRoute 연결 공용 피어 링을 사용 하 고 외부에 연결 하 여 회사 보안 정책에 따라 보호할 수는 여전히 기억 경고 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-318">Add how security is handled at each internet and ExpressRoute egress location to your plan, remember all ExpressRoute connections to Office 365 use public peering and must still be secured in accordance with your company security policies of connecting to external networks.</span></span>
  
<span data-ttu-id="a23ba-319">세부 정보 계획에을 추가 하는 방법에 대 한 사용자의 영향 받게될 어떤 유형의 중단 및 방법을 사람들 가장 간단한 방식으로 전체 용량으로 해당 작업을 수행할 수 있게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-319">Add details to your plan about which people will be affected by what type of outage and how those people will be able to perform their work at full capacity in the simplest manner.</span></span>
  
#### <a name="plan-bandwidth-requirements-including-skype-for-business-requirements-on-jitter-latency-congestion-and-headroom"></a><span data-ttu-id="a23ba-320">Skype를 포함 하 여 지터, 대기 시간, 정체, 및 여유 공간을 갖는 비즈니스 요구 사항에 대 한 대역폭 요구 사항 계획</span><span class="sxs-lookup"><span data-stu-id="a23ba-320">Plan bandwidth requirements including Skype for Business requirements on Jitter, Latency, Congestion, and Headroom</span></span>
  
<span data-ttu-id="a23ba-321">비즈니스 온라인 용 Skype [미디어 품질 및 비즈니스 온라인 용 Skype에서 네트워크 연결 성능](https://support.office.com/article/Media-Quality-and-Network-Connectivity-Performance-in-Skype-for-Business-Online-5fe3e01b-34cf-44e0-b897-b0b2a83f0917)문서에서 자세히 설명 하는 특정 추가 네트워크 요구 사항에도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-321">Skype for Business Online also has specific additional network requirements which are detailed in the article [Media Quality and Network Connectivity Performance in Skype for Business Online](https://support.office.com/article/Media-Quality-and-Network-Connectivity-Performance-in-Skype-for-Business-Online-5fe3e01b-34cf-44e0-b897-b0b2a83f0917).</span></span>
  
<span data-ttu-id="a23ba-322">[Office 365에 대 한 ExpressRoute와 네트워크 계획](https://support.office.com/article/Network-planning-with-ExpressRoute-for-Office-365-103208f1-e788-4601-aa45-504f896511cd)에 대 한 **대역폭 Azure ExpressRoute에 대 한 계획** 섹션을 읽어보십시오.</span><span class="sxs-lookup"><span data-stu-id="a23ba-322">Read the section **Bandwidth planning for Azure ExpressRoute** in [Network planning with ExpressRoute for Office 365](https://support.office.com/article/Network-planning-with-ExpressRoute-for-Office-365-103208f1-e788-4601-aa45-504f896511cd).</span></span>
  
<span data-ttu-id="a23ba-323">파일럿 사용자와 대역폭 평가 수행 하는 경우에 가이드;를 사용 하 여 다음 작업을 수 있습니다. [초기 계획 및 성능 기록을 사용 하 여 office 365 성능 조정](https://support.office.com/article/Office-365-performance-tuning-using-baselines-and-performance-history-1492cb94-bd62-43e6-b8d0-2a61ed88ebae)합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-323">When performing a bandwidth assessment with your pilot users, you can use our guide; [Office 365 performance tuning using baselines and performance history](https://support.office.com/article/Office-365-performance-tuning-using-baselines-and-performance-history-1492cb94-bd62-43e6-b8d0-2a61ed88ebae).</span></span>
  
#### <a name="plan-for-high-availability-requirements"></a><span data-ttu-id="a23ba-324">고가용성 요구 사항에 대 한 계획</span><span class="sxs-lookup"><span data-stu-id="a23ba-324">Plan for high availability requirements</span></span>
  
<span data-ttu-id="a23ba-p126">요구 사항에 맞게 업데이트 된 네트워크 토폴로지 다이어그램에이 통합 하는 고가용성에 대 한 계획을 만듭니다. **고가용성 및 장애 조치 Azure ExpressRoute와** [Office 365에 대 한 ExpressRoute와 네트워크](https://support.office.com/article/Network-planning-with-ExpressRoute-for-Office-365-103208f1-e788-4601-aa45-504f896511cd)계획 섹션을 읽어보십시오.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p126">Create a plan for high availability to meet your needs and incorporate this into your updated network topology diagram. Read the section **High availability and failover with Azure ExpressRoute** in [Network planning with ExpressRoute for Office 365](https://support.office.com/article/Network-planning-with-ExpressRoute-for-Office-365-103208f1-e788-4601-aa45-504f896511cd).</span></span>
  
#### <a name="plan-for-network-security-requirements"></a><span data-ttu-id="a23ba-327">네트워크 보안 요구 사항에 대 한 계획</span><span class="sxs-lookup"><span data-stu-id="a23ba-327">Plan for network security requirements</span></span>
  
<span data-ttu-id="a23ba-p127">네트워크 보안 요구 사항을 충족 하 고 업데이트 된 네트워크 토폴로지 다이어그램에이 통합 하는 계획을 만듭니다. [Office 365에 대 한 ExpressRoute와 네트워크 계획](https://support.office.com/article/Network-planning-with-ExpressRoute-for-Office-365-103208f1-e788-4601-aa45-504f896511cd)에서 **Office 365 시나리오에 대 한 Azure ExpressRoute에 보안 컨트롤을 적용** 섹션을 읽어보십시오.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p127">Create a plan to meet your network security requirements and incorporate this into your updated network topology diagram. Read the section **Applying security controls to Azure ExpressRoute for Office 365 scenarios** in [Network planning with ExpressRoute for Office 365](https://support.office.com/article/Network-planning-with-ExpressRoute-for-Office-365-103208f1-e788-4601-aa45-504f896511cd).</span></span>
  
### <a name="design-outbound-service-connectivity"></a><span data-ttu-id="a23ba-330">아웃 바운드 서비스 연결을 디자인 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-330">Design outbound service connectivity</span></span>
<span data-ttu-id="a23ba-331"><a name="outbound"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-331"></span></span>

<span data-ttu-id="a23ba-p128">Office 365에 대 한 ExpressRoute 친숙 한 되지 않을 수 있는 *아웃 바운드* 네트워크 요구 사항에 있습니다. 특히, IP 주소 탭과 Office 365에 사용자 및 네트워크를 나타내는 역할을 Microsoft에 대 한 아웃 바운드 네트워크 연결에 대 한 원본 끝점을 아래에서 설명 하는 특정 요구 사항을 따라야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p128">ExpressRoute for Office 365 has  *outbound*  network requirements that may be unfamiliar. Specifically, the IP addresses that represent your users and networks to Office 365 and act as the source endpoints for outbound network connections to Microsoft must follow specific requirements outlined below.</span></span>
  
1. <span data-ttu-id="a23ba-334">끝점을 회사 또는 ExpressRoute 연결을 제공 하는 통신 회사에 등록 된 공용 IP 주소 여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-334">The endpoints must be public IP addresses, that are registered to your company or to carrier providing ExpressRoute connectivity to you.</span></span>

2. <span data-ttu-id="a23ba-335">끝점은 ExpressRoute 하 여 Microsoft에 보급 하 고 유효성을 검사 하 고 허용 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-335">The endpoints must be advertised to Microsoft and validated/accepted by ExpressRoute.</span></span>

3. <span data-ttu-id="a23ba-336">같거나 보다 기본 라우팅 메트릭 사용 하 여 인터넷에 끝점을 보급 수 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-336">The endpoints must not be advertised to the Internet with the same or more preferred routing metric.</span></span>

4. <span data-ttu-id="a23ba-337">끝점을 ExpressRoute을 통해 구성 되지 않은 Microsoft 서비스에 대 한 연결에 대 한 사용 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-337">The endpoints must not be used for connectivity to Microsoft services that are not configured over ExpressRoute.</span></span>

<span data-ttu-id="a23ba-p129">네트워크 디자인 이러한 요구 하는 경우는 높은 위험 수준에 사용자는 Office 365 및 holing 검정 경로 또는 비대칭 라우팅으로 인해 다른 Microsoft 서비스에 연결 오류를 경험 하는 방법이 있습니다. 이 Microsoft 서비스에 대 한 요청 ExpressRoute를 통해 라우팅되는 하지만 인터넷을 통해 다시 또는 그 반대의 경우도 마찬가지 응답 라우팅되는 응답 방화벽 같은 상태 저장 네트워크 장치에 의해 삭제 된 때 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p129">If your network design doesn't meet these requirements, there is a high risk your users will experience connectivity failures to Office 365 and other Microsoft services due to route black holing or asymmetric routing. This occurs when requests to Microsoft services are routed over ExpressRoute, but responses are routed back across the internet, or vice versa, and the responses are dropped by stateful network devices such as firewalls.</span></span>
  
<span data-ttu-id="a23ba-p130">위의 요구를 충족 하기 위해 사용할 수 있는 가장 일반적인 방법은 원본 네트워크의 일부로 구현 또는 ExpressRoute 사업자에서 제공 되는 NAT를 사용 하는 것입니다. 원본 NAT를 사용 하면 자세한 내용 및 개인 IP 주소 지정 인터넷 네트워크의 ExpressRoute에서 추상 및; 적절 한 IP 경로 광고를 더불어는 메커니즘을 제공 쉽게 경로 대칭 되도록 합니다. ExpressRoute 피어 링 위치에만 적용 되는 상태 저장 네트워크 장치를 사용 하는 경우에 각 ExpressRoute 경로 대칭 되도록 피어 링에 대 한 별도 NAT 풀을 구현 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p130">The most common method you can use to meet the above requirements is to use source NAT, either implemented as a part of your network or provided by your ExpressRoute carrier. Source NAT allows you to abstract the details and private IP addressing of your internet network from ExpressRoute and; coupled with proper IP route advertisements, provide an easy mechanism to ensure path symmetry. If you're using stateful network devices that are specific to ExpressRoute peering locations, you must implement separate NAT pools for each ExpressRoute peering to ensure path symmetry.</span></span>
  
<span data-ttu-id="a23ba-343">[ExpressRoute NAT 요구 사항](https://azure.microsoft.com/documentation/articles/expressroute-nat/)에 대해 자세히 알아보십시오.</span><span class="sxs-lookup"><span data-stu-id="a23ba-343">Read more about the [ExpressRoute NAT requirements](https://azure.microsoft.com/documentation/articles/expressroute-nat/).</span></span>
  
<span data-ttu-id="a23ba-344">네트워크 토폴로지 다이어그램 아웃 바운드 연결에 대 한 변경 내용에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-344">Add the changes for the outbound connectivity to the network topology diagram.</span></span>
  
### <a name="design-inbound-service-connectivity"></a><span data-ttu-id="a23ba-345">인바운드 서비스 연결을 디자인 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-345">Design inbound service connectivity</span></span>
<span data-ttu-id="a23ba-346"><a name="inbound"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-346"></span></span>

<span data-ttu-id="a23ba-p131">대부분의 Office 365 엔터프라이즈 배포 가정 합니다. 온-프레미스 서비스를 Office 365에서 인바운드 연결의 일부 폼과 같은 Exchange, SharePoint 및 Skype 비즈니스 하이브리드 시나리오, 사서함 마이그레이션 및 ADFS를 사용 하 여 인증에 대 한 인프라입니다. 때 ExpressRoute 아웃 바운드 연결에 대 한 온-프레미스 네트워크와 Microsoft 간의 추가 라우팅 경로 사용 하도록 설정 하면, 이러한 인바운드 연결 수 실수로 수의 영향을 받는 비대칭 라우팅, 이러한 흐름에 계속 포함 하려는 경우에 인터넷을 사용 합니다. 인터넷에 아무런 영향을 주지 기반 온-프레미스 시스템에 Office 365에서 인바운드 흐름은 되도록 아래에서 설명 하는 몇가지 예방 조치를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p131">The majority of enterprise Office 365 deployments assume some form of inbound connectivity from Office 365 to on-premises services, such as for Exchange, SharePoint, and Skype for Business hybrid scenarios, mailbox migrations, and authentication using ADFS infrastructure. When ExpressRoute you enable an additional routing path between your on-premises network and Microsoft for outbound connectivity, these inbound connections may inadvertently be impacted by asymmetric routing, even if you intend to have those flows continue to use the Internet. A few precautions described below are recommended to ensure there is no impact to Internet based inbound flows from Office 365 to on-premises systems.</span></span>
  
<span data-ttu-id="a23ba-p132">인바운드 네트워크 트래픽 흐름에 대 한 비대칭 라우팅의 위험을 최소화 하려면 ExpressRoute에 대 한 라우팅 가시성 없는 네트워크의 세그먼트로 라우팅되는 자신이 전에 원본 NAT를 사용 해야 모든 인바운드 연결 합니다. 들어오는 연결 함께 사용할 수 있는 네트워크 세그먼트에 원본 NAT 없이 ExpressRoute에 대 한 라우팅 가시성, 하는 경우 Office 365에서 발생 한 요청은 인터넷에서 들어감 하지만 Office 365로 돌아가 대 한 응답은 ExpressRoute 것을 선호 합니다. 비대칭 라우팅 인해 발생 하는 Microsoft 네트워크에 다시 네트워크 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p132">To minimize the risks of asymmetric routing for inbound network traffic flows, all of the inbound connections should use source NAT before they're routed into segments of your network which have routing visibility into ExpressRoute. If the incoming connections are allowed onto a network segment with routing visibility into ExpressRoute without source NAT, requests originating from Office 365 will enter from the internet, but the response going back to Office 365 will prefer the ExpressRoute network path back to the Microsoft network, causing asymmetric routing.</span></span>
  
<span data-ttu-id="a23ba-352">이 요구 사항을 충족 하기 위해 다음과 같은 구현 패턴 중 하나를 고려할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-352">You may consider one of the following implementation patterns to satisfy this requirement:</span></span>
  
1. <span data-ttu-id="a23ba-353">온-프레미스 시스템에 경로에 대 한 분산 인터넷에서 로드 또는 방화벽와 같은 네트워킹 장비를 사용 하 여 내부 네트워크로 라우팅되는 요청 하기 전에 원본 NAT를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-353">Perform source NAT before requests are routed into your internal network using networking equipment such as firewalls or load balancers on the path from the Internet to your on-premises systems.</span></span>

2. <span data-ttu-id="a23ba-354">확인 ExpressRoute 경로 네트워크 세그먼트 앞 등의 인바운드 서비스 서버를 종료 하거나 역방향 프록시 시스템을 인터넷 연결 상주 처리 위치에 전파 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-354">Ensure that ExpressRoute routes are not propagated to the network segments where inbound services, such as front end servers or reverse proxy systems, handling Internet connections reside.</span></span>

<span data-ttu-id="a23ba-355">명시적으로 네트워크에서 이러한 시나리오에 대 한 계정 하 고 모든 인바운드 네트워크 트래픽을 지속적으로 업데이트를 배포 하 고 비대칭 라우팅 운영 위험을 최소화 하기 위해 인터넷 도움말을 통해 흐릅니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-355">Explicitly accounting for these scenarios in your network and keeping all inbound network traffic flows over the Internet helps to minimize deployment and operational risk of asymmetric routing.</span></span>
  
<span data-ttu-id="a23ba-p133">사용할 수는 있지만 일부 인바운드 흐름 ExpressRoute 연결을 통해 직접를 선택할 수 있습니다. 이러한 시나리오에 대해 다음 추가 고려 사항을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p133">There may be cases where you may choose to direct some inbound flows over ExpressRoute connections. For these scenarios, take the following additional considerations into account.</span></span>
  
1. <span data-ttu-id="a23ba-p134">Office 365에만 대상 온-프레미스 끝점을 공용 Ip를 사용 하는 수 있습니다. 즉, 온-프레미스 인바운드 끝점 ExpressRoute을 통해 Office 365에 노출만, 여전히 해야 공용 IP 연결 된 경우에 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p134">Office 365 can only target on-premises endpoints that use public IPs. This means that even if the on-premises inbound endpoint is only exposed to Office 365 over ExpressRoute, it still needs to have public IP associated with it.</span></span>

2. <span data-ttu-id="a23ba-p135">공용 DNS를 사용 하 여 발생 하는 Office 365 서비스 끝점을 온-프레미스를 해결 하려면 수행 하는 모든 DNS 이름 확인 합니다. 즉, 인바운드 서비스 끝점의 FQDN은 인터넷에서 IP 매핑을 등록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p135">All DNS name resolution that Office 365 services perform to resolve on-premises endpoints happen using public DNS. This means that you must register inbound service endpoints' FQDN to IP mappings on the Internet.</span></span>

3. <span data-ttu-id="a23ba-362">ExpressRoute을 통해 인바운드 네트워크 연결을 받으려면 이러한 끝점에 대 한 공용 IP 서브넷 ExpressRoute을 통해 Microsoft에 보급 될 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-362">In order to receive inbound network connections over ExpressRoute, the public IP subnets for these endpoints must to be advertised to Microsoft over ExpressRoute.</span></span>

4. <span data-ttu-id="a23ba-363">신중 하 게는 적절 한 보안을 확인 하려면 이러한 인바운드 네트워크 트래픽 흐름을 평가 하 고 네트워크 컨트롤 회사 보안 및 네트워크 정책에 맞게 자신에 게 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-363">Carefully evaluate these inbound network traffic flows to ensure that proper security and network controls are applied to them in accordance with your company security and network policies.</span></span>

5. <span data-ttu-id="a23ba-p136">한번 온-프레미스 인바운드 끝점은 ExpressRoute을 통해 Microsoft에 보급, ExpressRoute는 효과적으로 Office 365를 포함 하는 모든 Microsoft 서비스에 대 한 해당 끝점을 기본 설정된 라우팅 경로입니다. 즉, 이러한 끝점 서브넷 Office 365 서비스 및 Microsoft 네트워크에 없는 다른 서비스와의 통신에만 사용 해야 합니다. 그렇지 않은 경우 설계 비대칭 라우팅 여기서 서비스 라우팅하는 것을 선호 하는 다른 Microsoft의 인바운드 연결의 인바운드 ExpressRoute를 통해 반환 경로 인터넷을 사용 하는 동안 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p136">Once your on-premises inbound endpoints are advertised to Microsoft over ExpressRoute, ExpressRoute will effectively become the preferred routing path to those endpoints for all Microsoft services, including Office 365. This means that those endpoint subnets must only be used for communications with Office 365 services and no other services on the Microsoft network. Otherwise, your design will cause asymmetric routing where inbound connections from other Microsoft services prefer to route inbound over ExpressRoute, while the return path will use the Internet.</span></span>

6. <span data-ttu-id="a23ba-p137">이벤트는 ExpressRoute에서 회로 또는 충족-me 작동이 중지 위치, 온-프레미스를 확인 하는데 필요한 인바운드 끝점은 별도 네트워크 경로 통해 요청을 수락 하도록 계속 사용할 수 있습니다. 이 광고 서브넷 여러 ExpressRoute 회로 통해 이러한 끝점에 대 한 것일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p137">In the event an ExpressRoute circuit or meet-me location is down, you'll need to ensure the on-premises inbound endpoints are still available to accept requests over a separate network path. This may mean advertising subnets for those endpoints through multiple ExpressRoute circuits.</span></span>

7. <span data-ttu-id="a23ba-369">이러한 흐름 방화벽 같은 상태 저장 네트워크 장치를 교차 하는 경우에 특히 ExpressRoute 통해 네트워크를 입력 하는 모든 인바운드 네트워크 트래픽 흐름에 대 한 NAT 소스를 적용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-369">We recommend applying source NAT for all inbound network traffic flows entering your network through ExpressRoute, especially when these flows cross stateful network devices such as firewalls.</span></span>

8. <span data-ttu-id="a23ba-p138">Ad FS 프록시 또는 Exchange 자동 검색 등의 일부 온-프레미스 서비스는 Office 365 서비스와 인터넷에서 사용자의 인바운드 요청을 받을 수 있습니다. 이러한 요청에 대 한 Office 365 인터넷을 통해 사용자의 요청과 동일한 FQDN을 대상 됩니다. 중요 한 라우팅 복잡성을 나타냅니다 ExpressRoute를 사용 하 여 Office 365 연결 강제 실행 하는 동안 이러한 온-프레미스 끝점을 인터넷에서 인바운드 사용자 연결을 허용 합니다. 대부분의 고객에 대 한 ExpressRoute을 통해 이러한 복잡 한 시나리오를 구현 하지 않기 운영 고려 사항으로 인해 합니다. 이 추가 오버 헤드가 포함, 비대칭 라우팅의 위험을 관리 하 고 신중 하 게 여러 차원에서 라우팅 광고 및 정책을 관리 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p138">Some on-premises services, such as ADFS proxy or Exchange autodiscover, may receive inbound requests from both Office 365 services and users from the Internet. For these requests Office 365 will target the same FQDN as user requests over the Internet. Allowing inbound user connections from the internet to those on-premises endpoints, while forcing Office 365 connections to use ExpressRoute, represents significant routing complexity. For the vast majority of customers implementing such complex scenarios over ExpressRoute is not recommended due to operational considerations. This additional overhead includes, managing risks of asymmetric routing and will require you to carefully manage routing advertisements and policies across multiple dimensions.</span></span>

### <a name="update-your-network-topology-plan-to-show-how-you-would-avoid-asymmetric-routes"></a><span data-ttu-id="a23ba-375">비대칭 경로 방지이 하는 방법을 표시 하려면 네트워크 토폴로지 계획을 업데이트</span><span class="sxs-lookup"><span data-stu-id="a23ba-375">Update your network topology plan to show how you would avoid asymmetric routes</span></span>
<span data-ttu-id="a23ba-376"><a name="asymmetric"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-376"></span></span>

<span data-ttu-id="a23ba-p139">조직의 사용자 수 원활 하 게 사용 하 여 다른 중요 한 서비스와 Office 365 인터넷에서 비대칭 라우팅을 방지 하려고 합니다. 비대칭 라우팅 발생 하는 두 일반적인 구성은 고객이 됩니다. 이제는 작업을 사용 하 고 이러한 비대칭 라우팅 시나리오 중 하나가 존재할 수를 확인 하려면 계획 이라면 네트워크 구성을 검토 하려면 좋은 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p139">You want to avoid asymmetric routing to ensure people in your organization can seamlessly use Office 365 as well as other important services on the internet. There are two common configurations customers have that cause asymmetric routing. Now's a good time to review the network configuration you're planning to use and check if one of these asymmetric routing scenarios could exist.</span></span>
  
<span data-ttu-id="a23ba-p140">먼저, 다음 네트워크 다이어그램와 관련 된 다양 한 상황을 몇 살펴보겠습니다. 이 다이어그램에서는 인바운드 요청을 수신 하는 모든 서버에서에서 ADFS 또는 온-프레미스 하이브리드 서버 새로 저지 데이터 센터에 및 같은 인터넷으로 보급 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p140">To begin, we'll examine a few different situations associated with the following network diagram. In this diagram, all servers that receive inbound requests, such as ADFS or on-premises hybrid servers are in the New Jersey data center and are advertised to the internet.</span></span>
  
1. <span data-ttu-id="a23ba-382">경계 네트워크를 안전 하는 동안 방법이 소스 NAT 들어오는 요청을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-382">While the perimeter network is secure, there is no Source NAT available for incoming requests.</span></span>

2. <span data-ttu-id="a23ba-383">새로 만들기 저지 데이터 센터의 서버 인터넷 창과 ExpressRoute 경로 모두 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-383">The servers in the New Jersey data center are able to see both internet and ExpressRoute routes.</span></span>

![ExpressRoute 연결 개요 (영문)](media/8f074af6-ef38-44e8-bc5a-8b4d981fbb20.png)
  
<span data-ttu-id="a23ba-385">것 권한도 부여 제안 문제를 해결 하는 방법에 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-385">We also have suggestions on how to fix them.</span></span>
  
#### <a name="problem-1-cloud-to-on-premises-connection-over-the-internet"></a><span data-ttu-id="a23ba-386">문제 1: 클라우드 인터넷을 통해 온-프레미스 연결 하려면</span><span class="sxs-lookup"><span data-stu-id="a23ba-386">Problem 1: Cloud to on-premises connection over the Internet</span></span>
  
<span data-ttu-id="a23ba-387">다음 다이어그램에서는 네트워크 구성에는 인터넷을 통해 Microsoft 클라우드의 인바운드 요청에 대 한 NAT를 제공 하지 않습니다 때 사용 된 비대칭 네트워크 경로를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-387">The following diagram illustrates the asymmetric network path taken when your network configuration doesn't provide NAT for inbound requests from the Microsoft cloud over the internet.</span></span>
  
1. <span data-ttu-id="a23ba-388">Office 365의 인바운드 요청 공용 DNS에서 온-프레미스 끝점의 IP 주소를 검색 하 고 경계 네트워크에 요청을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-388">The inbound request from Office 365 retrieves the IP address of the on-premises endpoint from public DNS and sends the request to your perimeter network.</span></span>

2. <span data-ttu-id="a23ba-389">이 문제가 발생 한 구성에서 구성 된 원본 NAT 되었거나 트래픽을 경계 네트워크에서 사용할 수 있는 전송 되므로 실제 원본 IP 주소 반환 대상으로 사용 되 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-389">In this faulty configuration, there is no Source NAT configured or available at the perimeter network where the traffic is sent resulting in the actual source IP address being used as the return destination.</span></span>

  - <span data-ttu-id="a23ba-390">네트워크의 서버에 사용 가능한 모든 ExpressRoute 네트워크 연결을 통해 Office 365로 반환 되는 트래픽이 라우팅합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-390">The server on your network routes the return traffic to Office 365 through any available ExpressRoute network connection.</span></span>

  - <span data-ttu-id="a23ba-391">결과 연결이 끊어진된에서 발생 하는 Office 365에 해당 흐름에 대 한 비대칭 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-391">The result is an Asymmetric path for that flow to Office 365, resulting in a broken connection.</span></span>

![ExpressRoute Asymetric 라우팅 문제 1](media/9c210c2a-e0ea-4180-8ede-1bf41746ce7a.png)
  
##### <a name="solution-1a-source-nat"></a><span data-ttu-id="a23ba-393">솔루션 1a: NAT 원본</span><span class="sxs-lookup"><span data-stu-id="a23ba-393">Solution 1a: Source NAT</span></span>
  
<span data-ttu-id="a23ba-p141">단순히 추가 (영문) 원본 NAT는 인바운드 요청을이 잘못 구성 된 네트워크를 확인 합니다. 이 다이어그램:</span><span class="sxs-lookup"><span data-stu-id="a23ba-p141">Simply adding a source NAT to the inbound request resolves this misconfigured network. In this diagram:</span></span>
  
1. <span data-ttu-id="a23ba-p142">들어오는 요청을 새로 저지 데이터 센터의 경계 네트워크를 통해 입력 계속 합니다. 이 시간 원본 NAT가 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p142">The incoming request continues to enter through the New Jersey data center's perimeter network. This time Source NAT is available.</span></span>

2. <span data-ttu-id="a23ba-398">연관 된 동일한 네트워크 경로 따라 반환에 대 한 응답의 결과로 만들어진 원본 IP 주소 대신 원본 NAT IP 향해 다시 서버 경로에서 응답 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-398">The response from the server routes back toward the IP associated with the Source NAT instead of the original IP address, resulting in the response returning along the same network path.</span></span>

![ExpressRoute Asymetric 라우팅 솔루션 1](media/0e87a155-f8de-48ed-92ac-27367b727a82.png)
  
##### <a name="solution-1b-route-scoping"></a><span data-ttu-id="a23ba-400">솔루션 1b: 경로 범위 지정</span><span class="sxs-lookup"><span data-stu-id="a23ba-400">Solution 1b: Route Scoping</span></span>
  
<span data-ttu-id="a23ba-p143">또는 수도 있습니다 ExpressRoute BGP 접두사 보급 수를 허용 하지 않으려면 해당 컴퓨터에 대 한 대체 네트워크 경로 제거 합니다. 이 다이어그램:</span><span class="sxs-lookup"><span data-stu-id="a23ba-p143">Alternatively, you can choose to not allow the ExpressRoute BGP prefixes to be advertised, removing the alternate network path for those computers. In this diagram:</span></span>
  
1. <span data-ttu-id="a23ba-p144">들어오는 요청을 새로 저지 데이터 센터의 경계 네트워크를 통해 입력 계속 합니다. 이 시간 ExpressRoute 회로 통해 Microsoft에서 보급 접두사 새로 저지 데이터 센터에 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p144">The incoming request continues to enter through the New Jersey data center's perimeter network. This time the prefixes advertised from Microsoft over the ExpressRoute circuit are not available to the New Jersey data center.</span></span>

2. <span data-ttu-id="a23ba-405">동일한 네트워크 경로 따라 반환에 대 한 응답에 그 결과 사용할 수 있는 유일한 경로 통해 원래 IP 주소와 연결 된 IP 향해 다시 서버 경로에서 응답 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-405">The response from the server routes back toward the IP associated with the original IP address over the only route available, resulting in the response returning along the same network path.</span></span>

![ExpressRoute Asymetric 라우팅 솔루션 2](media/9cb4b2bf-7aa6-487a-bc02-e02af8a979f6.png)
  
#### <a name="problem-2-cloud-to-on-premises-connection-over-expressroute"></a><span data-ttu-id="a23ba-407">문제 2: 클라우드 ExpressRoute 통해 온-프레미스 연결 하려면</span><span class="sxs-lookup"><span data-stu-id="a23ba-407">Problem 2: Cloud to on-premises connection over ExpressRoute</span></span>
  
<span data-ttu-id="a23ba-408">다음 다이어그램에서는 네트워크 구성 ExpressRoute을 통해 Microsoft 클라우드의 인바운드 요청에 대 한 NAT를 제공 하지 않습니다 때 사용 된 비대칭 네트워크 경로를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-408">The following diagram illustrates the asymmetric network path taken when your network configuration doesn't provide NAT for inbound requests from the Microsoft cloud over ExpressRoute.</span></span>
  
1. <span data-ttu-id="a23ba-409">Office 365의 인바운드 요청 DNS에서 IP 주소를 검색 하 고 경계 네트워크에 요청을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-409">The inbound request from Office 365 retrieves the IP address from DNS and sends the request to your perimeter network.</span></span>

2. <span data-ttu-id="a23ba-410">이 문제가 발생 한 구성에서 구성 된 원본 NAT 되었거나 트래픽을 경계 네트워크에서 사용할 수 있는 전송 되므로 실제 원본 IP 주소 반환 대상으로 사용 되 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-410">In this faulty configuration, there is no Source NAT configured or available at the perimeter network where the traffic is sent resulting in the actual source IP address being used as the return destination.</span></span>

  - <span data-ttu-id="a23ba-411">네트워크에 컴퓨터 모든 사용 가능한 ExpressRoute 네트워크 연결을 통해 Office 365로 반환 되는 트래픽이 라우팅합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-411">The computer on your network routes the return traffic to Office 365 through any available ExpressRoute network connection.</span></span>

  - <span data-ttu-id="a23ba-412">Office 365에 대 한 비대칭 연결이 반환이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-412">The result is an Asymmetric connection to Office 365.</span></span>

![ExpressRoute Asymetric 라우팅 문제 2](media/f6fd155b-bbb7-472a-846e-039a99f09913.png)
  
##### <a name="solution-2-source-nat"></a><span data-ttu-id="a23ba-414">해결 방법 2: 소스 NAT</span><span class="sxs-lookup"><span data-stu-id="a23ba-414">Solution 2: Source NAT</span></span>
  
<span data-ttu-id="a23ba-p145">단순히 추가 (영문) 원본 NAT는 인바운드 요청을이 잘못 구성 된 네트워크를 확인 합니다. 이 다이어그램:</span><span class="sxs-lookup"><span data-stu-id="a23ba-p145">Simply adding a source NAT to the inbound request resolves this misconfigured network. In this diagram:</span></span>
  
1. <span data-ttu-id="a23ba-p146">들어오는 요청을 계속 뉴욕 데이터 센터의 경계 네트워크를 통해를 입력 합니다. 이 시간 원본 NAT가 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p146">The incoming request continues to enter through the New York data center's perimeter network. This time Source NAT is available.</span></span>

2. <span data-ttu-id="a23ba-419">연관 된 동일한 네트워크 경로 따라 반환에 대 한 응답의 결과로 만들어진 원본 IP 주소 대신 원본 NAT IP 향해 다시 서버 경로에서 응답 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-419">The response from the server routes back toward the IP associated with the Source NAT instead of the original IP address, resulting in the response returning along the same network path.</span></span>

![ExpressRoute Asymetric 라우팅 솔루션 3](media/a5d2b90d-a3ec-4047-afbf-6e6e99f376a7.png)
  
### <a name="paper-verify-that-the-network-design-has-path-symmetry"></a><span data-ttu-id="a23ba-421">용지는 네트워크 디자인에 경로 대칭 권한이 있는지 확인</span><span class="sxs-lookup"><span data-stu-id="a23ba-421">Paper verify that the network design has path symmetry</span></span>

<span data-ttu-id="a23ba-p147">이 시점 확인 용지에 구현 계획에는 사용할 Office 365 다양 한 시나리오에 대 한 경로 대칭을 제공 해야 합니다. 사용자는 서비스의 서로 다른 기능을 사용 하는 경우에 수행할 수 있는 특정 네트워크 경로 식별 합니다. 온-프레미스 네트워크 및 경계 장치로 연결 경로; WAN 라우팅 인터넷 또는 ExpressRoute 및 온라인 끝점에 대 한 연결에 로그온 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p147">At this point, you need to verify on paper that your implementation plan offers route symmetry for the different scenarios in which you'll be using Office 365. You'll identify the specific network route that is expected to be taken when a person uses different features of the service. From the on-premises network and WAN routing, to the perimeter devices, to the connectivity path; ExpressRoute or the internet, and on to the connection to the online endpoint.</span></span>
  
<span data-ttu-id="a23ba-425">이전에 조직을 채택 하는 서비스로 식별 된 Office 365 네트워크 서비스를 모두에 대해이 작업을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-425">You'll need to do this for all of the Office 365 network services that were previously identified as services that your organization will adopt.</span></span>
  
<span data-ttu-id="a23ba-p148">두번째 사용자와 경로 통해이 용지 워크를 수행 하는 데 도움이 됩니다. 각 네트워크 홉 필요한 곳에서 그 다음 경로 가져오고 라우팅 경로 함께 친숙 확인 하 고에 대 한 설명입니다. 기억 ExpressRoute 인터넷 기본 경로 보다 저렴 한 경로 제공 하는 Microsoft 서버 IP 주소에 대 한 더 범위가 지정 된 경로 항상 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p148">It helps to do this paper walk through of routes with a second person. Explain to them where each network hop is expected to get its next route from and ensure that you're familiar with the routing paths. Remember that ExpressRoute will always provide a more scoped route to Microsoft server IP addresses giving it lower route cost than an Internet default route.</span></span>
  
### <a name="design-client-connectivity-configuration"></a><span data-ttu-id="a23ba-429">디자인 클라이언트 연결 구성</span><span class="sxs-lookup"><span data-stu-id="a23ba-429">Design Client Connectivity Configuration</span></span>
<span data-ttu-id="a23ba-430"><a name="asymmetric"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-430"></span></span>

![ExpressRoute PAC 파일 사용](media/7cfa6482-dbae-416a-ae6f-a45e5f4de23b.png)
  
<span data-ttu-id="a23ba-p149">사용 중인 경우 프록시 서버를 인터넷에 대 한 바운드 트래픽은 다음 모든 PAC 조정할 필요가 또는 네트워크에 있는 클라이언트 컴퓨터를 확인 하려면 클라이언트 구성 파일을 전송 하 하지 않고 Office 365에 원하는 ExpressRoute 트래픽을 보낼 제대로 구성 사용 중인 프록시 서버와 일부 Office 365 트래픽을 포함 하는 나머지 트래픽을 관련 프록시에 전송 됩니다. PAC 파일을 예 [끝점을 Office 365 관리](https://aka.ms/manageo365endpoints) 에 대 한 가이드를 읽어보십시오.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p149">If you're using a proxy server for internet bound traffic then you need to adjust any PAC or client configuration files to ensure client computers on your network are correctly configured to send the ExpressRoute traffic you desire to Office 365 without transiting your proxy server, and the remaining traffic, including some Office 365 traffic, is sent to the relevant proxy. Read our guide on [managing Office 365 endpoints](https://aka.ms/manageo365endpoints) for example PAC files.</span></span>
  
> [!NOTE]
> <span data-ttu-id="a23ba-p150">끝점을 자주, 매주으로 자주 변경합니다. 만 최신 상태를 유지 하도록 해야 변경 횟수를 줄일 수 조직에 채택 하는 기능과 서비스를 기준으로 변경 내용을 확인 해야 합니다. RSS 피드 여기서 변경 된 내용을 발표 하 고 레코드를 유지할의 모든 변경 내용, IP 과거 발표 된 주소에서에서 **해당 날짜** 에 유의 해야를, 보급 되거나 유효 날짜에 도달할 때까지 광고에서 제거 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p150">The endpoints change frequently, as often as weekly. You should only make changes based on the services and features your organization has adopted to reduce the number of changes you'll need to make to stay current. Pay close attention to the **Effective Date** in the RSS feed where the changes are announced and a record is kept of all past changes, IP addresses that are announced may not be advertised, or removed from advertisement, until the effective date is reached.</span></span>
  
## <a name="build-your-deployment-and-testing-procedures"></a><span data-ttu-id="a23ba-437">배포 및 테스트 절차를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-437">Build your deployment and testing procedures</span></span>
<span data-ttu-id="a23ba-438"><a name="testing"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-438"></span></span>

<span data-ttu-id="a23ba-p151">구현 계획 테스트 및 롤백 계획 모두 포함 되어야 합니다. 가장 적은 영향을 주는 계획을 디자인 해야 구현을 예상 대로 작동 하지 않을, 하는 경우 문제를 검색 하기 전에 사용자의 번호입니다. 다음은 일부 높은 수준의 원칙 계획을 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p151">Your implementation plan should include both testing and rollback planning. If your implementation isn't functioning as expected, the plan should be designed to affect the least number of people before problems are discovered. The following are some high level principles your plan should consider.</span></span>
  
1. <span data-ttu-id="a23ba-442">중단을 최소화 하기 위해 네트워크 세그먼트 및 사용자 서비스 딩을 준비 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-442">Stage the network segment and user service onboarding to minimize disruption.</span></span>

2. <span data-ttu-id="a23ba-443">Traceroute와 경로 테스트 하기 위한 계획 하 고 별도 인터넷 연결 된 호스트에서 TCP 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-443">Plan for testing routes with traceroute and TCP connect from a separate internet connected host.</span></span>

3. <span data-ttu-id="a23ba-444">가능 하면, 인바운드 및 아웃 바운드 서비스의 테스트 테스트 Office 365 테 넌 트를 사용 하 여는 격리 된 테스트 네트워크에서 수행 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-444">Preferably, testing of inbound and outbound services should be done on an isolated test network with a test Office 365 tenant.</span></span>

      - <span data-ttu-id="a23ba-445">또는 테스트를 수행할 수 프로덕션 네트워크에서 고객은 Office 365를 아직 사용 하지 않거나 파일럿에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-445">Alternatively, testing can be performed on a production network if the customer is not yet using Office 365 or is in pilot.</span></span>

      - <span data-ttu-id="a23ba-446">또는 테스트 수는 테스트에 대해 별도로 설정 하는 프로덕션 중단 하는 동안 수행 하 고 모니터링만 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-446">Alternatively, testing can be performed during a production outage that is set aside for test and monitoring only.</span></span>

      - <span data-ttu-id="a23ba-p152">또는 테스트 각 계층 3 라우터 노드에서 각 서비스에 대 한 경로 확인 하 여 수행할 수 있습니다. 이 대체 위험을 소개 하는 실제 테스트의 부족 되므로 가능한이 없는 다른 테스트 하는 경우에 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p152">Alternatively, testing can be done by checking routes for each service on each layer 3 router node. This fall back should only be used if no other testing is possible since a lack of physical testing introduces risk.</span></span>

### <a name="build-your-deployment-procedures"></a><span data-ttu-id="a23ba-449">배포 절차를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-449">Build your deployment procedures</span></span>

<span data-ttu-id="a23ba-p153">배포 절차는 더 큰 그룹의 사용자에 게 배포 하기 전에 테스트를 허용 하도록 단계별로 소규모 사용자 그룹 롤아웃 해야 합니다. 다음은 ExpressRoute의 배포를 준비 하는 여러 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p153">Your deployment procedures should roll out to small groups of people in stages to allow for testing before deploying to larger groups of people. The following are several ways to stage the deployment of ExpressRoute.</span></span>
  
1. <span data-ttu-id="a23ba-452">Microsoft 피어 링와 ExpressRoute를 설정 하 고 단일 호스트에 대해서만 착신 전환 경로 광고 테스트 목적으로 준비 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-452">Set up ExpressRoute with Microsoft peering and have the route advertisements forwarded to a single host only for staged testing purposes.</span></span>

2. <span data-ttu-id="a23ba-453">처음에는 단일 네트워크 세그먼트에 ExpressRoute 네트워크에 대 한 경로가 광고 및 네트워크 세그먼트 또는 지역 경로 광고를 확장 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-453">Advertise routes to the ExpressRoute network to a single network segment at first and expand route advertisements by network segment or region.</span></span>

3. <span data-ttu-id="a23ba-454">처음에 대 한 Office 365를 배포 하는 경우에 소수의 사용자에 대 한 ExpressRoute 네트워크 배포 파일럿으로 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-454">If deploying Office 365 for the first time, use the ExpressRoute network deployment as a pilot for a small number of people.</span></span>

4. <span data-ttu-id="a23ba-455">프록시 서버를 사용 하는 경우 테스트 PAC 파일을 추가 하기 전에 소수의 사용자 ExpressRoute 테스트 및 피드백 사용 하 여 직접 또는 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-455">If using proxy servers, you can alternatively configure a test PAC file to direct a small number of people to ExpressRoute with testing and feedback before adding more.</span></span>

<span data-ttu-id="a23ba-p154">구현 계획을 문서화할 각각의 배포 절차를 수행 해야 하는 또는 네트워킹 구성 배포를 사용 하는 명령을 나열 해야 합니다. 네트워크 중단 시간 모든 미리 작성 된 작성 된 배포 계획 및 피어에서 되어야 만들어지는 변경 내용에 도달할 때를 검토 합니다. ExpressRoute의 기술 구성에 대 한 지침을 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p154">Your implementation plan should list each of the deployment procedures that must be taken or commands that need to be used to deploy the networking configuration. When the network outage time arrives all of the changes being made should be from the written deployment plan which was written in advance and peer reviewed. See our guidance on the technical configuration of ExpressRoute.</span></span>
  
- <span data-ttu-id="a23ba-459">전자 메일을 보내는 계속 하는 온-프레미스 서버에 대 한 IP 주소를 변경 했을 때 경우 SPF TXT 레코드를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-459">Updating your SPF TXT records if you've changed IP addresses for any on-premises servers that will continue to send email.</span></span>

- <span data-ttu-id="a23ba-460">새 NAT 구성에 맞게 IP 주소를 변경 했을 때 온-프레미스 서버에 대 한 모든 DNS 항목을 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-460">Updating any DNS entries for on-premises servers if you've changed IP addresses to accommodate a new NAT configuration.</span></span>

- <span data-ttu-id="a23ba-461">RSS 모든 라우팅 또는 프록시 구성을 유지 하기 위해 Office 365 끝점 알림을 대 한 피드를 구독 했는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-461">Ensure you've subscribed to the RSS feed for Office 365 endpoint notifications to maintain any routing or proxy configurations.</span></span>

<span data-ttu-id="a23ba-p155">ExpressRoute 배포가 완료 된 후 테스트 계획의 절차를 실행 해야 합니다. 각 절차에 대 한 결과 기록해 야 합니다. 테스트 계획 결과 구현 하지 못했습니다에 따르면 이벤트에 원래 프로덕션 환경으로 롤백에 대 한 절차를 포함 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p155">After your ExpressRoute deployment is complete the procedures in the test plan should be executed. Results for each procedure should be logged. You must include procedures for rolling back to the original production environment in the event the test plan results indicate the implementation was not successful.</span></span>
  
### <a name="build-your-test-procedures"></a><span data-ttu-id="a23ba-465">빌드 테스트 절차</span><span class="sxs-lookup"><span data-stu-id="a23ba-465">Build your test procedures</span></span>

<span data-ttu-id="a23ba-p156">테스트 절차는 ExpressRoute를 사용 하는 Office 365에 대 한 각 아웃 바운드 및 인바운드 네트워크 서비스와는 그렇지 않은 글꼴로 대 한 테스트를 포함 해야 합니다. 절차는 온-프레미스 회사 LAN에 없는 사용자를 포함 하 여 각 고유 네트워크 위치에서 테스트가 포함 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p156">Your testing procedures should include tests for each outbound and inbound network service for Office 365 both that will be using ExpressRoute and ones that will not. The procedures should include testing from each unique network location including users who are not on-premises in the corporate LAN.</span></span>
  
<span data-ttu-id="a23ba-468">테스트 작업의 일부 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-468">Some examples of test activities include the following.</span></span>
  
1. <span data-ttu-id="a23ba-469">온-프레미스 라우터에서 네트워크 연산자 라우터에 ping을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-469">Ping from your on-premises router to your network operator router.</span></span>

2. <span data-ttu-id="a23ba-470">500 명 이상의 Office 365 및 CRM Online IP 주소는 온-프레미스 라우터가 광고 수신의 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-470">Validate the 500+ Office 365 and CRM Online IP address advertisements are received by your on-premises router.</span></span>

3. <span data-ttu-id="a23ba-471">인바운드 사용자의 유효성을 검사 하 고 아웃 바운드 NAT ExpressRoute와 내부 네트워크 사이의 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-471">Validate your inbound and outbound NAT is operating between ExpressRoute and the internal network.</span></span>

4. <span data-ttu-id="a23ba-472">NAT 경로 라우터에서 보급 되 고 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-472">Validate that routes to your NAT are being advertised from your router.</span></span>

5. <span data-ttu-id="a23ba-473">ExpressRoute 프로그램 보급된 접두사를 수락 했습니다의 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-473">Validate that ExpressRoute has accepted your advertised prefixes.</span></span>

      - <span data-ttu-id="a23ba-474">다음 cmdlet를 사용 하 여 피어 링 광고를 확인 하려면:</span><span class="sxs-lookup"><span data-stu-id="a23ba-474">Use the following cmdlet to verify peering advertisements:</span></span>

      ```PowerShell
      Get-AzureRmExpressRouteCircuitRouteTable -DevicePath Primary -ExpressRouteCircuitName TestER -ResourceGroupName RG -PeeringType MicrosoftPeering
      ```

6. <span data-ttu-id="a23ba-475">공용 NAT IP 범위 ExpressRoute 또는 공용 인터넷 네트워크 회선을 통해 Microsoft에 보급 하지 않거나 하지 않은 경우 이전 예제와 같이 더 큰 범위의 특정 하위 집합의 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-475">Validate your public NAT IP range is not advertised to Microsoft through any other ExpressRoute or public Internet network circuit unless it is a specific subset of a larger range as in the previous example.</span></span>

7. <span data-ttu-id="a23ba-476">ExpressRoute 회로 쌍, BGP 세션을 모두 실행 되 고 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-476">ExpressRoute circuits are paired, validate that both BGP sessions are running.</span></span>

8. <span data-ttu-id="a23ba-p157">NAT의 내부에 단일 호스트를 설정 하 고 새 회로 호스트 outlook.office365.com 통해 연결을 테스트 하려면 ping, tracert 및 tcpping를 사용 합니다. 또는 Wireshark와 같은 도구를 사용할 수 또는 하면 유효성을 검사 하려면 MSEE 미러된 포트에서 Microsoft 네트워크 모니터 3.4는 outlook.office365.com와 연결 된 IP 주소에 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p157">Set up a single host on the inside of your NAT and use ping, tracert, and tcpping to test connectivity across the new circuit to the host outlook.office365.com. Alternatively, you could use a tool such as Wireshark or Microsoft Network Monitor 3.4 on a mirrored port to the MSEE to validate you're able to connect to the IP address associated with outlook.office365.com.</span></span>

9. <span data-ttu-id="a23ba-479">Exchange Online에 대 한 응용 프로그램 수준 기능을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-479">Test application level functionality for Exchange Online.</span></span>

  - <span data-ttu-id="a23ba-480">테스트 Outlook는 Exchange Online에 연결 하 고 전자 메일 보내기/받기 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-480">Test Outlook is able to connect to Exchange Online and send/receive email.</span></span>

  - <span data-ttu-id="a23ba-481">테스트 Outlook이 온라인 모드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-481">Test Outlook is able to use online-mode.</span></span>

  - <span data-ttu-id="a23ba-482">Smartphone 연결 하 고 보내기/받기 기능을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-482">Test smartphone connectivity and send/receive capability.</span></span>

10. <span data-ttu-id="a23ba-483">SharePoint Online에 대 한 응용 프로그램 수준 기능을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-483">Test application level functionality for SharePoint Online</span></span>

  - <span data-ttu-id="a23ba-484">비즈니스 동기화 클라이언트에 대 한 OneDrive를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-484">Test OneDrive for Business sync client.</span></span>

  - <span data-ttu-id="a23ba-485">SharePoint Online 웹 액세스를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-485">Test SharePoint Online web access.</span></span>

11. <span data-ttu-id="a23ba-486">비즈니스 호출 시나리오에 대 한 Skype에 대 한 응용 프로그램 수준 기능을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-486">Test application level functionality for Skype for Business calling scenarios:</span></span>

  - <span data-ttu-id="a23ba-487">[최종 사용자가 시작한 초대] 인증 된 사용자로 전화 회의에 참가 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-487">Join to conference call as authenticated user [invite initiated by end user].</span></span>

  - <span data-ttu-id="a23ba-488">[MCU에서 보낸 invite] 전화 회의에 사용자를 초대 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-488">Invite user to conference call [invite sent from MCU].</span></span>

  - <span data-ttu-id="a23ba-489">Skype를 사용 하 여 비즈니스 웹 응용 프로그램에 대 한 익명 사용자로 전화 회의 참가 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-489">Join conference as anonymous user using the Skype for Business web application.</span></span>

  - <span data-ttu-id="a23ba-490">유선된 PC 연결, IP 전화 및 모바일 장치에서 통화에 참가 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-490">Join call from your wired PC connection, IP phone, and mobile device.</span></span>

  - <span data-ttu-id="a23ba-491">페더레이션된 사용자 o PSTN 유효성 검사를 통화에 대 한 호출: 호출이 완료 된, 통화 품질은 적절 한는 연결 시간을 허용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-491">Call to federated user o Call to PSTN Validation: call is completed, call quality is acceptable, connection time is acceptable.</span></span>

  - <span data-ttu-id="a23ba-492">대화 상대에 대 한 현재 상태는 테 넌 트의 두 구성원에 대 한 업데이트 및 페더레이션 사용자를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-492">Verify presence status for contacts is updated for both members of the tenant and federated users.</span></span>

### <a name="common-problems"></a><span data-ttu-id="a23ba-493">일반적인 문제</span><span class="sxs-lookup"><span data-stu-id="a23ba-493">Common problems</span></span>

<span data-ttu-id="a23ba-p158">비대칭 라우팅은 가장 일반적인 구현 문제입니다. 일부 일반적인 소스를 찾도록 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p158">Asymmetric routing is the most common implementation problem. Here are some common sources to look for:</span></span>
  
- <span data-ttu-id="a23ba-496">열기 또는 플랫 네트워크 라우팅 토폴로지를 사용 하 여 원본 위치에는 NAT 없이 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-496">Using an open or flat network routing topology without source NAT in place.</span></span>

- <span data-ttu-id="a23ba-497">하지 SNAT를 사용 하 여 인바운드 라우팅 서비스 인터넷 및 ExpressRoute를 통해 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-497">Not using SNAT to route to inbound services through both the internet and ExpressRoute connections.</span></span>

- <span data-ttu-id="a23ba-498">광범위 하 게 배포 하기 전에 테스트 네트워크에서 ExpressRoute에서 인바운드 서비스를 테스트 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-498">Not testing inbound services on ExpressRoute on a test network prior to deploying broadly.</span></span>

## <a name="deploying-expressroute-connectivity-through-your-network"></a><span data-ttu-id="a23ba-499">네트워크를 통한 ExpressRoute 연결 배포</span><span class="sxs-lookup"><span data-stu-id="a23ba-499">Deploying ExpressRoute connectivity through your network</span></span>
<span data-ttu-id="a23ba-500"><a name="testing"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-500"></span></span>

<span data-ttu-id="a23ba-p159">연결 새로운 각 네트워크 세그먼트에 대 한 롤백 계획을 사용 하 여 네트워크의 다른 부분을 점진적으로 롤아웃 한번에 하나의 네트워크 세그먼트에 배포를 준비 합니다. 배포 하는 Office 365 배포에 맞추는, 먼저 Office 365 파일럿 사용자에 게 배포 하 고 여기에서 확장 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p159">Stage your deployment to one segment of the network at a time, progressively rolling out the connectivity to different parts of the network with a plan to roll back for each new network segment. If your deployment is aligned with an Office 365 deployment, deploy to your Office 365 pilot users first and extend from there.</span></span>
  
<span data-ttu-id="a23ba-503">처음으로 테스트를 위한 및 프로덕션 환경에 대해 다음:</span><span class="sxs-lookup"><span data-stu-id="a23ba-503">First for your test and then for production:</span></span>
  
- <span data-ttu-id="a23ba-504">ExpressRoute를 사용 하도록 설정 하는 배포 단계를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-504">Run the deployment steps to enable ExpressRoute.</span></span>

- <span data-ttu-id="a23ba-505">네트워크 경로 예상 대로 표시를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-505">Test your seeing the network routes are as expected.</span></span>

- <span data-ttu-id="a23ba-506">각 인바운드 및 아웃 바운드 서비스에 대 한 테스트를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-506">Perform testing on each inbound and outbound service.</span></span>

- <span data-ttu-id="a23ba-507">문제를 발견 하는 경우 롤백입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-507">Rollback if you discover any issues.</span></span>

### <a name="set-up-a-test-connection-to-expressroute-with-a-test-network-segment"></a><span data-ttu-id="a23ba-508">테스트 네트워크 세그먼트를 가진 ExpressRoute에 대 한 테스트 연결 설정</span><span class="sxs-lookup"><span data-stu-id="a23ba-508">Set up a test connection to ExpressRoute with a test network segment</span></span>

<span data-ttu-id="a23ba-p160">이제 용지에 완료 된 플랜이 작은 규모에서 테스트 하는 시간입니다. 이 테스트에서 온-프레미스 네트워크에 테스트 서브넷으로 Microsoft Peering와 단일 ExpressRoute 연결을 설정할를 수 있습니다. 테스트 서브넷에서 연결을 사용 하 여 [평가판 Office 365 테 넌 트](https://go.microsoft.com/fwlink/p/?LinkID=403802) 를 구성 수 있으며 테스트 서브넷에서 프로덕션 환경에서 사용 하는 모든 아웃 바운드 및 인바운드 서비스를 포함할 수 있습니다. 테스트 네트워크 세그먼트에 대 한 DNS를 설정 하 고 모든 인바운드 및 아웃 바운드 서비스를 설정 합니다. 테스트 계획을 실행 하 고 경로 전파 하 고 각 서비스에 대 한 라우팅 익숙한 것을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p160">Now that you have the completed plan on paper it is time to test at a small scale. In this test you will establish a single ExpressRoute connection with Microsoft Peering to a test subnet on your on-premises network. You can configure a [trial Office 365 tenant](https://go.microsoft.com/fwlink/p/?LinkID=403802) with connectivity to and from the test subnet and include all outbound and inbound services that you will be using in production in the test subnet. Set up DNS for the test network segment and establish all inbound and outbound services. Execute your test plan and ensure that you are familiar with the routing for each service and the route propagation.</span></span>
  
### <a name="execute-the-deployment-and-test-plans"></a><span data-ttu-id="a23ba-514">배포 및 테스트 계획을 실행</span><span class="sxs-lookup"><span data-stu-id="a23ba-514">Execute the deployment and test plans</span></span>

<span data-ttu-id="a23ba-515">위에서 설명한 항목을 완료 하면 팀 계획 테스트 및 배포를 실행 하기 전에 검토 하 고 완료 하 고 확인 하면 영역 해제 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-515">As you complete the items described above, check off the areas you've completed and ensure you and your team have reviewed them before executing your deployment and testing plans.</span></span>
  
- <span data-ttu-id="a23ba-516">네트워크 변경에 관련 된 아웃 바운드 및 인바운드 서비스의 목록입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-516">List of outbound and inbound services that are involved in the network change.</span></span>

- <span data-ttu-id="a23ba-517">인터넷 송신 및 ExpressRoute 모임 모두 표시 하는 글로벌 네트워크 아키텍처 다이어그램-나에 게 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-517">Global network architecture diagram showing both internet egress and ExpressRoute meet-me locations.</span></span>

- <span data-ttu-id="a23ba-518">배포 된 각 서비스에 사용 되는 다양 한 네트워크 경로 보여주는 네트워크 라우팅 다이어그램입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-518">Network routing diagram demonstrating the different network paths used for each service deployed.</span></span>

- <span data-ttu-id="a23ba-519">필요한 경우 변경 사항 및 롤백을 구현 하는 단계와 배포를 계획 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-519">A deployment plan with steps to implement the changes and rollback if needed.</span></span>

- <span data-ttu-id="a23ba-520">각 Office 365와 네트워크 서비스를 테스트 하기 위한 테스트 계획 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-520">A test plan for testing each Office 365 and network service.</span></span>

- <span data-ttu-id="a23ba-521">인바운드 및 아웃 바운드 서비스에 대 한 프로덕션 경로의 용지 유효성 검사를 완료 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-521">Completed paper validation of production routes for inbound and outbound services.</span></span>

- <span data-ttu-id="a23ba-522">가용성 테스트를 포함 하 여 테스트 네트워크 세그먼트를 통해 완료 된 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-522">A completed test across a test network segment including availability testing.</span></span>

<span data-ttu-id="a23ba-523">일부 문제를 해결 하는 것에 대 한 시간을 사용할 수 있으며 필요한 경우 다시 롤링에 대 한 시간, 정도로 길다고 전체 배포 계획 및 테스트 계획을 통해 실행 하는 중단 창이 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-523">Choose an outage window that is long enough to run through the entire deployment plan and the test plan, has some time available for troubleshooting and time for rolling back if required.</span></span>
  
> [!CAUTION]
> <span data-ttu-id="a23ba-524">인터넷 및 ExpressRoute을 통해 라우팅의 복잡 한 특성상 것이 좋습니다 처리 복잡 한 라우팅 문제를 해결 하려면이 창에 추가 추가 버퍼 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-524">Due to the complex nature of routing over both the internet and ExpressRoute, it is recommended that additional buffer time is added to this window to handle troubleshooting complex routing.</span></span>
  
### <a name="configure-qos-for-skype-for-business-online"></a><span data-ttu-id="a23ba-525">QoS에 대 한 구성 Skype 비즈니스에 대 한 온라인</span><span class="sxs-lookup"><span data-stu-id="a23ba-525">Configure QoS for Skype for Business Online</span></span>

<span data-ttu-id="a23ba-p161">QoS는 Skype에 대 한 온라인 비즈니스에 대 한 음성 및 회의 혜택을 얻을 필요는. ExpressRoute 네트워크 연결이 차단 하 여 다른 Office 365 서비스 액세스 중 하나 하지 하는지 확인 한 후 QoS를 구성할 수 있습니다. QoS에 대 한 구성 [ExpressRoute 및 비즈니스 온라인 용 Skype에서 QoS](https://support.office.com/article/ExpressRoute-and-QoS-in-Skype-for-Business-Online-20c654da-30ee-4e4f-a764-8b7d8844431d) 문서에 설명 되어있습니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p161">QoS is necessary to obtain voice and meeting benefits for Skype for Business Online. You can configure QoS after you have ensured that the ExpressRoute network connection does not block any of your other Office 365 service access. Configuration for QoS is described in the article [ExpressRoute and QoS in Skype for Business Online](https://support.office.com/article/ExpressRoute-and-QoS-in-Skype-for-Business-Online-20c654da-30ee-4e4f-a764-8b7d8844431d) .</span></span>
  
## <a name="troubleshooting-your-implementation"></a><span data-ttu-id="a23ba-529">구현을 문제해결</span><span class="sxs-lookup"><span data-stu-id="a23ba-529">Troubleshooting your implementation</span></span>
<span data-ttu-id="a23ba-530"><a name="troubleshooting"> </a></span><span class="sxs-lookup"><span data-stu-id="a23ba-530"></span></span>

<span data-ttu-id="a23ba-p162">모든 된이 구현 가이드의 단계에는 첫번째 곳은 구현 계획에서 누락 된? 뒤로 이동 하 고 소규모 네트워크의 오류를 복제 하 고 디버깅할 수를 가능한 경우 테스트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p162">The first place to look is at the steps in this implementation guide, were any missed in your implementation plan? Go back and run further small network testing if possible to replicate the error and debug it there.</span></span>
  
<span data-ttu-id="a23ba-p163">인바운드 또는 아웃 바운드 테스트 하는 동안 실패 한 서비스를 식별 합니다. 각 실패 하는 서비스에 대 한 IP 주소 및 서브넷 구체적으로 가져옵니다. 차지 코드로 미리 차 이동 하 고 용지에 네트워크 토폴로지 다이어그램을 진행 하 고 라우팅의 유효성을 검사 합니다. 특히 여기서 ExpressRoute 라우팅으로 보급 됩니다를 유효성을 검사, 해당 중단 된 시간 동안 가능한 경우 사용 하 여 라우팅을 추적을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p163">Identify which inbound or outbound services failed during testing. Get specifically the IP addresses and subnets for each of the services which failed. Go ahead and walk the network topology diagram on paper and validate the routing. Validate specifically where the ExpressRoute routing is advertised to, Test that routing during the outage if possible with traces.</span></span>
  
<span data-ttu-id="a23ba-p164">각 고객 끝점에 네트워크 추적을 사용 하 여 PSPing를 실행 하 고 예상 대로 되었는지 확인 하려면 원본 및 대상 IP 주소를 평가 합니다. Telnet 포트 25에서 노출 하 고 SNAT 형식이 예상 되는 경우 원래 원본 IP 주소를 숨기고 확인 하는 모든 메일 호스트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p164">Run PSPing with a network trace to each customer endpoint and evaluate source and destination IP addresses to validate that they are as expected. Run telnet to any mail host that you expose on port 25 and verify that SNAT is hiding the original source IP address if this is expected.</span></span>
  
<span data-ttu-id="a23ba-p165">ExpressRoute에 대 한 네트워크 구성을 확인 하는데 필요한 ExpressRoute 연결 된 Office 365를 배포 하는 동안 최적으로 설계 된 유념 하 고 클라이언트 컴퓨터와 같은 네트워크에 다른 구성 요소를 최적화도 했을 때 키를 누릅니다. 이 계획 가이드를 사용 하 여 부재중 단계 문제를 해결 하려면, 외에 쓴 [성능 문제해결 Office 365에 대 한 계획](https://support.office.com/article/Performance-troubleshooting-plan-for-Office-365-e241e5d9-b1d8-4f1d-a5c8-4106b7325f8c) 합니다.</span><span class="sxs-lookup"><span data-stu-id="a23ba-p165">Keep in mind that while deploying Office 365 with an ExpressRoute connection you'll need to ensure both the network configuration for ExpressRoute is optimally designed and you've also optimized the other components on your network such as client computers. In addition to using this planning guide to troubleshoot the steps you may have missed, we also have written a [Performance troubleshooting plan for Office 365](https://support.office.com/article/Performance-troubleshooting-plan-for-Office-365-e241e5d9-b1d8-4f1d-a5c8-4106b7325f8c) .</span></span>
  
<span data-ttu-id="a23ba-541">짧은 링크를 다시 사용할 수는 다음과 같습니다.[https://aka.ms/implementexpressroute365](https://aka.ms/implementexpressroute365)</span><span class="sxs-lookup"><span data-stu-id="a23ba-541">Here's a short link you can use to come back: [https://aka.ms/implementexpressroute365](https://aka.ms/implementexpressroute365)</span></span>
  
## <a name="related-topics"></a><span data-ttu-id="a23ba-542">관련 항목</span><span class="sxs-lookup"><span data-stu-id="a23ba-542">Related Topics</span></span>

[<span data-ttu-id="a23ba-543">Office 365에 대한 네트워크 연결</span><span class="sxs-lookup"><span data-stu-id="a23ba-543">Network connectivity to Office 365</span></span>](network-connectivity.md)
  
[<span data-ttu-id="a23ba-544">Office 365용 Azure ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="a23ba-544">Azure ExpressRoute for Office 365</span></span>](azure-expressroute.md)
  
[<span data-ttu-id="a23ba-545">Office 365 연결에 대한 ExpressRoute 관리</span><span class="sxs-lookup"><span data-stu-id="a23ba-545">Managing ExpressRoute for Office 365 connectivity</span></span>](managing-expressroute-for-connectivity.md)
  
[<span data-ttu-id="a23ba-546">Office 365용 ExpressRoute를 사용한 라우팅</span><span class="sxs-lookup"><span data-stu-id="a23ba-546">Routing with ExpressRoute for Office 365</span></span>](routing-with-expressroute.md)
  
[<span data-ttu-id="a23ba-547">Office 365용 ExpressRoute를 사용한 네트워크 계획</span><span class="sxs-lookup"><span data-stu-id="a23ba-547">Network planning with ExpressRoute for Office 365</span></span>](network-planning-with-expressroute.md)
  
[<span data-ttu-id="a23ba-548">ExpressRoute BGP 커뮤니티를 사용 하 여 Office 365 시나리오 (미리 보기)</span><span class="sxs-lookup"><span data-stu-id="a23ba-548">Using BGP communities in ExpressRoute for Office 365 scenarios (preview)</span></span>](bgp-communities-in-expressroute.md)
  
[<span data-ttu-id="a23ba-549">미디어 품질 및 온라인 비즈니스 Skype 네트워크 연결 성능</span><span class="sxs-lookup"><span data-stu-id="a23ba-549">Media Quality and Network Connectivity Performance in Skype for Business Online</span></span>](https://support.office.com/article/5fe3e01b-34cf-44e0-b897-b0b2a83f0917)
  
[<span data-ttu-id="a23ba-550">비즈니스 온라인 용 Skype에 대 한 네트워크를 최적화</span><span class="sxs-lookup"><span data-stu-id="a23ba-550">Optimizing your network for Skype for Business Online</span></span>](https://support.office.com/article/b363bdca-b00d-4150-96c3-ec7eab5a8a43)
  
[<span data-ttu-id="a23ba-551">ExpressRoute 및 온라인 비즈니스에 대 한 Skype에서 QoS</span><span class="sxs-lookup"><span data-stu-id="a23ba-551">ExpressRoute and QoS in Skype for Business Online</span></span>](https://support.office.com/article/20c654da-30ee-4e4f-a764-8b7d8844431d)
  
[<span data-ttu-id="a23ba-552">ExpressRoute를 사용 하 여 호출 흐름</span><span class="sxs-lookup"><span data-stu-id="a23ba-552">Call flow using ExpressRoute</span></span>](https://support.office.com/article/413acb29-ad83-4393-9402-51d88e7561ab)
  
[<span data-ttu-id="a23ba-553">초기 계획 및 성능 기록을 사용하여 Office 365 성능 조정</span><span class="sxs-lookup"><span data-stu-id="a23ba-553">Office 365 performance tuning using baselines and performance history</span></span>](performance-tuning-using-baselines-and-history.md)
  
[<span data-ttu-id="a23ba-554">Office 365 성능 문제 해결 계획</span><span class="sxs-lookup"><span data-stu-id="a23ba-554">Performance troubleshooting plan for Office 365</span></span>](performance-troubleshooting-plan.md)
  
[<span data-ttu-id="a23ba-555">Office 365 URL 및 IP 주소 범위</span><span class="sxs-lookup"><span data-stu-id="a23ba-555">Office 365 URLs and IP address ranges</span></span>](https://support.office.com/article/8548a211-3fe7-47cb-abb1-355ea5aa88a2)
  
[<span data-ttu-id="a23ba-556">Office 365 네트워크 및 성능 조정</span><span class="sxs-lookup"><span data-stu-id="a23ba-556">Office 365 network and performance tuning</span></span>](network-planning-and-performance.md)