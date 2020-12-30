# Frameworks, policies, controls and procedures

## NIST

National Institute for Standards and Technology

> Focus: innovation and industrial competitiveness

NIST Special Publication 800-53 (Security and Privacy Controls for Federal Information Systems and Organizations) and the Cyber Security Framework (CSF)

### SP 500-83

- Outlines controls that agencies need to put into place to be compliant with FIPS (Federal Information Processing Standards)

Provides guidance on how to select security controls as part of the risk management framework in SP 800-37

Control Categories:

- Management / Administrative
- Operation / Physical Controls
- Technical / Logical / Policy

## Cyber Security Framework

Published by NIST

Consists of three main components:

- Framework Core, 5 Functions, 22 categories, and 98 Subcategories
- Implementation Tiers
  - Categorizes tiers of rigod and  sophistocation
    - tier 1: Partial
    - tier 2: Risk Informed
    - tier 3: Repeatable
    - tier 4: Adaptive
- Framework Profile
  - Describes the state of an org with regard to CSF

Alignment with Core CSF Functions:

- Identity: understaning context, resources, risks
- Protect: Develop controls to mitigate risks in ways that make sense
- Detect: Discover threats in a timely manner
- Responsd: Quickly contain any known threats
- Recover: Return business functionality ASAP to a secure state

> CSF is voluntary, and not a one-size fits all

## ISO

International Organization for Standardization

International Electrotechnical Commission (IEC)

Information Security Management System (ISMS)

> ISO / IEC 27001 is a common standard for businesses, and attests to the organization's compliance level

## COBIT

The Control Objectives for Information and Related Technology

Framework and set of *control objectives* developed by **ISACA** (formerly the Information Systems Audit and Control Association), and the IT Governance Institute (ITGI)

It defines goals and controls to map to business needs

Broken down into four domains:

- Plan and organize
- Acquire and Implement
- Deliver and Support
- Monitor and Evaluate

> COBIT can bridge the gap between high-level framework and the selection and implementation of effective procedures and controls
>
> *Maymi, Fernando. CompTIA CySA+ Cybersecurity Analyst Certification Bundle (Exam CS0-001) . McGraw-Hill Education. Kindle Edition.*

## SABSA

The Sherwood Applied Business Security Architecture (SABSA), is a layered model

- What are you trying to do at this layer?
  - Assets to be protected by security
- Why are you doing it?
  - Motivation for *wanting* to apply security
- How are you trying to do it?
  - Functions needed to achieve security
- Who is involved?
  - People / organizations aspects of security
- Where are you doing it?
  - Locations where you apply security
- When are you doing it?
  - Time-related aspects

> SABSA provides a lifecycle model so that architecture can be constantly monitored and improved over time

## TOGAF (The Open Architecture Framework)

Security framework for the private sector (US DoD)

Allows four different views of the architecture:

- Business
- Data
- Application
- Technology

## ITIL

Information Technology Infrastructure Library

De facto standard for IT Service Management best practices

## Data Classification

Organize data according to its sensitivity to loss, alteration, disclosure or unavailability

## Data Onwership / Data Classification

Individual / team who owns and maintains data, provides access, and are the one(s) responsible for alterations to the data should the business needs arise

## The problem with Running as Root

 Windows operating systems allow you to right-click any program and select Run As... in order to elevate your privileges. From the command prompt, you can just use the command runas /user:\<AccountName> to accomplish the same goal.

In Mac OS X you use sudo from the terminal, or sudo open -a \<AppName>

## Evidence Production Procedures

> The manner in which evidence is introduced is almost as important as the evidence itself
>
> Make sure that your processes / procedures are thorough and well-documented (and enforced)

*Evidence Production* is a legal request for documents, files, or tangible items that may have precedence in a legal procdure

The discovery of ESI (Electronically Stored Information) is called *e-discovery*, this is the process of producing all ESI which mayb be pertinent to a legal proceeding

## Organizationally Defined Parameters

Is a variable that defines selected portions of the controls to support *specific organizational requirements or objectives*

## Control-Testing Procedures

Procedure of verifying security controls are implemented properly to mitigate the intended threat

## Regulatory Compliance

### Sarbanes-Oxley Act (SOX)

- Intended to protect investors and public against fraudulent and misleading activities for **public traded** companies

### Payment Card Industry Data Security Standard (PCI DSS)

- Industry standard for handling credit / debit card data

### Gramm-Leach-Bliley Act (GLBA)

- Provides *Safeguards Rule* which requires safeguards to protect the confidentiality and integrity of personal consumer info

### Federal Information Security Management Act (FISMA)

- Applies to federal agencies or contractors working on their behalf, provisions requirements for: risk assessment frequency, security training, incident response, continuity of operations

### Health Insurance Portability and Accountability Act (HIPAA)

- Healthcare oriented regulation, includes the Security and Privacy Rules which place specific requirements on CIA and privacy of patient data

### Maturity Models

Capability Maturity Model Integration (CMMI) comprehensive, integrated set of guidelines for developing products and software

CMMI provides a model that will:

- Create a more disciplined and repeatable method for development
- Reducing development lifecycle
- Providing better project management capabilities
- Allowing for milestones to be met in a timely manner
- Takes a more proactive approach than the less effective reactive approach

## Chapter Review

1. C. Virtual
2. A. Special Publication 800-53
3. ~~A. Information Security Mangement System~~ D. ITIL is the standard for IT Service Management
4. A. Cyber Security Framework
5. ~~C. Architecture Development Method (ADM)~~ B. International Organization for Standardization (ISO) and IEC 27000 series (ISMS Family of Standards), provides best practice recommendations for information security management
6. C. Data Owners
7. B. Maturity Model
8. B. Implementation Tiers
9. A. Identify, Protect, Detect, Respond, Recover
10. ~~A. Administrative Policy~~ C. Security Policy (policy passed by senior manageent, policy board, or committee that dictates the role security plays in an org)
