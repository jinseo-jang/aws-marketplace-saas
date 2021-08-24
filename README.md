AWS Marketplace SaaS 제품 에는세가지  **가격 모델** 이있습니다.

- **Contract** - 월간/연간/다년고정가격. 고객은AWS Marketplace에서 선불로계약을선택하고지불합니다. 판매자는고객이무엇을구매하고주문 실행 하는지AWS Marketplace API를 통해 확인 합니다.

    - [Contract (Tiered) Pricing Example](https://aws.amazon.com/marketplace/pp/B07XSMKK41)
    - [Contract (Configurable) Pricing Example](https://aws.amazon.com/marketplace/pp/B07MF4GHYW)
- **Subscription** – Pay-as-u-go/미터링. 선결제수수료가없습니다. 판매자는사용량을모니터링하고AWS Marketplace API를 통해레포팅하여고객에게청구합니다.
    - [Subscription Pricing Example](https://aws.amazon.com/marketplace/pp/B01LXMNGHB)
- **Contract with Consumption** - 추가 미터링과 고정가격의혼합.
    - [Contract (Tiered) with Consumption Pricing Example](https://aws.amazon.com/marketplace/pp/B07NDJJ2WK)
    - [Contract (Configurable) with Consumption Pricing Example](https://aws.amazon.com/marketplace/pp/B081NHJL88)

**고객** 온보딩 **프로세스** 과정 요약 :

- AWS Marketplace의제품세부정보페이지에서고객은(a)**Contract**에대해  **계약을** 선택하고 **선불로** 지불하거나(b)**Subscription**에대해  **가격** 정책 **동의합니다**.
- 구독후 고객은  **판매자가** 제공한 **방문 페이지로** 리디렉션됩니다 .
  - 리디렉션 과정에서 판매자는 AWS Marketplace API를 호출 하여 리디렉션된 방문자의 고유한 익명 **Customer ID** 를 **식별** 합니다. (\*이 때 PII가전달되지않습니다.)
  - 이 **Customer ID** 를사용하여 판매자는 AWS Marketplace API를 호출 하여(a)계약에 대해 고객이 이미 지불한 **계약의 세부 정보를** 검색 하거나 (b)사용량 보고서를 AWS Marketplace에 보내 고객의 청구합니다.
- **판매자가 고객 랜딩 페이지** 에서, 판매자는 고객에게 새로운 계정 또는 기존 계정에 로그인할 웹양식을 제공합니다.
  - 고객 계정 설정을 위해 판매자의 수동 작업이 필요한 경우 문의 양식을 제공해야 합니다. 고객이 이를 작성 후 제출하면,확인 페이지 및 확인 이메일에 SLA 및 연락처 정보를 제공하기를 권고 합니다.
  - 판매자가 지불 계약 초과 분을 고객에게 청구하려는 경우 판매자는 고객 사용량을 모니터링하고 AWS Marketplace에 추가 비용을 보내야 하며 이에 대한 책임을 갖습니다.

다음은 [**SaaS 통합 가이드**](https://awsmp-loadforms.s3.amazonaws.com/AWS+Marketplace+-+SaaS+Integration+Guide.pdf) 입니다. 자세한 내용은 판매자 가이드의 [다음 페이지](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-products.html  )에서 확인할 수 있습니다.

판매자는 AWS Marketplace 와의 integration 과정을 단순화 하여 제품 출시 시간을 단축 하기 위해 [AWS Marketplace **serverless SaaS integration](https://github.com/aws-samples/aws-marketplace-serverless-saas-integration)레퍼런스 아케텍처를 활용할 수 있습니다. 판매자는 이 아키텍처 세팅을 위해 [다음 배포 가이드를](https://github.com/aws-samples/aws-marketplace-serverless-saas-integration/blob/master/Serverless%20SaaS%20API%20Integration%20Deployment%20Guide_v1.2.pdf)사용할 수 있습니다.

다음은 AWS Marketplace와의 통합 과정에서 도움이될 수 있는  **API 문서** 입니다.

- **ResolveCustomer API** – Token을 Customer ID로 resolve 하기 위해 사용하는 API 입니다. 고객이 SaaS 랜딩페이지로 리디렉션될 때 토큰이 전달 됩니다. 이것은 AWS 계정ID가 아니라 이 판매자와 구매 고객과의 관계에 있어 고유한ID 입니다. 이는 모든SaaS 가격 모델에 필요합니다.

    - API Reference: [AWS API Reference > AWS Marketplace Metering Service > Actions > **ResolveCustomer**](https://docs.aws.amazon.com/marketplacemetering/latest/APIReference/API_ResolveCustomer.html)
    - AWS CLI Reference: [AWS CLI Command Reference > metering marketplace >  **resolve-customer**](https://docs.aws.amazon.com/cli/latest/reference/meteringmarketplace/resolve-customer.html)
    - AWS Java SDK Reference: [AWS Java SDK > marketplacemetering >  **resolveCustomer**](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/marketplacemetering/AWSMarketplaceMetering.html#resolveCustomer-com.amazonaws.services.marketplacemetering.model.ResolveCustomerRequest-)
    - IAM Permission needed: [aws-marketplace:ResolveCustomer](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsmarketplacemeteringservice.html#awsmarketplacemeteringservice-ResolveCustomer)

- **GetEntitlements API** - 이 API를 사용하여 특정 CustomerID가 이미 지불한 사용 자격(권한)을 가져 옵니다. 이는 SaaS **Contract 및 Contract with Consumption** 가격 책정 모델에 필요 합니다.

    - API Reference: [AWS API Reference > AWS Marketplace Entitlement Service > Actions > **GetEntitlements**](https://docs.aws.amazon.com/marketplaceentitlement/latest/APIReference/API_GetEntitlements.html)
    - AWS CLI Reference: [AWS CLI Command Reference > marketplace-entitlement > **get-entitlements**](https://docs.aws.amazon.com/cli/latest/reference/marketplace-entitlement/get-entitlements.html)
    - AWS Java SDK Reference: [AWS Java SDK > marketplaceentitlement > **getEntitlements**](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/marketplaceentitlement/AWSMarketplaceEntitlement.html#getEntitlements-com.amazonaws.services.marketplaceentitlement.model.GetEntitlementsRequest-)
    - IAM Permission needed: [**aws-marketplace:GetEntitlements**](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsmarketplaceentitlementservice.html#awsmarketplaceentitlementservice-GetEntitlements)

- **BatchMeterUsage API** – 이 API를 사용 하여 SaaS **Subscription** 또는 **Contract with Consumption** 가격 책정 모델에 대해 당월말에 사용자 에게 청구할 추가 사용량을 보고 합니다.

    - API Reference: [AWS API Reference > AWS Marketplace Metering Service > Actions > **BatchMeterUsage**](https://docs.aws.amazon.com/marketplacemetering/latest/APIReference/API_BatchMeterUsage.html)
    - AWS CLI Reference: [AWS CLI Command Reference > meteringmarketplace > **batch-meter-usage**](https://docs.aws.amazon.com/cli/latest/reference/meteringmarketplace/batch-meter-usage.html)
    - AWS Java SDK Reference: [AWS Java SDK > marketplacemetering > **batchMeterUsage**](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/marketplacemetering/AWSMarketplaceMetering.html#batchMeterUsage-com.amazonaws.services.marketplacemetering.model.BatchMeterUsageRequest-)
    - IAM Permission needed: [**aws-marketplace:BatchMeterUsage**](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsmarketplacemeteringservice.html#awsmarketplacemeteringservice-BatchMeterUsage)

End-to-end SaaS 제품 등록프로세스:

1. 판매자가 [AWS Marketplace Management Portal](https://aws.amazon.com/marketplace/management/products/) 에서SaaS 양식제출
2. AWS Marketplace(AWSMP) 판매자 운영팀이 제한된 제품 listing(\*판매자만 접근 가능한 AWS Marketplace 제품 페이지)생성합니다
3. 판매자는 최종 제품 노출 준비될 때 까지 API를 통합 하고 테스트합니다
4. AWSMP팀은 종단간 온보딩 테스트를 수행 하고 최종 승인을 위해 판매자에게 연락 합니다.
5. 판매자가 최종 승인을 제공 하고 AWSMP팀이 제한된 제품 listing을 공개 가능토록 게시합니다.

판매자은 기술팀이 AWS Marketplace와의 통합 진행을 시작 할 수 있도록 다음 과정을 제안 합니다.

- 아직 등록 하지 않은 경우 **테스트** 할 AWS Marketplace [판매자계정](https://aws.amazon.com/marketplace/management/tour) 을 등록하십시오. 법무팀이 T&C를 검토 하고 판매자 이름을 설정 하도록 합니다. 세금 및 은행 정보가 필요할 수 있습니다. 여러 셀러 계정을 만들 수 있습니다. 각 판매자 계정은 하나의 AWS 계정에 연결 됩니다.
- 제품 마케팅 세부 정보에 대하여 우선 Placeholder값을 사용 하여 **테스트** 할 [SaaS 제품](https://aws.amazon.com/marketplace/management/products/saas) 을 만듭니다. 구매자의 랜딩 페이지 URL 을 추가 하고 제품 등록 과정에서 요구되는 값을 설정 하세요. 제품 등록 신청서가 제출되면_판매자 __운영__ 팀은 AWS 계정 및 기타 허용 목록에 있는 계정만 액세스할 수 있는 제한된 미리 보기 제품 페이지를 준비하기 시작합니다. 이 제한된 미리보기 제품 페이지로 온보딩 프로세스를 테스트할 수 있습니다. 제한된 미리보기 제품 페이지를 통한 테스트를 위해 AWS Marketplace 팀은 Product Code 및 SNS Topic을 판매자에게 제공합니다.

감사합니다.
