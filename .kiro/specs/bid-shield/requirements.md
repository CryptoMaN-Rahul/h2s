# Requirements Document

## Introduction

Bid-Shield is an AI-powered SaaS platform designed to help Small and Medium Enterprises (SMEs) navigate the complex government tender process in India. With over 75,000 government tenders released monthly and SMEs capturing only 25% of central procurement by value despite constituting 99% of enterprises, there is a massive opportunity to level the playing field. The system analyzes tender documents to identify compliance requirements, detect potential disqualifications, and generate strategic pre-bid queries to challenge restrictive specifications that may violate Central Vigilance Commission (CVC) guidelines.

**Market Context:** India's public procurement market represents USD 500-600 billion annually (15-20% of GDP), yet SMEs face significant barriers including EMD forfeiture losses, time wasted on unwinnable tenders (90% failure rate), and hidden killer clauses that cause 20-30% of technical disqualifications.

## Glossary

- **System**: The Bid-Shield AI-powered tender compliance platform
- **User**: SME representatives who upload and analyze tender documents
- **Tender_Document**: PDF files containing government tender specifications and requirements
- **EMD**: Earnest Money Deposit - refundable security deposit (1-5% of tender value) required for participation
- **Killer_Clause**: Hidden restrictive requirements that technically disqualify bidders (cause 20-30% of rejections)
- **CVC**: Central Vigilance Commission - regulatory body overseeing government procurement
- **User_Profile**: Stored capability data including turnover, certifications, location, and experience
- **Compliance_Matrix**: Structured comparison of tender requirements against user capabilities
- **Pre_Bid_Query**: Legal challenge to restrictive tender specifications citing CVC guidelines
- **Textract_Queries**: AWS service for targeted data extraction using natural language questions
- **Lawyer_Agent**: AI component using Claude Sonnet 4.5 specialized for legal analysis and CVC compliance
- **Auditor_Agent**: AI component using Claude Sonnet 4.5 specialized for numerical compliance checking
- **GeM_Portal**: Government e-Marketplace platform facilitating INR 1,34,000 crore annually
- **MSME**: Micro, Small & Medium Enterprises eligible for EMD exemptions and special benefits
- **Bid_Win_Ratio**: Success rate metric (industry average <10% for SMEs)
- **Technical_Disqualification**: Rejection due to non-compliance with tender specifications

## Requirements

### Requirement 1: Document Upload and Processing

**User Story:** As an SME representative, I want to upload tender PDF documents to the platform, so that I can analyze compliance requirements and avoid costly EMD forfeiture (typically 1-5% of tender value).

#### Acceptance Criteria

1. WHEN a user uploads a PDF document, THE System SHALL accept files up to 100MB in size to handle complex government tender documents
2. WHEN a PDF is uploaded, THE System SHALL trigger automated processing within 30 seconds using AWS S3 event triggers
3. WHEN processing begins, THE System SHALL provide real-time status updates via WebSocket connections
4. WHEN upload fails due to file corruption or format issues, THE System SHALL display specific error messages with resolution steps
5. THE System SHALL support batch upload of up to 10 documents simultaneously for framework agreements
6. WHEN documents contain scanned images, THE System SHALL automatically detect and process them using OCR capabilities
7. THE System SHALL maintain upload history with timestamps and processing status for audit purposes

### Requirement 2: AI-Powered Document Parsing and Data Extraction

**User Story:** As an SME representative, I want the system to extract key tender requirements from complex PDF documents using AWS Textract Queries, so that I can quickly understand critical compliance criteria without spending hours on manual review.

#### Acceptance Criteria

1. WHEN processing a tender document, THE System SHALL use Textract Queries to ask "What is the EMD amount?" and extract values with 95% accuracy
2. WHEN processing a tender document, THE System SHALL use Textract Queries to ask "What is the required annual turnover?" and extract numerical values with 95% accuracy
3. WHEN processing a tender document, THE System SHALL use Textract Queries to ask "What experience criteria are required?" and extract years/project requirements
4. WHEN processing a tender document, THE System SHALL use Textract Queries to ask "What certifications are mandatory?" and extract specific certification names
5. WHEN processing a tender document, THE System SHALL convert non-standard PDF tables into structured JSON data preserving relationships
6. WHEN legal text is encountered, THE System SHALL preserve exact clause references with page numbers and section identifiers for citation
7. WHEN extraction completes, THE System SHALL store all extracted data in DynamoDB Compliance_Matrix with tender ID mapping
8. WHEN processing fails on specific sections, THE System SHALL flag incomplete extractions and allow manual review
9. THE System SHALL handle multi-language documents (Hindi/English) commonly found in state government tenders

### Requirement 3: User Capability Profile Management

**User Story:** As an SME representative, I want to maintain my company's capability profile with MSME registration details, so that the system can accurately assess my eligibility and automatically claim available exemptions.

#### Acceptance Criteria

1. WHEN creating a profile, THE System SHALL capture annual turnover for last 3 financial years with CA certificate validation
2. WHEN creating a profile, THE System SHALL record company location (state/district) for location-based tender filtering
3. WHEN creating a profile, THE System SHALL store all relevant certifications (ISO, BIS, MSME, etc.) with expiry date tracking
4. WHEN creating a profile, THE System SHALL capture years of experience in specific domains/sectors
5. WHEN MSME registration is provided, THE System SHALL automatically flag EMD exemption eligibility
6. WHEN updating profile data, THE System SHALL validate numerical inputs and flag inconsistencies
7. WHEN profile is complete, THE System SHALL calculate a "Capability Score" for tender matching
8. THE System SHALL maintain historical profile versions for compliance audit purposes
9. WHEN profile data is missing critical information, THE System SHALL prioritize prompts based on tender requirements
10. THE System SHALL integrate with MSME portal APIs to verify registration status automatically

### Requirement 4: Automated Compliance Assessment with Risk Scoring

**User Story:** As an SME representative, I want the system to automatically check my eligibility against tender requirements with a clear risk assessment, so that I can avoid bidding on tenders with high failure probability and focus on winnable opportunities.

#### Acceptance Criteria

1. WHEN compliance check runs, THE Auditor_Agent SHALL compare extracted requirements against User_Profile using AWS Step Functions orchestration
2. WHEN turnover requirements exceed user capability by >20%, THE System SHALL flag as "RED" with EMD loss risk warning
3. WHEN location requirements don't match user profile, THE System SHALL flag as "RED" with automatic disqualification warning
4. WHEN mandatory certifications are missing, THE System SHALL flag as "RED" with specific certification names
5. WHEN experience requirements exceed user capability, THE System SHALL flag as "YELLOW" with gap analysis
6. WHEN all requirements are met with margin, THE System SHALL display "GREEN" with confidence percentage
7. THE System SHALL calculate a "Bid Success Probability" score (0-100%) based on historical data and compliance gaps
8. THE System SHALL provide detailed explanations for each compliance flag with specific requirement citations
9. WHEN MSME exemptions apply, THE System SHALL automatically adjust compliance status and highlight benefits
10. THE System SHALL estimate potential EMD exposure and ROI analysis for each tender opportunity
11. WHEN similar tenders were previously analyzed, THE System SHALL show comparative success rates and lessons learned

### Requirement 5: Legal Analysis and CVC Compliance Checking

**User Story:** As an SME representative, I want the system to identify potentially restrictive tender clauses that violate CVC guidelines using a knowledge base of procurement regulations, so that I can challenge unfair specifications and improve my chances of winning.

#### Acceptance Criteria

1. WHEN analyzing terms and conditions, THE Lawyer_Agent SHALL scan for CVC guideline violations using Amazon Bedrock with Claude Sonnet 4.5
2. WHEN restrictive specifications are detected, THE System SHALL flag violations with specific CVC circular references and legal precedents
3. WHEN experience requirements exceed 3x project complexity, THE System SHALL identify as potentially restrictive per CVC guidelines
4. WHEN technical specifications mention specific brands or models, THE System SHALL flag as anti-competitive behavior
5. WHEN EMD requirements exceed 5% of tender value, THE System SHALL flag as excessive per CVC norms
6. WHEN performance guarantee exceeds 10% of contract value, THE System SHALL flag as potentially unreasonable
7. THE System SHALL maintain a vector database (Amazon OpenSearch) of CVC procurement guidelines, circulars, and case precedents
8. WHEN liquidated damages clauses exceed 10% of contract value, THE System SHALL flag as punitive
9. THE System SHALL identify "single source" specifications that artificially limit competition
10. WHEN payment terms exceed 90 days, THE System SHALL flag as SME-unfriendly per MSME guidelines
11. THE System SHALL detect automatic EMD forfeiture clauses for minor deviations and flag as harsh

### Requirement 6: Pre-Bid Query Generation with Legal Templates

**User Story:** As an SME representative, I want the system to generate legally sound pre-bid queries with proper formatting and citations, so that I can challenge restrictive tender specifications professionally and increase my chances of favorable amendments.

#### Acceptance Criteria

1. WHEN CVC violations are identified, THE System SHALL generate draft pre-bid queries citing specific CVC circular numbers and sections
2. WHEN generating queries, THE System SHALL reference relevant legal precedents from Supreme Court and High Court judgments
3. WHEN anti-competitive clauses are found, THE System SHALL draft queries citing Competition Act provisions and CCI guidelines
4. WHEN queries are generated, THE System SHALL provide editable templates following standard government submission formats
5. THE System SHALL format queries with proper legal language, respectful tone, and professional structure
6. WHEN multiple violations exist, THE System SHALL prioritize queries by impact potential and likelihood of acceptance
7. THE System SHALL include suggested alternative specifications that maintain technical requirements while increasing competition
8. WHEN generating queries, THE System SHALL provide supporting documentation references and relevant case studies
9. THE System SHALL create a submission timeline with deadlines for pre-bid meetings and query submissions
10. THE System SHALL generate follow-up templates for clarifications and additional queries based on tender authority responses
11. WHEN queries are successful, THE System SHALL track amendments and build a success rate database for future improvements

### Requirement 7: Interactive PDF Viewer with Smart Citation Overlays

**User Story:** As an SME representative, I want to view the original tender document with highlighted problem areas and clickable citations, so that I can understand exactly where issues occur and reference them in my queries.

#### Acceptance Criteria

1. WHEN displaying PDF documents, THE System SHALL render them in a responsive web-based viewer supporting zoom and navigation
2. WHEN compliance issues are detected, THE System SHALL overlay color-coded visual indicators (Red/Yellow/Green) on relevant text sections
3. WHEN users click warning indicators, THE System SHALL highlight the exact problematic clause and display a detailed explanation popup
4. WHEN displaying citations, THE System SHALL show page numbers, clause references, and relevant CVC guideline links
5. THE System SHALL support side-by-side view of original PDF and extracted structured data for verification
6. WHEN users select text in the PDF, THE System SHALL provide contextual analysis and suggest related compliance checks
7. THE System SHALL enable users to add personal notes and bookmarks to specific sections for future reference
8. WHEN printing or exporting, THE System SHALL maintain citation overlays and provide a summary report
9. THE System SHALL support search functionality within the PDF with highlighting of search terms
10. WHEN multiple documents are uploaded, THE System SHALL provide tabbed interface for easy comparison
11. THE System SHALL enable sharing of annotated PDFs with team members while maintaining security controls

### Requirement 8: Comprehensive Dashboard and Analytics

**User Story:** As an SME representative, I want a comprehensive dashboard showing my tender analysis results with financial impact metrics, so that I can make data-driven bidding decisions and demonstrate ROI to stakeholders.

#### Acceptance Criteria

1. WHEN users access the dashboard, THE System SHALL display a traffic light summary of all analyzed tenders with success probability scores
2. WHEN showing tender summaries, THE System SHALL include EMD amounts, estimated bid preparation costs, and potential contract values
3. WHEN generating reports, THE System SHALL calculate total EMD savings from avoided bad bids and time savings in hours
4. THE System SHALL track user bid-win ratio improvement over time with before/after Bid-Shield metrics
5. WHEN displaying analytics, THE System SHALL show sector-wise performance, average tender values, and competition analysis
6. THE System SHALL provide monthly/quarterly reports showing platform ROI and success metrics
7. WHEN exporting data, THE System SHALL support PDF executive summaries and detailed Excel reports for offline analysis
8. THE System SHALL display upcoming tender deadlines with priority rankings based on success probability
9. WHEN showing historical data, THE System SHALL provide trend analysis of government procurement patterns in user's sectors
10. THE System SHALL calculate and display "Opportunity Score" for each tender based on competition level and user fit
11. THE System SHALL provide benchmarking against industry averages and peer SME performance where available
12. WHEN generating insights, THE System SHALL recommend optimal tender categories and government departments to target

### Requirement 9: Enterprise Security and Data Governance

**User Story:** As an SME representative handling sensitive business and tender information, I want enterprise-grade security and compliance controls, so that my company data remains protected and I can meet audit requirements.

#### Acceptance Criteria

1. WHEN storing user data, THE System SHALL encrypt all sensitive information at rest using AWS KMS with customer-managed keys
2. WHEN transmitting data, THE System SHALL use TLS 1.3 encryption for all API communications and file transfers
3. THE System SHALL implement role-based access control (RBAC) supporting multiple user roles within organizations
4. WHEN data is accessed, THE System SHALL log all user activities with timestamps, IP addresses, and action details for security monitoring
5. THE System SHALL provide data retention policies allowing users to control how long their data is stored
6. WHEN processing documents, THE System SHALL maintain detailed audit logs of all analysis activities for compliance purposes
7. THE System SHALL implement multi-factor authentication (MFA) for all user accounts
8. WHEN users request data deletion, THE System SHALL provide complete data purging within 30 days per GDPR compliance
9. THE System SHALL provide data backup and disaster recovery capabilities with 99.9% availability SLA
10. WHEN integrating with external APIs, THE System SHALL use secure API keys and implement rate limiting
11. THE System SHALL conduct regular security assessments and vulnerability scans with automated patching
12. THE System SHALL provide data export functionality allowing users to download all their data in standard formats

### Requirement 10: High-Performance Scalable Architecture

**User Story:** As a platform administrator, I want the system to handle multiple concurrent users and large document processing loads with auto-scaling capabilities, so that the service remains responsive during peak tender submission periods.

#### Acceptance Criteria

1. WHEN processing documents up to 100MB, THE System SHALL complete analysis within 5 minutes using parallel processing
2. THE System SHALL support at least 500 concurrent users without performance degradation during peak hours
3. WHEN system load increases beyond 70% capacity, THE System SHALL automatically scale processing resources using AWS Auto Scaling
4. WHEN errors occur during processing, THE System SHALL implement exponential backoff retry mechanisms with circuit breakers
5. THE System SHALL maintain 99.5% uptime during business hours (9 AM - 6 PM IST) with automated failover
6. WHEN processing multiple documents simultaneously, THE System SHALL use AWS Step Functions for parallel workflow orchestration
7. THE System SHALL implement caching strategies for frequently accessed tender data and user profiles
8. WHEN API response times exceed 2 seconds, THE System SHALL trigger performance alerts and auto-scaling
9. THE System SHALL support horizontal scaling across multiple AWS availability zones for disaster recovery
10. WHEN database queries become slow, THE System SHALL implement read replicas and query optimization
11. THE System SHALL provide real-time monitoring dashboards for system performance and user activity metrics
12. THE System SHALL implement content delivery network (CDN) for fast PDF loading and global accessibility

### Requirement 11: Intelligent Tender Discovery and Matching

**User Story:** As an SME representative, I want the system to proactively discover and recommend relevant tenders based on my profile and success probability, so that I don't miss opportunities and can focus on the most promising bids.

#### Acceptance Criteria

1. WHEN new tenders are published on GeM and other portals, THE System SHALL automatically crawl and analyze them within 2 hours
2. WHEN analyzing new tenders, THE System SHALL calculate match scores based on user profile, location, and capability
3. WHEN high-probability matches are found, THE System SHALL send real-time notifications via email and SMS
4. THE System SHALL integrate with major tender portals (GeM, eTenders, CPP Portal) for comprehensive coverage
5. WHEN filtering tenders, THE System SHALL consider user's historical bid patterns and success rates
6. THE System SHALL provide sector-wise tender alerts based on user's business domains
7. WHEN tender deadlines approach, THE System SHALL send escalating reminders with preparation time estimates
8. THE System SHALL maintain a "Watchlist" feature for tracking specific departments or tender types
9. WHEN similar tenders are found, THE System SHALL group them for bulk analysis and comparison
10. THE System SHALL provide market intelligence on tender frequency and competition levels by sector

### Requirement 12: Collaborative Features and Team Management

**User Story:** As an SME owner, I want to collaborate with my team members and external consultants on tender analysis, so that we can leverage collective expertise and improve our bidding success.

#### Acceptance Criteria

1. WHEN inviting team members, THE System SHALL support role-based permissions (Admin, Analyst, Viewer)
2. WHEN sharing tender analysis, THE System SHALL enable commenting and annotation on specific document sections
3. THE System SHALL provide workflow management for tender review and approval processes
4. WHEN multiple users work on the same tender, THE System SHALL prevent conflicts with real-time collaboration features
5. THE System SHALL maintain version history of all analysis reports and user modifications
6. WHEN external consultants are involved, THE System SHALL provide time-limited access with specific permissions
7. THE System SHALL enable task assignment and deadline tracking for tender preparation activities
8. WHEN team decisions are made, THE System SHALL log decision rationale and responsible parties
9. THE System SHALL provide team performance analytics and individual contribution tracking
10. THE System SHALL support integration with popular project management tools (Slack, Microsoft Teams)

### Requirement 13: Financial Planning and EMD Management

**User Story:** As an SME representative, I want to track my EMD commitments and financial exposure across multiple tenders, so that I can manage cash flow and avoid over-commitment.

#### Acceptance Criteria

1. WHEN analyzing tenders, THE System SHALL calculate total EMD exposure if all recommended tenders are bid
2. THE System SHALL provide cash flow projections based on tender timelines and EMD requirements
3. WHEN EMD limits are approached, THE System SHALL warn users and suggest prioritization strategies
4. THE System SHALL track EMD refund timelines and send reminders for follow-up actions
5. WHEN MSME exemptions apply, THE System SHALL automatically calculate savings and highlight benefits
6. THE System SHALL integrate with banking APIs for EMD guarantee management where available
7. THE System SHALL provide ROI calculations comparing EMD investment to potential contract values
8. WHEN tenders are won or lost, THE System SHALL update financial tracking and success metrics
9. THE System SHALL generate financial reports for accounting and tax purposes
10. THE System SHALL provide budget planning tools for annual tendering activities

### Requirement 14: Mobile Application and Offline Capabilities

**User Story:** As an SME representative who travels frequently, I want mobile access to tender analysis and offline capabilities, so that I can review opportunities and make decisions from anywhere.

#### Acceptance Criteria

1. THE System SHALL provide a responsive mobile web application optimized for smartphones and tablets
2. WHEN offline, THE System SHALL allow users to view previously downloaded tender analyses
3. THE System SHALL enable offline note-taking and decision marking with sync when connection is restored
4. WHEN push notifications are enabled, THE System SHALL send alerts for urgent tender opportunities
5. THE System SHALL provide mobile-optimized PDF viewing with touch-friendly navigation
6. WHEN using mobile data, THE System SHALL compress content to reduce bandwidth usage
7. THE System SHALL support biometric authentication on mobile devices for quick access
8. THE System SHALL enable voice notes and dictation for quick feedback and comments
9. WHEN location services are enabled, THE System SHALL prioritize geographically relevant tenders
10. THE System SHALL provide quick action buttons for common tasks (approve, reject, flag for review)